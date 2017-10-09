---
title: "HDInsight Linux-alapú fürtökön - Azure hadooppal aaaHue |} Microsoft Docs"
description: "Megtudhatja, hogyan tooinstall Hue a HDInsight-fürtök és bújtatási tooroute hello kérelmek tooHue használja. Hue toobrowse tárhelyet használja, és futtassa a Hive vagy Pig."
keywords: hue hadoop
services: hdinsight
documentationcenter: 
author: nitinme
manager: jhubbard
editor: cgronlun
ms.assetid: 9e57fcca-e26c-479d-a745-7b80a9290447
ms.service: hdinsight
ms.custom: hdinsightactive,hdiseo17may2017
ms.workload: big-data
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/10/2017
ms.author: nitinme
ms.openlocfilehash: f086cbad2a90cc6903fbfccbf4a6be44f8999d07
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="install-and-use-hue-on-hdinsight-hadoop-clusters"></a>Telepítheti és használhatja Hue HDInsight Hadoop-fürtök

Megtudhatja, hogyan tooinstall Hue a HDInsight-fürtök és bújtatási tooroute hello kérelmek tooHue használja.

> [!IMPORTANT]
> hello jelen dokumentumban leírt lépések egy HDInsight-fürt által használt Linux igényelnek. Linux hello azt az egyetlen operációs rendszer, használja a HDInsight 3.4 vagy újabb verziója. További tudnivalókért lásd: [A HDInsight elavulása Windows rendszeren](hdinsight-component-versioning.md#hdinsight-windows-retirement).

## <a name="what-is-hue"></a>Mi az a Hue?
Hue, a webalkalmazások a használt toointeract egy Hadoop-fürthöz. Hue toobrowse hello storage (WASB, hello esetében a HDInsight-fürtök) Hadoop-fürthöz társított használja, futtassa a Hive-feladatok és a Pig-parancsfájlok, és így tovább. hello következő összetevőket is használhatók egy HDInsight Hadoop-fürt Hue való telepítéséhez.

* Méhviasz Hive szerkesztő
* Pig
* Metaadattárhoz manager
* Oozie
* FileBrowser (amely kiszolgálóhoz tooWASB alapértelmezett tároló)
* Feladat böngésző

> [!WARNING]
> Hello HDInsight-fürt összetevői teljes mértékben támogatottak, és a Microsoft Support fog tooisolate segítségével, és a kapcsolódó toothese összetevők problémák megoldásához.
>
> Egyéni összetevők kap minden üzleti szempontból ésszerű támogatási toohelp meg toofurther hello problémával kapcsolatos hibaelhárítás elősegítéséhez. Hello probléma megoldását, vagy kérni a tooengage elérhető csatorna az hello megnyitja részletes segítséget, hogy a technológiát találhatók technológiák eredményezhet. Például nincsenek sok közösségi webhelyek használható, például: [MSDN fórum hdinsight](https://social.msdn.microsoft.com/Forums/azure/en-US/home?forum=hdinsight), [http://stackoverflow.com](http://stackoverflow.com). Is Apache projektek rendelkezik projekt helyek [http://apache.org](http://apache.org), például: [Hadoop](http://hadoop.apache.org/).
>
>

## <a name="install-hue-using-script-actions"></a>A Parancsfájlműveletek segítségével Hue telepítése

hello parancsfájl tooinstall egy Linux-alapú HDInsight-fürt Hue megtalálható https://hdiconfigactions.blob.core.windows.net/linuxhueconfigactionv02/install-hue-uber-v02.sh. Alapértelmezett tárolóként használható a parancsfájl tooinstall Hue vagy az Azure Storage Blobs (WASB), vagy az Azure Data Lake Store-fürtökön.

Ez a szakasz bemutatja, hogyan toouse hello-e parancsfájl hello fürt hello Azure-portál használatával kiépítésekor.

> [!NOTE]
> Az Azure PowerShell, a hello Azure parancssori felület, a hello HDInsight .NET SDK vagy az Azure Resource Manager-sablonok is használt tooapply Parancsfájlműveletek. A futó fürtök parancsfájl műveletek tooalready is alkalmazhat. További információkért lásd: [testreszabása HDInsight-fürtök parancsfájlműveletekkel](hdinsight-hadoop-customize-cluster-linux.md).
>
>

1. Fürt elkezdhessen a hello lépések segítségével [Provision HDInsight-fürtök Linux rendszeren](hdinsight-hadoop-provision-linux-clusters.md), de kiépítése nem fejeződött be.

   > [!NOTE]
   > tooinstall Hue a HDInsight-fürtök esetén hello ajánlott headnode mérete legalább A4 (8 mag, 14 GB memória).
   >
   >
2. A hello **opcionális konfigurációs** panelen válassza **Parancsfájlműveletek**, és adja meg a hello információ alább látható módon:

    ![Adja meg parancsfájl művelet paramétereinek Hue](./media/hdinsight-hadoop-hue-linux/hue-script-action.png "nyújtása parancsfájl művelet paramétereinek Hue")

   * **NÉV**: Adjon meg egy rövid nevet hello parancsfájlművelet.
   * **PARANCSFÁJL URI azonosítója**: https://hdiconfigactions.blob.core.windows.net/linuxhueconfigactionv02/install-hue-uber-v02.sh
   * **HEAD**: ezt a beállítást
   * **MUNKAVÉGZŐ**: hagyja üresen a mezőt.
   * **ZOOKEEPER**: hagyja üresen a mezőt.
   * **PARAMÉTEREK**: hagyja üresen a mezőt.
3. Hello hello alján **Parancsfájlműveletek**, hello használata **válasszon** gombok toosave hello beállítása. Végezetül a hello használata **kiválasztása** hello hello alján gomb **opcionális konfigurációs** panel toosave hello opcionális konfigurációs információkat.
4. Kiépítés hello fürt, a folytatáshoz [Provision HDInsight-fürtök Linux rendszeren](hdinsight-hadoop-provision-linux-clusters.md).

## <a name="use-hue-with-hdinsight-clusters"></a>Hue használata a HDInsight-fürtök

SSH-bújtatás hello csak úgy tooaccess Hue hello fürtön, miután fut-e. SSH-kapcsolaton keresztül Tunneling lehetővé teszi, hogy hello forgalom toogo közvetlen toohello headnode hello a fürt Hue futtató. Követően hello fürt befejezte a kiépítést, használja a következő lépéseket toouse Hue HDInsight Linux fürtökön hello.

> [!NOTE]
> A Firefox webes böngésző toofollow hello az alábbi utasítások használatát javasoljuk.
>
>

1. A hello információk [használata SSH Tunneling tooaccess Ambari web UI, ResourceManager, JobHistory, NameNode, Oozie és egyéb webes felhasználói felületén](hdinsight-linux-ambari-ssh-tunnel.md) toocreate SSH alagúteszköz-a rendszer toohello ügyfél HDInsight-fürtjéhez, és adja meg a webes böngésző toouse hello SSH-alagút proxy-ként.

2. Ha rendelkezik az SSH-alagút létrehozása és konfigurálása a böngésző tooproxy forgalom rajta, keresse meg hello elsődleges átjárócsomópont hello állomásnevét. Ehhez csatlakozás toohello fürt SSH 22-es port használatával. Például `ssh USERNAME@CLUSTERNAME-ssh.azurehdinsight.net` ahol **felhasználónév** az SSH-felhasználónév és **CLUSTERNAME** hello a fürt neve van.

    További információ: [Az SSH használata HDInsighttal](hdinsight-hadoop-linux-use-ssh-unix.md).

3. Miután csatlakozott, használja a következő parancs tooobtain hello teljesen minősített tartománynevét hello elsődleges headnode hello:

        hostname -f

    Ezzel visszatér neve hasonló toohello következő:

        hn0-myhdi-nfebtpfdv1nubcidphpap2eq2b.ex.internal.cloudapp.net

    Ez az elsődleges headnode hello hello állomásnevét ahol hello Hue webhelyen-e.
4. Http://HOSTNAME:8888 hello böngésző tooopen hello Hue portál használatát. Cserélje le az ÁLLOMÁSNÉV hello előző lépésben beolvasott hello neve.

   > [!NOTE]
   > Jelentkezik be a hello az első alkalommal, amikor egy fiók toolog toohello Hue portál felszólító toocreate fogja. az itt megadott hello hitelesítő adatok korlátozott toohello portal lesz, és nincsenek kapcsolódó toohello rendszergazda vagy a kiépítés hello fürt a megadott SSH hitelesítő.
   >
   >

    ![Bejelentkezési toohello Hue portál](./media/hdinsight-hadoop-hue-linux/hdinsight-hue-portal-login.png "Hue portál hitelesítő adatok megadása")

### <a name="run-a-hive-query"></a>Hive-lekérdezések futtatása
1. A hello Hue portál, kattintson az **lekérdezés-szerkesztő**, és kattintson a **Hive** tooopen hello Hive szerkesztőben.

    ![A Hive használata](./media/hdinsight-hadoop-hue-linux/hdinsight-hue-portal-use-hive.png "a Hive használata")
2. A hello **Assist** lap **adatbázis**, megtekintheti az **hivesampletable**. Ez a modul a HDInsight Hadoop-fürtök a minta tábla. Adjon meg egy minta lekérdezést hello jobb oldali ablaktáblán, és hello kimenet jelenik meg a hello **eredmények** lapján hello alább látható módon hello képernyőfelvétel-készítés.

    ![Hive-lekérdezések futtatása](./media/hdinsight-hadoop-hue-linux/hdinsight-hue-portal-hive-query.png "futtatása Hive-lekérdezések")

    Is használhatja a hello **diagram** toosee hello eredmény vizuális ábrázolását fülre.

### <a name="browse-hello-cluster-storage"></a>Keresse meg a hello fürttároló
1. A hello Hue portál, kattintson az **fájl böngésző** hello menüsáv hello jobb felső sarkában.
2. Alapértelmezés szerint a hello fájl böngésző megnyitja a hello **/felhasználó/myuser** könyvtár. Kattintson a hello törtvonallal közvetlenül hello felhasználói directory hello elérési toogo toohello legfelső szintű hello az Azure storage-tároló hello-fürthöz tartozó előtt.

    ![Használjon fájl böngésző](./media/hdinsight-hadoop-hue-linux/hdinsight-hue-portal-file-browser.png "fájl böngésző használata")
3. Kattintson a jobb gombbal egy fájlra vagy mappára toosee hello elérhető műveletek. Használjon hello **feltöltése** gomb hello jobb sarkában található tooupload fájlok toohello aktuális könyvtárban található. Használjon hello **új** toocreate új fájlok vagy könyvtárak gombra.

> [!NOTE]
> Hue-fájl böngésző hello csak is hello alapértelmezett tároló hello HDInsight-fürthöz társított hello tartalmának megjelenítése. Bármely további fiókok/tárolókban, előfordulhat, hogy vannak társítva hello fürt nem lesz elérhető hello fájl böngésző használatával. Hello további tárolókat hello-fürthöz tartozó azonban mindig lesz hello Hive-feladatok számára. Például, ha hello parancs beírásakor `dfs -ls wasb://newcontainer@mystore.blob.core.windows.net` hello Hive szerkesztőben, lásd: hello tartalmát további tárolókat is beleértve. A parancs **newcontainer** nincs a fürthöz tartozó hello alapértelmezett tárolót.
>
>

## <a name="important-considerations"></a>Fontos tudnivalók találhatók
1. hello használt parancsfájl tooinstall Hue telepíti a szoftvert csak elsődleges headnode hello hello fürt.

2. A telepítés során több Hadoop-szolgáltatás (HDFS, YARN, MR2, Oozie) újraindul a hello konfigurációjának frissítése. Hue telepítése hello parancsfájl befejezése után eltarthat némi időbe, más Hadoop szolgáltatások toostart. A Hue teljesítmény kezdetben hatással lehet. Miután az összes szolgáltatás elindításához, Hue teljesen működőképes lesz.
3. Hue nem értelmezi Tez feladatokhoz hello aktuális alapértelmezett a Hive. Ha toouse MapReduce, a Hive-végrehajtó motor hello, frissítse a hello parancsfájl toouse hello a következő parancsot a parancsfájlban:

        set hive.execution.engine=mr;

4. Linux-fürtökkel akkor is egy olyan forgatókönyvet, ahol a szolgáltatások futnak hello elsődleges headnode hello erőforrás-kezelő sikertelen lehet a másodlagos hello futása közben. Ilyen esetben hello fürtön FUTÓ feladatok Hue tooview részleteit használatakor (lásd alább) hibákat eredményezhet. Azonban hello feladat részleteinek megtekintése, hello feladat befejezése.

   ![Hue portál hiba](./media/hdinsight-hadoop-hue-linux/hdinsight-hue-portal-error.png "Hue portál hiba")

   Ez az esedékes tooa ismert probléma. A probléma megoldásához módosítsa Ambari, hogy a hello aktív erőforrás-kezelő is fut hello elsődleges headnode.
5. Hue együttműködik a WebHDFS, amíg a HDInsight-fürtök használata az Azure Storage használatával `wasb://`. Igen hello parancsfájlművelet használt egyéni parancsfájl telepíti WebWasb, amely a WebHDFS-kompatibilis szolgáltatás tooWASB van szó. Igen, annak ellenére, hogy hello Hue portál szerint helyen HDFS (például amikor az egér átvitele hello **fájl böngésző**), úgy kell értelmezni, WASB.

## <a name="next-steps"></a>Következő lépések
* [Giraph telepíthető a HDInsight-fürtök](hdinsight-hadoop-giraph-install-linux.md). Fürt testreszabási tooinstall Giraph a HDInsight Hadoop-fürtök használata. Giraph tooperform Hadoop használatával diagramfeldolgozás lehetővé teszi, és az Azure HDInsight használható.
* [Solr telepíthető a HDInsight-fürtök](hdinsight-hadoop-solr-install-linux.md). Fürt testreszabási tooinstall Solr a HDInsight Hadoop-fürtök használata. Solr lehetővé teszi tooperform hatékony keresési műveletek tárolt adatokat.
* [A HDInsight-fürtökön R telepítéséhez](hdinsight-hadoop-r-scripts-linux.md). HDInsight Hadoop-fürtök fürt testreszabási tooinstall R használja. R egy nyílt forráskódú nyelv és környezet, amely statisztikai számításokhoz. Több száz beépített statisztikai függvények és a saját programozási nyelv, amely a működési és objektumorientált programozási aspektusát kombinálja biztosít. Nagy mennyiségű grafikus képességeket is tartalmaz.

[powershell-install-configure]: install-configure-powershell-linux.md
[hdinsight-provision]: hdinsight-provision-clusters-linux.md
[hdinsight-cluster-customize]: hdinsight-hadoop-customize-cluster-linux.md
