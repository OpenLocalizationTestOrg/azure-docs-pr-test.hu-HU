---
title: "Jupyter aaaInstall helyileg & Csatlakozás az Azure HDInsight Spark-fürt tooan |} Microsoft Docs"
description: "Megtudhatja, hogyan tooinstall Jupyter notebook helyileg a számítógépen, és csatlakoztassa a Azure HDInsight tooan Apache Spark-fürt."
services: hdinsight
documentationcenter: 
author: nitinme
manager: jhubbard
editor: cgronlun
tags: azure-portal
ms.assetid: 48593bdf-4122-4f2e-a8ec-fdc009e47c16
ms.service: hdinsight
ms.custom: hdinsightactive
ms.workload: big-data
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/10/2017
ms.author: nitinme
ms.openlocfilehash: 95c052110b84b677fd23048597af9511365cacfc
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="install-jupyter-notebook-on-your-computer-and-connect-tooapache-spark-on-hdinsight"></a>Jupyter notebook telepítse a számítógépre, és csatlakozzon a Spark on HDInsight tooApache

A cikkben megismerheti, hogyan tooinstall Jupyter notebook hello az egyéni PySpark (a Python) és (a Scala) Spark mag az Spark magic, és csatlakozzon a hello notebook tooan HDInsight-fürthöz. Számos okból tooinstall Jupyter a helyi számítógépen is lehet, és némi kihívást is lehet. Bővebben lásd: hello szakasz [Miért célszerű telepíteni Jupyter a számítógépemen](#why-should-i-install-jupyter-on-my-computer) hello Ez a cikk végén.

Három fő lépésből áll részt Jupyter és hello Spark magic telepíti a számítógépre.

* Jupyter notebook telepítése
* A Spark magic hello hello PySpark és a Spark mag telepítése
* A HDInsight Spark magic tooaccess Spark-fürt konfigurálása

Hello egyéni kernelek és hello Spark magic HDInsight-fürthöz használt Jupyter notebookokban elérhető kapcsolatos további információkért lásd: [Apache Spark Linux és a Jupyter notebookok elérhető kernelek a HDInsight-fürtök](hdinsight-apache-spark-jupyter-notebook-kernels.md).

## <a name="prerequisites"></a>Előfeltételek
az itt felsorolt hello Előfeltételek nincsenek Jupyter telepítéséhez. Ezek a kapcsolódó hello Jupyter notebook tooan HDInsight-fürthöz, hello notebook telepítése után.

* Azure-előfizetés. Lásd: [Ingyenes Azure-fiók létrehozása](https://azure.microsoft.com/documentation/videos/get-azure-free-trial-for-testing-hadoop-in-hdinsight/).
* A HDInsight az Apache Spark-fürt. Útmutatásért lásd: [létrehozása az Apache Spark on Azure hdinsight clusters](hdinsight-apache-spark-jupyter-spark-sql.md).

## <a name="install-jupyter-notebook-on-your-computer"></a>Jupyter notebook telepítse a számítógépre

Jupyter notebookok telepítése előtt telepítenie kell az Python. Érhetők el a Python és a Jupyter hello részeként [Anaconda terjesztési](https://www.continuum.io/downloads). Ha Anaconda telepíti, a Python egy terjesztési telepítése. Anaconda telepítése után hozzáadhat hello Jupyter telepítési megfelelő parancsok futtatásával.

1. Töltse le a hello [Anaconda telepítő](https://www.continuum.io/downloads) a platform és futtatási hello beállítása. Közben futó hello beállítása varázsló gondoskodjon róla, hogy hello beállítás tooadd Anaconda tooyour ELÉRÉSIÚT-változónak.
2. Futtassa a következő parancs tooinstall Jupyter hello.

        conda install jupyter

    Jupyter telepítésével kapcsolatos további információkért lásd: [telepítése Jupyter használatával Anaconda](http://jupyter.readthedocs.io/en/latest/install.html).

## <a name="install-hello-kernels-and-spark-magic"></a>Hello kernelek és Spark magic telepítése

Hogyan tooinstall hello Spark magic, hello PySpark és Spark mag, hello telepítési utasításokat követve hello kapcsolatos utasításokat [sparkmagic dokumentáció](https://github.com/jupyter-incubator/sparkmagic#installation) a Githubon. hello hello Spark magic dokumentáció első lépéseként kéri tooinstall Spark magic. Cserélje le, hogy első lépéseként hello hivatkozás hello a következő parancsokat, attól függően, hogy hello verziója hello HDInsight-fürthöz fog csatlakozni. Ezután kövesse a hátralévő lépéseket hello Spark magic dokumentációjának hello. Ha azt szeretné, hogy tooinstall hello különböző kernelek, 3. lépés a Spark magic telepítési utasításokat szakasz hello kell elvégeznie.

* A fürtök v3.4 telepítenie sparkmagic 0.2.3`pip install sparkmagic==0.2.3`

* A fürtök v3.5 és v3.6 telepítse a következő futtatásával sparkmagic 0.11.2`pip install sparkmagic==0.11.2`

## <a name="configure-spark-magic-tooconnect-toohdinsight-spark-cluster"></a>Spark magic tooconnect tooHDInsight Spark-fürt konfigurálása

Ez a szakasz korábbi tooconnect tooan Apache Spark-fürt, amely kell már létrehozta Azure hdinsightban telepített hello Spark magic konfigurálja.

1. hello Jupyter konfigurációs adatokat általában hello felhasználók kezdőkönyvtár van tárolva. toolocate a kezdőkönyvtár bármely platformon az operációs rendszer típusa hello a következő parancsokat.

    Indítsa el a hello Python rendszerhéj. Meg egy parancsablakot írja be a hello következő:

        python

    Hello Python rendszerhéj írja be a következő parancs toofind kimenő hello kezdőkönyvtár hello.

        import os
        print(os.path.expanduser('~'))

2. Nyissa meg a kezdőkönyvtár toohello, és hozzon létre egy nevű **.sparkmagic** Ha még nem létezik.
3. Hello mappában található nevű fájl létrehozása **config.json** , és adja hozzá a következő JSON részlet, azon belül hello.

        {
          "kernel_python_credentials" : {
            "username": "{USERNAME}",
            "base64_password": "{BASE64ENCODEDPASSWORD}",
            "url": "https://{CLUSTERDNSNAME}.azurehdinsight.net/livy"
          },
          "kernel_scala_credentials" : {
            "username": "{USERNAME}",
            "base64_password": "{BASE64ENCODEDPASSWORD}",
            "url": "https://{CLUSTERDNSNAME}.azurehdinsight.net/livy"
          }
        }

4. Helyettesítő **{USERNAME}**, **{CLUSTERDNSNAME}**, és **{BASE64ENCODEDPASSWORD}** megfelelő értékekkel. A kedvenc programozási nyelv vagy az online toogenerate a base64 kódolású jelszó segédprogramok számos használhatja a tényleges jelszót.

5. Hello jobb szívverés beállításainak konfigurálása `config.json`. Adja hozzá ezeket a beállításokat, mint hello azonos szinten hello `kernel_python_credentials` és `kernel_scala_credentials` kódtöredékek a hozzáadott korábban. Például hogyan és hol tooadd hello szívverés-beállítások a megjelenik ez [minta config.json](https://github.com/jupyter-incubator/sparkmagic/blob/master/sparkmagic/example_config.json).

    * A `sparkmagic 0.2.3` (v3.4 fürtöket) tartalmazza:

            "should_heartbeat": true,
            "heartbeat_refresh_seconds": 5,
            "heartbeat_retry_seconds": 1

    * A `sparkmagic 0.11.2` (fürtök v3.5 és v3.6), a következők:

            "heartbeat_refresh_seconds": 5,
            "livy_server_heartbeat_timeout_seconds": 60,
            "heartbeat_retry_seconds": 1

    >[!TIP]
    >Munkamenetek nem szivárognak tooensure szívveréseket küld. Ha egy számítógép toosleep vagy le van állítva, hello szívverés nem küldi el, a tisztítani eredményezve hello munkamenet alatt. A fürtök v3.4, ha ezt a viselkedést kívánja toodisable beállíthatja hello Livy config `livy.server.interactive.heartbeat.timeout` túl`0` a hello Ambari felhasználói felület. A fürtök v3.5 Ha nincs megadva a fenti hello 3.5 konfigurációs hello munkamenet nem törlődik.

6. Indítsa el a Jupyter. Parancs hello parancssorból a következő hello használata.

        jupyter notebook

7. Győződjön meg arról, hogy csatlakozhasson hello Jupyter notebook és, hogy használható hello Spark magic elérhető kernelek hello toohello fürtöt. Hajtsa végre a következő lépéseket hello.

    a. Hozzon létre új notebookot. Kattintson a jobb oldali sarokban hello, **új**. Megtekintheti az hello alapértelmezett kernel **Python2** és két új kernelek, a telepítés hello **PySpark** és **Spark**. Kattintson a **PySpark**.

    ![A Jupyter notebook kernelek](./media/hdinsight-apache-spark-jupyter-notebook-install-locally/jupyter-kernels.png "kernelek a Jupyter notebook")

    b. Futtassa a következő kódrészletet hello.

        %%sql
        SELECT * FROM hivesampletable LIMIT 5

    Hello kimeneti sikeresen le, ha a kapcsolat toohello HDInsight-fürthöz lett tesztelve.

    >[!TIP]
    >Ha azt szeretné, hogy tooupdate hello notebook konfigurációs tooconnect tooa másik fürtre, frissítse hello config.json hello új értékhalmazt, ahogy az a fenti a 3. lépés.

## <a name="why-should-i-install-jupyter-on-my-computer"></a>Miért kell telepíteni a számítógépre Jupyter?
Egy szám lehet okból miért lehet, hogy tooinstall Jupyter szeretné, hogy a számítógépen, és csatlakoztassa tooa a Spark hdinsight fürt.

* Annak ellenére, hogy a Jupyter notebookok már érhetők el az Azure HDInsight Spark-fürt hello, hello beállítás toocreate jegyzetfüzetek helyileg, egy futó fürtön hajtották az alkalmazás teszteléséhez, majd töltse fel a hello Jupyter telepíti a számítógépre biztosít notebookok toohello fürt. tooupload hello notebookok toohello fürt, vagy feltöltheti az azokat futtató hello Jupyter notebook vagy hello segítségével a fürt, vagy elmentheti toohello /HdiNotebooks mappa hello-fürthöz tartozó hello tárfiók. Notebookok hello fürtön tárolásával további információkért lásd: [Jupyter notebookok tároló](hdinsight-apache-spark-jupyter-notebook-kernels.md#where-are-the-notebooks-stored)?
* A rendelkezésre álló hello jegyzetfüzetek helyileg, csatlakoztathatja toodifferent Spark-fürtjei alapján az alkalmazás követelményeinek.
* GitHub tooimplement a forrásrendszerben vezérlő használja, és hello notebookok verzió-vezérlést. A együttműködési környezetekben, ahol több felhasználó is dolgozhat hello azonos is lehet notebookot.
* Dolgozhat notebookok helyileg egy fürtöt is nélkül. Csak akkor kell a fürt tootest jegyzetfüzetek ellen, nem toomanually jegyzetfüzetek vagy a környezet felügyeletéhez.
* Akkor lehet, hogy könnyebben tooconfigure a saját helyi fejlesztőkörnyezetet meghaladja a tooconfigure hello Jupyter telepítési hello fürtön.  Egy vagy több távoli fürtök beállítása nélkül helyileg telepített összes hello szoftver fordíthatja előnyére.

> [!WARNING]
> A helyi számítógépen Jupyter, több felhasználó is futtatja a hello: hello fürt azonos Spark hello azonos jegyzetfüzetet ugyanannyi időt vesz igénybe. Ilyen esetben több Livy munkamenetek jönnek létre. Ha problémát tapasztal, és szeretné, hogy toodebug, azokat, hogy egy összetett feladat tootrack, mely Livy munkamenet toowhich felhasználó tartozik.
>
>

## <a name="seealso"></a>Lásd még:
* [Overview: Apache Spark on Azure HDInsight (Áttekintés: Apache Spark on Azure HDInsight)](hdinsight-apache-spark-overview.md)

### <a name="scenarios"></a>Forgatókönyvek
* [Spark és BI: Interaktív adatelemzés végrehajtása a Spark on HDInsight használatával, BI-eszközökkel](hdinsight-apache-spark-use-bi-tools.md)
* [Spark és Machine Learning: A Spark on HDInsight használata az épület-hőmérséklet elemzésére HVAC-adatok alapján](hdinsight-apache-spark-ipython-notebook-machine-learning.md)
* [Spark és Machine Learning: használja a Spark on HDInsight toopredict élelmiszervizsgálati eredmények](hdinsight-apache-spark-machine-learning-mllib-ipython.md)
* [Spark Streaming: A Spark on HDInsight használata valós idejű streamelési alkalmazások összeállítására](hdinsight-apache-spark-eventhub-streaming.md)
* [A webhelynapló elemzése a Spark on HDInsight használatával](hdinsight-apache-spark-custom-library-website-log-analysis.md)

### <a name="create-and-run-applications"></a>Alkalmazások létrehozása és futtatása
* [Önálló alkalmazás létrehozása a Scala használatával](hdinsight-apache-spark-create-standalone-application.md)
* [Feladatok távoli futtatása Spark-fürtön a Livy használatával](hdinsight-apache-spark-livy-rest-interface.md)

### <a name="tools-and-extensions"></a>Eszközök és bővítmények
* [Toocreate IntelliJ IDEA HDInsight-eszközei beépülő használja, és küldje el a Spark Scala-alkalmazások](hdinsight-apache-spark-intellij-tool-plugin.md)
* [IntelliJ IDEA toodebug Spark-alkalmazások HDInsight-eszközei beépülő távolról használni](hdinsight-apache-spark-intellij-tool-plugin-debug-jobs-remotely.md)
* [Zeppelin notebookok használata Spark-fürttel HDInsighton](hdinsight-apache-spark-zeppelin-notebook.md)
* [Jupyter notebookokhoz elérhető kernelek a HDInsight Spark-fürtjében](hdinsight-apache-spark-jupyter-notebook-kernels.md)
* [Külső csomagok használata Jupyter notebookokkal](hdinsight-apache-spark-jupyter-notebook-use-external-packages.md)

### <a name="manage-resources"></a>Erőforrások kezelése
* [Az Azure HDInsight hello Apache Spark-fürt erőforrásainak kezelése](hdinsight-apache-spark-resource-manager.md)
* [Apache Spark-fürtön futó feladatok nyomon követése és hibakeresése a HDInsightban](hdinsight-apache-spark-job-debugging.md)
