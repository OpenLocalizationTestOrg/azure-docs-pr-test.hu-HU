---
title: "Azure-portál használatával hdinsight Hadoop-fürtök kezelése |} Microsoft Docs"
description: "Megtudhatja, hogyan hozhatja létre és kezelheti a HDInsight-fürtök az Azure portál használatával."
services: hdinsight
documentationcenter: 
author: mumian
manager: jhubbard
editor: cgronlun
tags: azure-portal
ms.assetid: 5a76f897-02e8-4437-8f2b-4fb12225854a
ms.service: hdinsight
ms.custom: hdinsightactive
ms.workload: big-data
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/15/2017
ms.author: jgao
ms.openlocfilehash: c9cb631aef71f72457c3517d02566a56919f82bc
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/29/2017
---
# <a name="manage-hadoop-clusters-in-hdinsight-by-using-the-azure-portal"></a>Hdinsight Hadoop-fürtök kezelése az Azure-portál használatával
[!INCLUDE [selector](../../includes/hdinsight-portal-management-selector.md)]

Használja a [Azure-portálon][azure-portal], kezelheti az Azure HDInsight Hadoop-fürtök. A lap választó használjon vonatkozó tudnivalókat más eszközök használatával hdinsight Hadoop-fürtök kezelése.

**Előfeltételek**

Ez a cikk megkezdése előtt rendelkeznie kell a következő elemek:

* **Azure-előfizetés**. Lásd: [Ingyenes Azure-fiók létrehozása](https://azure.microsoft.com/documentation/videos/get-azure-free-trial-for-testing-hadoop-in-hdinsight/).

## <a name="open-the-portal"></a>A portál megnyitása
1. Jelentkezzen be [https://portal.azure.com](https://portal.azure.com).
2. A portál megnyitása után végezhetők el:

   * Kattintson a **új** új fürt létrehozása a bal oldali menüből:

       ![a HDInsight-fürt új gomb](./media/hdinsight-administer-use-portal-linux/azure-portal-new-button.png)
   * Kattintson a **a HDInsight-fürtök** a meglévő fürtök elemet a bal oldali menüből

       ![Az Azure portál HDInsight fürt gomb](./media/hdinsight-administer-use-portal-linux/azure-portal-hdinsight-button.png)

       Ha nem látja a HDInsight-fürt, kattintson a **további szolgáltatások** a listában, és kattintson az alsó **a HDInsight-fürtök** alatt a **Eszközintelligencia + analitika** szakasz.


## <a name="create-clusters"></a>Fürtök létrehozása
[!INCLUDE [delete-cluster-warning](../../includes/hdinsight-delete-cluster-warning.md)]

HDInsight Hadoop széles tartomány-összetevők működik. A ellenőrizni és a támogatott összetevők listáját lásd: [Azure HDInsight Hadoop verziójának van](hdinsight-component-versioning.md). Általános fürt létrehozása információkért lásd: [Hadoop létrehozása a HDInsight-fürtök](hdinsight-hadoop-provision-linux-clusters.md).

### <a name="access-control-requirements"></a>A hozzáférés-vezérlésre vonatkozó követelmények

HDInsight-fürtök létrehozásakor meg kell adnia egy Azure-előfizetés. Ezen a fürtön vagy egy új Azure-erőforráscsoportot, vagy egy meglévő erőforráscsoportot is létrehozható. Az alábbi lépések segítségével ellenőrizze az engedélyeit a HDInsight-fürtök létrehozásához:

- Meglévő erőforráscsoport használata.

    1. Jelentkezzen be az [Azure Portalra](https://portal.azure.com).
    2. Kattintson a **erőforráscsoportok** az erőforráscsoportok elemet a bal oldali menüből.
    3. Kattintson a HDInsight-fürt létrehozásához használni kívánt erőforráscsoportot.
    4. Kattintson a **hozzáférés-vezérlés (IAM)**, és ellenőrizze, hogy Ön (vagy egy csoportot, amely az Ön is a tagja) van legalább az erőforráscsoport közreműködői eléréséhez.

- Új erőforráscsoport létrehozása

    1. Jelentkezzen be az [Azure Portalra](https://portal.azure.com).
    2. Kattintson a **előfizetés** a bal oldali menüből. Rendelkezik a sárga kulcs ikon. Az előfizetések listája megjelenik.
    3. Kattintson az előfizetést, a fürtök létrehozására használhatja. 
    4. Kattintson a **saját engedélyek**.  Azt illusztrálja a [szerepkör](../active-directory/role-based-access-control-what-is.md#built-in-roles) az előfizetésben. Legalább kell közreműködői access HDInsight-fürt létrehozásához.

Ha a NoRegisteredProviderFound hiba vagy a MissingSubscriptionRegistration hiba, lásd: [hibaelhárítás általános az Azure-telepítés az Azure Resource Manager](../azure-resource-manager/resource-manager-common-deployment-errors.md).

## <a name="list-and-show-clusters"></a>Listában, és a fürt megjelenítése
1. Jelentkezzen be [https://portal.azure.com](https://portal.azure.com).
2. Kattintson a **a HDInsight-fürtök** a meglévő fürtök elemet a bal oldali menüből.
3. Kattintson a fürt nevére. Ha a fürt lista hosszú, az oldal tetején a szűrő is használhatja.
4. Kattintson a listában, a – áttekintés oldalra fürt:

    ![Az Azure portál HDInsight fürt alapjai](./media/hdinsight-administer-use-portal-linux/hdinsight-essentials.png)

    * **Irányítópult**: megnyitja a fürt irányítópultot, amely Ambari Web Linux-alapú fürtökhöz.
    * **Biztonságos rendszerhéj**: jeleníti meg az utasításokat a fürtjét Secure Shell (SSH) kapcsolaton keresztül csatlakozni.
    * **Fürt méretezése**: lehetővé teszi a fürt feldolgozó csomópontok számának megváltoztatására.
    * **Törlés**: törli a fürtöt.
    * **Tevékenységi naplóit**: megjelenítése és a lekérdezés tevékenységi naplóit.
    * **Hozzáférés-vezérlés (IAM)**: szerepkör-hozzárendelésekkel.  Lásd: [az Azure-előfizetés erőforrásokhoz való hozzáférés kezelése a szerepkör-hozzárendelések segítségével](../active-directory/role-based-access-control-configure.md).
    * **Címkék**: lehetővé teszi a felhőalapú szolgáltatások egyéni besorolás meghatározásához kulcs/érték párok beállítását. Például létrehozhat nevű kulcs **projekt**, majd használja az adott projekthez tartozó összes szolgáltatás közös értéket.
    * **Hibáinak diagnosztizálásához és elhárításához**: hibaelhárítási információk megjelenítése.
    * **Zárolja**: hozzáadása a fürt folyamatban érdekében zárolási módosították vagy törölték.
    * **Automatizálási parancsfájl**: megjelenítési és a fürt az Azure Resource Manager sablon exportálása. Jelenleg csak exportálhatja a függő Azure storage-fiók. Lásd: [létrehozása Linux-alapú Hadoop-fürtök használata Azure Resource Manager-sablonok hdinsight](hdinsight-hadoop-create-linux-clusters-arm-templates.md).
    * **Gyors üzembe helyezési**: információit jeleníti meg, amely segít a HDInsight használatának megkezdésében.
    * **A HDInsight eszközök**: segítő információk a HDInsight kapcsolódó eszközök.
    * **A fürt bejelentkezési**: a fürt bejelentkezési adatok megjelenítéséhez.
    * **Előfizetés alapvető használati**: az előfizetéshez tartozó felhasznált és rendelkezésre álló magot megjelenítéséhez.
    * **Fürt méretezése**: növelése és a fürt feldolgozó csomópontok száma csökken. Lásd:[fürtök méretezése](hdinsight-administer-use-management-portal.md#scale-clusters).
    * **Biztonságos rendszerhéj**: jeleníti meg az utasításokat a fürtjét Secure Shell (SSH) kapcsolaton keresztül csatlakozni. További információ: [Az SSH használata HDInsighttal](hdinsight-hadoop-linux-use-ssh-unix.md).
    * **HDInsight-partnert**: a jelenlegi HDInsight-Partner hozzáadása/eltávolítása.
    * **Külső Metaadattárakat**: a Hive és az Oozie metastores megtekintése. A metaadattárakat csak konfigurálható úgy, hogy a fürt létrehozása során. Lásd: [használni a Hive/Oozie metaadattárhoz](hdinsight-hadoop-provision-linux-clusters.md#use-hiveoozie-metastore).
    * **Parancsfájl-műveletek**: futtassa a Bash parancsfájlok a fürtön. Lásd: [testreszabása Linux-alapú HDInsight-fürtök használata parancsfájlművelet](hdinsight-hadoop-customize-cluster-linux.md).
    * **Alkalmazások**: hozzáadása a HDInsight-alkalmazások.  Lásd: [egyéni HDInsight-alkalmazások telepítése](hdinsight-apps-install-custom-applications.md).
    * **Tulajdonságok**: a fürt tulajdonságainak megtekintése.
    * **Storage-fiókok**: a storage-fiókok és a kulcsok megtekintéséhez. A storage-fiókok vannak konfigurálva, a fürt létrehozása során.
    * **A fürt AAD-identitása**:
    * **Új támogatási kérelem**: lehetővé teszi egy támogatási jegy létrehozása a Microsoft támogatási szolgálatához.
    
6. Kattintson a **tulajdonságok**:

    A tulajdonságok a következők:

   * **Állomásnév**: fürt nevét.
   * **A fürt URL-cím**. Az Ambari webes felület URL-CÍMÉT.
   * **Állapot**: tartalmaznak megszakadt, fogadja el, ClusterStorageProvisioned, AzureVMConfiguration, HDInsightConfiguration, működési, fut, a hiba, törlése, törlése, időtúllépésbe került, DeleteQueued, DeleteTimedout, DeleteError, PatchQueued, CertRolloverQueued, ResizeQueued, ClusterCustomization
   * **A régióban**: Azure-beli hely. Támogatott Azure helyek listáját lásd: a **régió** lévő legördülő lista [HDInsight árképzési](https://azure.microsoft.com/pricing/details/hdinsight/).
   * **Létrehozás dátuma**.
   * **Operációs rendszer**: vagy **Windows** vagy **Linux**.
   * **Típus**: Hadoop, HBase, Storm, Spark.
   * **Verzió**. Lásd: [HDInsight-verziókról](hdinsight-component-versioning.md)
   * **Előfizetés**: előfizetés nevét.
   * **Alapértelmezett adatforrás**: az alapértelmezett fürt fájlrendszert.
   * **Munkavégző csomópontok mérete**.
   * **Csomópont méretének HEAD**.

## <a name="delete-clusters"></a>Fürtök törlése
A fürt törlése nem érinti az alapértelmezett tárfiók vagy az összes kapcsolt tárfiókot. Újra létrehozhatja a fürtöt azonos tárfiókok és ugyanazon a metaadattárakat használatával. Javasoljuk, hogy egy új alapértelmezett Blob tárolókat használja a fürt ismételt létrehozásakor.

1. Jelentkezzen be a [Portal][azure-portal].
2. Kattintson a **a HDInsight-fürtök** a bal oldali menüből. Ha nem lát **a HDInsight-fürtök**, kattintson a **további szolgáltatások** első.
3. Kattintson a törölni kívánt fürtöt.
4. Kattintson a **törlése** a felső menüben, majd kövesse az utasításokat.

Lásd még: [fürtök szünet/Leállítás](#pauseshut-down-clusters).

## <a name="add-additional-storage-accounts"></a>További tárfiókok hozzáadása

A fürt létrehozása után hozzáadhat további Azure Storage-fiókok és az Azure Data Lake Store-fiók. További információkért lásd: [Add additional storage accounts to HDInsight](./hdinsight-hadoop-add-storage.md) (További tárfiókok hozzáadása a HDInsighthoz).

## <a name="scale-clusters"></a>Fürtök méretezése
A fürt skálázás funkciót lehetővé teszi, hogy anélkül, hogy újra létre kell hoznia a fürt fut az Azure HDInsight fürt által használt feldolgozó csomópontok számának módosítása.

> [!NOTE]
> Csak verzió 3.1.3 hdinsight clusters vagy annál magasabb támogatottak. Ha biztos benne, hogy a fürt verzióját, a Tulajdonságok lapján ellenőrizheti.  Lásd: [listája és megjelenítése fürtök](#list-and-show-clusters).
>
>

A fürt a HDInsight által támogatott különböző típusú adatok csomópontok számának módosítása következményei:

* Hadoop

    Zökkenőmentesen növelheti adhatja meg, hogy minden folyamatban lévő vagy a futó feladatok befolyásolása nélkül fut egy Hadoop-fürt feldolgozó csomópontjainak számát. Új feladatokat is küldheti el, amíg a művelet folyamatban van. A méretezési művelet sikertelen szabályosan kezeli, hogy a fürt mindig működőképes állapotban marad.

    A Hadoop fürtök adatok csomópontok számának csökkentésével átméretezi, ha néhány, a fürt a szolgáltatások újraindításáig. Ez a viselkedés hatására az összes futó és függőben lévő feladatok meghiúsulhatnak, a méretezési művelet befejezését. Akkor is, azonban küldje el újra a feladatok a művelet végrehajtása után.
* HBase

    Akkor is zökkenőmentesen csomópontok hozzáadásához és eltávolításához a HBase-fürtöt a futtatása. Területi kiszolgálók automatikus elosztását a méretezési művelet befejezését néhány percen belül. Azonban Ön is manuálisan is elosztása a regionális kiszolgálók fürt headnode való bejelentkezés, és futtatja a következő parancsokat egy parancssori ablakot:

        >pushd %HBASE_HOME%\bin
        >hbase shell
        >balancer

    A HBase rendszerhéjjal további információkért lásd:]
* Storm

    Akkor is zökkenőmentesen csomópontok hozzáadásához és eltávolításához adatok Storm fürthöz való futtatása során. De a méretezési művelet sikeres befejezését követően szüksége lesz a topológia egyensúlyba.

    Kétféle módon valósítható meg újraelosztás:

  * A Storm webes felhasználói felület
  * Parancssori felület (CLI) eszköz

    Tekintse meg a [alatt futó Apache Storm-dokumentáció](http://storm.apache.org/documentation/Understanding-the-parallelism-of-a-Storm-topology.html) további részleteket.

    A Storm webes felhasználói felület érhető el a HDInsight-fürt:

    ![A HDInsight alatt futó Storm méretezési egyensúlyozza ki újra](./media/hdinsight-administer-use-portal-linux/hdinsight-portal-scale-cluster-storm-rebalance.png)

    Íme egy példa a CLI parancs használata a Storm-topológia egyensúlyba:

        ## Reconfigure the topology "mytopology" to use 5 worker processes,
        ## the spout "blue-spout" to use 3 executors, and
        ## the bolt "yellow-bolt" to use 10 executors
        $ storm rebalance mytopology -n 5 -e blue-spout=3 -e yellow-bolt=10

**Fürtök méretezése**

1. Jelentkezzen be a [Portal][azure-portal].
2. Kattintson a **a HDInsight-fürtök** a bal oldali menüből.
3. Kattintson arra a fürtre méretezésére.
3. Kattintson a **fürt méretezése**.
4. Adja meg **számát a feldolgozó csomópontok**. A fürtcsomópontok számára vonatkozó korlát az Azure-előfizetések függően változik. A korlát növeléséhez Számlázási támogatást kérhetnek.  A Költséginformációk tükrözi a módosítások a csomópontok számát.

    ![HDInsight hadoop hbase storm spark méretezési](./media/hdinsight-administer-use-portal-linux/hdinsight-portal-scale-cluster.png)

## <a name="pauseshut-down-clusters"></a>Fürtök szünet/leállítása

Hadoop-feladatokat többsége kötegelt feladatok, amelyek csak esetenként futnak. A legtöbb Hadoop-fürtök vonatkoznak-e nagy időszakokra, amely a fürt nem használják a feldolgozáshoz. A HDInsight az Azure Storage szolgáltatásban tárolja az adatokat, így biztonságosan törölhet olyan fürtöket, amelyek nincsenek használatban.
Ráadásul a HDInsight-fürtök akkor is díjkötelesek, amikor éppen nincsenek használatban. Mivel a fürt költsége a sokszorosa a tároló költségeinek, gazdaságossági szempontból is ésszerű törölni a használaton kívüli fürtöket.

Meg a program a folyamat számos módja van:

* Az Azure Data Factory felhasználó. Lásd: [létrehozása igény szerinti Linux-alapú Hadoop-fürtök Azure Data Factory használatával hdinsight](hdinsight-hadoop-create-linux-clusters-adf.md) igény szerinti HDInsight létrehozásához kapcsolódó szolgáltatások.
* Azure PowerShell használatával.  Lásd: [repülési késleltetés adatok elemzése](hdinsight-analyze-flight-delay-data.md).
* Az Azure CLI használata. Lásd: [kezelése HDInsight-fürtök Azure parancssori felület használatával](hdinsight-administer-use-command-line.md).
* A HDInsight .NET SDK használata. Lásd: [nyújt Hadoop-feladatokat](hdinsight-submit-hadoop-jobs-programmatically.md).

Díjszabási információkért lásd: [HDInsight árképzési](https://azure.microsoft.com/pricing/details/hdinsight/). Törölni a fürtöt a portálról, lásd: [fürtök törlése](#delete-clusters)


## <a name="upgrade-clusters"></a>Fürtök frissítése

Lásd: [újabb verzióra való frissítés HDInsight fürt](./hdinsight-upgrade-cluster.md).

## <a name="change-passwords"></a>Jelszavak módosítása
HDInsight-fürtök lehet két felhasználói fiókot. A HDInsight-fürtöt felhasználói fiók (más néven HTTP-felhasználói fiók) és az SSH-felhasználói fiókot a létrehozási folyamat során jönnek létre. Az Ambari webes felhasználói felület segítségével módosíthatja a fürt felhasználói fiók felhasználónevét és a jelszó és a Parancsfájlműveletek az SSH-felhasználói fiók módosítása

### <a name="change-the-cluster-user-password"></a>A fürt felhasználói jelszó módosítása
Az Ambari webes felhasználói felület segítségével módosíthatja a fürt felhasználói jelszavát. Ambari bejelentkezni, a meglévő fürt felhasználónevet és jelszót kell használnia.

> [!NOTE]
> A fürt (rendszergazda) felhasználói jelszó módosítása előfordulhat, hogy a parancsfájl műveletek lefutott sikertelen lesz a fürthöz. Ha bármely a megőrzött Parancsfájlműveletek adott cél munkavégző csomópontokhoz, ezek a parancsfájlok nem tud hozzá csomópontokat a fürthöz keresztül átméretezése műveletek. A Parancsfájlműveletek további információkért lásd: [testreszabása HDInsight-fürtök Parancsfájlműveletek segítségével](hdinsight-hadoop-customize-cluster-linux.md).
>
>

1. Jelentkezzen be az Ambari webes felhasználói felületén a HDInsight fürt felhasználói hitelesítő adatok használatával. Az alapértelmezett felhasználónév az **admin**. Az URL-címe **https://&lt;HDInsight-fürt neve > azurehdinsight.net**.
2. Kattintson a **Admin** a felső menüben, majd kattintson a "Kezelése Ambari".
3. Kattintson a bal oldali menü **felhasználók**.
4. Kattintson a **Admin**.
5. Kattintson a **jelszómódosítás**.

Ambari majd módosítja a jelszót, a fürt összes csomópontján.

### <a name="change-the-ssh-user-password"></a>Az SSH-felhasználói jelszó módosítása
1. Egy szövegszerkesztővel, mentse a következő szöveg nevű **changepassword.sh**.

   > [!IMPORTANT]
   > A sor befejezési LF használó szerkesztővé kell használnia. Ha a szerkesztő CRLF használ, majd a parancsfájl nem működik.
   >
   >

        #! /bin/bash
        USER=$1
        PASS=$2

        usermod --password $(echo $PASS | openssl passwd -1 -stdin) $USER
2. Feltölteni a fájlt, amely elérhető a HDInsight egy HTTP vagy HTTPS-címet a tárolóhelyre. Például egy nyilvános fájl tárolja, például a OneDrive- vagy Azure Blob storage. Ezt az URI a következő lépéshez szükséges mentse a fájlt, az URI (HTTP vagy HTTPS-cím).
3. Az Azure-portálon kattintson **a HDInsight-fürtök**.
4. Kattintson a HDInsight-fürthöz.
4. Kattintson a **parancsfájl-műveletek**.
4. Az a **Parancsfájlműveletek** panelen válassza **nyújt új**. Ha a **parancsfájlművelet** panel jelenik meg, írja be a következő adatokat:

   | Mező | Érték |
   | --- | --- |
   | Név |Ssh jelszó módosítása |
   | Bash parancsfájlok URI |Az URI-t a changepassword.sh fájl |
   | Csomópontok (Head munkavégző, Nimbus, felügyelő, Zookeeper, stb.) |Az összes csomópont felsorolt ✓ |
   | Paraméterek |Adja meg az SSH-felhasználónév és az új jelszót. A felhasználói nevet és jelszót egy térköze kell lennie. |
   | Parancsfájlműveletet... |Ezt a mezőt hagyja bejelölve. |
5. Válassza ki **létrehozása** alkalmazni a parancsfájlt. A parancsfájl befejezése után is kapcsolódik a fürthöz SSH használatával az új jelszóval.

## <a name="grantrevoke-access"></a>Hozzáférés biztosítása/visszavonása
A HDInsight-fürtök a következő HTTP webszolgáltatásokat (ezen szolgáltatások mindegyikéhez rendelkezik RESTful végpontok) rendelkezik:

* ODBC
* JDBC
* Ambari
* Oozie
* Lépni a Templeton

Alapértelmezés szerint ezek a szolgáltatások hozzáférés vonatkozóan biztosított. Akkor is a visszavonási/grant a hozzáférés segítségével [Azure CLI](hdinsight-administer-use-command-line.md#enabledisable-http-access-for-a-cluster) és [Azure PowerShell](hdinsight-administer-use-powershell.md#grantrevoke-access).

## <a name="find-the-subscription-id"></a>Az előfizetés-azonosító található

**Az Azure-előfizetéshez azonosítók kereséséhez**

1. Jelentkezzen be a [Portal][azure-portal].
2. Kattintson a **előfizetések**. Minden előfizetés rendelkezik egy nevet és egy.

Az egyes fürtökön Azure-előfizetés van kötve. Az előfizetés-azonosító jelenik meg a fürt **alapvető** csempére. Lásd: [listája és megjelenítése fürtök](#list-and-show-clusters).

## <a name="find-the-resource-group"></a>Az erőforráscsoport keresése
Az Azure Resource Manager módban mindegyik HDInsight-fürt létrehozása az Azure Resource Manager csoporttal. A Resource Manager-csoportot, amely egy fürt tagja jelenik meg:

* A fürt listája tartalmaz egy **erőforráscsoport** oszlop.
* Fürt **alapvető** csempére.  

Lásd: [listája és megjelenítése fürtök](#list-and-show-clusters).

## <a name="find-the-default-storage-account"></a>Az alapértelmezett tárfiók keresése
Minden egyes HDInsight-fürt rendelkezik egy alapértelmezett tárfiókot. Megjelenik az alapértelmezett tárfiók és a fürt kulcsait **Tárfiókok**. Lásd: [listája és megjelenítése fürtök](#list-and-show-clusters).

## <a name="run-hive-queries"></a>Hive-lekérdezések futtatása
Hive feladat nem futtatható közvetlenül az Azure portálról, de a Hive nézetben az Ambari webes felhasználói felületén.

**Ambari Hive nézet használata a Hive-lekérdezések futtatásához**

1. Jelentkezzen be az Ambari webes felhasználói felületén a HDInsight fürt felhasználói hitelesítő adatok használatával. Az alapértelmezett felhasználónév az **admin**. Az URL-címe **https://&lt;HDInsight-fürt neve > azurehdinsight.net**.
2. Hive nézet megnyitásához, az alábbi képernyőfelvételen látható módon:  

    ![HDInsight hive nézete](./media/hdinsight-administer-use-portal-linux/hdinsight-hive-view.png)
3. Kattintson a **lekérdezés** a felső menüben.
4. Adja meg a Hive-lekérdezések **Lekérdezésszerkesztő**, és kattintson a **Execute**.

## <a name="monitor-jobs"></a>Feladatok figyelése
Lásd: [kezelése HDInsight-fürtök az Ambari webes felhasználói felület használatával](hdinsight-hadoop-manage-ambari.md#monitoring).

## <a name="browse-files"></a>Fájlok tallózása
Az Azure portál használatával megkeresheti az alapértelmezett tároló tartalmának.

1. Jelentkezzen be [https://portal.azure.com](https://portal.azure.com).
2. Kattintson a **a HDInsight-fürtök** a meglévő fürtök elemet a bal oldali menüből.
3. Kattintson a fürt nevére. Ha a fürt lista hosszú, az oldal tetején a szűrő is használhatja.
4. Kattintson a **Tárfiókok** a fürt bal oldali menüből.
5. Kattintson a tárfiók.
7. Kattintson a **Blobok** csempére.
8. Kattintson az alapértelmezett tároló nevére.

## <a name="monitor-cluster-usage"></a>A figyelő fürt használata
A **használati** szakasz a HDInsight-fürt panelről az elérhető és a HDInsight együttes használata az előfizetés magok száma, valamint az ehhez a fürthöz, és azok elosztását vezérli a fürtben levő csomópontok számára lefoglalt magok száma információkat jelenít meg. Lásd: [listája és megjelenítése fürtök](#list-and-show-clusters).

> [!IMPORTANT]
> A HDInsight-fürt által nyújtott szolgáltatások figyeléséhez, az Ambari Web vagy az Ambari REST API-t kell használnia. További információ az Ambari használatával, lásd: [kezelése HDInsight-fürtök Ambari használatával](hdinsight-hadoop-manage-ambari.md)
>
>

## <a name="connect-to-a-cluster"></a>Csatlakozzon a fürthöz

* [A Hive használata a HDInsightban](hdinsight-hadoop-use-hive-ambari-view.md)
* [SSH használata a HDInsighttal](hdinsight-hadoop-linux-use-ssh-unix.md)

## <a name="next-steps"></a>Következő lépések
Ebben a cikkben megtanulta rendelkezik néhány alapvető adminisztratív funkciók. További tudnivalókért tekintse meg a következő cikkeket:

* [Felügyelheti a HDInsight az Azure PowerShell használatával](hdinsight-administer-use-powershell.md)
* [Felügyelheti a HDInsight az Azure parancssori felület használatával](hdinsight-administer-use-command-line.md)
* [A HDInsight-fürtök létrehozása](hdinsight-hadoop-provision-linux-clusters.md)
* [A Hive hdinsight használata](hdinsight-use-hive.md)
* [A Pig használata a Hdinsightban](hdinsight-use-pig.md)
* [Sqoop használata a Hdinsightban](hdinsight-use-sqoop.md)
* [Ismerkedés az Azure HDInsight](hdinsight-hadoop-linux-tutorial-get-started.md)
* [Azure HDInsight Hadoop verziójának van?](hdinsight-component-versioning.md)

[azure-portal]: https://portal.azure.com
[image-hadoopcommandline]: ./media/hdinsight-administer-use-portal-linux/hdinsight-hadoop-command-line.png "Hadoop parancssor"
