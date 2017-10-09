---
title: "Hadoop, Spark, Kafka, HBase vagy R Server - Azure HDInsight aaaCluster beállítása |} Microsoft Docs"
description: "Hadoop, Kafka, Spark, HBase, R Server, vagy a Storm fürtök beállítása a HDInsight egy böngészőt, hello Azure CLI-t, Azure PowerShell, REST vagy SDK."
keywords: "hadoop-fürt beállítása, kafka fürtbeállítás, spark-fürt beállítása, mi az hadoop-fürt"
services: hdinsight
documentationcenter: 
author: mumian
manager: jhubbard
editor: cgronlun
tags: azure-portal
ms.assetid: 23a01938-3fe5-4e2e-8e8b-3368e1bbe2ca
ms.service: hdinsight
ms.custom: hdinsightactive,hdiseo17may2017
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 06/06/2017
ms.author: jgao
ms.openlocfilehash: 80ec59d8a39f7fccb940503fd2dc3ae5afee6bcf
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="set-up-clusters-in-hdinsight-with-hadoop-spark-kafka-and-more"></a>Hdinsight Hadoop, Spark, Kafka és több fürt beállítása

[!INCLUDE [selector](../../includes/hdinsight-create-linux-cluster-selector.md)]

Megtudhatja, hogyan tooset össze, és a HDInsight Hadoop, Spark, Kafka, interaktív struktúra, HBase, R Server, vagy a Storm fürt konfigurálását. Emellett ismerje meg, hogyan toocustomize fürtök, és adja hozzá a biztonsági tooa tartományhoz való csatlakozás.

A Hadoop fürtök több virtuális gépek (csomópontok), a feladatok elosztott feldolgozásához használt áll. Az Azure HDInsight kezeli a megvalósítás részletei telepítési és konfigurációs az egyes csomópontokon, így csak tooprovide általános konfigurációs adatait. 

> [!IMPORTANT]
>HDInsight-fürt számlázási elindul, miután a fürt jön létre, és leállítja a hello fürt törlésekor. Számlázási, egyenletesen percenként történik, így mindig törölnie kell a fürthöz, amikor már nincs használatban. Ismerje meg, hogyan túl[törölheti a fürtöt.](hdinsight-delete-cluster.md)
>

## <a name="cluster-setup-methods"></a>Fürt telepítési módszer
hello következő táblázatban hello különböző módszereket is használhatja fel a HDInsight-fürtök tooset.

| Létrehozott fürtök | Webböngésző | Parancssor | REST API | SDK | 
| --- |:---:|:---:|:---:|:---:|
| [Azure Portal](hdinsight-hadoop-create-linux-clusters-portal.md) |✔ |&nbsp; |&nbsp; |&nbsp; |
| [Azure Data Factory](hdinsight-hadoop-create-linux-clusters-adf.md) |✔ |✔ |✔ |✔ |
| [Azure CLI](hdinsight-hadoop-create-linux-clusters-azure-cli.md) |&nbsp; |✔ |&nbsp; |&nbsp; |
| [Azure PowerShell](hdinsight-hadoop-create-linux-clusters-azure-powershell.md) |&nbsp; |✔ |&nbsp; |&nbsp; |
| [cURL](hdinsight-hadoop-create-linux-clusters-curl-rest.md) |&nbsp; |✔ |✔ |&nbsp; |
| [.NET SDK](hdinsight-hadoop-create-linux-clusters-dotnet-sdk.md) |&nbsp; |&nbsp; |&nbsp; |✔ |
| [Az Azure Resource Manager-sablonok](hdinsight-hadoop-create-linux-clusters-arm-templates.md) |&nbsp; |✔ |&nbsp; |&nbsp; |

## <a name="quick-create-basic-cluster-setup"></a>Gyorslétrehozás: alapvető fürt beállítása
Ez a cikk végigvezeti a hello beállításának [Azure-portálon](https://portal.azure.com), ahol hozhat létre egy HDInsight fürt *Gyorslétrehozás* vagy *egyéni*. 

Kövesse az utasításokat a hello képernyő toodo egy alapszintű fürt beállítása. Részletek az alább:

* [Az erőforráscsoport neve](#resource-group-name)
* [Fürt típusa és konfigurálása](#cluster-types) 
* [Fürt bejelentkezési és az SSH-felhasználónév](#cluster-login-and-ssh-username)
* [Hely](#location)

> [!IMPORTANT]
> Linux hello azt az egyetlen operációs rendszer, használja a HDInsight 3.4 vagy újabb verziója. További információkért lásd: [HDInsight 3.3 használatból való kivonást](hdinsight-component-versioning.md#hdinsight-windows-retirement).
>

## <a name="resource-group-name"></a>Az erőforráscsoport neve 

[Az Azure Resource Manager](../azure-resource-manager/resource-group-overview.md) hello az erőforrásokkal való munka az alkalmazásban, a csoporthoz, hivatkozott tooas egy Azure erőforráscsoport segítségével. Központi telepítése, frissítése, figyelheti vagy hello összes erőforrást törli az alkalmazás egyetlen, koordinált műveletben.

## <a name="cluster-types"></a>Fürt típusa és konfigurálása
Az Azure HDInsight tartalmaz hello következő fürt típusok, az összetevők tooprovide készlete bizonyos funkciók.

> [!IMPORTANT]
> A HDInsight-fürtök különböző egyetlen számítási feladat vagy technológia típusok érhetők el. Nincs nem támogatott metódus toocreate egy fürt, amely többféle, például a Storm és HBase egy fürtön egyesíti. Ha a megoldást igényel, amely több HDInsight-fürttípusok, vannak elosztva egy [Azure-beli virtuális hálózat](https://docs.microsoft.com/azure/virtual-network) szükséges hello fürttípusok kapcsolódhatnak. 
>
>

| Fürttípus | Funkció |
| --- | --- |
| [Hadoop](hdinsight-hadoop-introduction.md) |Kötegelt lekérdezés és a tárolt adatok elemzése |
| [HBase](hdinsight-hbase-overview.md) |Nagy mennyiségű séma nélküli, nosql típusú adatok számára történő feldolgozásakor. |
| [A Storm](hdinsight-storm-overview.md) |Valós idejű eseményfeldolgozás |
| [Spark](hdinsight-apache-spark-overview.md) |A memórián belüli feldolgozást, interaktív lekérdezések micro-kötegelt adatfolyam feldolgozása |
| [Kafka (előzetes verzió)](hdinsight-apache-kafka-introduction.md) | Egy elosztott adatfolyam platform, amely valós idejű streamelési adatok folyamatok használt toobuild és alkalmazások |
| [R Server](hdinsight-hadoop-r-server-overview.md) |Különböző big Data típusú adatok statisztika, prediktív modellezési és gépi tanulási képességek |
| [Interaktív struktúra (előzetes verzió)](hdinsight-hadoop-use-interactive-hive.md) |A memóriában történő gyorsítótárazás gyorsabb és interaktív Hive-lekérdezések |

### <a name="number-of-nodes-for-each-cluster-type"></a>Az egyes fürt a csomópontok száma
Minden egyes fürttípus csomópontok, terminológia csomópontokat, és az alapértelmezett Virtuálisgép-méretet a saját számú tartalmaz. A következő táblázat hello az egyes csomópont csomópontok száma hello zárójelek között van.

| Típus | Csomópontok | Ábra |
| --- | --- | --- |
| Hadoop |Átjárócsomópont (2) adatok csomópont (1 +) |![HDInsight Hadoop-fürt csomópontjain](./media/hdinsight-hadoop-provision-linux-clusters/hdinsight-hadoop-cluster-type-nodes.png) |
| HBase |HEAD kiszolgáló (2), a régióban (1 +), fő/ZooKeeper csomópont (3) |![HDInsight HBase fürt csomópontjain](./media/hdinsight-hadoop-provision-linux-clusters/hdinsight-hbase-cluster-type-setup.png) |
| Storm |Nimbus csomóponttal (2), felügyeleti kiszolgáló (1 +), ZooKeeper csomópont (3) |![A HDInsight alatt futó Storm-fürt csomópontjain](./media/hdinsight-hadoop-provision-linux-clusters/hdinsight-storm-cluster-type-setup.png) |
| Spark |Átjárócsomópont (2), munkavégző csomópont (1 +), ZooKeeper csomópont (3) (szabad A1 ZooKeeper Virtuálisgép-méretet a) |![HDInsight Spark-fürt csomópontjain](./media/hdinsight-hadoop-provision-linux-clusters/hdinsight-spark-cluster-type-setup.png) |

További információkért lásd: [csomópont konfigurációs és virtuális gépek méretei fürtök alapértelmezett](hdinsight-component-versioning.md#default-node-configuration-and-virtual-machine-sizes-for-clusters) a "Mik hello Hadoop-összetevők és a HDInsight-verziói?"

### <a name="hdinsight-version"></a>HDInsight-verzió
Válassza ki a fürt a HDInsight a hello verziót. További információkért lásd: [támogatott HDInsight-verziókról](hdinsight-component-versioning.md#supported-hdinsight-versions).

### <a name="cluster-tiers"></a>Fürt réteg: HDInsight szolgáltatásszintek

Az Azure HDInsight big data felhőajánlatokat hello két szolgáltatásrétegekben biztosít: Standard és Premium.  További információkért lásd: [HDInsight Standard és HDInsight prémium](hdinsight-component-versioning.md#hdinsight-standard-and-hdinsight-premium).

hello alábbi képernyőfelvételen látható hello fürt kiválasztása az Azure portál adatai.

![HDInsight prémium konfiguráció](./media/hdinsight-hadoop-provision-linux-clusters/hdinsight-cluster-type-configuration.png)


## <a name="cluster-login-and-ssh-user-name"></a>A fürt bejelentkezési és az SSH-felhasználónév
HDInsight-fürtökkel fürt létrehozása során két felhasználói fiókokat konfigurálhatja:

* Felhasználói HTTP: hello alapértelmezett felhasználónév *admin*. Az Azure-portálon hello hello alapszintű konfigurációs használ. Más néven "Fürt felhasználói."
* SSH-felhasználó (Linux-fürtök): használt tooconnect toohello fürthöz SSH-n keresztül. További információ: [Az SSH használata HDInsighttal](hdinsight-hadoop-linux-use-ssh-unix.md).

## <a name="location"></a>Fürtök és a tárolási helyét (régió)

Explicit módon nem kell toospecify hello fürt helyétől: hello fürt hello van hello alapértelmezett tárolási és ugyanazon a helyen. A támogatott régiók felsorolása, kattintson a hello **régió** legördülő listából válassza ki a [HDInsight árképzési](https://go.microsoft.com/fwLink/?LinkID=282635&clcid=0x409).

## <a name="storage-endpoints-for-clusters"></a>Tárolási végpontok fürtök

Bár a Hadoop egy helyszíni telepítését hello Hadoop elosztott fájlrendszerrel (HDFS) hello fürtön tárhelyet használ, hello felhőalapú tárolási végpontok használata csatlakoztatva toocluster. A HDInsight-fürtök bármelyikével [Azure Data Lake Store](hdinsight-hadoop-use-data-lake-store.md) vagy [az Azure Storage blobs](hdinsight-hadoop-use-blob-storage.md). Az Azure Storage vagy a Data Lake Store azt jelenti, miközben megőrzik az adatok számításhoz használt HDInsight-fürtök hello biztonságosan törölhető. 

> [!WARNING]
> Egy további storage-fiók egy másik helyen található a HDInsight-fürt hello használata nem támogatott.

Konfigurálása során hello alapértelmezett tárolási végpont meg egy Azure Storage-fiókot vagy egy Data Lake Store blob tárolóhoz. hello alapértelmezett tároló tartalmazza az alkalmazás- és naplókat. Szükség esetén is megadhat további csatolt Azure Storage-fiókok és a Data Lake Store-fiókok, amelyek a fürt hello érhetik el. HDInsight-fürt hello és hello függő tárolási fiókoknak kell lenniük a hello Azure ugyanazon a helyen.

![Tárolási beállításai: HDFS-kompatibilis tárolási végpontok](./media/hdinsight-hadoop-provision-linux-clusters/hdinsight-cluster-creation-storage.png)

[!INCLUDE [secure-transfer-enabled-storage-account](../../includes/hdinsight-secure-transfer.md)]


### <a name="optional-metastores"></a>Nem kötelező a metaadattárakat
Nem kötelező a Hive vagy az Oozie metaadattárakat hozhat létre. Azonban nem minden fürttípus támogatja a metaadattárakat, és Azure SQL Data Warehouse nem kompatibilis a metaadattárakat a. 

> [!IMPORTANT]
> Amikor létrehoz egy egyéni metaadattárhoz, kötőjeleket, kötőjeleket vagy szóközöket hello adatbázisnév ne használjon. Hello fürt létrehozási folyamat toofail okozhatnak.

### <a name="use-hiveoozie-metastore"></a>Hive metaadattárhoz

Ha azt szeretné, tooretain a Hive táblák HDInsight-fürtök törlése után, használja az egyéni metaadattárhoz. Majd csatolhat hello metaadattárhoz tooanother HDInsight-fürthöz.

A HDInsight-fürt különböző verzióin átívelő egy HDInsight metaadattárhoz, létrejön egy HDInsight-fürt verziószáma nem osztható meg. A HDInsight-verziók listáját lásd: [támogatott HDInsight-verziókról](hdinsight-component-versioning.md#supported-hdinsight-versions).

### <a name="oozie-metastore"></a>Oozie metaadattárhoz

tooincrease teljesítmény Oozie, ha egy egyéni metaadattárhoz használja. A metaadattárhoz is biztosít hozzáférést tooOozie feladatadatok a fürt törlése után. 

> [!IMPORTANT]
> Egy egyéni Oozie metaadattárhoz nem használhat újra. egy egyéni Oozie metaadattárhoz toouse, hello HDInsight-fürt létrehozásakor meg kell adnia egy üres Azure SQL-adatbázis.

## <a name="configure-cluster-size"></a>Fürt méretének beállítása

Mindaddig, amíg hello fürt létezik számlázása a csomópont használatát. Számlázási kezdődik, amikor a fürt jön létre, és leállítja a hello fürt törlésekor. Fürtök nem vonja lefoglalt és nem helyezhető tartásba.

a HDInsight-fürtök hello költségét hello csomópontok és a virtuális gépek méretei hello hello csomópontok száma határozza meg. 

Különböző fürttípussal érhető rendelkezik különböző csomóponttípusok, csomópontot, és a csomópont méretek száma:
* Hadoop-fürt típusa alapértelmezett: 
    * Két *átjárócsomópontokat*  
    * Négy *adatcsomópontokat*
* Storm-fürt típusa alapértelmezett: 
    * Két *Nimbus csomópontot*
    * Három *ZooKeeper csomópontok*
    * Négy *felügyelő csomópontok* 

Ha csak próbálja ki a HDInsight, ajánlott adatok egy csomópont. HDInsight árazással kapcsolatos további információkért lásd: [HDInsight árképzési](https://go.microsoft.com/fwLink/?LinkID=282635&clcid=0x409).

> [!NOTE]
> Azure-előfizetések hello fürt méretkorlátot függően változik. Ügyfél [Azure számlázási támogatás segítségét](https://docs.microsoft.com/azure/azure-supportability/how-to-create-azure-support-request) tooincrease hello korlátot.
>

Hello Azure portál tooconfigure hello fürt használatakor hello csomópont mérete hello keresztül elérhető **csomópontok árképzési szintjeinek** panelen. Hello portálon is megtekintheti hello hello másik csomópont méretek költsége. 

![HDInsight Virtuálisgép-csomópont méretek](./media/hdinsight-hadoop-provision-linux-clusters/hdinsight-node-sizes.png)

### <a name="virtual-machine-sizes"></a>Virtuálisgép-méretek 
Fürtök telepítésekor válassza ki a számítási erőforrásokat alapú hello megoldás toodeploy tervezi. hello a következő virtuális gépek HDInsight-fürthöz is használtak:
* A és a D1-4 adatsorozat virtuális gépeken: [General-purpose Linux Virtuálisgép-méretek](https://docs.microsoft.com/azure/virtual-machines/linux/sizes-general)
* VM D11-14 adatsorozat: [memóriaoptimalizált Linux Virtuálisgép-méretek](https://docs.microsoft.com/azure/virtual-machines/linux/sizes-memory)

kimenő értéket toofind használandó toospecify létrehozása a fürt használata során egy Virtuálisgép-méretet eltérő SDK-k hello, vagy az Azure PowerShell használata esetén lásd: [virtuális gép méretének a HDInsight-fürtök toouse](../cloud-services/cloud-services-sizes-specs.md#size-tables). A csatolt cikkből hello hello azonosító használata **mérete** hello táblázatok oszlopát.

> [!IMPORTANT]
> Ha több mint 32 munkavégző csomópontokhoz fürtben, ki kell választania egy átjárócsomóponttal mérete legalább 8 maggal és 14 GB RAM-mal.
>
>

További információkért lásd: [virtuális gépek méretei](../virtual-machines/windows/sizes.md). Különböző méretű hello az árazással kapcsolatos információkért lásd: [HDInsight árképzési](https://azure.microsoft.com/pricing/details/hdinsight).   

## <a name="custom-cluster-setup"></a>Egyéni fürt beállítása
Egyéni fürterőforrás telepítő épít hello gyors beállítások létrehozása, és hozzáadja az alábbi beállítások hello:
- [A HDInsight-alkalmazások](#hdinsight-applications)
- [Fürt mérete](#cluster-size)
- Speciális beállítások
  - [A Parancsfájlműveletek](#customize-clusters-using-script-action)
  - [Virtuális hálózat](#use-virtual-network)

## <a name="install-hdinsight-applications-on-clusters"></a>HDInsight-alkalmazások telepítése fürtökön

A HDInsight-alkalmazások olyan alkalmazások, amelyeket a felhasználók egy Linux-alapú HDInsight-fürtre telepíthetnek. Alkalmazások is használhat, feltéve Microsoft, a harmadik felek vagy, hogy most kialakított, saját magának. További információkért lásd: [külső Hadoop-alkalmazások telepítése Azure hdinsight](hdinsight-apps-install-applications.md).

A legtöbb hello HDInsight-alkalmazások telepítése egy üres élcsomópontot.  Egy üres élcsomópontot egy Linux virtuális gép hello ugyanazon ügyfél-eszközök telepítve, és mint hello átjárócsomópont konfigurálva. Hello fürt eléréséhez, az ügyfél alkalmazások tesztelése és az ügyfélalkalmazások üzemeltető hello élcsomópontot is használhatja. További információkért lásd: [üres peremhálózati csomópontok használata a Hdinsightban](hdinsight-apps-use-edge-node.md).

## <a name="advanced-settings-script-actions"></a>Speciális beállítások: parancsfájl-műveletek

További összetevők, vagy testre szabhatja a fürtkonfiguráció létrehozása során-parancsfájlok használatával. Az ilyen parancsprogramok keresztül kerül meghívásra **parancsfájlművelet**, amely hello Azure-portálon, a HDInsight Windows PowerShell-parancsmagok vagy a HDInsight .NET SDK hello szolgáló konfigurációs beállítás. További információkért lásd: [testreszabása HDInsight-fürtjéhez parancsfájlművelet](hdinsight-hadoop-customize-cluster-linux.md).

Néhány natív Java-összetevők, például Mahout és kaszkádolás, Java archív (JAR) fájlként hello fürtön futtathatja. A JAR-fájlok elosztott tooAzure tárolási és tooHDInsight fürtök Hadoop-feladat elküldése mechanizmusok az elküldött. További információkért lásd: [nyújt Hadoop feladatok programozott módon](hdinsight-submit-hadoop-jobs-programmatically.md).

> [!NOTE]
> Ha problémába ütközik JAR fájlok tooHDInsight fürtök telepítése, vagy hívja a JAR-fájlok a HDInsight-fürtökön, forduljon a [Microsoft Support](https://azure.microsoft.com/support/options/).
>
> Egymásra épülő HDInsight nem támogatja, és nincs for Microsoft Support jogosult. Támogatott összetevők listáját lásd: [What's new in HDInsight által biztosított hello fürt verziók](hdinsight-component-versioning.md).
>
>

Egyes esetekben kívánt konfigurációs fájlokat hello létrehozási folyamata során a következő tooconfigure hello:

* clusterIdentity.xml
* Core-site.xml
* Gateway.XML
* a hbase-env.xml
* a hbase-site.xml
* hdfs-site.xml
* Hive-env.xml
* Hive-site.xml
* mapred-hely
* oozie-site.xml
* oozie-env.xml
* a Storm-site.xml
* tez-site.xml
* webhcat-site.xml
* yarn-site.xml

További információkért lásd: [testreszabása HDInsight-fürtök használata rendszerindítási](hdinsight-hadoop-customize-cluster-bootstrap.md).

## <a name="advanced-settings-extend-clusters-with-a-virtual-network"></a>Speciális beállítások: kiterjesztése a fürtök virtuális hálózattal
Ha a megoldást igényel, amely több HDInsight-fürttípusok, vannak elosztva egy [Azure-beli virtuális hálózat](https://docs.microsoft.com/azure/virtual-network) szükséges hello fürttípusok kapcsolódhatnak. Ez a konfiguráció lehetővé teszi, hogy hello fürtöket, és bármely kód toothem telepít, toodirectly kommunikálnak egymással.

További információ az Azure virtuális hálózat használatával a hdinsight eszközzel, lásd: [kiterjesztése HDInsight az Azure virtuális hálózatokkal](hdinsight-extend-hadoop-virtual-network.md).

Például egy Azure virtuális hálózaton belül, kétféle fürt használatával, [elemezhet érzékelőadatokat a Storm és HBase](hdinsight-storm-sensor-data-analysis.md). További információ a HDInsight használata a virtuális hálózaton, megadott konfigurációs követelményekkel hello virtuális hálózat, beleértve: [kiterjesztése HDInsight képességek az Azure Virtual Network használatával](hdinsight-extend-hadoop-virtual-network.md).

## <a name="troubleshoot-access-control-issues"></a>Hozzáférés-vezérlő elhárítása

Ha problémába ütközik a HDInsight-fürtök létrehozása során, tekintse meg [a hozzáférés-vezérlésre vonatkozó követelményeket](hdinsight-administer-use-portal-linux.md#create-clusters).

## <a name="next-steps"></a>Következő lépések

- [Mik azok a HDInsight, a Hadoop ökoszisztémájának hello és a Hadoop-fürtök?](hdinsight-hadoop-introduction.md)
- [A Hadoop első lépései a HDInsightban](hdinsight-hadoop-linux-tutorial-get-started.md)
- [Működik a Hadoop on HDInsight from Windows-számítógépek](hdinsight-hadoop-windows-tools.md)
