---
title: a Hadoop - Azure HDInsight Sqoop aaaApache |} Microsoft Docs
description: "Megtudhatja, hogyan toouse Apache Sqoop tooimport és exportálás a HDInsight Hadoop és egy Azure SQL-adatbázis között."
editor: cgronlun
manager: jhubbard
services: hdinsight
documentationcenter: 
author: Blackmist
tags: azure-portal
keywords: hadoop sqoop, sqoop
ms.assetid: 303649a5-4be5-4933-bf1d-4b232083c354
ms.service: hdinsight
ms.custom: hdinsightactive,hdiseo17may2017
ms.workload: big-data
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/19/2017
ms.author: larryfr
ms.openlocfilehash: b256285659bbcf18ff05e220ccdf51c21eb8fbf7
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="use-apache-sqoop-tooimport-and-export-data-between-hadoop-on-hdinsight-and-sql-database"></a>Apache Sqoop tooimport használja, és a HDInsight Hadoop és SQL-adatbázis közötti adatok exportálása

[!INCLUDE [sqoop-selector](../../includes/hdinsight-selector-use-sqoop.md)]

Ismerje meg, hogyan toouse Apache Sqoop tooimport között egy Hadoop exportálási fürtön és az Azure HDInsight és az Azure SQL Database vagy Microsoft SQL Server adatbázis. Ez a dokumentum használatát hello lépései hello `sqoop` közvetlenül hello headnode hello Hadoop-fürt parancsot. SSH tooconnect toohello átjárócsomópont használ, és ez a dokumentum hello parancsok futtatását.

> [!IMPORTANT]
> hello ebben a dokumentumban csak a lépések Linux használó HDInsight-fürtökkel. Linux hello azt az egyetlen operációs rendszer, használja a HDInsight 3.4 vagy újabb verziója. További tudnivalókért lásd: [A HDInsight elavulása Windows rendszeren](hdinsight-component-versioning.md#hdinsight-windows-retirement).

## <a name="install-freetds"></a>FreeTDS telepítése

1. SSH tooconnect toohello HDInsight-fürt használatára. Például a következő parancs hello csatlakozik toohello elsődleges headnode nevű fürt `mycluster`:

    ```bash
    ssh CLUSTERNAME-ssh.azurehdinsight.net
    ```

    További információ: [Az SSH használata HDInsighttal](hdinsight-hadoop-linux-use-ssh-unix.md).

2. A következő parancs tooinstall FreeTDS hello használata:

    ```bash
    sudo apt --assume-yes install freetds-dev freetds-bin
    ```

    FreeTDS több lépéseket tooconnect tooSQL adatbázis használatban van.

## <a name="create-hello-table-in-sql-database"></a>Az SQL-adatbázis hello tábla létrehozása

> [!IMPORTANT]
> Ha hello HDInsight-fürtöt használ, és SQL-adatbázis létrehozása [fürt és az SQL-adatbázis létrehozása](hdinsight-use-sqoop.md), hagyja ki a jelen szakaszban szereplő hello lépéseket. hello adatbázis és tábla hoztak létre, hello részét szükséges lépések hello [fürt és az SQL-adatbázis létrehozása](hdinsight-use-sqoop.md) dokumentum.

1. Hello SSH-munkamenetet használja a következő parancs tooconnect toohello SQL adatbázis-kiszolgáló hello.

        TDSVER=8.0 tsql -H <serverName>.database.windows.net -U <adminLogin> -P <adminPassword> -p 1433 -D sqooptest

    Kimeneti hasonló toohello a következő szöveg jelenik meg:

        locale is "en_US.UTF-8"
        locale charset is "UTF-8"
        using default charset "UTF-8"
        Default database being set toosqooptest
        1>

2. A hello `1>` kéri, adja meg a következő lekérdezés hello:

    ```sql
    CREATE TABLE [dbo].[mobiledata](
    [clientid] [nvarchar](50),
    [querytime] [nvarchar](50),
    [market] [nvarchar](50),
    [deviceplatform] [nvarchar](50),
    [devicemake] [nvarchar](50),
    [devicemodel] [nvarchar](50),
    [state] [nvarchar](50),
    [country] [nvarchar](50),
    [querydwelltime] [float],
    [sessionid] [bigint],
    [sessionpagevieworder] [bigint])
    GO
    CREATE CLUSTERED INDEX mobiledata_clustered_index on mobiledata(clientid)
    GO
    ```

    Ha hello `GO` utasításban is meg kell adni, hello előző utasítás kiértékelése. Első lépésként hello **mobiledata** tábla létrehozása, majd egy fürtözött index kerül tooit (SQL-adatbázis szükséges.)

    A következő lekérdezés tooverify, amelyek a tábla hello használata hello jött létre:

    ```sql
    SELECT * FROM information_schema.tables
    GO
    ```

    Kimeneti hasonló toohello a következő szöveg jelenik meg:

        TABLE_CATALOG   TABLE_SCHEMA    TABLE_NAME      TABLE_TYPE
        sqooptest       dbo     mobiledata      BASE TABLE

3. Adja meg `exit` : hello `1>` tooexit hello tsql segédprogram kérni.

## <a name="sqoop-export"></a>Sqoop exportálása

1. Hello SSH kapcsolat toohello fürtből a következő parancs tooverify, hogy a Sqoop látja-e az SQL-adatbázis hello használata:

    ```bash
    sqoop list-databases --connect jdbc:sqlserver://<serverName>.database.windows.net:1433 --username <adminLogin> -P
    ```
    Amikor a rendszer kéri, adja meg hello jelszó hello SQL adatbázis-bejelentkezési adatokat.

    Ez a parancs visszaadja az adatbázisok, beleértve a hello listája **sqooptest** korábban létrehozott adatbázis.

2. tooexport adatait **hivesampletable** toohello **mobiledata** table, a következő parancs hello használata:

    ```bash
    sqoop export --connect 'jdbc:sqlserver://<serverName>.database.windows.net:1433;database=sqooptest' --username <adminLogin> -P --table 'mobiledata' --export-dir 'wasb:///hive/warehouse/hivesampletable' --fields-terminated-by '\t' -m 1
    ```

    Ez a parancs utasítja a Sqoop tooconnect toohello **sqooptest** adatbázis. Sqoop majd exportálja az adatokat a **wasb: / / / hive/adatraktár/hivesampletable** toohello **mobiledata** tábla.

    > [!IMPORTANT]
    > Használjon `wasb:///` Ha hello alapértelmezett tároló a fürt számára egy Azure Storage-fiókot. Használjon `adl:///` egy Azure Data Lake Store esetén.

3. Hello parancs befejezése után használja a következő parancs tooconnect toohello adatbázis TSQL használatával hello:

    ```bash
    TDSVER=8.0 tsql -H <serverName>.database.windows.net -U <adminLogin> -P -p 1433 -D sqooptest
    ```

    A csatlakozás után a következő utasítások tooverify, amely adatok hello használata hello lett-e az exportált toohello **mobiledata** tábla:

    ```sql
    SET ROWCOUNT 50;
    SELECT * FROM mobiledata
    GO
    ```

    Meg kell jelennie hello tábla listáját. Típus `exit` tooexit hello tsql segédprogram.

## <a name="sqoop-import"></a>Sqoop importálása

1. Használjon hello következő parancsot a hello tooimport adatait **mobiledata** tábla az SQL-adatbázis, toohello **wasb: / / / oktatóanyagok/usesqoop/importeddata** HDInsight könyvtárába:

    ```bash
    sqoop import --connect 'jdbc:sqlserver://<serverName>.database.windows.net:1433;database=sqooptest' --username <adminLogin> --password <adminPassword> --table 'mobiledata' --target-dir 'wasb:///tutorials/usesqoop/importeddata' --fields-terminated-by '\t' --lines-terminated-by '\n' -m 1
    ```

    hello adatok mezőinek hello tabulátor választják el, és hello sorok egy új sor karakter megszűnik.

2. Hello importálás befejezése után használja a következő parancs toolist kimenő hello hello könyvtára hello:

    ```bash
    hdfs dfs -text /tutorials/usesqoop/importeddata/part-m-00000
    ```

## <a name="using-sql-server"></a>SQL Server használata

Is használhat a Sqoop tooimport és exportál adatokat az SQL Server, vagy az adatközpont, vagy az Azure-ban futó virtuális gépen. SQL Database és SQL Server használata között hello különbségek a következők:

* HDInsight és az SQL Server kell lennie a hello azonos Azure virtuális hálózatban.

    Egy vonatkozó példáért lásd: hello [csatlakozás HDInsight tooyour a helyszíni hálózat](./connect-on-premises-network.md) dokumentum.

    A HDInsight használata az Azure virtuális hálózat további információkért lásd: hello [kiterjesztése HDInsight az Azure Virtual Network](hdinsight-extend-hadoop-virtual-network.md) dokumentum. Azure-beli virtuális hálózatra vonatkozó további információkért lásd: hello [virtuális hálózat áttekintése](../virtual-network/virtual-networks-overview.md) dokumentum.

* SQL Server konfigurált tooallow SQL-hitelesítést kell lennie. További információkért lásd: hello [válassza ki a kívánt hitelesítési mód](https://msdn.microsoft.com/ms144284.aspx) dokumentum.

* Előfordulhat, hogy tooconfigure SQL Server tooaccept távoli kapcsolatokat. További információkért lásd: hello [hogyan tootroubleshoot csatlakozó toohello SQL Server adatbázis-motor](http://social.technet.microsoft.com/wiki/contents/articles/2102.how-to-troubleshoot-connecting-to-the-sql-server-database-engine.aspx) dokumentum.

* Hozzon létre hello **sqooptest** egy segédprogram segítségével például SQL Server adatbázis **SQL Server Management Studio** vagy **tsql**. hello hello Azure CLI használatára vonatkozó csak a lépések az Azure SQL Database.

    A következő Transact-SQL utasítás toocreate hello használata hello **mobiledata** tábla:

    ```sql
    CREATE TABLE [dbo].[mobiledata](
    [clientid] [nvarchar](50),
    [querytime] [nvarchar](50),
    [market] [nvarchar](50),
    [deviceplatform] [nvarchar](50),
    [devicemake] [nvarchar](50),
    [devicemodel] [nvarchar](50),
    [state] [nvarchar](50),
    [country] [nvarchar](50),
    [querydwelltime] [float],
    [sessionid] [bigint],
    [sessionpagevieworder] [bigint])
    ```

* Ha toohello SQL Server a HDInsight-ból, előfordulhat, hogy az SQL Server hello toouse hello IP-címét. Példa:

    ```bash
    sqoop import --connect 'jdbc:sqlserver://10.0.1.1:1433;database=sqooptest' --username <adminLogin> --password <adminPassword> --table 'mobiledata' --target-dir 'wasb:///tutorials/usesqoop/importeddata' --fields-terminated-by '\t' --lines-terminated-by '\n' -m 1
    ```

## <a name="limitations"></a>Korlátozások

* Tömeges export - a Linux-alapú HDInsight, hello Sqoop használt tooexport adatok tooMicrosoft SQL Server vagy az Azure SQL Database jelenleg nem támogatja a tömeges beszúrások.

* Kötegelés – a Linux-alapú hdinsight eszközzel, hello használatakor `-batch` Beszúrások végrehajtásakor kapcsoló, a Sqoop lehetővé teszi több Beszúrások hello beszúrási műveletek kötegelése helyett.

## <a name="next-steps"></a>Következő lépések

Most megtanulta, hogyan toouse Sqoop. toolearn több, lásd:

* [Oozie használata a HDInsight][hdinsight-use-oozie]: egy Oozie munkafolyamat használja Sqoop műveletét.
* [HDInsight eszközzel repülési késleltetés adatok elemzése][hdinsight-analyze-flight-data]: tooanalyze felhőszolgáltató közötti átviteléhez használja Hive késleltetés az adatok, és majd a Sqoop tooexport tooan Azure SQL-adatbázis használata.
* [Töltse fel az adatok tooHDInsight][hdinsight-upload-data]: található adatok tooHDInsight vagy az Azure Blob storage feltöltése más módszerrel.

[hdinsight-versions]:  hdinsight-component-versioning.md
[hdinsight-provision]: hdinsight-hadoop-provision-linux-clusters.md
[hdinsight-get-started]: hdinsight-hadoop-linux-tutorial-get-started.md
[hdinsight-storage]: ../hdinsight-hadoop-use-blob-storage.md
[hdinsight-analyze-flight-data]: hdinsight-analyze-flight-delay-data.md
[hdinsight-use-oozie]: hdinsight-use-oozie.md
[hdinsight-upload-data]: hdinsight-upload-data.md
[hdinsight-submit-jobs]: hdinsight-submit-hadoop-jobs-programmatically.md

[sqldatabase-get-started]: ../sql-database-get-started.md
[sqldatabase-create-configue]: ../sql-database-create-configure.md

[powershell-start]: http://technet.microsoft.com/library/hh847889.aspx
[powershell-install]: /powershell/azureps-cmdlets-docs
[powershell-script]: http://technet.microsoft.com/library/ee176949.aspx

[sqoop-user-guide-1.4.4]: https://sqoop.apache.org/docs/1.4.4/SqoopUserGuide.html
