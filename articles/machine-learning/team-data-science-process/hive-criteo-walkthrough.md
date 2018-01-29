---
title: "Az Team tudományos folyamat művelet:-1 TB-os dataset Azure HDInsight Hadoop-fürtök használata |} Microsoft Docs"
description: "Az Team tudományos folyamat használ egy végpont forgatókönyv egy HDInsight Hadoop-fürt létrehozásához és telepítéséhez egy nagy méretű (1 TB) nyilvánosan elérhető adatkészlet modell alkalmazó"
services: machine-learning,hdinsight
documentationcenter: 
author: bradsev
manager: cgronlun
editor: cgronlun
ms.assetid: 72d958c4-3205-49b9-ad82-47998d400d2b
ms.service: machine-learning
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 11/29/2017
ms.author: bradsev
ms.openlocfilehash: 760e08643fb3e71478fc899278591569da1d515b
ms.sourcegitcommit: 5a6e943718a8d2bc5babea3cd624c0557ab67bd5
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 12/01/2017
---
# <a name="the-team-data-science-process-in-action---using-an-azure-hdinsight-hadoop-cluster-on-a-1-tb-dataset"></a>A művelet – 1 TB-os dataset Azure HDInsight Hadoop-fürtök használata az Team tudományos folyamat

Ez a forgatókönyv leírja, hogyan használható az Team tudományos folyamat egy végpont forgatókönyvet, és egy [Azure HDInsight Hadoop-fürt](https://azure.microsoft.com/services/hdinsight/) tárolására, vizsgálatát, visszafejtés a beállítást, és le a mintaadatokat egy nyilvánosan elérhető [ Criteo](http://labs.criteo.com/downloads/download-terabyte-click-logs/) adatkészletek. Az Azure Machine Learning segítségével a bináris osztályozási modell létrehozása ezeken az adatokon. Azt is bemutatja, hogyan közzététele egy ezek a modellek webszolgáltatásként.

Akkor is egy IPython notebook használatához jelenik meg ebben a bemutatóban feladatok elvégzését. Konzultáljon a felhasználók számára szeretné próbálni ezt a módszert használja a [Criteo forgatókönyv Hive ODBC-kapcsolat használatával](https://github.com/Azure/Azure-MachineLearning-DataScience/blob/master/Misc/DataScienceProcess/iPythonNotebooks/machine-Learning-data-science-process-hive-walkthrough-criteo.ipynb) témakör.

## <a name="dataset"></a>Criteo adatkészlet leírása
A Criteo adata kattintson előrejelzés adatkészletre mutató körülbelül 370 GB tömörített gzip TSV fájlok (tömörítetlen ~1.3TB), amely több mint 4.3 milliárd rögzíti. A 24 napos származik kattintson által elérhetővé tett adatok [Criteo](http://labs.criteo.com/downloads/download-terabyte-click-logs/). Adatszakértőkön kényelmi célokat szolgál a számunkra, hogy a rendelkezésre álló adatok tömörítetlen lett.

Ez az adatkészlet egyes rekord 40 oszlopokat tartalmazza:

* az első oszlop egy címke oszlopot, amely azt jelzi, hogy a felhasználó rákattint egy **hozzáadása** (érték-1) vagy nem kattintson egy (a 0 érték)
* Ezután 13 oszlopok numerikus, és
* utolsó 26 kategorikus oszlopok

Az oszlopok vannak anonimizált adatokon alapul, és használja fel nevek sorozata: a (a címke oszlop) "Oszlop1" "Col40" (az utolsó kategorikus oszlop).            

Az első 20 oszlopok két megfigyelések (sorok) a dataset adatkészletből cikkből a következő:

    Col1    Col2    Col3    Col4    Col5    Col6    Col7    Col8    Col9    Col10    Col11    Col12    Col13    Col14    Col15            Col16            Col17            Col18            Col19        Col20

    0       40      42      2       54      3       0       0       2       16      0       1       4448    4       1acfe1ee        1b2ff61f        2e8b2631        6faef306        c6fc10d3    6fcd6dcb           
    0               24              27      5               0       2       1               3       10064           9a8cb066        7a06385f        417e6103        2170fc56        acf676aa    6fcd6dcb                      

Hiányzó értékek vannak a numerikus és kategorikus oszlopok ehhez az adatkészlethez. Egy egyszerű módszer a hiányzó értékek kezelése leírása. További részletek az adatok írja őket a Hive táblák tárolásakor.

**Definíciója:** *Átkattintós gyakorisága (Parancsra):* Ez az adatok kattintással százaléka. Criteo DataSet adatkészletben a Parancsra, de hamarosan 3.3 % 0.033.

## <a name="mltasks"></a>Előrejelzés feladatok példák
Ez a forgatókönyv két minta előrejelzés problémákat tárgyalja:

1. **Bináris osztályozási**: előrejelzi, hogy a felhasználó rákattint egy hozzáadása:
   
   * Osztály: 0: Nincs kattintson
   * 1 osztály: kattintson a
2. **Regressziós**: előrejelzi egy felhasználó funkciókat ad kattintson valószínűségét.

## <a name="setup"></a>Tudományos adatok mentése beállítása egy HDInsight szabható Hadoop bemutatása-fürt
**Megjegyzés:** Ez általában az egy **Admin** feladat.

Állítsa be a HDInsight-fürtökkel három lépésben prediktív elemzési megoldások létrehozásához Azure Adattudomány környezetében:

1. [Hozzon létre egy tárfiókot](../../storage/common/storage-create-storage-account.md): ezt a tárfiókot az Azure Blob Storage adatok tárolására szolgál. A HDInsight-fürtök használt tárolja itt.
2. [Azure HDInsight Hadoop-fürtök testreszabása Adattudomány](customize-hadoop-cluster.md): Ezzel a lépéssel hoz létre egy Azure HDInsight Hadoop-fürt összes csomópontján telepítve 64 bites Anaconda Python 2.7. Amikor a HDInsight-fürtök testreszabása (a jelen témakörben ismertetett) két fontos lépésből áll.
   
   * A tárfiók létrehozásakor az 1. lépésben a HDInsight-fürthöz létrehozott kell kapcsolni. Ez a tárfiók eléréséhez szükséges adatok, amelyek a fürtön belül dolgozhatók szolgál.
   * Engedélyezni kell a távoli hozzáférést az átjárócsomóponthoz, a fürt létrehozása után. Az itt megadott (eltérnek a a létrehozásakor a fürthöz megadott) távoli hozzáférési hitelesítő adatok megjegyzése: kell őket a következő műveletek végrehajtásához.
3. [Hozzon létre egy Azure ML munkaterületet](../studio/create-workspace.md): az Azure Machine Learning-munkaterület használata után egy kezdeti adatok feltárása, és a HDInsight-fürt mintavételi le a machine learning modellek készítéséhez.

## <a name="getdata"></a>GET és egy nyilvános forrásból származó adatokat
A [Criteo](http://labs.criteo.com/downloads/download-terabyte-click-logs/) dataset hozzáférhet a hivatkozásra kattint, a használati feltételek elfogadása és megadása. Pillanatkép készítése néz Ez itt jelenik meg:

![Criteo feltételek elfogadása](./media/hive-criteo-walkthrough/hLxfI2E.png)

Kattintson a **tovább a letöltési** további a DataSet adatkészlet és elérhetőségét.

Az adatok találhatók a nyilvános [Azure blob storage](../../storage/blobs/storage-dotnet-how-to-use-blobs.md) helye: wasb://criteo@azuremlsampleexperiments.blob.core.windows.net/raw/. A "wasb" Azure Blob Storage-helyre hivatkozik. 

1. Az a nyilvános blob-tároló tartalmaz tömörítetlen adatok három almappája.
   
   1. A almappa *nyers és száma/* tartalmazza az első 21 napnyi adatot - nap után\_nap 00\_20
   2. A almappa *nyers vonat / /* áll az adatok egy nap napi\_21
   3. A almappa *nyers és tesztelési célú/* két napnyi adat, áll nap\_22-es és a nap\_23
2. Azok számára, akiknek elindítja a nyers gzip adatokkal, ezek is elérhetők a fő mappában *nyers /* , day_NN.gz, ahol NN ugrik 00-tól 23.

Egy másik módszert is el tudja érni, vizsgálatát, és ezeket az adatokat, nincs szükség semmilyen helyi letöltések kifejtett Ez az útmutató későbbi szakaszában Hive táblák létrehozásakor modell.

## <a name="login"></a>Jelentkezzen be a fürt headnode
Jelentkezzen be a fürt headnode, használja a [Azure-portálon](https://ms.portal.azure.com) keresse meg a fürt számára. A HDInsight elefánt ikonra a bal oldalon, és kattintson duplán a fürt neve. Keresse meg a **konfigurációs** fülre, kattintson duplán a CONNECT ikonra a lap alján, és adja meg a távelérés hitelesítő adatokat. Ezzel megnyitná a fürt headnode.

Íme egy jellegzetes első való bejelentkezéshez a fürt headnode néz:

![Jelentkezzen be a fürt](./media/hive-criteo-walkthrough/Yys9Vvm.png)

A bal oldalon a "Hadoop parancssori", amely az adatok feltárási a workhorse van. Figyelje meg, hogy két hasznos URL-címek – "Hadoop Yarn állapota" és "Hadoop neve csomópont". A csomópont URL-CÍMÉT részleteket nyújt a fürtkonfiguráció és a yarn állapot URL-címe jeleníti meg a feladat előrehaladását.

Most már be vannak állítva, és készen áll a forgatókönyv első része megkezdéséhez: Hive eszközzel és adatok Azure Machine Learning Felkészülés az adatok feltárása.

## <a name="hive-db-tables"></a>Hive-adatbázis és tábla létrehozása
Hive táblák a Criteo adatkészlet létrehozásához nyissa meg a ***Hadoop parancssori*** az átjárócsomóponthoz asztalán, és írja be a Hive directory parancs beírásával

    cd %hive_home%\bin

> [!NOTE]
> Ez a forgatókönyv minden Hive parancsot futtatja a Hive bin / directory kérdés. Ezzel létrehozza az elérési út probléma merül fel automatikusan. Használhatja a feltételek "Struktúra directory prompt", "struktúra bin / directory prompt", és a "Hadoop parancssori" gombokra.
> 
> [!NOTE]
> A Hive-lekérdezések végrehajtásához egy mindig használhatja a következő parancsokat:
> 
> 

        cd %hive_home%\bin
        hive

Miután a Hive REPL jelenik meg, amelyen egy "struktúra >"aláírásához, egyszerűen kivágása és illessze be a lekérdezést végrehajtásra.

Az alábbi kód létrehoz egy adatbázis "criteo", és akkor hoz létre a 4 táblák:

* egy *számok generálásához tábla* nap napi épülő\_nap 00\_20,
* egy *használni a vonat dataset tábla* nap épülő\_21, és
* két *használni a teszt adatkészletek táblák* nap épülő\_22-es és a nap\_23 kulcsattribútumokkal.

A tesztelési adatkészletnél felosztása két különböző tábla, mert a nap egyik szabadnap. A célja meghatározni, ha a modell észlelését szünnap és nem szünnap közötti különbségeket kattintások száma másodpercenként.

A parancsfájl [minta &#95; hive &#95; hozzon létre &#95; criteo &#95; adatbázis &#95; és &#95;tables.hql](https://github.com/Azure/Azure-MachineLearning-DataScience/blob/master/Misc/DataScienceProcess/DataScienceScripts/sample_hive_create_criteo_database_and_tables.hql) kényelmi itt jelenik meg:

    CREATE DATABASE IF NOT EXISTS criteo;
    DROP TABLE IF EXISTS criteo.criteo_count;
    CREATE TABLE criteo.criteo_count (
    col1 string,col2 double,col3 double,col4 double,col5 double,col6 double,col7 double,col8 double,col9 double,col10 double,col11 double,col12 double,col13 double,col14 double,col15 string,col16 string,col17 string,col18 string,col19 string,col20 string,col21 string,col22 string,col23 string,col24 string,col25 string,col26 string,col27 string,col28 string,col29 string,col30 string,col31 string,col32 string,col33 string,col34 string,col35 string,col36 string,col37 string,col38 string,col39 string,col40 string)
    ROW FORMAT DELIMITED FIELDS TERMINATED BY '\t'
    LINES TERMINATED BY '\n'
    STORED AS TEXTFILE LOCATION 'wasb://criteo@azuremlsampleexperiments.blob.core.windows.net/raw/count';

    DROP TABLE IF EXISTS criteo.criteo_train;
    CREATE TABLE criteo.criteo_train (
    col1 string,col2 double,col3 double,col4 double,col5 double,col6 double,col7 double,col8 double,col9 double,col10 double,col11 double,col12 double,col13 double,col14 double,col15 string,col16 string,col17 string,col18 string,col19 string,col20 string,col21 string,col22 string,col23 string,col24 string,col25 string,col26 string,col27 string,col28 string,col29 string,col30 string,col31 string,col32 string,col33 string,col34 string,col35 string,col36 string,col37 string,col38 string,col39 string,col40 string)
    ROW FORMAT DELIMITED FIELDS TERMINATED BY '\t'
    LINES TERMINATED BY '\n'
    STORED AS TEXTFILE LOCATION 'wasb://criteo@azuremlsampleexperiments.blob.core.windows.net/raw/train';

    DROP TABLE IF EXISTS criteo.criteo_test_day_22;
    CREATE TABLE criteo.criteo_test_day_22 (
    col1 string,col2 double,col3 double,col4 double,col5 double,col6 double,col7 double,col8 double,col9 double,col10 double,col11 double,col12 double,col13 double,col14 double,col15 string,col16 string,col17 string,col18 string,col19 string,col20 string,col21 string,col22 string,col23 string,col24 string,col25 string,col26 string,col27 string,col28 string,col29 string,col30 string,col31 string,col32 string,col33 string,col34 string,col35 string,col36 string,col37 string,col38 string,col39 string,col40 string)
    ROW FORMAT DELIMITED FIELDS TERMINATED BY '\t'
    LINES TERMINATED BY '\n'
    STORED AS TEXTFILE LOCATION 'wasb://criteo@azuremlsampleexperiments.blob.core.windows.net/raw/test/day_22';

    DROP TABLE IF EXISTS criteo.criteo_test_day_23;
    CREATE TABLE criteo.criteo_test_day_23 (
    col1 string,col2 double,col3 double,col4 double,col5 double,col6 double,col7 double,col8 double,col9 double,col10 double,col11 double,col12 double,col13 double,col14 double,col15 string,col16 string,col17 string,col18 string,col19 string,col20 string,col21 string,col22 string,col23 string,col24 string,col25 string,col26 string,col27 string,col28 string,col29 string,col30 string,col31 string,col32 string,col33 string,col34 string,col35 string,col36 string,col37 string,col38 string,col39 string,col40 string)
    ROW FORMAT DELIMITED FIELDS TERMINATED BY '\t'
    LINES TERMINATED BY '\n'
    STORED AS TEXTFILE LOCATION 'wasb://criteo@azuremlsampleexperiments.blob.core.windows.net/raw/test/day_23';

Ezek a táblázatok külső, így egyszerűen mutathat Azure Blob Storage (wasb) helyükre.

**BÁRMELY Hive-lekérdezések végrehajtásához két módja van:**

1. **A Hive REPL parancssori használata**: az egyik egy "hive" parancsot, és másolja ki és illessze be a lekérdezést, a Hive REPL parancssori. Ehhez a következőket kell tennie:
   
        cd %hive_home%\bin
        hive
   
     Most, parancssori REPL kivágja, és a lekérdezés Beillesztés hajtja végre azt.
2. **Lekérdezések mentése fájlba, és a parancs végrehajtása**: A második pedig a lekérdezések .hql fájlba mentése ([minta &#95; hive &#95; hozzon létre &#95; criteo &#95; adatbázis &#95; és &#95;tables.hql](https://github.com/Azure/Azure-MachineLearning-DataScience/blob/master/Misc/DataScienceProcess/DataScienceScripts/sample_hive_create_criteo_database_and_tables.hql)), és hogyan adhat ki a lekérdezés végrehajtása a következő parancsot:
   
        hive -f C:\temp\sample_hive_create_criteo_database_and_tables.hql

### <a name="confirm-database-and-table-creation"></a>Erősítse meg az adatbázis és tábla létrehozása
Ezt követően erősítse meg a létrehozni az adatbázist a Hive van a következő parancsot a / directory parancssorba:

        hive -e "show databases;"

Ezen a következő:

        criteo
        default
        Time taken: 1.25 seconds, Fetched: 2 row(s)

Ez megerősíti, hogy a "criteo" új adatbázis létrehozása.

Milyen táblák létrejöttek megtekintéséhez egyszerűen adja ki a parancsot itt az a Hive bin / directory parancssorba:

        hive -e "show tables in criteo;"

Ekkor a következő az alábbiakat:

        criteo_count
        criteo_test_day_22
        criteo_test_day_23
        criteo_train
        Time taken: 1.437 seconds, Fetched: 4 row(s)

## <a name="exploration"></a>Az adatok feltárása a Hive
Most már áll készen a Hive néhány alapvető adatok feltárása. A szerelvény szereplő példák számával kezdődik, és adattáblák tesztelése.

### <a name="number-of-train-examples"></a>Vonat példák száma
A tartalmát [minta &#95; hive &#95; száma &#95; vonat &#95; tábla &#95;examples.hql](https://github.com/Azure/Azure-MachineLearning-DataScience/blob/master/Misc/DataScienceProcess/DataScienceScripts/sample_hive_count_train_table_examples.hql) itt látható:

        SELECT COUNT(*) FROM criteo.criteo_train;

Ez eredményez:

        192215183
        Time taken: 264.154 seconds, Fetched: 1 row(s)

Azt is megteheti, egy is adhatnak ki a következő parancsot a Hive bin / directory parancssorba:

        hive -f C:\temp\sample_hive_count_criteo_train_table_examples.hql

### <a name="number-of-test-examples-in-the-two-test-datasets"></a>Tesztelési példák a két teszt adatkészletek száma
Most számolja össze a két teszt adatkészletek példák. A tartalmát [minta &#95; hive &#95; száma &#95; criteo &#95; teszt &#95; nap &#95; 22 &#95; tábla &#95;examples.hql](https://github.com/Azure/Azure-MachineLearning-DataScience/blob/master/Misc/DataScienceProcess/DataScienceScripts/sample_hive_count_criteo_test_day_22_table_examples.hql) itt:

        SELECT COUNT(*) FROM criteo.criteo_test_day_22;

Ez eredményez:

        189747893
        Time taken: 267.968 seconds, Fetched: 1 row(s)

A szokásos módon, előfordulhat, hogy is hívja a parancsfájl a Hive bin / könyvtár kérdezzen rá a parancs az kiadása:

        hive -f C:\temp\sample_hive_count_criteo_test_day_22_table_examples.hql

Végül, vizsgálja meg a tesztelési adatkészletnél nap alapján tesztelési példák száma\_23.

Ehhez a parancs hasonlít a most látható (hivatkoznak [minta &#95; hive &#95; száma &#95; criteo &#95; teszt &#95; nap &#95; 23 &#95;examples.hql](https://github.com/Azure/Azure-MachineLearning-DataScience/blob/master/Misc/DataScienceProcess/DataScienceScripts/sample_hive_count_criteo_test_day_23_examples.hql)):

        SELECT COUNT(*) FROM criteo.criteo_test_day_23;

Ezen a következő:

        178274637
        Time taken: 253.089 seconds, Fetched: 1 row(s)

### <a name="label-distribution-in-the-train-dataset"></a>Címke terjesztési vonat adatkészlet
A címke terjesztési vonat adatkészlet jelentőséggel bír. Ez megtekintéséhez tartalmának megjelenítése [minta &#95; hive &#95; criteo &#95; címke &#95; terjesztési &#95; vonat &#95;table.hql](https://github.com/Azure/Azure-MachineLearning-DataScience/blob/master/Misc/DataScienceProcess/DataScienceScripts/sample_hive_criteo_label_distribution_train_table.hql):

        SELECT Col1, COUNT(*) AS CT FROM criteo.criteo_train GROUP BY Col1;

Ezzel megkapják a címke terjesztési:

        1       6292903
        0       185922280
        Time taken: 459.435 seconds, Fetched: 2 row(s)

Vegye figyelembe, hogy pozitív címkék százalékos készül 3.3 % (konzisztensek legyenek az eredeti adathalmazból).

### <a name="histogram-distributions-of-some-numeric-variables-in-the-train-dataset"></a>Hisztogram disztribúciók vonat adatkészlet egyes numerikus változók
Használhatja a struktúrájának natív "hisztogram\_numerikus" függvényt, hogy a numerikus változók terjesztési néz ki. Az alábbiakban tartalmát [minta &#95; hive &#95; criteo &#95; hisztogram &#95;numeric.hql](https://github.com/Azure/Azure-MachineLearning-DataScience/blob/master/Misc/DataScienceProcess/DataScienceScripts/sample_hive_criteo_histogram_numeric.hql):

        SELECT CAST(hist.x as int) as bin_center, CAST(hist.y as bigint) as bin_height FROM
            (SELECT
            histogram_numeric(col2, 20) as col2_hist
            FROM
            criteo.criteo_train
            ) a
            LATERAL VIEW explode(col2_hist) exploded_table as hist;

Ez adja eredményül a következő:

        26      155878415
        2606    92753
        6755    22086
        11202   6922
        14432   4163
        17815   2488
        21072   1901
        24113   1283
        27429   1225
        30818   906
        34512   723
        38026   387
        41007   290
        43417   312
        45797   571
        49819   428
        53505   328
        56853   527
        61004   160
        65510   3446
        Time taken: 317.851 seconds, Fetched: 20 row(s)

NÉZET – OLDALIRÁNYÚ felbontása kombinációja a Hive szolgál a szokásos lista helyett egy SQL-szerű kimenet előállításához. Vegye figyelembe, hogy az ebben a táblában az első oszlop felel meg a bin center és a második, a bin gyakoriságát.

### <a name="approximate-percentiles-of-some-numeric-variables-in-the-train-dataset"></a>A vonat adatkészlet egyes numerikus változók hozzávetőleges százalékos érték
A numerikus változókkal érdeklő is hozzávetőleges százalékos érték kiszámításakor. Hive tartozó natív "PERCENTILIS\_hozzávetőleges" ezt az USA végzi. A tartalmát [minta &#95; hive &#95; criteo &#95; hozzávetőleges &#95;percentiles.hql](https://github.com/Azure/Azure-MachineLearning-DataScience/blob/master/Misc/DataScienceProcess/DataScienceScripts/sample_hive_criteo_approximate_percentiles.hql) vannak:

        SELECT MIN(Col2) AS Col2_min, PERCENTILE_APPROX(Col2, 0.1) AS Col2_01, PERCENTILE_APPROX(Col2, 0.3) AS Col2_03, PERCENTILE_APPROX(Col2, 0.5) AS Col2_median, PERCENTILE_APPROX(Col2, 0.8) AS Col2_08, MAX(Col2) AS Col2_max FROM criteo.criteo_train;

Ez eredményez:

        1.0     2.1418600917169246      2.1418600917169246    6.21887086390288 27.53454893115633       65535.0
        Time taken: 564.953 seconds, Fetched: 1 row(s)

A százalékos érték a terjesztési szorosan kapcsolódik a hisztogram terjesztési bármely numerikus változó általában.         

### <a name="find-number-of-unique-values-for-some-categorical-columns-in-the-train-dataset"></a>A vonat adatkészlet egyes kategorikus oszlopok egyedi értékek számának keresése
Az adatok feltárása a Folytatás található, néhány kategorikus oszlop tartanak egyedi értékek száma. Ehhez tartalmának megjelenítése [minta &#95; hive &#95; criteo &#95;egyedi; &#95; értékek &#95;categoricals.hql](https://github.com/Azure/Azure-MachineLearning-DataScience/blob/master/Misc/DataScienceProcess/DataScienceScripts/sample_hive_criteo_unique_values_categoricals.hql):

        SELECT COUNT(DISTINCT(Col15)) AS num_uniques FROM criteo.criteo_train;

Ez eredményez:

        19011825
        Time taken: 448.116 seconds, Fetched: 1 row(s)

Vegye figyelembe, hogy Col15 19M egyedi értékeket! Naïve módszerek, például a "egy közbeni kódolás" kódolni ilyen nagy dimenziós kategorikus változók használata nem megvalósítható. Különösen hatékony, hatékony módszer néven [tanulási a számlálás](http://blogs.technet.com/b/machinelearning/archive/2015/02/17/big-learning-made-easy-with-counts.aspx) probléma hatékonyan elleni alapján, és egy.

Végül tekintse meg néhány kategorikus oszlopot, valamint az egyedi értékek száma. A tartalmát [minta &#95; hive &#95; criteo &#95;egyedi; &#95; értékek &#95; több &#95;categoricals.hql](https://github.com/Azure/Azure-MachineLearning-DataScience/blob/master/Misc/DataScienceProcess/DataScienceScripts/sample_hive_criteo_unique_values_multiple_categoricals.hql) vannak:

        SELECT COUNT(DISTINCT(Col16)), COUNT(DISTINCT(Col17)),
        COUNT(DISTINCT(Col18), COUNT(DISTINCT(Col19), COUNT(DISTINCT(Col20))
        FROM criteo.criteo_train;

Ez eredményez:

        30935   15200   7349    20067   3
        Time taken: 1933.883 seconds, Fetched: 1 row(s)

Ebben az esetben ne feledje, hogy Col20, kivéve a többi oszlop sok egyedi értékeket.

### <a name="co-occurrence-counts-of-pairs-of-categorical-variables-in-the-train-dataset"></a>Pár vonat adatkészlet kategorikus változók száma párhuzamos előfordulásainak kívánt számát

Kategorikus változók párok közös előfordulási száma. az is fontos. Ez lehet meghatározni a kódot a [minta &#95; hive &#95; criteo &#95;párosított; &#95; kategorikus &#95;counts.hql](https://github.com/Azure/Azure-MachineLearning-DataScience/blob/master/Misc/DataScienceProcess/DataScienceScripts/sample_hive_criteo_paired_categorical_counts.hql):

        SELECT Col15, Col16, COUNT(*) AS paired_count FROM criteo.criteo_train GROUP BY Col15, Col16 ORDER BY paired_count DESC LIMIT 15;

Névkeresési rendezése a száma szerint a előfordulásainak kívánt számát, és tekintse meg az első 15 ebben az esetben. Biztosítanak számunkra:

        ad98e872        cea68cd3        8964458
        ad98e872        3dbb483e        8444762
        ad98e872        43ced263        3082503
        ad98e872        420acc05        2694489
        ad98e872        ac4c5591        2559535
        ad98e872        fb1e95da        2227216
        ad98e872        8af1edc8        1794955
        ad98e872        e56937ee        1643550
        ad98e872        d1fade1c        1348719
        ad98e872        977b4431        1115528
        e5f3fd8d        a15d1051        959252
        ad98e872        dd86c04a        872975
        349b3fec        a52ef97d        821062
        e5f3fd8d        a0aaffa6        792250
        265366bf        6f5c7c41        782142
        Time taken: 560.22 seconds, Fetched: 15 row(s)

## <a name="downsample"></a>Az Azure Machine Learning adathalmaz minta le
Az adatkészletek felfedezte és módjáról az ilyen típusú (beleértve a kombinációk), egyetlen változóra feltárása le minta adatkészletek, hogy az Azure Machine Learning modellek építhetők mutatja. Amely a probléma a fókusz visszaírási: Példa attribútumok (Col2 - Col40 szolgáltatás értékeit) megadott, előre jelezni, ha Oszlop1-e a 0 (nincs kattintson) vagy 1 (kattintson).

Minta a tanítási és tesztelési adatkészletek az eredeti méret % 1, struktúrájának natív RAND() függvény használatát. A következő parancsfájl [minta &#95; hive &#95; criteo &#95; felbontásának &#95; vonat &#95;dataset.hql](https://github.com/Azure/Azure-MachineLearning-DataScience/blob/master/Misc/DataScienceProcess/DataScienceScripts/sample_hive_criteo_downsample_train_dataset.hql) ezt a vonat adatkészlet végzi:

        CREATE TABLE criteo.criteo_train_downsample_1perc (
        col1 string,col2 double,col3 double,col4 double,col5 double,col6 double,col7 double,col8 double,col9 double,col10 double,col11 double,col12 double,col13 double,col14 double,col15 string,col16 string,col17 string,col18 string,col19 string,col20 string,col21 string,col22 string,col23 string,col24 string,col25 string,col26 string,col27 string,col28 string,col29 string,col30 string,col31 string,col32 string,col33 string,col34 string,col35 string,col36 string,col37 string,col38 string,col39 string,col40 string)
        ROW FORMAT DELIMITED FIELDS TERMINATED BY '\t'
        LINES TERMINATED BY '\n'
        STORED AS TEXTFILE;

        ---Now downsample and store in this table

        INSERT OVERWRITE TABLE criteo.criteo_train_downsample_1perc SELECT * FROM criteo.criteo_train WHERE RAND() <= 0.01;

Ez eredményez:

        Time taken: 12.22 seconds
        Time taken: 298.98 seconds

A parancsfájl [minta &#95; hive &#95; criteo &#95; felbontásának &#95; teszt &#95; nap &#95; 22 &#95;dataset.hql](https://github.com/Azure/Azure-MachineLearning-DataScience/blob/master/Misc/DataScienceProcess/DataScienceScripts/sample_hive_criteo_downsample_test_day_22_dataset.hql) minderre a tesztadatok, napi\_22:

        --- Now for test data (day_22)

        CREATE TABLE criteo.criteo_test_day_22_downsample_1perc (
        col1 string,col2 double,col3 double,col4 double,col5 double,col6 double,col7 double,col8 double,col9 double,col10 double,col11 double,col12 double,col13 double,col14 double,col15 string,col16 string,col17 string,col18 string,col19 string,col20 string,col21 string,col22 string,col23 string,col24 string,col25 string,col26 string,col27 string,col28 string,col29 string,col30 string,col31 string,col32 string,col33 string,col34 string,col35 string,col36 string,col37 string,col38 string,col39 string,col40 string)
        ROW FORMAT DELIMITED FIELDS TERMINATED BY '\t'
        LINES TERMINATED BY '\n'
        STORED AS TEXTFILE;

        INSERT OVERWRITE TABLE criteo.criteo_test_day_22_downsample_1perc SELECT * FROM criteo.criteo_test_day_22 WHERE RAND() <= 0.01;

Ez eredményez:

        Time taken: 1.22 seconds
        Time taken: 317.66 seconds


Végezetül, a parancsfájl [minta &#95; hive &#95; criteo &#95; felbontásának &#95; teszt &#95; nap &#95; 23 &#95;dataset.hql](https://github.com/Azure/Azure-MachineLearning-DataScience/blob/master/Misc/DataScienceProcess/DataScienceScripts/sample_hive_criteo_downsample_test_day_23_dataset.hql) minderre a tesztadatok, napi\_23:

        --- Finally test data day_23
        CREATE TABLE criteo.criteo_test_day_23_downsample_1perc (
        col1 string,col2 double,col3 double,col4 double,col5 double,col6 double,col7 double,col8 double,col9 double,col10 double,col11 double,col12 double,col13 double,col14 double,col15 string,col16 string,col17 string,col18 string,col19 string,col20 string,col21 string,col22 string,col23 string,col24 string,col25 string,col26 string,col27 string,col28 string,col29 string,col30 string,col31 string,col32 string,col33 string,col34 string,col35 string,col36 string,col37 string,col38 string,col39 string,col40 srical feature; tring)
        ROW FORMAT DELIMITED FIELDS TERMINATED BY '\t'
        LINES TERMINATED BY '\n'
        STORED AS TEXTFILE;

        INSERT OVERWRITE TABLE criteo.criteo_test_day_23_downsample_1perc SELECT * FROM criteo.criteo_test_day_23 WHERE RAND() <= 0.01;

Ez eredményez:

        Time taken: 1.86 seconds
        Time taken: 300.02 seconds

Ennek készen áll a mintában szereplő lefelé tanítási és felépítése az Azure Machine Learning modellek adathalmaz tesztelése.

Összetevő végső fontos vonatkozik a count tábla Azure Machine Learning lépés előtt. A count tábla a következő alárendelt szakaszban tárgyalt részletesen bemutatható.

## <a name="count"></a>A count tábla rövid döntéseken
Ahogy azt, több kategorikus változók nagyon magas granularitása adható meg. A forgatókönyv egy hatékony módszer néven [tanulási a számlálás](http://blogs.technet.com/b/machinelearning/archive/2015/02/17/big-learning-made-easy-with-counts.aspx) ezekre a változókra egy hatékony kódolására, hatékony módon számára jelenik meg. Ezzel a technikával további tájékoztatást a megadott hivatkozás van.

[!NOTE]
>Ebben a bemutatóban a elsősorban száma táblák használata kompakt ábrázolásai nagy dimenziós kategorikus szolgáltatások létrehozásához. Ez nem az egyetlen lehetőség kategorikus szolgáltatások kódolása egyéb technikák olvashat, érdekelt felhasználók lefoglalhatják [egy-közbeni-kódolás](http://en.wikipedia.org/wiki/One-hot) és [szolgáltatáskivonatolás](http://en.wikipedia.org/wiki/Feature_hashing).
>

A count adatokon-összeállítási táblázatok száma, használja az adatok a mappa nyers és száma. A modellezési területen felhasználó megjelenik, hogyan hozhat létre a következő száma táblák kategorikus szolgáltatások teljesen új, vagy másik lehetőségként a explorations egy előre elkészített száma tábla használandó. A következőkben Ha a "előzetesen elkészített száma táblák" hivatkozunk, azt jelenti, hogy vannak-e megadva a szám táblák használata. Ezek a táblázatok eléréséhez részletes útmutatást a következő szakaszban találhatók.

## <a name="aml"></a>A modell Azure Machine Learning segítségével létrehozása
A modell létrehozása az Azure Machine Learning folyamatot követi ezeket a lépéseket:

1. [Hive táblák az Azure Machine Learning az adatok beolvasása](#step1)
2. [A kísérlet létrehozása: az adatok és a count táblákkal featurize törlése](#step2)
3. [Build, betanítását és pontozását a modellben.](#step3)
4. [A modell kiértékelése](#step4)
5. [A modell egy webszolgáltatás közzététele](#step5)

Most már készen áll az Azure Machine Learning studio modellek létrehozásához. A lefelé mintaadatokat Hive táblák a fürt értékként menti. Használja az Azure Machine Learning **és adatokat importálhat** modul ezeket az adatokat olvasni. A fürt a tárfiók eléréséhez szükséges hitelesítő adatokat a következő szerepelnek.

### <a name="step1"></a>1. lépés: Adatokat lekérni a Hive táblák az Azure Machine Learning az adatok importálása modullal, és válassza ki azt a tartalmazó gépi tanulási kísérlet
Kiválasztásával indítsa el a **+ új** -> **kísérlet** -> **üres kísérlet**. Ezt követően az a **keresési** mezőben keressen a "Importálási adatok" marad, felül. Húzza a **és adatokat importálhat** modul be a kísérlet vászonra (a képernyő középső része) a modul használandó adat-hozzáférési.

Ez az, hogy mi a **és adatokat importálhat** tűnik a Hive tábla adatainak lekérése közben:

![Adatok importálása adatok beolvasása](./media/hive-criteo-walkthrough/i3zRaoj.png)

Az a **és adatokat importálhat** modul, a paraméterek, a kép által biztosított példákban csak a rendezés értékek meg kell adni. Itt van néhány általános útmutatóként is szolgálhatnak a paraméterkészlet az kitöltésére a **és adatokat importálhat** modul.

1. Válassza ki a "Hive-lekérdezések" a **adatforrás**
2. Az a **adatbázis-lekérdezés Hive** mezőbe egy egyszerű jelöljön ki * FROM < a\_adatbázis\_name.your\_tábla\_neve >-elegendő.
3. **Hcatalog kiszolgáló URI azonosítója**: Ha a fürt "abc", akkor ez az egyszerűen: https://abc.azurehdinsight.net
4. **Hadoop felhasználói fiók nevét**: A felhasználó nevét, a fürthöz beüzemelési időpontjában választott. (Nem a távelérési felhasználónév!)
5. **Hadoop felhasználói fiók jelszavát**: a felhasználó nevét, a fürthöz beüzemelési időpontjában kiválasztott jelszavát. (Nem a távelérési jelszó!)
6. **Az kimeneti adatok**: válassza az "Azure"
7. **Az Azure storage-fiók neve**: A tárfiókot, a fürthöz tartozó
8. **Azure storage-fiók kulcs**: a fürthöz tartozó a tárfiók kulcsa.
9. **Az Azure Tárolónév**: Ha a fürt neve "abc", akkor egyszerűen "abc", ami általában.

Egyszer a **és adatokat importálhat** befejeződik (megjelenik a zöld osztásjelek a modul) adatok, ezek az adatok mentése (a választott nevű) egy adatkészlet. Mi a következőhöz hasonló:

![Adatok importálása az adatok mentése](./media/hive-criteo-walkthrough/oxM73Np.png)

Kattintson a jobb gombbal a kimeneti portjára a **és adatokat importálhat** modul. Ez azt jelzi egy **dataset elmentse** beállítás és egy **Visualize** lehetőséget. A **Visualize** lehetőséget, ha kattintott, jeleníti meg az adatok, és a jobb oldali panelen, amely akkor hasznos, ha néhány statisztikák 100 sorát. Adatok mentéséhez egyszerűen válassza **dataset elmentse** , és kövesse az utasításokat.

Jelölje be a mentett dataset használható a machine learning kísérletben, keresse meg a adatkészletek használata a **keresési** be az alábbi ábrán látható. Egyszerűen csak írja be a nevét, az adatkészlet részben férni, és húzza az adatkészletet a fő megadott. Húzza azt a fő panelen kijelöli a machine learning modellezési használatos.

![A fő panelre Drage adatkészlet](./media/hive-criteo-walkthrough/cl5tpGw.png)

> [!NOTE]
> Ehhez a tanítási és tesztelési adathalmazokat. Ezenkívül fontos, hogy az adatbázis nevét és a táblaneveket, hogy a megadott, erre a célra. Az ábrán használt értékei kizárólag az ábra purposes.* *
> 
> 

### <a name="step2"></a>2. lépés: Egy egyszerű kísérlet létrehozása az Azure Machine Learning előre jelezni kattintással / egyetlen kattintással
Az Azure ML kísérlet így néz ki:

![Machine Learning kísérlet](./media/hive-criteo-walkthrough/xRpVfrY.png)

Most vizsgálja meg a legfontosabb összetevők, ez a kísérlet. Húzza a mentett tanítási és először tesztelje az adathalmazt a kísérletvászonra be.

#### <a name="clean-missing-data"></a>Hiányzó adatok törlése
A **Clean Missing Data** modul does a neve alapján: törli a hiányzó adatokat, hogy a felhasználó által megadott lehet. Ez a modul ez megismerhetők:

![Hiányzó adatok törlése](./media/hive-criteo-walkthrough/0ycXod6.png)

Itt döntött, hogy az összes hiányzó értékek cserélje le a 0. Nincsenek, valamint egyéb beállításokat, amelyek a legördülő lista a modul megnézzük látható.

#### <a name="feature-engineering-on-the-data"></a>Jellemzőkiemelés az adatokon
Egyedi értékeket az egyes kategorikus szolgáltatások nagy adatkészletek több millió lehet. A natív módszerek, például egy közbeni kódolását jelző ilyen nagy dimenziós kategorikus funkciók használata teljesen amelyeknek. Ez a forgatókönyv bemutatja, hogyan nagy dimenziós kategorikus változókhoz kompakt ábrázolásai létrehozásához használt beépített Azure Machine Learning modulok száma funkcióinak használatát. A végeredménynek modell kisebb méretet, a gyorsabb képzési és a metrikák teljesen összehasonlítható más módszerek használatával.

##### <a name="building-counting-transforms"></a>Leltár átalakítások felépítése
Count szolgáltatások létrehozásához használja a **Build számbavételi átalakítási** elérhető az Azure Machine Learning modulban. A modul így néz ki:

![Átalakítás számbavételi modul létrehozása](./media/hive-criteo-walkthrough/e0eqKtZ.png)
![Build számbavételi átalakító modul](./media/hive-criteo-walkthrough/OdDN0vw.png)

> [!IMPORTANT] 
> Az a **oszlopok száma** ezek az oszlopok számát az elvégezni kívánt adja meg. Ezek rendszerint (említettek) nagy dimenziós kategorikus oszlopok. Ne feledje, hogy rendelkezik-e a Criteo dataset 26 kategorikus oszlopok: az Col40 Col15 a. Ezek itt, count, és adja meg az indexek (a 15 40 látható vesszővel elválasztva).
> 

A modul használatára a MapReduce módban (megfelelő nagy adatkészletek), hozzá kell férnie egy HDInsight Hadoop-fürt (szolgáltatás feltárási használt felhasználhatók e célból) és a hitelesítő adatok. Az előző ábrán bemutatják, milyen a kitöltött értékeket néz (cserélje le a ábra azokat, a saját használati eset a megadott érték).

![Modul paraméterei](./media/hive-criteo-walkthrough/05IqySf.png)

Az előző ábra bemutatja, hogyan adja meg a bemeneti blob helyet. Ezen a helyen a count táblák létrehozása fenntartott adatokat tartalmaz.

Ha ez a modul befejezése után történik, az átalakítás később mentse kattintson a jobb gombbal a modult, majd válassza a **átalakító elmentse** lehetőséget:

!["Mentés másként átalakító" beállítás](./media/hive-criteo-walkthrough/IcVgvHR.png)

A fenti kísérlet architektúra esetén a dataset "ytransform2" felel meg pontosan mentett száma átalakító. Ehhez a kísérlethez hátralevő, feltételezzük, hogy az olvasó használja egy **Build számbavételi átalakítási** szolgáltatásokat biztosító modul az egyes adatok készítése a számát, és lehet majd ezek száma számláló létrehozásához a tanítási és tesztelési adatkészletek.

##### <a name="choosing-what-count-features-to-include-as-part-of-the-train-and-test-datasets"></a>A tanítási és tesztelési adatkészletek részét képező szolgáltatásokat milyen számának kiválasztása
Egyszer száma átalakítás kész, a felhasználó választhat a vonat felveendő és tesztelése adatkészletek használatával milyen funkciók a **száma tábla paraméterek módosítása** modul. A teljesség Ez a modul itt jelenik meg. De az egyszerűség ténylegesen használja fel a kísérletben.

![Count tábla paraméterek módosítása](./media/hive-criteo-walkthrough/PfCHkVg.png)

Ebben az esetben láthatók, a napló-valószínűleg a rendszer használni, és a háttér oszlop ki a rendszer figyelmen kívül hagyja. Paraméterek a szemétgyűjtési bin küszöbértéket, például hogy hány pszeudo előzetes példák adja hozzá a program simítást, és hogy minden Laplacian zaj vagy nem állíthat be. A fenti speciális funkciók és Megjegyzendő, hogy az alapértelmezett értékei a felhasználókra, akik még nem használta a szolgáltatás generálása az ilyen típusú jó kiindulási pont.

##### <a name="data-transformation-before-generating-the-count-features"></a>Adatok átalakítása a count szolgáltatások létrehozása előtt
Most koncentrál egy fontos a vonat átalakítása kapcsolatos mutasson, és tesztadatok száma szolgáltatások ténylegesen létrehozása előtt. Vegye figyelembe, hogy nincsenek két **R-parancsfájl végrehajtása** előtt a count transzformálás vonatkozik az adatok használt modulok.

![R-parancsfájl-modulokba végrehajtása](./media/hive-criteo-walkthrough/aF59wbc.png)

Az első R-parancsfájl a következő:

![Első R-parancsfájl](./media/hive-criteo-walkthrough/3hkIoMx.png)

Az R-parancsfájl az oszlopok átnevezi "Col40" "Oszlop1" nevét. Ennek oka az, a count átalakítása vár ebben a formátumban nevét.

A második R-parancsfájl kiegyensúlyozza a pozitív és negatív osztályok közötti (1 és 0 osztály rendre) mintavétellel le-a negatív osztály. Itt az R-parancsfájl bemutatja, hogyan ehhez:

![Második R-parancsfájl](./media/hive-criteo-walkthrough/91wvcwN.png)

A egyszerű R-parancsfájl a "pos\_Neg.\_arány" adja meg a pozitív és negatív osztályok közötti egyensúly szolgál. Ez azért fontos, mivel javítása osztály egyensúlyhiány általában rendelkezik jobb teljesítményt nyújt, a besorolás problémák az osztály terjesztési esetén válik egyenetlenné (ebben az esetben, hogy 3.3 % pozitív és negatív osztály 96.7 % visszaírási).

##### <a name="applying-the-count-transformation-on-our-data"></a>A count transzformáció alkalmazása az adatokon
Végül, használhatja a **alkalmazása átalakítása** modul a count átalakítások alkalmazni a tanítási és adatkészletek tesztelése. Ez a modul időt vesz igénybe a mentett száma átalakítása egy bemeneti és a más bemeneti szerelvény- vagy tesztkörnyezetben adatkészletek és count szolgáltatásokkal rendelkező adatokat ad vissza. Az itt látható:

![Átalakítás modul alkalmazása](./media/hive-criteo-walkthrough/xnQvsYf.png)

##### <a name="an-excerpt-of-what-the-count-features-look-like"></a>A count funkciókra kivonat tűnik.
Fontos információkat tartalmazó megtekintéséhez Mi a count szolgáltatások abban az esetben, ha. A cikkből a következő:

![Szolgáltatások száma](./media/hive-criteo-walkthrough/FO1nNfw.png)

A cikkből jeleníti meg, az oszlopok megszámlált, lekérése a számát, valamint naplófájl meg kívül bármely megfelelő backoffs valószínűségét.

Most már készen áll az Azure Machine Learning modell ezen átalakított adatkészletek létrehozásához. A következő szakasz bemutatja, hogyan ehhez.

### <a name="step3"></a>3. lépés: Build, betanítását és pontszámok számolása

#### <a name="choice-of-learner"></a>Tanuló kiválasztása
Először meg kell választania egy tanuló. A két osztályú súlyozott döntési fa használják a tanuló. Az alábbiakban a alapértelmezett beállításait a tanuló:

![Két osztályú súlyozott döntési fa paraméterek](./media/hive-criteo-walkthrough/bH3ST2z.png)

A kísérleti fázisú funkciókat válassza ki az alapértelmezett értékeket. Vegye figyelembe, hogy az alapértelmezett értékek általában értelmezhető-e, és egy jó módja gyors alaptervek a teljesítményre. Abszolút paraméterek, ha úgy dönt, hogy miután alapterv javíthatja a teljesítményt.

#### <a name="train-the-model"></a>A modell betanítása
Képzési, egyszerűen meghívni egy **tanítási modell** modul. A két bemenet rá a következők: a két osztályú súlyozott döntési fa tanuló és az vonat adatkészletet. Ez itt látható:

![Train Model-modul](./media/hive-criteo-walkthrough/2bZDZTy.png)

#### <a name="score-the-model"></a>A modell pontozása
Ha elvégezte a modell betanítását, készen áll a tesztelési adathalmazon pontozásához és kiértékelheti a teljesítményét. Ehhez használja a **Score Model** modul, valamint az alábbi ábrán látható egy **modell kiértékelése** modul:

![A Score model (Modell montozása) modul](./media/hive-criteo-walkthrough/fydcv6u.png)

### <a name="step4"></a>4. lépés: A modell kiértékelése
Végezetül elemezni kell a modell teljesítményét. A két osztály (bináris) besorolás problémákat általában jó mérték a AUC jelenti. Ez megjelenítéséhez, bekötése a **Score Model** modul egy **modell kiértékelése** a modul. Gombra kattintva **Visualize** a a **modell kiértékelése** modul a következő hasonló grafikus eredményez:

![A modul táblázatos adatbázisba kerülő modell kiértékelése](./media/hive-criteo-walkthrough/0Tl0cdg.png)

A bináris (vagy két osztály) besorolás problémák, az előrejelzés pontosságát jó mérték a terület a görbe (AUC). A következő szakasz jeleníti meg az eredményeket a tesztelési adatkészletnél ebben a modellben. Ahhoz, hogy ez, kattintson a jobb gombbal a kimeneti portjára a **modell kiértékelése** modult, majd **Visualize**.

![Modell kiértékelése modul megjelenítése](./media/hive-criteo-walkthrough/IRfc7fH.png)

### <a name="step5"></a>5. lépés: A modell egy webszolgáltatás közzététele.
Az Azure Machine Learning modell fuss legalább webszolgáltatásként közzétételére képes lehetővé teszi az értékes széles körben elérhető, hogy azok. Ezt követően, bárki is hívásokat a webszolgáltatással, hogy az előrejelzés van szükségük, és a webszolgáltatás a modellt használ, ezek előrejelzéseket vissza bemeneti adatokkal.

Ehhez először mentse a betanított modell Trained Model-objektumként. Ehhez kattintson a jobb gombbal a **tanítási modell** modul, és használja a **Trained Model elmentse** lehetőséget.

A következő létrehozni bemeneti és kimeneti portot a webszolgáltatáshoz:

* egy bemeneti porthoz adatok kerülnek, ugyanolyan formában az adatokat, amelyekre szüksége van az előrejelzés időt vesz igénybe.
* a kimeneti portra a pontozott címkék és a kapcsolódó valószínűség adja vissza.

#### <a name="select-a-few-rows-of-data-for-the-input-port"></a>Jelölje be néhány sornyi adatot a bemeneti port
Célszerű használni egy **SQL-átalakítás alkalmazása** modul csak 10 sorok szolgáljanak, a bemeneti porthoz adatok kiválasztásához. Ezeknek az itt látható az SQL-lekérdezést használja bemeneti porthoz adatsorokat kiválasztása:

![Bemeneti porthoz adatok](./media/hive-criteo-walkthrough/XqVtSxu.png)

#### <a name="web-service"></a>Webszolgáltatás
Most már készen áll egy kis kísérletet, amely segítségével a webszolgáltatás futtatásához.

#### <a name="generate-input-data-for-webservice"></a>A webszolgáltatás bemeneti adatok létrehozása
Zeroth lépésként a count tábla túl nagy, mivel igénybe vehet néhány sornyi vizsgálati adatok, és hozhat létre a kimeneti adatokat, count szolgáltatásokkal. Ez a bemeneti adatok formátumát a webservice lehetnek. Ez itt látható:

![A bemeneti adatok táblázatos adatbázisba kerülő létrehozása](./media/hive-criteo-walkthrough/OEJMmst.png)

> [!NOTE]
> A bemeneti adatok formátuma, használja a KIMENETÉT a **száma Featurizer** modul. Ez kísérletezhet futtató befejeződik, ha a kimenetét mentse a **száma Featurizer** adatkészletként modul. Ez az adatkészlet szolgál a webservice a bemeneti adatok.
> 
> 

#### <a name="scoring-experiment-for-publishing-webservice"></a>Kísérlet pontozás közzétételi webszolgáltatás
Először is látható, néz ez. Az alapvető struktúra egy **Score Model** modul, amely a betanított modell objektum és a bemeneti adatok segítségével az előző lépésben létrehozott néhány sornyi fogadja el a **száma Featurizer** modul. "Válassza oszlopok a Dataset" segítségével projekt Scored címkéket és a pontszám valószínűség.

![Oszlopok kiválasztása az adathalmaz](./media/hive-criteo-walkthrough/kRHrIbe.png)

Értesítés az **Select Columns in Dataset** modul "kiszűrése" adatok egy adatkészletből is használható. A tartalom itt látható:

![A Select Columns in Dataset modul szűrését](./media/hive-criteo-walkthrough/oVUJC9K.png)

Ahhoz, hogy a kék bemeneti és kimeneti portok, egyszerűen kattint **készítse elő a webservice** jobb alsó. Ez a kísérlet is lehetővé teszi, hogy a webszolgáltatás közzététele: kattintson a **WEBSZOLGÁLTATÁS közzététele** ikonra az alsó jobb, látható itt:

![Webszolgáltatás](./media/hive-criteo-walkthrough/WO0nens.png)

Miután közzétette a webservice, átirányítja az egy oldal, amely így néz ki:

![Webes szolgáltatás irányítópult](./media/hive-criteo-walkthrough/YKzxAA5.png)

Figyelje meg, hogy a két hivatkozások webservices bal oldalán:

* A **kérelem/válasz** szolgáltatás (vagy RR-EKET) egyetlen előrejelzéseket készült, és mi van szükség a workshop a rendszer.
* A **KÖTEGELT végrehajtási** szolgáltatás (BES) szolgál a kötegelt előrejelzéseket, és megköveteli, hogy a bemeneti adatok találhatók az Azure Blob Storage előrejelzéseket készítsen használja-e.

A hivatkozásra kattintva **kérelem/válasz** vesz igénybe, hogy olyan velünk előre konzerv C#, python, és R. az Ez a kód a WebService részére a hívások kényelmesen használható. Vegye figyelembe, hogy az API-kulcsot, ezen az oldalon kell használható a hitelesítéshez.

Célszerű a python kódot másolja át a IPython jegyzetfüzet új cellára.

Íme egy részének a python kódját a megfelelő API-kulccsal.

![Python-kód](./media/hive-criteo-walkthrough/f8N4L4g.png)

Vegye figyelembe, hogy az alapértelmezett API-kulcsot helyébe a webservices API-kulcsot. Kattintson a **futtatása** ezen a cellán egy IPython a notebook adja eredményül a következő választ:

![IPython válasz](./media/hive-criteo-walkthrough/KSxmia2.png)

A két tesztelése (JSON keretein belül a python-parancsfájl) ismételt példák, vissza válaszok formájában "Scored valószínűség Scored címke". Ebben az esetben az alapértelmezett értékeket kell megválasztani, hogy az előre előre összeállított kód szolgál a (0 összes numerikus oszlopot és a karakterlánc a "érték" minden kategorikus oszlop).

Ezzel befejezte a forgatókönyv bemutatja, hogyan kezeli az Azure Machine Learning segítségével nagy méretű adatkészlet. Az adatok egy terabájt használatába, összeállított egy előrejelzési modellt, és telepítve lett egy webszolgáltatás, a felhőben.

