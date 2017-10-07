---
title: a Power Query - Azure HDInsight aaaConnect Excel tooHadoop |} Microsoft Docs
description: "Ismerje meg, hogyan tootake előnyeit az üzleti intelligencia összetevők és használja a Power Query az Excel tooaccess adatok a HDInsight Hadoop tárolja."
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
ms.openlocfilehash: 1213849f1bc04e0ed206750b80766ff2268664b4
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="connect-excel-toohadoop-by-using-power-query"></a>Excel tooHadoop csatlakoztatása a Power Query használatával
Egy kulcs az hello Microsoft big data-megoldások szolgáltatása hello integrálása az Azure HDInsight Hadoop-fürtök Microsoft üzleti intelligenciával-összetevők. Egy elsődleges példája hello képességét tooconnect Excel toohello Azure Storage-fiók, amely a Hadoop-fürttel társított hello Microsoft Power Query az Excel beépülő modul használatával hello adatokat tartalmaz. Ez a cikk végigvezeti a hdinsight eszközzel tooset be és használja a Power Query tooquery adatokat Hadoop-fürthöz társított kezelésének.

### <a name="prerequisites"></a>Előfeltételek
Ez a cikk megkezdése előtt rendelkeznie kell a következő elemek hello:

* **HDInsight-fürtök**. tooconfigure, lásd: [Ismerkedés az Azure HDInsight][hdinsight-get-started].
* **A munkaállomás** , amely a Windows 7, Windows Server 2008 R2 vagy újabb operációs rendszert futtat.
* **Office 2016, Office 2013 Professional Plus, az Office 365 ProPlus, az Excel 2013 önálló vagy Office 2010 Professional Plus**.

## <a name="install-power-query"></a>A Power Query telepítése
A Power Query importálhatja az adatokat, amelyek már kimeneti vagy, amely egy Hadoop-feladat fut a HDInsight-fürtök létrejött.

Az Excel 2016 Power Query integrálva lett hello adatok menüszalag hello Get & átalakító szakasz alatt. Az Excel régebbi verzióit, töltse le a Microsoft Power Query az Excel programhoz a hello [Microsoft Download Center] [ powerquery-download] és telepítéséhez.

## <a name="import-hdinsight-data-into-excel"></a>A HDInsight-adatok importálása Excelbe
Power Query beépülő modul az Excel hello teszi, hogy könnyen tooimport adatokat Excelbe, ahol PowerPivot és Power térkép Üzletiintelligencia-eszközök is lehet használt tooinspect, elemzése és hello adatokat a HDInsight-fürthöz.

**HDInsight-fürtök tooimport adatait**

1. Nyissa meg Excel.
2. Hozzon létre egy új munkafüzetet.
3. Hajtsa végre a hello lépések hello Excel verziószámú változata alapján:

    - Excel 2016

        - Hello kattintson **adatok** menüben kattintson **adatok beolvasása** a hello **& átalakítási adatok beolvasása** menüszalag, kattintson a **az Azure**, és kattintson a ** Az Azure HDInsight(HDFS)**.

        ![HDI. PowerQuery.SelectHdiSource](./media/hdinsight-connect-excel-power-query/hdi.powerquery.selecthdisource.excel2016.png)

    - Az Excel 2013/2010

        - Kattintson a hello **Power Query** menüben kattintson **az Azure**, és kattintson a **a Microsoft Azure HDInsight**.
   
        ![HDI. PowerQuery.SelectHdiSource][image-hdi-powerquery-hdi-source]
       
        **Megjegyzés:** Ha nem lát hello **Power Query** menüben nyissa meg túl**fájl** > **beállítások** > **bővítmények**, és válassza ki **COM-bővítmények** a hello legördülő **kezelése** alján hello hello mezőbe. Jelölje be hello **Ugrás... ** gombra, és győződjön meg arról, hogy hello mezőben hello Power Query az Excel-bővítmény ellenőrizte a.
       
        **Megjegyzés:** Power Query segítségével is tooimport adatokat HDFS kattintva **egyéb forrásokból származó**.
4. A **fióknév**, írja be hello Azure Blob storage-fiók a fürthöz tartozó hello nevét, és kattintson a **OK**. Ez a fiók lehet hello [alapértelmezett tárfiók](hdinsight-administer-use-management-portal.md#find-the-default-storage-account) vagy a kapcsolt tárfiókra.  hello formátuma *https://&lt;StorageAccountName >.blob.core.windows.net/*.
5. A **Fiókkulcs**, adja meg a Blob storage-fiók hello hello kulcsot, és kattintson a **mentése**. (Kell tooenter hello fiók információk csak hello ezt a tárolót első megnyitásakor.)
6. A hello **Navigator** hello ablaktábla bal hello Lekérdezésszerkesztő részén, kattintson duplán a hello Blob storage-tároló nevének. Alapértelmezés szerint hello Tárolónév van hello azonos nevű hello fürt neveként.
7. Keresse meg **HiveSampleData.txt** a hello **neve** oszlop (hello mappa elérési út **... / hive/adatraktár/hivesampletable/**), és kattintson a **bináris** a HiveSampleData.txt hello balra. Minden hello fürt HiveSampleData.txt tartalmaz. Ha szükséges egy saját fájlt is használhatja.
   
    ![HDI. PowerQuery.ImportData][image-hdi-powerquery-importdata]
8. Ha azt szeretné, átnevezheti hello oszlopneveket. Amikor elkészült, kattintson **zárja be az & betölteni**.  hello már betöltött tooyour munkafüzet:
   
    ![HDI. PowerQuery.ImportedTable][image-hdi-powerquery-imported-table]

## <a name="next-steps"></a>Következő lépések
Ebben a cikkben megtanulta, hogyan meg toouse Power Query tooretrieve adatokat Excelbe HDInsight. Ehhez hasonlóan is visszaállíthatja az adatokat a HDInsight-ból az Azure SQL-adatbázisba. Akkor is HDInsight lehetséges tooupload adatokat. toolearn több, tekintse meg a következő cikkek hello:

* [Csatlakozás a Microsoft Hive ODBC-illesztő hello Excel tooHDInsight][hdinsight-ODBC]
* [Adatok tooHDInsight feltöltése][hdinsight-upload-data]

[hdinsight-ODBC]: hdinsight-connect-excel-hive-odbc-driver.md
[hdinsight-get-started]: hdinsight-hadoop-linux-tutorial-get-started.md
[hdinsight-upload-data]: hdinsight-upload-data.md

[image-hdi-powerquery-hdi-source]: ./media/hdinsight-connect-excel-power-query/hdi.powerquery.selecthdisource.png
[image-hdi-powerquery-importdata]: ./media/hdinsight-connect-excel-power-query/hdi.powerquery.importdata.png
[image-hdi-powerquery-imported-table]: ./media/hdinsight-connect-excel-power-query/hdi.powerquery.importedtable.PNG

[powerquery-download]: http://go.microsoft.com/fwlink/?LinkID=286689
