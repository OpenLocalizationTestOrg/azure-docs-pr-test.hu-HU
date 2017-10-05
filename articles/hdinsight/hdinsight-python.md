---
title: "Python UDF az Apache Hive és a sertésfelmérés – az Azure HDInsight |} Microsoft Docs"
description: "Ismerje meg, Python felhasználó definiált függvény (UDF) a Hive és a Pig használata a Hdinsightban, a Hadoop technológiai területekre az Azure-on."
services: hdinsight
documentationcenter: 
author: Blackmist
manager: jhubbard
editor: cgronlun
tags: azure-portal
ms.assetid: c44d6606-28cd-429b-b535-235e8f34a664
ms.service: hdinsight
ms.workload: big-data
ms.tgt_pltfrm: na
ms.devlang: python
ms.topic: article
ms.date: 07/17/2017
ms.author: larryfr
ms.custom: H1Hack27Feb2017,hdinsightactive
ms.openlocfilehash: 9b67ded05a52f1e68580434667495cf6cf939871
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/03/2017
---
# <a name="use-python-user-defined-functions-udf-with-hive-and-pig-in-hdinsight"></a><span data-ttu-id="ebe25-103">Használjon Python felhasználói függvény (UDF), a Hive és a Pig a Hdinsightban</span><span class="sxs-lookup"><span data-stu-id="ebe25-103">Use Python User Defined Functions (UDF) with Hive and Pig in HDInsight</span></span>

<span data-ttu-id="ebe25-104">Útmutató Apache Hive és az Azure hdinsight Hadoop Pig Python felhasználói függvény (UDF) használhat.</span><span class="sxs-lookup"><span data-stu-id="ebe25-104">Learn how to use Python user-defined functions (UDF) with Apache Hive and Pig in Hadoop on Azure HDInsight.</span></span>

## <span data-ttu-id="ebe25-105"><a name="python"></a>A HDInsight Python</span><span class="sxs-lookup"><span data-stu-id="ebe25-105"><a name="python"></a>Python on HDInsight</span></span>

<span data-ttu-id="ebe25-106">A HDInsight 3.0-s és újabb verziók alapértelmezés szerint telepítve van a Python2.7.</span><span class="sxs-lookup"><span data-stu-id="ebe25-106">Python2.7 is installed by default on HDInsight 3.0 and later.</span></span> <span data-ttu-id="ebe25-107">Apache Hive használható ebben a Python adatfolyam feldolgozásra.</span><span class="sxs-lookup"><span data-stu-id="ebe25-107">Apache Hive can be used with this version of Python for stream processing.</span></span> <span data-ttu-id="ebe25-108">Adatfolyam feldolgozása mechanizmusok adatok átadására a Hive és a UDF között használja az STDOUT és STDIN.</span><span class="sxs-lookup"><span data-stu-id="ebe25-108">Stream processing uses STDOUT and STDIN to pass data between Hive and the UDF.</span></span>

<span data-ttu-id="ebe25-109">HDInsight is Jython, amely a Java nyelven írt Python megvalósítása.</span><span class="sxs-lookup"><span data-stu-id="ebe25-109">HDInsight also includes Jython, which is a Python implementation written in Java.</span></span> <span data-ttu-id="ebe25-110">Jython közvetlenül a Java virtuálisgép fut, és nem használja a streaming.</span><span class="sxs-lookup"><span data-stu-id="ebe25-110">Jython runs directly on the Java Virtual Machine and does not use streaming.</span></span> <span data-ttu-id="ebe25-111">Jython a javasolt Python értelmező esetén a Pig használata a Python.</span><span class="sxs-lookup"><span data-stu-id="ebe25-111">Jython is the recommended Python interpreter when using Python with Pig.</span></span>

> [!WARNING]
> <span data-ttu-id="ebe25-112">A jelen dokumentumban leírt lépések hajtsa végre a következő előfeltételek:</span><span class="sxs-lookup"><span data-stu-id="ebe25-112">The steps in this document make the following assumptions:</span></span> 
>
> * <span data-ttu-id="ebe25-113">A helyi fejlesztési környezet a Python parancsfájlok hoz létre.</span><span class="sxs-lookup"><span data-stu-id="ebe25-113">You create the Python scripts on your local development environment.</span></span>
> * <span data-ttu-id="ebe25-114">A parancsfájlok HDInsight segítségével töltse fel a `scp` parancsot a helyi Bash munkamenet, vagy a megadott PowerShell-parancsfájlt.</span><span class="sxs-lookup"><span data-stu-id="ebe25-114">You upload the scripts to HDInsight using either the `scp` command from a local Bash session or the provided PowerShell script.</span></span>
>
> <span data-ttu-id="ebe25-115">Ha szeretné használni a [Azure Cloud rendszerhéj (bash)](https://docs.microsoft.com/azure/cloud-shell/overview) preview működéséhez a hdinsight eszközzel, majd be kell:</span><span class="sxs-lookup"><span data-stu-id="ebe25-115">If you want to use the [Azure Cloud Shell (bash)](https://docs.microsoft.com/azure/cloud-shell/overview) preview to work with HDInsight, then you must:</span></span>
>
> * <span data-ttu-id="ebe25-116">Hozzon létre a parancsfájlok a felhő rendszerhéj környezeten belül.</span><span class="sxs-lookup"><span data-stu-id="ebe25-116">Create the scripts inside the cloud shell environment.</span></span>
> * <span data-ttu-id="ebe25-117">Használjon `scp` HDInsight a felhő rendszerhéjból a fájlok feltöltéséhez.</span><span class="sxs-lookup"><span data-stu-id="ebe25-117">Use `scp` to upload the files from the cloud shell to HDInsight.</span></span>
> * <span data-ttu-id="ebe25-118">Használjon `ssh` a felhő rendszerhéjból HDInsight csatlakozni, és futtassa a példákat.</span><span class="sxs-lookup"><span data-stu-id="ebe25-118">Use `ssh` from the cloud shell to connect to HDInsight and run the examples.</span></span>

## <span data-ttu-id="ebe25-119"><a name="hivepython"></a>Hive UDF-ben</span><span class="sxs-lookup"><span data-stu-id="ebe25-119"><a name="hivepython"></a>Hive UDF</span></span>

<span data-ttu-id="ebe25-120">Python változtatás nélkül használhatók a HiveQL a Hive a egy UDF `TRANSFORM` utasítást.</span><span class="sxs-lookup"><span data-stu-id="ebe25-120">Python can be used as a UDF from Hive through the HiveQL `TRANSFORM` statement.</span></span> <span data-ttu-id="ebe25-121">Például a következő HiveQL meghívja a `hiveudf.py` a fürt az alapértelmezett Azure Storage-fiókban tárolt fájl.</span><span class="sxs-lookup"><span data-stu-id="ebe25-121">For example, the following HiveQL invokes the `hiveudf.py` file stored in the default Azure Storage account for the cluster.</span></span>

<span data-ttu-id="ebe25-122">**Linux-alapú HDInsight**</span><span class="sxs-lookup"><span data-stu-id="ebe25-122">**Linux-based HDInsight**</span></span>

```hiveql
add file wasb:///hiveudf.py;

SELECT TRANSFORM (clientid, devicemake, devicemodel)
    USING 'python hiveudf.py' AS
    (clientid string, phoneLable string, phoneHash string)
FROM hivesampletable
ORDER BY clientid LIMIT 50;
```

<span data-ttu-id="ebe25-123">**Windows-alapú HDInsight**</span><span class="sxs-lookup"><span data-stu-id="ebe25-123">**Windows-based HDInsight**</span></span>

```hiveql
add file wasb:///hiveudf.py;

SELECT TRANSFORM (clientid, devicemake, devicemodel)
    USING 'D:\Python27\python.exe hiveudf.py' AS
    (clientid string, phoneLable string, phoneHash string)
FROM hivesampletable
ORDER BY clientid LIMIT 50;
```

> [!NOTE]
> <span data-ttu-id="ebe25-124">Windows-alapú HDInsight-fürtök a `USING` záradék python.exe teljes elérési útja kell megadnia.</span><span class="sxs-lookup"><span data-stu-id="ebe25-124">On Windows-based HDInsight clusters, the `USING` clause must specify the full path to python.exe.</span></span>

<span data-ttu-id="ebe25-125">Ez a példa funkciója:</span><span class="sxs-lookup"><span data-stu-id="ebe25-125">Here's what this example does:</span></span>

1. <span data-ttu-id="ebe25-126">A `add file` a fájl elején utasítás hozzáadja a `hiveudf.py` az elosztott gyorsítótáras, így a fürt összes csomópontja által elérhető fájlt.</span><span class="sxs-lookup"><span data-stu-id="ebe25-126">The `add file` statement at the beginning of the file adds the `hiveudf.py` file to the distributed cache, so it's accessible by all nodes in the cluster.</span></span>
2. <span data-ttu-id="ebe25-127">A `SELECT TRANSFORM ... USING` utasítás kiválasztja az adatokat a `hivesampletable`.</span><span class="sxs-lookup"><span data-stu-id="ebe25-127">The `SELECT TRANSFORM ... USING` statement selects data from the `hivesampletable`.</span></span> <span data-ttu-id="ebe25-128">A clientid, devicemake és devicemodel értékeket is átadja a `hiveudf.py` parancsfájl.</span><span class="sxs-lookup"><span data-stu-id="ebe25-128">It also passes the clientid, devicemake, and devicemodel values to the `hiveudf.py` script.</span></span>
3. <span data-ttu-id="ebe25-129">A `AS` záradék által visszaadott mezőket ismerteti `hiveudf.py`.</span><span class="sxs-lookup"><span data-stu-id="ebe25-129">The `AS` clause describes the fields returned from `hiveudf.py`.</span></span>

<a name="streamingpy"></a>

### <a name="create-the-hiveudfpy-file"></a><span data-ttu-id="ebe25-130">A hiveudf.py fájl létrehozása</span><span class="sxs-lookup"><span data-stu-id="ebe25-130">Create the hiveudf.py file</span></span>


<span data-ttu-id="ebe25-131">A fejlesztési környezetet hozzon létre egy szövegfájlt nevű `hiveudf.py`.</span><span class="sxs-lookup"><span data-stu-id="ebe25-131">On your development environment, create a text file named `hiveudf.py`.</span></span> <span data-ttu-id="ebe25-132">A fájl tartalmát az alábbira használata:</span><span class="sxs-lookup"><span data-stu-id="ebe25-132">Use the following code as the contents of the file:</span></span>

```python
#!/usr/bin/env python
import sys
import string
import hashlib

while True:
    line = sys.stdin.readline()
    if not line:
        break

    line = string.strip(line, "\n ")
    clientid, devicemake, devicemodel = string.split(line, "\t")
    phone_label = devicemake + ' ' + devicemodel
    print "\t".join([clientid, phone_label, hashlib.md5(phone_label).hexdigest()])
```

<span data-ttu-id="ebe25-133">Ezt a parancsfájlt a következő műveleteket hajtja végre:</span><span class="sxs-lookup"><span data-stu-id="ebe25-133">This script performs the following actions:</span></span>

1. <span data-ttu-id="ebe25-134">Egy adatsort STDIN olvasni.</span><span class="sxs-lookup"><span data-stu-id="ebe25-134">Read a line of data from STDIN.</span></span>
2. <span data-ttu-id="ebe25-135">A záró soremelés karaktert segítségével távolítja el `string.strip(line, "\n ")`.</span><span class="sxs-lookup"><span data-stu-id="ebe25-135">The trailing newline character is removed using `string.strip(line, "\n ")`.</span></span>
3. <span data-ttu-id="ebe25-136">Az adatfolyam feldolgozása során egy sorba tabulátor minden érték közötti minden a értékeket tartalmaz.</span><span class="sxs-lookup"><span data-stu-id="ebe25-136">When doing stream processing, a single line contains all the values with a tab character between each value.</span></span> <span data-ttu-id="ebe25-137">Ezért `string.split(line, "\t")` segítségével ossza fel a bemenet minden egyes lapját, csak a mezőket ad vissza.</span><span class="sxs-lookup"><span data-stu-id="ebe25-137">So `string.split(line, "\t")` can be used to split the input at each tab, returning just the fields.</span></span>
4. <span data-ttu-id="ebe25-138">Ha feldolgozása befejeződött, a kimenetet kell írni STDOUT sortörés, egy lap a mezők között.</span><span class="sxs-lookup"><span data-stu-id="ebe25-138">When processing is complete, the output must be written to STDOUT as a single line, with a tab between each field.</span></span> <span data-ttu-id="ebe25-139">Például: `print "\t".join([clientid, phone_label, hashlib.md5(phone_label).hexdigest()])`.</span><span class="sxs-lookup"><span data-stu-id="ebe25-139">For example, `print "\t".join([clientid, phone_label, hashlib.md5(phone_label).hexdigest()])`.</span></span>
5. <span data-ttu-id="ebe25-140">A `while` hurok ismétlődik, amíg nem `line` olvasható.</span><span class="sxs-lookup"><span data-stu-id="ebe25-140">The `while` loop repeats until no `line` is read.</span></span>

<span data-ttu-id="ebe25-141">Parancsfájl eredménye a bemeneti érték a összefűzése `devicemake` és `devicemodel`, és a összefűzött érték kivonatát.</span><span class="sxs-lookup"><span data-stu-id="ebe25-141">The script output is a concatenation of the input values for `devicemake` and `devicemodel`, and a hash of the concatenated value.</span></span>

<span data-ttu-id="ebe25-142">Lásd: [fut a példák](#running) ebben a példában a HDInsight-fürt futtatására.</span><span class="sxs-lookup"><span data-stu-id="ebe25-142">See [Running the examples](#running) for how to run this example on your HDInsight cluster.</span></span>

## <span data-ttu-id="ebe25-143"><a name="pigpython"></a>Pig UDF-ben</span><span class="sxs-lookup"><span data-stu-id="ebe25-143"><a name="pigpython"></a>Pig UDF</span></span>

<span data-ttu-id="ebe25-144">A Python-parancsfájl egy UDF a Pig keresztül használható a `GENERATE` utasítást.</span><span class="sxs-lookup"><span data-stu-id="ebe25-144">A Python script can be used as a UDF from Pig through the `GENERATE` statement.</span></span> <span data-ttu-id="ebe25-145">A parancsfájl Jython vagy C Python segítségével is futtathatja.</span><span class="sxs-lookup"><span data-stu-id="ebe25-145">You can run the script using either Jython or C Python.</span></span>

* <span data-ttu-id="ebe25-146">Jython a JVM-et futtatja, és a Pig natív módon kell meghívni.</span><span class="sxs-lookup"><span data-stu-id="ebe25-146">Jython runs on the JVM, and can natively be called from Pig.</span></span>
* <span data-ttu-id="ebe25-147">C Python egy külső folyamatban, így a Pig a JVM-et a adatait egy Python folyamatban futó parancsfájl által kiküldött.</span><span class="sxs-lookup"><span data-stu-id="ebe25-147">C Python is an external process, so the data from Pig on the JVM is sent out to the script running in a Python process.</span></span> <span data-ttu-id="ebe25-148">A Python-parancsfájl újra üzembe a Pig zajlik.</span><span class="sxs-lookup"><span data-stu-id="ebe25-148">The output of the Python script is sent back into Pig.</span></span>

<span data-ttu-id="ebe25-149">Adja meg a Python értelmező `register` való hivatkozáskor a Python-parancsfájl.</span><span class="sxs-lookup"><span data-stu-id="ebe25-149">To specify the Python interpreter, use `register` when referencing the Python script.</span></span> <span data-ttu-id="ebe25-150">Az alábbi példák mint a Pig parancsfájlok regisztrálása `myfuncs`:</span><span class="sxs-lookup"><span data-stu-id="ebe25-150">The following examples register scripts with Pig as `myfuncs`:</span></span>

* <span data-ttu-id="ebe25-151">**Jython használandó**:`register '/path/to/pigudf.py' using jython as myfuncs;`</span><span class="sxs-lookup"><span data-stu-id="ebe25-151">**To use Jython**: `register '/path/to/pigudf.py' using jython as myfuncs;`</span></span>
* <span data-ttu-id="ebe25-152">**C Python használandó**:`register '/path/to/pigudf.py' using streaming_python as myfuncs;`</span><span class="sxs-lookup"><span data-stu-id="ebe25-152">**To use C Python**: `register '/path/to/pigudf.py' using streaming_python as myfuncs;`</span></span>

> [!IMPORTANT]
> <span data-ttu-id="ebe25-153">Jython használata esetén a fájl elérési útját pig_jython lehet-e, vagy helyi elérési utat, vagy a WASB: / / elérési út.</span><span class="sxs-lookup"><span data-stu-id="ebe25-153">When using Jython, the path to the pig_jython file can be either a local path or a WASB:// path.</span></span> <span data-ttu-id="ebe25-154">C Python használata esetén egy fájlt a helyi fájlrendszerben, a csomópont, Ön által használt elküldeni a Pig feladatot kell hivatkoznia.</span><span class="sxs-lookup"><span data-stu-id="ebe25-154">However, when using C Python, you must reference a file on the local file system of the node that you are using to submit the Pig job.</span></span>

<span data-ttu-id="ebe25-155">Egyszer túli regisztráció, a Pig Latin ehhez a példához megegyezik a is:</span><span class="sxs-lookup"><span data-stu-id="ebe25-155">Once past registration, the Pig Latin for this example is the same for both:</span></span>

```pig
LOGS = LOAD 'wasb:///example/data/sample.log' as (LINE:chararray);
LOG = FILTER LOGS by LINE is not null;
DETAILS = FOREACH LOG GENERATE myfuncs.create_structure(LINE);
DUMP DETAILS;
```

<span data-ttu-id="ebe25-156">Ez a példa funkciója:</span><span class="sxs-lookup"><span data-stu-id="ebe25-156">Here's what this example does:</span></span>

1. <span data-ttu-id="ebe25-157">Az első sor betölti a mintaadatfájlokat `sample.log` történő `LOGS`.</span><span class="sxs-lookup"><span data-stu-id="ebe25-157">The first line loads the sample data file, `sample.log` into `LOGS`.</span></span> <span data-ttu-id="ebe25-158">Mint minden rekordot is meghatározza egy `chararray`.</span><span class="sxs-lookup"><span data-stu-id="ebe25-158">It also defines each record as a `chararray`.</span></span>
2. <span data-ttu-id="ebe25-159">A következő sorban kiszűri a null értékeket, tárolja a művelet eredményét `LOG`.</span><span class="sxs-lookup"><span data-stu-id="ebe25-159">The next line filters out any null values, storing the result of the operation into `LOG`.</span></span>
3. <span data-ttu-id="ebe25-160">A következő megismétli a blobban található rekordok keresztül `LOG` , és `GENERATE` meghívni a `create_structure` tölti be a Python/Jython parancsfájlban szereplő `myfuncs`.</span><span class="sxs-lookup"><span data-stu-id="ebe25-160">Next, it iterates over the records in `LOG` and uses `GENERATE` to invoke the `create_structure` method contained in the Python/Jython script loaded as `myfuncs`.</span></span> <span data-ttu-id="ebe25-161">`LINE`a függvény az aktuális rekord átadására szolgál.</span><span class="sxs-lookup"><span data-stu-id="ebe25-161">`LINE` is used to pass the current record to the function.</span></span>
4. <span data-ttu-id="ebe25-162">Végezetül a kimenetek vannak kiírt STDOUT való használata a `DUMP` parancsot.</span><span class="sxs-lookup"><span data-stu-id="ebe25-162">Finally, the outputs are dumped to STDOUT using the `DUMP` command.</span></span> <span data-ttu-id="ebe25-163">A parancs megjeleníti az eredményeket, a művelet befejeződése után.</span><span class="sxs-lookup"><span data-stu-id="ebe25-163">This command displays the results after the operation completes.</span></span>

### <a name="create-the-pigudfpy-file"></a><span data-ttu-id="ebe25-164">A pigudf.py fájl létrehozása</span><span class="sxs-lookup"><span data-stu-id="ebe25-164">Create the pigudf.py file</span></span>

<span data-ttu-id="ebe25-165">A fejlesztési környezetet hozzon létre egy szövegfájlt nevű `pigudf.py`.</span><span class="sxs-lookup"><span data-stu-id="ebe25-165">On your development environment, create a text file named `pigudf.py`.</span></span> <span data-ttu-id="ebe25-166">A fájl tartalmát az alábbira használata:</span><span class="sxs-lookup"><span data-stu-id="ebe25-166">Use the following code as the contents of the file:</span></span>

<a name="streamingpy"></a>

```python
# Uncomment the following if using C Python
#from pig_util import outputSchema

@outputSchema("log: {(date:chararray, time:chararray, classname:chararray, level:chararray, detail:chararray)}")
def create_structure(input):
    if (input.startswith('java.lang.Exception')):
        input = input[21:len(input)] + ' - java.lang.Exception'
    date, time, classname, level, detail = input.split(' ', 4)
    return date, time, classname, level, detail
```

<span data-ttu-id="ebe25-167">A Pig Latin példában meghatározott a `LINE` adjon meg egy chararray, mert nincs a bemeneti konzisztens séma.</span><span class="sxs-lookup"><span data-stu-id="ebe25-167">In the Pig Latin example, we defined the `LINE` input as a chararray because there is no consistent schema for the input.</span></span> <span data-ttu-id="ebe25-168">A Python-parancsfájl az adatok átalakítja a kimeneti konzisztens marad.</span><span class="sxs-lookup"><span data-stu-id="ebe25-168">The Python script transforms the data into a consistent schema for output.</span></span>

1. <span data-ttu-id="ebe25-169">A `@outputSchema` nyilatkozat meghatározása a Pig számára visszaadott adatok formátuma.</span><span class="sxs-lookup"><span data-stu-id="ebe25-169">The `@outputSchema` statement defines the format of the data that is returned to Pig.</span></span> <span data-ttu-id="ebe25-170">Ebben az esetben van egy **adatok tulajdonságcsomag**, amely a Pig adattípus.</span><span class="sxs-lookup"><span data-stu-id="ebe25-170">In this case, it's a **data bag**, which is a Pig data type.</span></span> <span data-ttu-id="ebe25-171">A csomagban van, amelyek mindegyike chararray (karakterláncok), a következő mezőket tartalmazza:</span><span class="sxs-lookup"><span data-stu-id="ebe25-171">The bag contains the following fields, all of which are chararray (strings):</span></span>

   * <span data-ttu-id="ebe25-172">dátum - naplófájlbejegyzést létrehozásának dátuma</span><span class="sxs-lookup"><span data-stu-id="ebe25-172">date - the date the log entry was created</span></span>
   * <span data-ttu-id="ebe25-173">idő - naplófájlbejegyzést létrehozásának időpontja</span><span class="sxs-lookup"><span data-stu-id="ebe25-173">time - the time the log entry was created</span></span>
   * <span data-ttu-id="ebe25-174">osztálynév - az osztály nevét a bejegyzés lett létrehozva</span><span class="sxs-lookup"><span data-stu-id="ebe25-174">classname - the class name the entry was created for</span></span>
   * <span data-ttu-id="ebe25-175">szint – a naplózási szint</span><span class="sxs-lookup"><span data-stu-id="ebe25-175">level - the log level</span></span>
   * <span data-ttu-id="ebe25-176">Részletek - részletes adatait a naplóbejegyzés</span><span class="sxs-lookup"><span data-stu-id="ebe25-176">detail - verbose details for the log entry</span></span>

2. <span data-ttu-id="ebe25-177">Ezt követően a `def create_structure(input)` azt a Pig kapott sor elemek függvényt.</span><span class="sxs-lookup"><span data-stu-id="ebe25-177">Next, the `def create_structure(input)` defines the function that Pig passes line items to.</span></span>

3. <span data-ttu-id="ebe25-178">A példaadatokat `sample.log`, főleg megfelel-e a dátum, idő, osztálynév szint, és részletesen sémát kell visszaadnia.</span><span class="sxs-lookup"><span data-stu-id="ebe25-178">The example data, `sample.log`, mostly conforms to the date, time, classname, level, and detail schema we want to return.</span></span> <span data-ttu-id="ebe25-179">Azonban tartalmaz néhány kezdődő sorok `*java.lang.Exception*`.</span><span class="sxs-lookup"><span data-stu-id="ebe25-179">However, it contains a few lines that begin with `*java.lang.Exception*`.</span></span> <span data-ttu-id="ebe25-180">Ezek a sorok felel meg a sémának módosítani kell.</span><span class="sxs-lookup"><span data-stu-id="ebe25-180">These lines must be modified to match the schema.</span></span> <span data-ttu-id="ebe25-181">A `if` utasítás ellenőrzi azok számára, majd a bemeneti adatok áthelyezése massages a `*java.lang.Exception*` karakterlánc végén, mihamarabb elérhetővé tenni az adatok beágyazott a várt kimeneti sémával.</span><span class="sxs-lookup"><span data-stu-id="ebe25-181">The `if` statement checks for those, then massages the input data to move the `*java.lang.Exception*` string to the end, bringing the data in-line with our expected output schema.</span></span>

4. <span data-ttu-id="ebe25-182">Ezt követően a `split` parancs segítségével az adatok, az első négy karaktereket.</span><span class="sxs-lookup"><span data-stu-id="ebe25-182">Next, the `split` command is used to split the data at the first four space characters.</span></span> <span data-ttu-id="ebe25-183">A kimeneti hozzá van rendelve, a `date`, `time`, `classname`, `level`, és `detail`.</span><span class="sxs-lookup"><span data-stu-id="ebe25-183">The output is assigned into `date`, `time`, `classname`, `level`, and `detail`.</span></span>

5. <span data-ttu-id="ebe25-184">Végül az értékek Pig visszatér.</span><span class="sxs-lookup"><span data-stu-id="ebe25-184">Finally, the values are returned to Pig.</span></span>

<span data-ttu-id="ebe25-185">Amikor adatokat küld vissza a Pig, a rendelkezik konzisztens marad a `@outputSchema` utasítást.</span><span class="sxs-lookup"><span data-stu-id="ebe25-185">When the data is returned to Pig, it has a consistent schema as defined in the `@outputSchema` statement.</span></span>

## <span data-ttu-id="ebe25-186"><a name="running"></a>Töltse fel, és futtassa a példák</span><span class="sxs-lookup"><span data-stu-id="ebe25-186"><a name="running"></a>Upload and run the examples</span></span>

> [!IMPORTANT]
> <span data-ttu-id="ebe25-187">A **SSH** csak a lépések egy Linux-alapú HDInsight-fürthöz.</span><span class="sxs-lookup"><span data-stu-id="ebe25-187">The **SSH** steps only work with a Linux-based HDInsight cluster.</span></span> <span data-ttu-id="ebe25-188">A **PowerShell** lépéseket egy Linux vagy a Windows-alapú HDInsight-fürthöz működik, de a Windows-ügyfél szükséges.</span><span class="sxs-lookup"><span data-stu-id="ebe25-188">The **PowerShell** steps work with either a Linux or Windows-based HDInsight cluster, but require a Windows client.</span></span>

### <a name="ssh"></a><span data-ttu-id="ebe25-189">SSH</span><span class="sxs-lookup"><span data-stu-id="ebe25-189">SSH</span></span>

<span data-ttu-id="ebe25-190">További információ az SSH használatával, lásd: [az SSH a Hdinsighttal](hdinsight-hadoop-linux-use-ssh-unix.md).</span><span class="sxs-lookup"><span data-stu-id="ebe25-190">For more information on using SSH, see [Use SSH with HDInsight](hdinsight-hadoop-linux-use-ssh-unix.md).</span></span>

1. <span data-ttu-id="ebe25-191">Használjon `scp` átmásolni a fájlokat a HDInsight-fürthöz.</span><span class="sxs-lookup"><span data-stu-id="ebe25-191">Use `scp` to copy the files to your HDInsight cluster.</span></span> <span data-ttu-id="ebe25-192">Például a következő parancsot a másolja a fájlokat egy fürt nevű **sajátfürt**.</span><span class="sxs-lookup"><span data-stu-id="ebe25-192">For example, the following command copies the files to a cluster named **mycluster**.</span></span>

    ```bash
    scp hiveudf.py pigudf.py myuser@mycluster-ssh.azurehdinsight.net:
    ```

2. <span data-ttu-id="ebe25-193">SSH használatával csatlakozhat a fürthöz.</span><span class="sxs-lookup"><span data-stu-id="ebe25-193">Use SSH to connect to the cluster.</span></span>

    ```bash
    ssh myuser@mycluster-ssh.azurehdinsight.net
    ```

3. <span data-ttu-id="ebe25-194">Az SSH-munkamenetet a python-fájlok a fürt WASB tárolóhelyét korábban feltöltött hozzá.</span><span class="sxs-lookup"><span data-stu-id="ebe25-194">From the SSH session, add the python files uploaded previously to the WASB storage for the cluster.</span></span>

    ```bash
    hdfs dfs -put hiveudf.py /hiveudf.py
    hdfs dfs -put pigudf.py /pigudf.py
    ```

<span data-ttu-id="ebe25-195">A fájlok feltöltése után az alábbi lépések segítségével a Hive és a Pig feladatok futtatása.</span><span class="sxs-lookup"><span data-stu-id="ebe25-195">After uploading the files, use the following steps to run the Hive and Pig jobs.</span></span>

#### <a name="use-the-hive-udf"></a><span data-ttu-id="ebe25-196">Az UDF-ben a Hive használata</span><span class="sxs-lookup"><span data-stu-id="ebe25-196">Use the Hive UDF</span></span>

1. <span data-ttu-id="ebe25-197">Használja a `hive` parancs használatával indítsa el a hive rendszerhéjat.</span><span class="sxs-lookup"><span data-stu-id="ebe25-197">Use the `hive` command to start the hive shell.</span></span> <span data-ttu-id="ebe25-198">Megjelenik egy `hive>` után betöltötte a rendszerhéj kérni.</span><span class="sxs-lookup"><span data-stu-id="ebe25-198">You should see a `hive>` prompt once the shell has loaded.</span></span>

2. <span data-ttu-id="ebe25-199">Adja meg a következő lekérdezés: a `hive>` parancssorba:</span><span class="sxs-lookup"><span data-stu-id="ebe25-199">Enter the following query at the `hive>` prompt:</span></span>

   ```hive
   add file wasb:///hiveudf.py;
   SELECT TRANSFORM (clientid, devicemake, devicemodel)
       USING 'python hiveudf.py' AS
       (clientid string, phoneLabel string, phoneHash string)
   FROM hivesampletable
   ORDER BY clientid LIMIT 50;
   ```

3. <span data-ttu-id="ebe25-200">Után utolsó sora bevitelét, a feladat elindul.</span><span class="sxs-lookup"><span data-stu-id="ebe25-200">After entering the last line, the job should start.</span></span> <span data-ttu-id="ebe25-201">Ha a feladat befejeződik, azt kimenetet visszaadja a következőhöz hasonló:</span><span class="sxs-lookup"><span data-stu-id="ebe25-201">Once the job completes, it returns output similar to the following example:</span></span>

        100041    RIM 9650    d476f3687700442549a83fac4560c51c
        100041    RIM 9650    d476f3687700442549a83fac4560c51c
        100042    Apple iPhone 4.2.x    375ad9a0ddc4351536804f1d5d0ea9b9
        100042    Apple iPhone 4.2.x    375ad9a0ddc4351536804f1d5d0ea9b9
        100042    Apple iPhone 4.2.x    375ad9a0ddc4351536804f1d5d0ea9b9

#### <a name="use-the-pig-udf"></a><span data-ttu-id="ebe25-202">A Pig UDF használata</span><span class="sxs-lookup"><span data-stu-id="ebe25-202">Use the Pig UDF</span></span>

1. <span data-ttu-id="ebe25-203">Használja a `pig` parancsot a rendszerhéj elindításához.</span><span class="sxs-lookup"><span data-stu-id="ebe25-203">Use the `pig` command to start the shell.</span></span> <span data-ttu-id="ebe25-204">Megjelenik egy `grunt>` után betöltötte a rendszerhéj kérni.</span><span class="sxs-lookup"><span data-stu-id="ebe25-204">You see a `grunt>` prompt once the shell has loaded.</span></span>

2. <span data-ttu-id="ebe25-205">Adja meg az alábbi utasításokat a `grunt>` parancssorba:</span><span class="sxs-lookup"><span data-stu-id="ebe25-205">Enter the following statements at the `grunt>` prompt:</span></span>

   ```pig
   Register wasb:///pigudf.py using jython as myfuncs;
   LOGS = LOAD 'wasb:///example/data/sample.log' as (LINE:chararray);
   LOG = FILTER LOGS by LINE is not null;
   DETAILS = foreach LOG generate myfuncs.create_structure(LINE);
   DUMP DETAILS;
   ```

3. <span data-ttu-id="ebe25-206">Írja be a következő parancsot, miután a feladat elindul.</span><span class="sxs-lookup"><span data-stu-id="ebe25-206">After entering the following line, the job should start.</span></span> <span data-ttu-id="ebe25-207">Ha a feladat befejeződik, az-kimenet visszaadása hasonló a következő adatokat:</span><span class="sxs-lookup"><span data-stu-id="ebe25-207">Once the job completes, it returns output similar to the following data:</span></span>

        ((2012-02-03,20:11:56,SampleClass5,[TRACE],verbose detail for id 990982084))
        ((2012-02-03,20:11:56,SampleClass7,[TRACE],verbose detail for id 1560323914))
        ((2012-02-03,20:11:56,SampleClass8,[DEBUG],detail for id 2083681507))
        ((2012-02-03,20:11:56,SampleClass3,[TRACE],verbose detail for id 1718828806))
        ((2012-02-03,20:11:56,SampleClass3,[INFO],everything normal for id 530537821))

4. <span data-ttu-id="ebe25-208">Használjon `quit` kattintva lépjen ki a Grunt rendszerhéjat, és a következők segítségével szerkesztheti a pigudf.py fájlt a helyi fájlrendszerben:</span><span class="sxs-lookup"><span data-stu-id="ebe25-208">Use `quit` to exit the Grunt shell, and then use the following to edit the pigudf.py file on the local file system:</span></span>

    ```bash
    nano pigudf.py
    ```

5. <span data-ttu-id="ebe25-209">Egyszer-szerkesztőben, állítsa vissza ezt a következő sor eltávolításával a `#` karakter eltávolítása a sor elejére:</span><span class="sxs-lookup"><span data-stu-id="ebe25-209">Once in the editor, uncomment the following line by removing the `#` character from the beginning of the line:</span></span>

    ```bash
    #from pig_util import outputSchema
    ```

    <span data-ttu-id="ebe25-210">A változás nem lett végrehajtva, ha a Ctrl + X segítségével zárja be a szerkesztőt.</span><span class="sxs-lookup"><span data-stu-id="ebe25-210">Once the change has been made, use Ctrl+X to exit the editor.</span></span> <span data-ttu-id="ebe25-211">Válassza ki az Y, és írja be a módosítások mentéséhez.</span><span class="sxs-lookup"><span data-stu-id="ebe25-211">Select Y, and then enter to save the changes.</span></span>

6. <span data-ttu-id="ebe25-212">Használja a `pig` indítsa újra a rendszerhéj parancsot.</span><span class="sxs-lookup"><span data-stu-id="ebe25-212">Use the `pig` command to start the shell again.</span></span> <span data-ttu-id="ebe25-213">Miután a a `grunt>` kérni, használja a következő a Python-parancsfájl használata a C Python értelmező futtatásához.</span><span class="sxs-lookup"><span data-stu-id="ebe25-213">Once you are at the `grunt>` prompt, use the following to run the Python script using the C Python interpreter.</span></span>

   ```pig
   Register 'pigudf.py' using streaming_python as myfuncs;
   LOGS = LOAD 'wasb:///example/data/sample.log' as (LINE:chararray);
   LOG = FILTER LOGS by LINE is not null;
   DETAILS = foreach LOG generate myfuncs.create_structure(LINE);
   DUMP DETAILS;
   ```

    <span data-ttu-id="ebe25-214">Miután a feladat befejeződik, megtekintheti az azonos kimenethez, ha korábban már futtatta a parancsfájl segítségével történő Jython.</span><span class="sxs-lookup"><span data-stu-id="ebe25-214">Once this job completes, you should see the same output as when you previously ran the script using Jython.</span></span>

### <a name="powershell-upload-the-files"></a><span data-ttu-id="ebe25-215">PowerShell: A fájl feltöltése</span><span class="sxs-lookup"><span data-stu-id="ebe25-215">PowerShell: Upload the files</span></span>

<span data-ttu-id="ebe25-216">PowerShell segítségével töltse fel a fájlokat a HDInsight-kiszolgálóra.</span><span class="sxs-lookup"><span data-stu-id="ebe25-216">You can use PowerShell to upload the files to the HDInsight server.</span></span> <span data-ttu-id="ebe25-217">A következő parancsfájl használata a Python fájlok feltöltéséhez:</span><span class="sxs-lookup"><span data-stu-id="ebe25-217">Use the following script to upload the Python files:</span></span>

> [!IMPORTANT] 
> <span data-ttu-id="ebe25-218">A jelen szakaszban szereplő lépéseket használhatja az Azure Powershellt.</span><span class="sxs-lookup"><span data-stu-id="ebe25-218">The steps in this section use Azure PowerShell.</span></span> <span data-ttu-id="ebe25-219">További információ az Azure PowerShell használatával, lásd: [telepítése és konfigurálása az Azure PowerShell](/powershell/azure/overview).</span><span class="sxs-lookup"><span data-stu-id="ebe25-219">For more information on using Azure PowerShell, see [How to install and configure Azure PowerShell](/powershell/azure/overview).</span></span>

```powershell
# Login to your Azure subscription
# Is there an active Azure subscription?
$sub = Get-AzureRmSubscription -ErrorAction SilentlyContinue
if(-not($sub))
{
    Add-AzureRmAccount
}

# Get cluster info
$clusterName = Read-Host -Prompt "Enter the HDInsight cluster name"
# Change the path to match the file location on your system
$pathToStreamingFile = "C:\path\to\hiveudf.py"
$pathToJythonFile = "C:\path\to\pigudf.py"

$clusterInfo = Get-AzureRmHDInsightCluster -ClusterName $clusterName
$resourceGroup = $clusterInfo.ResourceGroup
$storageAccountName=$clusterInfo.DefaultStorageAccount.split('.')[0]
$container=$clusterInfo.DefaultStorageContainer
$storageAccountKey=(Get-AzureRmStorageAccountKey `
    -Name $storageAccountName `
-ResourceGroupName $resourceGroup)[0].Value

#Create a storage content and upload the file
$context = New-AzureStorageContext `
    -StorageAccountName $storageAccountName `
    -StorageAccountKey $storageAccountKey

Set-AzureStorageBlobContent `
    -File $pathToStreamingFile `
    -Blob "hiveudf.py" `
    -Container $container `
    -Context $context

Set-AzureStorageBlobContent `
    -File $pathToJythonFile `
    -Blob "pigudf.py" `
    -Container $container `
    -Context $context
```
> [!IMPORTANT]
> <span data-ttu-id="ebe25-220">Módosítsa a `C:\path\to` értéket a fejlesztési környezetet a fájlok elérési útját.</span><span class="sxs-lookup"><span data-stu-id="ebe25-220">Change the `C:\path\to` value to the path to the files on your development environment.</span></span>

<span data-ttu-id="ebe25-221">Ezt a parancsfájlt a HDInsight-fürthöz adatainak beolvasása majd kibontja a fiók és az alapértelmezett tárfiók kulcsa, és feltölti a fájlok a tároló, a legfelső szintű.</span><span class="sxs-lookup"><span data-stu-id="ebe25-221">This script retrieves information for your HDInsight cluster, then extracts the account and key for the default storage account, and uploads the files to the root of the container.</span></span>

> [!NOTE]
> <span data-ttu-id="ebe25-222">Fájlok feltöltésével kapcsolatos további információkért lásd: a [feltölteni az adatokat a HDInsight Hadoop-feladatok](hdinsight-upload-data.md) dokumentum.</span><span class="sxs-lookup"><span data-stu-id="ebe25-222">For more information on uploading files, see the [Upload data for Hadoop jobs in HDInsight](hdinsight-upload-data.md) document.</span></span>

#### <a name="powershell-use-the-hive-udf"></a><span data-ttu-id="ebe25-223">PowerShell: A Hive UDF használata</span><span class="sxs-lookup"><span data-stu-id="ebe25-223">PowerShell: Use the Hive UDF</span></span>

<span data-ttu-id="ebe25-224">PowerShell távolról ugyanúgy futtathatják a Hive-lekérdezéseket is használható.</span><span class="sxs-lookup"><span data-stu-id="ebe25-224">PowerShell can also be used to remotely run Hive queries.</span></span> <span data-ttu-id="ebe25-225">A következő PowerShell-parancsfájl segítségével használó Hive-lekérdezések futtatása **hiveudf.py** parancsfájlt:</span><span class="sxs-lookup"><span data-stu-id="ebe25-225">Use the following PowerShell script to run a Hive query that uses **hiveudf.py** script:</span></span>

> [!IMPORTANT]
> <span data-ttu-id="ebe25-226">Előtt fut, a parancsprogram kéri a HTTPs/rendszergazdai fiók adatait a HDInsight-fürthöz.</span><span class="sxs-lookup"><span data-stu-id="ebe25-226">Before running, the script prompts you for the HTTPs/Admin account information for your HDInsight cluster.</span></span>

```powershell
# Login to your Azure subscription
# Is there an active Azure subscription?
$sub = Get-AzureRmSubscription -ErrorAction SilentlyContinue
if(-not($sub))
{
    Add-AzureRmAccount
}

# Get cluster info
$clusterName = Read-Host -Prompt "Enter the HDInsight cluster name"
$creds=Get-Credential -Message "Enter the login for the cluster"

# If using a Windows-based HDInsight cluster, change the USING statement to:
# "USING 'D:\Python27\python.exe hiveudf.py' AS " +
$HiveQuery = "add file wasb:///hiveudf.py;" +
                "SELECT TRANSFORM (clientid, devicemake, devicemodel) " +
                "USING 'python hiveudf.py' AS " +
                "(clientid string, phoneLabel string, phoneHash string) " +
                "FROM hivesampletable " +
                "ORDER BY clientid LIMIT 50;"

$jobDefinition = New-AzureRmHDInsightHiveJobDefinition `
    -Query $HiveQuery

$job = Start-AzureRmHDInsightJob `
    -ClusterName $clusterName `
    -JobDefinition $jobDefinition `
    -HttpCredential $creds
Write-Host "Wait for the Hive job to complete ..." -ForegroundColor Green
Wait-AzureRmHDInsightJob `
    -JobId $job.JobId `
    -ClusterName $clusterName `
    -HttpCredential $creds
# Uncomment the following to see stderr output
# Get-AzureRmHDInsightJobOutput `
#   -Clustername $clusterName `
#   -JobId $job.JobId `
#   -HttpCredential $creds `
#   -DisplayOutputType StandardError
Write-Host "Display the standard output ..." -ForegroundColor Green
Get-AzureRmHDInsightJobOutput `
    -Clustername $clusterName `
    -JobId $job.JobId `
    -HttpCredential $creds
```

<span data-ttu-id="ebe25-227">A kimenet a **Hive** feladatot az alábbi példához hasonlóan kell megjelennie:</span><span class="sxs-lookup"><span data-stu-id="ebe25-227">The output for the **Hive** job should appear similar to the following example:</span></span>

    100041    RIM 9650    d476f3687700442549a83fac4560c51c
    100041    RIM 9650    d476f3687700442549a83fac4560c51c
    100042    Apple iPhone 4.2.x    375ad9a0ddc4351536804f1d5d0ea9b9
    100042    Apple iPhone 4.2.x    375ad9a0ddc4351536804f1d5d0ea9b9
    100042    Apple iPhone 4.2.x    375ad9a0ddc4351536804f1d5d0ea9b9

#### <a name="pig-jython"></a><span data-ttu-id="ebe25-228">Pig (Jython)</span><span class="sxs-lookup"><span data-stu-id="ebe25-228">Pig (Jython)</span></span>

<span data-ttu-id="ebe25-229">PowerShell is használható a Pig Latin feladatok futtatásához.</span><span class="sxs-lookup"><span data-stu-id="ebe25-229">PowerShell can also be used to run Pig Latin jobs.</span></span> <span data-ttu-id="ebe25-230">A Pig Latin feladat által használt futtatásához a **pigudf.py** parancsfájl, a következő PowerShell-parancsfájl:</span><span class="sxs-lookup"><span data-stu-id="ebe25-230">To run a Pig Latin job that uses the **pigudf.py** script, use the following PowerShell script:</span></span>

> [!NOTE]
> <span data-ttu-id="ebe25-231">Ha távolról elküld egy PowerShell-lel feladatot, nincs lehetőség a értelmező C Python használandó.</span><span class="sxs-lookup"><span data-stu-id="ebe25-231">When remotely submitting a job using PowerShell, it is not possible to use C Python as the interpreter.</span></span>

```powershell
# Login to your Azure subscription
# Is there an active Azure subscription?
$sub = Get-AzureRmSubscription -ErrorAction SilentlyContinue
if(-not($sub))
{
    Add-AzureRmAccount
}

# Get cluster info
$clusterName = Read-Host -Prompt "Enter the HDInsight cluster name"
$creds=Get-Credential -Message "Enter the login for the cluster"

$PigQuery = "Register wasb:///pigudf.py using jython as myfuncs;" +
            "LOGS = LOAD 'wasb:///example/data/sample.log' as (LINE:chararray);" +
            "LOG = FILTER LOGS by LINE is not null;" +
            "DETAILS = foreach LOG generate myfuncs.create_structure(LINE);" +
            "DUMP DETAILS;"

$jobDefinition = New-AzureRmHDInsightPigJobDefinition -Query $PigQuery

$job = Start-AzureRmHDInsightJob `
    -ClusterName $clusterName `
    -JobDefinition $jobDefinition `
    -HttpCredential $creds

Write-Host "Wait for the Pig job to complete ..." -ForegroundColor Green
Wait-AzureRmHDInsightJob `
    -Job $job.JobId `
    -ClusterName $clusterName `
    -HttpCredential $creds
# Uncomment the following to see stderr output
# Get-AzureRmHDInsightJobOutput `
#    -Clustername $clusterName `
#    -JobId $job.JobId `
#    -HttpCredential $creds `
#    -DisplayOutputType StandardError
Write-Host "Display the standard output ..." -ForegroundColor Green
Get-AzureRmHDInsightJobOutput `
    -Clustername $clusterName `
    -JobId $job.JobId `
    -HttpCredential $creds
```

<span data-ttu-id="ebe25-232">A kimenet a **Pig** feladat a következő adatok hasonlóan kell megjelennie:</span><span class="sxs-lookup"><span data-stu-id="ebe25-232">The output for the **Pig** job should appear similar to the following data:</span></span>

    ((2012-02-03,20:11:56,SampleClass5,[TRACE],verbose detail for id 990982084))
    ((2012-02-03,20:11:56,SampleClass7,[TRACE],verbose detail for id 1560323914))
    ((2012-02-03,20:11:56,SampleClass8,[DEBUG],detail for id 2083681507))
    ((2012-02-03,20:11:56,SampleClass3,[TRACE],verbose detail for id 1718828806))
    ((2012-02-03,20:11:56,SampleClass3,[INFO],everything normal for id 530537821))

## <span data-ttu-id="ebe25-233"><a name="troubleshooting"></a>Hibaelhárítás</span><span class="sxs-lookup"><span data-stu-id="ebe25-233"><a name="troubleshooting"></a>Troubleshooting</span></span>

### <a name="errors-when-running-jobs"></a><span data-ttu-id="ebe25-234">Ha a futó feladatok hibák</span><span class="sxs-lookup"><span data-stu-id="ebe25-234">Errors when running jobs</span></span>

<span data-ttu-id="ebe25-235">A hive-feladatokban futtatásakor találkozhat az alábbihoz hasonló hiba:</span><span class="sxs-lookup"><span data-stu-id="ebe25-235">When running the hive job, you may encounter an error similar to the following text:</span></span>

    Caused by: org.apache.hadoop.hive.ql.metadata.HiveException: [Error 20001]: An error occurred while reading or writing to your custom script. It may have crashed with an error.

<span data-ttu-id="ebe25-236">Ez a probléma a sorvégződések a Python-fájl okozhatja.</span><span class="sxs-lookup"><span data-stu-id="ebe25-236">This problem may be caused by the line endings in the Python file.</span></span> <span data-ttu-id="ebe25-237">Az CRLF a sor befejezési, de a Linux-alkalmazások általában számos Windows szerkesztők alapértelmezett LF várt.</span><span class="sxs-lookup"><span data-stu-id="ebe25-237">Many Windows editors default to using CRLF as the line ending, but Linux applications usually expect LF.</span></span>

<span data-ttu-id="ebe25-238">A következő PowerShell-utasítások segítségével a CR karakter eltávolítása előtt a fájl feltöltése a HDInsight:</span><span class="sxs-lookup"><span data-stu-id="ebe25-238">You can use the following PowerShell statements to remove the CR characters before uploading the file to HDInsight:</span></span>

```powershell
$original_file ='c:\path\to\hiveudf.py'
$text = [IO.File]::ReadAllText($original_file) -replace "`r`n", "`n"
[IO.File]::WriteAllText($original_file, $text)
```

### <a name="powershell-scripts"></a><span data-ttu-id="ebe25-239">PowerShell-parancsfájlok</span><span class="sxs-lookup"><span data-stu-id="ebe25-239">PowerShell scripts</span></span>

<span data-ttu-id="ebe25-240">A példa a példák futtatásához használt PowerShell-parancsfájlok tartalmazniuk jeleníti meg a feladat kimenetének hiba megjegyzésként sorra.</span><span class="sxs-lookup"><span data-stu-id="ebe25-240">Both of the example PowerShell scripts used to run the examples contain a commented line that displays error output for the job.</span></span> <span data-ttu-id="ebe25-241">Ha nem Ön a feladat várható kimenete, állítsa vissza a következő sort, és tekintse meg, ha a hiba adataival kapcsolatos problémát jelez.</span><span class="sxs-lookup"><span data-stu-id="ebe25-241">If you are not seeing the expected output for the job, uncomment the following line and see if the error information indicates a problem.</span></span>

```powershell
# Get-AzureRmHDInsightJobOutput `
        -Clustername $clusterName `
        -JobId $job.JobId `
        -HttpCredential $creds `
        -DisplayOutputType StandardError
```

<span data-ttu-id="ebe25-242">A hibával kapcsolatos információk (STDERR) és a feladat (STDOUT) eredménye is bekerül a HDInsight-tárolóba.</span><span class="sxs-lookup"><span data-stu-id="ebe25-242">The error information (STDERR) and the result of the job (STDOUT) are also logged to your HDInsight storage.</span></span>

| <span data-ttu-id="ebe25-243">A feladat...</span><span class="sxs-lookup"><span data-stu-id="ebe25-243">For this job...</span></span> | <span data-ttu-id="ebe25-244">Nézze meg ezeket a fájlokat a blob a tárolóban</span><span class="sxs-lookup"><span data-stu-id="ebe25-244">Look at these files in the blob container</span></span> |
| --- | --- |
| <span data-ttu-id="ebe25-245">Hive</span><span class="sxs-lookup"><span data-stu-id="ebe25-245">Hive</span></span> |<span data-ttu-id="ebe25-246">/ HivePython/stderr</span><span class="sxs-lookup"><span data-stu-id="ebe25-246">/HivePython/stderr</span></span><p><span data-ttu-id="ebe25-247">/ HivePython/stdout</span><span class="sxs-lookup"><span data-stu-id="ebe25-247">/HivePython/stdout</span></span> |
| <span data-ttu-id="ebe25-248">Pig</span><span class="sxs-lookup"><span data-stu-id="ebe25-248">Pig</span></span> |<span data-ttu-id="ebe25-249">/ PigPython/stderr</span><span class="sxs-lookup"><span data-stu-id="ebe25-249">/PigPython/stderr</span></span><p><span data-ttu-id="ebe25-250">/ PigPython/stdout</span><span class="sxs-lookup"><span data-stu-id="ebe25-250">/PigPython/stdout</span></span> |

## <span data-ttu-id="ebe25-251"><a name="next"></a>Következő lépések</span><span class="sxs-lookup"><span data-stu-id="ebe25-251"><a name="next"></a>Next steps</span></span>

<span data-ttu-id="ebe25-252">Ha alapértelmezés szerint nem biztosított Python-modulok betöltése szüksége, tekintse meg [központi telepítése egy modul Azure HDInsight](http://blogs.msdn.com/b/benjguin/archive/2014/03/03/how-to-deploy-a-python-module-to-windows-azure-hdinsight.aspx).</span><span class="sxs-lookup"><span data-stu-id="ebe25-252">If you need to load Python modules that aren't provided by default, see [How to deploy a module to Azure HDInsight](http://blogs.msdn.com/b/benjguin/archive/2014/03/03/how-to-deploy-a-python-module-to-windows-azure-hdinsight.aspx).</span></span>

<span data-ttu-id="ebe25-253">Más használatának módjai sertésfelmérés, struktúra, és a MapReduce használatával kapcsolatos további tudnivalókért lásd a következő dokumentumokat:</span><span class="sxs-lookup"><span data-stu-id="ebe25-253">For other ways to use Pig, Hive, and to learn about using MapReduce, see the following documents:</span></span>

* [<span data-ttu-id="ebe25-254">A Hive használata a HDInsightban</span><span class="sxs-lookup"><span data-stu-id="ebe25-254">Use Hive with HDInsight</span></span>](hdinsight-use-hive.md)
* [<span data-ttu-id="ebe25-255">A Pig használata a HDInsightban</span><span class="sxs-lookup"><span data-stu-id="ebe25-255">Use Pig with HDInsight</span></span>](hdinsight-use-pig.md)
* [<span data-ttu-id="ebe25-256">Use MapReduce with HDInsight</span><span class="sxs-lookup"><span data-stu-id="ebe25-256">Use MapReduce with HDInsight</span></span>](hdinsight-use-mapreduce.md)
