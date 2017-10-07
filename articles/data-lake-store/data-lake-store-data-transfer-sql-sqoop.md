---
title: "Data Lake Store és a Sqoop használatával Azure SQL-adatbázis közötti aaaCopy adatok |} Microsoft Docs"
description: "Sqoop toocopy adatokat az Azure SQL Database és a Data Lake Store között"
services: data-lake-store
documentationcenter: 
author: nitinme
manager: jhubbard
editor: cgronlun
ms.assetid: 3f914b2a-83cc-4950-b3f7-69c921851683
ms.service: data-lake-store
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 06/29/2017
ms.author: nitinme
ms.openlocfilehash: f58483455f0ebe9544673a1d5c5884f2721c800c
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="copy-data-between-data-lake-store-and-azure-sql-database-using-sqoop"></a>Data Lake Store és a Sqoop használatával Azure SQL-adatbázis közötti adatok másolása
Megtudhatja, hogyan toouse Apache Sqoop tooimport és exportálási Azure SQL Database és a Data Lake Store között adatokat.

## <a name="what-is-sqoop"></a>Mi az a Sqoop?
Big data-alkalmazások olyan természetes téve a strukturálatlan és félig strukturált adatok, például a naplókat, valamint a fájlok feldolgozása. Azonban előfordulhat, hogy is van a szükséges strukturált tooprocess relációs adatbázisokban tárolt adatokat.

[Apache Sqoop](https://sqoop.apache.org/docs/1.4.4/SqoopUserGuide.html) egy eszköz tootransfer adatokat relációs adatbázisok és a big Data típusú adatok tára, például a Data Lake Store között van. Is használhatja az tooimport adatait egy relációs adatbázis-kezelő rendszer (RDBMS) például az Azure SQL Database Data Lake Store. Majd átalakíthatja és adatelemzés hello segítségével a big data-számítási feladatok és majd hello adatok exportálása vissza egy RDBMS. Ebben az oktatóanyagban az Azure SQL Database, a relációs adatbázis tooimport/exportálása a használja.

## <a name="prerequisites"></a>Előfeltételek
Ez a cikk elkezdéséhez hello következő kell rendelkeznie:

* **Azure-előfizetés**. Lásd: [Ingyenes Azure-fiók létrehozása](https://azure.microsoft.com/pricing/free-trial/).
* **Egy Azure Data Lake Store-fiók**. Útmutatást toocreate egy, lásd: [Ismerkedés az Azure Data Lake Store](data-lake-store-get-started-portal.md)
* **Az Azure HDInsight-fürt** a Data Lake Store-fiók hozzáférési tooa. Lásd: [HDInsight-fürtök létrehozása a Data Lake Store](data-lake-store-hdinsight-hadoop-use-portal.md). Ez a cikk feltételezi, hogy rendelkezik-e egy Data Lake Store-hozzáféréssel rendelkező HDInsight Linux-fürt.
* **Azure SQL Database** Útmutatást toocreate egy, lásd: [Azure SQL-adatbázis létrehozása](../sql-database/sql-database-get-started.md)

## <a name="do-you-learn-fast-with-videos"></a>Gyorsan tanul videók segítségével?
[A videó](https://mix.office.com/watch/1butcdjxmu114) hogyan toocopy adatok Azure Storage Blobs és a Data Lake Store használatának ból a DistCp között.

## <a name="create-sample-tables-in-hello-azure-sql-database"></a>A minta táblázatok hello Azure SQL-adatbázis létrehozása
1. toostart rendelkező, két minta táblázatok hello Azure SQL-adatbázis létrehozása. Használjon [SQL Server Management Studio](../sql-database/sql-database-connect-query-ssms.md) vagy a Visual Studio tooconnect toohello Azure SQL Database lekérdezések futtatása hello kövesse.

    **Table1 létrehozása**

        CREATE TABLE [dbo].[Table1](
        [ID] [int] NOT NULL,
        [FName] [nvarchar](50) NOT NULL,
        [LName] [nvarchar](50) NOT NULL,
         CONSTRAINT [PK_Table_4] PRIMARY KEY CLUSTERED
            (
                   [ID] ASC
            )
        ) ON [PRIMARY]
        GO

    **Table2 létrehozása**

        CREATE TABLE [dbo].[Table2](
        [ID] [int] NOT NULL,
        [FName] [nvarchar](50) NOT NULL,
        [LName] [nvarchar](50) NOT NULL,
         CONSTRAINT [PK_Table_4] PRIMARY KEY CLUSTERED
            (
                   [ID] ASC
            )
        ) ON [PRIMARY]
        GO
2. A **Table1**, vegye fel néhány adatot. Hagyja **Table2** üres. Az adatok fogunk importálni **Table1** a Data Lake Store. Majd, hogy exportálja az adatokat a Data Lake Store az **Table2**. Futtassa a következő kódrészletet hello.

        INSERT INTO [dbo].[Table1] VALUES (1,'Neal','Kell'), (2,'Lila','Fulton'), (3, 'Erna','Myers'), (4,'Annette','Simpson');


## <a name="use-sqoop-from-an-hdinsight-cluster-with-access-toodata-lake-store"></a>A HDInsight fürtök használata Sqoop tooData Lake áruház eléréséhez
HDInsight-fürtök már hello Sqoop csomagok érhető el. Ha hello HDInsight fürt toouse Data Lake Store további tárterületként állította be, használhatja (nélkül végrehajtott bármilyen konfigurációs módosításokat) Sqoop tooimport/exportálja az adatokat egy relációs adatbázisban (a példában az Azure SQL Database) között és egy Data Lake Store fiók.

1. Ebben az oktatóanyagban feltételezzük, hogy létrehozott egy Linux-fürt, hogy az SSH tooconnect toohello fürt kell használni. Lásd: [Connect tooa Linux-alapú HDInsight-fürt](../hdinsight/hdinsight-hadoop-linux-use-ssh-unix.md).
2. Győződjön meg arról, hogy van-e hozzáférési hello Data Lake Store-fiók hello fürtből. Futtassa a parancsot követő hello SSH parancssorból hello:

        hdfs dfs -ls adl://<data_lake_store_account>.azuredatalakestore.net/

    Ez biztosítania kell a fájlok és mappák a Data Lake Store-fiók hello listáját.

### <a name="import-data-from-azure-sql-database-into-data-lake-store"></a>Adatokat importálhat az Azure SQL Database Data Lake Store
1. Keresse meg a toohello könyvtár, ahol a Sqoop csomagok érhetők el. Általában ez lesz `/usr/hdp/<version>/sqoop/bin`.
2. Hello adatokat importálni **Table1** hello Data Lake Store-fiók be. A következő szintaxist hello használata:

        sqoop-import --connect "jdbc:sqlserver://<sql-database-server-name>.database.windows.net:1433;username=<username>@<sql-database-server-name>;password=<password>;database=<sql-database-name>" --table Table1 --target-dir adl://<data-lake-store-name>.azuredatalakestore.net/Sqoop/SqoopImportTable1

    Vegye figyelembe, hogy **sql-adatbázis-kiszolgálónév** helyőrző hello hello hello Azure SQL-adatbázist futtató kiszolgáló nevét. **SQL-adatbázis-neve** helyőrző hello tényleges adatbázis neve.

    Például:


        sqoop-import --connect "jdbc:sqlserver://mysqoopserver.database.windows.net:1433;username=nitinme@mysqoopserver;password=<password>;database=mysqoopdatabase" --table Table1 --target-dir adl://myadlstore.azuredatalakestore.net/Sqoop/SqoopImportTable1

1. Győződjön meg arról, hogy korábban már hello átvitt toohello Data Lake Store-fiók. Futtassa a következő parancs hello:

        hdfs dfs -ls adl://hdiadlstore.azuredatalakestore.net/Sqoop/SqoopImportTable1/

    A következő kimeneti hello kell megjelennie.


        -rwxrwxrwx   0 sshuser hdfs          0 2016-02-26 21:09 adl://hdiadlstore.azuredatalakestore.net/Sqoop/SqoopImportTable1/_SUCCESS
        -rwxrwxrwx   0 sshuser hdfs         12 2016-02-26 21:09 adl://hdiadlstore.azuredatalakestore.net/Sqoop/SqoopImportTable1/part-m-00000
        -rwxrwxrwx   0 sshuser hdfs         14 2016-02-26 21:09 adl://hdiadlstore.azuredatalakestore.net/Sqoop/SqoopImportTable1/part-m-00001
        -rwxrwxrwx   0 sshuser hdfs         13 2016-02-26 21:09 adl://hdiadlstore.azuredatalakestore.net/Sqoop/SqoopImportTable1/part-m-00002
        -rwxrwxrwx   0 sshuser hdfs         18 2016-02-26 21:09 adl://hdiadlstore.azuredatalakestore.net/Sqoop/SqoopImportTable1/part-m-00003

    Minden egyes **rész-m -*** fájl megfelel tooa sor hello forrástáblában, **Table1**. Megtekintheti a hello rész - m - hello tartalmát * tooverify fájlok.


### <a name="export-data-from-data-lake-store-into-azure-sql-database"></a>Adatok exportálása az Azure SQL Database Data Lake Store-ból
1. A Data Lake Store fiók toohello üres táblából, hello adatok exportálása **Table2**, az Azure SQL Database hello. A következő szintaxist hello használata.

        sqoop-export --connect "jdbc:sqlserver://<sql-database-server-name>.database.windows.net:1433;username=<username>@<sql-database-server-name>;password=<password>;database=<sql-database-name>" --table Table2 --export-dir adl://<data-lake-store-name>.azuredatalakestore.net/Sqoop/SqoopImportTable1 --input-fields-terminated-by ","

    Például:


        sqoop-export --connect "jdbc:sqlserver://mysqoopserver.database.windows.net:1433;username=nitinme@mysqoopserver;password=<password>;database=mysqoopdatabase" --table Table2 --export-dir adl://myadlstore.azuredatalakestore.net/Sqoop/SqoopImportTable1 --input-fields-terminated-by ","

1. Győződjön meg arról, hogy hello adatok lett feltöltve toohello SQL-adatbázis táblája. Használjon [SQL Server Management Studio](../sql-database/sql-database-connect-query-ssms.md) vagy a Visual Studio tooconnect toohello Azure SQL Database és a lekérdezés futtatása hello kövesse.

        SELECT * FROM TABLE2

    Ez a következő kimeneti hello kell rendelkeznie.

         ID  FName   LName
        ------------------
        1    Neal    Kell
        2    Lila    Fulton
        3    Erna    Myers
        4    Annette    Simpson

## <a name="performance-considerations-while-using-sqoop"></a>Sqoop használatakor a teljesítménnyel kapcsolatos szempontok

Teljesítményhangolás a Sqoop toocopy adattár tooData Lake feladat, a következő témakörben: [Sqoop teljesítmény dokumentum](https://blogs.msdn.microsoft.com/bigdatasupport/2015/02/17/sqoop-job-performance-tuning-in-hdinsight-hadoop/).

## <a name="see-also"></a>Lásd még:
* [Adatok másolása az Azure Storage Blobs tooData Lake Store-ból](data-lake-store-copy-data-azure-storage-blob.md)
* [Biztonságos adattárolás a Data Lake Store-ban](data-lake-store-secure-data.md)
* [Az Azure Data Lake Analytics használata a Data Lake Store-ral](../data-lake-analytics/data-lake-analytics-get-started-portal.md)
* [Az Azure HDInsight használata a Data Lake Store-ral](data-lake-store-hdinsight-hadoop-use-portal.md)
