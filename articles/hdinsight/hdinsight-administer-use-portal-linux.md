---
title: "aaaManage Hadoop-fürtök a HDInsight az Azure portál használatával |} Microsoft Docs"
description: "Megtudhatja, hogyan toocreate és hello Azure portál használata a HDInsight-fürtök kezelése."
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
ms.openlocfilehash: c242d43d4ccea7cf1e7be19c3f3d7ed3c4f50918
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="manage-hadoop-clusters-in-hdinsight-by-using-hello-azure-portal"></a>Hdinsight Hadoop-fürtök kezelése hello Azure-portál használatával
[!INCLUDE [selector](../../includes/hdinsight-portal-management-selector.md)]

Hello segítségével [Azure-portálon][azure-portal], kezelheti az Azure HDInsight Hadoop-fürtök. Használjon hello lapon választó vonatkozó tudnivalókat más eszközök használatával hdinsight Hadoop-fürtök kezelése.

**Előfeltételek**

Ez a cikk megkezdése előtt rendelkeznie kell a következő elemek hello:

* **Azure-előfizetés**. Lásd: [Ingyenes Azure-fiók létrehozása](https://azure.microsoft.com/documentation/videos/get-azure-free-trial-for-testing-hadoop-in-hdinsight/).

## <a name="open-hello-portal"></a>Nyissa meg hello portál
1. Jelentkezzen be a túl[https://portal.azure.com](https://portal.azure.com).
2. Hello portál megnyitása után végezhetők el:

   * Kattintson a **új** a hello bal oldali menü toocreate új fürt:

       ![a HDInsight-fürt új gomb](./media/hdinsight-administer-use-portal-linux/azure-portal-new-button.png)
   * Kattintson a **a HDInsight-fürtök** hello a bal oldali menü toolist hello meglévő fürtök

       ![Az Azure portál HDInsight fürt gomb](./media/hdinsight-administer-use-portal-linux/azure-portal-hdinsight-button.png)

       Ha nem látja a HDInsight-fürt, kattintson a **további szolgáltatások** hello hello lista aljára, és kattintson a **a HDInsight-fürtök** alatt hello **Eszközintelligencia + analitika** a szakasz.


## <a name="create-clusters"></a>Fürtök létrehozása
[!INCLUDE [delete-cluster-warning](../../includes/hdinsight-delete-cluster-warning.md)]

HDInsight Hadoop széles tartomány-összetevők működik. Hello összetevők ellenőrzése és a támogatott hello listájáért lásd: [Azure HDInsight Hadoop verziójának van](hdinsight-component-versioning.md). Hello általános fürt létrehozása információkért lásd: [Hadoop létrehozása a HDInsight-fürtök](hdinsight-hadoop-provision-linux-clusters.md).

### <a name="access-control-requirements"></a>A hozzáférés-vezérlésre vonatkozó követelmények

HDInsight-fürtök létrehozásakor meg kell adnia egy Azure-előfizetés. Ezen a fürtön vagy egy új Azure-erőforráscsoportot, vagy egy meglévő erőforráscsoportot is létrehozható. Használhatja a következő lépéseket tooverify hello az engedélyeit a HDInsight-fürtök létrehozásához:

- toouse egy meglévő erőforráscsoportot.

    1. Jelentkezzen be toohello [Azure-portálon](https://portal.azure.com).
    2. Kattintson a **erőforráscsoportok** erőforráscsoportokból hello bal oldali menü toolist hello.
    3. Kattintson a kívánt toouse létrehozására a HDInsight-fürt hello erőforráscsoport.
    4. Kattintson a **hozzáférés-vezérlés (IAM)**, és ellenőrizze, hogy Ön (vagy egy csoportot, amely tartozik) rendelkeznek legalább hello közreműködői hozzáférés toohello erőforráscsoportot.

- Új erőforráscsoport toocreate

    1. Jelentkezzen be toohello [Azure-portálon](https://portal.azure.com).
    2. Kattintson a **előfizetés** hello bal oldali menüből. Rendelkezik a sárga kulcs ikon. Az előfizetések listája megjelenik.
    3. Kattintson a hello előfizetésre toocreate fürtök használatát. 
    4. Kattintson a **saját engedélyek**.  Azt illusztrálja a [szerepkör](../active-directory/role-based-access-control-what-is.md#built-in-roles) hello az előfizetésben. Legalább kell közreműködői hozzáférés toocreate HDInsight-fürthöz.

Ha hello NoRegisteredProviderFound hiba vagy hello MissingSubscriptionRegistration hiba, lásd: [hibaelhárítás általános az Azure-telepítés az Azure Resource Manager](../azure-resource-manager/resource-manager-common-deployment-errors.md).

## <a name="list-and-show-clusters"></a>Listában, és a fürt megjelenítése
1. Jelentkezzen be a túl[https://portal.azure.com](https://portal.azure.com).
2. Kattintson a **a HDInsight-fürtök** hello a bal oldali menü toolist hello meglévő fürtök.
3. Kattintson a hello fürt nevére. Ha hello fürtlista hosszú, a szűrő hello felül hello lap is használhatja.
4. Kattintson a fürt hello lista toosee hello – Áttekintés lapon:

    ![Az Azure portál HDInsight fürt alapjai](./media/hdinsight-administer-use-portal-linux/hdinsight-essentials.png)

    * **Irányítópult**: megnyílik hello fürt irányítópult, amely Ambari Web Linux-alapú fürtökhöz.
    * **Biztonságos rendszerhéj**: látható hello utasításokat tooconnect toohello fürtjét Secure Shell (SSH) kapcsolaton keresztül.
    * **Fürt méretezése**: lehetővé teszi a fürt feldolgozó csomópontjainak számát toochange hello.
    * **Törlés**: hello fürt törlése.
    * **Tevékenységi naplóit**: megjelenítése és a lekérdezés tevékenységi naplóit.
    * **Hozzáférés-vezérlés (IAM)**: szerepkör-hozzárendelésekkel.  Lásd: [szerepkör hozzárendelések toomanage tooyour Azure-előfizetés erőforrások eléréséhez használjon](../active-directory/role-based-access-control-configure.md).
    * **Címkék**: lehetővé teszi, hogy a hogy tooset kulcs/érték párok toodefine a felhőalapú szolgáltatások egy egyéni besorolást. Például létrehozhat nevű kulcs **projekt**, majd használja az adott projekthez tartozó összes szolgáltatás közös értéket.
    * **Hibáinak diagnosztizálásához és elhárításához**: hibaelhárítási információk megjelenítése.
    * **Zárolja**: hozzáadása a zárolás tooprevent hello fürt alatt, módosított vagy törölt.
    * **Automatizálási parancsfájl**: megjelenítési és exportálási hello Azure Resource Manager sablon hello fürthöz. Jelenleg csak exportálhatja hello függő Azure storage-fiók. Lásd: [létrehozása Linux-alapú Hadoop-fürtök használata Azure Resource Manager-sablonok hdinsight](hdinsight-hadoop-create-linux-clusters-arm-templates.md).
    * **Gyors üzembe helyezési**: információit jeleníti meg, amely segít a HDInsight használatának megkezdésében.
    * **A HDInsight eszközök**: segítő információk a HDInsight kapcsolódó eszközök.
    * **A fürt bejelentkezési**: hello fürt bejelentkezési adatok megjelenítéséhez.
    * **Előfizetés alapvető használati**: megjelenítési hello felhasznált és rendelkezésre álló magot az előfizetéséhez.
    * **Fürt méretezése**: növelését, és csökkentse a fürt feldolgozó csomópontok száma hello. Lásd:[fürtök méretezése](hdinsight-administer-use-management-portal.md#scale-clusters).
    * **Biztonságos rendszerhéj**: látható hello utasításokat tooconnect toohello fürtjét Secure Shell (SSH) kapcsolaton keresztül. További információ: [Az SSH használata HDInsighttal](hdinsight-hadoop-linux-use-ssh-unix.md).
    * **HDInsight-partnert**: hozzáadása hello jelenlegi HDInsight-partnert.
    * **Külső Metaadattárakat**: hello struktúra és az Oozie metastores megtekintése. hello metastores csak hello fürt létrehozási folyamata során lehet megadni. Lásd: [használni a Hive/Oozie metaadattárhoz](hdinsight-hadoop-provision-linux-clusters.md#use-hiveoozie-metastore).
    * **Parancsfájl-műveletek**: hello fürtön futtatni Bash parancsfájlok. Lásd: [testreszabása Linux-alapú HDInsight-fürtök használata parancsfájlművelet](hdinsight-hadoop-customize-cluster-linux.md).
    * **Alkalmazások**: hozzáadása a HDInsight-alkalmazások.  Lásd: [egyéni HDInsight-alkalmazások telepítése](hdinsight-apps-install-custom-applications.md).
    * **Tulajdonságok**: hello tulajdonságainak megtekintése.
    * **Storage-fiókok**: hello storage-fiókok és hello kulcsok megtekintéséhez. hello storage-fiókok vannak konfigurálva hello fürt létrehozási folyamata során.
    * **A fürt AAD-identitása**:
    * **Új támogatási kérelem**: lehetővé teszi a toocreate egy támogatási jegy, a Microsoft támogatási szolgálatához.
    
6. Kattintson a **tulajdonságok**:

    hello tulajdonságai a következők:

   * **Állomásnév**: fürt nevét.
   * **A fürt URL-cím**. hello Ambari webes felület hello URL-címe
   * **Állapot**: tartalmaznak megszakadt, fogadja el, ClusterStorageProvisioned, AzureVMConfiguration, HDInsightConfiguration, működési, fut, a hiba, törlése, törlése, időtúllépésbe került, DeleteQueued, DeleteTimedout, DeleteError, PatchQueued, CertRolloverQueued, ResizeQueued, ClusterCustomization
   * **A régióban**: Azure-beli hely. Támogatott Azure helyek listáját lásd: hello **régió** lévő legördülő lista [HDInsight árképzési](https://azure.microsoft.com/pricing/details/hdinsight/).
   * **Létrehozás dátuma**.
   * **Operációs rendszer**: vagy **Windows** vagy **Linux**.
   * **Típus**: Hadoop, HBase, Storm, Spark.
   * **Verzió**. Lásd: [HDInsight-verziókról](hdinsight-component-versioning.md)
   * **Előfizetés**: előfizetés nevét.
   * **Alapértelmezett adatforrás**: hello alapértelmezett fürt fájlrendszer.
   * **Munkavégző csomópontok mérete**.
   * **Csomópont méretének HEAD**.

## <a name="delete-clusters"></a>Fürtök törlése
A fürt törlése nem érinti hello alapértelmezett tárfiókot, vagy az összes kapcsolt tárfiókot. Használatával újból létrehozhatja hello fürtöt hello ugyanazon tárfiók, és ugyanazt a metaadattárakat hello. Az ajánlott a egy új alapértelmezett Blob tároló toouse hello fürt ismételt létrehozásakor.

1. Jelentkezzen be toohello [Portal][azure-portal].
2. Kattintson a **a HDInsight-fürtök** hello bal oldali menüből. Ha nem lát **a HDInsight-fürtök**, kattintson a **további szolgáltatások** első.
3. Kattintson a megjeleníteni kívánt toodelete hello fürtre.
4. Kattintson a **törlése** hello felső menüjében, és kövesse az utasításokat hello.

Lásd még: [fürtök szünet/Leállítás](#pauseshut-down-clusters).

## <a name="add-additional-storage-accounts"></a>További tárfiókok hozzáadása

A fürt létrehozása után hozzáadhat további Azure Storage-fiókok és az Azure Data Lake Store-fiók. További információkért lásd: [adja hozzá a további tárhely fiókok tooHDInsight](./hdinsight-hadoop-add-storage.md).

## <a name="scale-clusters"></a>Fürtök méretezése
hello fürt skálázás funkció lehetővé teszi, hogy anélkül, hogy toore fut az Azure HDInsight fürt által használt feldolgozó csomópontok száma toochange hello-hello fürt létrehozása.

> [!NOTE]
> Csak verzió 3.1.3 hdinsight clusters vagy annál magasabb támogatottak. Ha biztos benne, hogy a fürt hello verziója, ellenőrizheti a hello tulajdonságlapján.  Lásd: [listája és megjelenítése fürtök](#list-and-show-clusters).
>
>

a fürt a HDInsight által támogatott különböző típusú adatok csomópontok hello számának módosítása hello következményei:

* Hadoop

    Zökkenőmentesen növelheti hello adhatja meg, hogy minden folyamatban lévő vagy a futó feladatok befolyásolása nélkül fut egy Hadoop-fürt feldolgozó csomópontjainak számát. Új feladatokat is küldheti el, amíg hello művelet van folyamatban. A méretezési művelet sikertelen szabályosan kezeli, így hello fürt mindig marad működőképes állapotban.

    A Hadoop fürtök adatok csomópontok száma hello csökkentésével átméretezi, ha néhány hello fürt hello szolgáltatás újraindul. Ez a viselkedés hatására az összes futó és függőben lévő feladatok toofail művelet skálázás hello hello megvalósításának következő. Akkor is, azonban küldje el újra hello feladatok hello művelet végrehajtása után.
* HBase

    Zökkenőmentesen hozzáadása vagy eltávolítása a csomópontok tooyour HBase-fürtöt futtatása. A területi kiszolgálók hello skálázás művelet befejezése néhány percen belül automatikusan elosztását. Azonban Ön kézzel is eloszthatja hello területi kiszolgálók történő naplózásának révén a fürt és a következő parancsok parancssori ablakból futó hello toohello headnode:

        >pushd %HBASE_HOME%\bin
        >hbase shell
        >balancer

    Hello HBase rendszerhéj használatával kapcsolatos további információkért lásd:]
* Storm

    Zökkenőmentesen hozzáadása vagy eltávolítása adatok csomópontok tooyour Storm-fürt futása közben is. De hello skálázás művelet sikeres befejezése után kell toorebalance hello topológia.

    Kétféle módon valósítható meg újraelosztás:

  * A Storm webes felhasználói felület
  * Parancssori felület (CLI) eszköz

    Tekintse meg a toohello [alatt futó Apache Storm-dokumentáció](http://storm.apache.org/documentation/Understanding-the-parallelism-of-a-Storm-topology.html) további részleteket.

    HDInsight-fürt hello hello Storm webes felhasználói felület érhető el:

    ![A HDInsight alatt futó Storm méretezési egyensúlyozza ki újra](./media/hdinsight-administer-use-portal-linux/hdinsight-portal-scale-cluster-storm-rebalance.png)

    Például hogyan toouse hello CLI parancssori toorebalance hello Storm-topológia:

        ## Reconfigure hello topology "mytopology" toouse 5 worker processes,
        ## hello spout "blue-spout" toouse 3 executors, and
        ## hello bolt "yellow-bolt" toouse 10 executors
        $ storm rebalance mytopology -n 5 -e blue-spout=3 -e yellow-bolt=10

**tooscale fürtök**

1. Jelentkezzen be toohello [Portal][azure-portal].
2. Kattintson a **a HDInsight-fürtök** hello bal oldali menüből.
3. Kattintson a kívánt tooscale hello fürtre.
3. Kattintson a **fürt méretezése**.
4. Adja meg **számát a feldolgozó csomópontok**. Azure-előfizetések hello hello fürtcsomópontok számára vonatkozó korlátozást függően változik. Kapcsolatba léphet a számlázási támogatás tooincrease hello korlátot.  hello Költséginformációk hello végrehajtott módosítások csomópontok száma toohello tükrözi.

    ![HDInsight hadoop hbase storm spark méretezési](./media/hdinsight-administer-use-portal-linux/hdinsight-portal-scale-cluster.png)

## <a name="pauseshut-down-clusters"></a>Fürtök szünet/leállítása

Hadoop-feladatokat többsége kötegelt feladatok, amelyek csak esetenként futnak. A legtöbb Hadoop-fürtök vonatkoznak-e nagy időszakok idő hello fürthöz nem használja a feldolgozáshoz. A HDInsight az Azure Storage szolgáltatásban tárolja az adatokat, így biztonságosan törölhet olyan fürtöket, amelyek nincsenek használatban.
Ráadásul a HDInsight-fürtök akkor is díjkötelesek, amikor éppen nincsenek használatban. Mivel hello díjak hello fürt sokszor több mint hello a tárolási, érdemes gazdasági toodelete fürtök amikor nincsenek használatban.

Számos módon meg a program hello folyamat:

* Az Azure Data Factory felhasználó. Lásd: [létrehozása igény szerinti Linux-alapú Hadoop-fürtök Azure Data Factory használatával hdinsight](hdinsight-hadoop-create-linux-clusters-adf.md) igény szerinti HDInsight létrehozásához kapcsolódó szolgáltatások.
* Azure PowerShell használatával.  Lásd: [repülési késleltetés adatok elemzése](hdinsight-analyze-flight-delay-data.md).
* Az Azure CLI használata. Lásd: [kezelése HDInsight-fürtök Azure parancssori felület használatával](hdinsight-administer-use-command-line.md).
* A HDInsight .NET SDK használata. Lásd: [nyújt Hadoop-feladatokat](hdinsight-submit-hadoop-jobs-programmatically.md).

Díjszabási információkért hello, lásd: [HDInsight árképzési](https://azure.microsoft.com/pricing/details/hdinsight/). toodelete fürt hello Portal, lásd: [fürtök törlése](#delete-clusters)


## <a name="upgrade-clusters"></a>Fürtök frissítése

Lásd: [frissítése HDInsight fürt tooa újabb verzióra](./hdinsight-upgrade-cluster.md).

## <a name="change-passwords"></a>Jelszavak módosítása
HDInsight-fürtök lehet két felhasználói fiókot. a HDInsight fürt felhasználói fiók (más néven hello HTTP-felhasználói fiók) és az SSH-felhasználói fiókhoz hello hello létrehozási folyamata során jönnek létre. Hello Ambari webes felhasználói felület toochange hello fürt felhasználói fiók felhasználónevét és a jelszó és a parancsfájl műveletek toochange hello SSH-felhasználói fiókhoz

### <a name="change-hello-cluster-user-password"></a>Hello fürt felhasználói jelszó módosítása
Hello Ambari webes felhasználói felületén toochange hello fürt felhasználói jelszó is használhatja. a tooAmbari toolog, a hello meglévő fürt felhasználónevet és jelszót kell használnia.

> [!NOTE]
> Változó hello fürt (rendszergazda) felhasználói jelszó műveletek lefutott-e a fürt toofail parancsfájl okozhat. Ha bármely a megőrzött Parancsfájlműveletek munkavégző csomópontokhoz célzó, ezek a parancsfájlok nem tud csomópontok toohello fürt átméretezési műveletek révén hozzáadása. A Parancsfájlműveletek további információkért lásd: [testreszabása HDInsight-fürtök Parancsfájlműveletek segítségével](hdinsight-hadoop-customize-cluster-linux.md).
>
>

1. A bejelentkezéshez toohello Ambari webes felhasználói felületén hello HDInsight fürt felhasználói hitelesítő adatokat. hello alapértelmezett felhasználónév az **admin**. URL-cím hello **https://&lt;HDInsight-fürt neve > azurehdinsight.net**.
2. Kattintson a **Admin** hello felső menüjében, majd kattintson a "Ambari kezelése".
3. Hello bal oldali menüben kattintson **felhasználók**.
4. Kattintson a **Admin**.
5. Kattintson a **jelszómódosítás**.

Ambari majd módosítja hello jelszót hello fürt összes csomópontján.

### <a name="change-hello-ssh-user-password"></a>Hello SSH felhasználói jelszó módosítása
1. Egy szövegszerkesztővel, mentse a szöveg nevű fájlba a következő hello **changepassword.sh**.

   > [!IMPORTANT]
   > Hello sor befejezési LF használó szerkesztővé kell használnia. Ha hello szerkesztő CRLF használ, majd hello parancsfájl nem működik.
   >
   >

        #! /bin/bash
        USER=$1
        PASS=$2

        usermod --password $(echo $PASS | openssl passwd -1 -stdin) $USER
2. Töltse fel a hello fájl tooa tárhelyen találhatók, amely a HDInsight egy HTTP vagy HTTPS-címet a elérhetők. Például egy nyilvános fájl tárolja, például a OneDrive- vagy Azure Blob storage. Hello URI (HTTP vagy HTTPS-cím) toohello fájl, akkor menteni, mert ezt az URI a következő lépésben hello van szükség.
3. A hello Azure-portálon, kattintson az **a HDInsight-fürtök**.
4. Kattintson a HDInsight-fürthöz.
4. Kattintson a **parancsfájl-műveletek**.
4. A hello **Parancsfájlműveletek** panelen válassza **nyújt új**. Ha hello **parancsfájlművelet** panel jelenik meg, írja be a következő információ hello:

   | Mező | Érték |
   | --- | --- |
   | Név |Ssh jelszó módosítása |
   | Bash parancsfájlok URI |hello URI toohello changepassword.sh fájl |
   | Csomópontok (Head munkavégző, Nimbus, felügyelő, Zookeeper, stb.) |Az összes csomópont felsorolt ✓ |
   | Paraméterek |Adja meg a hello SSH-felhasználónév és hello új jelszót. Egy tárolóhely hello felhasználónév és jelszó hello között kell lennie. |
   | Parancsfájlműveletet... |Ezt a mezőt hagyja bejelölve. |
5. Válassza ki **létrehozása** tooapply hello parancsfájl. Hello parancsfájl befejezése után képes tooconnect toohello fürt SSH használatával hello új jelszóval áll.

## <a name="grantrevoke-access"></a>Hozzáférés biztosítása/visszavonása
A HDInsight-fürtök (ezen szolgáltatások mindegyikéhez rendelkezik RESTful végpontok) HTTP-webszolgáltatások a következő hello rendelkezik:

* ODBC
* JDBC
* Ambari
* Oozie
* Lépni a Templeton

Alapértelmezés szerint ezek a szolgáltatások hozzáférés vonatkozóan biztosított. Akkor is a visszavonási/grant hello hozzáférés segítségével [Azure CLI](hdinsight-administer-use-command-line.md#enabledisable-http-access-for-a-cluster) és [Azure PowerShell](hdinsight-administer-use-powershell.md#grantrevoke-access).

## <a name="find-hello-subscription-id"></a>Hello előfizetés-azonosító található

**toofind az Azure-előfizetéshez azonosítók**

1. Jelentkezzen be toohello [Portal][azure-portal].
2. Kattintson a **előfizetések**. Minden előfizetés rendelkezik egy nevet és egy.

Az egyes fürtökön kapcsolt tooan Azure-előfizetés. előfizetés-azonosító jelenik meg hello fürt hello **alapvető** csempére. Lásd: [listája és megjelenítése fürtök](#list-and-show-clusters).

## <a name="find-hello-resource-group"></a>Hello erőforráscsoportban található
Hello Azure Resource Manager módban mindegyik HDInsight-fürt létrehozása az Azure Resource Manager csoporttal. a fürt tartozik tooappears az erőforrás-kezelő csoport hello:

* hello fürt listája tartalmaz egy **erőforráscsoport** oszlop.
* Fürt **alapvető** csempére.  

Lásd: [listája és megjelenítése fürtök](#list-and-show-clusters).

## <a name="find-hello-default-storage-account"></a>Hello alapértelmezett tárfiók keresése
Minden egyes HDInsight-fürt rendelkezik egy alapértelmezett tárfiókot. hello alapértelmezett tárfiók és a kulcsok alatt megjelenik a fürt **Tárfiókok**. Lásd: [listája és megjelenítése fürtök](#list-and-show-clusters).

## <a name="run-hive-queries"></a>Hive-lekérdezések futtatása
Hive feladat nem futtatható közvetlenül a hello Azure-portálon, de használhatja hello Hive View az Ambari webes felhasználói felületén.

**Ambari Hive nézet használata a toorun Hive-lekérdezések**

1. A bejelentkezéshez toohello Ambari webes felhasználói felületén hello HDInsight fürt felhasználói hitelesítő adatokat. hello alapértelmezett felhasználónév az **admin**. URL-cím hello **https://&lt;HDInsight-fürt neve > azurehdinsight.net**.
2. Nyissa meg a Hive View, ahogy az alábbi képernyőfelvétel a hello:  

    ![HDInsight hive nézete](./media/hdinsight-administer-use-portal-linux/hdinsight-hive-view.png)
3. Kattintson a **lekérdezés** hello felső menüjében.
4. Adja meg a Hive-lekérdezések **Lekérdezésszerkesztő**, és kattintson a **Execute**.

## <a name="monitor-jobs"></a>Feladatok figyelése
Lásd: [kezelése HDInsight-fürtök hello Ambari webes felhasználói felület használatával](hdinsight-hadoop-manage-ambari.md#monitoring).

## <a name="browse-files"></a>Fájlok tallózása
Hello Azure-portál használatával, böngészhet a hello alapértelmezett tároló hello tartalmát.

1. Jelentkezzen be a túl[https://portal.azure.com](https://portal.azure.com).
2. Kattintson a **a HDInsight-fürtök** hello a bal oldali menü toolist hello meglévő fürtök.
3. Kattintson a hello fürt nevére. Ha hello fürtlista hosszú, a szűrő hello felül hello lap is használhatja.
4. Kattintson a **Tárfiókok** hello fürt bal oldali menüből.
5. Kattintson a tárfiók.
7. Kattintson a hello **Blobok** csempére.
8. Kattintson a hello alapértelmezett tároló neve.

## <a name="monitor-cluster-usage"></a>A figyelő fürt használata
Hello **használati** hello HDInsight-fürt panelén szakasza magok elérhető tooyour előfizetés a hdinsight eszközzel használható hello számát, valamint toothis fürt, és hogyan lefoglalt magok száma hello információit jeleníti meg. a fürt hello csomópontja számára lefoglalt. Lásd: [listája és megjelenítése fürtök](#list-and-show-clusters).

> [!IMPORTANT]
> hello által biztosított toomonitor hello szolgáltatások HDInsight-fürtre, használja az Ambari Web vagy hello Ambari REST API-t kell. További információ az Ambari használatával, lásd: [kezelése HDInsight-fürtök Ambari használatával](hdinsight-hadoop-manage-ambari.md)
>
>

## <a name="connect-tooa-cluster"></a>Csatlakoztassa tooa fürtöt

* [A Hive használata a HDInsightban](hdinsight-hadoop-use-hive-ambari-view.md)
* [SSH használata a HDInsighttal](hdinsight-hadoop-linux-use-ssh-unix.md)

## <a name="next-steps"></a>Következő lépések
Ebben a cikkben megtanulta rendelkezik néhány alapvető adminisztratív funkciók. toolearn több, tekintse meg a következő cikkek hello:

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
