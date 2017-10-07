---
title: "aaaAnalyze és a folyamat JSON-dokumentumokat a Hive HDInsight |} Microsoft Docs"
description: "Megtudhatja, hogyan toouse JSON-dokumentumokat, és elemezheti őket a HDInsight Hive eszközzel."
services: hdinsight
documentationcenter: 
author: mumian
manager: jhubbard
editor: cgronlun
ms.assetid: e17794e8-faae-4264-9434-67f61ea78f13
ms.service: hdinsight
ms.custom: hdinsightactive
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 04/26/2017
ms.author: jgao
ms.openlocfilehash: b4b20172e8553f91a446615dc52f2ea2ef24cd04
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="process-and-analyze-json-documents-using-hive-in-hdinsight"></a>Feldolgozhatja és elemezheti a JSON-dokumentumok Hive hdinsight eszközzel

Megtudhatja, hogyan tooprocess és elemzése a Hive HDInsight JSON-fájlokat. a következő JSON-dokumentum hello hello oktatóanyag szerepel:

    {
        "StudentId": "trgfg-5454-fdfdg-4346",
        "Grade": 7,
        "StudentDetails": [
            {
                "FirstName": "Peggy",
                "LastName": "Williams",
                "YearJoined": 2012
            }
        ],
        "StudentClassCollection": [
            {
                "ClassId": "89084343",
                "ClassParticipation": "Satisfied",
                "ClassParticipationRank": "High",
                "Score": 93,
                "PerformedActivity": false
            },
            {
                "ClassId": "78547522",
                "ClassParticipation": "NotSatisfied",
                "ClassParticipationRank": "None",
                "Score": 74,
                "PerformedActivity": false
            },
            {
                "ClassId": "78675563",
                "ClassParticipation": "Satisfied",
                "ClassParticipationRank": "Low",
                "Score": 83,
                "PerformedActivity": true
            }
        ]
    }

hello fájl található a wasb://processjson@hditutorialdata.blob.core.windows.net/. További információ az Azure Blob storage használatával a hdinsight eszközzel, lásd: [használatát a HDFS-kompatibilis Azure Blob storage hadooppal a Hdinsightban](hdinsight-hadoop-use-blob-storage.md). Hello fájl toohello alapértelmezett tárolót a fürt másolhatja.

Ebben az oktatóanyagban hello Hive konzol használata.  További tájékoztatást a hello Hive konzol megnyitása [használata a távoli asztal hdinsight Hadoop Hive](hdinsight-hadoop-use-hive-remote-desktop.md).

## <a name="flatten-json-documents"></a>JSON-dokumentumok egybesimítására
hello hello a következő szakaszban felsorolt módszerekhez hello JSON-dokumentum egy sorban. Ezért kell egybesimítására hello JSON-dokumentum tooa karakterlánc. Ha már egybesimított-e a JSON-dokumentumában, kihagyhatja ezt a lépést, és folytassa egyenes toohello a következő szakasz elemzése JSON-adatokat.

    DROP TABLE IF EXISTS StudentsRaw;
    CREATE EXTERNAL TABLE StudentsRaw (textcol string) STORED AS TEXTFILE LOCATION "wasb://processjson@hditutorialdata.blob.core.windows.net/";

    DROP TABLE IF EXISTS StudentsOneLine;
    CREATE EXTERNAL TABLE StudentsOneLine
    (
      json_body string
    )
    STORED AS TEXTFILE LOCATION '/json/students';

    INSERT OVERWRITE TABLE StudentsOneLine
    SELECT CONCAT_WS(' ',COLLECT_LIST(textcol)) AS singlelineJSON
          FROM (SELECT INPUT__FILE__NAME,BLOCK__OFFSET__INSIDE__FILE, textcol FROM StudentsRaw DISTRIBUTE BY INPUT__FILE__NAME SORT BY BLOCK__OFFSET__INSIDE__FILE) x
          GROUP BY INPUT__FILE__NAME;

    SELECT * FROM StudentsOneLine

hello nyers JSON-fájl itt található:  **wasb://processjson@hditutorialdata.blob.core.windows.net/** . Hello *StudentsRaw* Hive tábla toohello nyers áttetsző JSON-dokumentum mutat.

Hello *StudentsOneLine* Hive tábla hello adatot tárol a HDInsight alapértelmezett fájlrendszer alatt hello hello */json/diákok/* elérési útja.

hello INSERT utasítás egybesimított hello JSON-adatokat tartalmazó hello StudentOneLine táblát tölti fel.

hello SELECT utasítás csak kell egy olyan sor visszaadása.

Hello SELECT utasítás hello kimenete a következő:

![Egybesimítását hello JSON-dokumentum.][image-hdi-hivejson-flatten]

## <a name="analyze-json-documents-in-hive"></a>A Hive JSON-dokumentumok elemzése
Hive toorun lekérdezések Ez a három különböző mechanizmusok JSON-dokumentumokat:

* használja a hello GET\_JSON\_objektum UDF (felhasználó által definiált függvény)
* hello JSON_TUPLE UDF használata
* Egyéni SerDe használata
* írás a Python vagy egyéb nyelvek UDF saját. Lásd: [Ez a cikk] [ hdinsight-python] a Hive és a Python kód fut.

### <a name="use-hello-getjsonobject-udf"></a>Használjon hello GET\_JSON_OBJECT UDF-ben
Hive biztosít egy beépített UDF nevű [json-objektum](https://cwiki.apache.org/confluence/display/Hive/LanguageManual+UDF#LanguageManualUDF-get_json_object), amely JSON lekérdezése futásidőben hajthat végre. Ez a módszer két argumentummal – hello táblanév és metódus neve, amelynek hello egybesimított JSON dokumentum és hello JSON mező, amelyet a toobe értelmezni. Egy példa toosee az UDF működése vizsgáljuk meg.

Hello utó- és vezetéknevet minden student beolvasása

    SELECT
      GET_JSON_OBJECT(StudentsOneLine.json_body,'$.StudentDetails.FirstName'),
      GET_JSON_OBJECT(StudentsOneLine.json_body,'$.StudentDetails.LastName')
    FROM StudentsOneLine;

Itt egy hello kimenet, amikor ez a lekérdezés futtatása a konzolablakban.

![get_json_object UDF-ben][image-hdi-hivejson-getjsonobject]

Néhány korlátozás hello get-json_object az UDF-ben.

* Egyes mezők hello lekérdezés használatához hello lekérdezés reparsing, hello teljesítmény hatással van.
* ELSŐ\_JSON_OBJECT() hello karakterláncos ábrázolása tömbjét adja vissza. tooconvert a tömb tooa Hive tömb rendelkezik toouse reguláris kifejezések tooreplace szögletes zárójelek hello ' ["és"] ", és ezután is hívás osztani tooget hello tömb.

Ezért hello Hive wiki a json_tuple használatát javasolja.  

### <a name="use-hello-jsontuple-udf"></a>Hello JSON_TUPLE UDF használata
Egy másik UDF Hive által biztosított nevezik [json_tuple](https://cwiki.apache.org/confluence/display/Hive/LanguageManual+UDF#LanguageManualUDF-json_tuple), melyik teljesít jobban, mint [get_ json _object](https://cwiki.apache.org/confluence/display/Hive/LanguageManual+UDF#LanguageManualUDF-get_json_object). Ez a módszer kulcsok és a JSON karakterláncnak vesz igénybe, és egy függvény használatával értékek egy rekordot ad vissza. hello következő lekérdezés adja vissza hello student azonosítója és hello osztályú hello JSON-dokumentum:

    SELECT q1.StudentId, q1.Grade
      FROM StudentsOneLine jt
      LATERAL VIEW JSON_TUPLE(jt.json_body, 'StudentId', 'Grade') q1
        AS StudentId, Grade;

a parancsfájl hello Hive konzolon hello kimenete:

![json_tuple UDF-ben][image-hdi-hivejson-jsontuple]

JSON\_REKORDOT használ hello [nézet oldalirányú](https://cwiki.apache.org/confluence/display/Hive/LanguageManual+LateralView) struktúra, amely lehetővé teszi a json szintaxist\_rekordot toocreate hello UDT függvény tooeach hello eredeti táblázat sorában alkalmazásával virtuális tábla.  Ismétlődő hello miatt túl kezelése nehézkessé válik összetett JSONs OLDALIRÁNYÚ nézet használja. Ezenkívül JSON_TUPLE beágyazott JSONs nem tudja kezelni.

### <a name="use-custom-serde"></a>Egyéni SerDe használata
SerDe hello legjobb választás az elemzés beágyazott JSON-dokumentumokat, lehetővé teszi toodefine hello JSON-séma, és hello séma tooparse hello dokumentumokhoz. Ebben az oktatóanyagban valamelyikét használja hello által kidolgozott népszerűbbnek SerDe [Roberto Congiu](https://github.com/rcongiu).

**toouse egyéni SerDe hello:**

1. Telepítés [Java SE Development Kit 7u55 JDK 1.7.0_55](http://www.oracle.com/technetwork/java/javase/downloads/java-archive-downloads-javase7-521261.html#jdk-7u55-oth-JPR). Válassza a hello JDK hello Windows X64 verziót, ha toobe hello központi Windows-telepítési a HDInsight használatával fog
   
   > [!WARNING]
   > A SerDe JDK 1.8 nem működik.
   > 
   > 
   
    Hello telepítés befejezése után adjon hozzá egy új felhasználó környezeti változó:
   
   1. Nyissa meg **nézet Speciális rendszerbeállítások** hello Windows képernyőről.
   2. Kattintson a **környezeti változók**.  
   3. Adjon hozzá egy új **JAVA_HOME** környezeti változó túl mutat**C:\Program Files\Java\jdk1.7.0_55** vagy a JDK telepítve legyen.
      
      ![JDK megfelelő konfigurációs értékeire beállítása][image-hdi-hivejson-jdk]
2. Telepítés [Maven 3.3.1](http://mirror.olnevhost.net/pub/apache/maven/maven-3/3.3.1/binaries/apache-maven-3.3.1-bin.zip)
   
    Hello bin mappa tooyour elérési út megnyitásával tooControl Panel--> Szerkesztés hello rendszerváltozók a fiók környezeti változók. hello alábbi képernyőfelvételen bemutatja, hogyan toodo ez.
   
    ![Maven beállítása][image-hdi-hivejson-maven]
3. A Klónozás hello projekt [Hive-JSON-SerDe](https://github.com/sheetaldolas/Hive-JSON-Serde/tree/master) github webhelyen. Ez látható módon a következő képernyőkép hello hello "Töltse le a Zip" gombra kattintva teheti meg.
   
    ![A Klónozás hello projekt][image-hdi-hivejson-serde]

4: Nyissa meg toohello mappát, ahova a csomagot, és a "mvn csomag" típusú letöltötte rendelkezik. Ez kell hello szükséges jar-fájlok létrehozása, amely ezután másolhatja toohello fürt keresztül.

5: Ugrás toohello célmappa hello csomag kezelőportálon hello gyökérmappájában. Töltse fel a hello json-serde-1.1.9.9-Hive13-jar-with-dependencies.jar fájl toohead csomópontos a fürt. I általában használatba hello hive bináris mappában: C:\apps\dist\hive-0.13.0.2.1.11.0-2316\bin vagy más hasonló.

6: hello hive parancssorba írja be a "Hozzáadás jar /path/to/json-serde-1.1.9.9-Hive13-jar-with-dependencies.jar". Mivel abban az esetben hello jar hello C:\apps\dist\hive-0.13.x\bin mappában, hello jar hello neve, ahogy a közvetlenül hozzáadok is:

    add jar json-serde-1.1.9.9-Hive13-jar-with-dependencies.jar;

   ![JAR tooyour projekt hozzáadása][image-hdi-hivejson-addjar]

Most már áll készen toouse hello SerDe toorun lekérdezések hello JSON-dokumentum.

a következő utasítás hello táblázatot hoz létre a megadott séma:

    DROP TABLE json_table;
    CREATE EXTERNAL TABLE json_table (
      StudentId string,
      Grade int,
      StudentDetails array<struct<
          FirstName:string,
          LastName:string,
          YearJoined:int
          >
      >,
      StudentClassCollection array<struct<
          ClassId:string,
          ClassParticipation:string,
          ClassParticipationRank:string,
          Score:int,
          PerformedActivity:boolean
          >
      >
    ) ROW FORMAT SERDE 'org.openx.data.jsonserde.JsonSerDe'
    LOCATION '/json/students';

hello utó- és vezetékneve hello student toolist

    SELECT StudentDetails.FirstName, StudentDetails.LastName FROM json_table;

Itt található hello eredmény hello Hive konzolról.

![1 SerDe lekérdezés][image-hdi-hivejson-serde_query1]

pontszámok hello JSON-dokumentum toocalculate hello összege

    SELECT SUM(scores)
    FROM json_table jt
      lateral view explode(jt.StudentClassCollection.Score) collection as scores;

lekérdezés használ megelőző hello [oldalirányú nézet felbontása](https://cwiki.apache.org/confluence/display/Hive/LanguageManual+LateralView) UDF tooexpand hello pontszámok tömbje, így ezeket hozzáadja.

Itt egy hello kimenet hello Hive konzolról.

![SerDe lekérdezés 2][image-hdi-hivejson-serde_query2]

toofind, amely egy adott hallgató témák rendelkezik program pontozza a mennyiségeket több mint 80 pontok:

    SELECT  
      jt.StudentClassCollection.ClassId
    FROM json_table jt
      lateral view explode(jt.StudentClassCollection.Score) collection as score  where score > 80;

hello előző lekérdezés tömböt ad vissza, a Hive eltérően get\_json\_objektum, amely egy karakterláncot ad vissza.

![SerDe lekérdezés 3][image-hdi-hivejson-serde_query3]

Ha azt szeretné, hogy tooskil nem megfelelően formázott JSON, majd azt hello mint [wiki lapján](https://github.com/sheetaldolas/Hive-JSON-Serde/tree/master) , ez SerDe érhet el, írja be a kódját a következő hello:  

    ALTER TABLE json_table SET SERDEPROPERTIES ( "ignore.malformed.json" = "true");




## <a name="summary"></a>Összefoglalás
Végezetül hello JSON operátor a struktúrát, amelyben a forgatókönyvtől függ. Ha egy egyszerű JSON-dokumentum, és csak akkor kell egy mező toolook – választhat toouse hello Hive UDF get\_json\_objektum. Ha egynél több kulcsfontosságú toolook, json_tuple használhatja. Ha a beágyazott dokumentumok, akkor hello JSON SerDe kell használnia.

## <a name="next-steps"></a>Következő lépések

Az egyéb kapcsolódó cikkek lásd:

* [A HDInsight tooanalyze Apache log4j mintafájl Hadoop Hive és a HiveQL használata](hdinsight-use-hive.md)
* [Repülési késleltetés adatok elemzése a Hive HDInsight használatával](hdinsight-analyze-flight-delay-data.md)
* [Hdinsight Hive eszközzel Twitter-adatok elemzése](hdinsight-analyze-twitter-data.md)
* [Egy Azure Cosmos DB és a HDInsight Hadoop-feladat futtatása](../documentdb/documentdb-run-hadoop-with-hdinsight.md)

[hdinsight-python]: hdinsight-python.md

[image-hdi-hivejson-flatten]: ./media/hdinsight-using-json-in-hive/flatten.png
[image-hdi-hivejson-getjsonobject]: ./media/hdinsight-using-json-in-hive/getjsonobject.png
[image-hdi-hivejson-jsontuple]: ./media/hdinsight-using-json-in-hive/jsontuple.png
[image-hdi-hivejson-jdk]: ./media/hdinsight-using-json-in-hive/jdk.png
[image-hdi-hivejson-maven]: ./media/hdinsight-using-json-in-hive/maven.png
[image-hdi-hivejson-serde]: ./media/hdinsight-using-json-in-hive/serde.png
[image-hdi-hivejson-addjar]: ./media/hdinsight-using-json-in-hive/addjar.png
[image-hdi-hivejson-serde_query1]: ./media/hdinsight-using-json-in-hive/serde_query1.png
[image-hdi-hivejson-serde_query2]: ./media/hdinsight-using-json-in-hive/serde_query2.png
[image-hdi-hivejson-serde_query3]: ./media/hdinsight-using-json-in-hive/serde_query3.png
[image-hdi-hivejson-serde_result]: ./media/hdinsight-using-json-in-hive/serde_result.png
