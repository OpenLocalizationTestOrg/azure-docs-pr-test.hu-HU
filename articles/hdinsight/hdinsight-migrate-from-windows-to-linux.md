---
title: "Áttelepítheti a Windows-alapú HDInsight Linux-alapú HDInsight - Azure |} Microsoft Docs"
description: "Megtudhatja, hogyan telepíthetők át egy Windows-alapú HDInsight-fürtöt egy Linux-alapú HDInsight-fürtöt."
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
ms.openlocfilehash: 35e80efe27081cd43243f488fa60447b76a20c32
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/03/2017
---
# <a name="migrate-from-a-windows-based-hdinsight-cluster-to-a-linux-based-cluster"></a>Linux-alapú fürtre Windows-alapú HDInsight-fürtök áttelepítése

Ez a dokumentum a Windows és Linux HDInsight és útmutatás meglévő munkaterheléseket telepít át egy Linux-alapú fürt közötti különbségekről részleteit.

Amíg a Windows-alapú HDInsight Hadoop-ban történő használatáról a felhő egyszerű lehetőséget biztosít, szükség lehet egy Linux-alapú fürtbe történő áttelepítéséhez. Például Linux-alapú eszközök és technológiák a megoldás a szükséges mértékben történő kihasználásához. A Hadoop ökoszisztémájának számos elemet a Linux-alapú rendszereken fejlesztett, és nem lehet a Windows-alapú hdinsight eszközzel használható. Emellett számos könyvek, videókat és egyéb oktatóanyag feltételezi Linux rendszert a használ, amikor olyan Hadoop.

> [!NOTE]
> A HDInsight-fürtök Ubuntu hosszú távú támogatási (LTS) használja az operációs rendszert a fürt csomópontjaihoz. A HDInsight az elérhető Ubuntu verziójának más összetevők verziószámozása információk mellett további információkért lásd: [HDInsight összetevő verziók](hdinsight-component-versioning.md).

## <a name="migration-tasks"></a>Áttelepítési feladatok

Az áttelepítés általános munkafolyamata a következőképpen történik.

![Áttelepítési munkafolyamat diagramja](./media/hdinsight-migrate-from-windows-to-linux/workflow.png)

1. Olvassa el a dokumentum megérteni a módosításokat, ha a meglévő munkafolyamat, feladatok, stb. Linux-alapú fürtre történő van szükség minden egyes szakaszát.

2. Hozzon létre egy Linux-alapú fürtöt, a teszt/minőségi megbízhatósági környezetekben. A Linux-alapú fürtök létrehozásáról további információk: [hdinsight létrehozása Linux-alapú fürtökön](hdinsight-hadoop-provision-linux-clusters.md).

3. Másolja a meglévő feladatokat, az adatforrások és mosdók az új környezetbe.

4. Hajtsa végre az ellenőrzés alá vonni, győződjön meg arról, hogy a feladatok az új fürt a várt módon működik-e.

Miután ellenőrizte, hogy minden megfelelően működik-e, az áttelepítés tervezze. Üzemszünet, során a következő műveleteket:

1. Készítsen biztonsági másolatot a fürtcsomópontokon helyileg tárolt átmeneti adatok. Ha például közvetlenül egy átjárócsomóponttal tárolt adatokat.

2. A Windows-alapú fürt törlésekor.

3. Alapértelmezett ugyanazon adattár, a Windows-alapú fürt által használt Linux-alapú fürtöt létrehozni. A Linux-alapú fürt folytathatják a munkát a meglévő éles adatok alapján.

4. Biztonsági másolatot készített az átmeneti adatok importálása.

5. Kezdő feladatok/folytatni az új fürt segítségével.

### <a name="copy-data-to-the-test-environment"></a>A tesztkörnyezet adatok másolása

Másolja az adatokat, a feladatok számos módszer áll rendelkezésre, azonban a jelen szakaszban bemutatott két a legegyszerűbb módszer áll közvetlenül helyezze át a fájlokat egy tesztfürthöz.

#### <a name="hdfs-copy"></a>HDFS másolása

Az alábbi lépéseket követve másolja át az adatok a termelési fürtből a tesztfürthöz. Az alábbi a `hdfs dfs` segédprogram, amely tartalmazza a hdinsight eszközzel.

1. A szolgáltatás fiók és az alapértelmezett tároló adatai a meglévő fürt található. A következő példában PowerShell ezek az információk beolvasása:

    ```powershell
    $clusterName="Your existing HDInsight cluster name"
    $clusterInfo = Get-AzureRmHDInsightCluster -ClusterName $clusterName
    write-host "Storage account name: $clusterInfo.DefaultStorageAccount.split('.')[0]"
    write-host "Default container: $clusterInfo.DefaultStorageContainer"
    ```

2. Egy tesztkörnyezetben létrehozásához kövesse a HDInsight-dokumentum létrehozása Linux-alapú fürtök. A fürt létrehozása előtt állítsa le, és ehelyett válassza **opcionális konfigurációs**.

3. Opcionális konfigurációs panelen válassza ki a **a társított Tárfiókokban**.

4. Válassza ki **tárolási kulcs hozzáadása**, és amikor a rendszer kéri, válassza ki a tárfiókot, az 1. lépésben a PowerShell-parancsfájl által visszaadott. Kattintson a **válasszon** minden panelen. Végezetül hozza létre a fürtöt.

5. A fürt létrehozása után használatával kapcsolódik hozzá **SSH.** További információ: [Az SSH használata HDInsighttal](hdinsight-hadoop-linux-use-ssh-unix.md).

6. Az SSH-munkamenetből a következő paranccsal fájlok másolását a kapcsolt tárfiókra az új alapértelmezett tárfiók. Cserélje le a tároló PowerShell által visszaadott tároló adatokkal. Cserélje le __fiók__ , fiókja néven. Cserélje le az adatok elérési útja az elérési útját egy adatfájlt.

    ```bash
    hdfs dfs -cp wasb://CONTAINER@ACCOUNT.blob.core.windows.net/path/to/old/data /path/to/new/location
    ```

    > [!NOTE]
    > Ha a könyvtárstruktúra, amely tartalmazza az adatokat nem létezik a tesztkörnyezetben, létrehozhatja a következő parancsot:

    ```bash
    hdfs dfs -mkdir -p /new/path/to/create
    ```

    A `-p` kapcsoló létrehozását lehetővé tevő összes könyvtár az elérési út.

#### <a name="direct-copy-between-blobs-in-azure-storage"></a>Az Azure Storage blobs közötti közvetlen másolása

Azt is megteheti, érdemes lehet használni a `Start-AzureStorageBlobCopy` Azure PowerShell-parancsmag segítségével másolja át a BLOB storage-fiókok HDInsight eszközön kívüli között. További információkért lásd: a hogyan kezelheti az Azure PowerShell használata az Azure Storage Azure BLOB szakasza.

## <a name="client-side-technologies"></a>Ügyféloldali technológiák

Ügyféloldali technológiák többek között [Azure PowerShell-parancsmagok](/powershell/azureps-cmdlets-docs), [Azure CLI](../cli-install-nodejs.md), vagy a [.NET SDK for Hadoop](https://hadoopsdk.codeplex.com/) Linux-alapú fürtökön működni. Ezek a technológiák a REST API-k, amelyek azonos mindkét fürttípusok az operációs rendszer különböző támaszkodnak.

## <a name="server-side-technologies"></a>Kiszolgálóoldali technológiák

A következő táblázat áttelepítése kiszolgálóoldali összetevőket, amelyek Windows-specifikus nyújt útmutatást.

| Ha ezt a technológiát használja... | Ez a művelet végrehajtása... |
| --- | --- |
| **PowerShell** (kiszolgálóoldali parancsfájlok, beleértve a fürt létrehozása során használt Parancsfájlműveletek) |Az újraírási, Bash parancsfájlok. A Parancsfájlműveletek, lásd: [testreszabása Linux-alapú HDInsight parancsfájlműveletekkel](hdinsight-hadoop-customize-cluster-linux.md) és [parancsfájl-művelet fejlesztési a Linux-alapú HDInsight](hdinsight-hadoop-script-actions-linux.md). |
| **Az Azure CLI** (kiszolgálóoldali parancsfájlok) |Az Azure parancssori felület Linux rendszeren érhető el, amíg azt nem olyan előre telepített a HDInsight fürt központi csomópontján. Az Azure parancssori felület telepítésével kapcsolatos további információkért lásd: [Ismerkedés az Azure CLI 2.0](https://docs.microsoft.com/cli/azure/get-started-with-azure-cli). |
| **.NET-összetevők** |.NET támogatott Linux-alapú HDInsight segítségével [monó](https://mono-project.com). További információkért lásd: [át .NET Linux-alapú HDInsight-megoldások](hdinsight-hadoop-migrate-dotnet-to-linux.md). |
| **A Win32-összetevők vagy egyéb csak Windows technológia** |Útmutatás az összetevő vagy a technológia függ. Esetleg található, amely kompatibilis a Linux-verzió, vagy egy másik megoldás található, vagy írja át az összetevő szükség lehet. |

> [!IMPORTANT]
> A HDInsight-kezelési SDK nincs teljes mértékben kompatibilis a monó. Nem használandó jelenleg a HDInsight-fürthöz telepített megoldások részeként.

## <a name="cluster-creation"></a>A fürt létrehozása

Ez a szakasz tájékoztatást nyújt a fürt létrehozása különbségeit.

### <a name="ssh-user"></a>SSH felhasználó

Linux-alapú HDInsight-fürtök használata a **Secure Shell (SSH)** protokollt, hogy az a fürt csomópontjai távoli hozzáférést biztosítanak. Távoli asztal a Windows-alapú fürtök esetében eltérően a legtöbb SSH-ügyfél nem biztosítanak egy grafikus felhasználói felület. Ehelyett SSH-ügyfél adjon meg egy parancssort, amely lehetővé teszi, hogy parancsokat futtatnak majd a fürt. Egyes ügyfelek (például [MobaXterm](http://mobaxterm.mobatek.net/)) adjon meg egy grafikus fájl rendszer böngésző távoli parancssor mellett.

Fürt létrehozása, és meg kell adnia az SSH-felhasználó vagy egy **jelszó** vagy **nyilvános kulcsú tanúsítvány** hitelesítéshez.

Azt javasoljuk, nyilvános kulcsú tanúsítvány, mert az biztonságosabb a jelszót. Tanúsítványhitelesítés aláírt nyilvános/titkos kulcspár előállításához, akkor a nyilvános kulcs megadása a fürt létrehozásakor működik. A kiszolgálóhoz való csatlakozáskor a titkos kulcsot az ügyfél biztosítja a hitelesítést a kapcsolathoz.

További információ: [Az SSH használata HDInsighttal](hdinsight-hadoop-linux-use-ssh-unix.md).

### <a name="cluster-customization"></a>Fürt testreszabása

**Parancsfájl-műveletek** használt Linux-alapú fürtök úgy kell megírni, a Bash parancsfájlok. Parancsfájlműveletek fürt létrehozása során is használható, amíg a Linux-alapú fürtök is lehetnek használt testreszabási végrehajtásához, miután a fürt működik-e és fut. További információkért lásd: [testreszabása Linux-alapú HDInsight parancsfájlműveletekkel](hdinsight-hadoop-customize-cluster-linux.md) és [parancsfájl-művelet fejlesztési a Linux-alapú HDInsight](hdinsight-hadoop-script-actions-linux.md).

Egy másik testreszabási a szolgáltatás **bootstrap**. Windows-fürtök esetén ez a funkció lehetővé teszi további könyvtárak használatra helyének megadása a Hive. Fürt létrehozása után ezek a könyvtárak használhatók automatikusan a Hive-lekérdezések használata nélkül `ADD JAR`.

Linux-alapú fürtökhöz a rendszerindítási szolgáltatás nem nyújt ezt a funkciót. Ehelyett használja a dokumentált parancsfájlművelet [szalagtárak Hive hozzáadása a fürt létrehozása során](hdinsight-hadoop-add-hive-libraries.md).

### <a name="virtual-networks"></a>Virtuális hálózatok

Windows-alapú HDInsight-fürtök a klasszikus virtuális hálózatok csak használata pedig Linux-alapú HDInsight-fürtök igényelnek erőforrás-kezelő virtuális hálózatok. Ha a klasszikus virtuális hálózatot, amely a Linux-HDInsight-fürt csatlakoztatni kell az erőforrásokat, olvassa el [a klasszikus virtuális hálózatot egy erőforrás-kezelő virtuális hálózathoz való csatlakozás](../vpn-gateway/vpn-gateway-connect-different-deployment-models-portal.md).

Az Azure virtuális hálózatok és a HDInsight együttes használatával konfigurációkra vonatkozó további információkért lásd: [kiterjesztése HDInsight képességek a virtuális hálózat segítségével](hdinsight-extend-hadoop-virtual-network.md).

## <a name="management-and-monitoring"></a>Management and monitoring

A web UI használta a Windows-alapú hdinsight eszközzel, például a feladatelőzmények vagy a Yarn felhasználói felületen, számos Ambari keresztül érhető el. Emellett az Ambari Hive View megoldást egy webböngésző segítségével Hive-lekérdezések futtatásához. Az Ambari webes felhasználói felületén Linux-alapú fürtökön https://CLUSTERNAME.azurehdinsight.net címen érhető el.

Ambari használatával kapcsolatos további információkért lásd a következő dokumentumokat:

* [Ambari webes](hdinsight-hadoop-manage-ambari.md)
* [Ambari REST API-n](hdinsight-hadoop-manage-ambari-rest-api.md)

### <a name="ambari-alerts"></a>Ambari riasztások

Ambari, amelyre egy riasztási rendszer, amelyek segítségével megállapíthatja, hogy a fürt érintő lehetséges problémákat. Az Ambari webes felhasználói felületén, sárga vagy piros bejegyzésként riasztás jelenik meg, de is helyreállíthatók a REST API-n keresztül.

> [!IMPORTANT]
> Ambari riasztás jelzi, hogy *előfordulhat, hogy* probléma, nem az, hogy legyenek *van* probléma. Kaphat például, hogy hiveserver2-n nem érhető el, riasztást, annak ellenére, hogy a szokásos módon el tudja érni azt.
>
> Sok riasztást időközönkénti lekérdezések írásában, a szolgáltatás megvalósítása, és egy megadott időkereten belül választ várt. A riasztás nem feltétlenül jelenti azt, hogy a szolgáltatás le van állítva, így most, hogy nem adja vissza az eredményeket a várt időn belül.

Ki kell értékelnie figyelmeztetés fordult elő hosszabb ideig, vagy felhasználói problémák rajta megtétele előtt jelentett tükrözi.

## <a name="file-system-locations"></a>Rendszer helye

A Linux-fürt fájlrendszer működnek, mint a Windows-alapú HDInsight-fürtök elrendezését. A következő táblázattal általánosan használt fájlok.

| Kell keresése... | Helyezkedik... |
| --- | --- |
| Konfiguráció |`/etc`. Például: `/etc/hadoop/conf/core-site.xml` |
| Naplófájlok |`/var/logs` |
| Hortonworks Data Platform (HDP) |`/usr/hdp`. Nincsenek a két címtár itt található, egy, az aktuális HDP verzió és `current`. A `current` directory fájlok és könyvtárak verzió száma könyvtárában található szimbolikus hivatkozásokat tartalmaz. A `current` directory kerül a HDP fájlok elérésének a verzió megváltozása óta, a HDP könnyen verzióra frissül. |
| hadoop-streaming.jar |`/usr/hdp/current/hadoop-mapreduce-client/hadoop-streaming.jar` |

Ismeri annak a fájlnak a nevét, is található a fájl elérési útja általában használja az SSH-munkamenetet a következő parancsot:

    find / -name FILENAME 2>/dev/null

A fájlnév helyettesítő karaktereket is használható. Például `find / -name *streaming*.jar 2>/dev/null` elérési útját "streaming" a fájlnév részét képező szót tartalmazó jar fájlok adja.

## <a name="hive-pig-and-mapreduce"></a>Hive, Pig és MapReduce

A Pig és a MapReduce munkaterhelések hasonlóak a Linux-alapú fürtökön. Azonban Linux-alapú HDInsight-fürtök hozhatók létre Hadoop Hive és a Pig újabb verzióját használja. Ezen verzió különbségek vezethetnek hogyan változásairól a meglévő megoldásokat függvény. A HDInsight részét alkotó verzióin további információkért lásd: [HDInsight-összetevők verziószámozása](hdinsight-component-versioning.md).

Linux-alapú HDInsight nem biztosít a távoli asztali funkciókat. Ehelyett az SSH segítségével távolról kapcsolódni a központi fürtcsomópontokon. További információkért lásd a következő dokumentumokat:

* [A Hive használata a SSH](hdinsight-hadoop-use-hive-ssh.md)
* [A Pig használata SSH](hdinsight-hadoop-use-pig-ssh.md)
* [SSH MapReduce használata](hdinsight-hadoop-use-mapreduce-ssh.md)

### <a name="hive"></a>Hive

> [!IMPORTANT]
> Ha egy külső Hive metaadattárhoz használ, készítsen biztonsági másolatot a metaadattárhoz és a Linux-alapú HDInsight együttes használatához. Linux-alapú HDInsight egy struktúra, amelynek kompatibilitási problémák lehet újabb verziója érhető el a korábbi verziók által létrehozott metastores.

Az alábbi ábra a Hive munkaterhelések áttelepítését nyújt útmutatást.

| A Windows-alapú, használni... | A Linux-alapú... |
| --- | --- |
| **Hive szerkesztő** |[Az Ambari Hive nézete](hdinsight-hadoop-use-hive-ambari-view.md) |
| `set hive.execution.engine=tez;`Tez engedélyezése |Tez érték az alapértelmezett végrehajtó motorja Linux-alapú fürtökhöz, ezért már nem szükséges a set utasítás. |
| C# felhasználó által definiált függvények | Információ a C#-összetevők Linux-alapú HDInsight érvényesítése: [át .NET Linux-alapú HDInsight-megoldások](hdinsight-hadoop-migrate-dotnet-to-linux.md) |
| CMD-fájlokat vagy parancsprogramokat meghívni egy Hive-feladat részeként a kiszolgálón |Bash parancsfájlok használata |
| `hive`a távoli asztal parancs |Használjon [Beeline](hdinsight-hadoop-use-hive-beeline.md) vagy [SSH-munkamenetet a Hive](hdinsight-hadoop-use-hive-ssh.md) |

### <a name="pig"></a>Pig

| A Windows-alapú, használni... | A Linux-alapú... |
| --- | --- |
| C# felhasználó által definiált függvények | Információ a C#-összetevők Linux-alapú HDInsight érvényesítése: [át .NET Linux-alapú HDInsight-megoldások](hdinsight-hadoop-migrate-dotnet-to-linux.md) |
| CMD-fájlokat vagy parancsprogramokat a kiszolgálón, a Pig feladatot részeként meghívni |Bash parancsfájlok használata |

### <a name="mapreduce"></a>MapReduce

| A Windows-alapú, használni... | A Linux-alapú... |
| --- | --- |
| C#-hozzárendelést és nyomáscsökkentő összetevők | Információ a C#-összetevők Linux-alapú HDInsight érvényesítése: [át .NET Linux-alapú HDInsight-megoldások](hdinsight-hadoop-migrate-dotnet-to-linux.md) |
| CMD-fájlokat vagy parancsprogramokat meghívni egy Hive-feladat részeként a kiszolgálón |Bash parancsfájlok használata |

## <a name="oozie"></a>Oozie

> [!IMPORTANT]
> Ha egy külső Oozie metaadattárhoz használ, készítsen biztonsági másolatot a metaadattárhoz és a Linux-alapú HDInsight együttes használatához. Linux-alapú HDInsight egy Oozie, amelynek kompatibilitási problémák lehet újabb verziója érhető el a korábbi verziók által létrehozott metastores.

Oozie munkafolyamatok lehetővé teszik a rendszerhéj műveleteket. Rendszerhéj műveletek az operációs rendszer alapértelmezett rendszerhéját használja a parancssori parancsok futtatásához. Ha Oozie-munkafolyamatok, amelyek a Windows rendszerhéj támaszkodnak, meg kell írniuk a munkafolyamatokat a Linux-rendszerhéj környezet (Bash) támaszkodnak. Rendszerhéj műveletek használatáról az Oozie további információkért lásd: [Oozie rendszerhéj művelet bővítmény](http://oozie.apache.org/docs/3.3.0/DG_ShellActionExtension.html).

Ha a C# alkalmazás rendszerhéj műveletek elindításra használó munkafolyamatok Oozie, ellenőrizni kell ezeket az alkalmazásokat egy Linux-környezetben. További információkért lásd: [át .NET Linux-alapú HDInsight-megoldások](hdinsight-hadoop-migrate-dotnet-to-linux.md).

## <a name="storm"></a>Storm

| A Windows-alapú, használni... | A Linux-alapú... |
| --- | --- |
| A Storm irányítópultja |A Storm irányítópultjának nem érhető el. Lásd: [a Linux-alapú HDInsight központi telepítése és kezelése Storm topológiák](hdinsight-storm-deploy-monitor-topology-linux.md) küldeni topológiákat módon a |
| A Storm felhasználói felülete |A Storm felhasználói felülete https://CLUSTERNAME.azurehdinsight.net/stormui címen érhető el |
| A Visual Studio létrehozásához, telepítéséhez és felügyeletéhez a C# vagy hibrid topológiák |A Visual Studio létrehozásához, telepítéséhez és felügyeletéhez a C# (SCP.NET) vagy a Linux-alapú Storm on HDInsight-fürtök létrehozása után 10/28/2016 hibrid topológiák is használható. |

## <a name="hbase"></a>HBase

Linux-alapú fürtökön a HBase znode szülője van `/hbase-unsecure`. Állítsa be ezt az értéket a konfigurációban a Java-ügyfél natív HBase Java API-t használó alkalmazások.

Lásd: [létre egy Java-alapú HBase-alkalmazás](hdinsight-hbase-build-java-maven.md) egy példa ügyfél, amely beállítja ezt az értéket.

## <a name="spark"></a>Spark

A Spark-fürtök előzetes érhető el a Windows-fürtök volt. Spark GA esetén csak Linux-alapú fürtökön érhető el. Nincs fürtről a Windows-alapú Spark előzetes kiadás Linux-alapú Spark-fürt az áttelepítési útvonal.

## <a name="known-issues"></a>Ismert problémák

### <a name="azure-data-factory-custom-net-activities"></a>Az Azure Data Factory egyéni .NET-tevékenységek

Az Azure Data Factory egyéni .NET-tevékenységek jelenleg nem támogatottak a Linux-alapú HDInsight-fürtökön. Ehelyett használjon a következő módszerek egyikét a ADF folyamat részeként egyéni tevékenységek végrehajtásához.

* .NET-tevékenységek végrehajtása az Azure Batch-készlet. További részletekért lásd a használata Azure Batch szolgáltatás szakasza kapcsolódó [egyéni tevékenységeket használni egy Azure Data Factory-folyamat](../data-factory/data-factory-use-custom-activities.md)
* A MapReduce művelethez a tevékenység végrehajtása. További információkért lásd: [MapReduce program meghívása az adat-előállító](../data-factory/data-factory-map-reduce.md).

### <a name="line-endings"></a>Sorvégződések

Windows-alapú rendszereken sorvégeinek általában használja a CRLF, a Linux-alapú rendszerek LF használja. Ha előállítani, vagy várja, adatok CRLF sorvégeket, esetleg módosítsa a gyártó vagy a fogyasztók történő együttműködésre a LF sor befejezése.

Például Azure PowerShell használatával történő lekérdezés HDInsight egy Windows-alapú fürtön adatait jeleníti meg a CRLF. Egy Linux-alapú fürttel ugyanabban a lekérdezésben LF adja vissza. Kell ellenőrizze, hogy ha a sor befejezése előtt Linux-alapú fürtre történő a solutuion probléma okozza.

Ha vannak olyan parancsprogramjai, amelyek közvetlenül a Linux-fürt csomópontjain, mindig célszerű használni LF a sor befejezése. Ha CRLF használ, előfordulhat, hogy hibába ütközik, amikor futtatja a parancsfájlokat egy Linux-alapú fürtön.

Ha tudja, hogy a parancsfájlok nem tartalmaz beágyazott CR karakter karakterláncok, tömegesen módosítása a sorvégződések, az alábbi módszerek egyikének használatával:

* **A fürt való feltöltés előtt**: a következő PowerShell-utasítások segítségével módosítsa a sorvégződések CRLF értékről LF a parancsfájl a fürthöz való feltöltés előtt.

    ```powershell
    $original_file ='c:\path\to\script.py'
    $text = [IO.File]::ReadAllText($original_file) -replace "`r`n", "`n"
    [IO.File]::WriteAllText($original_file, $text)
    ```

* **A fürthöz feltöltése után**: a Linux-alapú fürthöz SSH-munkamenetet a következő parancs segítségével módosítsa a parancsfájlt.

    ```bash
    hdfs dfs -get wasb:///path/to/script.py oldscript.py
    tr -d '\r' < oldscript.py > script.py
    hdfs dfs -put -f script.py wasb:///path/to/script.py
    ```

## <a name="next-steps"></a>Következő lépések

* [Ismerje meg a Linux-alapú HDInsight-fürtök létrehozása](hdinsight-hadoop-provision-linux-clusters.md)
* [Az SSH használata a HDInsight való kapcsolódáshoz](hdinsight-hadoop-linux-use-ssh-unix.md)
* [A Linux-alapú fürt Ambari kezelése](hdinsight-hadoop-manage-ambari.md)
