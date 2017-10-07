---
title: "a Hadoop aaaRun feladat Azure Cosmos DB és HDInsight használatával |} Microsoft Docs"
description: "Ismerje meg, hogyan toorun egy egyszerű Hive, Pig és MapReduce feladat Azure Cosmos DB és az Azure HDInsight."
services: cosmos-db
author: dennyglee
manager: jhubbard
editor: mimig
documentationcenter: 
ms.assetid: 06f0ea9d-07cb-4593-a9c5-ab912b62ac42
ms.service: cosmos-db
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: java
ms.topic: article
ms.date: 06/08/2017
ms.author: denlee
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 2e27499f2c4ba951af9a1ade1bcc9c1b6d298fcd
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="Azure Cosmos DB-HDInsight"></a>Egy Azure Cosmos DB és HDInsight Apache Hive, Pig vagy Hadoop feladat futtatása
Az oktatóanyag bemutatja, hogyan toorun [Apache Hive][apache-hive], [Apache Pig][apache-pig], és [Apache Hadoop] [ apache-hadoop] MapReduce-feladatok on Azure HDInsight Cosmos DB Hadoop-összekötővel együtt. Cosmos DB a Hadoop-összekötő lehetővé teszi, hogy a forrás-és a Hive, Pig és MapReduce-feladatok fogadó Cosmos DB tooact. Ez az oktatóanyag hello adatforrás és a célként megadott Cosmos DB Hadoop-feladatokat használja.

Ez az oktatóanyag befejezése után be fog tudni tooanswer hello a következő kérdéseket:

* Hogyan betölteni az adatokat a Hive, Pig vagy MapReduce feladat használatával Cosmos-Adatbázisból?
* Hogyan adat tárolása Cosmos DB használatával a Hive, Pig vagy MapReduce feladatot?

Azt javasoljuk, hogy Kezdésként tekintse a következő videó, ahol azt végigfuttatása egy Cosmos-adatbázis és a HDInsight Hive-feladat hello.

> [!VIDEO https://channel9.msdn.com/Blogs/Azure/Use-Azure-DocumentDB-Hadoop-Connector-with-Azure-HDInsight/player]
>
>

Ezt követően térjen vissza toothis cikk, ahol értesítjük, hogyan futtathat analytics-feladatok a Cosmos DB adatai a hello részletesen.

> [!TIP]
> Ez az oktatóanyag feltételezi, hogy rendelkezik-e előzetes tapasztalata az Apache Hadoop, a Hive és/vagy a Pig használatával kapcsolatban. Ha új tooApache Hadoop Hive és a Pig, ajánlott hello látogató [Apache Hadoop-dokumentáció][apache-hadoop-doc]. Ez az oktatóanyag azt is feltételezi, hogy rendelkezik tapasztalattal a Cosmos DB, és Cosmos DB fiókkal rendelkezik. Ha új tooCosmos DB, vagy Ön nem rendelkezik egy Cosmos-DB-fiókot, vegye ki a [bevezetés] [ getting-started] lap.
>
>

Nincs ideje toocomplete hello oktatóanyag, és csak szeretné a Hive, Pig és MapReduce tooget hello a teljes minta PowerShell-parancsfájlok? Nem probléma, azokat [Itt][hdinsight-samples]. hello letöltési hello hql, a pig és a java fájljait a következő mintákat is tartalmazza.

## <a name="NewestVersion"></a>Legújabb verziója
<table border='1'>
    <tr><th>Hadoop Connector ezen verziója</th>
        <td>1.2.0</td></tr>
    <tr><th>Parancsfájl URI azonosítója</th>
        <td>https://portalcontent.BLOB.Core.Windows.NET/scriptaction/documentdb-hadoop-Installer-v04.ps1</td></tr>
    <tr><th>Módosítás dátuma</th>
        <td>04/26/2016</td></tr>
    <tr><th>Támogatott HDInsight-verziókról</th>
        <td>3.1, 3.2</td></tr>
    <tr><th>Módosítási napló</th>
        <td>Az Azure Cosmos DB Java SDK too1.6.0 frissítése</br>
            A forrás-és a fogadó particionált gyűjtemények támogatása</br>
        </td></tr>
</table>

## <a name="Prerequisites"></a>Előfeltételek
Mielőtt hozzálátna hello utasításokat ebben az oktatóanyagban, ellenőrizze, hogy hello következő:

* Egy Cosmos-DB-fiók, adatbázis és a dokumentumok egy gyűjtemény. További információkért lásd: [Ismerkedés a Cosmos DB][getting-started]. Minta adatok importálása hello Cosmos DB fiókját [Cosmos DB import eszközt][import-data].
* Átviteli sebesség. Beolvassa és a HDInsight-ból írások beleszámítanak a meghatározott időn belül kérelemegység a gyűjteményekben felé.
* Egyes belül egy további tárolt eljárást a kapacitás kimeneti gyűjtemény. hello tárolt eljárások az eredményül kapott dokumentumok átvitele használnak.
* Hello eredményül kapott dokumentumok hello Hive, Pig vagy MapReduce-feladatok kapacitása.
* [*Nem kötelező*] kapacitásának meghatározása egy további gyűjteményt.

> [!WARNING]
> A sorrend tooavoid hello létrehozási hello feladatok az új gyűjtemény vagy hello eredmények toostdout nyomtatása, elmentheti hello kimeneti tooyour WASB tároló, vagy adjon meg egy már létező gyűjteményt. Hello esetben adja meg egy meglévő gyűjteményt, új dokumentumok hello gyűjtemény belül létrejön, és már meglévő dokumentumokat csak az érintett ütközése esetén *azonosítók*. **hello összekötő automatikusan felülírja meglévő dokumentumok ütközését**. Ez a szolgáltatás úgy, hogy hello upsert beállítás toofalse kikapcsolható. Ha upsert értéke false, és ütközés lép fel, hello Hadoop-feladat meghiúsul; egy azonosító ütközés hibát jelez.
>
>

## <a name="ProvisionHDInsight"></a>1. lépés:, Hozzon létre egy új HDInsight-fürt
Ez az oktatóanyag használ parancsfájlművelet a hello Azure Portal toocustomize a HDInsight-fürthöz. Ebben az oktatóanyagban használjuk hello Azure Portal toocreate a HDInsight-fürthöz. Útmutatásért hogyan toouse PowerShell-parancsmagok vagy hello HDInsight .NET SDK-t, tekintse meg a [testreszabása HDInsight-fürtök használata parancsfájlművelet] [ hdinsight-custom-provision] cikk.

1. Jelentkezzen be toohello [Azure Portal][azure-portal].
2. Kattintson a **+ új** keresése a bal oldali navigációs hello hello felső részén **HDInsight** hello felső keresősávban hello új panelen.
3. **HDInsight** által közzétett **Microsoft** hello eredmények hello tetején fog megjelenni. Kattintson rá, és kattintson a **létrehozása**.
4. Hozzon létre új HDInsight-fürt hello panel, adja meg a **fürtnév** és select hello **előfizetés** ehhez az erőforráshoz a tooprovision szeretné.

    <table border='1'>
        <tr><td>Fürt neve</td><td>Hello fürt nevét.<br/>
DNS-beli név kell elindítani egy alfanumerikus karakterrel végződik és kötőjeleket tartalmazhat.<br/>
hello mező 3 és 63 karakter közötti karakterláncnak kell lennie.</td></tr>
        <tr><td>Előfizetés neve</td>
            <td>Ha több Azure-előfizetéssel rendelkezik, válassza ki a hello-előfizetéssel, amely a HDInsight-fürt tárolására. </td></tr>
    </table>
5.Kattintson a **fürt típusának kiválasztása** és a következő tulajdonságok toohello set hello megadott értéket.

    <table border='1'>
        <tr><td>Fürttípus</td><td><strong>Hadoop</strong></td></tr>
        <tr><td>Fürt réteg</td><td><strong>Standard</strong></td></tr>
        <tr><td>Operációs rendszer</td><td><strong>Windows</strong></td></tr>
        <tr><td>Verzió</td><td>legújabb verzió</td></tr>
    </table>

    Most kattintson **válasszon**.

    ![Adja meg a Hadoop HDInsight a fürtcsomópontok kezdeti adatait][image-customprovision-page1]
6. Kattintson a **hitelesítő adatok** tooset a bejelentkezési és távelérési hitelesítő adatainak. Válassza ki a **a fürt bejelentkezési felhasználónevének** és **a fürt bejelentkezési jelszó**.

    Ha tooremote a fürtbe, jelölje be *Igen* hello hello panel alsó részén, és adja meg a felhasználónevet és jelszót.
7. Kattintson a **adatforrás** tooset az elsődleges helyet adatok eléréséhez. Válassza ki a hello **kijelöléséről** , és adjon meg egy már meglévő tárfiókot, vagy hozzon létre egy újat.
8. Az azonos panel hello, adjon meg egy **alapértelmezett tároló** és egy **hely**. Majd kattintson a gombra **válasszon**.

   > [!NOTE]
   > Válassza ki a hely Bezárás tooyour Cosmos DB régióját a jobb teljesítmény
   >
   >
9. Kattintson a **árazás** tooselect hello számát és típusát a csomópontok. Beállíthatja, hogy hello alapértelmezett konfiguráció és a feldolgozó csomópontok száma méretezési hello később.
10. Kattintson a **opcionális konfigurációs**, majd **Parancsfájlműveletek** hello opcionális konfigurációs panelen található.

     A Parancsfájlműveletek adja meg a következő információk toocustomize hello a HDInsight-fürthöz.

     <table border='1'>
         <tr><th>Tulajdonság</th><th>Érték</th></tr>
         <tr><td>Név</td>
             <td>Adja meg a hello parancsfájlművelet nevét.</td></tr>
         <tr><td>A parancsfájl URI azonosítója</td>
             <td>Hello URI toohello parancsfájl megadásához, amely meghívott toocustomize hello fürt.</br></br>
Adja meg: </br> <strong>https://portalcontent.BLOB.Core.Windows.NET/scriptaction/documentdb-hadoop-Installer-v04.ps1</strong>.</td></tr>
         <tr><td>Fej</td>
             <td>Kattintson a hello jelölőnégyzet toorun hello hello átjárócsomópont PowerShell parancsfájlt.</br></br>
             <strong>Ezt a jelölőnégyzetet</strong>.</td></tr>
         <tr><td>Munkavégző</td>
             <td>Kattintson a hello jelölőnégyzet toorun hello PowerShell parancsfájlt hello munkavégző csomópont.</br></br>
             <strong>Ezt a jelölőnégyzetet</strong>.</td></tr>
         <tr><td>Zookeeper</td>
             <td>Kattintson a hello jelölőnégyzet toorun hello PowerShell parancsfájl hello Zookeeper-kiszolgálóra.</br></br>
             <strong>Nincs szükség</strong>.
             </td></tr>
         <tr><td>Paraméterek</td>
             <td>Adja meg a hello paraméterek hello parancsfájl által szükség esetén.</br></br>
             <strong>Nem szükséges paramétereket</strong>.</td></tr>
     </table>
11.Vagy hozzon létre egy új **erőforráscsoport** , vagy használjon egy meglévő erőforráscsoportot az Azure-előfizetéshez tartozó.
12. Ellenőrizze, **PIN-kód toodashboard** tootrack kattintson, és a központi telepítési **létrehozása**!

## <a name="InstallCmdlets"></a>2. lépés: Telepítse és konfigurálja az Azure PowerShell
1. Az Azure PowerShell telepítése. Útmutatás található [Itt][powershell-install-configure].

   > [!NOTE]
   > Azt is megteheti csak a Hive-lekérdezéseket, használhatja a HDInsight tartozó online Hive szerkesztő. toodo tehát bejelentkezés toohello [Azure Portal][azure-portal], kattintson a **HDInsight** on hello bal oldali ablaktáblán tooview a HDInsight-fürtök listáját. Kattintson a kívánt toorun Hive-lekérdezéseket, és kattintson a hello fürt **lekérdezés konzol**.
   >
   >
2. Nyissa meg az Azure PowerShell integrált parancsfájlkezelési környezet hello:

   * A Windows 8 vagy Windows Server 2012 vagy újabb rendszerű számítógépen is használhatja hello beépített keresési. Hello kezdőképernyőről írja be a **powershell ise** kattintson **Enter**.
   * A Windows 8 vagy Windows Server 2012-nél korábbi rendszerű számítógépen hello Start menü használata. Hello Start menüben írja be a **parancssor** hello keresési mezőbe, majd az eredmények hello listában, kattintson a **parancssor**. Hello parancssort, írja be **powershell_ise** kattintson **Enter**.
3. Adja hozzá az Azure-fiókjával.

   1. A konzol ablaktáblában hello, írja be **Add-AzureAccount** kattintson **Enter**.
   2. Írja be az Azure-előfizetéshez társított hello e-mail címét, és kattintson a **Folytatás**.
   3. Adja meg az Azure-előfizetéshez hello jelszavát.
   4. Kattintson a **bejelentkezés**.
4. a következő diagram hello hello fontos részek az Azure PowerShell-parancsfájl-kezelési környezet azonosítja.

    ![Az Azure PowerShell diagramja][azure-powershell-diagram]

## <a name="RunHive"></a>3. lépés: Egy Cosmos-adatbázis és a HDInsight Hive-feladat futtatása
> [!IMPORTANT]
> < > Által jelzett minden értéket meg kell adni a konfigurációs beállítások használata.
>
>

1. A következő változók a PowerShell-parancsfájl ablaktáblán hello beállítása.

        # Provide Azure subscription name, hello Azure Storage account and container that is used for hello default HDInsight file system.
        $subscriptionName = "<SubscriptionName>"
        $storageAccountName = "<AzureStorageAccountName>"
        $containerName = "<AzureStorageContainerName>"

        # Provide hello HDInsight cluster name where you want toorun hello Hive job.
        $clusterName = "<HDInsightClusterName>"
2. <p>Most először hoz létre, a lekérdezési karakterlánc. Hive lekérdezés összes dokumentumot (_ts) rendszer által létrehozott időbélyegeket és egyedi azonosítók (_rid) egy Azure Cosmos DB gyűjteményből, összes dokumentumot számolja hello percenként, majd tárolja egy új Azure Cosmos DB gyűjteménybe hello eredmények azt fog írni.</p>

    <p>Először hozzon létre olyan Hive táblát az Azure Cosmos DB gyűjteményből. Adja hozzá a következő kód részlet toohello PowerShell-parancsfájl ablaktábla hello <strong>után</strong> hello kódrészletet # 1. Ellenőrizze, hogy a dokumentumok toojust _ts és _rid hello választható DocumentDB.query paraméter t vágás telepíthet.</p>

   > [!NOTE]
   > **DocumentDB.inputCollections elnevezési nem hiba volt.** Igen, azt felvételének engedélyezése több gyűjtemény bemenetként: </br>
   >
   >

        '*DocumentDB.inputCollections*' = '*\<DocumentDB Input Collection Name 1\>*,*\<DocumentDB Input Collection Name 2\>*' A1A</br> hello collection names are separated without spaces, using only a single comma.

        # Create a Hive table using data from DocumentDB. Pass DocumentDB hello query toofilter transferred data too_rid and _ts.
        $queryStringPart1 = "drop table DocumentDB_timestamps; "  +
                            "create external table DocumentDB_timestamps(id string, ts BIGINT) "  +
                            "stored by 'com.microsoft.azure.documentdb.hive.DocumentDBStorageHandler' "  +
                            "tblproperties ( " +
                                "'DocumentDB.endpoint' = '<DocumentDB Endpoint>', " +
                                "'DocumentDB.key' = '<DocumentDB Primary Key>', " +
                                "'DocumentDB.db' = '<DocumentDB Database Name>', " +
                                "'DocumentDB.inputCollections' = '<DocumentDB Input Collection Name>', " +
                                "'DocumentDB.query' = 'SELECT r._rid AS id, r._ts AS ts FROM root r' ); "

1. Következő lépésként hozzon létre egy Hive tábla hello kimeneti gyűjtemény. hello kimeneti dokumentumtulajdonságok lesz hello hónap, nap, óra, perc és hello előfordulások száma.

   > [!NOTE]
   > **Még újra DocumentDB.outputCollections elnevezési nem hiba volt.** Igen, azt felvételének engedélyezése több gyűjtemény kimenetként: </br>
   > "*DocumentDB.outputCollections*'='*\<DocumentDB-gyűjtemény Kimenetnév 1\>*,*\<DocumentDB-gyűjtemény Kimenetnév 2\>*" </br> hello gyűjteménynevek nélkül szóközt tartalmaz, csak egyetlen vesszővel válassza el egymástól. </br></br>
   > Dokumentumok lesz elosztott ciklikus multiplexelés több gyűjtemények között. A kötegelt dokumentumok fogja tárolni egy gyűjteményt, majd a második kötegelt dokumentumok fogja tárolni hello tovább gyűjteményben, és így tovább.
   >
   >

       # Create a Hive table for hello output data tooDocumentDB.
       $queryStringPart2 = "drop table DocumentDB_analytics; " +
                             "create external table DocumentDB_analytics(Month INT, Day INT, Hour INT, Minute INT, Total INT) " +
                             "stored by 'com.microsoft.azure.documentdb.hive.DocumentDBStorageHandler' " +
                             "tblproperties ( " +
                                 "'DocumentDB.endpoint' = '<DocumentDB Endpoint>', " +
                                 "'DocumentDB.key' = '<DocumentDB Primary Key>', " +  
                                 "'DocumentDB.db' = '<DocumentDB Database Name>', " +
                                 "'DocumentDB.outputCollections' = '<DocumentDB Output Collection Name>' ); "
2. Végezetül most egyeztetési hello dokumentumok által hónap, nap, óra és perc és a Beszúrás hello eredmények hello programba kimeneti Hive tábla.

        # GROUP BY minute, COUNT entries for each, INSERT INTO output Hive table.
        $queryStringPart3 = "INSERT INTO table DocumentDB_analytics " +
                              "SELECT month(from_unixtime(ts)) as Month, day(from_unixtime(ts)) as Day, " +
                              "hour(from_unixtime(ts)) as Hour, minute(from_unixtime(ts)) as Minute, " +
                              "COUNT(*) AS Total " +
                              "FROM DocumentDB_timestamps " +
                              "GROUP BY month(from_unixtime(ts)), day(from_unixtime(ts)), " +
                              "hour(from_unixtime(ts)) , minute(from_unixtime(ts)); "
3. Adja hozzá a hello parancsfájl részlet toocreate Hive feladat definícióját követő hello előző lekérdezésből.

        # Create a Hive job definition.
        $queryString = $queryStringPart1 + $queryStringPart2 + $queryStringPart3
        $hiveJobDefinition = New-AzureHDInsightHiveJobDefinition -Query $queryString

    Is hello - fájl kapcsoló toospecify egy HDFS a HiveQL-parancsfájlt.
4. Adja hozzá a következő kódrészletet toosave hello kezdési ideje hello és hello Hive feladat elküldéséhez.

        # Save hello start time and submit hello job toohello cluster.
        $startTime = Get-Date
        Select-AzureSubscription $subscriptionName
        $hiveJob = Start-AzureHDInsightJob -Cluster $clusterName -JobDefinition $hiveJobDefinition
5. Adja hozzá a következő toowait hello Hive feladat toocomplete a hello.

        # Wait for hello Hive job toocomplete.
        Wait-AzureHDInsightJob -Job $hiveJob -WaitTimeoutInSeconds 3600
6. Adja hozzá a következő szabványos-es számú tooprint hello kimeneti és hello rendszerhez készült start hello és befejezési időpontja.

        # Print hello standard error, hello standard output of hello Hive job, and hello start and end time.
        $endTime = Get-Date
        Get-AzureHDInsightJobOutput -Cluster $clusterName -JobId $hiveJob.JobId -StandardOutput
        Write-Host "Start: " $startTime ", End: " $endTime -ForegroundColor Green
7. **Futtatás** az új parancsfájl! **Kattintson a** hello zöld végrehajtás gombra.
8. Hello eredmények ellenőrzése. Jelentkezzen be a hello [Azure Portal][azure-portal].

   1. Kattintson a <strong>Tallózás</strong> hello bal oldali panelen. </br>
   2. Kattintson a <strong>mindent</strong> hello felső – jobb hello Tallózás panel. </br>
   3. Keresse meg és kattintson a <strong>Azure Cosmos DB fiókok</strong>. </br>
   4. Ezt követően keresse meg a <strong>Azure Cosmos DB fiók</strong>, majd <strong>Azure Cosmos DB adatbázis</strong> és a <strong>Azure Cosmos DB gyűjtemény</strong> megadott hello kimeneti gyűjteményhez társított a Hive-lekérdezést.</br>
   5. Végezetül kattintson <strong>dokumentumkezelő</strong> alatt <strong>fejlesztői eszközök</strong>.</br></p>

   A Hive-lekérdezések eredményeinek hello jelenik meg.

   ![Hive lekérdezés eredményei][image-hive-query-results]

## <a name="RunPig"></a>4. lépés: A Pig feladatot Cosmos DB és HDInsight használatával futtassa
> [!IMPORTANT]
> < > Által jelzett minden értéket meg kell adni a konfigurációs beállítások használata.
>
>

1. A következő változók a PowerShell-parancsfájl ablaktáblán hello beállítása.

        # Provide Azure subscription name.
        $subscriptionName = "Azure Subscription Name"

        # Provide HDInsight cluster name where you want toorun hello Pig job.
        $clusterName = "Azure HDInsight Cluster Name"
2. <p>Most először hoz létre, a lekérdezési karakterlánc. Pig lekérdezés összes dokumentumot (_ts) rendszer által létrehozott időbélyegeket és egyedi azonosítók (_rid) egy Azure Cosmos DB gyűjteményből, összes dokumentumot számolja hello percenként, majd tárolja egy új Azure Cosmos DB gyűjteménybe hello eredmények azt fog írni.</p>
    <p>Első lépésként betölteni a HDInsight a Cosmos-Adatbázisból dokumentumok. Adja hozzá a következő kód részlet toohello PowerShell-parancsfájl ablaktábla hello <strong>után</strong> hello kódrészletet # 1. Győződjön meg arról, hogy a dokumentumok toojust _ts és _rid tooadd a DocumentDB lekérdezése az toohello választható DocumentDB lekérdezési paraméter tootrim.</p>

   > [!NOTE]
   > Igen, azt felvételének engedélyezése több gyűjtemény bemenetként: </br>
   > "*\<DocumentDB bemeneti gyűjtemény neve 1\>*,*\<DocumentDB bemeneti gyűjteménynév 2\>*"</br> hello gyűjteménynevek nélkül szóközt tartalmaz, csak egyetlen vesszővel válassza el egymástól. </b>
   >
   >

    Dokumentumok lesz elosztott ciklikus multiplexelés több gyűjtemények között. A kötegelt dokumentumok fogja tárolni egy gyűjteményt, majd a második kötegelt dokumentumok fogja tárolni hello tovább gyűjteményben, és így tovább.

        # Load data from Cosmos DB. Pass DocumentDB query toofilter transferred data too_rid and _ts.
        $queryStringPart1 = "DocumentDB_timestamps = LOAD '<DocumentDB Endpoint>' USING com.microsoft.azure.documentdb.pig.DocumentDBLoader( " +
                                                        "'<DocumentDB Primary Key>', " +
                                                        "'<DocumentDB Database Name>', " +
                                                        "'<DocumentDB Input Collection Name>', " +
                                                        "'SELECT r._rid AS id, r._ts AS ts FROM root r' ); "
3. A következő most egyeztetési hello dokumentumok hello hónap, nap, óra, perc és hello előfordulások száma szerint.

       # GROUP BY minute and COUNT entries for each.
       $queryStringPart2 = "timestamp_record = FOREACH DocumentDB_timestamps GENERATE `$0#'id' as id:int, ToDate((long)(`$0#'ts') * 1000) as timestamp:datetime; " +
                           "by_minute = GROUP timestamp_record BY (GetYear(timestamp), GetMonth(timestamp), GetDay(timestamp), GetHour(timestamp), GetMinute(timestamp)); " +
                           "by_minute_count = FOREACH by_minute GENERATE FLATTEN(group) as (Year:int, Month:int, Day:int, Hour:int, Minute:int), COUNT(timestamp_record) as Total:int; "
4. Végezetül most hello eredmények tárolására az új kimeneti gyűjteménybe.

   > [!NOTE]
   > Igen, azt felvételének engedélyezése több gyűjtemény kimenetként: </br>
   > "\<DocumentDB-gyűjtemény Kimenetnév 1\>,\<DocumentDB-gyűjtemény Kimenetnév 2\>"</br> hello gyűjteménynevek nélkül szóközt tartalmaz, csak egyetlen vesszővel válassza el egymástól.</br>
   > Dokumentumok lesz több gyűjteményt kell elosztott ciklikus multiplexelés hello között. A kötegelt dokumentumok fogja tárolni egy gyűjteményt, majd a második kötegelt dokumentumok fogja tárolni hello tovább gyűjteményben, és így tovább.
   >
   >

        # Store output data tooCosmos DB.
        $queryStringPart3 = "STORE by_minute_count INTO '<DocumentDB Endpoint>' " +
                            "USING com.microsoft.azure.documentdb.pig.DocumentDBStorage( " +
                                "'<DocumentDB Primary Key>', " +
                                "'<DocumentDB Database Name>', " +
                                "'<DocumentDB Output Collection Name>'); "
5. Adja hozzá a következő parancsfájl részlet toocreate egy olyan feladatdefinícióban hello előző lekérdezésből hello.

        # Create a Pig job definition.
        $queryString = $queryStringPart1 + $queryStringPart2 + $queryStringPart3
        $pigJobDefinition = New-AzureHDInsightPigJobDefinition -Query $queryString -StatusFolder $statusFolder

    Is hello - fájl kapcsoló toospecify HDFS a Pig parancsfájl.
6. Adja hozzá a következő kódrészletet toosave hello kezdési ideje hello és elküldeni a Pig feladatot hello.

        # Save hello start time and submit hello job toohello cluster.
        $startTime = Get-Date
        Select-AzureSubscription $subscriptionName
        $pigJob = Start-AzureHDInsightJob -Cluster $clusterName -JobDefinition $pigJobDefinition
7. Adja hozzá a következő toowait hello Pig feladatot toocomplete a hello.

        # Wait for hello Pig job toocomplete.
        Wait-AzureHDInsightJob -Job $pigJob -WaitTimeoutInSeconds 3600
8. Adja hozzá a következő szabványos-es számú tooprint hello kimeneti és hello rendszerhez készült start hello és befejezési időpontja.

        # Print hello standard error, hello standard output of hello Hive job, and hello start and end time.
        $endTime = Get-Date
        Get-AzureHDInsightJobOutput -Cluster $clusterName -JobId $pigJob.JobId -StandardOutput
        Write-Host "Start: " $startTime ", End: " $endTime -ForegroundColor Green
9. **Futtatás** az új parancsfájl! **Kattintson a** hello zöld végrehajtás gombra.
10. Hello eredmények ellenőrzése. Jelentkezzen be a hello [Azure Portal][azure-portal].

    1. Kattintson a <strong>Tallózás</strong> hello bal oldali panelen. </br>
    2. Kattintson a <strong>mindent</strong> hello felső – jobb hello Tallózás panel. </br>
    3. Keresse meg és kattintson a <strong>Azure Cosmos DB fiókok</strong>. </br>
    4. Ezt követően keresse meg a <strong>Azure Cosmos DB fiók</strong>, majd <strong>Azure Cosmos DB adatbázis</strong> és a <strong>Azure Cosmos DB gyűjtemény</strong> megadott hello kimeneti gyűjteményhez társított a Pig-lekérdezést.</br>
    5. Végezetül kattintson <strong>dokumentumkezelő</strong> alatt <strong>fejlesztői eszközök</strong>.</br></p>

    A Pig lekérdezési eredmények hello jelenik meg.

    ![A Pig lekérdezés eredményei][image-pig-query-results]

## <a name="RunMapReduce"></a>5. lépés: Azure Cosmos DB és HDInsight MapReduce feladat futtatása
1. A következő változók a PowerShell-parancsfájl ablaktáblán hello beállítása.

        $subscriptionName = "<SubscriptionName>"   # Azure subscription name
        $clusterName = "<ClusterName>"             # HDInsight cluster name
2. A MapReduce feladatot, amely minden egyes dokumentum tulajdonság az Azure Cosmos DB gyűjteményből előfordulások száma hello számolja azt fogja végrehajtani. Adja hozzá a parancsfájl részlet **után** fenti hello részlet.

        # Define hello MapReduce job.
        $TallyPropertiesJobDefinition = New-AzureHDInsightMapReduceJobDefinition -JarFile "wasb:///example/jars/TallyProperties-v01.jar" -ClassName "TallyProperties" -Arguments "<DocumentDB Endpoint>","<DocumentDB Primary Key>", "<DocumentDB Database Name>","<DocumentDB Input Collection Name>","<DocumentDB Output Collection Name>","<[Optional] DocumentDB Query>"

   > [!NOTE]
   > Egyéni telepítése hello hello Cosmos DB Hadoop összekötő TallyProperties-v01.jar tartalmaz.
   >
   >
3. Adja hozzá a következő parancs toosubmit hello MapReduce feladatot hello.

        # Save hello start time and submit hello job.
        $startTime = Get-Date
        Select-AzureSubscription $subscriptionName
        $TallyPropertiesJob = Start-AzureHDInsightJob -Cluster $clusterName -JobDefinition $TallyPropertiesJobDefinition | Wait-AzureHDInsightJob -WaitTimeoutInSeconds 3600  

    Ezenkívül toohello MapReduce feladatdefiníció, itt is megadni hello HDInsight fürt toorun hello MapReduce feladatot, és hello hitelesítő adatokat. hello Start-AzureHDInsightJob egy aszinkron módjára való átkapcsolás hívás. hello feladat, használjon hello toocheck hello megvalósításának *várakozási-AzureHDInsightJob* parancsmag.
4. Adja hozzá a következő parancs toocheck hello futó hello MapReduce feladatot hibákat.

        # Get hello job output and print hello start and end time.
        $endTime = Get-Date
        Get-AzureHDInsightJobOutput -Cluster $clusterName -JobId $TallyPropertiesJob.JobId -StandardError
        Write-Host "Start: " $startTime ", End: " $endTime -ForegroundColor Green
5. **Futtatás** az új parancsfájl! **Kattintson a** hello zöld végrehajtás gombra.
6. Hello eredmények ellenőrzése. Jelentkezzen be a hello [Azure Portal][azure-portal].

   1. Kattintson a <strong>Tallózás</strong> hello bal oldali panelen.
   2. Kattintson a <strong>mindent</strong> hello felső – jobb hello Tallózás panel.
   3. Keresse meg és kattintson a <strong>Azure Cosmos DB fiókok</strong>.
   4. Ezt követően keresse meg a <strong>Azure Cosmos DB fiók</strong>, majd <strong>Azure Cosmos DB adatbázis</strong> és a <strong>Azure Cosmos DB gyűjtemény</strong> megadott hello kimeneti gyűjteményhez társított a MapReduce feladatot.
   5. Végezetül kattintson <strong>dokumentumkezelő</strong> alatt <strong>fejlesztői eszközök</strong>.

      A MapReduce feladatot hello eredményeit jelenik meg.

      ![MapReduce lekérdezés eredményei][image-mapreduce-query-results]

## <a name="NextSteps"></a>Következő lépések
Gratulálunk! Csak az első Hive, Pig és MapReduce feladatok Azure Cosmos DB és HDInsight futtatta.

Tudunk nyissa meg a Hadoop összekötő forrása. Ha szeretné használni, akkor a járulhat [GitHub][github].

toolearn több, tekintse meg a következő cikkek hello:

* [A Documentdb Java-alkalmazások fejlesztése][documentdb-java-application]
* [A hdinsight Hadoop Java MapReduce programok fejlesztése][hdinsight-develop-deploy-java-mapreduce]
* [Hadoop használatának megkezdésében a Hive HDInsight tooanalyze mobil kézibeszélő használatban][hdinsight-get-started]
* [Use MapReduce with HDInsight][hdinsight-use-mapreduce]
* [A Hive használata a HDInsightban][hdinsight-use-hive]
* [A Pig használata a HDInsightban][hdinsight-use-pig]
* [Parancsfájlművelet HDInsight-fürtök testreszabása][hdinsight-hadoop-customize-cluster]

[apache-hadoop]: http://hadoop.apache.org/
[apache-hadoop-doc]: http://hadoop.apache.org/docs/current/
[apache-hive]: http://hive.apache.org/
[apache-pig]: http://pig.apache.org/
[getting-started]: documentdb-get-started.md

[azure-portal]: https://portal.azure.com/
[azure-powershell-diagram]: ./media/run-hadoop-with-hdinsight/azurepowershell-diagram-med.png

[hdinsight-samples]: http://portalcontent.blob.core.windows.net/samples/documentdb-hdinsight-samples.zip
[github]: https://github.com/Azure/azure-documentdb-hadoop
[documentdb-java-application]: documentdb-java-application.md
[import-data]: import-data.md

[hdinsight-custom-provision]: ../hdinsight/hdinsight-provision-clusters.md
[hdinsight-develop-deploy-java-mapreduce]: ../hdinsight/hdinsight-develop-deploy-java-mapreduce-linux.md
[hdinsight-hadoop-customize-cluster]: ../hdinsight/hdinsight-hadoop-customize-cluster.md
[hdinsight-get-started]: ../hdinsight/hdinsight-hadoop-tutorial-get-started-windows.md
[hdinsight-storage]: ../hdinsight/hdinsight-hadoop-use-blob-storage.md
[hdinsight-use-hive]: ../hdinsight/hdinsight-use-hive.md
[hdinsight-use-mapreduce]: ../hdinsight/hdinsight-use-mapreduce.md
[hdinsight-use-pig]: ../hdinsight/hdinsight-use-pig.md

[image-customprovision-page1]: ./media/run-hadoop-with-hdinsight/customprovision-page1.png
[image-hive-query-results]: ./media/run-hadoop-with-hdinsight/hivequeryresults.PNG
[image-mapreduce-query-results]: ./media/run-hadoop-with-hdinsight/mapreducequeryresults.PNG
[image-pig-query-results]: ./media/run-hadoop-with-hdinsight/pigqueryresults.PNG

[powershell-install-configure]: https://docs.microsoft.com/powershell/azure/install-azurerm-ps?view=azurermps-4.0.0
