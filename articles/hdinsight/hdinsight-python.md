---
title: "az Apache Hive és a Pig - Azure HDInsight UDF aaaPython |} Microsoft Docs"
description: "Ismerje meg, hogyan toouse Python felhasználó definiált függvény (UDF) a Hive és a hdinsight Hadoop technológia hello Pig verem az Azure-on."
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
ms.openlocfilehash: 26d8160cc6ed7fc22c3f06f7c1c9954c224b2366
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="use-python-user-defined-functions-udf-with-hive-and-pig-in-hdinsight"></a><span data-ttu-id="fe814-103">Használjon Python felhasználói függvény (UDF), a Hive és a Pig a Hdinsightban</span><span class="sxs-lookup"><span data-stu-id="fe814-103">Use Python User Defined Functions (UDF) with Hive and Pig in HDInsight</span></span>

<span data-ttu-id="fe814-104">Ismerje meg, hogyan toouse Python felhasználói függvény (UDF) Apache Hive és az Azure hdinsight Hadoop Pig.</span><span class="sxs-lookup"><span data-stu-id="fe814-104">Learn how toouse Python user-defined functions (UDF) with Apache Hive and Pig in Hadoop on Azure HDInsight.</span></span>

## <span data-ttu-id="fe814-105"><a name="python"></a>A HDInsight Python</span><span class="sxs-lookup"><span data-stu-id="fe814-105"><a name="python"></a>Python on HDInsight</span></span>

<span data-ttu-id="fe814-106">A HDInsight 3.0-s és újabb verziók alapértelmezés szerint telepítve van a Python2.7.</span><span class="sxs-lookup"><span data-stu-id="fe814-106">Python2.7 is installed by default on HDInsight 3.0 and later.</span></span> <span data-ttu-id="fe814-107">Apache Hive használható ebben a Python adatfolyam feldolgozásra.</span><span class="sxs-lookup"><span data-stu-id="fe814-107">Apache Hive can be used with this version of Python for stream processing.</span></span> <span data-ttu-id="fe814-108">Az adatfolyam feldolgozása STDOUT és STDIN toopass struktúra és adatok hello UDF használja.</span><span class="sxs-lookup"><span data-stu-id="fe814-108">Stream processing uses STDOUT and STDIN toopass data between Hive and hello UDF.</span></span>

<span data-ttu-id="fe814-109">HDInsight is Jython, amely a Java nyelven írt Python megvalósítása.</span><span class="sxs-lookup"><span data-stu-id="fe814-109">HDInsight also includes Jython, which is a Python implementation written in Java.</span></span> <span data-ttu-id="fe814-110">Jython közvetlenül hello Java virtuális gép fut, és nem használja a streaming.</span><span class="sxs-lookup"><span data-stu-id="fe814-110">Jython runs directly on hello Java Virtual Machine and does not use streaming.</span></span> <span data-ttu-id="fe814-111">Jython hello Python parancsértelmező ajánlott, ha a Pig használata a Python.</span><span class="sxs-lookup"><span data-stu-id="fe814-111">Jython is hello recommended Python interpreter when using Python with Pig.</span></span>

> [!WARNING]
> <span data-ttu-id="fe814-112">jelen dokumentumban leírt lépések hello hajtsa végre a következő feltételek hello:</span><span class="sxs-lookup"><span data-stu-id="fe814-112">hello steps in this document make hello following assumptions:</span></span> 
>
> * <span data-ttu-id="fe814-113">Létrehozhat hello Python-parancsfájlok helyi fejlesztési környezetben.</span><span class="sxs-lookup"><span data-stu-id="fe814-113">You create hello Python scripts on your local development environment.</span></span>
> * <span data-ttu-id="fe814-114">Hello parancsfájlok tooHDInsight vagy hello segítségével feltölti `scp` egy helyi Bash vagy hello PowerShell-parancsfájl a megadott parancsot.</span><span class="sxs-lookup"><span data-stu-id="fe814-114">You upload hello scripts tooHDInsight using either hello `scp` command from a local Bash session or hello provided PowerShell script.</span></span>
>
> <span data-ttu-id="fe814-115">Ha azt szeretné, hogy toouse hello [Azure Cloud rendszerhéj (bash)](https://docs.microsoft.com/azure/cloud-shell/overview) toowork a hdinsight eszközzel, tekintse meg kell majd:</span><span class="sxs-lookup"><span data-stu-id="fe814-115">If you want toouse hello [Azure Cloud Shell (bash)](https://docs.microsoft.com/azure/cloud-shell/overview) preview toowork with HDInsight, then you must:</span></span>
>
> * <span data-ttu-id="fe814-116">Hozzon létre hello parancsfájlok hello felhő rendszerhéj környezeten belül.</span><span class="sxs-lookup"><span data-stu-id="fe814-116">Create hello scripts inside hello cloud shell environment.</span></span>
> * <span data-ttu-id="fe814-117">Használjon `scp` tooupload hello fájlok hello felhőalapú rendszerhéj tooHDInsight.</span><span class="sxs-lookup"><span data-stu-id="fe814-117">Use `scp` tooupload hello files from hello cloud shell tooHDInsight.</span></span>
> * <span data-ttu-id="fe814-118">Használjon `ssh` hello felhő rendszerhéj tooconnect tooHDInsight és futtatási hello példák.</span><span class="sxs-lookup"><span data-stu-id="fe814-118">Use `ssh` from hello cloud shell tooconnect tooHDInsight and run hello examples.</span></span>

## <span data-ttu-id="fe814-119"><a name="hivepython"></a>Hive UDF-ben</span><span class="sxs-lookup"><span data-stu-id="fe814-119"><a name="hivepython"></a>Hive UDF</span></span>

<span data-ttu-id="fe814-120">Python használható egy UDF a Hive hello HiveQL keresztül `TRANSFORM` utasítást.</span><span class="sxs-lookup"><span data-stu-id="fe814-120">Python can be used as a UDF from Hive through hello HiveQL `TRANSFORM` statement.</span></span> <span data-ttu-id="fe814-121">Például a következő HiveQL hello hív hello `hiveudf.py` hello alapértelmezett Azure Storage-fiók hello fürt a tárolt fájl.</span><span class="sxs-lookup"><span data-stu-id="fe814-121">For example, hello following HiveQL invokes hello `hiveudf.py` file stored in hello default Azure Storage account for hello cluster.</span></span>

<span data-ttu-id="fe814-122">**Linux-alapú HDInsight**</span><span class="sxs-lookup"><span data-stu-id="fe814-122">**Linux-based HDInsight**</span></span>

```hiveql
add file wasb:///hiveudf.py;

SELECT TRANSFORM (clientid, devicemake, devicemodel)
    USING 'python hiveudf.py' AS
    (clientid string, phoneLable string, phoneHash string)
FROM hivesampletable
ORDER BY clientid LIMIT 50;
```

<span data-ttu-id="fe814-123">**Windows-alapú HDInsight**</span><span class="sxs-lookup"><span data-stu-id="fe814-123">**Windows-based HDInsight**</span></span>

```hiveql
add file wasb:///hiveudf.py;

SELECT TRANSFORM (clientid, devicemake, devicemodel)
    USING 'D:\Python27\python.exe hiveudf.py' AS
    (clientid string, phoneLable string, phoneHash string)
FROM hivesampletable
ORDER BY clientid LIMIT 50;
```

> [!NOTE]
> <span data-ttu-id="fe814-124">A Windows-alapú HDInsight-fürtökön, hello `USING` záradék hello teljes elérési útja toopython.exe kell megadnia.</span><span class="sxs-lookup"><span data-stu-id="fe814-124">On Windows-based HDInsight clusters, hello `USING` clause must specify hello full path toopython.exe.</span></span>

<span data-ttu-id="fe814-125">Ez a példa funkciója:</span><span class="sxs-lookup"><span data-stu-id="fe814-125">Here's what this example does:</span></span>

1. <span data-ttu-id="fe814-126">Hello `add file` utasítás hello elején hello fájl hozzáadása hello `hiveudf.py` fájl toohello elosztott gyorsítótár, így hello fürt összes csomópontja által elérhető.</span><span class="sxs-lookup"><span data-stu-id="fe814-126">hello `add file` statement at hello beginning of hello file adds hello `hiveudf.py` file toohello distributed cache, so it's accessible by all nodes in hello cluster.</span></span>
2. <span data-ttu-id="fe814-127">Hello `SELECT TRANSFORM ... USING` utasítás adatok kiválaszt hello `hivesampletable`.</span><span class="sxs-lookup"><span data-stu-id="fe814-127">hello `SELECT TRANSFORM ... USING` statement selects data from hello `hivesampletable`.</span></span> <span data-ttu-id="fe814-128">Hello clientid devicemake és devicemodel értékek toohello is átadja `hiveudf.py` parancsfájl.</span><span class="sxs-lookup"><span data-stu-id="fe814-128">It also passes hello clientid, devicemake, and devicemodel values toohello `hiveudf.py` script.</span></span>
3. <span data-ttu-id="fe814-129">Hello `AS` záradék által visszaadott hello mezők ismerteti `hiveudf.py`.</span><span class="sxs-lookup"><span data-stu-id="fe814-129">hello `AS` clause describes hello fields returned from `hiveudf.py`.</span></span>

<a name="streamingpy"></a>

### <a name="create-hello-hiveudfpy-file"></a><span data-ttu-id="fe814-130">Hello hiveudf.py fájl létrehozása</span><span class="sxs-lookup"><span data-stu-id="fe814-130">Create hello hiveudf.py file</span></span>


<span data-ttu-id="fe814-131">A fejlesztési környezetet hozzon létre egy szövegfájlt nevű `hiveudf.py`.</span><span class="sxs-lookup"><span data-stu-id="fe814-131">On your development environment, create a text file named `hiveudf.py`.</span></span> <span data-ttu-id="fe814-132">Kód hello hello fájl tartalmát, a következő hello használata:</span><span class="sxs-lookup"><span data-stu-id="fe814-132">Use hello following code as hello contents of hello file:</span></span>

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

<span data-ttu-id="fe814-133">Ez a parancsfájl hello a következő műveleteket hajtja végre:</span><span class="sxs-lookup"><span data-stu-id="fe814-133">This script performs hello following actions:</span></span>

1. <span data-ttu-id="fe814-134">Egy adatsort STDIN olvasni.</span><span class="sxs-lookup"><span data-stu-id="fe814-134">Read a line of data from STDIN.</span></span>
2. <span data-ttu-id="fe814-135">Új sor karaktereket hello segítségével távolítja el `string.strip(line, "\n ")`.</span><span class="sxs-lookup"><span data-stu-id="fe814-135">hello trailing newline character is removed using `string.strip(line, "\n ")`.</span></span>
3. <span data-ttu-id="fe814-136">Adatfolyam feldolgozása során egy sorba a tabulátor minden érték közötti minden hello értékeket tartalmaz.</span><span class="sxs-lookup"><span data-stu-id="fe814-136">When doing stream processing, a single line contains all hello values with a tab character between each value.</span></span> <span data-ttu-id="fe814-137">Ezért `string.split(line, "\t")` használt toosplit hello adjon meg minden lapjának csak hello mezők vissza is lehet.</span><span class="sxs-lookup"><span data-stu-id="fe814-137">So `string.split(line, "\t")` can be used toosplit hello input at each tab, returning just hello fields.</span></span>
4. <span data-ttu-id="fe814-138">Ha feldolgozása befejeződött, hello kimeneti úgy kell megírni tooSTDOUT sortörés, egy lap a mezők között.</span><span class="sxs-lookup"><span data-stu-id="fe814-138">When processing is complete, hello output must be written tooSTDOUT as a single line, with a tab between each field.</span></span> <span data-ttu-id="fe814-139">Például: `print "\t".join([clientid, phone_label, hashlib.md5(phone_label).hexdigest()])`.</span><span class="sxs-lookup"><span data-stu-id="fe814-139">For example, `print "\t".join([clientid, phone_label, hashlib.md5(phone_label).hexdigest()])`.</span></span>
5. <span data-ttu-id="fe814-140">Hello `while` hurok ismétlődik, amíg nem `line` olvasható.</span><span class="sxs-lookup"><span data-stu-id="fe814-140">hello `while` loop repeats until no `line` is read.</span></span>

<span data-ttu-id="fe814-141">hello parancsfájl eredménye a bemeneti értékek hello összefűzése `devicemake` és `devicemodel`, és hello kivonatát összefűzendő érték.</span><span class="sxs-lookup"><span data-stu-id="fe814-141">hello script output is a concatenation of hello input values for `devicemake` and `devicemodel`, and a hash of hello concatenated value.</span></span>

<span data-ttu-id="fe814-142">Lásd: [hello példák futtató](#running) arról, hogyan toorun ebben a példában a HDInsight-fürtre.</span><span class="sxs-lookup"><span data-stu-id="fe814-142">See [Running hello examples](#running) for how toorun this example on your HDInsight cluster.</span></span>

## <span data-ttu-id="fe814-143"><a name="pigpython"></a>Pig UDF-ben</span><span class="sxs-lookup"><span data-stu-id="fe814-143"><a name="pigpython"></a>Pig UDF</span></span>

<span data-ttu-id="fe814-144">A Python-parancsfájl használható egy UDF a Pig keresztül hello `GENERATE` utasítást.</span><span class="sxs-lookup"><span data-stu-id="fe814-144">A Python script can be used as a UDF from Pig through hello `GENERATE` statement.</span></span> <span data-ttu-id="fe814-145">Jython vagy C Python hello parancsfájl futtatása.</span><span class="sxs-lookup"><span data-stu-id="fe814-145">You can run hello script using either Jython or C Python.</span></span>

* <span data-ttu-id="fe814-146">Jython hello JVM fut, és a Pig natív módon kell meghívni.</span><span class="sxs-lookup"><span data-stu-id="fe814-146">Jython runs on hello JVM, and can natively be called from Pig.</span></span>
* <span data-ttu-id="fe814-147">C Python egy külső folyamatban, így a hello adatait Pig hello a JVM által kiküldött toohello parancsfájl Python folyamatokhoz.</span><span class="sxs-lookup"><span data-stu-id="fe814-147">C Python is an external process, so hello data from Pig on hello JVM is sent out toohello script running in a Python process.</span></span> <span data-ttu-id="fe814-148">hello Python-parancsfájl kimenete hello újra üzembe a Pig zajlik.</span><span class="sxs-lookup"><span data-stu-id="fe814-148">hello output of hello Python script is sent back into Pig.</span></span>

<span data-ttu-id="fe814-149">toospecify hello Python parancsértelmező használata `register` való hivatkozáskor hello Python-parancsfájl.</span><span class="sxs-lookup"><span data-stu-id="fe814-149">toospecify hello Python interpreter, use `register` when referencing hello Python script.</span></span> <span data-ttu-id="fe814-150">hello alábbi példák regisztrálása parancsfájlok, a Pig `myfuncs`:</span><span class="sxs-lookup"><span data-stu-id="fe814-150">hello following examples register scripts with Pig as `myfuncs`:</span></span>

* <span data-ttu-id="fe814-151">**toouse Jython**:`register '/path/to/pigudf.py' using jython as myfuncs;`</span><span class="sxs-lookup"><span data-stu-id="fe814-151">**toouse Jython**: `register '/path/to/pigudf.py' using jython as myfuncs;`</span></span>
* <span data-ttu-id="fe814-152">**C Python toouse**:`register '/path/to/pigudf.py' using streaming_python as myfuncs;`</span><span class="sxs-lookup"><span data-stu-id="fe814-152">**toouse C Python**: `register '/path/to/pigudf.py' using streaming_python as myfuncs;`</span></span>

> [!IMPORTANT]
> <span data-ttu-id="fe814-153">Jython használatakor a hello elérési toohello pig_jython fájl lehet-e, vagy helyi elérési utat, vagy a WASB: / / elérési út.</span><span class="sxs-lookup"><span data-stu-id="fe814-153">When using Jython, hello path toohello pig_jython file can be either a local path or a WASB:// path.</span></span> <span data-ttu-id="fe814-154">C Python használata esetén a fájl hello helyi fájlrendszerben toosubmit hello Pig feladatot használt hello csomópont kell hivatkoznia.</span><span class="sxs-lookup"><span data-stu-id="fe814-154">However, when using C Python, you must reference a file on hello local file system of hello node that you are using toosubmit hello Pig job.</span></span>

<span data-ttu-id="fe814-155">Amint túli regisztrációs, ebben a példában a Pig Latin hello hello azonos mindkét:</span><span class="sxs-lookup"><span data-stu-id="fe814-155">Once past registration, hello Pig Latin for this example is hello same for both:</span></span>

```pig
LOGS = LOAD 'wasb:///example/data/sample.log' as (LINE:chararray);
LOG = FILTER LOGS by LINE is not null;
DETAILS = FOREACH LOG GENERATE myfuncs.create_structure(LINE);
DUMP DETAILS;
```

<span data-ttu-id="fe814-156">Ez a példa funkciója:</span><span class="sxs-lookup"><span data-stu-id="fe814-156">Here's what this example does:</span></span>

1. <span data-ttu-id="fe814-157">első sor hello betölti hello mintaadatfájlokat `sample.log` történő `LOGS`.</span><span class="sxs-lookup"><span data-stu-id="fe814-157">hello first line loads hello sample data file, `sample.log` into `LOGS`.</span></span> <span data-ttu-id="fe814-158">Mint minden rekordot is meghatározza egy `chararray`.</span><span class="sxs-lookup"><span data-stu-id="fe814-158">It also defines each record as a `chararray`.</span></span>
2. <span data-ttu-id="fe814-159">hello következő sor kiszűri a null értékeket, tárolja a hello művelet hello eredményét `LOG`.</span><span class="sxs-lookup"><span data-stu-id="fe814-159">hello next line filters out any null values, storing hello result of hello operation into `LOG`.</span></span>
3. <span data-ttu-id="fe814-160">A következő megismétli a hello rekordokat `LOG` , és `GENERATE` tooinvoke hello `create_structure` hello Python/Jython parancsfájlban szereplő tölti be `myfuncs`.</span><span class="sxs-lookup"><span data-stu-id="fe814-160">Next, it iterates over hello records in `LOG` and uses `GENERATE` tooinvoke hello `create_structure` method contained in hello Python/Jython script loaded as `myfuncs`.</span></span> <span data-ttu-id="fe814-161">`LINE`nem használt toopass hello aktuális rekord toohello függvény.</span><span class="sxs-lookup"><span data-stu-id="fe814-161">`LINE` is used toopass hello current record toohello function.</span></span>
4. <span data-ttu-id="fe814-162">Végezetül hello kimenetek hello segítségével dömpingelt tooSTDOUT `DUMP` parancsot.</span><span class="sxs-lookup"><span data-stu-id="fe814-162">Finally, hello outputs are dumped tooSTDOUT using hello `DUMP` command.</span></span> <span data-ttu-id="fe814-163">Ez a parancs hello eredményeit jeleníti meg, hello művelet befejeződése után.</span><span class="sxs-lookup"><span data-stu-id="fe814-163">This command displays hello results after hello operation completes.</span></span>

### <a name="create-hello-pigudfpy-file"></a><span data-ttu-id="fe814-164">Hello pigudf.py fájl létrehozása</span><span class="sxs-lookup"><span data-stu-id="fe814-164">Create hello pigudf.py file</span></span>

<span data-ttu-id="fe814-165">A fejlesztési környezetet hozzon létre egy szövegfájlt nevű `pigudf.py`.</span><span class="sxs-lookup"><span data-stu-id="fe814-165">On your development environment, create a text file named `pigudf.py`.</span></span> <span data-ttu-id="fe814-166">Kód hello hello fájl tartalmát, a következő hello használata:</span><span class="sxs-lookup"><span data-stu-id="fe814-166">Use hello following code as hello contents of hello file:</span></span>

<a name="streamingpy"></a>

```python
# Uncomment hello following if using C Python
#from pig_util import outputSchema

@outputSchema("log: {(date:chararray, time:chararray, classname:chararray, level:chararray, detail:chararray)}")
def create_structure(input):
    if (input.startswith('java.lang.Exception')):
        input = input[21:len(input)] + ' - java.lang.Exception'
    date, time, classname, level, detail = input.split(' ', 4)
    return date, time, classname, level, detail
```

<span data-ttu-id="fe814-167">Hello a Pig Latin példában hello meghatározott `LINE` adjon meg egy chararray, mivel nincs egységes séma hello bemeneti.</span><span class="sxs-lookup"><span data-stu-id="fe814-167">In hello Pig Latin example, we defined hello `LINE` input as a chararray because there is no consistent schema for hello input.</span></span> <span data-ttu-id="fe814-168">hello Python-parancsfájl hello adatok átalakítja a kimeneti konzisztens marad.</span><span class="sxs-lookup"><span data-stu-id="fe814-168">hello Python script transforms hello data into a consistent schema for output.</span></span>

1. <span data-ttu-id="fe814-169">Hello `@outputSchema` utasítás meghatározása hello formátuma hello tooPig visszaadott adatokat.</span><span class="sxs-lookup"><span data-stu-id="fe814-169">hello `@outputSchema` statement defines hello format of hello data that is returned tooPig.</span></span> <span data-ttu-id="fe814-170">Ebben az esetben van egy **adatok tulajdonságcsomag**, amely a Pig adattípus.</span><span class="sxs-lookup"><span data-stu-id="fe814-170">In this case, it's a **data bag**, which is a Pig data type.</span></span> <span data-ttu-id="fe814-171">hello tulajdonságcsomag tartalmazza, amelyek mindegyike chararray (karakterláncok), a mezők a következő hello:</span><span class="sxs-lookup"><span data-stu-id="fe814-171">hello bag contains hello following fields, all of which are chararray (strings):</span></span>

   * <span data-ttu-id="fe814-172">dátum - hello dátum hello naplóbejegyzés lett létrehozva</span><span class="sxs-lookup"><span data-stu-id="fe814-172">date - hello date hello log entry was created</span></span>
   * <span data-ttu-id="fe814-173">idő – hello létrehozásakor hello naplóbejegyzés</span><span class="sxs-lookup"><span data-stu-id="fe814-173">time - hello time hello log entry was created</span></span>
   * <span data-ttu-id="fe814-174">osztálynév - hello osztály hello bejegyzése lett létrehozva</span><span class="sxs-lookup"><span data-stu-id="fe814-174">classname - hello class name hello entry was created for</span></span>
   * <span data-ttu-id="fe814-175">szint – hello naplózási szint</span><span class="sxs-lookup"><span data-stu-id="fe814-175">level - hello log level</span></span>
   * <span data-ttu-id="fe814-176">Részletek - hello részletes adatainak bejegyzést naplóz az olyan</span><span class="sxs-lookup"><span data-stu-id="fe814-176">detail - verbose details for hello log entry</span></span>

2. <span data-ttu-id="fe814-177">A következő hello `def create_structure(input)` hello függvény Pig kapott sor elemek határozza meg.</span><span class="sxs-lookup"><span data-stu-id="fe814-177">Next, hello `def create_structure(input)` defines hello function that Pig passes line items to.</span></span>

3. <span data-ttu-id="fe814-178">Példa adatok hello `sample.log`, főleg toohello dátum, idő, osztálynév szint megfelel, és szeretnénk tooreturn séma részletességi.</span><span class="sxs-lookup"><span data-stu-id="fe814-178">hello example data, `sample.log`, mostly conforms toohello date, time, classname, level, and detail schema we want tooreturn.</span></span> <span data-ttu-id="fe814-179">Azonban tartalmaz néhány kezdődő sorok `*java.lang.Exception*`.</span><span class="sxs-lookup"><span data-stu-id="fe814-179">However, it contains a few lines that begin with `*java.lang.Exception*`.</span></span> <span data-ttu-id="fe814-180">Ezek a sorok módosított toomatch hello sémát kell lennie.</span><span class="sxs-lookup"><span data-stu-id="fe814-180">These lines must be modified toomatch hello schema.</span></span> <span data-ttu-id="fe814-181">Hello `if` utasítás ellenőrzi azok számára, majd masszázs hello bemeneti adatok toomove hello `*java.lang.Exception*` karakterlánc toohello célból kapcsolásának hello adatok beágyazott a várt kimeneti sémával.</span><span class="sxs-lookup"><span data-stu-id="fe814-181">hello `if` statement checks for those, then massages hello input data toomove hello `*java.lang.Exception*` string toohello end, bringing hello data in-line with our expected output schema.</span></span>

4. <span data-ttu-id="fe814-182">A következő hello `split` parancs: használt toosplit hello adatok hello első négy karaktereket.</span><span class="sxs-lookup"><span data-stu-id="fe814-182">Next, hello `split` command is used toosplit hello data at hello first four space characters.</span></span> <span data-ttu-id="fe814-183">hello kimeneti hozzá van rendelve, a `date`, `time`, `classname`, `level`, és `detail`.</span><span class="sxs-lookup"><span data-stu-id="fe814-183">hello output is assigned into `date`, `time`, `classname`, `level`, and `detail`.</span></span>

5. <span data-ttu-id="fe814-184">Végezetül hello értékek tooPig is megjelennek.</span><span class="sxs-lookup"><span data-stu-id="fe814-184">Finally, hello values are returned tooPig.</span></span>

<span data-ttu-id="fe814-185">Hello adat tooPig, amikor a hello rendelkezik konzisztens marad `@outputSchema` utasítást.</span><span class="sxs-lookup"><span data-stu-id="fe814-185">When hello data is returned tooPig, it has a consistent schema as defined in hello `@outputSchema` statement.</span></span>

## <span data-ttu-id="fe814-186"><a name="running"></a>Töltse fel, és futtassa a hello példák</span><span class="sxs-lookup"><span data-stu-id="fe814-186"><a name="running"></a>Upload and run hello examples</span></span>

> [!IMPORTANT]
> <span data-ttu-id="fe814-187">Hello **SSH** csak a lépések egy Linux-alapú HDInsight-fürthöz.</span><span class="sxs-lookup"><span data-stu-id="fe814-187">hello **SSH** steps only work with a Linux-based HDInsight cluster.</span></span> <span data-ttu-id="fe814-188">Hello **PowerShell** lépéseket egy Linux vagy a Windows-alapú HDInsight-fürthöz működik, de a Windows-ügyfél szükséges.</span><span class="sxs-lookup"><span data-stu-id="fe814-188">hello **PowerShell** steps work with either a Linux or Windows-based HDInsight cluster, but require a Windows client.</span></span>

### <a name="ssh"></a><span data-ttu-id="fe814-189">SSH</span><span class="sxs-lookup"><span data-stu-id="fe814-189">SSH</span></span>

<span data-ttu-id="fe814-190">További információ az SSH használatával, lásd: [az SSH a Hdinsighttal](hdinsight-hadoop-linux-use-ssh-unix.md).</span><span class="sxs-lookup"><span data-stu-id="fe814-190">For more information on using SSH, see [Use SSH with HDInsight](hdinsight-hadoop-linux-use-ssh-unix.md).</span></span>

1. <span data-ttu-id="fe814-191">Használjon `scp` toocopy hello fájlok tooyour HDInsight-fürthöz.</span><span class="sxs-lookup"><span data-stu-id="fe814-191">Use `scp` toocopy hello files tooyour HDInsight cluster.</span></span> <span data-ttu-id="fe814-192">Például a következő másolatok hello fájlok tooa fürt nevű parancs hello **sajátfürt**.</span><span class="sxs-lookup"><span data-stu-id="fe814-192">For example, hello following command copies hello files tooa cluster named **mycluster**.</span></span>

    ```bash
    scp hiveudf.py pigudf.py myuser@mycluster-ssh.azurehdinsight.net:
    ```

2. <span data-ttu-id="fe814-193">SSH tooconnect toohello-fürt használatára.</span><span class="sxs-lookup"><span data-stu-id="fe814-193">Use SSH tooconnect toohello cluster.</span></span>

    ```bash
    ssh myuser@mycluster-ssh.azurehdinsight.net
    ```

3. <span data-ttu-id="fe814-194">Hello SSH-munkamenetet hozzá hello python fájlok korábban feltöltött toohello WASB hello fürt tárolóhelyét.</span><span class="sxs-lookup"><span data-stu-id="fe814-194">From hello SSH session, add hello python files uploaded previously toohello WASB storage for hello cluster.</span></span>

    ```bash
    hdfs dfs -put hiveudf.py /hiveudf.py
    hdfs dfs -put pigudf.py /pigudf.py
    ```

<span data-ttu-id="fe814-195">Hello fájlok feltöltése után használata hello következő toorun hello Hive és a Pig-feladatokhoz szükséges lépések.</span><span class="sxs-lookup"><span data-stu-id="fe814-195">After uploading hello files, use hello following steps toorun hello Hive and Pig jobs.</span></span>

#### <a name="use-hello-hive-udf"></a><span data-ttu-id="fe814-196">Hive UDF hello használata</span><span class="sxs-lookup"><span data-stu-id="fe814-196">Use hello Hive UDF</span></span>

1. <span data-ttu-id="fe814-197">Használjon hello `hive` toostart hello hive parancsrendszerhéjában.</span><span class="sxs-lookup"><span data-stu-id="fe814-197">Use hello `hive` command toostart hello hive shell.</span></span> <span data-ttu-id="fe814-198">Megjelenik egy `hive>` kérni, ha hello rendszerhéj be van töltve.</span><span class="sxs-lookup"><span data-stu-id="fe814-198">You should see a `hive>` prompt once hello shell has loaded.</span></span>

2. <span data-ttu-id="fe814-199">Adja meg a következő lekérdezést: hello hello `hive>` parancssorba:</span><span class="sxs-lookup"><span data-stu-id="fe814-199">Enter hello following query at hello `hive>` prompt:</span></span>

   ```hive
   add file wasb:///hiveudf.py;
   SELECT TRANSFORM (clientid, devicemake, devicemodel)
       USING 'python hiveudf.py' AS
       (clientid string, phoneLabel string, phoneHash string)
   FROM hivesampletable
   ORDER BY clientid LIMIT 50;
   ```

3. <span data-ttu-id="fe814-200">Után utolsó sora hello bevitelét, hello feladat elindul.</span><span class="sxs-lookup"><span data-stu-id="fe814-200">After entering hello last line, hello job should start.</span></span> <span data-ttu-id="fe814-201">Miután hello feladat befejeződött, a következő példa kimenet hasonló toohello adja vissza:</span><span class="sxs-lookup"><span data-stu-id="fe814-201">Once hello job completes, it returns output similar toohello following example:</span></span>

        100041    RIM 9650    d476f3687700442549a83fac4560c51c
        100041    RIM 9650    d476f3687700442549a83fac4560c51c
        100042    Apple iPhone 4.2.x    375ad9a0ddc4351536804f1d5d0ea9b9
        100042    Apple iPhone 4.2.x    375ad9a0ddc4351536804f1d5d0ea9b9
        100042    Apple iPhone 4.2.x    375ad9a0ddc4351536804f1d5d0ea9b9

#### <a name="use-hello-pig-udf"></a><span data-ttu-id="fe814-202">A Pig UDF hello használata</span><span class="sxs-lookup"><span data-stu-id="fe814-202">Use hello Pig UDF</span></span>

1. <span data-ttu-id="fe814-203">Használjon hello `pig` toostart hello parancsrendszerhéjában.</span><span class="sxs-lookup"><span data-stu-id="fe814-203">Use hello `pig` command toostart hello shell.</span></span> <span data-ttu-id="fe814-204">Megjelenik egy `grunt>` kérni, ha hello rendszerhéj be van töltve.</span><span class="sxs-lookup"><span data-stu-id="fe814-204">You see a `grunt>` prompt once hello shell has loaded.</span></span>

2. <span data-ttu-id="fe814-205">Adja meg a következő utasításokat hello hello `grunt>` parancssorba:</span><span class="sxs-lookup"><span data-stu-id="fe814-205">Enter hello following statements at hello `grunt>` prompt:</span></span>

   ```pig
   Register wasb:///pigudf.py using jython as myfuncs;
   LOGS = LOAD 'wasb:///example/data/sample.log' as (LINE:chararray);
   LOG = FILTER LOGS by LINE is not null;
   DETAILS = foreach LOG generate myfuncs.create_structure(LINE);
   DUMP DETAILS;
   ```

3. <span data-ttu-id="fe814-206">A következő sor hello bevitele, után hello feladat kell kezdődnie.</span><span class="sxs-lookup"><span data-stu-id="fe814-206">After entering hello following line, hello job should start.</span></span> <span data-ttu-id="fe814-207">Miután hello feladat befejeződött, kimeneti hasonló toohello a következő adatokat adja vissza:</span><span class="sxs-lookup"><span data-stu-id="fe814-207">Once hello job completes, it returns output similar toohello following data:</span></span>

        ((2012-02-03,20:11:56,SampleClass5,[TRACE],verbose detail for id 990982084))
        ((2012-02-03,20:11:56,SampleClass7,[TRACE],verbose detail for id 1560323914))
        ((2012-02-03,20:11:56,SampleClass8,[DEBUG],detail for id 2083681507))
        ((2012-02-03,20:11:56,SampleClass3,[TRACE],verbose detail for id 1718828806))
        ((2012-02-03,20:11:56,SampleClass3,[INFO],everything normal for id 530537821))

4. <span data-ttu-id="fe814-208">Használjon `quit` tooexit hello Grunt rendszerhéjat, és a következő tooedit hello pigudf.py fájl hello helyi fájlrendszerben hello:</span><span class="sxs-lookup"><span data-stu-id="fe814-208">Use `quit` tooexit hello Grunt shell, and then use hello following tooedit hello pigudf.py file on hello local file system:</span></span>

    ```bash
    nano pigudf.py
    ```

5. <span data-ttu-id="fe814-209">Egyszer hello szerkesztő, állítsa vissza a következő sor hello eltávolításával hello `#` karakter eltávolítása hello sor elejére hello:</span><span class="sxs-lookup"><span data-stu-id="fe814-209">Once in hello editor, uncomment hello following line by removing hello `#` character from hello beginning of hello line:</span></span>

    ```bash
    #from pig_util import outputSchema
    ```

    <span data-ttu-id="fe814-210">Miután hello változás nem lett végrehajtva, használja a Ctrl + X tooexit hello szerkesztő.</span><span class="sxs-lookup"><span data-stu-id="fe814-210">Once hello change has been made, use Ctrl+X tooexit hello editor.</span></span> <span data-ttu-id="fe814-211">Válassza ki az Y, és írja be toosave hello módosításokat.</span><span class="sxs-lookup"><span data-stu-id="fe814-211">Select Y, and then enter toosave hello changes.</span></span>

6. <span data-ttu-id="fe814-212">Használjon hello `pig` parancsfelület toostart hello újra.</span><span class="sxs-lookup"><span data-stu-id="fe814-212">Use hello `pig` command toostart hello shell again.</span></span> <span data-ttu-id="fe814-213">Miután a hello `grunt>` kérni, használja a következő toorun hello Python-parancsfájl használatával hello C Python parancsértelmező hello.</span><span class="sxs-lookup"><span data-stu-id="fe814-213">Once you are at hello `grunt>` prompt, use hello following toorun hello Python script using hello C Python interpreter.</span></span>

   ```pig
   Register 'pigudf.py' using streaming_python as myfuncs;
   LOGS = LOAD 'wasb:///example/data/sample.log' as (LINE:chararray);
   LOG = FILTER LOGS by LINE is not null;
   DETAILS = foreach LOG generate myfuncs.create_structure(LINE);
   DUMP DETAILS;
   ```

    <span data-ttu-id="fe814-214">Miután a feladat befejeződik, megtekintheti az azonos kimeneti hello, ha korábban már futtatta hello parancsfájl segítségével történő Jython.</span><span class="sxs-lookup"><span data-stu-id="fe814-214">Once this job completes, you should see hello same output as when you previously ran hello script using Jython.</span></span>

### <a name="powershell-upload-hello-files"></a><span data-ttu-id="fe814-215">PowerShell: Hello fájlokat</span><span class="sxs-lookup"><span data-stu-id="fe814-215">PowerShell: Upload hello files</span></span>

<span data-ttu-id="fe814-216">PowerShell tooupload hello fájlok toohello HDInsight-kiszolgáló is használhatja.</span><span class="sxs-lookup"><span data-stu-id="fe814-216">You can use PowerShell tooupload hello files toohello HDInsight server.</span></span> <span data-ttu-id="fe814-217">Használja a következő parancsfájl tooupload hello Python fájlok hello:</span><span class="sxs-lookup"><span data-stu-id="fe814-217">Use hello following script tooupload hello Python files:</span></span>

> [!IMPORTANT] 
> <span data-ttu-id="fe814-218">Ebben a szakaszban található lépéseket hello Azure PowerShell használata.</span><span class="sxs-lookup"><span data-stu-id="fe814-218">hello steps in this section use Azure PowerShell.</span></span> <span data-ttu-id="fe814-219">További információ az Azure PowerShell használatával, lásd: [hogyan tooinstall és konfigurálja az Azure Powershellt](/powershell/azure/overview).</span><span class="sxs-lookup"><span data-stu-id="fe814-219">For more information on using Azure PowerShell, see [How tooinstall and configure Azure PowerShell](/powershell/azure/overview).</span></span>

```powershell
# Login tooyour Azure subscription
# Is there an active Azure subscription?
$sub = Get-AzureRmSubscription -ErrorAction SilentlyContinue
if(-not($sub))
{
    Add-AzureRmAccount
}

# Get cluster info
$clusterName = Read-Host -Prompt "Enter hello HDInsight cluster name"
# Change hello path toomatch hello file location on your system
$pathToStreamingFile = "C:\path\to\hiveudf.py"
$pathToJythonFile = "C:\path\to\pigudf.py"

$clusterInfo = Get-AzureRmHDInsightCluster -ClusterName $clusterName
$resourceGroup = $clusterInfo.ResourceGroup
$storageAccountName=$clusterInfo.DefaultStorageAccount.split('.')[0]
$container=$clusterInfo.DefaultStorageContainer
$storageAccountKey=(Get-AzureRmStorageAccountKey `
    -Name $storageAccountName `
-ResourceGroupName $resourceGroup)[0].Value

#Create a storage content and upload hello file
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
> <span data-ttu-id="fe814-220">Változás hello `C:\path\to` toohello elérési toohello fájlokat a fejlesztési környezet érték.</span><span class="sxs-lookup"><span data-stu-id="fe814-220">Change hello `C:\path\to` value toohello path toohello files on your development environment.</span></span>

<span data-ttu-id="fe814-221">Ezt a parancsfájlt a HDInsight-fürt adatait kéri le, majd kinyeri hello fiók és a kulcs hello alapértelmezett tárfiók, és feltöltések hello fájlok toohello legfelső szintű hello tároló.</span><span class="sxs-lookup"><span data-stu-id="fe814-221">This script retrieves information for your HDInsight cluster, then extracts hello account and key for hello default storage account, and uploads hello files toohello root of hello container.</span></span>

> [!NOTE]
> <span data-ttu-id="fe814-222">Fájlok feltöltésével kapcsolatos további információkért lásd: hello [feltölteni az adatokat a HDInsight Hadoop-feladatok](hdinsight-upload-data.md) dokumentum.</span><span class="sxs-lookup"><span data-stu-id="fe814-222">For more information on uploading files, see hello [Upload data for Hadoop jobs in HDInsight](hdinsight-upload-data.md) document.</span></span>

#### <a name="powershell-use-hello-hive-udf"></a><span data-ttu-id="fe814-223">PowerShell: Hello Hive UDF használata</span><span class="sxs-lookup"><span data-stu-id="fe814-223">PowerShell: Use hello Hive UDF</span></span>

<span data-ttu-id="fe814-224">PowerShell futtatásához használt tooremotely Hive-lekérdezéseket is lehet.</span><span class="sxs-lookup"><span data-stu-id="fe814-224">PowerShell can also be used tooremotely run Hive queries.</span></span> <span data-ttu-id="fe814-225">A következő PowerShell-parancsfájl toorun használó Hive-lekérdezések használata hello **hiveudf.py** parancsfájlt:</span><span class="sxs-lookup"><span data-stu-id="fe814-225">Use hello following PowerShell script toorun a Hive query that uses **hiveudf.py** script:</span></span>

> [!IMPORTANT]
> <span data-ttu-id="fe814-226">Előtt fut, hello parancsfájl kéri hello HTTPs/rendszergazdai fiók adatait a HDInsight-fürthöz.</span><span class="sxs-lookup"><span data-stu-id="fe814-226">Before running, hello script prompts you for hello HTTPs/Admin account information for your HDInsight cluster.</span></span>

```powershell
# Login tooyour Azure subscription
# Is there an active Azure subscription?
$sub = Get-AzureRmSubscription -ErrorAction SilentlyContinue
if(-not($sub))
{
    Add-AzureRmAccount
}

# Get cluster info
$clusterName = Read-Host -Prompt "Enter hello HDInsight cluster name"
$creds=Get-Credential -Message "Enter hello login for hello cluster"

# If using a Windows-based HDInsight cluster, change hello USING statement to:
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
Write-Host "Wait for hello Hive job toocomplete ..." -ForegroundColor Green
Wait-AzureRmHDInsightJob `
    -JobId $job.JobId `
    -ClusterName $clusterName `
    -HttpCredential $creds
# Uncomment hello following toosee stderr output
# Get-AzureRmHDInsightJobOutput `
#   -Clustername $clusterName `
#   -JobId $job.JobId `
#   -HttpCredential $creds `
#   -DisplayOutputType StandardError
Write-Host "Display hello standard output ..." -ForegroundColor Green
Get-AzureRmHDInsightJobOutput `
    -Clustername $clusterName `
    -JobId $job.JobId `
    -HttpCredential $creds
```

<span data-ttu-id="fe814-227">hello kimenetének hello **Hive** feladat a következő példa hasonló toohello kell megjelennie:</span><span class="sxs-lookup"><span data-stu-id="fe814-227">hello output for hello **Hive** job should appear similar toohello following example:</span></span>

    100041    RIM 9650    d476f3687700442549a83fac4560c51c
    100041    RIM 9650    d476f3687700442549a83fac4560c51c
    100042    Apple iPhone 4.2.x    375ad9a0ddc4351536804f1d5d0ea9b9
    100042    Apple iPhone 4.2.x    375ad9a0ddc4351536804f1d5d0ea9b9
    100042    Apple iPhone 4.2.x    375ad9a0ddc4351536804f1d5d0ea9b9

#### <a name="pig-jython"></a><span data-ttu-id="fe814-228">Pig (Jython)</span><span class="sxs-lookup"><span data-stu-id="fe814-228">Pig (Jython)</span></span>

<span data-ttu-id="fe814-229">PowerShell is használható toorun Pig Latin feladatok.</span><span class="sxs-lookup"><span data-stu-id="fe814-229">PowerShell can also be used toorun Pig Latin jobs.</span></span> <span data-ttu-id="fe814-230">a Pig Latin feladat által használt hello toorun **pigudf.py** parancsfájl, a következő PowerShell-parancsfájl hello használata:</span><span class="sxs-lookup"><span data-stu-id="fe814-230">toorun a Pig Latin job that uses hello **pigudf.py** script, use hello following PowerShell script:</span></span>

> [!NOTE]
> <span data-ttu-id="fe814-231">Egy feladat PowerShell használatával távolról elküldése esetén nem lehetséges toouse C Python hello parancsértelmező szerint.</span><span class="sxs-lookup"><span data-stu-id="fe814-231">When remotely submitting a job using PowerShell, it is not possible toouse C Python as hello interpreter.</span></span>

```powershell
# Login tooyour Azure subscription
# Is there an active Azure subscription?
$sub = Get-AzureRmSubscription -ErrorAction SilentlyContinue
if(-not($sub))
{
    Add-AzureRmAccount
}

# Get cluster info
$clusterName = Read-Host -Prompt "Enter hello HDInsight cluster name"
$creds=Get-Credential -Message "Enter hello login for hello cluster"

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

Write-Host "Wait for hello Pig job toocomplete ..." -ForegroundColor Green
Wait-AzureRmHDInsightJob `
    -Job $job.JobId `
    -ClusterName $clusterName `
    -HttpCredential $creds
# Uncomment hello following toosee stderr output
# Get-AzureRmHDInsightJobOutput `
#    -Clustername $clusterName `
#    -JobId $job.JobId `
#    -HttpCredential $creds `
#    -DisplayOutputType StandardError
Write-Host "Display hello standard output ..." -ForegroundColor Green
Get-AzureRmHDInsightJobOutput `
    -Clustername $clusterName `
    -JobId $job.JobId `
    -HttpCredential $creds
```

<span data-ttu-id="fe814-232">hello kimenetének hello **Pig** feladat a következő adatok hasonló toohello kell megjelennie:</span><span class="sxs-lookup"><span data-stu-id="fe814-232">hello output for hello **Pig** job should appear similar toohello following data:</span></span>

    ((2012-02-03,20:11:56,SampleClass5,[TRACE],verbose detail for id 990982084))
    ((2012-02-03,20:11:56,SampleClass7,[TRACE],verbose detail for id 1560323914))
    ((2012-02-03,20:11:56,SampleClass8,[DEBUG],detail for id 2083681507))
    ((2012-02-03,20:11:56,SampleClass3,[TRACE],verbose detail for id 1718828806))
    ((2012-02-03,20:11:56,SampleClass3,[INFO],everything normal for id 530537821))

## <span data-ttu-id="fe814-233"><a name="troubleshooting"></a>Hibaelhárítás</span><span class="sxs-lookup"><span data-stu-id="fe814-233"><a name="troubleshooting"></a>Troubleshooting</span></span>

### <a name="errors-when-running-jobs"></a><span data-ttu-id="fe814-234">Ha a futó feladatok hibák</span><span class="sxs-lookup"><span data-stu-id="fe814-234">Errors when running jobs</span></span>

<span data-ttu-id="fe814-235">Hello hive feladat futtatásakor a következő szöveg egy hiba hasonló toohello merülhetnek fel:</span><span class="sxs-lookup"><span data-stu-id="fe814-235">When running hello hive job, you may encounter an error similar toohello following text:</span></span>

    Caused by: org.apache.hadoop.hive.ql.metadata.HiveException: [Error 20001]: An error occurred while reading or writing tooyour custom script. It may have crashed with an error.

<span data-ttu-id="fe814-236">Ez a probléma hello sorvégződések hello Python-fájl okozhatja.</span><span class="sxs-lookup"><span data-stu-id="fe814-236">This problem may be caused by hello line endings in hello Python file.</span></span> <span data-ttu-id="fe814-237">Számos Windows szerkesztők toousing CRLF alapértelmezett befejezési hello sorként, de általában a várt LF Linux alkalmazások.</span><span class="sxs-lookup"><span data-stu-id="fe814-237">Many Windows editors default toousing CRLF as hello line ending, but Linux applications usually expect LF.</span></span>

<span data-ttu-id="fe814-238">Hello PowerShell utasítások tooremove hello CR karakter után hello fájl tooHDInsight feltöltés előtt használható:</span><span class="sxs-lookup"><span data-stu-id="fe814-238">You can use hello following PowerShell statements tooremove hello CR characters before uploading hello file tooHDInsight:</span></span>

```powershell
$original_file ='c:\path\to\hiveudf.py'
$text = [IO.File]::ReadAllText($original_file) -replace "`r`n", "`n"
[IO.File]::WriteAllText($original_file, $text)
```

### <a name="powershell-scripts"></a><span data-ttu-id="fe814-239">PowerShell-parancsfájlok</span><span class="sxs-lookup"><span data-stu-id="fe814-239">PowerShell scripts</span></span>

<span data-ttu-id="fe814-240">Mindkét hello példában PowerShell-parancsfájlok toorun hello példákat tartalmaz megjegyzésként sorokkal hello feladat hiba kimenet megjelenítése.</span><span class="sxs-lookup"><span data-stu-id="fe814-240">Both of hello example PowerShell scripts used toorun hello examples contain a commented line that displays error output for hello job.</span></span> <span data-ttu-id="fe814-241">Ha nem Ön hello feladathoz tartozó várt hello kimeneti, állítsa vissza a hello alábbi. sor, és tekintse meg, ha hello hibainformációk kapcsolatos problémát jelez.</span><span class="sxs-lookup"><span data-stu-id="fe814-241">If you are not seeing hello expected output for hello job, uncomment hello following line and see if hello error information indicates a problem.</span></span>

```powershell
# Get-AzureRmHDInsightJobOutput `
        -Clustername $clusterName `
        -JobId $job.JobId `
        -HttpCredential $creds `
        -DisplayOutputType StandardError
```

<span data-ttu-id="fe814-242">hello hibainformációk (STDERR) és hello feladat (STDOUT) hello eredménye egyaránt naplózott tooyour HDInsight-tárolóba.</span><span class="sxs-lookup"><span data-stu-id="fe814-242">hello error information (STDERR) and hello result of hello job (STDOUT) are also logged tooyour HDInsight storage.</span></span>

| <span data-ttu-id="fe814-243">A feladat...</span><span class="sxs-lookup"><span data-stu-id="fe814-243">For this job...</span></span> | <span data-ttu-id="fe814-244">Nézze meg a fájlok hello blob a tárolóban</span><span class="sxs-lookup"><span data-stu-id="fe814-244">Look at these files in hello blob container</span></span> |
| --- | --- |
| <span data-ttu-id="fe814-245">Hive</span><span class="sxs-lookup"><span data-stu-id="fe814-245">Hive</span></span> |<span data-ttu-id="fe814-246">/ HivePython/stderr</span><span class="sxs-lookup"><span data-stu-id="fe814-246">/HivePython/stderr</span></span><p><span data-ttu-id="fe814-247">/ HivePython/stdout</span><span class="sxs-lookup"><span data-stu-id="fe814-247">/HivePython/stdout</span></span> |
| <span data-ttu-id="fe814-248">Pig</span><span class="sxs-lookup"><span data-stu-id="fe814-248">Pig</span></span> |<span data-ttu-id="fe814-249">/ PigPython/stderr</span><span class="sxs-lookup"><span data-stu-id="fe814-249">/PigPython/stderr</span></span><p><span data-ttu-id="fe814-250">/ PigPython/stdout</span><span class="sxs-lookup"><span data-stu-id="fe814-250">/PigPython/stdout</span></span> |

## <span data-ttu-id="fe814-251"><a name="next"></a>Következő lépések</span><span class="sxs-lookup"><span data-stu-id="fe814-251"><a name="next"></a>Next steps</span></span>

<span data-ttu-id="fe814-252">Ha alapértelmezés szerint nem biztosított tooload Python-modulok van szüksége, tekintse meg [hogyan toodeploy egy modul tooAzure HDInsight](http://blogs.msdn.com/b/benjguin/archive/2014/03/03/how-to-deploy-a-python-module-to-windows-azure-hdinsight.aspx).</span><span class="sxs-lookup"><span data-stu-id="fe814-252">If you need tooload Python modules that aren't provided by default, see [How toodeploy a module tooAzure HDInsight](http://blogs.msdn.com/b/benjguin/archive/2014/03/03/how-to-deploy-a-python-module-to-windows-azure-hdinsight.aspx).</span></span>

<span data-ttu-id="fe814-253">Más módokon toouse Pig, a Hive és a toolearn MapReduce használatáról tekintse meg a következő dokumentumok hello:</span><span class="sxs-lookup"><span data-stu-id="fe814-253">For other ways toouse Pig, Hive, and toolearn about using MapReduce, see hello following documents:</span></span>

* [<span data-ttu-id="fe814-254">A Hive használata a HDInsightban</span><span class="sxs-lookup"><span data-stu-id="fe814-254">Use Hive with HDInsight</span></span>](hdinsight-use-hive.md)
* [<span data-ttu-id="fe814-255">A Pig használata a HDInsightban</span><span class="sxs-lookup"><span data-stu-id="fe814-255">Use Pig with HDInsight</span></span>](hdinsight-use-pig.md)
* [<span data-ttu-id="fe814-256">Use MapReduce with HDInsight</span><span class="sxs-lookup"><span data-stu-id="fe814-256">Use MapReduce with HDInsight</span></span>](hdinsight-use-mapreduce.md)
