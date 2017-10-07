---
title: "aaaCreate Scala app toorun a Spark-fürtjei - Azure HDInsight |} Microsoft Docs"
description: "Az Apache Maven scalában írt, hello rendszer és egy meglévő Maven archetype építése IntelliJ IDEA által biztosított Scala Spark-alkalmazás létrehozása."
services: hdinsight
documentationcenter: 
author: nitinme
manager: jhubbard
editor: cgronlun
tags: azure-portal
ms.assetid: b2467a40-a340-4b80-bb00-f2c3339db57b
ms.service: hdinsight
ms.custom: hdinsightactive
ms.workload: big-data
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/25/2017
ms.author: nitinme
ms.openlocfilehash: b25291b60921021486f55d78b4832a070a54d163
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="create-a-scala-maven-application-toorun-on-apache-spark-cluster-on-hdinsight"></a>Hozzon létre egy Scala Maven alkalmazás toorun az Apache Spark-fürttel hdinsighton

Megtudhatja, hogyan toocreate a Maven használata IntelliJ IDEA scalában írt Spark-alkalmazások. hello a cikkben az Apache Maven, hello rendszert, és az IntelliJ IDEA által biztosított Scala egy meglévő Maven archetype kezdődik.  A Scala-alkalmazások létrehozása az IntelliJ IDEA magában foglalja a hello a következő lépéseket:

* Hello buildelési rendszer Maven használata.
* Projekt Object Model (POM) fájl tooresolve Spark modul függőségek frissítése.
* Írja be az alkalmazás Scala.
* A jar-fájlra, amely elküldött tooHDInsight Spark-fürtök létrehozása.
* Futtassa a hello alkalmazást a Spark-fürt Livy használatával.

> [!NOTE]
> HDInsight is biztosít az IntelliJ IDEA beépülő modul eszköz tooease hello folyamat létrehozása és elküldése alkalmazások tooan HDInsight Spark-fürt Linux rendszeren. További információkért lásd: [használata a HDInsight-eszközei beépülő moduljának IntelliJ IDEA toocreate, és küldje el a Spark-alkalmazások](hdinsight-apache-spark-intellij-tool-plugin.md).
> 
> 

## <a name="prerequisites"></a>Előfeltételek

* Azure-előfizetés. Lásd: [Ingyenes Azure-fiók létrehozása](https://azure.microsoft.com/documentation/videos/get-azure-free-trial-for-testing-hadoop-in-hdinsight/).
* A HDInsight az Apache Spark-fürt. Útmutatásért lásd: [létrehozása az Apache Spark on Azure hdinsight clusters](hdinsight-apache-spark-jupyter-spark-sql.md).
* Oracle Java fejlesztői készlet. A későbbiekben telepítheti az [Itt](http://www.oracle.com/technetwork/java/javase/downloads/jdk8-downloads-2133151.html).
* A Java IDE. Ebben a cikkben az IntelliJ IDEA 15.0.1. A későbbiekben telepítheti az [Itt](https://www.jetbrains.com/idea/download/).

## <a name="install-scala-plugin-for-intellij-idea"></a>Az IntelliJ Idea Scala beépülő modul telepítése
IntelliJ IDEA telepítési volt nem kéri a felhasználót a Scala beépülő modul engedélyezése, ha indítsa el az IntelliJ IDEA, és hajtsa végre a következő lépéseket tooinstall hello beépülő modul hello:

1. Indítsa el az IntelliJ IDEA és az üdvözlőképernyőn kattintson **konfigurálása** majd **beépülő modulok**.
   
    ![Scala beépülő modul engedélyezése](./media/hdinsight-apache-spark-create-standalone-application/enable-scala-plugin.png)
2. Hello következő képernyőn kattintson **JetBrains telepítése beépülő modul** a hello bal alsó sarokban. A hello **Tallózás JetBrains beépülő modulok** párbeszédpanel megnyílik, keresse meg a Scala, és kattintson **telepítése**.
   
    ![Scala beépülő modul telepítése](./media/hdinsight-apache-spark-create-standalone-application/install-scala-plugin.png)
3. Hello beépülő modul sikeres telepítése után kattintson a hello **indítsa újra az IntelliJ IDEA gomb** toorestart hello IDE.

## <a name="create-a-standalone-scala-project"></a>Önálló Scala-projekt létrehozása
1. Indítsa el az IntelliJ IDEA, és hozzon létre egy új projektet. Hello új projekt párbeszédpanel, győződjön meg a következő lehetőségek hello, és kattintson a **következő**.
   
    ![Maven-projekt létrehozása](./media/hdinsight-apache-spark-create-standalone-application/create-maven-project.png)
   
   * Válassza ki **Maven** hello projekt típusként.
   * Adjon meg egy **SDK projekt**. Az új gombra, és keresse meg a toohello Java telepítési könyvtárat, általában `C:\Program Files\Java\jdk1.8.0_66`.
   * Jelölje be hello **létrehozása a archetype** lehetőséget.
   * Archetypes hello listában jelölje ki **org.scala tools.archetypes:scala-archetype-egyszerű**. Ez létrehoz hello jobb könyvtárszerkezetét és hello szükséges alapértelmezett függőségek toowrite Scala program letöltése.
2. Adja meg a megfelelő értékeket **GroupId**, **artifactid szakaszát**, és **verzió**. Kattintson a **Tovább** gombra.
3. Hello következő párbeszédpanelen, amelyben meg kell határoznia Maven kezdőkönyvtár és egyéb felhasználói beállításokat, fogadja el a hello alapértelmezett beállításokat, majd kattintson **következő**.
4. A hello utolsó párbeszédpanelen adja meg a projekt nevét és helyét, és kattintson a **Befejezés**.
5. Törölje a hello **MySpec.Scala** a(z) **src\test\scala\com\microsoft\spark\example**. Nem kell a hello alkalmazáshoz.
6. Ha szükséges, nevezze át a hello alapértelmezett forrás- és a vizsgálati fájlokat. Az IntelliJ IDEA hello hello bal oldali ablaktáblán, keresse meg túl**src\main\scala\com.microsoft.spark.example**. Kattintson a jobb gombbal **App.scala**, kattintson a **Refactor**, kattintson a fájl. Nevezze át, és a hello párbeszédpanelen adja meg a hello hello alkalmazás új nevét, majd **Refactor**.
   
    ![Fájl átnevezése](./media/hdinsight-apache-spark-create-standalone-application/rename-scala-files.png)  
7. A későbbi lépésekben hello frissíteni fogja a Spark Scala alkalmazás hello hello pom.xml toodefine hello függőségeit. Azok a toobe letöltött, és automatikusan feloldani a Maven ennek megfelelően kell konfigurálni.
   
    ![Automatikus letöltés Maven konfigurálása](./media/hdinsight-apache-spark-create-standalone-application/configure-maven.png)
   
   1. A hello **fájl** menüben kattintson a **beállítások**.
   2. A hello **beállítások** párbeszédpanelen válassza túl**Build, a végrehajtási, a központi telepítési** > **Build Tools** > **Maven**  >  **Importálása**.
   3. A beállításnak a hello túl**Import Maven projektek automatikusan**.
   4. Kattintson a **alkalmaz**, és kattintson a **OK**.
8. Az alkalmazás kódjában hello Scala forrás fájl tooinclude frissítése Nyissa meg, és cserélje le a meglévő mintakód a következő kód hello hello és hello módosítások mentése. Ez a kód hello adatokat olvas hello HVAC.csv (az összes HDInsight Spark-fürtjei elérhető), hello tartalmazó sorok csak egy számjegy hello hatodik oszlopban lekéri és hello kimenetet túl írja**/HVACOut** hello alapértelmezett tároló alatt hello fürt tárolója.
   
        package com.microsoft.spark.example
   
        import org.apache.spark.SparkConf
        import org.apache.spark.SparkContext
   
        /**
          * Test IO toowasb
          */
        object WasbIOTest {
          def main (arg: Array[String]): Unit = {
            val conf = new SparkConf().setAppName("WASBIOTest")
            val sc = new SparkContext(conf)
   
            val rdd = sc.textFile("wasb:///HdiSamples/HdiSamples/SensorSampleData/hvac/HVAC.csv")
   
            //find hello rows which have only one digit in hello 7th column in hello CSV
            val rdd1 = rdd.filter(s => s.split(",")(6).length() == 1)
   
            rdd1.saveAsTextFile("wasb:///HVACout")
          }
        }
9. Hello pom.xml frissítése.
   
   1. Belül `<project>\<properties>` adja hozzá a következő hello:
      
          <scala.version>2.10.4</scala.version>
          <scala.compat.version>2.10.4</scala.compat.version>
          <scala.binary.version>2.10</scala.binary.version>
   2. Belül `<project>\<dependencies>` adja hozzá a következő hello:
      
           <dependency>
             <groupId>org.apache.spark</groupId>
             <artifactId>spark-core_${scala.binary.version}</artifactId>
             <version>1.4.1</version>
           </dependency>
      
      Mentse a módosításokat toopom.xml.
10. Hozzon létre hello .jar fájlt. IntelliJ IDEA lehetővé teszi a JAR létrehozását, a projekt összetevő. Hajtsa végre a következő lépéseket hello.
    
    1. A hello **fájl** menüben kattintson a **szerkezetének**.
    2. A hello **szerkezetének** párbeszédpanel, kattintson a **összetevők** , majd a hello plusz jelre. A hello előugró párbeszédpanelen kattintson az **JAR**, és kattintson a **a függőségekkel rendelkező modulok**.
       
        ![Hozzon létre JAR](./media/hdinsight-apache-spark-create-standalone-application/create-jar-1.png)
    3. A hello **modulokban létrehozása JAR** párbeszédpanel kattintson hello három pont (![három pont](./media/hdinsight-apache-spark-create-standalone-application/ellipsis.png) ) hello elleni **fő osztály**.
    4. A hello **fő osztály kiválasztása** párbeszédpanel megnyitásához, jelölje be hello osztály, amely alapértelmezés szerint jelenik meg, és kattintson a **OK**.
       
        ![Hozzon létre JAR](./media/hdinsight-apache-spark-create-standalone-application/create-jar-2.png)
    5. A hello **modulokban létrehozása JAR** párbeszédpanelen győződjön meg arról, hogy hello beállítás túl**toohello cél JAR kibontása** van kiválasztva, és kattintson **OK**. Ez minden függőség egyetlen JAR hoz létre.
       
        ![Hozzon létre JAR](./media/hdinsight-apache-spark-create-standalone-application/create-jar-3.png)
    6. hello kimeneti elrendezés lap felsorolja az összes hello JAR-fájlok kivételével, amelyek tartalmazzák a hello Maven project. Kiválaszthatja, és azokat, amelyeken hello Scala alkalmazás törlése hello nincs közvetlen függőség van. Itt létrehozzuk hello alkalmazáshoz, akkor eltávolíthatja az összes, de az utolsó hello (**SparkSimpleApp fordítási kimeneti**). Hello JAR-fájlok kivételével toodelete válasszon, majd kattintson a hello **törlése** ikonra.
       
        ![Hozzon létre JAR](./media/hdinsight-apache-spark-create-standalone-application/delete-output-jars.png)
       
        Győződjön meg arról, hogy **győződjön Build** be van jelölve, amely biztosítja, hogy hello jar minden alkalommal létrejön hello projekt beépített vagy frissíteni. Kattintson a **alkalmazása** , majd **OK**.
    7. Hello menüsorában kattintson **Build**, és kattintson a **ellenőrizze projekt**. Is **Build összetevők** toocreate hello jar. hello kimeneti jar alatt létrejön **\out\artifacts**.
       
        ![Hozzon létre JAR](./media/hdinsight-apache-spark-create-standalone-application/output.png)

## <a name="run-hello-application-on-hello-spark-cluster"></a>Hello Spark-fürt hello az alkalmazás futtatása
toorun hello alkalmazás hello fürtön, el kell végeznie a következő hello:

* **Másolás hello alkalmazás jar toohello Azure storage-blob** hello-fürthöz tartozó. Használhat [ **AzCopy**](../storage/common/storage-use-azcopy.md), a parancs így sor segédprogram, toodo. Nincsenek más ügyfelek is használható tooupload adatok számos. A rájuk vonatkozó további található [feltölteni az adatokat a HDInsight Hadoop-feladatok](hdinsight-upload-data.md).
* **Egy alkalmazás feladat Livy toosubmit távolról használható** toohello Spark-fürt. A HDInsight Spark-fürtök REST végpontok tooremotely submit Spark feladatok elérhetővé tévő Livy tartalmazza. További információkért lásd: [Livy távolról használata Spark-fürtök a HDInsight Spark küldje el feladatok](hdinsight-apache-spark-livy-rest-interface.md).

## <a name="next-step"></a>Következő lépés

Az e cikkben megtanulta, hogyan meg toocreate a Spark scala-alkalmazások. Előzetes toohello tovább cikk toolearn hogyan toorun az alkalmazást egy HDInsight Spark fürt Livy használatával.

> [!div class="nextstepaction"]
>[Feladatok távoli futtatása Spark-fürtön a Livy használatával](hdinsight-apache-spark-livy-rest-interface.md)

