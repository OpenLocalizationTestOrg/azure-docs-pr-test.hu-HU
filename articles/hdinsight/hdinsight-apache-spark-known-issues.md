---
title: "aaaTroubleshoot problémákat az Apache Spark on Azure hdinsight fürt |} Microsoft Docs"
description: "Az Azure HDInsight Spark-fürtjei kapcsolódó tooApache problémák megismerése és hogyan toowork körül azokat."
services: hdinsight
documentationcenter: 
author: mumian
manager: jhubbard
editor: cgronlun
tags: azure-portal
ms.assetid: 610c4103-ffc8-4ec0-ad06-fdaf3c4d7c10
ms.service: hdinsight
ms.custom: hdinsightactive
ms.workload: big-data
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/10/2017
ms.author: nitinme
ms.openlocfilehash: 7373b90524ae5dbb10ab8ded593aa38d12c14b55
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="known-issues-for-apache-spark-cluster-on-hdinsight"></a>A HDInsight az Apache Spark-fürt kapcsolatos ismert problémák

Ez a dokumentum nyomon követi az összes hello ismert problémái hello HDInsight Spark nyilvános előzetes verziójához.  

## <a name="livy-leaks-interactive-session"></a>Livy szivárgást interaktív munkamenet
Újraindításakor Livy (az Ambari vagy tooheadnode 0 virtuális gép újraindítása miatt) egy interaktív munkamenet-életben egy interaktív feladat munkamenet szivárgását okozhatja. Emiatt az új feladatok is hello elfogadott állapotban ragadt, és nem indítható el.

**Megoldás:**

A következő eljárás tooworkaround hello probléma hello használata:

1. Ssh headnode be. További információk: [Az SSH használata HDInsighttal](hdinsight-hadoop-linux-use-ssh-unix.md).

2. Futtassa a következő parancs toofind hello alkalmazás hello azonosítók hello interaktív feladatok elindítása Livy használatával. 
   
        yarn application –list
   
    hello alapértelmezett feladat neve lesz Livy, ha nincs megadva, a hello explicit nevekkel hello feladatok indított egy Livy interaktív munkamenet Livy munkamenet Jupyter notebook indította, hello feladatnév indul rendelkező remotesparkmagics_ *. 
3. Futtassa a következő parancs tookill hello ezeket a feladatokat. 
   
        yarn application –kill <Application ID>

Új feladat futtatása elindul. 

## <a name="spark-history-server-not-started"></a>Spark előzmények kiszolgáló nem indult el
Spark előzmények kiszolgáló nem automatikusan elindul a fürt létrehozása után.  

**Megoldás:** 

Manuálisan indítsa el az Ambari hello előzmények kiszolgáló.

## <a name="permission-issue-in-spark-log-directory"></a>A Spark naplókönyvtár engedély probléma
Amikor hdiuser spark-submit egy feladatot ad meg, nincs-e egy hiba java.io.FileNotFoundException: /var/log/spark/sparkdriver_hdiuser.log (engedély megtagadva) és hello illesztőprogram napló nem készül. 

**Megoldás:**

1. Hdiuser toohello Hadoop csoport hozzáadása. 
2. Adja meg a 777 engedélyek /var/log/spark a fürt létrehozása után. 
3. Frissítési hello spark napló helyét Ambari toobe könyvtár használatával 777 engedélyekkel.  
4. Futtassa a spark-nyújt, sudo.  

## <a name="spark-phoenix-connector-is-not-supported"></a>A Spark-Phoenix összekötő nem támogatott

Hello Spark-Phoenix összekötő egy HDInsight Spark-fürt nem támogatott.

**Megoldás:**

Hello Spark-HBase összekötő kell helyette használni. Útmutatásért lásd: [hogyan toouse Spark-HBase-összekötő](https://blogs.msdn.microsoft.com/azuredatalake/2016/07/25/hdinsight-how-to-use-spark-hbase-connector/).

## <a name="issues-related-toojupyter-notebooks"></a>Kapcsolódó problémák tooJupyter notebookok
Az alábbiakban néhány ismert problémák kapcsolódó tooJupyter notebookok.

### <a name="notebooks-with-non-ascii-characters-in-filenames"></a>A fájlnevek nem ASCII-karaktereket notebookokban
A Spark HDInsight-fürtökkel használt Jupyter notebookok fájlnevekben nem rendelkezhet nem ASCII-karaktereket. Ha tooupload hello Jupyter felhasználói felület, amely a nem ASCII-fájl nevét a fájlban, akkor sikertelen lesz csendes (Ez azt jelenti, hogy Jupyter nem engedi hello fájl feltöltéséhez, de a vagy azt nem throw látható hiba). 

### <a name="error-while-loading-notebooks-of-larger-sizes"></a>Nagyobb méretű notebookok betöltése közben hiba
Láthatja, hogy hiba ** `Error loading notebook` ** Ha nagyobb méretű notebookok tölthető be.  

**Megoldás:**

Ha ez a hibaüzenet azt jelenti az adatok elveszett vagy sérült.  Továbbra is a lemezen vannak jegyzetfüzetek `/var/lib/jupyter`, ráadásul SSH hello fürt tooaccess be őket. További információk: [Az SSH használata HDInsighttal](hdinsight-hadoop-linux-use-ssh-unix.md).

Ha SSH használatával toohello fürt csatlakozott, átmásolhatja a jegyzetfüzetek a fürt tooyour helyi számítógép (SCP vagy WinSCP használatával) hello notebook a fontos adatok biztonsági mentés tooprevent hello veszteségként. Ezek közül SSH-alagút be a következő port 8001 tooaccess Jupyter headnode hello átjáró áthaladás nélkül.  Ezekből a notebook hello kimenete törölje és mentse újra toominimize hello jegyzetfüzet méretét.

tooprevent Ez a hiba a jövőbeli, hajtsa végre az tanácsokat hello le:

* Fontos tookeep hello notebook kis méretű legyen. Bármely olyan kimenete a Spark feladatok küldött vissza tooJupyter hello jegyzetfüzet megőrződjenek.  Az ajánlott eljárás a Jupyter futtató általános tooavoid `.collect()` a nagy RDD vagy dataframes; helyette, ha azt szeretné, hogy egy RDD tartalmát, toopeek, fontolja meg a futó `.take()` vagy `.sample()` , hogy a kimenet nem get túl nagy.
* Is a notebook mentésekor törölje az összes kimeneti cellák tooreduce hello méretét.

### <a name="notebook-initial-startup-takes-longer-than-expected"></a>Kezdeti indítási notebook vártnál hosszabb ideig tart.
Jupyter notebook használatával Spark magic utasításnak elsőként kód több mint egy percbe is beletelhet.  

**Magyarázat:**

Ez akkor fordul elő, mert hello első kódcella futtatásakor. Hello háttérben kezdeményez a munkamenet-konfigurációhoz és Spark, SQL, és a Hive-környezeteket. Miután ezek a környezetek van beállítva, hello első utasítása fut, és így az, hogy hello utasítás tartott egy hosszú ideig toocomplete hello benyomást.

### <a name="jupyter-notebook-timeout-in-creating-hello-session"></a>Jupyter notebook időtúllépés hello munkamenet létrehozása
Spark-fürt kifogyott az erőforrásokból, hello Spark és a a hello Jupyter notebook Pyspark kernel toocreate hello munkamenet közben időtúllépés lesz. 

**Megoldást:** 

1. Szabadítson fel a Spark-fürt egyes erőforrások:
   
   * Egyéb külső notebookok toohello is zárja be és a Halt menüből, vagy kattintson a Leállítás hello notebook Explorer leállítása.
   * A YARN más Spark-alkalmazások leállítása.
2. Indítsa újra a hello notebook toostart próbált fel. Erőforrásokkal elérhetőknek kell lenniük a akkor toocreate most egy munkamenet.

## <a name="see-also"></a>Lásd még:
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
* [Toocreate IntelliJ IDEA HDInsight-eszközei beépülő használja, és küldje el a Spark Scala applicatons](hdinsight-apache-spark-intellij-tool-plugin.md)
* [IntelliJ IDEA toodebug Spark-alkalmazások HDInsight-eszközei beépülő távolról használni](hdinsight-apache-spark-intellij-tool-plugin-debug-jobs-remotely.md)
* [Zeppelin notebookok használata Spark-fürttel HDInsighton](hdinsight-apache-spark-zeppelin-notebook.md)
* [Jupyter notebookokhoz elérhető kernelek a HDInsight Spark-fürtjében](hdinsight-apache-spark-jupyter-notebook-kernels.md)
* [Külső csomagok használata Jupyter notebookokkal](hdinsight-apache-spark-jupyter-notebook-use-external-packages.md)
* [Jupyter telepítse a számítógépre, és csatlakozzon a HDInsight Spark-fürt tooan](hdinsight-apache-spark-jupyter-notebook-install-locally.md)

### <a name="manage-resources"></a>Erőforrások kezelése
* [Az Azure HDInsight hello Apache Spark-fürt erőforrásainak kezelése](hdinsight-apache-spark-resource-manager.md)
* [Apache Spark-fürtön futó feladatok nyomon követése és hibakeresése a HDInsightban](hdinsight-apache-spark-job-debugging.md)

