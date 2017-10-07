---
title: "művelet – 1 TB-os dataset Azure HDInsight Hadoop-fürtök használata a csapat az tudományos folyamata hello |} Microsoft Docs"
description: "Hello Team adatok tudományos folyamat használja egy végpont forgatókönyv egy HDInsight Hadoop alkalmazó toobuild fürt, és egy nagy méretű (1 TB) nyilvánosan elérhető adatkészlet a modell rendszerbe állítása"
services: machine-learning,hdinsight
documentationcenter: 
author: bradsev
manager: jhubbard
editor: cgronlun
ms.assetid: 72d958c4-3205-49b9-ad82-47998d400d2b
ms.service: machine-learning
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/29/2017
ms.author: bradsev
ms.openlocfilehash: 59b2af02e7840cb60a4b5b2f2c8ab0611df198ec
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="hello-team-data-science-process-in-action---using-an-azure-hdinsight-hadoop-cluster-on-a-1-tb-dataset"></a>hello Team adatok tudományos folyamat művelet:-1 TB-os dataset Azure HDInsight Hadoop-fürtök használata

Ebben a bemutatóban bemutatjuk használatával Team adatok tudományos folyamat hello egy végpont forgatókönyvet, és egy [Azure HDInsight Hadoop-fürt](https://azure.microsoft.com/services/hdinsight/) toostore, megismerkedhet a beállítást, a visszafejtés és nyilvánosan le a mintaadatokat egy hello rendelkezésre álló [Criteo](http://labs.criteo.com/downloads/download-terabyte-click-logs/) adatkészletek. A bináris osztályozási modell Azure Machine Learning toobuild ezeken az adatokon használjuk. Is megmutatjuk, hogyan toopublish ezek közül a modellek webszolgáltatásként.

Akkor is lehetséges toouse egy IPython notebook tooaccomplish hello feladatok jelenik meg ebben a forgatókönyvben. Felhasználók, akik volna, ez a megközelítés konzultáljon tootry például hello [Criteo forgatókönyv Hive ODBC-kapcsolat használatával](https://github.com/Azure/Azure-MachineLearning-DataScience/blob/master/Misc/DataScienceProcess/iPythonNotebooks/machine-Learning-data-science-process-hive-walkthrough-criteo.ipynb) témakör.

## <a name="dataset"></a>Criteo adatkészlet leírása
hello Criteo adata kattintson előrejelzés adatkészletre mutató körülbelül 370 GB tömörített gzip TSV fájlok (tömörítetlen ~1.3TB), amely több mint 4.3 milliárd rögzíti. A 24 napos származik kattintson által elérhetővé tett adatok [Criteo](http://labs.criteo.com/downloads/download-terabyte-click-logs/). Hello kényelmét szolgálja adatszakértőkön azt kell unzipped a toous tooexperiment elérhető adatokat.

Ez az adatkészlet egyes rekord 40 oszlopokat tartalmazza:

* hello első oszlopa egy címke oszlopot, amely azt jelzi, hogy a felhasználó rákattint egy **hozzáadása** (érték-1) vagy nem kattintson egy (a 0 érték)
* Ezután 13 oszlopok numerikus, és
* utolsó 26 kategorikus oszlopok

hello oszlopok vannak anonimizált adatokon alapul, és használja fel nevek sorozata: "Col1" (a címke oszlop hello) túl "Col40" (a hello utolsó kategorikus oszlop).            

Íme hello kivonat első 20 oszlopok a dataset adatkészletből két megfigyelések (sorok):

    Col1    Col2    Col3    Col4    Col5    Col6    Col7    Col8    Col9    Col10    Col11    Col12    Col13    Col14    Col15            Col16            Col17            Col18            Col19        Col20

    0       40      42      2       54      3       0       0       2       16      0       1       4448    4       1acfe1ee        1b2ff61f        2e8b2631        6faef306        c6fc10d3    6fcd6dcb           
    0               24              27      5               0       2       1               3       10064           9a8cb066        7a06385f        417e6103        2170fc56        acf676aa    6fcd6dcb                      

Nincsenek hiányzó értékek mindkét hello numerikus és kategorikus oszlopban ehhez az adatkészlethez. Azt ismerteti, hogy egy egyszerű módszer a hiányzó értékek hello kezelése. További részletek hello adatok írja során a Hive táblák tároljuk őket.

**Definíciója:** *Átkattintós gyakorisága (Parancsra):* Ez az hello adatok kattintással hello százaléka. Criteo DataSet adatkészletben hello Parancsra, de hamarosan 3.3 % 0.033.

## <a name="mltasks"></a>Előrejelzés feladatok példák
Ez a forgatókönyv két minta előrejelzés problémákat tárgyalja:

1. **Bináris osztályozási**: előrejelzi, hogy a felhasználó rákattint egy hozzáadása:
   
   * Osztály: 0: Nincs kattintson
   * 1 osztály: kattintson a
2. **Regressziós**: hello valószínűségét, hogy egy felhasználó funkciókat ad kattintson előrejelzi.

## <a name="setup"></a>Tudományos adatok mentése beállítása egy HDInsight szabható Hadoop bemutatása-fürt
**Megjegyzés:** Ez általában az egy **Admin** feladat.

Állítsa be a HDInsight-fürtökkel három lépésben prediktív elemzési megoldások létrehozásához Azure Adattudomány környezetében:

1. [Hozzon létre egy tárfiókot](../storage/common/storage-create-storage-account.md): ezt a tárfiókot az Azure Blob Storage tárolóban használt toostore adatai. a HDInsight-fürtök használt hello adatok itt tárolja.
2. [Azure HDInsight Hadoop-fürtök testreszabása Adattudomány](machine-learning-data-science-customize-hadoop-cluster.md): Ezzel a lépéssel hoz létre egy Azure HDInsight Hadoop-fürt összes csomópontján telepítve 64 bites Anaconda Python 2.7. Nincsenek két fontos lépések az (a jelen témakörben ismertetett) toocomplete hello HDInsight-fürtök testreszabása.
   
   * Az 1. lépésben a HDInsight-fürthöz létrehozott létrehozásakor hello tárfiókban kell kapcsolni. Ez a tárfiók hello fürtön belül feldolgozható adatainak eléréséhez használatos.
   * Létrehozás után engedélyeznie kell a távelérés toohello átjárócsomópont hello fürt. Az itt megadott (eltérnek a saját létrehozásakor hello fürt számára megadott) hello távoli hozzáférési hitelesítő adatok megjegyzése: mintaadathalmazt toocomplete hello eljárásokat követve.
3. [Hozzon létre egy Azure ML munkaterületet](machine-learning-create-workspace.md): az Azure Machine Learning-munkaterület használata a machine learning modellek létrehozása után egy kezdeti adatok feltárása, és a mintavételi hello HDInsight-fürt le.

## <a name="getdata"></a>GET és egy nyilvános forrásból származó adatokat
Hello [Criteo](http://labs.criteo.com/downloads/download-terabyte-click-logs/) dataset hozzáférhet a hello hivatkozásra kattint, hello használati feltételek elfogadása és megadása. Pillanatkép készítése néz Ez itt jelenik meg:

![Criteo feltételek elfogadása](./media/machine-learning-data-science-process-hive-criteo-walkthrough/hLxfI2E.png)

Kattintson a **Folytatás tooDownload** tooread további hello dataset és elérhetőségét.

hello adatok találhatók a nyilvános [Azure blob storage](../storage/blobs/storage-dotnet-how-to-use-blobs.md) helye: wasb://criteo@azuremlsampleexperiments.blob.core.windows.net/raw/. hello "wasb" tooAzure Blob Storage-helyre hivatkozik. 

1. a nyilvános blob Storage tárolóban hello adatok három almappák tömörítetlen adatok állnak.
   
   1. almappa hello *nyers és száma/* tartalmaz hello első 21 napnyi adatot - nap után\_00 tooday\_20
   2. almappa hello *nyers vonat / /* áll az adatok egy nap napi\_21
   3. almappa hello *nyers és tesztelési célú/* két napnyi adat, áll nap\_22-es és a nap\_23
2. Azok számára, akik toostart hello nyers gzip adatokkal, ezek is hello fő mappában találhatók *nyers /* day_NN.gz, ahol NN 00 too23 tartalma szerint.

Egy másik módszert tooaccess vizsgálatát, és ezeket az adatokat bármely helyi letöltések kifejtett Ez az útmutató későbbi szakaszában Hive táblák létrehozásakor nem igénylő modell.

## <a name="login"></a>Jelentkezzen be toohello fürt headnode
a fürtbe hello használata hello toohello headnode toolog [Azure-portálon](https://ms.portal.azure.com) toolocate hello fürt. Hello HDInsight elefánt ikonra a bal oldali hello, és kattintson duplán a fürt hello neve. Keresse meg a toohello **konfigurációs** lapon hello CONNECT ikonra a hello hello lap aljára, és adja meg a távelérés hitelesítő adatokat. Ezzel megnyitná toohello headnode hello fürt.

Íme egy tipikus első bejelentkezéskor toohello fürt headnode néz:

![Jelentkezzen be toocluster](./media/machine-learning-data-science-process-hive-criteo-walkthrough/Yys9Vvm.png)

Hello bal oldali hello "Hadoop parancssori", amely a workhorse hello adatok feltárási látható. Azt is láthatja, két hasznos URL-címek – "Hadoop Yarn állapota" és "Hadoop neve csomópont". hello csomópont URL-CÍMÉT hello fürtkonfiguráció részleteket nyújt és hello yarn állapot URL-CÍMÉT jeleníti meg a feladat előrehaladását.

Most azt vannak beállítva, és kész toobegin első hello forgatókönyv része: Hive eszközzel és adatok Azure Machine Learning Felkészülés az adatok feltárása.

## <a name="hive-db-tables"></a>Hive-adatbázis és tábla létrehozása
toocreate Hive táblák a Criteo az adatkészlethez, nyissa meg hello ***Hadoop parancssori*** a hello hello átjárócsomópont asztalát, és írja be a hello Hive directory hello parancs megadásával

    cd %hive_home%\bin

> [!NOTE]
> Ez a forgatókönyv minden Hive parancsot futtatja hello Hive bin / directory kérdés. Ezzel létrehozza az elérési út probléma merül fel automatikusan. "Struktúra directory prompt" hello kifejezéseket használjuk a "struktúra bin / directory prompt", és a "Hadoop parancssori" gombokra.
> 
> [!NOTE]
> tooexecute bármely Hive-lekérdezést egy mindig használhatja a következő parancsok hello:
> 
> 

        cd %hive_home%\bin
        hive

Hello után Hive REPL jelenik meg, amelyen egy "struktúra >" aláírásához, egyszerűen Kivágás, és illessze be a hello lekérdezés tooexecute azt.

hello alábbi kód létrehoz egy "criteo" adatbázist és majd hoz létre 4 táblák:

* egy *számok generálásához tábla* nap napi épülő\_00 tooday\_20,
* egy *tábla használatra, vonat dataset hello* nap épülő\_21, és
* két *hello használni táblák tesztelése adatkészletek* nap épülő\_22-es és a nap\_23 kulcsattribútumokkal.

A Microsoft ossza fel a tesztelési adatkészletnél két különböző tábla mert hello nap egyik Szabadnap, és toodetermine azt szeretnénk, ha hello modell észleli hello átkattintós sebessége szünnap és nem szünnap közötti különbségeket.

parancsfájl hello [minta &#95; hive &#95; hozzon létre &#95; criteo &#95; adatbázis &#95; és &#95;tables.hql](https://github.com/Azure/Azure-MachineLearning-DataScience/blob/master/Misc/DataScienceProcess/DataScienceScripts/sample_hive_create_criteo_database_and_tables.hql) kényelmi itt jelenik meg:

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

A Microsoft ne feledje, hogy ezek a táblázatok külső, azt egyszerűen pont tooAzure Blob Storage (wasb) helyét.

**Két módon tooexecute bármely Hive-lekérdezések, amelyek most említik van.**

1. **Hive REPL parancssori használata hello**: hello először tooissue "hive" parancsot, és másolja és illessze be a lekérdezést a Hive REPL parancssori hello. toodo a, tegye:
   
        cd %hive_home%\bin
        hive
   
     Most, hello parancssori REPL, Kivágás és beillesztés a hello lekérdezés végrehajtása.
2. **Mentés lekérdezi tooa fájl- és hello parancs végrehajtása**: hello második egy toosave hello lekérdezések tooa .hql fájl ([minta &#95; hive &#95; hozzon létre &#95; criteo &#95; adatbázis &#95; és &#95;tables.hql](https://github.com/Azure/Azure-MachineLearning-DataScience/blob/master/Misc/DataScienceProcess/DataScienceScripts/sample_hive_create_criteo_database_and_tables.hql)) és arról, hogy a probléma hello következő majd parancsot a tooexecute hello lekérdezést:
   
        hive -f C:\temp\sample_hive_create_criteo_database_and_tables.hql

### <a name="confirm-database-and-table-creation"></a>Erősítse meg az adatbázis és tábla létrehozása
A következő Megerősítjük hello hello adatbázisának létrehozása a parancs követően – hello Hive bin hello / directory parancssorba:

        hive -e "show databases;"

Ezen a következő:

        criteo
        default
        Time taken: 1.25 seconds, Fetched: 2 row(s)

Ez megerősíti, hogy hello új adatbázis, "criteo" hello létrehozását.

toosee milyen táblák létrehozott, egyszerűen kapni hello parancsot itt hello Hive bin / directory parancssorba:

        hive -e "show tables in criteo;"

A következő kimeneti hello majd látható:

        criteo_count
        criteo_test_day_22
        criteo_test_day_23
        criteo_train
        Time taken: 1.437 seconds, Fetched: 4 row(s)

## <a name="exploration"></a>Az adatok feltárása a Hive
Most a folyamatban van, kész toodo néhány alapvető adatok feltárása struktúrában. A Microsoft hello vonat szereplő példák hello száma alapján kezdődik, és adattáblák tesztelése.

### <a name="number-of-train-examples"></a>Vonat példák száma
hello tartalmát [minta &#95; hive &#95; száma &#95; vonat &#95; tábla &#95;examples.hql](https://github.com/Azure/Azure-MachineLearning-DataScience/blob/master/Misc/DataScienceProcess/DataScienceScripts/sample_hive_count_train_table_examples.hql) itt látható:

        SELECT COUNT(*) FROM criteo.criteo_train;

Ez eredményez:

        192215183
        Time taken: 264.154 seconds, Fetched: 1 row(s)

Azt is megteheti, egy lehetséges, hogy is kiadhatnak hello parancs követően – hello Hive bin / directory parancssorba:

        hive -f C:\temp\sample_hive_count_criteo_train_table_examples.hql

### <a name="number-of-test-examples-in-hello-two-test-datasets"></a>Tesztelési példák hello két teszt adatkészletek száma
A Microsoft most megadott számú hello hello két teszt adatkészletek példák. hello tartalmát [minta &#95; hive &#95; száma &#95; criteo &#95; teszt &#95; nap &#95; 22 &#95; tábla &#95;examples.hql](https://github.com/Azure/Azure-MachineLearning-DataScience/blob/master/Misc/DataScienceProcess/DataScienceScripts/sample_hive_count_criteo_test_day_22_table_examples.hql) itt:

        SELECT COUNT(*) FROM criteo.criteo_test_day_22;

Ez eredményez:

        189747893
        Time taken: 267.968 seconds, Fetched: 1 row(s)

A szokásos módon, előfordulhat, hogy is nevezzük hello parancsfájl a hello Hive bin / könyvtár hello parancsot a parancssor:

        hive -f C:\temp\sample_hive_count_criteo_test_day_22_table_examples.hql

Végül azt vizsgálja meg az hello tesztelési adatkészletnél nap alapján tesztelési példák hello száma\_23.

hello parancs toodo Ez az egyik csak látható hasonló toohello (tekintse meg a túl[minta &#95; hive &#95; száma &#95; criteo &#95; teszt &#95; nap &#95; 23 &#95;examples.hql](https://github.com/Azure/Azure-MachineLearning-DataScience/blob/master/Misc/DataScienceProcess/DataScienceScripts/sample_hive_count_criteo_test_day_23_examples.hql)):

        SELECT COUNT(*) FROM criteo.criteo_test_day_23;

Ezen a következő:

        178274637
        Time taken: 253.089 seconds, Fetched: 1 row(s)

### <a name="label-distribution-in-hello-train-dataset"></a>Címke terjesztési hello vonat adatkészlet
hello címke terjesztési hello vonat adatkészlet jelentőséggel bír. toosee, tartalmát megmutatjuk [minta &#95; hive &#95; criteo &#95; címke &#95; terjesztési &#95; vonat &#95;table.hql](https://github.com/Azure/Azure-MachineLearning-DataScience/blob/master/Misc/DataScienceProcess/DataScienceScripts/sample_hive_criteo_label_distribution_train_table.hql):

        SELECT Col1, COUNT(*) AS CT FROM criteo.criteo_train GROUP BY Col1;

Ez a hello címke terjesztési eredményez:

        1       6292903
        0       185922280
        Time taken: 459.435 seconds, Fetched: 2 row(s)

Vegye figyelembe, hogy hello százalékos pozitív címkék készül 3.3 % (konzisztens hello eredeti adatkészlet).

### <a name="histogram-distributions-of-some-numeric-variables-in-hello-train-dataset"></a>Néhány numerikus változók hello hisztogram disztribúciók betanítása adatkészlet
Használhatunk struktúrájának natív "hisztogram\_numerikus" milyen hello terjesztési hello numerikus változók a következőképpen néz ki toofind működik. Az alábbiakban hello tartalmát [minta &#95; hive &#95; criteo &#95; hisztogram &#95;numeric.hql](https://github.com/Azure/Azure-MachineLearning-DataScience/blob/master/Misc/DataScienceProcess/DataScienceScripts/sample_hive_criteo_histogram_numeric.hql):

        SELECT CAST(hist.x as int) as bin_center, CAST(hist.y as bigint) as bin_height FROM
            (SELECT
            histogram_numeric(col2, 20) as col2_hist
            FROM
            criteo.criteo_train
            ) a
            LATERAL VIEW explode(col2_hist) exploded_table as hist;

Ez adja eredményül a következő hello:

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

OLDALIRÁNYÚ NÉZET – hello felbontása Hive szolgál tooproduce hello szokásos lista helyett egy SQL-szerű kimeneti kombinációja. Vegye figyelembe, hogy a hello a tábla, hello első oszlop felel meg a toohello bin center és hello második toohello bin gyakoriságát.

### <a name="approximate-percentiles-of-some-numeric-variables-in-hello-train-dataset"></a>Néhány numerikus változókra hello hozzávetőleges százalékos érték betanítása adatkészlet
A numerikus változókkal érdeklő a hello számítási hozzávetőleges százalékos érték a. Hive tartozó natív "PERCENTILIS\_hozzávetőleges" ezt az USA végzi. hello tartalmát [minta &#95; hive &#95; criteo &#95; hozzávetőleges &#95;percentiles.hql](https://github.com/Azure/Azure-MachineLearning-DataScience/blob/master/Misc/DataScienceProcess/DataScienceScripts/sample_hive_criteo_approximate_percentiles.hql) vannak:

        SELECT MIN(Col2) AS Col2_min, PERCENTILE_APPROX(Col2, 0.1) AS Col2_01, PERCENTILE_APPROX(Col2, 0.3) AS Col2_03, PERCENTILE_APPROX(Col2, 0.5) AS Col2_median, PERCENTILE_APPROX(Col2, 0.8) AS Col2_08, MAX(Col2) AS Col2_max FROM criteo.criteo_train;

Ez eredményez:

        1.0     2.1418600917169246      2.1418600917169246    6.21887086390288 27.53454893115633       65535.0
        Time taken: 564.953 seconds, Fetched: 1 row(s)

A Microsoft remark, hogy százalékos érték hello terjesztése általában-e szorosan kapcsolódó toohello hisztogram terjesztési bármely numerikus változó.         

### <a name="find-number-of-unique-values-for-some-categorical-columns-in-hello-train-dataset"></a>Hello vonat adatkészlet egyes kategorikus oszlopok egyedi értékek számának keresése
A Folytatás hello adatok feltárása, most találtunk, néhány kategorikus oszlop hello tartanak egyedi értékek száma. toodo, tartalmát megmutatjuk [minta &#95; hive &#95; criteo & #95 egyedi; &#95; értékek &#95;categoricals.hql](https://github.com/Azure/Azure-MachineLearning-DataScience/blob/master/Misc/DataScienceProcess/DataScienceScripts/sample_hive_criteo_unique_values_categoricals.hql):

        SELECT COUNT(DISTINCT(Col15)) AS num_uniques FROM criteo.criteo_train;

Ez eredményez:

        19011825
        Time taken: 448.116 seconds, Fetched: 1 row(s)

Megjegyezzük, hogy rendelkezik-e Col15 19M egyedi értékeket! Például a "egy közbeni kódolás" tooencode naïve technikák ilyen nagy dimenziós kategorikus változók használata lehessen hozni. Különösen azt ismertetik, és mutassa be nevű hatékony, hatékony módszer [tanulási a számlálás](http://blogs.technet.com/b/machinelearning/archive/2015/02/17/big-learning-made-easy-with-counts.aspx) a hatékony elleni probléma.

Ez alterület kategorikus oszlopokat, valamint az egyedi értékek számának hello megtekintésével végén azt. hello tartalmát [minta &#95; hive &#95; criteo & #95 egyedi; &#95; értékek &#95; több &#95;categoricals.hql](https://github.com/Azure/Azure-MachineLearning-DataScience/blob/master/Misc/DataScienceProcess/DataScienceScripts/sample_hive_criteo_unique_values_multiple_categoricals.hql) vannak:

        SELECT COUNT(DISTINCT(Col16)), COUNT(DISTINCT(Col17)),
        COUNT(DISTINCT(Col18), COUNT(DISTINCT(Col19), COUNT(DISTINCT(Col20))
        FROM criteo.criteo_train;

Ez eredményez:

        30935   15200   7349    20067   3
        Time taken: 1933.883 seconds, Fetched: 1 row(s)

Újra látható, hogy Col20, kivéve az összes hello más oszlopok számos egyedi értékeket tartalmaznak.

### <a name="co-occurrence-counts-of-pairs-of-categorical-variables-in-hello-train-dataset"></a>Közös előfordulási számát is párok hello vonat adatkészlet kategorikus változók

hello közös előfordulási számát is párok kategorikus változók az is fontos. Ez lehet meghatározni a hello kóddal [minta &#95; hive &#95; criteo & #95 párosított; &#95; kategorikus &#95;counts.hql](https://github.com/Azure/Azure-MachineLearning-DataScience/blob/master/Misc/DataScienceProcess/DataScienceScripts/sample_hive_criteo_paired_categorical_counts.hql):

        SELECT Col15, Col16, COUNT(*) AS paired_count FROM criteo.criteo_train GROUP BY Col15, Col16 ORDER BY paired_count DESC LIMIT 15;

A Microsoft sorrend hello számok megfordíthatja a előfordulásainak kívánt számát, és tekintse meg hello felső 15 ebben az esetben. Biztosítanak számunkra:

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

## <a name="downsample"></a>Mintaként használható adathalmazt hello Azure Machine Learning le
Felderített hello adatkészleteket, és meg mutatja, hogyan lehet végezzük az ilyen típusú változó (többek között a következőket kombinációk), azt most le hello mintaadatkészletek feltárása, hogy azt az Azure Machine Learning modellek hozhat létre. Adott azt összpontosítani hello probléma visszahívása: megadott példa attribútumok (Col2 - Col40 értékeinek szolgáltatás), azt megjósolható, hogy Oszlop1-e a 0 (nincs kattintson) vagy 1 (kattintson).

toodown a vonat sample, és tesztelje adatkészletek too1 % hello eredeti méret, struktúrájának natív RAND() függvény használjuk. hello a következő parancsfájl [minta &#95; hive &#95; criteo &#95; felbontásának &#95; vonat &#95;dataset.hql](https://github.com/Azure/Azure-MachineLearning-DataScience/blob/master/Misc/DataScienceProcess/DataScienceScripts/sample_hive_criteo_downsample_train_dataset.hql) ezt hello vonat adatkészlet végzi:

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

parancsfájl hello [minta &#95; hive &#95; criteo &#95; felbontásának &#95; teszt &#95; nap &#95; 22 &#95;dataset.hql](https://github.com/Azure/Azure-MachineLearning-DataScience/blob/master/Misc/DataScienceProcess/DataScienceScripts/sample_hive_criteo_downsample_test_day_22_dataset.hql) minderre a tesztadatok, napi\_22:

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


Végezetül, a parancsfájl hello [minta &#95; hive &#95; criteo &#95; felbontásának &#95; teszt &#95; nap &#95; 23 &#95;dataset.hql](https://github.com/Azure/Azure-MachineLearning-DataScience/blob/master/Misc/DataScienceProcess/DataScienceScripts/sample_hive_criteo_downsample_test_day_23_dataset.hql) minderre a tesztadatok, napi\_23:

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

Ennek, vagy folyamatban, a régebbi mintát készen toouse betanítása és felépítése az Azure Machine Learning modellek adathalmaz tesztelése.

Összetevő végső fontos azt áthelyezni a Machine Learning, amely aggályokat hello száma tábla tooAzure előtt. Hello következő alárendelt szakaszban arról lesz szó ez részletesen bemutatható.

## <a name="count"></a>Hello száma tábla rövid döntéseken
Látott, mert több kategorikus változók nagyon magas granularitása adható meg. A forgatókönyv azt jelenleg egy hatékony módszer néven [tanulási a számlálás](http://blogs.technet.com/b/machinelearning/archive/2015/02/17/big-learning-made-easy-with-counts.aspx) tooencode egy hatékony és hatékony módon változókhoz. További információk ezzel a technikával hello hivatkozás van.

[!NOTE]
>A forgatókönyv azt összpontosítani száma táblák tooproduce kompakt ábrázolásai nagy dimenziós kategorikus szolgáltatások használatával. Ez nem az hello csak tooencode kategorikus szolgáltatások; egyéb technikák olvashat, érdekelt felhasználók lefoglalhatják [egy-közbeni-kódolás](http://en.wikipedia.org/wiki/One-hot) és [szolgáltatáskivonatolás](http://en.wikipedia.org/wiki/Feature_hashing).
>

toobuild száma táblák hello számának adatai, a hello mappa nyers és száma használjuk hello adatokat. A felhasználók megmutatjuk szakaszban modellezési hello hogyan toobuild ezek száma táblák teljesen, vagy másik lehetőségként toouse kategorikus szolgáltatások egy előre elkészített száma táblázatban azok explorations. Mi a következő, amikor irányítjuk túl "előzetesen elkészített száma táblák", azt jelenti, hogy általunk biztosított hello száma táblák használata. Hogyan tooaccess ezek a táblázatok biztosított hello a következő szakaszban részletes útmutatást.

## <a name="aml"></a>A modell Azure Machine Learning segítségével létrehozása
A modell létrehozása az Azure Machine Learning folyamatot követi ezeket a lépéseket:

1. [Hello adatokat lekérni a Hive táblák az Azure gépi tanulás](#step1)
2. [Hello kísérlet létrehozása: hello adatok és count táblákkal featurize törlése](#step2)
3. [Build, tanítási és pontszám hello modell](#step3)
4. [Hello modell kiértékelése](#step4)
5. [Hello modell egy webszolgáltatás közzététele](#step5)

Most már készen áll a toobuild modellek az Azure Machine Learning studio folyamatban. A lefelé mintaadatokat Hive táblák hello fürt értékként menti. Hello Azure Machine Learning használjuk **és adatokat importálhat** modul tooread ezeket az adatokat. a következőkben hello hitelesítő adatok tooaccess hello tárfiók a fürt vannak megadva.

### <a name="step1"></a>1. lépés: Adatokat lekérni a Hive táblák az Azure Machine Learning hello adatokat importálhat modullal, és válassza ki azt a tartalmazó gépi tanulási kísérlet
Kiválasztásával indítsa el a **+ új** -> **kísérlet** -> **üres kísérlet**. Ezt követően a hello **keresési** hello felül balra, keresse meg az "Adatok importálása" mezőbe. A fogd és vidd hello **és adatokat importálhat** toohello kísérlet vászonra (hello üdvözlő képernyőt középső része) toouse hello modul az adatelérési modul.

Ez az, hogy milyen hello **és adatokat importálhat** tűnik hello Hive tábla adatainak lekérése közben:

![Adatok importálása adatok beolvasása](./media/machine-learning-data-science-process-hive-criteo-walkthrough/i3zRaoj.png)

A hello **és adatokat importálhat** modul, feltéve, hogy a hello kép csak példák hello rendezésére a értékekkel hello paramétereket hello értékének tooprovide kell. Íme néhány általános útmutatást ki hello paraméter toofill hogyan hello beállítani a **és adatokat importálhat** modul.

1. Válassza ki a "Hive-lekérdezések" a **adatforrás**
2. A hello **adatbázis-lekérdezés Hive** mezőbe egy egyszerű jelöljön ki * FROM < a\_adatbázis\_name.your\_tábla\_neve >-elegendő.
3. **Hcatalog kiszolgáló URI azonosítója**: Ha a fürt "abc", akkor ez az egyszerűen: https://abc.azurehdinsight.net
4. **Hadoop felhasználói fiók nevét**: hello helyreállításkor hello fürthöz beüzemelési kiválasztott hello felhasználónevet. (Nem hello távelérési felhasználónév!)
5. **Hadoop felhasználói fiók jelszavát**: hello helyreállításkor hello fürthöz beüzemelési kiválasztott hello felhasználónév hello jelszava. (Nem hello távelérési jelszó!)
6. **Az kimeneti adatok**: válassza az "Azure"
7. **Az Azure storage-fiók neve**: hello hello-fürthöz tartozó tárfiók
8. **Azure storage-fiók kulcs**: hello hello-fürthöz tartozó hello tárfiók kulcsa.
9. **Az Azure Tárolónév**: Ha hello fürtnév "abc", akkor egyszerűen "abc", ami általában.

Egyszer hello **és adatokat importálhat** befejeződik (látható zöld hello osztásjelek hello modul) adatok, ezek az adatok mentése (a választott nevű) egy adatkészlet. Mi a következőhöz hasonló:

![Adatok importálása az adatok mentése](./media/machine-learning-data-science-process-hive-criteo-walkthrough/oxM73Np.png)

Kattintson a jobb gombbal hello kimeneti portjára hello **és adatokat importálhat** modul. Ez azt jelzi egy **dataset elmentse** beállítás és egy **Visualize** lehetőséget. Hello **Visualize** lehetőséget, ha kattintott, jeleníti meg a hello adatok, és a jobb oldali panelen, amely akkor hasznos, ha néhány statisztikák 100 sorát. toosave adatok, egyszerűen válassza **dataset elmentse** , és kövesse az utasításokat.

tooselect mentett hello dataset használható a machine learning kísérletben, keresse meg a hello adatkészletekhez hello **keresési** mezőben hello a következő ábrán látható. Ezután egyszerűen ki adott hello név típus hello dataset részben, és húzza hello alakzatot dataset tooaccess hello fő panelje. Húzza azt hello fő panelen kijelöli a machine learning modellezési használatra.

![Hello fő panelre Drage adatkészlet](./media/machine-learning-data-science-process-hive-criteo-walkthrough/cl5tpGw.png)

> [!NOTE]
> Ehhez a hello tanítási és a hello teszt adatkészletek. Továbbá ne feledje, toouse hello adatbázis nevét és a táblaneveket, hogy a megadott, erre a célra. hello hello ábrán használt értékei kizárólag az ábra purposes.* *
> 
> 

### <a name="step2"></a>2. lépés: Egy egyszerű kísérlet létrehozása az Azure Machine Learning toopredict kattintással / egyetlen kattintással
Az Azure ML kísérlet így néz ki:

![Machine Learning kísérlet](./media/machine-learning-data-science-process-hive-criteo-walkthrough/xRpVfrY.png)

A Microsoft hello legfontosabb összetevők, ez a kísérlet most vizsgálja meg. Ne feledje a mentett betanítása és adathalmazokat tooour kísérletvászonra először tesztelje toodrag kell.

#### <a name="clean-missing-data"></a>Hiányzó adatok törlése
Hello **Clean Missing Data** modul does a neve alapján: törli a hiányzó adatokat, hogy a felhasználó által megadott lehet. Ez a modul be keresése, azt jelenik meg:

![Hiányzó adatok törlése](./media/machine-learning-data-science-process-hive-criteo-walkthrough/0ycXod6.png)

Itt választottuk tooreplace összes hiányzó értéket 0-val. Nincsenek, valamint egyéb beállításokat, amelyek hello legördülő listák megnyílásának hello modul megnézzük látható.

#### <a name="feature-engineering-on-hello-data"></a>Jellemzőkiemelés hello adatokon
Egyedi értékeket az egyes kategorikus szolgáltatások nagy adatkészletek több millió lehet. A natív módszerek, például egy közbeni kódolását jelző ilyen nagy dimenziós kategorikus funkciók használata teljesen amelyeknek. Ebben a bemutatóban bemutatjuk, hogyan toouse száma funkciók beépített Azure Machine Learning modulok toogenerate tömörítésével nagy dimenziós kategorikus változókhoz ábrázolásai. hello end-eredménye modell mérete kisebb lesz, gyorsabb képzési és teljesítménymutatók, amelyek teljesen összehasonlítható toousing más módszereket.

##### <a name="building-counting-transforms"></a>Leltár átalakítások felépítése
hello használjuk toobuild száma funkciókat, **Build számbavételi átalakítási** elérhető az Azure Machine Learning modulban. hello modul így néz ki:

![Átalakítás számbavételi modul létrehozása](./media/machine-learning-data-science-process-hive-criteo-walkthrough/e0eqKtZ.png)
![Build számbavételi átalakító modul](./media/machine-learning-data-science-process-hive-criteo-walkthrough/OdDN0vw.png)

> [!IMPORTANT] 
> A hello **oszlopok száma** mezőbe, hogy jelenítsük tooperform megjeleníti ilyen oszlopokat azt adja meg. Ezek rendszerint (említettek) nagy dimenziós kategorikus oszlopok. Hello indításkor azt említett adott hello Criteo dataset adatkészletben 26 kategorikus oszlopok: az Col15 tooCol40. Itt, hogy ezek száma és az indexek adjon (a 15 too40 látható vesszővel elválasztva).
> 

toouse hello moduljának hello MapReduce mód (megfelelő nagy adatkészletek), nem kell hozzáférni tooan HDInsight Hadoop-fürt (erre a célra egy szolgáltatás feltárása a felhasználhatók hello) és a hitelesítő adatok. hello előző ábrák bemutatják, milyen hello kitöltött értékeket néz (cserélje le az azokat, a saját használati eset illusztrációs célokat hello értékek).

![Modul paraméterei](./media/machine-learning-data-science-process-hive-criteo-walkthrough/05IqySf.png)

Hello a fenti ábrán szereplő megmutatjuk, hogyan tooenter hello bemeneti blob helyét. Ezen a helyen száma táblák létrehozása fenntartott hello adatokat tartalmaz.

Ez a modul végeztével is mentheti a hello átalakító később kattintson a jobb gombbal a hello modult, majd válassza a hello **átalakító elmentse** lehetőséget:

!["Mentés másként átalakító" beállítás](./media/machine-learning-data-science-process-hive-criteo-walkthrough/IcVgvHR.png)

A fenti kísérlet architektúra esetén hello dataset "ytransform2" pontosan száma átalakító mentett tooa felel meg. Ehhez a kísérlethez hello hátralévő, feltételezzük, hogy a használt hello olvasó egy **Build számbavételi átalakítási** modul bizonyos adatok toogenerate számát, és majd is használja azokat számok toogenerate száma szolgáltatásait hello vonat, és tesztelje a adatkészletek.

##### <a name="choosing-what-count-features-tooinclude-as-part-of-hello-train-and-test-datasets"></a>Milyen funkciókat tooinclude száma hello vonat részeként és adatkészletek tesztelése kiválasztása
Miután tudunk átalakítás kész számát, hello milyen funkciók tooinclude adja meg a tanítási és a felhasználónak adatkészletekhez hello tesztelése **száma tábla paraméterek módosítása** modul. Ez a modul csak megmutatjuk Itt a teljesség, de az egyszerűség ténylegesen használja fel a kísérletben.

![Count tábla paraméterek módosítása](./media/machine-learning-data-science-process-hive-criteo-walkthrough/PfCHkVg.png)

Ebben az esetben láthatók, mivel azt választotta, toouse csak hello napló-valószínűleg és tooignore hello biztonsági oszlop ki. Azt is beállíthatja például hello szemétgyűjtési bin küszöbértéket, a program simítást, hogy hány pszeudo előzetes példák tooadd paramétereket, és hogy toouse bármely Laplacian zaj vagy nem. A fenti speciális funkciók és toobe nincs jelezve, hogy hello alapértelmezett értékei a felhasználók számára szolgáltatás generációs típusú új toothis jó kiindulási pont.

##### <a name="data-transformation-before-generating-hello-count-features"></a>Adatok átalakítása hello száma szolgáltatások létrehozása előtt
Most azt a vonat átalakítása kapcsolatos fontos megfontolandó összpontosít, és tesztelje a adatok előzetes tooactually száma szolgáltatások létrehozásakor. Vegye figyelembe, hogy nincsenek két **R-parancsfájl végrehajtása** előtt hello számának átalakító tooour adatai érvénybe lépni használt modulok.

![R-parancsfájl-modulokba végrehajtása](./media/machine-learning-data-science-process-hive-criteo-walkthrough/aF59wbc.png)

Hello első R-parancsfájl a következő:

![Első R-parancsfájl](./media/machine-learning-data-science-process-hive-criteo-walkthrough/3hkIoMx.png)

Az R-parancsfájl, a Microsoft nevezze át az oszlopok toonames "Oszlop1" túl "Col40". Ennek az az oka hello száma átalakító vár ebben a formátumban nevét.

Hello második R-parancsfájl, a Microsoft hello terjesztési pozitív és negatív osztályok között egyenleg (1 és 0 osztály rendre) egyszerűsítés hello negatív osztály. hello R parancsfájl itt bemutatja, hogyan toodo ezt:

![Második R-parancsfájl](./media/machine-learning-data-science-process-hive-criteo-walkthrough/91wvcwN.png)

Ez egyszerű R-parancsfájl használjuk "pos\_Neg.\_arány" hello pozitív és negatív hello osztályok közötti egyensúly tooset hello mennyiségét. Ez azért fontos, mivel osztály egyensúlyhiány javítása általában jobb teljesítményt nyújt a besorolási problémákat hello osztály terjesztési esetén toodo válik egyenetlenné (visszaírási, hogy ebben az esetben tudunk 3.3 % pozitív és negatív osztály 96.7 %).

##### <a name="applying-hello-count-transformation-on-our-data"></a>Az adatok a hello száma átalakítása alkalmazása
Végezetül használhatjuk hello **alkalmazása átalakítása** modul tooapply száma átalakítások a vonaton hello és adatkészletek tesztelése. Ez a modul egy bemeneti és a hello betanítása vagy adatkészletek tesztelése, egyéb beviteli hello vesz igénybe a mentett hello száma átalakító, és count szolgáltatásokkal rendelkező adatokat ad vissza. Az itt látható:

![Átalakítás modul alkalmazása](./media/machine-learning-data-science-process-hive-criteo-walkthrough/xnQvsYf.png)

##### <a name="an-excerpt-of-what-hello-count-features-look-like"></a>Hogyan meg hello száma szolgáltatások kivonat
Milyen hello száma funkciók jellegét esetünkben a fontos információkat tartalmazó toosee. Itt azt az a cikkből megjelenítése:

![Szolgáltatások száma](./media/machine-learning-data-science-process-hive-criteo-walkthrough/FO1nNfw.png)

A cikkből megmutatjuk, hogy hello olyan oszlopok esetében, azt a megszámlált, azt lekérése hello számát és bejelentkezés valószínűleg hozzáadása tooany vonatkozó backoffs.

Dolgozunk most már készen áll a toobuild az Azure Machine Learning modell ezen átalakított adatkészletek. A következő szakaszban hello megmutatjuk, hogyan ehhez.

### <a name="step3"></a>3. lépés: Létrehozása, betanítását és pontozását hello modell.

#### <a name="choice-of-learner"></a>Tanuló kiválasztása
Először igazolnia kell a tanuló toochoose. Folyamatban, mint a tanuló folyamatos toouse egy két osztályú súlyozott döntési fa. Az alábbiakban a tanuló hello alapértelmezett beállítások:

![Két osztályú súlyozott döntési fa paraméterek](./media/machine-learning-data-science-process-hive-criteo-walkthrough/bH3ST2z.png)

A kísérleti fázisú funkciókat hogy folyamatos toochoose hello alapértelmezett értékei lesznek. Megjegyezzük, hogy hello alapértelmezés szerint általában értelmezhető és egy jó módszer tooget gyors alaptervek a teljesítményre. Abszolút paraméterek Ha úgy dönt, hogy tooonce alapterv javíthatja a teljesítményt.

#### <a name="train-hello-model"></a>Hello tanítási
Képzési, hogy egyszerűen meghívni egy **tanítási modell** modul. hello két bemenet tooit a következők: hello két osztályú súlyozott döntési fa tanuló és az vonat adatkészletet. Ez itt látható:

![Train Model-modul](./media/machine-learning-data-science-process-hive-criteo-walkthrough/2bZDZTy.png)

#### <a name="score-hello-model"></a>Pontszám hello modell
Miután a betanított modell tudunk, készen áll-e azt tooscore hello a DataSet adatkészlet és teszteléséhez tooevaluate a teljesítményét. Jelenleg ezt úgy teheti meg hello **Score Model** . ábrát, valamint a következő hello látható modul egy **modell kiértékelése** modul:

![A Score model (Modell montozása) modul](./media/machine-learning-data-science-process-hive-criteo-walkthrough/fydcv6u.png)

### <a name="step4"></a>4. lépés: Hello modell kiértékelése
Végezetül tapasztalatairól tooanalyze modell teljesítményét. A két osztály (bináris) besorolás problémákat általában jó mérték hello AUC jelenti. toovisualize, azt a számítógéphez hello **Score Model** modul tooan **modell kiértékelése** a modul. Kattintson a **Visualize** hello a **modell kiértékelése** modul például a következő egy hello képet eredményez:

![A modul táblázatos adatbázisba kerülő modell kiértékelése](./media/machine-learning-data-science-process-hive-criteo-walkthrough/0Tl0cdg.png)

A bináris (vagy két osztály) besorolás problémák, az előrejelzés pontosságát jó mérték van hello terület a görbe (AUC). A következő megmutatjuk, az eredményeket a tesztelési adatkészletnél ebben a modellben. tooget e, kattintson a jobb gombbal hello kimeneti portjára hello **modell kiértékelése** modult, majd **Visualize**.

![Modell kiértékelése modul megjelenítése](./media/machine-learning-data-science-process-hive-criteo-walkthrough/IRfc7fH.png)

### <a name="step5"></a>5. lépés: Hello modell közzététele egy webszolgáltatás
Adja meg a megfelelő széles körben elérhető értékes hello képességét toopublish webszolgáltatásként fuss legalább egy Azure Machine Learning-modell nem. Ezt követően, bárki, aki hívások toohello webszolgáltatás bemeneti adatokkal, hogy az előrejelzés van szükségük, és hello webszolgáltatás használja hello modell tooreturn e előrejelzéseket teheti meg.

toodo, először elmentjük a betanított modell Trained Model-objektumként. Ehhez kattintson a jobb gombbal a hello **tanítási modell** modul és a hello segítségével **Trained Model elmentse** lehetőséget.

A következő toocreate bemeneti és kimeneti kell a webszolgáltatás által használt portokat:

* egy bemeneti porthoz adatok az azonos alkotnak, igazolnia kell, hogy az előrejelzés hello adatként hello időt vesz igénybe.
* a kimeneti portra hello pontozott címkék és a kapcsolódó hello valószínűség adja vissza.

#### <a name="select-a-few-rows-of-data-for-hello-input-port"></a>Jelölje be néhány sornyi adatot hello bemeneti port
Tetszés szerinti toouse egy **SQL-átalakítás alkalmazása** modul tooselect csak 10 sorok tooserve, hello bemeneti port adatai. Ezeknek az itt látható hello SQL lekérdezést használva bemeneti porthoz adatsorokat kiválasztása:

![Bemeneti porthoz adatok](./media/machine-learning-data-science-process-hive-criteo-walkthrough/XqVtSxu.png)

#### <a name="web-service"></a>Webszolgáltatás
Most a folyamatban van, egy kis kísérletet, amely használt toopublish toorun kész webes szolgáltatás.

#### <a name="generate-input-data-for-webservice"></a>A webszolgáltatás bemeneti adatok létrehozása
Zeroth lépésként hello száma tábla nagy, mivel azt igénybe vehet néhány sornyi Tesztadatok és hozhat létre a kimeneti adatokat, count szolgáltatásokkal. Ez a szolgálhatnak a webservice hello bemeneti adatok formátuma. Ez itt látható:

![A bemeneti adatok táblázatos adatbázisba kerülő létrehozása](./media/machine-learning-data-science-process-hive-criteo-walkthrough/OEJMmst.png)

> [!NOTE]
> A bemeneti adatok formátuma hello, most használjuk hello hello KIMENETE **száma Featurizer** modul. Amennyiben ez kísérletezhet futtató befejeződik, mentése hello kimeneti hello **száma Featurizer** adatkészletként modul. Ez az adatkészlet hello bemeneti adatok hello webservice szolgál.
> 
> 

#### <a name="scoring-experiment-for-publishing-webservice"></a>Kísérlet pontozás közzétételi webszolgáltatás
Első lépésként megmutatjuk néz ez. hello alapvető struktúra egy **Score Model** modul, amely elfogadja a betanított modell objektum és a bemeneti adatok azt hello segítségével hello előző lépésekben létrehozott néhány sornyi **száma Featurizer** modul. "Válassza oszlopok a Dataset" tooproject hello Scored címkék és hello pontszám valószínűség használjuk.

![Oszlopok kiválasztása az adathalmaz](./media/machine-learning-data-science-process-hive-criteo-walkthrough/kRHrIbe.png)

Figyelje meg, hogyan hello **Select Columns in Dataset** modul "kiszűrése" adatok egy adatkészletből is használható. Megmutatjuk, itt hello tartalma:

![A Dataset modul Select Columns hello szűrése](./media/machine-learning-data-science-process-hive-criteo-walkthrough/oVUJC9K.png)

tooget hello kék bemeneti és kimeneti portok, egyszerűen kattint **készítse elő a webservice** jobb hello alján. Ez a kísérlet is lehetővé teszi a us toopublish hello webszolgáltatás: hello kattintson **WEBSZOLGÁLTATÁS közzététele** ikonra hello alsó jobb, látható itt:

![Webszolgáltatás](./media/machine-learning-data-science-process-hive-criteo-walkthrough/WO0nens.png)

Hello webservice van közzétéve, amint azt lekérése átirányított tooa lap, így a következőhöz:

![Webes szolgáltatás irányítópult](./media/machine-learning-data-science-process-hive-criteo-walkthrough/YKzxAA5.png)

Két hivatkozások webservices hello bal oldalán látható:

* Hello **kérelem/válasz** szolgáltatás (vagy RR-EKET) egyetlen előrejelzéseket készült, és mi azt használják a workshop a rendszer.
* Hello **KÖTEGELT végrehajtási** szolgáltatás (BES) szolgál a kötegelt előrejelzéseket, és megköveteli, hogy hello használt bemeneti adatok toomake előrejelzéseket találhatók az Azure Blob Storage tárolóban.

Hello hivatkozásra kattintva **kérelem/válasz** tooa vesz igénybe, olyan előre konzerv C#, python, és R. az Ezt a kódot, hogy a hívások toohello webservice kényelmesen használható. Vegye figyelembe, hogy hello API-kulcs ezen az oldalon kell toobe hitelesítéshez használt.

A python-kód tooa új cellára hello IPython jegyzetfüzet kényelmes toocopy.

Itt megmutatjuk, python kódját egy szegmens hello megfelelő API-kulccsal.

![Python-kód](./media/machine-learning-data-science-process-hive-criteo-walkthrough/f8N4L4g.png)

Vegye figyelembe, hogy a Microsoft hello alapértelmezett API-kulcs helyére a problémák megoldásához segítséget az API-kulcs. Kattintson a **futtatása** ezen a cellán egy IPython a notebook adja eredményül a következő válasz hello:

![IPython válasz](./media/machine-learning-data-science-process-hive-criteo-walkthrough/KSxmia2.png)

Látható, hogy a hello két vizsgálati példák azt ismételt (a python-parancsfájl hello hello JSON keretében), azt vissza választ kaphat hello formában "Scored valószínűség Scored címke". Vegye figyelembe, hogy ebben az esetben választottuk hello alapértelmezett értékek hello előre előre összeállított kódot tartalmaz (0 összes numerikus oszlopok és hello karakterlánc "érték" minden kategorikus oszlopokhoz).

Ezzel befejezte a végpont forgatókönyv ábrázoló hogyan toohandle nagyméretű dataset Azure Machine Learning segítségével. Azt a terabájt az adatok használatába, összeállított egy előrejelzési modellt, és telepítve lett egy webszolgáltatás hello felhőben.

