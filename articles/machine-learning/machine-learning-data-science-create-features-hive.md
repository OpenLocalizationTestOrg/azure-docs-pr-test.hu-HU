---
title: "a Hive-lekérdezésekkel Hadoop-fürtben lévő adatok aaaCreate szolgáltatások |} Microsoft Docs"
description: "Példák a szolgáltatások készítése az Azure HDInsight Hadoop-fürt tárolt adatokat a Hive-lekérdezéseket."
services: machine-learning
documentationcenter: 
author: bradsev
manager: jhubbard
editor: cgronlun
ms.assetid: e8a94c71-979b-4707-b8fd-85b47d309a30
ms.service: machine-learning
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/24/2017
ms.author: hangzh;bradsev
ms.openlocfilehash: 686282bf0fb84ea82758d3c5b7de2bd90f0cf159
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="create-features-for-data-in-an-hadoop-cluster-using-hive-queries"></a>Funkciók létrehozása az adatokhoz egy Hadoop-fürtben Hive-lekérdezések segítségével
Ez a dokumentum látható, hogyan toocreate szolgáltatások adatok az Azure HDInsight Hadoop-fürt Hive-lekérdezésekkel tárol. A Hive-lekérdezéseket beágyazott Hive felhasználó által megadott funkciókat (UDF) használatához hello parancsfájlok, amelynek vannak megadva.

hello szükséges műveletek toocreate szolgáltatások memóriaigényes lehet. Hive-lekérdezések teljesítményét hello kritikus fontosságú ebben az esetben lesz, és bizonyos paraméterek hangolása javítja. hello végső szakaszban tárgyalt hello beállítása a következő paraméterek közül.

Hello lekérdezések, amelyek bemutatják többek között az adott toohello [NYC Taxi út adatok](http://chriswhong.com/open-data/foil_nyc_taxi/) forgatókönyvek is szerepelnek [GitHub-tárházban](https://github.com/Azure/Azure-MachineLearning-DataScience/tree/master/Misc/DataScienceProcess/DataScienceScripts). Ezeket a lekérdezéseket már adatok séma van megadva, és készen áll a toobe benyújtott toorun. Hello a végső szakaszban paramétereket, így növelhető a Hive-lekérdezések teljesítményét hello észlelheti a felhasználók is ismerteti.

[!INCLUDE [cap-create-features-data-selector](../../includes/cap-create-features-selector.md)]

Ez **menü** hivatkozásokat tartalmaz, amelyek ismertetik, hogyan toocreate-adatok különböző környezetekben funkcióit tootopics. Ez a feladat ez hello lépés [Team adatok tudományos folyamat (TDSP)](https://azure.microsoft.com/documentation/learning-paths/cortana-analytics-process/).

## <a name="prerequisites"></a>Előfeltételek
Ez a cikk feltételezi, hogy rendelkezik:

* Egy Azure storage-fiók létrehozása. Ha módosítania kell az utasításokat, lásd: [egy Azure Storage-fiók létrehozása](../storage/common/storage-create-storage-account.md#create-a-storage-account)
* A testre szabott Hadoop-fürt a HDInsight-szolgáltatás hello kiépítve.  Ha módosítania kell az utasításokat, lásd: [testreszabása Azure HDInsight Hadoop-fürtök az Advanced Analytics](machine-learning-data-science-customize-hadoop-cluster.md).
* hello már feltöltött tooHive táblák az Azure HDInsight Hadoop-fürtök. Ha még nem, kövesse [létrehozása és a betöltés tooHive adattáblák](machine-learning-data-science-move-hive-tables.md) tooupload adatok tooHive először táblázatot.
* Engedélyezett távoli hozzáférési toohello fürt. Ha módosítania kell az utasításokat, lásd: [hozzáférés hello Head csomópont a Hadoop-fürt](machine-learning-data-science-customize-hadoop-cluster.md#headnode).

## <a name="hive-featureengineering"></a>Szolgáltatás létrehozása
Ez a szakasz számos példát, amelyben funkciókat is lehet generálása Hive-lekérdezésekkel hello módszereket ismerteti. Miután létrehozta a további szolgáltatásokat, oszlopok toohello meglévő táblaként adja hozzá, vagy hozzon létre egy új tábla hello további funkciók és elsődleges kulcs, amely majd lehetne illeszteni hello eredeti táblázatban. Az alábbiakban bemutatott hello példák:

1. [Gyakoriság alapján a szolgáltatás létrehozása](#hive-frequencyfeature)
2. [A bináris osztályozási Kategorikus változók kockázata](#hive-riskfeature)
3. [Bontsa ki a szolgáltatásokat a DateTime típusú mező](#hive-datefeatures)
4. [Szolgáltatások kinyerése szövegmező](#hive-textfeatures)
5. [Kiszámíthatja GPS-koordináták közötti távolság](#hive-gpsdistance)

### <a name="hive-frequencyfeature"></a>Gyakoriság alapján a szolgáltatás létrehozása
Ez a legtöbbször hasznos toocalculate hello gyakoriságát hello szintek kategorikus változó, vagy több kategorikus változók szintjét egyes kombinációi hello gyakoriságát. Felhasználók az e használja a következő parancsfájl toocalculate hello:

        select
            a.<column_name1>, a.<column_name2>, a.sub_count/sum(a.sub_count) over () as frequency
        from
        (
            select
                <column_name1>,<column_name2>, count(*) as sub_count
            from <databasename>.<tablename> group by <column_name1>, <column_name2>
        )a
        order by frequency desc;


### <a name="hive-riskfeature"></a>A bináris osztályozási Kategorikus változók kockázata
A bináris osztályozási igazolnia kell tooconvert nem numerikus kategorikus változók numerikus funkciók be hello modellek használt csak numerikus szolgáltatások kerül. Ez történik, azáltal, hogy minden nem numerikus szinthez a numerikus kockázata. Ebben a szakaszban megmutatjuk, néhány általános hello kockázati értékek (napló valószínűleg) kategorikus változó kiszámításához Hive-lekérdezéseket.

        set smooth_param1=1;
        set smooth_param2=20;
        select
            <column_name1>,<column_name2>,
            ln((sum_target+${hiveconf:smooth_param1})/(record_count-sum_target+${hiveconf:smooth_param2}-${hiveconf:smooth_param1})) as risk
        from
            (
            select
                <column_nam1>, <column_name2>, sum(binary_target) as sum_target, sum(1) as record_count
            from
                (
                select
                    <column_name1>, <column_name2>, if(target_column>0,1,0) as binary_target
                from <databasename>.<tablename>
                )a
            group by <column_name1>, <column_name2>
            )b

Ebben a példában, változók `smooth_param1` és `smooth_param2` set toosmooth hello kockázati értékek kiszámítása hello adatokból. Kockázatok hogy a tartomány -Inf és Inf között. A kockázatok > 0 azt jelzi, hogy hello valószínűsége, hogy a célkiszolgáló hello egyenlő too1 0,5-nél nagyobb.

Hello kockázat tábla kiszámítása, miután felhasználók kockázati értékek tooa tábla hozzárendeléséhez csatlakoztatná hello kockázat táblával. az előző szakaszban hello Hive csatlakozó lekérdezés lett megadva.

### <a name="hive-datefeatures"></a>Bontsa ki a szolgáltatásokat a Datetime mezők
Hive tartalmaz egy felhasználó által megadott függvények a datetime mezők feldolgozásához. A Hive, hello alapértelmezett dátum és idő formátuma: "éééé-hh-nn 00:00:00" ("1970-01-01 12:21:32" például). Ebben a szakaszban megmutatjuk, bontsa ki a hónap, a DateTime típusú mező hello hónap napja hello példákat és egyéb példák, amelyek a dátum/idő karakterlánc formátuma eltérő hello alapértelmezett formátum tooa dátum/idő karakterláncot az alapértelmezett formázni.

        select day(<datetime field>), month(<datetime field>)
        from <databasename>.<tablename>;

A Hive-lekérdezést feltételezi, hogy hello *&#60; DateTime típusú mező >* hello alapértelmezett dátum és idő formátumban.

Ha egy DateTime típusú mező nincs a hello alapértelmezett formátuma, először a Unix időbélyeg szüksége tooconvert hello DateTime típusú mező, majd a konvertálás hello Unix idő stamp tooa dátum/idő karakterlánc, amely hello alapértelmezett formátumban. Hello dátum és idő formátuma alapértelmezett, amikor a felhasználók alkalmazhatja a beágyazott hello dátum és idő felhasználó által megadott függvények tooextract funkciót.

        select from_unixtime(unix_timestamp(<datetime field>,'<pattern of hello datetime field>'))
        from <databasename>.<tablename>;

Ebben a lekérdezésben, ha hello *&#60; DateTime típusú mező >* hello minta like rendelkezik *2015-03/26 12:04:39*, hello *"&#60; hello DateTime típusú mező mintát >"* kell lennie `'MM/dd/yyyy HH:mm:ss'`. tootest, felhasználók futtatható

        select from_unixtime(unix_timestamp('05/15/2015 09:32:10','MM/dd/yyyy HH:mm:ss'))
        from hivesampletable limit 1;

Hello *hivesampletable* ebben a lekérdezésben előre telepítve van az összes Azure HDInsight Hadoop-fürtök alapértelmezés szerint hello fürtök kiosztásakor.

### <a name="hive-textfeatures"></a>Szolgáltatások kinyerése szövegmezők tartalma
Amikor hello Hive tábla egy szövegmezőben, hogy szóközök határolja karakterláncot tartalmazó, hello következő lekérdezés kibontása hello hello karakterlánc, és hello szavak száma a hello karakterlánc hossza.

        select length(<text field>) as str_len, size(split(<text field>,' ')) as word_num
        from <databasename>.<tablename>;

### <a name="hive-gpsdistance"></a>Kiszámíthatja GPS koordinátákat közötti távolság
Ebben a szakaszban megadott hello lekérdezés közvetlenül alkalmazott toohello NYC Taxi út adatok lehet. hello Ez a lekérdezés célja tooshow hogyan egy beágyazott tooapply matematikai funkciók Hive toogenerate funkciói.

hello ebben a lekérdezésben használt mezők szerepelnek a felvételi és dropoff helyeket nevű hello GPS-koordinátáit *a felvételi\_hosszúság*, *a felvételi\_szélesség*,  *dropoff\_hosszúság*, és *dropoff\_szélesség*. hello közvetlen közötti távolság szerint hello felvétel és dropoff koordináták számító hello lekérdezéseket a következők:

        set R=3959;
        set pi=radians(180);
        select pickup_longitude, pickup_latitude, dropoff_longitude, dropoff_latitude,
            ${hiveconf:R}*2*2*atan((1-sqrt(1-pow(sin((dropoff_latitude-pickup_latitude)
            *${hiveconf:pi}/180/2),2)-cos(pickup_latitude*${hiveconf:pi}/180)
            *cos(dropoff_latitude*${hiveconf:pi}/180)*pow(sin((dropoff_longitude-pickup_longitude)*${hiveconf:pi}/180/2),2)))
            /sqrt(pow(sin((dropoff_latitude-pickup_latitude)*${hiveconf:pi}/180/2),2)
            +cos(pickup_latitude*${hiveconf:pi}/180)*cos(dropoff_latitude*${hiveconf:pi}/180)*
            pow(sin((dropoff_longitude-pickup_longitude)*${hiveconf:pi}/180/2),2))) as direct_distance
        from nyctaxi.trip
        where pickup_longitude between -90 and 0
        and pickup_latitude between 30 and 90
        and dropoff_longitude between -90 and 0
        and dropoff_latitude between 30 and 90
        limit 10;

hello Matematikai egyenleteket két GPS-koordináták hello távolságát számító hello található <a href="http://www.movable-type.co.uk/scripts/latlong.html" target="_blank">Movable Type parancsfájlok</a> hely Peter Lapisu által létrehozott. A Javascript, a függvény hello `toRad()` csak van *lat_or_lon*pi/180 *, fok tooradians alakítja át, amely. Itt *lat_or_lon* hello szélességi és hosszúsági van. Mivel struktúra nem ad hello függvény `atan2`, de hello függvény `atan`, hello `atan2` függvény megvalósítja `atan` függvény a fent megadott hello definition Hive-lekérdezés hello <a href="http://en.wikipedia.org/wiki/Atan2" target="_blank"> Wikipedia</a>.

![Munkaterület létrehozása](./media/machine-learning-data-science-create-features-hive/atan2new.png)

A Hive hello található beágyazott felhasználó által megadott függvények teljes listáját **beépített funkciók** szakaszt, hello <a href="https://cwiki.apache.org/confluence/display/Hive/LanguageManual+UDF#LanguageManualUDF-MathematicalFunctions" target="_blank">Apache Hive wiki</a>).  

## <a name="tuning"></a>Speciális témakörök: hangolási Hive paraméterek tooImprove lekérdezés sebesség
hello Hive fürt beállítások nem feltétlenül alkalmas hello Hive-lekérdezéseket és hello lekérdezések feldolgozás alatt hello adatok alapértelmezett paraméter. Ebben a szakaszban arról lesz szó néhány paramétereket, amelyek a felhasználók észlelheti a Hive-lekérdezések hello teljesítményének javítása. A felhasználóknak kell tooadd hello paraméter hangolása lekérdezések hello lekérdezések adatok feldolgozása előtt.

1. **Java halommemória terület**: lekérdezések nagy adatkészletek csatlakozni, vagy hosszú rekordjának feldolgozásáért **elegendő szabad terület halommemória** hello közös hiba egyike. Ez a paraméter beállításával szabályozható *mapreduce.map.java.opts* és *mapreduce.task.io.sort.mb* toodesired értékeket. Például:
   
        set mapreduce.map.java.opts=-Xmx4096m;
        set mapreduce.task.io.sort.mb=-Xmx1024m;

    Ez a paraméter lefoglalt halommemória tooJava 4GB memória, és is teszi rendezés hatékonyabb több memóriát oszt ki azt. Azt nem egy jó ötlet tooplay a megoldást, ha a feladat sikertelen hibák kapcsolódó tooheap terület.

1. **Az elosztott Fájlrendszerbeli blokkméret** : Ez a paraméter állandóként állítja be a hello legkisebb egység fájl rendszer tárolók hello adatok. Tegyük fel hello elosztott Fájlrendszerbeli blokk mérete 128MB majd mérete adatot kevesebb mint, illetve a hierarchiában felfelé too128MB tárolódnak a egyetlen blokkot tartalmaz, amely nagyobb, mint 128MB számára engedélyezett a felesleges blokkok adatainak közben. Nagy terhek nagyon kis blokkméret kiválasztása hatására a Hadoop, mivel hello neve csomópont csak a tooprocess számos további kérelmeket toofind hello megfelelő blokk vonatkozó toohello fájl. A javasolt beállítás foglalkozó gigabájt (vagy nagyobb) adat:
   
        set dfs.block.size=128m;
2. **Hive join művelet optimalizálása** : közben hello térkép/csökkentése keretrendszer összekapcsolási műveletek általában kerül sor a hello fázisban, néha csökkentéséhez hatalmas növekedését elérhető illesztések ütemezésével hello térkép fázisban (más néven "mapjoins"). toodirect struktúra toodo ez amikor csak lehetséges, azt állíthatja be:
   
        set hive.auto.convert.join=true;
3. **Mappers tooHive hello számát megadó** : közben Hadoop lehetővé teszi, hogy a hello felhasználói tooset hello száma szűkítő, hello száma mappers általában hello felhasználó nem állítható be. Amely lehetővé teszi, hogy ez a szám a vezérlő bizonyos fokú körben toochoose hello Hadoop változók *mapred.min.split.size* és *mapred.max.split.size* minden térkép hello méreteként feladat határozza meg:
   
        num_maps = max(mapred.min.split.size, min(mapred.max.split.size, dfs.block.size))
   
    Általában az alapértelmezett érték hello *mapred.min.split.size* 0, az *mapred.max.split.size* van **Long.MAX** és az *dfs.block.size* 64 MB. Láthatja, adott hello adatok mérete, mert ezek a paraméterek "beállítása" hangolása őket lehetővé teszi használt mappers tootune hello száma.
4. Még más néhány **speciális beállítások** Hive optimalizálási teljesítmény alatt szerepelnek. Ezek lehetővé teszik tooset hello lefoglalt memória toomap és csökkentse a feladatok, és elősegíti a teljesítmény tökéletesítse. Ne feledje, hogy hello *mapreduce.reduce.memory.mb* nem lehet nagyobb, mint az egyes feldolgozó csomópontok hello Hadoop-fürt hello fizikai memória méretét.
   
        set mapreduce.map.memory.mb = 2048;
        set mapreduce.reduce.memory.mb=6144;
        set mapreduce.reduce.java.opts=-Xmx8192m;
        set mapred.reduce.tasks=128;
        set mapred.tasktracker.reduce.tasks.maximum=128;

