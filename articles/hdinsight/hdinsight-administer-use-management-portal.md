---
title: "Windows-alapú Hadoop-fürtök aaaManage a Hdinsightban az Azure-portálon hello |} Microsoft Docs"
description: "Megtudhatja, hogyan tooadminister HDInsight-szolgáltatás. Hozzon létre egy HDInsight fürt, nyitott hello interaktív JavaScript-konzoljába, és nyissa meg hello Hadoop parancskonzolról."
services: hdinsight
documentationcenter: 
tags: azure-portal
author: mumian
manager: jhubbard
editor: cgronlun
ms.assetid: 9295a988-bd88-453a-8c8b-55fa103bf39c
ms.service: hdinsight
ms.workload: big-data
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/25/2017
ms.author: jgao
ROBOTS: NOINDEX
ms.openlocfilehash: a237726b0e37a08005ce22e96581739e93edb050
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="manage-windows-based-hadoop-clusters-in-hdinsight-by-using-hello-azure-portal"></a>Hdinsight Windows-alapú Hadoop-fürtök kezelése hello Azure-portál használatával

Hello segítségével [Azure-portálon][azure-portal], akkor a Windows-alapú Hadoop-fürtök létrehozása az Azure HDInsight, Hadoop felhasználói jelszó módosításához és Remote Desktop Protocol (RDP) engedélyezése, hogy hozzáférjen a hello Hadoop parancskonzolról hello fürtön.

a cikkben szereplő információkat hello tooWindow-alapú HDInsight-fürtök csak érvényes. Linux-alapú fürtökön kezeléséről további információért lásd: [kezelése Hadoop-fürtöket a HDInsight használatával hello Azure-portálon](hdinsight-administer-use-portal-linux.md).

> [!IMPORTANT]
> Linux hello azt az egyetlen operációs rendszer, használja a HDInsight 3.4 vagy újabb verziója. További tudnivalókért lásd: [A HDInsight elavulása Windows rendszeren](hdinsight-component-versioning.md#hdinsight-windows-retirement).


## <a name="prerequisites"></a>Előfeltételek

Ez a cikk elkezdéséhez hello következő kell rendelkeznie:

* **Azure-előfizetés**. Lásd: [Ingyenes Azure-fiók létrehozása](https://azure.microsoft.com/documentation/videos/get-azure-free-trial-for-testing-hadoop-in-hdinsight/).
* **Az Azure Storage-fiók** -egy HDInsight-fürtöt használ egy Azure Blob storage tárolók hello alapértelmezett fájlrendszer. Hogyan nyújt az Azure Blob storage a HDInsight-fürtökkel zökkenőmentes élményt kapcsolatos további információkért lásd: [az Azure Blob Storage a hdinsight eszközzel](hdinsight-hadoop-use-blob-storage.md). Egy Azure Storage-fiók létrehozásával kapcsolatos részletekért lásd: [hogyan tooCreate Tárfiók](../storage/common/storage-create-storage-account.md).

## <a name="open-hello-portal"></a>Nyissa meg a portál hello
1. Jelentkezzen be a túl[https://portal.azure.com](https://portal.azure.com).
2. Hello portál megnyitása után végezhetők el:

   * Kattintson a **új** a hello bal oldali menü toocreate új fürt:

       ![a HDInsight-fürt új gomb](./media/hdinsight-administer-use-management-portal/azure-portal-new-button.png)
   * Kattintson a **a HDInsight-fürtök** hello bal oldali menüből.

       ![Az Azure portál HDInsight fürt gomb](./media/hdinsight-administer-use-management-portal/azure-portal-hdinsight-button.png)

     Ha **HDInsight** nem jelennek meg hello bal oldali menüben, kattintson a **Tallózás**.

     ![Az Azure portál fürt gomb](./media/hdinsight-administer-use-management-portal/azure-portal-browse-button.png)

## <a name="create-clusters"></a>Fürtök létrehozása
Hello létrehozása hello portál használatával, lásd: [HDInsight-fürtök létrehozása](hdinsight-hadoop-provision-linux-clusters.md).

HDInsight Hadoop széles tartomány-összetevők működik. Hello összetevők ellenőrzése és a támogatott hello listájáért lásd: [Azure HDInsight Hadoop verziójának van](hdinsight-component-versioning.md). Testre szabhatja a HDInsight hello a következő lehetőségek egyikének használatával:

* Parancsfájlművelet toorun egyéni parancsfájlok használata, amely egy fürt tooeither testre szabhatja a fürt konfigurációjának módosítása, vagy például Giraph vagy Solr egyéni összetevők telepítéséhez. További információkért lásd: [testreszabása HDInsight-fürtjéhez parancsfájlművelet](hdinsight-hadoop-customize-cluster.md).
* Testreszabási Fürtparaméterek hello hello HDInsight .NET SDK vagy az Azure PowerShell használata a fürt létrehozása során. Konfigurációs módosítások keresztül hello fürt hello élettartama majd megmaradnak, és nem érinti a fürt csomópont reimages Azure platformon rendszeres karbantartás hajtja végre. A fürt hello testreszabási paraméterek használatával további információkért lásd: [HDInsight-fürtök létrehozása](hdinsight-hadoop-provision-linux-clusters.md).
* Néhány natív Java-összetevők, például Mahout és kaszkádolás, JAR-fájlok formájában hello fürtön futtathatja. A JAR-fájlok elosztott tooAzure Blob-tároló lehet, és tooHDInsight fürtök benyújtott Hadoop-feladat elküldése mechanizmusokon keresztül. További információkért lásd: [nyújt Hadoop feladatok programozott módon](hdinsight-submit-hadoop-jobs-programmatically.md).

  > [!NOTE]
  > Ha problémába ütközik JAR fájlok tooHDInsight fürtök telepítése, vagy hívja a JAR-fájlok a HDInsight-fürtökön, forduljon a [Microsoft Support](https://azure.microsoft.com/support/options/).
  >
  > Egymásra épülő HDInsight nem támogatja, és nincs for Microsoft Support jogosult. Támogatott összetevők listáját lásd: [What's new in HDInsight által biztosított hello fürt verziók](hdinsight-component-versioning.md).
  >
  >

A távoli asztali kapcsolattal hello fürtön egyéni szoftver telepítése nem támogatott. Ne hello átjárócsomópont hello meghajtóit lévő fájlok tárolásához mert azok elvesznek toore szüksége-hello fürtöket létrehozni. Javasoljuk, hogy az Azure Blob Storage tárolóban lévő fájlok tárolásához. A BLOB storage egy állandó.

## <a name="list-and-show-clusters"></a>Listában, és a fürt megjelenítése
1. Jelentkezzen be a túl[https://portal.azure.com](https://portal.azure.com).
2. Kattintson a **a HDInsight-fürtök** hello bal oldali menüből.
3. Kattintson a hello fürt nevére. Ha hello fürtlista hosszú, a szűrő hello felül hello lap is használhatja.
4. Kattintson duplán a hello listája tooshow hello részleteinek a fürtöt.

    **Menü és essentials**:

    ![Az Azure portál HDInsight fürt alapjai](./media/hdinsight-administer-use-management-portal/hdinsight-essentials.png)

   * toocustomize hello menüben kattintson a jobb gombbal a hello menüben bárhol, és kattintson **Testreszabás**.
   * **Beállítások** és **összes beállítás**: Megjeleníti hello **beállítások** hello fürt, amely lehetővé teszi tooaccess panel részletes hello fürt konfigurációs adatait.
   * **Irányítópult**, **fürt irányítópult** és **URL-cím: ezek a összes módon tooaccess hello fürt irányítópult, amely Ambari Web Linux-alapú fürtökhöz. -**Biztonságos rendszerhéj **: Hello utasításokat tooconnect toohello fürtjét Secure Shell (SSH) kapcsolaton keresztül jeleníti meg.
   * **Fürt méretezése**: lehetővé teszi a fürt feldolgozó csomópontjainak számát toochange hello.
   * **Törlés**: hello fürt törlése.
   * **Gyors üzembe helyezés**: információit jeleníti meg, amelyek segítségével a HDInsight használatának megkezdésében.
   * ** Felhasználók: Lehetővé teszi a tooset engedélyeinek *portál felügyeleti* a fürt más felhasználók számára az Azure-előfizetése.

     > [!IMPORTANT]
     > Ez *csak* hatással van a hozzáférést és engedélyeket toothis fürtben lévő hello Azure-portálon, és nincs hatással a kapcsolódást tooor küldés feladatok toohello HDInsight-fürthöz.
     >
     >
   * **Címkék**: a címkék lehetővé meg tooset kulcs/érték párok toodefine a felhőalapú szolgáltatások egy egyéni besorolást. Például létrehozhat nevű kulcs **projekt**, majd használja az adott projekthez tartozó összes szolgáltatás közös értéket.
   * **Az Ambari nézetek**: tooAmbari webes hivatkozásokat tartalmaz.

     > [!IMPORTANT]
     > hello által biztosított toomanage hello szolgáltatások HDInsight-fürtre, használja az Ambari Web vagy hello Ambari REST API-t kell. További információ az Ambari használatával, lásd: [kezelése HDInsight-fürtök Ambari használatával](hdinsight-hadoop-manage-ambari.md).
     >
     >

     **Használati**:

     ![Az Azure portál HDInsight fürt használata](./media/hdinsight-administer-use-management-portal/hdinsight-portal-cluster-usage.png)
5. Kattintson a **beállítások**.

    ![Az Azure portál HDInsight fürt használata](./media/hdinsight-administer-use-management-portal/hdinsight.portal.cluster.settings.png)

   * **Tulajdonságok**: hello tulajdonságainak megtekintése.
   * **A fürt AAD-identitása**:
   * **Az Azure Storage-kulcsok**: hello alapértelmezett tárfiók és a kulcsok megtekintéséhez. a beállítás hello tárfiók hello fürt létrehozási folyamata során.
   * **A fürt bejelentkezési**: hello fürt HTTP felhasználónév és jelszó módosítása.
   * **Külső Metaadattárakat**: hello struktúra és az Oozie metastores megtekintése. hello metastores csak hello fürt létrehozási folyamata során lehet megadni.
   * **Fürt méretezése**: növelését, és csökkentse a fürt feldolgozó csomópontok száma hello.
   * **A távoli asztal**: engedélyezése és letiltása a távoli asztal (RDP) eléréséhez és hello RDP felhasználónév konfigurálása.  hello távoli asztalhoz tartozó felhasználónév hello HTTP felhasználónévben különbözőnek kell lennie.
   * **Bejegyzett partner**:

     > [!NOTE]
     > Ez az egy általános rendelkezésre álló beállítások listája; nem minden tárolódjanak minden fürt esetében.
     >
     >
6. Kattintson a **tulajdonságok**:

    hello tulajdonságok szakaszának hello következőket tartalmazza:

   * **Állomásnév**: fürt nevét.
   * **A fürt URL-cím**.
   * **Állapot**: tartalmaznak megszakadt, fogadja el, ClusterStorageProvisioned, AzureVMConfiguration, HDInsightConfiguration, működési, fut, a hiba, törlése, törlése, időtúllépésbe került, DeleteQueued, DeleteTimedout, DeleteError, PatchQueued, CertRolloverQueued, ResizeQueued, ClusterCustomization
   * **A régióban**: Azure-beli hely. Támogatott Azure helyek listáját lásd: hello **régió** lévő legördülő lista [HDInsight árképzési](https://azure.microsoft.com/pricing/details/hdinsight/).
   * **Létrehozott adatokat**.
   * **Operációs rendszer**: vagy **Windows** vagy **Linux**.
   * **Típus**: Hadoop, HBase, Storm, Spark.
   * **Verzió**. Lásd: [HDInsight-verziókról](hdinsight-component-versioning.md)
   * **Előfizetés**: előfizetés nevét.
   * **Előfizetés-azonosító**.
   * **Elsődleges adatforrás**. hello Azure Blob storage-fiók hello alapértelmezettként használt Hadoop-fájlrendszer.
   * **IP-címek munkavégző csomópontokhoz**.
   * **Átjárócsomópont tarifacsomag**.

## <a name="delete-clusters"></a>Fürtök törlése
Törli a fürt hello alapértelmezett tárfiókot, vagy az összes kapcsolt tárfiókot nem törli. Használatával újból létrehozhatja hello fürtöt hello ugyanazon tárfiók, és ugyanazt a metaadattárakat hello.

1. Jelentkezzen be toohello [Portal][azure-portal].
2. Kattintson a **összes tallózása** hello bal oldali menüben kattintson **a HDInsight-fürtök**, kattintson a fürt nevére.
3. Kattintson a **törlése** hello felső menüjében, és kövesse az utasításokat hello.

Lásd még: [fürtök szünet/Leállítás](#pauseshut-down-clusters).

## <a name="scale-clusters"></a>Fürtök méretezése
hello fürt skálázás funkció lehetővé teszi, hogy anélkül, hogy toore fut az Azure HDInsight fürt által használt feldolgozó csomópontok száma toochange hello-hello fürt létrehozása.

> [!NOTE]
> Csak verzió 3.1.3 hdinsight clusters vagy annál magasabb támogatottak. Ha biztos benne, hogy a fürt hello verziója, ellenőrizheti a hello tulajdonságlapján.  Lásd: [listája és megjelenítése fürtök](#list-and-show-clusters).
>
>

a fürt a HDInsight által támogatott különböző típusú adatok csomópontok hello számának módosítása hello következményei:

* Hadoop

    Zökkenőmentesen növelheti hello adhatja meg, hogy minden folyamatban lévő vagy a futó feladatok befolyásolása nélkül fut egy Hadoop-fürt feldolgozó csomópontjainak számát. Új feladatokat is küldheti el, amíg hello művelet van folyamatban. A méretezési művelet sikertelen szabályosan kezeli, így hello fürt mindig marad működőképes állapotban.

    A Hadoop fürtök adatok csomópontok száma hello csökkentésével átméretezi, ha néhány hello fürt hello szolgáltatás újraindul. Ennek hatására az összes futó és függőben lévő feladatok toofail művelet skálázás hello hello megvalósításának következő. Akkor is, azonban küldje el újra hello feladatok hello művelet végrehajtása után.
* HBase

    Zökkenőmentesen hozzáadása vagy eltávolítása a csomópontok tooyour HBase-fürtöt futtatása. A területi kiszolgálók hello skálázás művelet befejezése néhány percen belül automatikusan elosztását. Azonban Ön kézzel is eloszthatja hello területi kiszolgálók jelentkezzen be a fürt és a következő parancsok parancssori ablakból futó hello hello headnode:

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

    ![A HDInsight alatt futó storm méretezési egyensúlyozza ki újra](./media/hdinsight-administer-use-management-portal/hdinsight-portal-scale-cluster-storm-rebalance.png)

    Például hogyan toouse hello CLI parancssori toorebalance hello Storm-topológia:

        ## Reconfigure hello topology "mytopology" toouse 5 worker processes,
        ## hello spout "blue-spout" toouse 3 executors, and
        ## hello bolt "yellow-bolt" toouse 10 executors
        $ storm rebalance mytopology -n 5 -e blue-spout=3 -e yellow-bolt=10

**tooscale fürtök**

1. Jelentkezzen be toohello [Portal][azure-portal].
2. Kattintson a **összes tallózása** hello bal oldali menüben kattintson **a HDInsight-fürtök**, kattintson a fürt nevére.
3. Kattintson a **beállítások** hello felső menüben, majd kattintson a **méretezési fürt**.
4. Adja meg **számát a feldolgozó csomópontok**. Azure-előfizetések hello fürtcsomópont hello számára vonatkozó korlátozást függően változik. Kapcsolatba léphet a számlázási támogatás tooincrease hello korlátot.  hello Költséginformációk hello végrehajtott módosítások csomópontok száma toohello fogja tartalmazni.

    ![HDInsight Hadoop HBase Storm Spark méretezési](./media/hdinsight-administer-use-management-portal/hdinsight.portal.scale.cluster.png)

## <a name="pauseshut-down-clusters"></a>Fürtök szünet/leállítása
A legtöbb Hadoop-feladat kötegelt feladatok, amelyek csak esetenként futott. A legtöbb Hadoop-fürtök vonatkoznak-e nagy időszakok idő hello fürthöz nem használja a feldolgozáshoz. A HDInsight az Azure Storage szolgáltatásban tárolja az adatokat, így biztonságosan törölhet olyan fürtöket, amelyek nincsenek használatban.
Ráadásul a HDInsight-fürtök akkor is díjkötelesek, amikor éppen nincsenek használatban. Mivel hello díjak hello fürt sokszor több mint hello a tárolási, érdemes gazdasági toodelete fürtök amikor nincsenek használatban.

Számos módon meg a program hello folyamat:

* Az Azure Data Factory felhasználó. Lásd: [Azure HDInsight társított szolgáltatás](../data-factory/data-factory-compute-linked-services.md) és [alakít át és elemez az Azure Data Factory használatával](../data-factory/data-factory-data-transformation-activities.md) igény szerinti és önálló definiált hdinsight összekapcsolt szolgáltatások.
* Azure PowerShell használatával.  Lásd: [repülési késleltetés adatok elemzése](hdinsight-analyze-flight-delay-data.md).
* Az Azure CLI használata. Lásd: [kezelése HDInsight-fürtök Azure parancssori felület használatával](hdinsight-administer-use-command-line.md).
* A HDInsight .NET SDK használata. Lásd: [nyújt Hadoop-feladatokat](hdinsight-submit-hadoop-jobs-programmatically.md).

Díjszabási információkért hello, lásd: [HDInsight árképzési](https://azure.microsoft.com/pricing/details/hdinsight/). toodelete fürt hello Portal, lásd: [fürtök törlése](#delete-clusters)

## <a name="change-cluster-username"></a>Változás fürt felhasználónév
HDInsight-fürtök lehet két felhasználói fiókot. a HDInsight fürt felhasználói fiók hello hello létrehozási folyamata során jön létre. RDP-kapcsolaton keresztül hello fürt eléréséhez egy RDP-felhasználói fiókot is létrehozhat. Lásd: [engedélyezése a távoli asztal](#connect-to-hdinsight-clusters-by-using-rdp).

**toochange hello HDInsight-fürt felhasználónév és jelszó**

1. Jelentkezzen be toohello [Portal][azure-portal].
2. Kattintson a **összes tallózása** hello bal oldali menüben kattintson **a HDInsight-fürtök**, kattintson a fürt nevére.
3. Kattintson a **beállítások** hello felső menüben, majd kattintson a **fürt bejelentkezési**.
4. Ha **fürt bejelentkezési** lett engedélyezve van, meg kell nyomnia **letiltása**, és kattintson a **engedélyezése** hello felhasználónév és jelszó módosítása előtt...
5. Változás hello **fürt bejelentkezési neve** és/vagy hello **fürt bejelentkezési jelszó**, és kattintson a **mentése**.

    ![A HDInsight fürt felhasználói felhasználónév jelszó http felhasználó módosítása](./media/hdinsight-administer-use-management-portal/hdinsight.portal.change.username.password.png)

## <a name="grantrevoke-access"></a>Hozzáférés biztosítása/visszavonása
A HDInsight-fürtök (ezen szolgáltatások mindegyikéhez rendelkezik RESTful végpontok) HTTP-webszolgáltatások a következő hello rendelkezik:

* ODBC
* JDBC
* Ambari
* Oozie
* Lépni a Templeton

Alapértelmezés szerint ezek a szolgáltatások hozzáférés vonatkozóan biztosított. Meg is revoke/grant hello hozzáférés a hello Azure-portálon.

> [!NOTE]
> Hello hozzáférés biztosítása/visszavonása, visszaállítja, hello fürt felhasználónevet és jelszót.
>
>

**toogrant vagy a visszavonáshoz HTTP web services elérése**

1. Jelentkezzen be toohello [Portal][azure-portal].
2. Kattintson a **összes tallózása** hello bal oldali menüben kattintson **a HDInsight-fürtök**, kattintson a fürt nevére.
3. Kattintson a **beállítások** hello felső menüben, majd kattintson a **fürt bejelentkezési**.
4. Ha **fürt bejelentkezési** lett engedélyezve van, meg kell nyomnia **letiltása**, és kattintson a **engedélyezése** hello felhasználónév és jelszó módosítása előtt...
5. A **fürt bejelentkezési felhasználónevének** és **fürt bejelentkezési jelszó**, adja meg hello új felhasználónevet és jelszót (illetve) hello fürt.
6. Kattintson a **SAVE** (Mentés) gombra.

    ![HDInsight teljes http webes hozzáférés eltávolítása](./media/hdinsight-administer-use-management-portal/hdinsight.portal.change.username.password.png)

## <a name="find-hello-default-storage-account"></a>Hello alapértelmezett tárfiók keresése
Minden egyes HDInsight-fürt rendelkezik egy alapértelmezett tárfiókot. hello alapértelmezett tárfiók és a kulcsok alatt megjelenik a fürt **beállítások**/**tulajdonságok**/**Azure Storage kulcsok**. Lásd: [listája és megjelenítése fürtök](#list-and-show-clusters).

## <a name="find-hello-resource-group"></a>Hello erőforráscsoportban található
Hello Azure Resource Manager módban mindegyik HDInsight-fürt létrehozása egy Azure-erőforrás csoporttal. hello Azure-erőforráscsoportot, amely egy fürt tartozik a tooappears:

* hello fürt listája tartalmaz egy **erőforráscsoport** oszlop.
* Fürt **alapvető** csempére.  

Lásd: [listája és megjelenítése fürtök](#list-and-show-clusters).

## <a name="open-hdinsight-query-console"></a>Nyissa meg a HDInsight lekérdezés konzolt
HDInsight lekérdezés konzol hello hello a következő szolgáltatásokat tartalmazza:

* **Hive szerkesztő**: webes felülete A grafikus felhasználói Felülettel Hive-feladatok elküldésekor.  Lásd: [Hive futtatása lekérdezések használatával hello lekérdezés konzol](hdinsight-hadoop-use-hive-query-console.md).

    ![HDInsight portál hive szerkesztő](./media/hdinsight-administer-use-management-portal/hdinsight-hive-editor.png)
* **Feladatelőzmények**: Monitor Hadoop-feladatokat.  

    ![HDInsight portál feladatelőzmények](./media/hdinsight-administer-use-management-portal/hdinsight-job-history.png)

    Kattintson a **lekérdezésnév** tooshow hello részletek feladat tulajdonságai, például **feladat lekérdezés**, és ** Job Output. Hello lekérdezés és a hello kimeneti tooyour munkaállomás is letöltheti.
* **Fájl a böngésző**: keresse meg az alapértelmezett tárfiók hello és hello társított tárfiókokat.

    ![HDInsight portál fájl böngésző Tallózás](./media/hdinsight-administer-use-management-portal/hdinsight-file-browser.png)

    Hello képernyőfelvétel a hello  **<Account>**  típus azt jelenti, hello cikk egy Azure storage-fiókot.  Kattintson a hello fiók neve toobrowse hello fájlokat.
* **Hadoop-UI**.

    ![HDInsight portal Hadoop felhasználói felület](./media/hdinsight-administer-use-management-portal/hdinsight-hadoop-ui.png)

    A **Hadoop felhasználói felület*, keresse meg a fájlokat, és ellenőrizze a naplókat.
* **A yarn felhasználói felület**.

    ![HDInsight portal YARN felhasználói felületen](./media/hdinsight-administer-use-management-portal/hdinsight-yarn-ui.png)

## <a name="run-hive-queries"></a>Hive-lekérdezések futtatása
hello portálon, a Hive-feladatok tooran kattintson **Hive szerkesztő** hello HDInsight lekérdezés-konzol. Lásd: [nyissa meg a HDInsight-lekérdezés konzol](#open-hdinsight-query-console).

## <a name="monitor-jobs"></a>Feladatok figyelése
toomonitor feladatokat azok hello portál, kattintson a **Feladatelőzményekben** hello HDInsight lekérdezés-konzol. Lásd: [nyissa meg a HDInsight-lekérdezés konzol](#open-hdinsight-query-console).

## <a name="browse-files"></a>Fájlok tallózása
toobrowse fájlok hello alapértelmezett tárfiók tárolja és hello társított tárfiókokat, kattintson a **fájl böngésző** hello HDInsight lekérdezés-konzol. Lásd: [nyissa meg a HDInsight-lekérdezés konzol](#open-hdinsight-query-console).

Is használhatja a hello **hello fájlrendszer Tallózás** segédprogram a hello **Hadoop felhasználói felület** hello HDInsight-konzolon.  Lásd: [nyissa meg a HDInsight-lekérdezés konzol](#open-hdinsight-query-console).

## <a name="monitor-cluster-usage"></a>A figyelő fürt használata
Hello **használati** hello HDInsight-fürt panelén szakasza magok elérhető tooyour előfizetés a hdinsight eszközzel használható hello számát, valamint toothis fürt, és hogyan lefoglalt magok száma hello információit jeleníti meg. a fürt hello csomópontja számára lefoglalt. Lásd: [listája és megjelenítése fürtök](#list-and-show-clusters).

> [!IMPORTANT]
> hello által biztosított toomonitor hello szolgáltatások HDInsight-fürtre, használja az Ambari Web vagy hello Ambari REST API-t kell. További információ az Ambari használatával, lásd: [kezelése HDInsight-fürtök Ambari használatával](hdinsight-hadoop-manage-ambari.md)
>
>

## <a name="open-hadoop-ui"></a>Nyissa meg a Hadoop felhasználói felület
toomonitor hello fürt, Tallózás hello fájlrendszert és ellenőrizze a naplókat, kattintson az **Hadoop felhasználói felület** hello HDInsight lekérdezés-konzol. Lásd: [nyissa meg a HDInsight-lekérdezés konzol](#open-hdinsight-query-console).

## <a name="open-yarn-ui"></a>Nyissa meg a Yarn felhasználói felületen
toouse Yarn felhasználói felületen, kattintson a **Yarn felhasználói felületen** hello HDInsight lekérdezés-konzol. Lásd: [nyissa meg a HDInsight-lekérdezés konzol](#open-hdinsight-query-console).

## <a name="connect-tooclusters-using-rdp"></a>Csatlakozás RDP Funkciót használnak tooclusters
hello hitelesítő adatokat a saját létrehozásakor megadott hello fürt toohello szolgáltatások hello fürt, de nem toohello fürt távoli asztalon keresztül hozzáférést biztosít. Ha a fürt, vagy a fürt létesítése után kapcsolhatja be a távoli asztal eléréséhez. Hello távoli asztal engedélyezése a létrehozásakor vonatkozó utasításokért lásd: [hozzon létre HDInsight-fürt](hdinsight-hadoop-provision-linux-clusters.md).

**a távoli asztal tooenable**

1. Jelentkezzen be toohello [Portal][azure-portal].
2. Kattintson a **összes tallózása** hello bal oldali menüben kattintson **a HDInsight-fürtök**, kattintson a fürt nevére.
3. Kattintson a **beállítások** hello felső menüben, majd kattintson a **távoli asztal**.
4. Adja meg **lejárati dátuma**, **távoli asztali felhasználónévnek** és **távoli asztal jelszó**, és kattintson a **engedélyezése**.

    ![HDInsight engedélyezi letiltja a távoli asztal konfigurálása](./media/hdinsight-administer-use-management-portal/hdinsight.portal.remote.desktop.png)

    hello alapértelmezett értékeinek lejárati dátuma a hét.

   > [!NOTE]
   > Hello HDInsight .NET SDK tooenable távoli asztal fürtön is használható. Használjon hello **EnableRdp** hello HDInsight ügyfélobjektum hello módon a következő metódust: **ügyfél. EnableRdp (fürtnév, hely, "rdpuser", "rdppassword" DateTime.Now.AddDays(6))**. Hasonlóképpen, a távoli asztal toodisable hello fürtön, használhatja **ügyfél. (A fürtnév, a hely) DisableRdp**. E módszerekkel kapcsolatos további információkért lásd: [HDInsight .NET SDK-dokumentáció](http://go.microsoft.com/fwlink/?LinkId=529017). Ez az csak a HDInsight-fürtökön futó Windows alkalmazható.
   >
   >

**tooconnect tooa fürt RDP használatával**

1. Jelentkezzen be toohello [Portal][azure-portal].
2. Kattintson a **összes tallózása** hello bal oldali menüben kattintson **a HDInsight-fürtök**, kattintson a fürt nevére.
3. Kattintson a **beállítások** hello felső menüben, majd kattintson a **távoli asztal**.
4. Kattintson a **Connect** és hello utasítások szerint. Ha Connect tiltsa le, engedélyeznie kell azt először. Győződjön meg arról, hogy hello távoli asztali felhasználói felhasználónévvel és jelszóval.  Hello fürt felhasználó hitelesítő adatai nem használhatók.

## <a name="open-hadoop-command-line"></a>Nyissa meg a Hadoop parancssor
tooconnect toohello fürt távoli asztal és a használata hello Hadoop parancssor használatával kell először engedélyezte a távoli asztal hozzáférés toohello fürt hello előző szakaszban leírtak szerint.

**a Hadoop parancssor tooopen**

1. Csatlakoztassa a távoli asztal toohello fürtöt.
2. Kattintson duplán a hello asztalról **Hadoop parancssori**.

    ![HDI. HadoopCommandLine][image-hadoopcommandline]

    További információk a Hadoop-parancsok: [Hadoop-parancsok hivatkozás](http://hadoop.apache.org/docs/current/hadoop-project-dist/hadoop-common/CommandsManual.html).

Az előző képernyőképet hello hello mappanév verziószáma hello Hadoop beágyazott. hello verziószám hello Hadoop-összetevők telepítve hello fürt módosított alapján hello verziója is. Hadoop környezeti változók toorefer toothose mappákat használhatja. Példa:

    cd %hadoop_home%
    cd %hive_home%
    cd %hbase_home%
    cd %pig_home%
    cd %sqoop_home%
    cd %hcatalog_home%

## <a name="next-steps"></a>Következő lépések
Ebben a cikkben megtanulhatta, hogyan toocreate a HDInsight-fürtök hello portálon, és hogyan tooopen hello Hadoop parancssori eszközt. toolearn több, tekintse meg a következő cikkek hello:

* [Felügyelheti a HDInsight az Azure PowerShell használatával](hdinsight-administer-use-powershell.md)
* [Felügyelheti a HDInsight az Azure parancssori felület használatával](hdinsight-administer-use-command-line.md)
* [A HDInsight-fürtök létrehozása](hdinsight-hadoop-provision-linux-clusters.md)
* [Hadoop-feladatokat programozott módon küldhet](hdinsight-submit-hadoop-jobs-programmatically.md)
* [Ismerkedés az Azure HDInsight](hdinsight-hadoop-linux-tutorial-get-started.md)
* [Azure HDInsight Hadoop verziójának van?](hdinsight-component-versioning.md)

[azure-portal]: https://portal.azure.com
[image-hadoopcommandline]: ./media/hdinsight-administer-use-management-portal/hdinsight-hadoop-command-line.png "Hadoop parancssor"
