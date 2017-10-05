---
title: "Excel csatlakoztatása a Power Query - az Azure HDInsight Hadoop |} Microsoft Docs"
description: "Megtudhatja, hogyan előnyeit az üzleti intelligenciát biztosító összetevők, és használja a Power Query az Excel programhoz a hdinsight Hadoop hozzáférés adataihoz."
services: hdinsight
documentationcenter: 
tags: azure-portal
author: mumian
manager: jhubbard
editor: cgronlun
ms.assetid: 01ad2f90-7520-44d9-8c16-4d936faaff9b
ms.service: hdinsight
ms.custom: hdinsightactive
ms.workload: big-data
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/11/2017
ms.author: jgao
ms.openlocfilehash: 6a15efb091b4faaa7305bb8faa362c62fac595cb
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/03/2017
---
# <a name="connect-excel-to-hadoop-by-using-power-query"></a>Az Excel és a Hadoop csatlakoztatása a Power Query használatával
A Microsoft big data-megoldás egyik fontos jellemzők az üzleti intelligenciával összetevők Microsoft Azure hdinsight Hadoop-fürtök integrálását. Egy elsődleges példa azt a képességet Excel csatlakoztatása az Azure Storage-fiók, amely a Microsoft Power Query az Excel beépülő modul használatával a Hadoop-fürthöz kapcsolódó adatokat tartalmazza. Ez a cikk bemutatja, hogyan állítson be és a Power Query használatával kezeli a HDInsight Hadoop-fürthöz társított adatait kérdezi le.

### <a name="prerequisites"></a>Előfeltételek
Ez a cikk megkezdése előtt rendelkeznie kell a következő elemek:

* **HDInsight-fürtök**. Egy konfigurálásához lásd: [Ismerkedés az Azure HDInsight][hdinsight-get-started].
* **A munkaállomás** , amely a Windows 7, Windows Server 2008 R2 vagy újabb operációs rendszert futtat.
* **Office 2016, Office 2013 Professional Plus, az Office 365 ProPlus, az Excel 2013 önálló vagy Office 2010 Professional Plus**.

## <a name="install-power-query"></a>A Power Query telepítése
A Power Query importálhatja az adatokat, amelyek már kimeneti vagy, amely egy Hadoop-feladat fut a HDInsight-fürtök létrejött.

Az Excel 2016 Power Query integrálva lett a adatok szalagon Get & átalakító szakaszban. Az Excel régebbi verzióit, töltse le a Microsoft Power Query az Excel programhoz a [Microsoft Download Center] [ powerquery-download] és telepítéséhez.

## <a name="import-hdinsight-data-into-excel"></a>A HDInsight-adatok importálása Excelbe
A Power Query beépülő modul az Excel segítségével egyszerűen adatok importálása Excelbe, ahol Üzletiintelligencia-eszközök például a PowerPivot és Power térkép használható vizsgálata, elemzése és az adatokat a HDInsight-fürthöz.

**Adatok importálása a HDInsight-fürtök**

1. Nyissa meg Excel.
2. Hozzon létre egy új munkafüzetet.
3. Hajtsa végre az alábbi lépéseket az Excel verziószámú változata alapján:

    - Excel 2016

        - Kattintson a **adatok** menüben kattintson **adatok beolvasása** a a **& átalakítási adatok beolvasása** menüszalag, kattintson a **az Azure**, és kattintson a **Az Azure HDInsight(HDFS)**.

        ![HDI. PowerQuery.SelectHdiSource](./media/hdinsight-connect-excel-power-query/hdi.powerquery.selecthdisource.excel2016.png)

    - Az Excel 2013/2010

        - Kattintson a **Power Query** menüben kattintson a **az Azure**, és kattintson a **a Microsoft Azure HDInsight**.
   
        ![HDI. PowerQuery.SelectHdiSource][image-hdi-powerquery-hdi-source]
       
        **Megjegyzés:** Ha nem látja a **Power Query** menüben keresse fel **fájl** > **beállítások** > **bővítmények**, és válassza ki **COM-bővítmények** a legördülő **kezelése** be az oldal alján. Válassza ki a **Ugrás...**  gombra, és győződjön meg arról, hogy a Power Query az Excel-bővítmény a jelölőnégyzet be van adva.
       
        **Megjegyzés:** Power Query is lehetővé teszi, hogy adatokat importáljon HDFS kattintva **egyéb forrásokból származó**.
4. A **fióknév**, adja meg a fürthöz tartozó Azure Blob storage-fiók nevét, és kattintson a **OK**. Ezt a fiókot a [alapértelmezett tárfiók](hdinsight-administer-use-management-portal.md#find-the-default-storage-account) vagy a kapcsolt tárfiókra.  A formátum *https://&lt;StorageAccountName >.blob.core.windows.net/*.
5. A **Fiókkulcs**, adja meg a kulcsot a Blob storage-fiók esetében, és kattintson a **mentése**. (Meg kell adnia a fiók információ csak az első használatakor ezt a tárolót.)
6. Az a **Navigator** a a lekérdezés-szerkesztő, bal oldali ablaktáblán kattintson duplán a Blob storage-tároló nevének. Alapértelmezés szerint a tároló neve neve megegyezik a fürt neve.
7. Keresse meg **HiveSampleData.txt** a a **neve** oszlop (a mappa elérési útja **... / hive/adatraktár/hivesampletable/**), és kattintson a **bináris** bal oldalán HiveSampleData.txt. A fürt HiveSampleData.txt tartalmaz. Ha szükséges egy saját fájlt is használhatja.
   
    ![HDI. PowerQuery.ImportData][image-hdi-powerquery-importdata]
8. Ha azt szeretné, átnevezheti az oszlopok neveit. Amikor elkészült, kattintson **zárja be az & betölteni**.  Az adatok betöltése a munkafüzethez:
   
    ![HDI. PowerQuery.ImportedTable][image-hdi-powerquery-imported-table]

## <a name="next-steps"></a>Következő lépések
Ebben a cikkben megtanulta, HDInsight Excelbe adatainak lekérése a Power Query segítségével. Ehhez hasonlóan is visszaállíthatja az adatokat a HDInsight-ból az Azure SQL-adatbázisba. Akkor is a HDInsight feltölteni az adatokat. További tudnivalókért tekintse meg a következő cikkeket:

* [Excel csatlakoztatása a Microsoft Hive ODBC-illesztőprogram HDInsight][hdinsight-ODBC]
* [Upload Data to HDInsight][hdinsight-upload-data]

[hdinsight-ODBC]: hdinsight-connect-excel-hive-odbc-driver.md
[hdinsight-get-started]: hdinsight-hadoop-linux-tutorial-get-started.md
[hdinsight-upload-data]: hdinsight-upload-data.md

[image-hdi-powerquery-hdi-source]: ./media/hdinsight-connect-excel-power-query/hdi.powerquery.selecthdisource.png
[image-hdi-powerquery-importdata]: ./media/hdinsight-connect-excel-power-query/hdi.powerquery.importdata.png
[image-hdi-powerquery-imported-table]: ./media/hdinsight-connect-excel-power-query/hdi.powerquery.importedtable.PNG

[powerquery-download]: http://go.microsoft.com/fwlink/?LinkID=286689
