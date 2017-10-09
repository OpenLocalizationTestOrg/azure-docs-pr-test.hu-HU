---
title: "aaaCreate Hadoop-fürtök webböngésző - Azure HDInsight használata |} Microsoft Docs"
description: "Ismerje meg, hogyan toocreate Hadoop, HBase, Storm vagy Spark-fürtök Linux rendszeren a webböngésző segítségével HDInsight és hello Azure betekintő portálon."
services: hdinsight
documentationcenter: 
author: nitinme
manager: jhubbard
editor: cgronlun
tags: azure-portal
ms.assetid: 697278cf-0032-4f7c-b9b2-a84c4347659e
ms.service: hdinsight
ms.custom: hdinsightactive
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 05/10/2017
ms.author: nitinme
ms.openlocfilehash: 63027e35e2d66dd76218aff3e0c085fc811736ec
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="create-linux-based-clusters-in-hdinsight-using-hello-azure-portal"></a>Linux-alapú fürtök létrehozása a Hdinsightban hello Azure-portál használatával
[!INCLUDE [selector](../../includes/hdinsight-create-linux-cluster-selector.md)]

hello Azure-portálon a szolgáltatások és erőforrások hello Microsoft Azure felhőben üzemeltetett webalapú felügyeleti eszköz. Ebben a cikkben megtudhatja, hogyan toocreate Linux-alapú HDInsight clusters hello portál használatával.

## <a name="prerequisites"></a>Előfeltételek
[!INCLUDE [delete-cluster-warning](../../includes/hdinsight-delete-cluster-warning.md)]

* **Azure-előfizetés**. Lásd: [Ingyenes Azure-fiók létrehozása](https://azure.microsoft.com/documentation/videos/get-azure-free-trial-for-testing-hadoop-in-hdinsight/).
* **Modern webböngésző**. hello Azure-portálon HTML5-ös és a Javascript használ, és esetleg nem működnek megfelelően korábbi verziójú böngészőt.

## <a name="create-clusters"></a>Fürtök létrehozása
hello Azure-portálon a legtöbb hello tulajdonságait mutatja. Azure Resource Manager-sablonnal, elrejtheti a nagy mennyiségű részleteit. További információkért lásd: [létrehozása Linux-alapú Hadoop-fürtök használata Azure Resource Manager-sablonok hdinsight](hdinsight-hadoop-create-linux-clusters-arm-templates.md).

[!INCLUDE [secure-transfer-enabled-storage-account](../../includes/hdinsight-secure-transfer.md)]


1. Jelentkezzen be toohello [Azure-portálon](https://portal.azure.com).
2. Kattintson a  **+** , kattintson a **Eszközintelligencia + analitika**, és kattintson a **HDInsight**.
   
    ![Új fürt létrehozása az Azure-portálon hello](./media/hdinsight-hadoop-create-linux-cluster-portal/hdinsight-create-cluster.png "hello Azure-portálon az új fürt létrehozása")

3. A hello **HDInsight** panelen kattintson **egyéni (mérete, beállítások, alkalmazásokat)**, kattintson a **alapjai**, majd adja meg a következő információ hello.

    ![Új fürt létrehozása az Azure-portálon hello](./media/hdinsight-hadoop-create-linux-cluster-portal/hdinsight-create-cluster-basics.png "hello Azure-portálon az új fürt létrehozása")

    * A **Fürt neve** mezőben adja meg a fürt nevét, amelynek globálisan egyedinek kell lennie.

    * A hello **előfizetés** legördülő listából válassza hello Azure-előfizetéssel, amely jelzi a hello fürt.

    * Kattintson a **típusú fürt**, majd válassza ki:
   
        * **Fürt típusa**: Ha nem tudja, milyen toochoose, válassza ki a **Hadoop**. Hello legnépszerűbb fürt típusa.
     
            > [!IMPORTANT]
            > HDInsight fürtöket a különböző típusú, amely toohello munkaterhelés származnak, vagy technológia, amely a fürt hello hangolt. Nincs nem támogatott metódus toocreate egy fürt, amely többféle, például a Storm és HBase egy fürtön egyesíti. 
            > 
            > 
        
        * **Operációs rendszer**: válassza a **Linux** lehetőséget.
        
        * **Verzió**: hello alapértelmezett verzióját használja, ha nem tudja, milyen toochoose. További tájékoztatás a [HDInsight cluster versions](hdinsight-component-versioning.md) (A HDInsight-fürtök verziói) című cikkben olvasható.
        * **A fürt réteg**: Azure HDInsight big data felhőajánlatokat hello két kategóriába biztosít: Standard csomagban és a prémium csomagban. További tájékoztatás a [Cluster tiers](hdinsight-hadoop-provision-linux-clusters.md#cluster-tiers) (A fürtök szintjei) című cikkben olvasható.

    * A **fürt bejelentkezési felhasználónevének** és **fürt bejelentkezési jelszó**, hello felhasználónév és jelszó hello rendszergazda felhasználó számára.

    * Adjon meg egy **SSH felhasználónév** , és ha toohave hello SSH-jelszó ugyanaz, mint a korábban megadott rendszergazdai jelszó hello, jelölje be a hello **ugyanazt a jelszót használják a fürt bejelentkezési** jelölőnégyzetet. Ha nem, adja meg, vagy egy **jelszó** vagy **nyilvános kulcs**, amely használt tooauthenticate hello SSH felhasználó lesz. A nyilvános kulcs használata ajánlott megközelítést alkalmazva hello. Kattintson a **válasszon** hello alsó toosave hello hitelesítői adatainak konfigurációja a következő.
   
        További információk: [Az SSH használata HDInsighttal](hdinsight-hadoop-linux-use-ssh-unix.md).

    * A **erőforráscsoport**, adja meg, hogy szeretné, hogy toocreate egy új erőforráscsoportot, vagy használjon egy meglévőt.

    * Adjon meg egy adott adatközpont **hely** hello fürt létrehozásának helyét.

    * Kattintson a **Tovább** gombra.

4. A hello **tárolási** panelen adja meg, hogy Azure Storage (WASB) vagy a Data Lake Store az alapértelmezett tárolóként. Tekintse meg az alábbi táblázatban hello további információt.

    ![Új fürt létrehozása az Azure-portálon hello](./media/hdinsight-hadoop-create-linux-cluster-portal/hdinsight-create-cluster-storage.png "hello Azure-portálon az új fürt létrehozása")

    | Storage                                      | Leírás |
    |----------------------------------------------|-------------|
    | **Az Azure Storage Blobs alapértelmezett tárolóként**   | <ul><li>A **elsődleges tárolótípus**, jelölje be **Azure Storage**. Ezt követően a **kijelöléséről**, dönthet úgy, **a saját előfizetések** toospecify egy tárfiókot, amely része az Azure-előfizetéshez és hello jelölje ki a tárfiók gombra. Ellenkező esetben kattintson a **hozzáférési kulcs** és adja meg, amelyet meg szeretne-e a toochoose kívül az Azure-előfizetéshez hello tárfiók hello információkat.</li><li>A **alapértelmezett tároló**, válassza ki a toogo hello hello portál által javasolt alapértelmezett tároló néven, vagy adja meg a saját.</li><li>Ha WASB alapértelmezett tárolóként használ, (opcionális) kattintson **további Tárfiókok** toospecify további tárfiókok tooassociate hello fürttel. A hello **Azure Storage kulcsok** panelen kattintson a **tárolási kulcs hozzáadása**, és megadhatja a storage-fiókok Azure-előfizetését, illetve más előfizetések (megadásával hello storage-fiók hozzáférési kulcs).</li><li>Ha WASB alapértelmezett tárolóként használ, (opcionális) kattintson **Data Lake Store hozzáférés** toospecify Azure Data Lake Store további tárolóként. További információkért lásd: [HDInsight-fürtök létrehozása az Azure portál használatával a Data Lake Store](../data-lake-store/data-lake-store-hdinsight-hadoop-use-portal.md).</li></ul> |
    | **Azure Data Lake Store alapértelmezett tárolóként** | A **elsődleges tárolótípus**, jelölje be **Data Lake Store** majd tekintse át a toohello cikk [HDInsight-fürtök létrehozása az Azure portál használatával a Data Lake Store](../data-lake-store/data-lake-store-hdinsight-hadoop-use-portal.md) a utasításokat. |
    | **Külső metaadattárakat**                      | Másik lehetőségként megadhat egy SQL adatbázis toosave struktúra és az Oozie-metaadatok hello-fürthöz tartozó. A **jelöljön ki egy SQL-adatbázist a Hive** jelöljön ki egy SQL-adatbázist, és adja meg hello felhasználónév/jelszó hello adatbázis. Ismételje ezeket a lépéseket az Oozie-metaadatok.<br><br>Azt is számításba kell metastores az Azure SQL-adatbázis használata során. <ul><li>hello Azure SQL adatbázis hello metaadattárhoz használt engedélyezniük kell kapcsolatot tooother Azure-szolgáltatásokkal, beleértve az Azure HDInsight. Hello Azure SQL adatbázis irányítópulton, hello jobb oldalán hello kiszolgáló nevére kattint. Ez az hello mely hello SQL database-példányt futtató kiszolgáló. Miután hello server nézetben, kattintson a **konfigurálása**, és majd a **Azure Services**, kattintson a **Igen**, és kattintson a **mentése**.</li><li>A metaadattárhoz létrehozásakor ne használja az adatbázis nevét, amely tartalmazza a kötőjelek és kötőjeleket tartalmazhat, ennek hatására hello fürt létrehozási folyamat toofail.</li></ul>                                                                                                                                                                       |

    Kattintson a **Tovább** gombra. 

    > [!WARNING]
    > Egy további storage-fiók egy másik helyen található mint hello HDInsight-fürt használata nem támogatott.

5. Ha úgy gondolja, a **alkalmazások** tooinstall a HDInsight-fürtök együtt használható alkalmazások. Ezek az alkalmazások lehetnek a Microsoft, független szoftvergyártók (ISV-k) vagy a felhasználók fejlesztései. További információkért lásd: [telepítése HDInsight-alkalmazások](hdinsight-apps-install-applications.md#install-applications-during-cluster-creation).


6. Kattintson a **a fürt méretét** ehhez a fürthöz létrehozott csomópontok hello toodisplay információt. Hello hello fürt igénylő feldolgozó csomópontok számának beállítása. hello becsült hello fürt költsége hello panel belül megjelenik.
   
    ![A csomópont árképzési Rétegek panel](./media/hdinsight-hadoop-create-linux-cluster-portal/hdinsight-create-cluster-nodes.png "adja meg a fürtcsomópontok számára")
   
   > [!IMPORTANT]
   > Ha azt tervezi, több mint 32 munkavégző csomóponton, vagy a fürt létrehozásakor vagy skálázással hello fürt létrehozása után, akkor ki kell választania egy átjárócsomóponttal mérete legalább 8 maggal és 14GB ram.
   > 
   > A csomópont-méretek és a társuló költségeket további információkért lásd: [HDInsight árképzési](https://azure.microsoft.com/pricing/details/hdinsight/).
   > 
   > 
   
   Kattintson a **következő** toosave hello csomópont árképzési konfigurációs.

7. Kattintson a **speciális beállítások** tooconfigure egyéb választható beállításai, például használatával **Parancsfájlműveletek** toocustomize egy fürt tooinstall egyéni összetevők vagy csatlakoztatása egy **virtuális hálózati**. Tekintse meg az alábbi táblázatban hello további információt.

    ![A csomópont árképzési Rétegek panel](./media/hdinsight-hadoop-create-linux-cluster-portal/hdinsight-create-cluster-advanced.png "adja meg a fürtcsomópontok számára")

    | Beállítás | Leírás |
    |--------|-------------|
    | **A Parancsfájlműveletek** | Használja ezt a beállítást Ha azt szeretné, toouse egy egyéni parancsfájl toocustomize egy fürt hello fürt létrehozását. A Parancsfájlműveletek kapcsolatos további információkért lásd: [testreszabása HDInsight-fürtök használata parancsfájlművelet](hdinsight-hadoop-customize-cluster-linux.md). |
    | **Virtuális hálózat** | Egy Azure virtuális hálózat és hello alhálózati akkor válassza, ha azt szeretné, hogy tooplace hello fürt virtuális hálózatba. Információ a HDInsight használata a virtuális hálózaton, többek között a virtuális hálózati hello megadott konfigurációs követelményekkel kapcsolatban: [kiterjesztése HDInsight képességek az Azure virtuális hálózat használatával](hdinsight-extend-hadoop-virtual-network.md). |

    Kattintson a **Tovább** gombra.

8. A hello **összegzés** panelen ellenőrizze a korábban megadott hello adatokat, és kattintson a **létrehozása**.

    ![A csomópont árképzési Rétegek panel](./media/hdinsight-hadoop-create-linux-cluster-portal/hdinsight-create-cluster-summary.png "adja meg a fürtcsomópontok számára")
    
    > [!NOTE]
    > Némi időbe, hello fürt toobe létrehozása általában körülbelül 15 percig tart. Hello Kezdőpulton hello csempe használja, vagy hello **értesítések** hello bevitelének maradt a hello lap toocheck hello létesítésének folyamatát kell használnia.
    > 
    > 
12. Miután hello létrehozási folyamat befejeződött, kattintson a hello csempe hello fürt hello Kezdőpulton toolaunch hello fürt paneljén. hello fürt paneljén hello a következő információkat biztosít.
    
    ![Fürt paneljén](./media/hdinsight-hadoop-create-linux-cluster-portal/hdinsight-create-cluster-completed.png "fürt tulajdonságai")
    
    A következő toounderstand hello ikonok hello felső panel hello használata.
    
    * **Áttekintés** lapon minden hello alapvető információkat nyújt azokról hello fürt például hello nevét, a hello erőforráscsoport tartozik, hello helyét, hello operációs rendszer, URL-címe hello fürt irányítópult stb.
    * **Irányítópult** toohello Ambari portal hello-fürthöz tartozó irányítja.
    * **Biztonságos rendszerhéj**: SSH használatával tooaccess hello fürt szükséges információk.
    * **Skála fürt** megadható, hogy növeli hello társított hello fürt feldolgozó csomópontjainak számát.
    * **Törlés**: hello HDInsight-fürt törlése.
    

## <a name="customize-clusters"></a>Fürtök személyre szabása
* Lásd: [testreszabása HDInsight-fürtök használata rendszerindítási](hdinsight-hadoop-customize-cluster-bootstrap.md).
* Lásd: [testreszabása HDInsight-fürtök használata parancsfájlművelet](hdinsight-hadoop-customize-cluster-linux.md).

## <a name="delete-hello-cluster"></a>Hello fürt törlése
[!INCLUDE [delete-cluster-warning](../../includes/hdinsight-delete-cluster-warning.md)]

## <a name="troubleshoot"></a>Hibaelhárítás

Ha problémába ütközik a HDInsight-fürtök létrehozása során, tekintse meg [a hozzáférés-vezérlésre vonatkozó követelményeket](hdinsight-administer-use-portal-linux.md#create-clusters).

## <a name="next-steps"></a>Következő lépések
Most, hogy sikeresen létrehozott egy HDInsight-fürtre, használja a következő toolearn hogyan hello toowork a fürthöz:

### <a name="hadoop-clusters"></a>Hadoop-fürtök
* [A Hive használata a HDInsightban](hdinsight-use-hive.md)
* [A Pig használata a HDInsightban](hdinsight-use-pig.md)
* [Use MapReduce with HDInsight](hdinsight-use-mapreduce.md)

### <a name="hbase-clusters"></a>HBase-fürtökkel
* [Az a HDInsight HBase első lépései](hdinsight-hbase-tutorial-get-started-linux.md)
* [A HDInsight HBase Java-alkalmazások fejlesztése](hdinsight-hbase-build-java-maven-linux.md)

### <a name="storm-clusters"></a>Storm-fürtök
* [Java-topológiák fejlesztése hdinsight alatt futó Storm](hdinsight-storm-develop-java-topology.md)
* [A HDInsight alatt futó Storm Python-összetevőket használnak](hdinsight-storm-develop-python-topology.md)
* [Telepítheti és figyelheti a HDInsight alatt futó Storm topológiák](hdinsight-storm-deploy-monitor-topology-linux.md)

### <a name="spark-clusters"></a>A Spark-fürtök
* [Önálló alkalmazás létrehozása a Scala használatával](hdinsight-apache-spark-create-standalone-application.md)
* [Feladatok távoli futtatása Spark-fürtön a Livy használatával](hdinsight-apache-spark-livy-rest-interface.md)
* [Spark és BI: Interaktív adatelemzés végrehajtása a Spark on HDInsight használatával, BI-eszközökkel](hdinsight-apache-spark-use-bi-tools.md)
* [Spark és Machine Learning: használja a Spark on HDInsight toopredict élelmiszervizsgálati eredmények](hdinsight-apache-spark-machine-learning-mllib-ipython.md)
* [Spark Streaming: A Spark on HDInsight használata valós idejű streamelési alkalmazások összeállítására](hdinsight-apache-spark-eventhub-streaming.md)

