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
# <a name="process-and-analyze-json-documents-using-hive-in-hdinsight"></a><span data-ttu-id="99ddc-103">Feldolgozhatja és elemezheti a JSON-dokumentumok Hive hdinsight eszközzel</span><span class="sxs-lookup"><span data-stu-id="99ddc-103">Process and analyze JSON documents using Hive in HDInsight</span></span>

<span data-ttu-id="99ddc-104">Megtudhatja, hogyan feldolgozhatja és elemezheti a Hive HDInsight JSON-fájlokat.</span><span class="sxs-lookup"><span data-stu-id="99ddc-104">Learn how to process and analyze JSON files using Hive in HDInsight.</span></span> <span data-ttu-id="99ddc-105">A következő JSON-dokumentum az oktatóanyag célja:</span><span class="sxs-lookup"><span data-stu-id="99ddc-105">The following JSON document is used in the tutorial:</span></span>

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

<span data-ttu-id="99ddc-106">A fájl megtalálható wasb://processjson@hditutorialdata.blob.core.windows.net/.</span><span class="sxs-lookup"><span data-stu-id="99ddc-106">The file can be found at wasb://processjson@hditutorialdata.blob.core.windows.net/.</span></span> <span data-ttu-id="99ddc-107">További információ az Azure Blob storage használatával a hdinsight eszközzel, lásd: [használatát a HDFS-kompatibilis Azure Blob storage hadooppal a Hdinsightban](hdinsight-hadoop-use-blob-storage.md).</span><span class="sxs-lookup"><span data-stu-id="99ddc-107">For more information on using Azure Blob storage with HDInsight, see [Use HDFS-compatible Azure Blob storage with Hadoop in HDInsight](hdinsight-hadoop-use-blob-storage.md).</span></span> <span data-ttu-id="99ddc-108">Az alapértelmezett tároló, a fürt másolhat a fájlt.</span><span class="sxs-lookup"><span data-stu-id="99ddc-108">You can copy the file to the default container of your cluster.</span></span>

<span data-ttu-id="99ddc-109">Ebben az oktatóanyagban a Hive-konzol használata.</span><span class="sxs-lookup"><span data-stu-id="99ddc-109">In this tutorial, you use the Hive console.</span></span>  <span data-ttu-id="99ddc-110">A Hive-konzol megnyitása utasításokért lásd: [használata a távoli asztal hdinsight Hadoop Hive](hdinsight-hadoop-use-hive-remote-desktop.md).</span><span class="sxs-lookup"><span data-stu-id="99ddc-110">For instructions of opening the Hive console, see [Use Hive with Hadoop on HDInsight with Remote Desktop](hdinsight-hadoop-use-hive-remote-desktop.md).</span></span>

## <a name="flatten-json-documents"></a><span data-ttu-id="99ddc-111">JSON-dokumentumok egybesimítására</span><span class="sxs-lookup"><span data-stu-id="99ddc-111">Flatten JSON documents</span></span>
<span data-ttu-id="99ddc-112">A következő szakaszban felsorolt módszerekhez a JSON-dokumentum, egy sorban.</span><span class="sxs-lookup"><span data-stu-id="99ddc-112">The methods listed in the next section require the JSON document in a single row.</span></span> <span data-ttu-id="99ddc-113">Ezért kell egybesimítására a JSON-dokumentum karakterláncra.</span><span class="sxs-lookup"><span data-stu-id="99ddc-113">So you must flatten the JSON document to a string.</span></span> <span data-ttu-id="99ddc-114">Ha már egybesimított-e a JSON-dokumentumában, kihagyhatja ezt a lépést, és, lépjen a következő szakaszban a elemzése JSON-adatokat.</span><span class="sxs-lookup"><span data-stu-id="99ddc-114">If your JSON document is already flattened, you can skip this step and go straight to the next section on Analyzing JSON data.</span></span>

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

<span data-ttu-id="99ddc-115">A nyers JSON-fájl  **wasb://processjson@hditutorialdata.blob.core.windows.net/** .</span><span class="sxs-lookup"><span data-stu-id="99ddc-115">The raw JSON file is located at **wasb://processjson@hditutorialdata.blob.core.windows.net/**.</span></span> <span data-ttu-id="99ddc-116">A *StudentsRaw* Hive tábla pontok a nyers áttetsző JSON-dokumentumot.</span><span class="sxs-lookup"><span data-stu-id="99ddc-116">The *StudentsRaw* Hive table points to the raw unflattened JSON document.</span></span>

<span data-ttu-id="99ddc-117">A *StudentsOneLine* Hive tábla tárolja az adatokat a HDInsight alapértelmezett fájlrendszer alatt a */json/diákok/* elérési útja.</span><span class="sxs-lookup"><span data-stu-id="99ddc-117">The *StudentsOneLine* Hive table stores the data in the HDInsight default file system under the */json/students/* path.</span></span>

<span data-ttu-id="99ddc-118">Az INSERT utasítás tölti fel a egybesimított JSON-adatokat tartalmazó StudentOneLine táblát.</span><span class="sxs-lookup"><span data-stu-id="99ddc-118">The INSERT statement populates the StudentOneLine table with the flattened JSON data.</span></span>

<span data-ttu-id="99ddc-119">A SELECT utasítás csak kell egy olyan sor visszaadása.</span><span class="sxs-lookup"><span data-stu-id="99ddc-119">The SELECT statement shall only return one row.</span></span>

<span data-ttu-id="99ddc-120">A SELECT utasítás kimenete a következő:</span><span class="sxs-lookup"><span data-stu-id="99ddc-120">Here is the output of the SELECT statement:</span></span>

![A JSON-dokumentum egybesimítását.][image-hdi-hivejson-flatten]

## <a name="analyze-json-documents-in-hive"></a><span data-ttu-id="99ddc-122">A Hive JSON-dokumentumok elemzése</span><span class="sxs-lookup"><span data-stu-id="99ddc-122">Analyze JSON documents in Hive</span></span>
<span data-ttu-id="99ddc-123">Hive három különböző mechanizmusokat, lekérdezéseket futtathat a JSON-dokumentumok biztosítja:</span><span class="sxs-lookup"><span data-stu-id="99ddc-123">Hive provides three different mechanisms to run queries on JSON documents:</span></span>

* <span data-ttu-id="99ddc-124">használja a GET\_JSON\_objektum UDF (felhasználó által definiált függvény)</span><span class="sxs-lookup"><span data-stu-id="99ddc-124">use the GET\_JSON\_OBJECT UDF (User-defined function)</span></span>
* <span data-ttu-id="99ddc-125">Használja a JSON_TUPLE UDF-ben</span><span class="sxs-lookup"><span data-stu-id="99ddc-125">use the JSON_TUPLE UDF</span></span>
* <span data-ttu-id="99ddc-126">Egyéni SerDe használata</span><span class="sxs-lookup"><span data-stu-id="99ddc-126">use custom SerDe</span></span>
* <span data-ttu-id="99ddc-127">írás a Python vagy egyéb nyelvek UDF saját.</span><span class="sxs-lookup"><span data-stu-id="99ddc-127">write you own UDF using Python or other languages.</span></span> <span data-ttu-id="99ddc-128">Lásd: [Ez a cikk] [ hdinsight-python] a Hive és a Python kód fut.</span><span class="sxs-lookup"><span data-stu-id="99ddc-128">See [this article][hdinsight-python] on running your own Python code with Hive.</span></span>

### <a name="use-the-getjsonobject-udf"></a><span data-ttu-id="99ddc-129">Használja a GET\_JSON_OBJECT UDF-ben</span><span class="sxs-lookup"><span data-stu-id="99ddc-129">Use the GET\_JSON_OBJECT UDF</span></span>
<span data-ttu-id="99ddc-130">Hive biztosít egy beépített UDF nevű [json-objektum](https://cwiki.apache.org/confluence/display/Hive/LanguageManual+UDF#LanguageManualUDF-get_json_object), amely JSON lekérdezése futásidőben hajthat végre.</span><span class="sxs-lookup"><span data-stu-id="99ddc-130">Hive provides a built-in UDF called [get json object](https://cwiki.apache.org/confluence/display/Hive/LanguageManual+UDF#LanguageManualUDF-get_json_object), which can perform JSON querying during run time.</span></span> <span data-ttu-id="99ddc-131">Ez a módszer két argumentummal – a táblázat neve és a metódus nevét, amelynek a egybesimított JSON-dokumentum, és elemezni kell JSON mező.</span><span class="sxs-lookup"><span data-stu-id="99ddc-131">This method takes two arguments – the table name and method name, which has the flattened JSON document and the JSON field that needs to be parsed.</span></span> <span data-ttu-id="99ddc-132">Nézzük például a UDF működése megjelenítéséhez.</span><span class="sxs-lookup"><span data-stu-id="99ddc-132">Let’s look at an example to see how this UDF works.</span></span>

<span data-ttu-id="99ddc-133">A első és utolsó nevét az egyes student beolvasása</span><span class="sxs-lookup"><span data-stu-id="99ddc-133">Get the first name and last name for each student</span></span>

    SELECT
      GET_JSON_OBJECT(StudentsOneLine.json_body,'$.StudentDetails.FirstName'),
      GET_JSON_OBJECT(StudentsOneLine.json_body,'$.StudentDetails.LastName')
    FROM StudentsOneLine;

<span data-ttu-id="99ddc-134">A kimeneti Ez a lekérdezés futtatásakor a konzolablakban.</span><span class="sxs-lookup"><span data-stu-id="99ddc-134">Here is the output when running this query in console window.</span></span>

![get_json_object UDF-ben][image-hdi-hivejson-getjsonobject]

<span data-ttu-id="99ddc-136">A get-json_object UDF néhány korlátozások is.</span><span class="sxs-lookup"><span data-stu-id="99ddc-136">There are a few limitations of the get-json_object UDF.</span></span>

* <span data-ttu-id="99ddc-137">A lekérdezés minden mezője használatához a lekérdezés reparsing, az hatással van a teljesítményre.</span><span class="sxs-lookup"><span data-stu-id="99ddc-137">Because each field in the query requires reparsing the query, it affects the performance.</span></span>
* <span data-ttu-id="99ddc-138">ELSŐ\_JSON_OBJECT() a tömb karakterlánc alakot adja vissza.</span><span class="sxs-lookup"><span data-stu-id="99ddc-138">GET\_JSON_OBJECT() returns the string representation of an array.</span></span> <span data-ttu-id="99ddc-139">A tömb konvertálása a Hive-tömb, kell használni a reguláris kifejezések lecseréli a kapcsos zárójeleket "[" és "]" is majd hívja a tömb első felosztás.</span><span class="sxs-lookup"><span data-stu-id="99ddc-139">To convert this array to a Hive array, you have to use regular expressions to replace the square brackets ‘[‘ and ‘]’ and then also call split to get the array.</span></span>

<span data-ttu-id="99ddc-140">Ezért a Hive wiki a json_tuple használatát javasolja.</span><span class="sxs-lookup"><span data-stu-id="99ddc-140">This is why the Hive wiki recommends using json_tuple.</span></span>  

### <a name="use-the-jsontuple-udf"></a><span data-ttu-id="99ddc-141">Használja a JSON_TUPLE UDF-ben</span><span class="sxs-lookup"><span data-stu-id="99ddc-141">Use the JSON_TUPLE UDF</span></span>
<span data-ttu-id="99ddc-142">Egy másik UDF Hive által biztosított nevezik [json_tuple](https://cwiki.apache.org/confluence/display/Hive/LanguageManual+UDF#LanguageManualUDF-json_tuple), melyik teljesít jobban, mint [get_ json _object](https://cwiki.apache.org/confluence/display/Hive/LanguageManual+UDF#LanguageManualUDF-get_json_object).</span><span class="sxs-lookup"><span data-stu-id="99ddc-142">Another UDF provided by Hive is called [json_tuple](https://cwiki.apache.org/confluence/display/Hive/LanguageManual+UDF#LanguageManualUDF-json_tuple), which performs better than [get_ json _object](https://cwiki.apache.org/confluence/display/Hive/LanguageManual+UDF#LanguageManualUDF-get_json_object).</span></span> <span data-ttu-id="99ddc-143">Ez a módszer kulcsok és a JSON karakterláncnak vesz igénybe, és egy függvény használatával értékek egy rekordot ad vissza.</span><span class="sxs-lookup"><span data-stu-id="99ddc-143">This method takes a set of keys and a JSON string, and returns a tuple of values using one function.</span></span> <span data-ttu-id="99ddc-144">A következő lekérdezés a JSON-dokumentum a student-azonosítót és a besorolási adja vissza:</span><span class="sxs-lookup"><span data-stu-id="99ddc-144">The following query returns the student id and the grade from the JSON document:</span></span>

    SELECT q1.StudentId, q1.Grade
      FROM StudentsOneLine jt
      LATERAL VIEW JSON_TUPLE(jt.json_body, 'StudentId', 'Grade') q1
        AS StudentId, Grade;

<span data-ttu-id="99ddc-145">A Hive-konzolon a parancsfájl kimenete:</span><span class="sxs-lookup"><span data-stu-id="99ddc-145">The output of this script in the Hive console:</span></span>

![json_tuple UDF-ben][image-hdi-hivejson-jsontuple]

<span data-ttu-id="99ddc-147">JSON\_REKORDOT használja a [nézet oldalirányú](https://cwiki.apache.org/confluence/display/Hive/LanguageManual+LateralView) struktúra, amely lehetővé teszi a json szintaxist\_virtuális tábla létrehozása az UDT függvény alkalmazásával az eredeti táblázat minden egyes sorára rekordot.</span><span class="sxs-lookup"><span data-stu-id="99ddc-147">JSON\_TUPLE uses the [lateral view](https://cwiki.apache.org/confluence/display/Hive/LanguageManual+LateralView) syntax in Hive, which allows json\_tuple to create a virtual table by applying the UDT function to each row of the original table.</span></span>  <span data-ttu-id="99ddc-148">Összetett JSONs OLDALIRÁNYÚ nézet ismételt használata miatt túl kezelése nehézkessé válik.</span><span class="sxs-lookup"><span data-stu-id="99ddc-148">Complex JSONs become too unwieldy because of the repeated use of LATERAL VIEW.</span></span> <span data-ttu-id="99ddc-149">Ezenkívül JSON_TUPLE beágyazott JSONs nem tudja kezelni.</span><span class="sxs-lookup"><span data-stu-id="99ddc-149">Furthermore, JSON_TUPLE cannot handle nested JSONs.</span></span>

### <a name="use-custom-serde"></a><span data-ttu-id="99ddc-150">Egyéni SerDe használata</span><span class="sxs-lookup"><span data-stu-id="99ddc-150">Use custom SerDe</span></span>
<span data-ttu-id="99ddc-151">SerDe a legjobb választás elemzése beágyazott JSON-dokumentumokat, lehetővé teszi a JSON-séma határozza meg, és a séma használata a dokumentumok elemzése.</span><span class="sxs-lookup"><span data-stu-id="99ddc-151">SerDe is the best choice for parsing nested JSON documents, it allows you to define the JSON schema, and use the schema to parse the documents.</span></span> <span data-ttu-id="99ddc-152">Ebben az oktatóanyagban valamelyikét használja a népszerűbbnek SerDe által kidolgozott [Roberto Congiu](https://github.com/rcongiu).</span><span class="sxs-lookup"><span data-stu-id="99ddc-152">In this tutorial, you use one of the more popular SerDe that has been developed by [Roberto Congiu](https://github.com/rcongiu).</span></span>

<span data-ttu-id="99ddc-153">**Az egyéni SerDe használata:**</span><span class="sxs-lookup"><span data-stu-id="99ddc-153">**To use the custom SerDe:**</span></span>

1. <span data-ttu-id="99ddc-154">Telepítés [Java SE Development Kit 7u55 JDK 1.7.0_55](http://www.oracle.com/technetwork/java/javase/downloads/java-archive-downloads-javase7-521261.html#jdk-7u55-oth-JPR).</span><span class="sxs-lookup"><span data-stu-id="99ddc-154">Install [Java SE Development Kit 7u55 JDK 1.7.0_55](http://www.oracle.com/technetwork/java/javase/downloads/java-archive-downloads-javase7-521261.html#jdk-7u55-oth-JPR).</span></span> <span data-ttu-id="99ddc-155">A JDK Windows X64-verzión válassza, ha a központi Windows-telepítési a HDInsight használni fog</span><span class="sxs-lookup"><span data-stu-id="99ddc-155">Choose the Windows X64 version of the JDK if you are going to be using the Windows deployment of HDInsight</span></span>
   
   > [!WARNING]
   > <span data-ttu-id="99ddc-156">A SerDe JDK 1.8 nem működik.</span><span class="sxs-lookup"><span data-stu-id="99ddc-156">JDK 1.8 doesn't work with this SerDe.</span></span>
   > 
   > 
   
    <span data-ttu-id="99ddc-157">A telepítés befejezése után adjon hozzá egy új felhasználó környezeti változó:</span><span class="sxs-lookup"><span data-stu-id="99ddc-157">After the installation is completed, add a new user environment variable:</span></span>
   
   1. <span data-ttu-id="99ddc-158">Nyissa meg **nézet Speciális rendszerbeállítások** a Windows képernyőről.</span><span class="sxs-lookup"><span data-stu-id="99ddc-158">Open **View advanced system settings** from the Windows screen.</span></span>
   2. <span data-ttu-id="99ddc-159">Kattintson a **környezeti változók**.</span><span class="sxs-lookup"><span data-stu-id="99ddc-159">Click **Environment Variables**.</span></span>  
   3. <span data-ttu-id="99ddc-160">Adjon hozzá egy új **JAVA_HOME** környezeti változó mutató **C:\Program Files\Java\jdk1.7.0_55** vagy a JDK telepítve legyen.</span><span class="sxs-lookup"><span data-stu-id="99ddc-160">Add a new **JAVA_HOME** environment variable is pointing to **C:\Program Files\Java\jdk1.7.0_55** or wherever your JDK is installed.</span></span>
      
      ![JDK megfelelő konfigurációs értékeire beállítása][image-hdi-hivejson-jdk]
2. <span data-ttu-id="99ddc-162">Telepítés [Maven 3.3.1](http://mirror.olnevhost.net/pub/apache/maven/maven-3/3.3.1/binaries/apache-maven-3.3.1-bin.zip)</span><span class="sxs-lookup"><span data-stu-id="99ddc-162">Install [Maven 3.3.1](http://mirror.olnevhost.net/pub/apache/maven/maven-3/3.3.1/binaries/apache-maven-3.3.1-bin.zip)</span></span>
   
    <span data-ttu-id="99ddc-163">Az elérési út a vezérlő a bin mappa hozzáadása Panel--> a fiók környezeti változók a rendszer változóinak szerkesztése.</span><span class="sxs-lookup"><span data-stu-id="99ddc-163">Add the bin folder to your path by going to Control Panel-->Edit the System Variables for your account Environment variables.</span></span> <span data-ttu-id="99ddc-164">Az alábbi képernyőfelvételen látható ennek módjáról.</span><span class="sxs-lookup"><span data-stu-id="99ddc-164">The following screenshot shows you how to do this.</span></span>
   
    ![Maven beállítása][image-hdi-hivejson-maven]
3. <span data-ttu-id="99ddc-166">Klónozza a projektet a [Hive-JSON-SerDe](https://github.com/sheetaldolas/Hive-JSON-Serde/tree/master) github webhelyen.</span><span class="sxs-lookup"><span data-stu-id="99ddc-166">Clone the project from [Hive-JSON-SerDe](https://github.com/sheetaldolas/Hive-JSON-Serde/tree/master) github site.</span></span> <span data-ttu-id="99ddc-167">Ez az alábbi képernyőfelvételen látható módon a "Töltse le a Zip" gombra kattintva teheti meg.</span><span class="sxs-lookup"><span data-stu-id="99ddc-167">You can do this by clicking on the “Download Zip” button as shown in the following screenshot.</span></span>
   
    ![A projekt klónozása][image-hdi-hivejson-serde]

<span data-ttu-id="99ddc-169">4: Nyissa meg a mappát, ahová letöltötte a csomaghoz, és a "mvn csomag" típusú.</span><span class="sxs-lookup"><span data-stu-id="99ddc-169">4: Go to the folder where you have downloaded this package and then type “mvn package”.</span></span> <span data-ttu-id="99ddc-170">Ez a szükséges jar-fájlokat, majd másolhatja át a fürt hozzon létre.</span><span class="sxs-lookup"><span data-stu-id="99ddc-170">This should create the necessary jar files that you can then copy over to the cluster.</span></span>

<span data-ttu-id="99ddc-171">5: a cél mappában, amelybe letöltötte a csomag gyökere alatt.</span><span class="sxs-lookup"><span data-stu-id="99ddc-171">5: Go to the target folder under the root folder where you downloaded the package.</span></span> <span data-ttu-id="99ddc-172">A fürt átjárócsomópontjához a json-serde-1.1.9.9-Hive13-jar-with-dependencies.jar fájl feltöltéséhez.</span><span class="sxs-lookup"><span data-stu-id="99ddc-172">Upload the json-serde-1.1.9.9-Hive13-jar-with-dependencies.jar file to head-node of your cluster.</span></span> <span data-ttu-id="99ddc-173">I általában elhelyezi a hive bináris mappában: C:\apps\dist\hive-0.13.0.2.1.11.0-2316\bin vagy más hasonló.</span><span class="sxs-lookup"><span data-stu-id="99ddc-173">I usually put it under the hive binary folder: C:\apps\dist\hive-0.13.0.2.1.11.0-2316\bin or something similar.</span></span>

<span data-ttu-id="99ddc-174">6: hive parancssorba írja be a "Hozzáadás jar /path/to/json-serde-1.1.9.9-Hive13-jar-with-dependencies.jar".</span><span class="sxs-lookup"><span data-stu-id="99ddc-174">6: In the hive prompt, type “add jar /path/to/json-serde-1.1.9.9-Hive13-jar-with-dependencies.jar”.</span></span> <span data-ttu-id="99ddc-175">Mivel abban az esetben a jar a C:\apps\dist\hive-0.13.x\bin mappában, képes közvetlenül hozzáadok nevű jar látható módon:</span><span class="sxs-lookup"><span data-stu-id="99ddc-175">Since in my case, the jar is in the C:\apps\dist\hive-0.13.x\bin folder, I can directly add the jar with the name as shown:</span></span>

    add jar json-serde-1.1.9.9-Hive13-jar-with-dependencies.jar;

   ![JAR hozzáadása a projekthez][image-hdi-hivejson-addjar]

<span data-ttu-id="99ddc-177">Most már készen áll a SerDe használandó lekérdezéseinek futtatásához a JSON-dokumentum.</span><span class="sxs-lookup"><span data-stu-id="99ddc-177">Now, you are ready to use the SerDe to run queries against the JSON document.</span></span>

<span data-ttu-id="99ddc-178">A következő utasítás táblázatot hoz létre a megadott séma:</span><span class="sxs-lookup"><span data-stu-id="99ddc-178">The following statement creates a table with a defined schema:</span></span>

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

<span data-ttu-id="99ddc-179">Utónév és a student vezetékneve listáját</span><span class="sxs-lookup"><span data-stu-id="99ddc-179">To list the first name and last name of the student</span></span>

    SELECT StudentDetails.FirstName, StudentDetails.LastName FROM json_table;

<span data-ttu-id="99ddc-180">Ez az eredmény a Hive-konzolról.</span><span class="sxs-lookup"><span data-stu-id="99ddc-180">Here is the result from the Hive console.</span></span>

![1 SerDe lekérdezés][image-hdi-hivejson-serde_query1]

<span data-ttu-id="99ddc-182">A JSON-dokumentum eredményeinek összegét kiszámításához</span><span class="sxs-lookup"><span data-stu-id="99ddc-182">To calculate the sum of scores of the JSON document</span></span>

    SELECT SUM(scores)
    FROM json_table jt
      lateral view explode(jt.StudentClassCollection.Score) collection as scores;

<span data-ttu-id="99ddc-183">Az előző lekérdezés használ [oldalirányú nézet felbontása](https://cwiki.apache.org/confluence/display/Hive/LanguageManual+LateralView) , hogy ezeket is hozzáadja, bontsa ki a tömb a pontszámok UDF.</span><span class="sxs-lookup"><span data-stu-id="99ddc-183">The preceding query uses [lateral view explode](https://cwiki.apache.org/confluence/display/Hive/LanguageManual+LateralView) UDF to expand the array of scores so that they can be summed.</span></span>

<span data-ttu-id="99ddc-184">Ez a struktúra konzol kimenetét.</span><span class="sxs-lookup"><span data-stu-id="99ddc-184">Here is the output from the Hive console.</span></span>

![SerDe lekérdezés 2][image-hdi-hivejson-serde_query2]

<span data-ttu-id="99ddc-186">Kereséséhez, amelyek egy adott hallgató témák rendelkezik pontozza a mennyiségeket több mint 80 pontok:</span><span class="sxs-lookup"><span data-stu-id="99ddc-186">To find which subjects a given student has scored more than 80 points:</span></span>

    SELECT  
      jt.StudentClassCollection.ClassId
    FROM json_table jt
      lateral view explode(jt.StudentClassCollection.Score) collection as score  where score > 80;

<span data-ttu-id="99ddc-187">Az előző lekérdezés get eltérően a Hive tömböt ad vissza,\_json\_objektum, amely egy karakterláncot ad vissza.</span><span class="sxs-lookup"><span data-stu-id="99ddc-187">The preceding query returns a Hive array unlike get\_json\_object, which returns a string.</span></span>

![SerDe lekérdezés 3][image-hdi-hivejson-serde_query3]

<span data-ttu-id="99ddc-189">Ha azt szeretné, hogy skil nem megfelelően formázott JSON, majd, amint azt a [wiki lapján](https://github.com/sheetaldolas/Hive-JSON-Serde/tree/master) , ez SerDe érhet el, írja be a következő kódot:</span><span class="sxs-lookup"><span data-stu-id="99ddc-189">If you want to skil malformed JSON, then as explained in the [wiki page](https://github.com/sheetaldolas/Hive-JSON-Serde/tree/master) of this SerDe you can achieve that by typing the following code:</span></span>  

    ALTER TABLE json_table SET SERDEPROPERTIES ( "ignore.malformed.json" = "true");




## <a name="summary"></a><span data-ttu-id="99ddc-190">Összefoglalás</span><span class="sxs-lookup"><span data-stu-id="99ddc-190">Summary</span></span>
<span data-ttu-id="99ddc-191">Végezetül a JSON operátor szerepel a Hive, az Ön által függ a forgatókönyvéhez.</span><span class="sxs-lookup"><span data-stu-id="99ddc-191">In conclusion, the type of JSON operator in Hive that you choose depends on your scenario.</span></span> <span data-ttu-id="99ddc-192">Ha egy egyszerű JSON-dokumentum, és csak akkor kell – kereshet egy mezőt is szeretne használni a Hive UDF get\_json\_objektum.</span><span class="sxs-lookup"><span data-stu-id="99ddc-192">If you have a simple JSON document and you only have one field to look up on – you can choose to use the Hive UDF get\_json\_object.</span></span> <span data-ttu-id="99ddc-193">Ha egynél több kulcs keressük meg, majd használhatja json_tuple.</span><span class="sxs-lookup"><span data-stu-id="99ddc-193">If you have more than one key to look up on, then you can use json_tuple.</span></span> <span data-ttu-id="99ddc-194">Ha a beágyazott dokumentumok, akkor a JSON SerDe kell használnia.</span><span class="sxs-lookup"><span data-stu-id="99ddc-194">If you have a nested document, then you should use the JSON SerDe.</span></span>

## <a name="next-steps"></a><span data-ttu-id="99ddc-195">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="99ddc-195">Next steps</span></span>

<span data-ttu-id="99ddc-196">Az egyéb kapcsolódó cikkek lásd:</span><span class="sxs-lookup"><span data-stu-id="99ddc-196">For other related articles, see</span></span>

* [<span data-ttu-id="99ddc-197">Apache log4j mintafájl elemzéséhez a HDInsight Hadoop Hive és a HiveQL használata</span><span class="sxs-lookup"><span data-stu-id="99ddc-197">Use Hive and HiveQL with Hadoop in HDInsight to analyze a sample Apache log4j file</span></span>](hdinsight-use-hive.md)
* [<span data-ttu-id="99ddc-198">Repülési késleltetés adatok elemzése a Hive HDInsight használatával</span><span class="sxs-lookup"><span data-stu-id="99ddc-198">Analyze flight delay data by using Hive in HDInsight</span></span>](hdinsight-analyze-flight-delay-data.md)
* [<span data-ttu-id="99ddc-199">Hdinsight Hive eszközzel Twitter-adatok elemzése</span><span class="sxs-lookup"><span data-stu-id="99ddc-199">Analyze Twitter data using Hive in HDInsight</span></span>](hdinsight-analyze-twitter-data.md)
* [<span data-ttu-id="99ddc-200">Egy Azure Cosmos DB és a HDInsight Hadoop-feladat futtatása</span><span class="sxs-lookup"><span data-stu-id="99ddc-200">Run a Hadoop job using Azure Cosmos DB and HDInsight</span></span>](../documentdb/documentdb-run-hadoop-with-hdinsight.md)

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
