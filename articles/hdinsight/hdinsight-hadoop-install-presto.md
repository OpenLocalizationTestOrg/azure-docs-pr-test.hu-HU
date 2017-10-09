---
title: aaaInstall Presto Azure HDInsight Linux clusters |} Microsoft Docs
description: "Ismerje meg, hogyan tooinstall Presto és Airpal a Linux-alapú HDInsight Hadoop-fürtök Parancsfájlműveletek használatával."
services: hdinsight
documentationcenter: 
author: nitinme
manager: jhubbard
editor: cgronlun
ms.service: hdinsight
ms.custom: hdinsightactive
ms.workload: big-data
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/17/2017
ms.author: nitinme
ms.openlocfilehash: 5d54d0efc3e5fdc6f5a8d3a94ad2f61d16df24c2
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="install-and-use-presto-on-hdinsight-hadoop-clusters"></a>Telepítheti és használhatja Presto HDInsight Hadoop-fürtök

Ebben a témakörben elsajátíthatja, hogyan tooinstall Presto a HDInsight Hadoop-fürtök parancsfájlművelet használatával. Azt is megtudhatja, hogyan tooinstall Airpal egy meglévő Presto HDInsight-fürtre.

> [!IMPORTANT]
> hello jelen dokumentumban leírt lépések szükséges egy **HDInsight 3.5 Hadoop-fürt** , amely Linux használ. Linux hello azt az egyetlen operációs rendszer, használja a HDInsight 3.4 vagy újabb verziója. További információkért lásd: [HDInsight-verziókról](hdinsight-component-versioning.md).

## <a name="what-is-presto"></a>Mi az az Presto?
[Presto](https://prestodb.io/overview.html) egy gyors elosztott SQL lekérdezési motor a big data. Presto alkalmas adatmennyiségig interaktív lekérdezése. Presto, és hogyan működnek együtt hello összetevők további információkért lásd: [Presto fogalmak](https://github.com/prestodb/presto/blob/master/presto-docs/src/main/sphinx/overview/concepts.rst).

> [!WARNING]
> Hello HDInsight-fürt összetevői teljes mértékben támogatottak, és a Microsoft Support fog tooisolate segítségével, és a kapcsolódó toothese összetevők problémák megoldásához.
> 
> Egyéni összetevők, például Presto, minden üzleti szempontból ésszerű támogatási toohelp fogadni, toofurther hello problémával kapcsolatos hibaelhárítás elősegítéséhez. Hello probléma megoldását, vagy kérni a tooengage elérhető csatorna az hello megnyitja részletes segítséget, hogy a technológiát találhatók technológiák eredményezhet. Például nincsenek sok közösségi webhelyek használható, például: [MSDN fórum hdinsight](https://social.msdn.microsoft.com/Forums/azure/home?forum=hdinsight), [http://stackoverflow.com](http://stackoverflow.com). Is Apache projektek rendelkezik projekt helyek [http://apache.org](http://apache.org), például: [Hadoop](http://hadoop.apache.org/).
> 
> 


## <a name="install-presto-using-script-action"></a>Parancsfájl műveletével Presto telepítése

Ez a szakasz útmutatás hogyan toouse hello parancsfájlpélda használatával új fürt létrehozásakor hello Azure-portálon. 

1. Fürt elkezdhessen a hello lépések segítségével [Provision Linux-alapú HDInsight-fürtök](hdinsight-hadoop-create-linux-clusters-portal.md). Ellenőrizze, hogy hello segítségével hello fürtöt hoz létre **egyéni** fürt létrehozási folyamata. Gondoskodnia kell arról, létrehozhat hello fürthöz megfelel-e hello követelményeknek.

    a. A 3.5-ös verziója HDInsight Hadoop-fürttel kell lennie.

    b. Azure Storage azt kell használnia, mint hello adattár. Presto használatával olyan fürtön, használó Azure Data Lake Store hello tárolási lehetőség még nem támogatott. 

    ![Egyéni beállítások használata a HDInsight-fürt létrehozása](./media/hdinsight-hadoop-install-presto/hdinsight-install-custom.png)

2. A hello **speciális beállítások** panelen válassza **Parancsfájlműveletek**, és adja meg az alábbi részleteket hello:
   
   * **NÉV**: Adjon meg egy rövid nevet hello parancsfájlművelet.
   * **Bash-szkript URI azonosítója**: `https://raw.githubusercontent.com/hdinsight/presto-hdinsight/master/installpresto.sh`
   * **HEAD**: ezt a beállítást
   * **MUNKAVÉGZŐ**: ezt a beállítást
   * **ZOOKEEPER**: törölje a jelet a jelölőnégyzetből
   * **PARAMÉTEREK**: ezt a mezőt hagyja üresen


3. Hello hello alján **Parancsfájlműveletek** panelen hello kattintson **válasszon** gombok toosave hello beállítása. Végül kattintson a hello **válassza** hello hello alján gomb **speciális beállítások** panel toosave hello konfigurációs adatait.

4. Kiépítés hello fürt, a folytatáshoz [Provision Linux-alapú HDInsight-fürtök](hdinsight-hadoop-create-linux-clusters-portal.md).

    > [!NOTE]
    > Az Azure PowerShell, a hello Azure parancssori felület, a hello HDInsight .NET SDK vagy az Azure Resource Manager-sablonok is használt tooapply Parancsfájlműveletek. A futó fürtök parancsfájl műveletek tooalready is alkalmazhat. További információkért lásd: [testreszabása HDInsight-fürtök parancsfájlműveletekkel](hdinsight-hadoop-customize-cluster-linux.md).
    > 
    > 

## <a name="use-presto-with-hdinsight"></a>Presto használata a hdinsight eszközzel

Hajtsa végre a fent ismertetett lépéseket hello segítségével telepítése után a következő lépéseket toouse Presto a HDInsight-fürtök hello.

1. Csatlakozzon az SSH használatával toohello HDInsight-fürt:
   
        ssh USERNAME@CLUSTERNAME-ssh.azurehdinsight.net
   
    További információ: [Az SSH használata HDInsighttal](hdinsight-hadoop-linux-use-ssh-unix.md).
     

2. Indítsa el a hello Presto rendszerhéj hello a következő parancs használatával.
   
        presto --schema default

3. Lekérdezés futtatható egy mintatáblát **hivesampletable**, alapértelmezés szerint az összes HDInsight-fürtök elérhető.
   
        select count (*) from hivesampletable;
   
    Alapértelmezés szerint [Hive](https://prestodb.io/docs/current/connector/hive.html) és [TPCH](https://prestodb.io/docs/current/connector/tpch.html) csatlakozók a Presto már be van állítva. Hive összekötő konfigurált toouse hello alapértelmezés szerint telepített Hive telepítési, így a Hive táblák összes hello Presto automatikusan látható lesz.

    A Presto használatát egy részletes ismertetését lásd: [Presto dokumentáció](https://prestodb.io/docs/current/index.html).

## <a name="use-airpal-with-presto"></a>Presto Airpal használata

[Airpal](https://github.com/airbnb/airpal#airpal) Presto van egy nyílt forráskódú webes lekérdezési felületet. A Airpal további információkért lásd: [Airpal dokumentáció](https://github.com/airbnb/airpal#airpal).

Ebben a szakaszban úgy tekintünk hello lépéseket túl**Airpal telepíthető hello edgenode** egy HDInsight Hadoop-fürt, amelyen már megtalálható a Presto telepítve. Ez biztosítja, hogy hello Airpal webes lekérdezés illesztő elérhető hello interneten keresztül.

1. SSH használatával csatlakozzon a toohello headnode Presto telepített hello HDInsight-fürt:
   
        ssh USERNAME@CLUSTERNAME-ssh.azurehdinsight.net
   
    További információ: [Az SSH használata HDInsighttal](hdinsight-hadoop-linux-use-ssh-unix.md).

2. Miután csatlakozott, futtassa a következő parancs hello.

        sudo slider registry  --name presto1 --getexp presto 
   
    Hello hasonló kimenetnek kell megjelennie:

        {
            "coordinator_address" : [ {
                "value" : "10.0.0.12:9090",
                "level" : "application",
                "updatedTime" : "Mon Apr 03 20:13:41 UTC 2017"
        } ]

3. A hello kimenet, vegye figyelembe a hello hello értéke **érték** tulajdonság. Szüksége lesz a hello fürt edgenode Airpal telepítése közben. Hello kimenetéről fent hello érték, amelyre szüksége lesz az **10.0.0.12:9090**.

4. Hello sablon használata  **[Itt](https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2Fhdinsight%2Fpresto-hdinsight%2Fmaster%2Fairpal-deploy.json)**  toocreate egy HDInsight fürt edgenode, és adjon meg hello értékeket, ahogy az alábbi képernyőfelvétel a hello.

    ![HDInsight telepítés Airpal Presto fürtön](./media/hdinsight-hadoop-install-presto/hdinsight-install-airpal.png)

5. Kattintson a **Purchase** (Vásárlás) gombra.

6. Hello módosítások alkalmazott toohello fürtkonfiguráció, után a hello Airpal webes felület hello lépések használatával végezheti el.

    a. Hello-fürt panelén kattintson **alkalmazások**.

    ![HDInsight indítási Airpal Presto fürtön](./media/hdinsight-hadoop-install-presto/hdinsight-presto-launch-airpal.png)

    b. A hello **telepített alkalmazások** panelen kattintson a **Portal** airpal ellen.

    ![HDInsight indítási Airpal Presto fürtön](./media/hdinsight-hadoop-install-presto/hdinsight-presto-launch-airpal-1.png)

    c. Amikor a rendszer kéri, adja meg a hello rendszergazdai hitelesítő adataival megadott hello HDInsight Hadoop-fürt létrehozása során.

## <a name="customize-a-presto-installation-on-hdinsight-cluster"></a>A HDInsight-fürt Presto telepítés testreszabása

Presto egy HDInsight Hadoop-fürt telepítése után hello telepítési toomake módosítások – például a frissítés memóriabeállításait testreszabása, módosítsa az összekötők stb. Hajtsa végre a következő lépéseket toodo így hello.

1. SSH használatával csatlakozzon a toohello headnode Presto telepített hello HDInsight-fürt:
   
        ssh USERNAME@CLUSTERNAME-ssh.azurehdinsight.net
   
    További információ: [Az SSH használata HDInsighttal](hdinsight-hadoop-linux-use-ssh-unix.md).

2. A konfigurációs módosításokat hello fájlban `/var/lib/presto/presto-hdinsight-master/appConfig-default.json`. Presto konfiguráció további információkért lásd: [YARN-alapú fürtök Presto konfigurációs](https://prestodb.io/presto-yarn/installation-yarn-configuration-options.html).

3. Állítsa le és hello aktuális futó példányát Presto kill.

        sudo slider stop presto1 --force
        sudo slider destroy presto1 --force

4. Egy új példányát Presto kezdődnie hello testreszabása.

       sudo slider create presto1 --template /var/lib/presto/presto-hdinsight-master/appConfig-default.json --resources /var/lib/presto/presto-hdinsight-master/resources-default.json

5. Várjon, amíg hello új példány toobe készen áll, és jegyezze fel a presto koordinátorának címe.


       sudo slider registry --name presto1 --getexp presto

## <a name="generate-benchmark-data-for-hdinsight-clusters-that-run-presto"></a>Teljesítményteszt adatok Presto futtató HDInsight-fürtök létrehozása

TPC-DS hello iparági szabvány sok döntési támogatási rendszerek, beleértve a big data-rendszereket hello teljesítményének méréséhez. HDInsight-fürtök toogenerate adatok Presto használatát, és értékelje ki, hogyan összehasonlítja saját HDInsight teljesítményteszt adatokkal. További információkért lásd: [Itt](https://github.com/hdinsight/tpcds-datagen-as-hive-query/blob/master/README.md).



## <a name="see-also"></a>Lásd még:
* [Telepítheti és használhatja a HDInsight-fürtök Hue](hdinsight-hadoop-hue-linux.md). Hue webes felhasználói felületén, így könnyen toocreate, futtassa az és mentse a Pig és Hive-feladatok, is, keresse meg hello alapértelmezett storage a HDInsight-fürthöz.

* [Giraph telepíthető a HDInsight-fürtök](hdinsight-hadoop-giraph-install-linux.md). Fürt testreszabási tooinstall Giraph a HDInsight Hadoop-fürtök használata. Giraph lehetővé teszi a Hadoop használatával tooperform diagramfeldolgozás, és az Azure HDInsight használható.

* [Solr telepíthető a HDInsight-fürtök](hdinsight-hadoop-solr-install-linux.md). Fürt testreszabási tooinstall Solr a HDInsight Hadoop-fürtök használata. Solr lehetővé teszi tooperform hatékony keresési műveletek tárolt adatokat.

[hdinsight-install-r]: hdinsight-hadoop-r-scripts-linux.md
[hdinsight-cluster-customize]: hdinsight-hadoop-customize-cluster-linux.md
