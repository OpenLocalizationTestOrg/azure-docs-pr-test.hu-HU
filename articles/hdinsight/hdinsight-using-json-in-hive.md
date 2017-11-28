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
# <a name="process-and-analyze-json-documents-using-hive-in-hdinsight"></a><span data-ttu-id="de54a-103">Feldolgozhatja és elemezheti a JSON-dokumentumok Hive hdinsight eszközzel</span><span class="sxs-lookup"><span data-stu-id="de54a-103">Process and analyze JSON documents using Hive in HDInsight</span></span>

<span data-ttu-id="de54a-104">Megtudhatja, hogyan tooprocess és elemzése a Hive HDInsight JSON-fájlokat.</span><span class="sxs-lookup"><span data-stu-id="de54a-104">Learn how tooprocess and analyze JSON files using Hive in HDInsight.</span></span> <span data-ttu-id="de54a-105">a következő JSON-dokumentum hello hello oktatóanyag szerepel:</span><span class="sxs-lookup"><span data-stu-id="de54a-105">hello following JSON document is used in hello tutorial:</span></span>

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

<span data-ttu-id="de54a-106">hello fájl található a wasb://processjson@hditutorialdata.blob.core.windows.net/.</span><span class="sxs-lookup"><span data-stu-id="de54a-106">hello file can be found at wasb://processjson@hditutorialdata.blob.core.windows.net/.</span></span> <span data-ttu-id="de54a-107">További információ az Azure Blob storage használatával a hdinsight eszközzel, lásd: [használatát a HDFS-kompatibilis Azure Blob storage hadooppal a Hdinsightban](hdinsight-hadoop-use-blob-storage.md).</span><span class="sxs-lookup"><span data-stu-id="de54a-107">For more information on using Azure Blob storage with HDInsight, see [Use HDFS-compatible Azure Blob storage with Hadoop in HDInsight](hdinsight-hadoop-use-blob-storage.md).</span></span> <span data-ttu-id="de54a-108">Hello fájl toohello alapértelmezett tárolót a fürt másolhatja.</span><span class="sxs-lookup"><span data-stu-id="de54a-108">You can copy hello file toohello default container of your cluster.</span></span>

<span data-ttu-id="de54a-109">Ebben az oktatóanyagban hello Hive konzol használata.</span><span class="sxs-lookup"><span data-stu-id="de54a-109">In this tutorial, you use hello Hive console.</span></span>  <span data-ttu-id="de54a-110">További tájékoztatást a hello Hive konzol megnyitása [használata a távoli asztal hdinsight Hadoop Hive](hdinsight-hadoop-use-hive-remote-desktop.md).</span><span class="sxs-lookup"><span data-stu-id="de54a-110">For instructions of opening hello Hive console, see [Use Hive with Hadoop on HDInsight with Remote Desktop](hdinsight-hadoop-use-hive-remote-desktop.md).</span></span>

## <a name="flatten-json-documents"></a><span data-ttu-id="de54a-111">JSON-dokumentumok egybesimítására</span><span class="sxs-lookup"><span data-stu-id="de54a-111">Flatten JSON documents</span></span>
<span data-ttu-id="de54a-112">hello hello a következő szakaszban felsorolt módszerekhez hello JSON-dokumentum egy sorban.</span><span class="sxs-lookup"><span data-stu-id="de54a-112">hello methods listed in hello next section require hello JSON document in a single row.</span></span> <span data-ttu-id="de54a-113">Ezért kell egybesimítására hello JSON-dokumentum tooa karakterlánc.</span><span class="sxs-lookup"><span data-stu-id="de54a-113">So you must flatten hello JSON document tooa string.</span></span> <span data-ttu-id="de54a-114">Ha már egybesimított-e a JSON-dokumentumában, kihagyhatja ezt a lépést, és folytassa egyenes toohello a következő szakasz elemzése JSON-adatokat.</span><span class="sxs-lookup"><span data-stu-id="de54a-114">If your JSON document is already flattened, you can skip this step and go straight toohello next section on Analyzing JSON data.</span></span>

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

<span data-ttu-id="de54a-115">hello nyers JSON-fájl itt található:  **wasb://processjson@hditutorialdata.blob.core.windows.net/** .</span><span class="sxs-lookup"><span data-stu-id="de54a-115">hello raw JSON file is located at **wasb://processjson@hditutorialdata.blob.core.windows.net/**.</span></span> <span data-ttu-id="de54a-116">Hello *StudentsRaw* Hive tábla toohello nyers áttetsző JSON-dokumentum mutat.</span><span class="sxs-lookup"><span data-stu-id="de54a-116">hello *StudentsRaw* Hive table points toohello raw unflattened JSON document.</span></span>

<span data-ttu-id="de54a-117">Hello *StudentsOneLine* Hive tábla hello adatot tárol a HDInsight alapértelmezett fájlrendszer alatt hello hello */json/diákok/* elérési útja.</span><span class="sxs-lookup"><span data-stu-id="de54a-117">hello *StudentsOneLine* Hive table stores hello data in hello HDInsight default file system under hello */json/students/* path.</span></span>

<span data-ttu-id="de54a-118">hello INSERT utasítás egybesimított hello JSON-adatokat tartalmazó hello StudentOneLine táblát tölti fel.</span><span class="sxs-lookup"><span data-stu-id="de54a-118">hello INSERT statement populates hello StudentOneLine table with hello flattened JSON data.</span></span>

<span data-ttu-id="de54a-119">hello SELECT utasítás csak kell egy olyan sor visszaadása.</span><span class="sxs-lookup"><span data-stu-id="de54a-119">hello SELECT statement shall only return one row.</span></span>

<span data-ttu-id="de54a-120">Hello SELECT utasítás hello kimenete a következő:</span><span class="sxs-lookup"><span data-stu-id="de54a-120">Here is hello output of hello SELECT statement:</span></span>

![Egybesimítását hello JSON-dokumentum.][image-hdi-hivejson-flatten]

## <a name="analyze-json-documents-in-hive"></a><span data-ttu-id="de54a-122">A Hive JSON-dokumentumok elemzése</span><span class="sxs-lookup"><span data-stu-id="de54a-122">Analyze JSON documents in Hive</span></span>
<span data-ttu-id="de54a-123">Hive toorun lekérdezések Ez a három különböző mechanizmusok JSON-dokumentumokat:</span><span class="sxs-lookup"><span data-stu-id="de54a-123">Hive provides three different mechanisms toorun queries on JSON documents:</span></span>

* <span data-ttu-id="de54a-124">használja a hello GET\_JSON\_objektum UDF (felhasználó által definiált függvény)</span><span class="sxs-lookup"><span data-stu-id="de54a-124">use hello GET\_JSON\_OBJECT UDF (User-defined function)</span></span>
* <span data-ttu-id="de54a-125">hello JSON_TUPLE UDF használata</span><span class="sxs-lookup"><span data-stu-id="de54a-125">use hello JSON_TUPLE UDF</span></span>
* <span data-ttu-id="de54a-126">Egyéni SerDe használata</span><span class="sxs-lookup"><span data-stu-id="de54a-126">use custom SerDe</span></span>
* <span data-ttu-id="de54a-127">írás a Python vagy egyéb nyelvek UDF saját.</span><span class="sxs-lookup"><span data-stu-id="de54a-127">write you own UDF using Python or other languages.</span></span> <span data-ttu-id="de54a-128">Lásd: [Ez a cikk] [ hdinsight-python] a Hive és a Python kód fut.</span><span class="sxs-lookup"><span data-stu-id="de54a-128">See [this article][hdinsight-python] on running your own Python code with Hive.</span></span>

### <a name="use-hello-getjsonobject-udf"></a><span data-ttu-id="de54a-129">Használjon hello GET\_JSON_OBJECT UDF-ben</span><span class="sxs-lookup"><span data-stu-id="de54a-129">Use hello GET\_JSON_OBJECT UDF</span></span>
<span data-ttu-id="de54a-130">Hive biztosít egy beépített UDF nevű [json-objektum](https://cwiki.apache.org/confluence/display/Hive/LanguageManual+UDF#LanguageManualUDF-get_json_object), amely JSON lekérdezése futásidőben hajthat végre.</span><span class="sxs-lookup"><span data-stu-id="de54a-130">Hive provides a built-in UDF called [get json object](https://cwiki.apache.org/confluence/display/Hive/LanguageManual+UDF#LanguageManualUDF-get_json_object), which can perform JSON querying during run time.</span></span> <span data-ttu-id="de54a-131">Ez a módszer két argumentummal – hello táblanév és metódus neve, amelynek hello egybesimított JSON dokumentum és hello JSON mező, amelyet a toobe értelmezni.</span><span class="sxs-lookup"><span data-stu-id="de54a-131">This method takes two arguments – hello table name and method name, which has hello flattened JSON document and hello JSON field that needs toobe parsed.</span></span> <span data-ttu-id="de54a-132">Egy példa toosee az UDF működése vizsgáljuk meg.</span><span class="sxs-lookup"><span data-stu-id="de54a-132">Let’s look at an example toosee how this UDF works.</span></span>

<span data-ttu-id="de54a-133">Hello utó- és vezetéknevet minden student beolvasása</span><span class="sxs-lookup"><span data-stu-id="de54a-133">Get hello first name and last name for each student</span></span>

    SELECT
      GET_JSON_OBJECT(StudentsOneLine.json_body,'$.StudentDetails.FirstName'),
      GET_JSON_OBJECT(StudentsOneLine.json_body,'$.StudentDetails.LastName')
    FROM StudentsOneLine;

<span data-ttu-id="de54a-134">Itt egy hello kimenet, amikor ez a lekérdezés futtatása a konzolablakban.</span><span class="sxs-lookup"><span data-stu-id="de54a-134">Here is hello output when running this query in console window.</span></span>

![get_json_object UDF-ben][image-hdi-hivejson-getjsonobject]

<span data-ttu-id="de54a-136">Néhány korlátozás hello get-json_object az UDF-ben.</span><span class="sxs-lookup"><span data-stu-id="de54a-136">There are a few limitations of hello get-json_object UDF.</span></span>

* <span data-ttu-id="de54a-137">Egyes mezők hello lekérdezés használatához hello lekérdezés reparsing, hello teljesítmény hatással van.</span><span class="sxs-lookup"><span data-stu-id="de54a-137">Because each field in hello query requires reparsing hello query, it affects hello performance.</span></span>
* <span data-ttu-id="de54a-138">ELSŐ\_JSON_OBJECT() hello karakterláncos ábrázolása tömbjét adja vissza.</span><span class="sxs-lookup"><span data-stu-id="de54a-138">GET\_JSON_OBJECT() returns hello string representation of an array.</span></span> <span data-ttu-id="de54a-139">tooconvert a tömb tooa Hive tömb rendelkezik toouse reguláris kifejezések tooreplace szögletes zárójelek hello ' ["és"] ", és ezután is hívás osztani tooget hello tömb.</span><span class="sxs-lookup"><span data-stu-id="de54a-139">tooconvert this array tooa Hive array, you have toouse regular expressions tooreplace hello square brackets ‘[‘ and ‘]’ and then also call split tooget hello array.</span></span>

<span data-ttu-id="de54a-140">Ezért hello Hive wiki a json_tuple használatát javasolja.</span><span class="sxs-lookup"><span data-stu-id="de54a-140">This is why hello Hive wiki recommends using json_tuple.</span></span>  

### <a name="use-hello-jsontuple-udf"></a><span data-ttu-id="de54a-141">Hello JSON_TUPLE UDF használata</span><span class="sxs-lookup"><span data-stu-id="de54a-141">Use hello JSON_TUPLE UDF</span></span>
<span data-ttu-id="de54a-142">Egy másik UDF Hive által biztosított nevezik [json_tuple](https://cwiki.apache.org/confluence/display/Hive/LanguageManual+UDF#LanguageManualUDF-json_tuple), melyik teljesít jobban, mint [get_ json _object](https://cwiki.apache.org/confluence/display/Hive/LanguageManual+UDF#LanguageManualUDF-get_json_object).</span><span class="sxs-lookup"><span data-stu-id="de54a-142">Another UDF provided by Hive is called [json_tuple](https://cwiki.apache.org/confluence/display/Hive/LanguageManual+UDF#LanguageManualUDF-json_tuple), which performs better than [get_ json _object](https://cwiki.apache.org/confluence/display/Hive/LanguageManual+UDF#LanguageManualUDF-get_json_object).</span></span> <span data-ttu-id="de54a-143">Ez a módszer kulcsok és a JSON karakterláncnak vesz igénybe, és egy függvény használatával értékek egy rekordot ad vissza.</span><span class="sxs-lookup"><span data-stu-id="de54a-143">This method takes a set of keys and a JSON string, and returns a tuple of values using one function.</span></span> <span data-ttu-id="de54a-144">hello következő lekérdezés adja vissza hello student azonosítója és hello osztályú hello JSON-dokumentum:</span><span class="sxs-lookup"><span data-stu-id="de54a-144">hello following query returns hello student id and hello grade from hello JSON document:</span></span>

    SELECT q1.StudentId, q1.Grade
      FROM StudentsOneLine jt
      LATERAL VIEW JSON_TUPLE(jt.json_body, 'StudentId', 'Grade') q1
        AS StudentId, Grade;

<span data-ttu-id="de54a-145">a parancsfájl hello Hive konzolon hello kimenete:</span><span class="sxs-lookup"><span data-stu-id="de54a-145">hello output of this script in hello Hive console:</span></span>

![json_tuple UDF-ben][image-hdi-hivejson-jsontuple]

<span data-ttu-id="de54a-147">JSON\_REKORDOT használ hello [nézet oldalirányú](https://cwiki.apache.org/confluence/display/Hive/LanguageManual+LateralView) struktúra, amely lehetővé teszi a json szintaxist\_rekordot toocreate hello UDT függvény tooeach hello eredeti táblázat sorában alkalmazásával virtuális tábla.</span><span class="sxs-lookup"><span data-stu-id="de54a-147">JSON\_TUPLE uses hello [lateral view](https://cwiki.apache.org/confluence/display/Hive/LanguageManual+LateralView) syntax in Hive, which allows json\_tuple toocreate a virtual table by applying hello UDT function tooeach row of hello original table.</span></span>  <span data-ttu-id="de54a-148">Ismétlődő hello miatt túl kezelése nehézkessé válik összetett JSONs OLDALIRÁNYÚ nézet használja.</span><span class="sxs-lookup"><span data-stu-id="de54a-148">Complex JSONs become too unwieldy because of hello repeated use of LATERAL VIEW.</span></span> <span data-ttu-id="de54a-149">Ezenkívül JSON_TUPLE beágyazott JSONs nem tudja kezelni.</span><span class="sxs-lookup"><span data-stu-id="de54a-149">Furthermore, JSON_TUPLE cannot handle nested JSONs.</span></span>

### <a name="use-custom-serde"></a><span data-ttu-id="de54a-150">Egyéni SerDe használata</span><span class="sxs-lookup"><span data-stu-id="de54a-150">Use custom SerDe</span></span>
<span data-ttu-id="de54a-151">SerDe hello legjobb választás az elemzés beágyazott JSON-dokumentumokat, lehetővé teszi toodefine hello JSON-séma, és hello séma tooparse hello dokumentumokhoz.</span><span class="sxs-lookup"><span data-stu-id="de54a-151">SerDe is hello best choice for parsing nested JSON documents, it allows you toodefine hello JSON schema, and use hello schema tooparse hello documents.</span></span> <span data-ttu-id="de54a-152">Ebben az oktatóanyagban valamelyikét használja hello által kidolgozott népszerűbbnek SerDe [Roberto Congiu](https://github.com/rcongiu).</span><span class="sxs-lookup"><span data-stu-id="de54a-152">In this tutorial, you use one of hello more popular SerDe that has been developed by [Roberto Congiu](https://github.com/rcongiu).</span></span>

<span data-ttu-id="de54a-153">**toouse egyéni SerDe hello:**</span><span class="sxs-lookup"><span data-stu-id="de54a-153">**toouse hello custom SerDe:**</span></span>

1. <span data-ttu-id="de54a-154">Telepítés [Java SE Development Kit 7u55 JDK 1.7.0_55](http://www.oracle.com/technetwork/java/javase/downloads/java-archive-downloads-javase7-521261.html#jdk-7u55-oth-JPR).</span><span class="sxs-lookup"><span data-stu-id="de54a-154">Install [Java SE Development Kit 7u55 JDK 1.7.0_55](http://www.oracle.com/technetwork/java/javase/downloads/java-archive-downloads-javase7-521261.html#jdk-7u55-oth-JPR).</span></span> <span data-ttu-id="de54a-155">Válassza a hello JDK hello Windows X64 verziót, ha toobe hello központi Windows-telepítési a HDInsight használatával fog</span><span class="sxs-lookup"><span data-stu-id="de54a-155">Choose hello Windows X64 version of hello JDK if you are going toobe using hello Windows deployment of HDInsight</span></span>
   
   > [!WARNING]
   > <span data-ttu-id="de54a-156">A SerDe JDK 1.8 nem működik.</span><span class="sxs-lookup"><span data-stu-id="de54a-156">JDK 1.8 doesn't work with this SerDe.</span></span>
   > 
   > 
   
    <span data-ttu-id="de54a-157">Hello telepítés befejezése után adjon hozzá egy új felhasználó környezeti változó:</span><span class="sxs-lookup"><span data-stu-id="de54a-157">After hello installation is completed, add a new user environment variable:</span></span>
   
   1. <span data-ttu-id="de54a-158">Nyissa meg **nézet Speciális rendszerbeállítások** hello Windows képernyőről.</span><span class="sxs-lookup"><span data-stu-id="de54a-158">Open **View advanced system settings** from hello Windows screen.</span></span>
   2. <span data-ttu-id="de54a-159">Kattintson a **környezeti változók**.</span><span class="sxs-lookup"><span data-stu-id="de54a-159">Click **Environment Variables**.</span></span>  
   3. <span data-ttu-id="de54a-160">Adjon hozzá egy új **JAVA_HOME** környezeti változó túl mutat**C:\Program Files\Java\jdk1.7.0_55** vagy a JDK telepítve legyen.</span><span class="sxs-lookup"><span data-stu-id="de54a-160">Add a new **JAVA_HOME** environment variable is pointing too**C:\Program Files\Java\jdk1.7.0_55** or wherever your JDK is installed.</span></span>
      
      ![JDK megfelelő konfigurációs értékeire beállítása][image-hdi-hivejson-jdk]
2. <span data-ttu-id="de54a-162">Telepítés [Maven 3.3.1](http://mirror.olnevhost.net/pub/apache/maven/maven-3/3.3.1/binaries/apache-maven-3.3.1-bin.zip)</span><span class="sxs-lookup"><span data-stu-id="de54a-162">Install [Maven 3.3.1](http://mirror.olnevhost.net/pub/apache/maven/maven-3/3.3.1/binaries/apache-maven-3.3.1-bin.zip)</span></span>
   
    <span data-ttu-id="de54a-163">Hello bin mappa tooyour elérési út megnyitásával tooControl Panel--> Szerkesztés hello rendszerváltozók a fiók környezeti változók.</span><span class="sxs-lookup"><span data-stu-id="de54a-163">Add hello bin folder tooyour path by going tooControl Panel-->Edit hello System Variables for your account Environment variables.</span></span> <span data-ttu-id="de54a-164">hello alábbi képernyőfelvételen bemutatja, hogyan toodo ez.</span><span class="sxs-lookup"><span data-stu-id="de54a-164">hello following screenshot shows you how toodo this.</span></span>
   
    ![Maven beállítása][image-hdi-hivejson-maven]
3. <span data-ttu-id="de54a-166">A Klónozás hello projekt [Hive-JSON-SerDe](https://github.com/sheetaldolas/Hive-JSON-Serde/tree/master) github webhelyen.</span><span class="sxs-lookup"><span data-stu-id="de54a-166">Clone hello project from [Hive-JSON-SerDe](https://github.com/sheetaldolas/Hive-JSON-Serde/tree/master) github site.</span></span> <span data-ttu-id="de54a-167">Ez látható módon a következő képernyőkép hello hello "Töltse le a Zip" gombra kattintva teheti meg.</span><span class="sxs-lookup"><span data-stu-id="de54a-167">You can do this by clicking on hello “Download Zip” button as shown in hello following screenshot.</span></span>
   
    ![A Klónozás hello projekt][image-hdi-hivejson-serde]

<span data-ttu-id="de54a-169">4: Nyissa meg toohello mappát, ahova a csomagot, és a "mvn csomag" típusú letöltötte rendelkezik.</span><span class="sxs-lookup"><span data-stu-id="de54a-169">4: Go toohello folder where you have downloaded this package and then type “mvn package”.</span></span> <span data-ttu-id="de54a-170">Ez kell hello szükséges jar-fájlok létrehozása, amely ezután másolhatja toohello fürt keresztül.</span><span class="sxs-lookup"><span data-stu-id="de54a-170">This should create hello necessary jar files that you can then copy over toohello cluster.</span></span>

<span data-ttu-id="de54a-171">5: Ugrás toohello célmappa hello csomag kezelőportálon hello gyökérmappájában.</span><span class="sxs-lookup"><span data-stu-id="de54a-171">5: Go toohello target folder under hello root folder where you downloaded hello package.</span></span> <span data-ttu-id="de54a-172">Töltse fel a hello json-serde-1.1.9.9-Hive13-jar-with-dependencies.jar fájl toohead csomópontos a fürt.</span><span class="sxs-lookup"><span data-stu-id="de54a-172">Upload hello json-serde-1.1.9.9-Hive13-jar-with-dependencies.jar file toohead-node of your cluster.</span></span> <span data-ttu-id="de54a-173">I általában használatba hello hive bináris mappában: C:\apps\dist\hive-0.13.0.2.1.11.0-2316\bin vagy más hasonló.</span><span class="sxs-lookup"><span data-stu-id="de54a-173">I usually put it under hello hive binary folder: C:\apps\dist\hive-0.13.0.2.1.11.0-2316\bin or something similar.</span></span>

<span data-ttu-id="de54a-174">6: hello hive parancssorba írja be a "Hozzáadás jar /path/to/json-serde-1.1.9.9-Hive13-jar-with-dependencies.jar".</span><span class="sxs-lookup"><span data-stu-id="de54a-174">6: In hello hive prompt, type “add jar /path/to/json-serde-1.1.9.9-Hive13-jar-with-dependencies.jar”.</span></span> <span data-ttu-id="de54a-175">Mivel abban az esetben hello jar hello C:\apps\dist\hive-0.13.x\bin mappában, hello jar hello neve, ahogy a közvetlenül hozzáadok is:</span><span class="sxs-lookup"><span data-stu-id="de54a-175">Since in my case, hello jar is in hello C:\apps\dist\hive-0.13.x\bin folder, I can directly add hello jar with hello name as shown:</span></span>

    add jar json-serde-1.1.9.9-Hive13-jar-with-dependencies.jar;

   ![JAR tooyour projekt hozzáadása][image-hdi-hivejson-addjar]

<span data-ttu-id="de54a-177">Most már áll készen toouse hello SerDe toorun lekérdezések hello JSON-dokumentum.</span><span class="sxs-lookup"><span data-stu-id="de54a-177">Now, you are ready toouse hello SerDe toorun queries against hello JSON document.</span></span>

<span data-ttu-id="de54a-178">a következő utasítás hello táblázatot hoz létre a megadott séma:</span><span class="sxs-lookup"><span data-stu-id="de54a-178">hello following statement creates a table with a defined schema:</span></span>

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

<span data-ttu-id="de54a-179">hello utó- és vezetékneve hello student toolist</span><span class="sxs-lookup"><span data-stu-id="de54a-179">toolist hello first name and last name of hello student</span></span>

    SELECT StudentDetails.FirstName, StudentDetails.LastName FROM json_table;

<span data-ttu-id="de54a-180">Itt található hello eredmény hello Hive konzolról.</span><span class="sxs-lookup"><span data-stu-id="de54a-180">Here is hello result from hello Hive console.</span></span>

![1 SerDe lekérdezés][image-hdi-hivejson-serde_query1]

<span data-ttu-id="de54a-182">pontszámok hello JSON-dokumentum toocalculate hello összege</span><span class="sxs-lookup"><span data-stu-id="de54a-182">toocalculate hello sum of scores of hello JSON document</span></span>

    SELECT SUM(scores)
    FROM json_table jt
      lateral view explode(jt.StudentClassCollection.Score) collection as scores;

<span data-ttu-id="de54a-183">lekérdezés használ megelőző hello [oldalirányú nézet felbontása](https://cwiki.apache.org/confluence/display/Hive/LanguageManual+LateralView) UDF tooexpand hello pontszámok tömbje, így ezeket hozzáadja.</span><span class="sxs-lookup"><span data-stu-id="de54a-183">hello preceding query uses [lateral view explode](https://cwiki.apache.org/confluence/display/Hive/LanguageManual+LateralView) UDF tooexpand hello array of scores so that they can be summed.</span></span>

<span data-ttu-id="de54a-184">Itt egy hello kimenet hello Hive konzolról.</span><span class="sxs-lookup"><span data-stu-id="de54a-184">Here is hello output from hello Hive console.</span></span>

![SerDe lekérdezés 2][image-hdi-hivejson-serde_query2]

<span data-ttu-id="de54a-186">toofind, amely egy adott hallgató témák rendelkezik program pontozza a mennyiségeket több mint 80 pontok:</span><span class="sxs-lookup"><span data-stu-id="de54a-186">toofind which subjects a given student has scored more than 80 points:</span></span>

    SELECT  
      jt.StudentClassCollection.ClassId
    FROM json_table jt
      lateral view explode(jt.StudentClassCollection.Score) collection as score  where score > 80;

<span data-ttu-id="de54a-187">hello előző lekérdezés tömböt ad vissza, a Hive eltérően get\_json\_objektum, amely egy karakterláncot ad vissza.</span><span class="sxs-lookup"><span data-stu-id="de54a-187">hello preceding query returns a Hive array unlike get\_json\_object, which returns a string.</span></span>

![SerDe lekérdezés 3][image-hdi-hivejson-serde_query3]

<span data-ttu-id="de54a-189">Ha azt szeretné, hogy tooskil nem megfelelően formázott JSON, majd azt hello mint [wiki lapján](https://github.com/sheetaldolas/Hive-JSON-Serde/tree/master) , ez SerDe érhet el, írja be a kódját a következő hello:</span><span class="sxs-lookup"><span data-stu-id="de54a-189">If you want tooskil malformed JSON, then as explained in hello [wiki page](https://github.com/sheetaldolas/Hive-JSON-Serde/tree/master) of this SerDe you can achieve that by typing hello following code:</span></span>  

    ALTER TABLE json_table SET SERDEPROPERTIES ( "ignore.malformed.json" = "true");




## <a name="summary"></a><span data-ttu-id="de54a-190">Összefoglalás</span><span class="sxs-lookup"><span data-stu-id="de54a-190">Summary</span></span>
<span data-ttu-id="de54a-191">Végezetül hello JSON operátor a struktúrát, amelyben a forgatókönyvtől függ.</span><span class="sxs-lookup"><span data-stu-id="de54a-191">In conclusion, hello type of JSON operator in Hive that you choose depends on your scenario.</span></span> <span data-ttu-id="de54a-192">Ha egy egyszerű JSON-dokumentum, és csak akkor kell egy mező toolook – választhat toouse hello Hive UDF get\_json\_objektum.</span><span class="sxs-lookup"><span data-stu-id="de54a-192">If you have a simple JSON document and you only have one field toolook up on – you can choose toouse hello Hive UDF get\_json\_object.</span></span> <span data-ttu-id="de54a-193">Ha egynél több kulcsfontosságú toolook, json_tuple használhatja.</span><span class="sxs-lookup"><span data-stu-id="de54a-193">If you have more than one key toolook up on, then you can use json_tuple.</span></span> <span data-ttu-id="de54a-194">Ha a beágyazott dokumentumok, akkor hello JSON SerDe kell használnia.</span><span class="sxs-lookup"><span data-stu-id="de54a-194">If you have a nested document, then you should use hello JSON SerDe.</span></span>

## <a name="next-steps"></a><span data-ttu-id="de54a-195">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="de54a-195">Next steps</span></span>

<span data-ttu-id="de54a-196">Az egyéb kapcsolódó cikkek lásd:</span><span class="sxs-lookup"><span data-stu-id="de54a-196">For other related articles, see</span></span>

* [<span data-ttu-id="de54a-197">A HDInsight tooanalyze Apache log4j mintafájl Hadoop Hive és a HiveQL használata</span><span class="sxs-lookup"><span data-stu-id="de54a-197">Use Hive and HiveQL with Hadoop in HDInsight tooanalyze a sample Apache log4j file</span></span>](hdinsight-use-hive.md)
* [<span data-ttu-id="de54a-198">Repülési késleltetés adatok elemzése a Hive HDInsight használatával</span><span class="sxs-lookup"><span data-stu-id="de54a-198">Analyze flight delay data by using Hive in HDInsight</span></span>](hdinsight-analyze-flight-delay-data.md)
* [<span data-ttu-id="de54a-199">Hdinsight Hive eszközzel Twitter-adatok elemzése</span><span class="sxs-lookup"><span data-stu-id="de54a-199">Analyze Twitter data using Hive in HDInsight</span></span>](hdinsight-analyze-twitter-data.md)
* [<span data-ttu-id="de54a-200">Egy Azure Cosmos DB és a HDInsight Hadoop-feladat futtatása</span><span class="sxs-lookup"><span data-stu-id="de54a-200">Run a Hadoop job using Azure Cosmos DB and HDInsight</span></span>](../documentdb/documentdb-run-hadoop-with-hdinsight.md)

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
