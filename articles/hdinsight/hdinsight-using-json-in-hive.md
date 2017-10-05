---
title: "Elemzése és a folyamat JSON-dokumentumokat a Hive HDInsight |} Microsoft Docs"
description: "Megtudhatja, hogyan használja a JSON-dokumentumok és elemezheti őket a HDInsight Hive eszközzel."
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
ms.openlocfilehash: bd136afebeceb0cd9c24cfc5f15601caa80a755e
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/03/2017
---
# <a name="process-and-analyze-json-documents-using-hive-in-hdinsight"></a>Feldolgozhatja és elemezheti a JSON-dokumentumok Hive hdinsight eszközzel

Megtudhatja, hogyan feldolgozhatja és elemezheti a Hive HDInsight JSON-fájlokat. A következő JSON-dokumentum az oktatóanyag célja:

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

A fájl megtalálható wasb://processjson@hditutorialdata.blob.core.windows.net/. További információ az Azure Blob storage használatával a hdinsight eszközzel, lásd: [használatát a HDFS-kompatibilis Azure Blob storage hadooppal a Hdinsightban](hdinsight-hadoop-use-blob-storage.md). Az alapértelmezett tároló, a fürt másolhat a fájlt.

Ebben az oktatóanyagban a Hive-konzol használata.  A Hive-konzol megnyitása utasításokért lásd: [használata a távoli asztal hdinsight Hadoop Hive](hdinsight-hadoop-use-hive-remote-desktop.md).

## <a name="flatten-json-documents"></a>JSON-dokumentumok egybesimítására
A következő szakaszban felsorolt módszerekhez a JSON-dokumentum, egy sorban. Ezért kell egybesimítására a JSON-dokumentum karakterláncra. Ha már egybesimított-e a JSON-dokumentumában, kihagyhatja ezt a lépést, és, lépjen a következő szakaszban a elemzése JSON-adatokat.

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

A nyers JSON-fájl  **wasb://processjson@hditutorialdata.blob.core.windows.net/** . A *StudentsRaw* Hive tábla pontok a nyers áttetsző JSON-dokumentumot.

A *StudentsOneLine* Hive tábla tárolja az adatokat a HDInsight alapértelmezett fájlrendszer alatt a */json/diákok/* elérési útja.

Az INSERT utasítás tölti fel a egybesimított JSON-adatokat tartalmazó StudentOneLine táblát.

A SELECT utasítás csak kell egy olyan sor visszaadása.

A SELECT utasítás kimenete a következő:

![A JSON-dokumentum egybesimítását.][image-hdi-hivejson-flatten]

## <a name="analyze-json-documents-in-hive"></a>A Hive JSON-dokumentumok elemzése
Hive három különböző mechanizmusokat, lekérdezéseket futtathat a JSON-dokumentumok biztosítja:

* használja a GET\_JSON\_objektum UDF (felhasználó által definiált függvény)
* Használja a JSON_TUPLE UDF-ben
* Egyéni SerDe használata
* írás a Python vagy egyéb nyelvek UDF saját. Lásd: [Ez a cikk] [ hdinsight-python] a Hive és a Python kód fut.

### <a name="use-the-getjsonobject-udf"></a>Használja a GET\_JSON_OBJECT UDF-ben
Hive biztosít egy beépített UDF nevű [json-objektum](https://cwiki.apache.org/confluence/display/Hive/LanguageManual+UDF#LanguageManualUDF-get_json_object), amely JSON lekérdezése futásidőben hajthat végre. Ez a módszer két argumentummal – a táblázat neve és a metódus nevét, amelynek a egybesimított JSON-dokumentum, és elemezni kell JSON mező. Nézzük például a UDF működése megjelenítéséhez.

A első és utolsó nevét az egyes student beolvasása

    SELECT
      GET_JSON_OBJECT(StudentsOneLine.json_body,'$.StudentDetails.FirstName'),
      GET_JSON_OBJECT(StudentsOneLine.json_body,'$.StudentDetails.LastName')
    FROM StudentsOneLine;

A kimeneti Ez a lekérdezés futtatásakor a konzolablakban.

![get_json_object UDF-ben][image-hdi-hivejson-getjsonobject]

A get-json_object UDF néhány korlátozások is.

* A lekérdezés minden mezője használatához a lekérdezés reparsing, az hatással van a teljesítményre.
* ELSŐ\_JSON_OBJECT() a tömb karakterlánc alakot adja vissza. A tömb konvertálása a Hive-tömb, kell használni a reguláris kifejezések lecseréli a kapcsos zárójeleket "[" és "]" is majd hívja a tömb első felosztás.

Ezért a Hive wiki a json_tuple használatát javasolja.  

### <a name="use-the-jsontuple-udf"></a>Használja a JSON_TUPLE UDF-ben
Egy másik UDF Hive által biztosított nevezik [json_tuple](https://cwiki.apache.org/confluence/display/Hive/LanguageManual+UDF#LanguageManualUDF-json_tuple), melyik teljesít jobban, mint [get_ json _object](https://cwiki.apache.org/confluence/display/Hive/LanguageManual+UDF#LanguageManualUDF-get_json_object). Ez a módszer kulcsok és a JSON karakterláncnak vesz igénybe, és egy függvény használatával értékek egy rekordot ad vissza. A következő lekérdezés a JSON-dokumentum a student-azonosítót és a besorolási adja vissza:

    SELECT q1.StudentId, q1.Grade
      FROM StudentsOneLine jt
      LATERAL VIEW JSON_TUPLE(jt.json_body, 'StudentId', 'Grade') q1
        AS StudentId, Grade;

A Hive-konzolon a parancsfájl kimenete:

![json_tuple UDF-ben][image-hdi-hivejson-jsontuple]

JSON\_REKORDOT használja a [nézet oldalirányú](https://cwiki.apache.org/confluence/display/Hive/LanguageManual+LateralView) struktúra, amely lehetővé teszi a json szintaxist\_virtuális tábla létrehozása az UDT függvény alkalmazásával az eredeti táblázat minden egyes sorára rekordot.  Összetett JSONs OLDALIRÁNYÚ nézet ismételt használata miatt túl kezelése nehézkessé válik. Ezenkívül JSON_TUPLE beágyazott JSONs nem tudja kezelni.

### <a name="use-custom-serde"></a>Egyéni SerDe használata
SerDe a legjobb választás elemzése beágyazott JSON-dokumentumokat, lehetővé teszi a JSON-séma határozza meg, és a séma használata a dokumentumok elemzése. Ebben az oktatóanyagban valamelyikét használja a népszerűbbnek SerDe által kidolgozott [Roberto Congiu](https://github.com/rcongiu).

**Az egyéni SerDe használata:**

1. Telepítés [Java SE Development Kit 7u55 JDK 1.7.0_55](http://www.oracle.com/technetwork/java/javase/downloads/java-archive-downloads-javase7-521261.html#jdk-7u55-oth-JPR). A JDK Windows X64-verzión válassza, ha a központi Windows-telepítési a HDInsight használni fog
   
   > [!WARNING]
   > A SerDe JDK 1.8 nem működik.
   > 
   > 
   
    A telepítés befejezése után adjon hozzá egy új felhasználó környezeti változó:
   
   1. Nyissa meg **nézet Speciális rendszerbeállítások** a Windows képernyőről.
   2. Kattintson a **környezeti változók**.  
   3. Adjon hozzá egy új **JAVA_HOME** környezeti változó mutató **C:\Program Files\Java\jdk1.7.0_55** vagy a JDK telepítve legyen.
      
      ![JDK megfelelő konfigurációs értékeire beállítása][image-hdi-hivejson-jdk]
2. Telepítés [Maven 3.3.1](http://mirror.olnevhost.net/pub/apache/maven/maven-3/3.3.1/binaries/apache-maven-3.3.1-bin.zip)
   
    Az elérési út a vezérlő a bin mappa hozzáadása Panel--> a fiók környezeti változók a rendszer változóinak szerkesztése. Az alábbi képernyőfelvételen látható ennek módjáról.
   
    ![Maven beállítása][image-hdi-hivejson-maven]
3. Klónozza a projektet a [Hive-JSON-SerDe](https://github.com/sheetaldolas/Hive-JSON-Serde/tree/master) github webhelyen. Ez az alábbi képernyőfelvételen látható módon a "Töltse le a Zip" gombra kattintva teheti meg.
   
    ![A projekt klónozása][image-hdi-hivejson-serde]

4: Nyissa meg a mappát, ahová letöltötte a csomaghoz, és a "mvn csomag" típusú. Ez a szükséges jar-fájlokat, majd másolhatja át a fürt hozzon létre.

5: a cél mappában, amelybe letöltötte a csomag gyökere alatt. A fürt átjárócsomópontjához a json-serde-1.1.9.9-Hive13-jar-with-dependencies.jar fájl feltöltéséhez. I általában elhelyezi a hive bináris mappában: C:\apps\dist\hive-0.13.0.2.1.11.0-2316\bin vagy más hasonló.

6: hive parancssorba írja be a "Hozzáadás jar /path/to/json-serde-1.1.9.9-Hive13-jar-with-dependencies.jar". Mivel abban az esetben a jar a C:\apps\dist\hive-0.13.x\bin mappában, képes közvetlenül hozzáadok nevű jar látható módon:

    add jar json-serde-1.1.9.9-Hive13-jar-with-dependencies.jar;

   ![JAR hozzáadása a projekthez][image-hdi-hivejson-addjar]

Most már készen áll a SerDe használandó lekérdezéseinek futtatásához a JSON-dokumentum.

A következő utasítás táblázatot hoz létre a megadott séma:

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

Utónév és a student vezetékneve listáját

    SELECT StudentDetails.FirstName, StudentDetails.LastName FROM json_table;

Ez az eredmény a Hive-konzolról.

![1 SerDe lekérdezés][image-hdi-hivejson-serde_query1]

A JSON-dokumentum eredményeinek összegét kiszámításához

    SELECT SUM(scores)
    FROM json_table jt
      lateral view explode(jt.StudentClassCollection.Score) collection as scores;

Az előző lekérdezés használ [oldalirányú nézet felbontása](https://cwiki.apache.org/confluence/display/Hive/LanguageManual+LateralView) , hogy ezeket is hozzáadja, bontsa ki a tömb a pontszámok UDF.

Ez a struktúra konzol kimenetét.

![SerDe lekérdezés 2][image-hdi-hivejson-serde_query2]

Kereséséhez, amelyek egy adott hallgató témák rendelkezik pontozza a mennyiségeket több mint 80 pontok:

    SELECT  
      jt.StudentClassCollection.ClassId
    FROM json_table jt
      lateral view explode(jt.StudentClassCollection.Score) collection as score  where score > 80;

Az előző lekérdezés get eltérően a Hive tömböt ad vissza,\_json\_objektum, amely egy karakterláncot ad vissza.

![SerDe lekérdezés 3][image-hdi-hivejson-serde_query3]

Ha azt szeretné, hogy skil nem megfelelően formázott JSON, majd, amint azt a [wiki lapján](https://github.com/sheetaldolas/Hive-JSON-Serde/tree/master) , ez SerDe érhet el, írja be a következő kódot:  

    ALTER TABLE json_table SET SERDEPROPERTIES ( "ignore.malformed.json" = "true");




## <a name="summary"></a>Összefoglalás
Végezetül a JSON operátor szerepel a Hive, az Ön által függ a forgatókönyvéhez. Ha egy egyszerű JSON-dokumentum, és csak akkor kell – kereshet egy mezőt is szeretne használni a Hive UDF get\_json\_objektum. Ha egynél több kulcs keressük meg, majd használhatja json_tuple. Ha a beágyazott dokumentumok, akkor a JSON SerDe kell használnia.

## <a name="next-steps"></a>Következő lépések

Az egyéb kapcsolódó cikkek lásd:

* [Apache log4j mintafájl elemzéséhez a HDInsight Hadoop Hive és a HiveQL használata](hdinsight-use-hive.md)
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
