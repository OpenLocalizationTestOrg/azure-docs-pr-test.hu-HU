---
title: "a Windows-alapú HDInsight-ból aaaMigrate tooLinux-alapú HDInsight - Azure |} Microsoft Docs"
description: "Ismerje meg, hogyan toomigrate a Windows-alapú HDInsight fürt tooa Linux-alapú HDInsight-fürtöt."
services: hdinsight
documentationcenter: 
author: Blackmist
manager: jhubbard
editor: cgronlun
ms.assetid: ff35be59-bae3-42fd-9edc-77f0041bab93
ms.service: hdinsight
ms.custom: hdinsightactive
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 07/12/2017
ms.author: larryfr
ms.openlocfilehash: 7e5e536e8672d7e7c3086c6860cec062d05eda65
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="migrate-from-a-windows-based-hdinsight-cluster-tooa-linux-based-cluster"></a>Windows-alapú HDInsight fürt tooa Linux-alapú fürtök áttelepítése

Ez a dokumentum részletesen a HDInsight a Windows és Linux, és az útmutatás hello különbségei toomigrate meglévő munkaterhelések tooa Linux rendszerű fürt.

Amíg a Windows-alapú HDInsight biztosít egy egyszerűen toouse hello felhőalapú Hadoop, szükség lehet a toomigrate tooa Linux rendszerű fürt. Például, a Linux-alapú eszközök és a megoldás a szükséges technológiák tootake előnyeit. A Hadoop ökoszisztémájának hello számos elemet a Linux-alapú rendszereken fejlesztett, és nem lehet a Windows-alapú hdinsight eszközzel használható. Emellett számos könyvek, videókat és egyéb oktatóanyag feltételezi Linux rendszert a használ, amikor olyan Hadoop.

> [!NOTE]
> A HDInsight-fürtök használata Ubuntu hosszú távú támogatási (LTS) hello operációs rendszer hello hello fürt csomópontja. Hello verzióját a HDInsight az elérhető Ubuntu más összetevők verziószámozása információk mellett további információkért lásd: [HDInsight összetevő verziók](hdinsight-component-versioning.md).

## <a name="migration-tasks"></a>Áttelepítési feladatok

hello áttelepítés általános munkafolyamata a következőképpen történik.

![Áttelepítési munkafolyamat diagramja](./media/hdinsight-migrate-from-windows-to-linux/workflow.png)

1. Minden szakasz ebben a dokumentumban olvasható toounderstand módosításokat, a meglévő munkafolyamat, feladatok, stb. tooa Linux-alapú fürt áttelepítésekor van szükség.

2. Hozzon létre egy Linux-alapú fürtöt, a teszt/minőségi megbízhatósági környezetekben. A Linux-alapú fürtök létrehozásáról további információk: [hdinsight létrehozása Linux-alapú fürtökön](hdinsight-hadoop-provision-linux-clusters.md).

3. Másolás meglévő feladatokat, adatforrások, és új környezet toohello fogadók esetében.

4. Tesztelési meg arról, hogy a feladatok hello új fürt a várt módon működik-e toomake végez.

Miután ellenőrizte, hogy minden megfelelően működik-e, tervezze hello áttelepítésre. Üzemszünet a következő műveletek hello végrehajtania:

1. Bármely átmeneti hello fürtcsomópontokon helyben tárolt adatok biztonsági mentését. Ha például közvetlenül egy átjárócsomóponttal tárolt adatokat.

2. Hello Windows-alapú fürt törlése.

3. Azonos alapértelmezett használt Windows-alapú fürt hello adattároló hello Linux-alapú fürtöt létrehozni. Linux-alapú fürt hello folytathatják a munkát a meglévő éles adatok alapján.

4. Biztonsági másolatot készített az átmeneti adatok importálása.

5. Kezdő feladatok/folytatni hello új fürt segítségével.

### <a name="copy-data-toohello-test-environment"></a>Másolja az adatokat toohello tesztkörnyezetben

Sok módszerek toocopy hello adatok és feladatokat, azonban a két hello ebben a szakaszban tárgyalt hello legegyszerűbb módszer toodirectly áthelyezés fájlok tooa tesztfürthöz.

#### <a name="hdfs-copy"></a>HDFS másolása

Hello követő lépéseket toocopy adatok hello éles fürt toohello teszt fürtből használja. Ezeket a lépéseket hello használata `hdfs dfs` segédprogram, amely tartalmazza a hdinsight eszközzel.

1. Hello tárolási fiók alapértelmezett tároló adatai és a meglévő fürt található. a következő példa hello PowerShell tooretrieve használja ezeket az adatokat:

    ```powershell
    $clusterName="Your existing HDInsight cluster name"
    $clusterInfo = Get-AzureRmHDInsightCluster -ClusterName $clusterName
    write-host "Storage account name: $clusterInfo.DefaultStorageAccount.split('.')[0]"
    write-host "Default container: $clusterInfo.DefaultStorageContainer"
    ```

2. egy tesztkörnyezetben toocreate kövesse hello hello létrehozása Linux-alapú fürtökön a HDInsight-dokumentumban. Hello fürt létrehozása előtt állítsa le, és ehelyett válassza **opcionális konfigurációs**.

3. Hello opcionális konfigurációs panelen válassza ki **a társított Tárfiókokban**.

4. Válassza ki **tárolási kulcs hozzáadása**, amikor a rendszer kéri, válassza ki a hello az 1. lépésben a PowerShell-parancsfájl által visszaadott hello tárfiók. Kattintson a **válasszon** minden panelen. Végezetül hozza létre a hello fürtöt.

5. Hello fürt létrehozása után tooit protokoll használatával kapcsolódó levelezőprogramokkal **SSH.** További információ: [Az SSH használata HDInsighttal](hdinsight-hadoop-linux-use-ssh-unix.md).

6. Hello SSH-munkamenetet, a következő parancs toocopy fájlok tárfiókból hello kapcsolódó fiók toohello új alapértelmezett tárolási hello használja. Cserélje le a tároló PowerShell által visszaadott hello tároló adatokkal. Cserélje le __fiók__ hello fióknévvel. Cserélje le hello elérési toodata hello elérési tooa adatfájlt.

    ```bash
    hdfs dfs -cp wasb://CONTAINER@ACCOUNT.blob.core.windows.net/path/to/old/data /path/to/new/location
    ```

    > [!NOTE]
    > Ha hello könyvtárstruktúrát hello adatokat tartalmazó nem létezik a hello tesztkörnyezetben, létrehozhat hello a következő parancs használatával:

    ```bash
    hdfs dfs -mkdir -p /new/path/to/create
    ```

    Hello `-p` kapcsoló lehetővé teszi a hello elérési út minden könyvtár hello létrehozását.

#### <a name="direct-copy-between-blobs-in-azure-storage"></a>Az Azure Storage blobs közötti közvetlen másolása

Azt is megteheti, érdemes lehet toouse hello `Start-AzureStorageBlobCopy` Azure PowerShell parancsmag toocopy BLOB storage-fiókok HDInsight eszközön kívüli között. További információkért lásd: hello hogyan toomanage Azure PowerShell használata az Azure Storage Azure BLOB szakasza.

## <a name="client-side-technologies"></a>Ügyféloldali technológiák

Ügyféloldali technológiák többek között [Azure PowerShell-parancsmagok](/powershell/azureps-cmdlets-docs), [Azure CLI](../cli-install-nodejs.md), vagy hello [.NET SDK for Hadoop](https://hadoopsdk.codeplex.com/) továbbra is toowork Linux-alapú fürtökön. Ezek a technológiák a REST API-t a rendszer hello azonos mindkét fürttípusok az operációs rendszer különböző támaszkodnak.

## <a name="server-side-technologies"></a>Kiszolgálóoldali technológiák

a következő táblázat hello áttelepítése kiszolgálóoldali összetevőket, amelyek Windows-specifikus nyújt útmutatást.

| Ha ezt a technológiát használja... | Ez a művelet végrehajtása... |
| --- | --- |
| **PowerShell** (kiszolgálóoldali parancsfájlok, beleértve a fürt létrehozása során használt Parancsfájlműveletek) |Az újraírási, Bash parancsfájlok. A Parancsfájlműveletek, lásd: [testreszabása Linux-alapú HDInsight parancsfájlműveletekkel](hdinsight-hadoop-customize-cluster-linux.md) és [parancsfájl-művelet fejlesztési a Linux-alapú HDInsight](hdinsight-hadoop-script-actions-linux.md). |
| **Az Azure CLI** (kiszolgálóoldali parancsfájlok) |Hello Azure CLI Linux rendszeren érhető el, amíg azt nem olyan előre telepített HDInsight központi fürtcsomóponton hello. Hello Azure parancssori felület telepítésével kapcsolatos további információkért lásd: [Ismerkedés az Azure CLI 2.0](https://docs.microsoft.com/cli/azure/get-started-with-azure-cli). |
| **.NET-összetevők** |.NET támogatott Linux-alapú HDInsight segítségével [monó](https://mono-project.com). További információkért lásd: [át .NET megoldások tooLinux-alapú HDInsight](hdinsight-hadoop-migrate-dotnet-to-linux.md). |
| **A Win32-összetevők vagy egyéb csak Windows technológia** |Útmutatás a hello összetevő vagy technológia függ. Előfordulhat, hogy képes toofind Linux kompatibilis verziót, vagy lehet szüksége toofind egy másik megoldás, vagy írja át ezt az összetevőt. |

> [!IMPORTANT]
> hello HDInsight management SDK nincs teljes mértékben kompatibilis a monó. Ez nem használható megoldások részeként telepített toohello HDInsight-fürt most.

## <a name="cluster-creation"></a>A fürt létrehozása

Ez a szakasz tájékoztatást nyújt a fürt létrehozása különbségeit.

### <a name="ssh-user"></a>SSH felhasználó

Linux-alapú HDInsight-fürtök használata hello **Secure Shell (SSH)** tooprovide távelérési toohello fürtcsomópontok protokoll. Távoli asztal a Windows-alapú fürtök esetében eltérően a legtöbb SSH-ügyfél nem biztosítanak egy grafikus felhasználói felület. Ehelyett az SSH-ügyfél, amely lehetővé teszi a hello fürtön toorun parancsok parancssorból adja meg. Egyes ügyfelek (például [MobaXterm](http://mobaxterm.mobatek.net/)) adjon meg egy grafikus fájl rendszer böngésző hozzáadása tooa távoli parancssorban.

Fürt létrehozása, és meg kell adnia az SSH-felhasználó vagy egy **jelszó** vagy **nyilvános kulcsú tanúsítvány** hitelesítéshez.

Azt javasoljuk, nyilvános kulcsú tanúsítvány, mert az biztonságosabb a jelszót. Tanúsítványhitelesítés aláírt nyilvános/titkos kulcspár előállításához, akkor hello nyilvános kulcs megadása hello fürt létrehozásakor működik. Toohello-kiszolgálóhoz kapcsolódáskor hello titkos kulcs hello ügyfél biztosítja a hitelesítést a hello kapcsolat.

További információ: [Az SSH használata HDInsighttal](hdinsight-hadoop-linux-use-ssh-unix.md).

### <a name="cluster-customization"></a>Fürt testreszabása

**Parancsfájl-műveletek** használt Linux-alapú fürtök úgy kell megírni, a Bash parancsfájlok. Amíg a Parancsfájlműveletek Linux-alapú fürtök is el használt tooperform testreszabási után a fürt működik-e és fut a fürt létrehozása során is használható. További információkért lásd: [testreszabása Linux-alapú HDInsight parancsfájlműveletekkel](hdinsight-hadoop-customize-cluster-linux.md) és [parancsfájl-művelet fejlesztési a Linux-alapú HDInsight](hdinsight-hadoop-script-actions-linux.md).

Egy másik testreszabási a szolgáltatás **bootstrap**. Windows-fürtök esetén ez a funkció lehetővé teszi további könyvtárak Hive való használatra toospecify hello helyét. Fürt létrehozása után ezek a könyvtárak használhatók automatikusan a Hive-lekérdezések nélkül hello kell toouse `ADD JAR`.

Linux-alapú fürtök hello a rendszerindítási szolgáltatás nem nyújt ezt a funkciót. Ehelyett használja a dokumentált parancsfájlművelet [szalagtárak Hive hozzáadása a fürt létrehozása során](hdinsight-hadoop-add-hive-libraries.md).

### <a name="virtual-networks"></a>Virtuális hálózatok

Windows-alapú HDInsight-fürtök a klasszikus virtuális hálózatok csak használata pedig Linux-alapú HDInsight-fürtök igényelnek erőforrás-kezelő virtuális hálózatok. Ha erőforrások csatlakozni kell-e további információ: a klasszikus virtuális hálózatot, amely Linux-HDInsight-fürt hello [egy klasszikus virtuális hálózatot tooa erőforrás-kezelő virtuális hálózathoz csatlakozó](../vpn-gateway/vpn-gateway-connect-different-deployment-models-portal.md).

Az Azure virtuális hálózatok és a HDInsight együttes használatával konfigurációkra vonatkozó további információkért lásd: [kiterjesztése HDInsight képességek a virtuális hálózat segítségével](hdinsight-extend-hadoop-virtual-network.md).

## <a name="management-and-monitoring"></a>Management and monitoring

Hello web UI használta a Windows-alapú hdinsight eszközzel, például a feladatelőzmények vagy a Yarn felhasználói felületen, számos Ambari keresztül érhető el. Hello Ambari Hive View emellett olyan módon toorun webböngészővel Hive-lekérdezéseket. Linux-alapú fürtökön https://CLUSTERNAME.azurehdinsight.net: hello Ambari webes felhasználói felület érhető el.

Az Ambari munkáról bővebben lásd a következő dokumentumok hello:

* [Ambari webes](hdinsight-hadoop-manage-ambari.md)
* [Ambari REST API-n](hdinsight-hadoop-manage-ambari-rest-api.md)

### <a name="ambari-alerts"></a>Ambari riasztások

Ambari, amelyre egy riasztási rendszer, amelyek segítségével megállapíthatja, hogy hello fürt érintő lehetséges problémákat. Hello Ambari webes felhasználói felületén, sárga vagy piros bejegyzésként riasztás jelenik meg, de is helyreállíthatók hello REST API-n keresztül.

> [!IMPORTANT]
> Ambari riasztás jelzi, hogy *előfordulhat, hogy* probléma, nem az, hogy legyenek *van* probléma. Kaphat például, hogy hiveserver2-n nem érhető el, riasztást, annak ellenére, hogy a szokásos módon el tudja érni azt.
>
> Sok riasztást időközönkénti lekérdezések írásában, a szolgáltatás megvalósítása, és egy megadott időkereten belül választ várt. Hello riasztás nem feltétlenül jelenti azt, hogy hello szolgáltatás le van állítva, így most, hogy azt nem adott vissza eredményt várt hello időkereten belül.

Ki kell értékelnie figyelmeztetés fordult elő hosszabb ideig, vagy felhasználói problémák rajta megtétele előtt jelentett tükrözi.

## <a name="file-system-locations"></a>Rendszer helye

Linux-fürt fájlrendszer hello működnek, mint a Windows-alapú HDInsight-fürtök elrendezését. Használja a következő tábla toofind általánosan használt fájlok hello.

| Toofind kell... | Helyezkedik... |
| --- | --- |
| Konfiguráció |`/etc`. Például: `/etc/hadoop/conf/core-site.xml` |
| Naplófájlok |`/var/logs` |
| Hortonworks Data Platform (HDP) |`/usr/hdp`. Nincsenek a két címtár itt található, egyik hello aktuális HDP verziójú és `current`. Hello `current` könyvtár neve tartalmazza a szimbolikus csatolást toofiles és könyvtárak hello verzió száma könyvtárban található. Hello `current` directory kerül a HDP fájlok elérésének óta hello verziója megváltozik, hello HDP könnyen verzióra frissül. |
| hadoop-streaming.jar |`/usr/hdp/current/hadoop-mapreduce-client/hadoop-streaming.jar` |

Általában ha tudja hello hello fájl nevét, egy SSH-munkamenet toofind hello fájl elérési útról a parancs következő hello használhatja:

    find / -name FILENAME 2>/dev/null

Hello fájlnév helyettesítő karaktereket is használható. Például `find / -name *streaming*.jar 2>/dev/null` adja vissza a hello elérési tooany hello word "streaming" hello fájlnév részeként tartalmazó jar-fájlok.

## <a name="hive-pig-and-mapreduce"></a>Hive, Pig és MapReduce

A Pig és a MapReduce munkaterhelések hasonlóak a Linux-alapú fürtökön. Azonban Linux-alapú HDInsight-fürtök hozhatók létre Hadoop Hive és a Pig újabb verzióját használja. Ezen verzió különbségek vezethetnek hogyan változásairól a meglévő megoldásokat függvény. A HDInsight részét alkotó hello verzióin további információkért lásd: [HDInsight-összetevők verziószámozása](hdinsight-component-versioning.md).

Linux-alapú HDInsight nem biztosít a távoli asztali funkciókat. Ehelyett használhatja az SSH tooremotely fürtcsomópontok toohello head. További információkért tekintse meg a következő dokumentumok hello:

* [A Hive használata a SSH](hdinsight-hadoop-use-hive-ssh.md)
* [A Pig használata SSH](hdinsight-hadoop-use-pig-ssh.md)
* [SSH MapReduce használata](hdinsight-hadoop-use-mapreduce-ssh.md)

### <a name="hive"></a>Hive

> [!IMPORTANT]
> Ha egy külső Hive metaadattárhoz használ, készítsen biztonsági másolatot hello metaadattárhoz és a Linux-alapú HDInsight együttes használatához. Linux-alapú HDInsight egy struktúra, amelynek kompatibilitási problémák lehet újabb verziója érhető el a korábbi verziók által létrehozott metastores.

a következő diagram hello nyújt útmutatást a Hive munkaterhelések áttelepítését.

| A Windows-alapú, használni... | A Linux-alapú... |
| --- | --- |
| **Hive szerkesztő** |[Az Ambari Hive nézete](hdinsight-hadoop-use-hive-ambari-view.md) |
| `set hive.execution.engine=tez;`Tez tooenable |Tez hello alapértelmezett végrehajtó motorja Linux-alapú fürtökhöz, így hello utasítás beállítása már nem szükséges. |
| C# felhasználó által definiált függvények | Információ a C#-összetevők Linux-alapú HDInsight érvényesítése: [át .NET megoldások tooLinux-alapú HDInsight](hdinsight-hadoop-migrate-dotnet-to-linux.md) |
| CMD-fájlokat vagy parancsprogramokat meghívni egy Hive-feladat részeként hello kiszolgálón |Bash parancsfájlok használata |
| `hive`a távoli asztal parancs |Használjon [Beeline](hdinsight-hadoop-use-hive-beeline.md) vagy [SSH-munkamenetet a Hive](hdinsight-hadoop-use-hive-ssh.md) |

### <a name="pig"></a>Pig

| A Windows-alapú, használni... | A Linux-alapú... |
| --- | --- |
| C# felhasználó által definiált függvények | Információ a C#-összetevők Linux-alapú HDInsight érvényesítése: [át .NET megoldások tooLinux-alapú HDInsight](hdinsight-hadoop-migrate-dotnet-to-linux.md) |
| CMD-fájlokat vagy parancsprogramokat a Pig feladatot részeként meghívott hello kiszolgálón |Bash parancsfájlok használata |

### <a name="mapreduce"></a>MapReduce

| A Windows-alapú, használni... | A Linux-alapú... |
| --- | --- |
| C#-hozzárendelést és nyomáscsökkentő összetevők | Információ a C#-összetevők Linux-alapú HDInsight érvényesítése: [át .NET megoldások tooLinux-alapú HDInsight](hdinsight-hadoop-migrate-dotnet-to-linux.md) |
| CMD-fájlokat vagy parancsprogramokat meghívni egy Hive-feladat részeként hello kiszolgálón |Bash parancsfájlok használata |

## <a name="oozie"></a>Oozie

> [!IMPORTANT]
> Ha egy külső Oozie metaadattárhoz használ, készítsen biztonsági másolatot hello metaadattárhoz és a Linux-alapú HDInsight együttes használatához. Linux-alapú HDInsight egy Oozie, amelynek kompatibilitási problémák lehet újabb verziója érhető el a korábbi verziók által létrehozott metastores.

Oozie munkafolyamatok lehetővé teszik a rendszerhéj műveleteket. Rendszerhéj műveletek hello operációs rendszer toorun parancssori parancsokat hello alapértelmezett rendszerhéját használja. Ha a rendszerhéj Windows hello használó munkafolyamatok Oozie, meg kell írniuk hello munkafolyamatok toorely hello Linux rendszerhéj környezetben (Bash). Rendszerhéj műveletek használatáról az Oozie további információkért lásd: [Oozie rendszerhéj művelet bővítmény](http://oozie.apache.org/docs/3.3.0/DG_ShellActionExtension.html).

Ha a C# alkalmazás rendszerhéj műveletek elindításra használó munkafolyamatok Oozie, ellenőrizni kell ezeket az alkalmazásokat egy Linux-környezetben. További információkért lásd: [át .NET megoldások tooLinux-alapú HDInsight](hdinsight-hadoop-migrate-dotnet-to-linux.md).

## <a name="storm"></a>Storm

| A Windows-alapú, használni... | A Linux-alapú... |
| --- | --- |
| A Storm irányítópultja |a Storm irányítópultja hello nem érhető el. Lásd: [a Linux-alapú HDInsight központi telepítése és kezelése Storm topológiák](hdinsight-storm-deploy-monitor-topology-linux.md) a módon toosubmit topológiák |
| A Storm felhasználói felülete |hello Storm felhasználói felülete https://CLUSTERNAME.azurehdinsight.net/stormui címen érhető el |
| A Visual Studio toocreate, telepítéséhez és felügyeletéhez a C# vagy hibrid topológiák |Visual Studio használt toocreate kell, telepítése és kezelése a C# (SCP.NET) vagy a Linux-alapú Storm on HDInsight-fürtök létrehozása után 10/28/2016 hibrid topológiák. |

## <a name="hbase"></a>HBase

Linux-alapú fürtökön HBase hello znode szülője van `/hbase-unsecure`. Ezt az értéket állíthat be hello konfigurációs natív HBase Java API-t használó Java ügyfél alkalmazásokhoz.

Lásd: [létre egy Java-alapú HBase-alkalmazás](hdinsight-hbase-build-java-maven.md) egy példa ügyfél, amely beállítja ezt az értéket.

## <a name="spark"></a>Spark

A Spark-fürtök előzetes érhető el a Windows-fürtök volt. Spark GA esetén csak Linux-alapú fürtökön érhető el. Nincs a Windows-alapú Spark preview fürt tooa kiadás Linux-alapú Spark-fürt áttelepítési útvonal.

## <a name="known-issues"></a>Ismert problémák

### <a name="azure-data-factory-custom-net-activities"></a>Az Azure Data Factory egyéni .NET-tevékenységek

Az Azure Data Factory egyéni .NET-tevékenységek jelenleg nem támogatottak a Linux-alapú HDInsight-fürtökön. Ehelyett hello módszerek tooimplement egyéni tevékenységeket a ADF folyamat részeként a következő egyikét kell használnia.

* .NET-tevékenységek végrehajtása az Azure Batch-készlet. Című rész hello kapcsolódó használata Azure Batch szolgáltatás [egyéni tevékenységeket használni egy Azure Data Factory-folyamat](../data-factory/data-factory-use-custom-activities.md)
* A MapReduce művelethez hello tevékenység végrehajtása. További információkért lásd: [MapReduce program meghívása az adat-előállító](../data-factory/data-factory-map-reduce.md).

### <a name="line-endings"></a>Sorvégződések

Windows-alapú rendszereken sorvégeinek általában használja a CRLF, a Linux-alapú rendszerek LF használja. Ha előállítani, vagy várja, adatok CRLF sorvégeket, szükség lehet a toomodify hello gyártók vagy fogyasztók hello LF sor befejezése a toowork.

Például tooquery Azure PowerShell használatával Windows-alapú fürt HDInsight adatait jeleníti meg a CRLF. hello ugyanabban a lekérdezésben egy Linux-alapú fürttel adja vissza LF. Ha a hello sor befejezése a solutuion problémája miatt áttelepítése előtt kell tesztelni toosee tooa Linux rendszerű fürt.

Ha vannak olyan parancsprogramjai, amelyek közvetlenül a hello Linux-fürt csomópontjain, mindig célszerű használni LF hello sor befejezése. Ha CRLF használ, előfordulhat, hogy hibába ütközik, egy Linux-alapú fürtön hello parancsfájlok futtatásakor.

Ha tudja, hogy hello parancsfájlok tartalmaz beágyazott CR karakter karakterláncok, tömegesen módosítás hello sorvégződések hello a következő módszerek egyikével:

* **Toohello fürt feltöltés előtt**: használata hello CRLF tooLF PowerShell utasítások toochange hello sorvégződések követően – hello parancsfájl toohello fürt feltöltés előtt.

    ```powershell
    $original_file ='c:\path\to\script.py'
    $text = [IO.File]::ReadAllText($original_file) -replace "`r`n", "`n"
    [IO.File]::WriteAllText($original_file, $text)
    ```

* **Toohello fürt feltöltése után**: használata hello következő parancsot a egy SSH-munkamenet toohello fürt Linux-alapú toomodify hello parancsfájl.

    ```bash
    hdfs dfs -get wasb:///path/to/script.py oldscript.py
    tr -d '\r' < oldscript.py > script.py
    hdfs dfs -put -f script.py wasb:///path/to/script.py
    ```

## <a name="next-steps"></a>Következő lépések

* [Ismerje meg, hogyan toocreate Linux-alapú HDInsight-fürtök](hdinsight-hadoop-provision-linux-clusters.md)
* [SSH tooconnect tooHDInsight használata](hdinsight-hadoop-linux-use-ssh-unix.md)
* [A Linux-alapú fürt Ambari kezelése](hdinsight-hadoop-manage-ambari.md)
