---
title: "aaaCreate Hive táblák és az adatok betöltése az Azure Blob Storage |} Microsoft Docs"
description: "Hive táblák létrehozására és betöltésére blob toohive táblázatok adatait"
services: machine-learning,storage
documentationcenter: 
author: bradsev
manager: jhubbard
editor: cgronlun
ms.assetid: cff9280d-18ce-4b66-a54f-19f358d1ad90
ms.service: machine-learning
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/29/2017
ms.author: bradsev
ms.openlocfilehash: 09622972bcac31c2971858393a8340f24e4b7390
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="create-hive-tables-and-load-data-from-azure-blob-storage"></a>Hive táblák létrehozása és az adatok betöltése az Azure Blob Storage
Ebből a témakörből megismerheti, hogy a Hive táblák létrehozása és az adatok betöltése az Azure blob storage általános Hive-lekérdezéseket. Néhány is útmutatást a Hive Táblák particionálása és hello optimalizált sor oszlopos (ORC) formázási tooimprove teljesítmény-küszöbérték használatával.

Ez **menü** hivatkozásokat tartalmaz, amelyek ismertetik, hogyan tooingest adatok cél környezetekben, ahol hello adatok is tárolhatók és feldolgozhatók, során hello Team adatok tudományos folyamat (TDSP) tootopics.

[!INCLUDE [cap-ingest-data-selector](../../includes/cap-ingest-data-selector.md)]

## <a name="prerequisites"></a>Előfeltételek
Ez a cikk feltételezi, hogy rendelkezik:

* Egy Azure storage-fiók létrehozása. Ha módosítania kell az utasításokat, lásd: [tudnivalók az Azure storage-fiókok](../storage/common/storage-create-storage-account.md).
* A testre szabott Hadoop-fürt a HDInsight-szolgáltatás hello kiépítve.  Ha módosítania kell az utasításokat, lásd: [testreszabása az Azure HDInsight Hadoop-fürtök Speciális elemzésekre](machine-learning-data-science-customize-hadoop-cluster.md).
* Engedélyezett távoli hozzáférési toohello fürt, jelentkezett be, és hello Hadoop parancssori konzol megnyitása. Ha módosítania kell az utasításokat, lásd: [hozzáférés hello Head csomópont a Hadoop-fürt](machine-learning-data-science-customize-hadoop-cluster.md#headnode).

## <a name="upload-data-tooazure-blob-storage"></a>Töltse fel az adatok tooAzure blob-tároló
Ha létrehozott egy Azure virtuális gép hello megjelenő utasításokat követve [állítson be egy Azure virtuális gép speciális elemzésekre](machine-learning-data-science-setup-virtual-machine.md), a parancsfájl letöltött toohello kellett volna *C:\\ Felhasználók\\\<felhasználónév\>\\dokumentumok\\adatok tudományos parancsfájlok* hello virtuális gép könyvtárába. A következő Hive-lekérdezések csak szükséges, csatlakoztassa a saját adatok séma és az Azure blob storage konfigurációs hello megfelelő mezőket toobe készen áll, hogy elküldhesse a.

Feltételezzük, hogy a Hive táblák hello adatok van egy **tömörítetlen** táblázatos formátumban, és hogy hello már feltöltött toohello alapértelmezett (vagy további tooan) tároló hello Hadoop-fürt által használt hello tárfiók.

Ha azt szeretné, hogy a hello toopractice **NYC Taxi út adatok**, kell:

* **Töltse le** hello 24 [NYC Taxi út adatok](http://www.andresmh.com/nyctaxitrips) (12 út fájlok és 12 jegy ára fájlok),
* **Csomagolja ki** .csv fájlokat, az összes fájl, majd
* **Töltse fel** őket toohello alapértelmezett (vagy a megfelelő tárolót) az Azure storage-fiók hello hello ismertetett eljárással létrehozott hello [testreszabása az Azure HDInsight Hadoop-fürtök Advanced Analytics folyamat és a technológia ](machine-learning-data-science-customize-hadoop-cluster.md) témakör. Itt található hello folyamat tooupload hello .csv fájlok toohello alapértelmezett tároló hello tárfiók [lap](machine-learning-data-science-process-hive-walkthrough.md#upload).

## <a name="submit"></a>Hogyan toosubmit Hive-lekérdezések
Hive-lekérdezések segítségével küldheti el:

1. [A Hadoop-fürt headnode keresztül Hadoop parancssori Hive-lekérdezések elküldése](#headnode)
2. [A Hive szerkesztő hello Hive-lekérdezések elküldése](#hive-editor)
3. [Küldje el az Azure PowerShell-parancsokkal Hive-lekérdezések](#ps)

Hive-lekérdezések SQL-szerű. Ha ismeri az SQL, azt tapasztalhatja hello [SQL felhasználók Cheat lap Hive](http://hortonworks.com/wp-content/uploads/2013/05/hql_cheat_sheet.pdf) hasznos.

A Hive-lekérdezés elküldésekor azt is meghatározhatja, Hive-lekérdezések eredményének hello hello céljának hello képernyő vagy tooa helyi fájl hello átjárócsomópont vagy az Azure blob tooan legyen.

### <a name="headnode"></a> 1. A Hadoop-fürt headnode keresztül Hadoop parancssori Hive-lekérdezések elküldése
Hello Hive lekérdezés esetén összetett, közvetlenül a hello hello Hadoop-fürt átjárócsomópontjához általában vezet toofaster kapcsolja körül, mint a Hive szerkesztő vagy az Azure PowerShell-parancsfájlok továbbítás elküldése.

Jelentkezzen be toohello hello Hadoop-fürt átjárócsomópontjához, hello Hadoop parancssori hello központi csomópont hello asztalon nyissa meg, és írja be a parancs `cd %hive_home%\bin`.

Három módon toosubmit Hive-lekérdezések hello Hadoop parancssori rendelkezik:

* közvetlenül
* .hql fájlok használata
* a hello Hive parancskonzolról

#### <a name="submit-hive-queries-directly-in-hadoop-command-line"></a>Küldje el közvetlenül a Hadoop parancssori Hive-lekérdezéseket.
Például a parancs futtatása `hive -e "<your hive query>;` toosubmit egyszerű Hive-lekérdezéseket közvetlenül a Hadoop parancssor. Íme egy példa, ahol piros hello mezőben vázol fel hello parancs, amellyel hello Hive-lekérdezést, és hello zöld mező vázol fel hello kimenete hello Hive-lekérdezést.

![Munkaterület létrehozása](./media/machine-learning-data-science-move-hive-tables/run-hive-queries-1.png)

#### <a name="submit-hive-queries-in-hql-files"></a>Küldje el a Hive-lekérdezéseket a .hql fájlok
Hello Hive-lekérdezések bonyolultabb, és több vonal van, a parancssor vagy a Hive parancskonzolról lekérdezések szerkesztése esetén nem gyakorlati. Alternatív hello átjárócsomópontjához hello Hadoop fürthöz toosave hello Hive-lekérdezések egy helyi könyvtárat a hello átjárócsomópont .hql fájlban toouse egy szövegszerkesztőben. Ezután a Hive-lekérdezések hello hello .hql fájlban hello segítségével küldheti el `-f` argumentum az alábbiak szerint:

    hive -f "<path toohello .hql file>"

![Munkaterület létrehozása](./media/machine-learning-data-science-move-hive-tables/run-hive-queries-3.png)

**Ne jelenjen meg többé folyamat állapota képernyőn nyomtatás Hive-lekérdezések**

Alapértelmezés szerint a Hadoop parancssorban Hive-lekérdezés elküldése után hello hello térkép vagy csökkentse a feladat előrehaladását nyomtatása a képernyőn. toosuppress hello hello térkép vagy csökkentse a feladat előrehaladását a képernyő Nyomtatás, argumentumot használja `-S` ("S" nagybetűvel) a hello a parancssor az alábbiak szerint:

    hive -S -f "<path toohello .hql file>"
.    Hive -S -e "<Hive queries>"

#### <a name="submit-hive-queries-in-hive-command-console"></a>Küldje el a Hive parancskonzolról Hive-lekérdezéseket.
Először is megadhat az hello Hive parancskonzolról parancs futtatásával `hive` a Hadoop parancssor, és küldje el a Hive parancskonzolról Hive-lekérdezéseket. Íme egy példa. Ebben a példában hello két piros mezőkbe kiemelési hello használt parancsok tooenter hello Hive parancskonzolról, és Hive-lekérdezés Hive parancskonzolról, illetve benyújtott hello. zöld hello mezőben hello Hive-lekérdezések hello kimenetét mutatja be.

![Munkaterület létrehozása](./media/machine-learning-data-science-move-hive-tables/run-hive-queries-2.png)

hello előző példák közvetlenül kimeneti hello Hive lekérdezés eredményeit a képernyőn. Hello kimeneti tooa helyi fájlt hello átjárócsomópont, vagy az Azure blob tooan is írhat. Ezután használhatja más eszközök toofurther hello Hive-lekérdezések kimenetének elemzése.

**Kimeneti Hive lekérdezés eredményei tooa helyi fájl.**
toooutput Hive lekérdezés eredményei tooa helyi könyvtárába hello átjárócsomópont, rendelkezik toosubmit hello Hive-lekérdezések hello Hadoop parancssor az alábbiak szerint:

    hive -e "<hive query>" > <local path in hello head node>

A következő példa hello, Hive-lekérdezések eredményének hello egy fájlba írása `hivequeryoutput.txt` könyvtárban `C:\apps\temp`.

![Munkaterület létrehozása](./media/machine-learning-data-science-move-hive-tables/output-hive-results-1.png)

**Kimeneti Hive lekérdezés eredményei tooan Azure blob**

Emellett a kimenetre küldheti hello Hive lekérdezés eredményei tooan Azure blob, hello alapértelmezett tároló hello Hadoop-fürt belül. hello Hive-lekérdezést a következőképpen történik:

    insert overwrite directory wasb:///<directory within hello default container> <select clause from ...>

A következő példa hello, a Hive-lekérdezések eredményének hello íródott tooa blob directory `queryoutputdir` hello alapértelmezett tároló hello Hadoop-fürt belül. Itt csak akkor kell tooprovide hello könyvtár nevét, hello blob név nélkül. Hiba történt kívánja megadni a címtár és a blob neve, mint például `wasb:///queryoutputdir/queryoutput.txt`.

![Munkaterület létrehozása](./media/machine-learning-data-science-move-hive-tables/output-hive-results-2.png)

Hello alapértelmezett tároló hello Hadoop-fürt használata Azure Tártallózó nyit meg, ha hello Hive-lekérdezések eredményének hello láthatja, ahogy az ábra a következő hello. Hello (vörös téglalappal által kiemelt) szűrő tooonly lekérése hello blob nevek megadott betűjét is alkalmazhatja.

![Munkaterület létrehozása](./media/machine-learning-data-science-move-hive-tables/output-hive-results-3.png)

### <a name="hive-editor"></a> 2. A Hive szerkesztő hello Hive-lekérdezések elküldése
Hello lekérdezés konzol (Hive szerkesztő) használhatja a hello URL-cím megadásával *https://&#60; Hadoop-fürt neve >.azurehdinsight.net/Home/HiveEditor* egy webböngészőbe. Kell hello itt talál: naplózza ezt a konzolt, és így a Hadoop fürthöz hitelesítő adatait itt kell.

### <a name="ps"></a> 3. Küldje el az Azure PowerShell-parancsokkal Hive-lekérdezések
PowerShell toosubmit Hive-lekérdezéseket is használhatja. Útmutatásért lásd: [elküldeni a Hive feladatok PowerShell-lel](../hdinsight/hdinsight-hadoop-use-hive-powershell.md).

## <a name="create-tables"></a>Hive-adatbázis és tábla létrehozása
hello Hive-lekérdezéseket is meg van osztva hello [GitHub-tárházban](https://github.com/Azure/Azure-MachineLearning-DataScience/tree/master/Misc/DataScienceProcess/DataScienceScripts/sample_hive_create_db_tbls_load_data_generic.hql) , és innen tölthető le.

Itt található hello Hive-lekérdezést, amely egy Hive táblát hoz létre.

    create database if not exists <database name>;
    CREATE EXTERNAL TABLE if not exists <database name>.<table name>
    (
        field1 string,
        field2 int,
        field3 float,
        field4 double,
        ...,
        fieldN string
    )
    ROW FORMAT DELIMITED FIELDS TERMINATED BY '<field separator>' lines terminated by '<line separator>'
    STORED AS TEXTFILE LOCATION '<storage location>' TBLPROPERTIES("skip.header.line.count"="1");

Az alábbiakban hello mezőket, amelyekre szüksége van a tooplug és egyéb beállításokra hello leírása:

* **&#60; adatbázis neve >**: hello adatbázis, amelyet az toocreate hello nevét. Ha csak toouse hello alapértelmezett adatbázis, a lekérdezés hello *adatbázis létrehozása...*  kihagyható.
* **&#60; tábla neve >**: hello tábla, amelyet az toocreate hello megadott adatbázison belül hello neve. Ha azt szeretné, hogy toouse hello alapértelmezett adatbázis, hello tábla is közvetlenül elé *&#60; tábla neve >* nélkül &#60; adatbázis neve >.
* **&#60; mező elválasztó >**: hello elválasztó hello adatok fájl toobe mezőinek határoló feltöltött toohello Hive tábla.
* **&#60; Sorelválasztó >**: hello adatfájl sorainak határoló hello elválasztó.
* **&#60; tárolási helye >**: az Azure storage toosave hello helyadatok Hive táblák hello. Ha nincs megadva *hely &#60; a tárolási helye >*, adatbázis hello és hello táblák tárolódnak *hive/adatraktár/* könyvtárban lévő hello alapértelmezett tároló hello Hive fürt alapértelmezés szerint. Ha azt szeretné, hogy toospecify hello tárolási helye, hello tárolási helye van toobe hello alapértelmezett tárolóban hello adatbázis és a táblára. Ezen a helyen van hely relatív toohello alapértelmezett tároló hello fürt hello formátumban néven toobe *"wasb: / / / &#60; 1 könyvtár > /"* vagy *"wasb: / / / &#60; 1 könyvtár > / &#60; Directory 2 > / "*stb. Miután hello lekérdezés végrehajtása, hello relatív könyvtárak hello alapértelmezett tárolóban jönnek létre.
* **TBLPROPERTIES("Skip.Header.line.Count"="1")**: Ha hello adatfájl fejléc rendelkezik, akkor tooadd ezt a tulajdonságot **hello végén** a hello *tábla létrehozása* lekérdezés. Ellenkező esetben hello fejlécsort rekord toohello táblázatként be van töltve. Hello adatok fájlban nincs fejléc, ha ez a konfiguráció elhagyható hello lekérdezésben.

## <a name="load-data"></a>Adattáblák tooHive betöltése
Itt található, amely adatokat tölt be egy Hive tábla hello Hive-lekérdezést.

    LOAD DATA INPATH '<path tooblob data>' INTO TABLE <database name>.<table name>;

* **&#60; elérési út tooblob adatok >**: hello alapértelmezett tárolót a HDInsight Hadoop-fürt hello hello blob fájl feltöltése toobe toohello Hive tábla esetén hello *&#60; elérési út tooblob adatok >* hello formátumúnak kell lennie *"wasb: / / / &#60; ebben a tárolóban directory > / &#60; blob fájl neve >'*. hello fájlját egy további hello HDInsight Hadoop-fürt-tárolót is lehet. Ebben az esetben *&#60; elérési út tooblob adatok >* hello formátumúnak kell lennie *"wasb: / / &#60; a tároló neve > @&#60; tárfiók neve >.blob.core.windows.net/ &#60; a blob-fájl neve >'*.

  > [!NOTE]
  > hello blob adatok feltöltése toobe tooHive táblához toobe hello alapértelmezett vagy hello storage-fiókjának hello Hadoop-fürt további tárolóban. Ellenkező esetben hello *adatok betöltése* lekérdezés nem sikerült panaszos, hogy hello adatok nem férhet hozzá.
  >
  >

## <a name="partition-orc"></a>Speciális témakörök: particionált tábla és a tároló Hive adatok ORC formátumban
Hello adatok mérete nagy, ha a lekérdezések csak tooscan hello tábla partíciója néhány hasznos hello tábla particionáló. Például akkor ésszerű toopartition hello naplóadatokat egy webhely dátuma alapján.

Továbbá toopartitioning a Hive táblákat, a is előnyös toostore hello Hive adatok hello optimalizált sor oszlopos (ORC) formátumban. A formázás ORC további információkért lásd: <a href="https://cwiki.apache.org/confluence/display/Hive/LanguageManual+ORC#LanguageManualORC-ORCFiles" target="_blank">használatával ORC fájlokat javítja a teljesítményt, ha Hive olvasása, írása, és adatfeldolgozás</a>.

### <a name="partitioned-table"></a>Particionált tábla
Itt található hello Hive-lekérdezést, amely létrehoz egy particionált táblához, és adatokat tölt be.

    CREATE EXTERNAL TABLE IF NOT EXISTS <database name>.<table name>
    (field1 string,
    ...
    fieldN string
    )
    PARTITIONED BY (<partitionfieldname> vartype) ROW FORMAT DELIMITED FIELDS TERMINATED BY '<field separator>'
         lines terminated by '<line separator>' TBLPROPERTIES("skip.header.line.count"="1");
    LOAD DATA INPATH '<path toohello source file>' INTO TABLE <database name>.<partitioned table name>
        PARTITION (<partitionfieldname>=<partitionfieldvalue>);

Particionált táblák lekérdezésekor ajánlott tooadd hello partíció feltételének hello **kezdete** a hello `where` záradék, ez növeli a hello jelentősen keresés hatékonyságát.

    select
        field1, field2, ..., fieldN
    from <database name>.<partitioned table name>
    where <partitionfieldname>=<partitionfieldvalue> and ...;

### <a name="orc"></a>Hive adattárolásra ORC formátumban
Ön közvetlenül adatai nem tölthetők be blobtárolóból hello ORC formátumban tárolt Hive táblákba. Az alábbiakban hello lépéseket, hogy szüksége van az Azure-ból tootake tooload adatok hello blobok tooHive táblák ORC formátumban tárolja.

Létrehoz egy külső táblát **tárolt AS TEXTFILE** és az adatok betöltése a blob storage toohello táblából.

        CREATE EXTERNAL TABLE IF NOT EXISTS <database name>.<external textfile table name>
        (
            field1 string,
            field2 int,
            ...
            fieldN date
        )
        ROW FORMAT DELIMITED FIELDS TERMINATED BY '<field separator>'
            lines terminated by '<line separator>' STORED AS TEXTFILE
            LOCATION 'wasb:///<directory in Azure blob>' TBLPROPERTIES("skip.header.line.count"="1");

        LOAD DATA INPATH '<path toohello source file>' INTO TABLE <database name>.<table name>;

Egy belső tábla létrehozása a hello hello külső tábla 1. lépésben hello ugyanaz a mezőhatároló, és tárolja az azonos sémából hello Hive adatok hello ORC formátumban.

        CREATE TABLE IF NOT EXISTS <database name>.<ORC table name>
        (
            field1 string,
            field2 int,
            ...
            fieldN date
        )
        ROW FORMAT DELIMITED FIELDS TERMINATED BY '<field separator>' STORED AS ORC;

Válassza ki az adatok az 1. lépésben hello külső táblázatból, majd szúrja be a hello ORC táblába

        INSERT OVERWRITE TABLE <database name>.<ORC table name>
            SELECT * FROM <database name>.<external textfile table name>;

> [!NOTE]
> Ha hello TEXTFILE tábla *&#60; adatbázis neve >. &#60; külső textfile táblanév >* partíciókkal rendelkezik, a 3. LÉPÉSBEN, hello `SELECT * FROM <database name>.<external textfile table name>` parancsot választja ki hello partíció változó hello mező adatkészlet visszaadott formában. Hello való beillesztése történik *&#60; adatbázis neve >. &#60; ORC táblanév >* óta sikertelen *&#60; adatbázis neve >. &#60; ORC táblanév >* nincs hello partíció változó, egy hello táblaséma mező. Ebben az esetben szüksége toospecifically válassza hello mezők toobe túl beszúrni*&#60; adatbázis neve >. &#60; ORC táblanév >* az alábbiak szerint:
>
>

        INSERT OVERWRITE TABLE <database name>.<ORC table name> PARTITION (<partition variable>=<partition value>)
           SELECT field1, field2, ..., fieldN
           FROM <database name>.<external textfile table name>
           WHERE <partition variable>=<partition value>;

Biztonságos toodrop hello *&#60; külső textfile táblanév >* amikor lekérdezés után az összes adat a következő hello segítségével e behelyezve *&#60; adatbázis neve >. &#60; ORC táblanév >*:

        DROP TABLE IF EXISTS <database name>.<external textfile table name>;

A következő eljárással, miután hello ORC formátum készen toouse-adatokat tartalmazó táblát kell rendelkeznie.  
