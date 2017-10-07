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
# <a name="use-python-user-defined-functions-udf-with-hive-and-pig-in-hdinsight"></a>Használjon Python felhasználói függvény (UDF), a Hive és a Pig a Hdinsightban

Ismerje meg, hogyan toouse Python felhasználói függvény (UDF) Apache Hive és az Azure hdinsight Hadoop Pig.

## <a name="python"></a>A HDInsight Python

A HDInsight 3.0-s és újabb verziók alapértelmezés szerint telepítve van a Python2.7. Apache Hive használható ebben a Python adatfolyam feldolgozásra. Az adatfolyam feldolgozása STDOUT és STDIN toopass struktúra és adatok hello UDF használja.

HDInsight is Jython, amely a Java nyelven írt Python megvalósítása. Jython közvetlenül hello Java virtuális gép fut, és nem használja a streaming. Jython hello Python parancsértelmező ajánlott, ha a Pig használata a Python.

> [!WARNING]
> jelen dokumentumban leírt lépések hello hajtsa végre a következő feltételek hello: 
>
> * Létrehozhat hello Python-parancsfájlok helyi fejlesztési környezetben.
> * Hello parancsfájlok tooHDInsight vagy hello segítségével feltölti `scp` egy helyi Bash vagy hello PowerShell-parancsfájl a megadott parancsot.
>
> Ha azt szeretné, hogy toouse hello [Azure Cloud rendszerhéj (bash)](https://docs.microsoft.com/azure/cloud-shell/overview) toowork a hdinsight eszközzel, tekintse meg kell majd:
>
> * Hozzon létre hello parancsfájlok hello felhő rendszerhéj környezeten belül.
> * Használjon `scp` tooupload hello fájlok hello felhőalapú rendszerhéj tooHDInsight.
> * Használjon `ssh` hello felhő rendszerhéj tooconnect tooHDInsight és futtatási hello példák.

## <a name="hivepython"></a>Hive UDF-ben

Python használható egy UDF a Hive hello HiveQL keresztül `TRANSFORM` utasítást. Például a következő HiveQL hello hív hello `hiveudf.py` hello alapértelmezett Azure Storage-fiók hello fürt a tárolt fájl.

**Linux-alapú HDInsight**

```hiveql
add file wasb:///hiveudf.py;

SELECT TRANSFORM (clientid, devicemake, devicemodel)
    USING 'python hiveudf.py' AS
    (clientid string, phoneLable string, phoneHash string)
FROM hivesampletable
ORDER BY clientid LIMIT 50;
```

**Windows-alapú HDInsight**

```hiveql
add file wasb:///hiveudf.py;

SELECT TRANSFORM (clientid, devicemake, devicemodel)
    USING 'D:\Python27\python.exe hiveudf.py' AS
    (clientid string, phoneLable string, phoneHash string)
FROM hivesampletable
ORDER BY clientid LIMIT 50;
```

> [!NOTE]
> A Windows-alapú HDInsight-fürtökön, hello `USING` záradék hello teljes elérési útja toopython.exe kell megadnia.

Ez a példa funkciója:

1. Hello `add file` utasítás hello elején hello fájl hozzáadása hello `hiveudf.py` fájl toohello elosztott gyorsítótár, így hello fürt összes csomópontja által elérhető.
2. Hello `SELECT TRANSFORM ... USING` utasítás adatok kiválaszt hello `hivesampletable`. Hello clientid devicemake és devicemodel értékek toohello is átadja `hiveudf.py` parancsfájl.
3. Hello `AS` záradék által visszaadott hello mezők ismerteti `hiveudf.py`.

<a name="streamingpy"></a>

### <a name="create-hello-hiveudfpy-file"></a>Hello hiveudf.py fájl létrehozása


A fejlesztési környezetet hozzon létre egy szövegfájlt nevű `hiveudf.py`. Kód hello hello fájl tartalmát, a következő hello használata:

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

Ez a parancsfájl hello a következő műveleteket hajtja végre:

1. Egy adatsort STDIN olvasni.
2. Új sor karaktereket hello segítségével távolítja el `string.strip(line, "\n ")`.
3. Adatfolyam feldolgozása során egy sorba a tabulátor minden érték közötti minden hello értékeket tartalmaz. Ezért `string.split(line, "\t")` használt toosplit hello adjon meg minden lapjának csak hello mezők vissza is lehet.
4. Ha feldolgozása befejeződött, hello kimeneti úgy kell megírni tooSTDOUT sortörés, egy lap a mezők között. Például: `print "\t".join([clientid, phone_label, hashlib.md5(phone_label).hexdigest()])`.
5. Hello `while` hurok ismétlődik, amíg nem `line` olvasható.

hello parancsfájl eredménye a bemeneti értékek hello összefűzése `devicemake` és `devicemodel`, és hello kivonatát összefűzendő érték.

Lásd: [hello példák futtató](#running) arról, hogyan toorun ebben a példában a HDInsight-fürtre.

## <a name="pigpython"></a>Pig UDF-ben

A Python-parancsfájl használható egy UDF a Pig keresztül hello `GENERATE` utasítást. Jython vagy C Python hello parancsfájl futtatása.

* Jython hello JVM fut, és a Pig natív módon kell meghívni.
* C Python egy külső folyamatban, így a hello adatait Pig hello a JVM által kiküldött toohello parancsfájl Python folyamatokhoz. hello Python-parancsfájl kimenete hello újra üzembe a Pig zajlik.

toospecify hello Python parancsértelmező használata `register` való hivatkozáskor hello Python-parancsfájl. hello alábbi példák regisztrálása parancsfájlok, a Pig `myfuncs`:

* **toouse Jython**:`register '/path/to/pigudf.py' using jython as myfuncs;`
* **C Python toouse**:`register '/path/to/pigudf.py' using streaming_python as myfuncs;`

> [!IMPORTANT]
> Jython használatakor a hello elérési toohello pig_jython fájl lehet-e, vagy helyi elérési utat, vagy a WASB: / / elérési út. C Python használata esetén a fájl hello helyi fájlrendszerben toosubmit hello Pig feladatot használt hello csomópont kell hivatkoznia.

Amint túli regisztrációs, ebben a példában a Pig Latin hello hello azonos mindkét:

```pig
LOGS = LOAD 'wasb:///example/data/sample.log' as (LINE:chararray);
LOG = FILTER LOGS by LINE is not null;
DETAILS = FOREACH LOG GENERATE myfuncs.create_structure(LINE);
DUMP DETAILS;
```

Ez a példa funkciója:

1. első sor hello betölti hello mintaadatfájlokat `sample.log` történő `LOGS`. Mint minden rekordot is meghatározza egy `chararray`.
2. hello következő sor kiszűri a null értékeket, tárolja a hello művelet hello eredményét `LOG`.
3. A következő megismétli a hello rekordokat `LOG` , és `GENERATE` tooinvoke hello `create_structure` hello Python/Jython parancsfájlban szereplő tölti be `myfuncs`. `LINE`nem használt toopass hello aktuális rekord toohello függvény.
4. Végezetül hello kimenetek hello segítségével dömpingelt tooSTDOUT `DUMP` parancsot. Ez a parancs hello eredményeit jeleníti meg, hello művelet befejeződése után.

### <a name="create-hello-pigudfpy-file"></a>Hello pigudf.py fájl létrehozása

A fejlesztési környezetet hozzon létre egy szövegfájlt nevű `pigudf.py`. Kód hello hello fájl tartalmát, a következő hello használata:

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

Hello a Pig Latin példában hello meghatározott `LINE` adjon meg egy chararray, mivel nincs egységes séma hello bemeneti. hello Python-parancsfájl hello adatok átalakítja a kimeneti konzisztens marad.

1. Hello `@outputSchema` utasítás meghatározása hello formátuma hello tooPig visszaadott adatokat. Ebben az esetben van egy **adatok tulajdonságcsomag**, amely a Pig adattípus. hello tulajdonságcsomag tartalmazza, amelyek mindegyike chararray (karakterláncok), a mezők a következő hello:

   * dátum - hello dátum hello naplóbejegyzés lett létrehozva
   * idő – hello létrehozásakor hello naplóbejegyzés
   * osztálynév - hello osztály hello bejegyzése lett létrehozva
   * szint – hello naplózási szint
   * Részletek - hello részletes adatainak bejegyzést naplóz az olyan

2. A következő hello `def create_structure(input)` hello függvény Pig kapott sor elemek határozza meg.

3. Példa adatok hello `sample.log`, főleg toohello dátum, idő, osztálynév szint megfelel, és szeretnénk tooreturn séma részletességi. Azonban tartalmaz néhány kezdődő sorok `*java.lang.Exception*`. Ezek a sorok módosított toomatch hello sémát kell lennie. Hello `if` utasítás ellenőrzi azok számára, majd masszázs hello bemeneti adatok toomove hello `*java.lang.Exception*` karakterlánc toohello célból kapcsolásának hello adatok beágyazott a várt kimeneti sémával.

4. A következő hello `split` parancs: használt toosplit hello adatok hello első négy karaktereket. hello kimeneti hozzá van rendelve, a `date`, `time`, `classname`, `level`, és `detail`.

5. Végezetül hello értékek tooPig is megjelennek.

Hello adat tooPig, amikor a hello rendelkezik konzisztens marad `@outputSchema` utasítást.

## <a name="running"></a>Töltse fel, és futtassa a hello példák

> [!IMPORTANT]
> Hello **SSH** csak a lépések egy Linux-alapú HDInsight-fürthöz. Hello **PowerShell** lépéseket egy Linux vagy a Windows-alapú HDInsight-fürthöz működik, de a Windows-ügyfél szükséges.

### <a name="ssh"></a>SSH

További információ az SSH használatával, lásd: [az SSH a Hdinsighttal](hdinsight-hadoop-linux-use-ssh-unix.md).

1. Használjon `scp` toocopy hello fájlok tooyour HDInsight-fürthöz. Például a következő másolatok hello fájlok tooa fürt nevű parancs hello **sajátfürt**.

    ```bash
    scp hiveudf.py pigudf.py myuser@mycluster-ssh.azurehdinsight.net:
    ```

2. SSH tooconnect toohello-fürt használatára.

    ```bash
    ssh myuser@mycluster-ssh.azurehdinsight.net
    ```

3. Hello SSH-munkamenetet hozzá hello python fájlok korábban feltöltött toohello WASB hello fürt tárolóhelyét.

    ```bash
    hdfs dfs -put hiveudf.py /hiveudf.py
    hdfs dfs -put pigudf.py /pigudf.py
    ```

Hello fájlok feltöltése után használata hello következő toorun hello Hive és a Pig-feladatokhoz szükséges lépések.

#### <a name="use-hello-hive-udf"></a>Hive UDF hello használata

1. Használjon hello `hive` toostart hello hive parancsrendszerhéjában. Megjelenik egy `hive>` kérni, ha hello rendszerhéj be van töltve.

2. Adja meg a következő lekérdezést: hello hello `hive>` parancssorba:

   ```hive
   add file wasb:///hiveudf.py;
   SELECT TRANSFORM (clientid, devicemake, devicemodel)
       USING 'python hiveudf.py' AS
       (clientid string, phoneLabel string, phoneHash string)
   FROM hivesampletable
   ORDER BY clientid LIMIT 50;
   ```

3. Után utolsó sora hello bevitelét, hello feladat elindul. Miután hello feladat befejeződött, a következő példa kimenet hasonló toohello adja vissza:

        100041    RIM 9650    d476f3687700442549a83fac4560c51c
        100041    RIM 9650    d476f3687700442549a83fac4560c51c
        100042    Apple iPhone 4.2.x    375ad9a0ddc4351536804f1d5d0ea9b9
        100042    Apple iPhone 4.2.x    375ad9a0ddc4351536804f1d5d0ea9b9
        100042    Apple iPhone 4.2.x    375ad9a0ddc4351536804f1d5d0ea9b9

#### <a name="use-hello-pig-udf"></a>A Pig UDF hello használata

1. Használjon hello `pig` toostart hello parancsrendszerhéjában. Megjelenik egy `grunt>` kérni, ha hello rendszerhéj be van töltve.

2. Adja meg a következő utasításokat hello hello `grunt>` parancssorba:

   ```pig
   Register wasb:///pigudf.py using jython as myfuncs;
   LOGS = LOAD 'wasb:///example/data/sample.log' as (LINE:chararray);
   LOG = FILTER LOGS by LINE is not null;
   DETAILS = foreach LOG generate myfuncs.create_structure(LINE);
   DUMP DETAILS;
   ```

3. A következő sor hello bevitele, után hello feladat kell kezdődnie. Miután hello feladat befejeződött, kimeneti hasonló toohello a következő adatokat adja vissza:

        ((2012-02-03,20:11:56,SampleClass5,[TRACE],verbose detail for id 990982084))
        ((2012-02-03,20:11:56,SampleClass7,[TRACE],verbose detail for id 1560323914))
        ((2012-02-03,20:11:56,SampleClass8,[DEBUG],detail for id 2083681507))
        ((2012-02-03,20:11:56,SampleClass3,[TRACE],verbose detail for id 1718828806))
        ((2012-02-03,20:11:56,SampleClass3,[INFO],everything normal for id 530537821))

4. Használjon `quit` tooexit hello Grunt rendszerhéjat, és a következő tooedit hello pigudf.py fájl hello helyi fájlrendszerben hello:

    ```bash
    nano pigudf.py
    ```

5. Egyszer hello szerkesztő, állítsa vissza a következő sor hello eltávolításával hello `#` karakter eltávolítása hello sor elejére hello:

    ```bash
    #from pig_util import outputSchema
    ```

    Miután hello változás nem lett végrehajtva, használja a Ctrl + X tooexit hello szerkesztő. Válassza ki az Y, és írja be toosave hello módosításokat.

6. Használjon hello `pig` parancsfelület toostart hello újra. Miután a hello `grunt>` kérni, használja a következő toorun hello Python-parancsfájl használatával hello C Python parancsértelmező hello.

   ```pig
   Register 'pigudf.py' using streaming_python as myfuncs;
   LOGS = LOAD 'wasb:///example/data/sample.log' as (LINE:chararray);
   LOG = FILTER LOGS by LINE is not null;
   DETAILS = foreach LOG generate myfuncs.create_structure(LINE);
   DUMP DETAILS;
   ```

    Miután a feladat befejeződik, megtekintheti az azonos kimeneti hello, ha korábban már futtatta hello parancsfájl segítségével történő Jython.

### <a name="powershell-upload-hello-files"></a>PowerShell: Hello fájlokat

PowerShell tooupload hello fájlok toohello HDInsight-kiszolgáló is használhatja. Használja a következő parancsfájl tooupload hello Python fájlok hello:

> [!IMPORTANT] 
> Ebben a szakaszban található lépéseket hello Azure PowerShell használata. További információ az Azure PowerShell használatával, lásd: [hogyan tooinstall és konfigurálja az Azure Powershellt](/powershell/azure/overview).

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
> Változás hello `C:\path\to` toohello elérési toohello fájlokat a fejlesztési környezet érték.

Ezt a parancsfájlt a HDInsight-fürt adatait kéri le, majd kinyeri hello fiók és a kulcs hello alapértelmezett tárfiók, és feltöltések hello fájlok toohello legfelső szintű hello tároló.

> [!NOTE]
> Fájlok feltöltésével kapcsolatos további információkért lásd: hello [feltölteni az adatokat a HDInsight Hadoop-feladatok](hdinsight-upload-data.md) dokumentum.

#### <a name="powershell-use-hello-hive-udf"></a>PowerShell: Hello Hive UDF használata

PowerShell futtatásához használt tooremotely Hive-lekérdezéseket is lehet. A következő PowerShell-parancsfájl toorun használó Hive-lekérdezések használata hello **hiveudf.py** parancsfájlt:

> [!IMPORTANT]
> Előtt fut, hello parancsfájl kéri hello HTTPs/rendszergazdai fiók adatait a HDInsight-fürthöz.

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

hello kimenetének hello **Hive** feladat a következő példa hasonló toohello kell megjelennie:

    100041    RIM 9650    d476f3687700442549a83fac4560c51c
    100041    RIM 9650    d476f3687700442549a83fac4560c51c
    100042    Apple iPhone 4.2.x    375ad9a0ddc4351536804f1d5d0ea9b9
    100042    Apple iPhone 4.2.x    375ad9a0ddc4351536804f1d5d0ea9b9
    100042    Apple iPhone 4.2.x    375ad9a0ddc4351536804f1d5d0ea9b9

#### <a name="pig-jython"></a>Pig (Jython)

PowerShell is használható toorun Pig Latin feladatok. a Pig Latin feladat által használt hello toorun **pigudf.py** parancsfájl, a következő PowerShell-parancsfájl hello használata:

> [!NOTE]
> Egy feladat PowerShell használatával távolról elküldése esetén nem lehetséges toouse C Python hello parancsértelmező szerint.

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

hello kimenetének hello **Pig** feladat a következő adatok hasonló toohello kell megjelennie:

    ((2012-02-03,20:11:56,SampleClass5,[TRACE],verbose detail for id 990982084))
    ((2012-02-03,20:11:56,SampleClass7,[TRACE],verbose detail for id 1560323914))
    ((2012-02-03,20:11:56,SampleClass8,[DEBUG],detail for id 2083681507))
    ((2012-02-03,20:11:56,SampleClass3,[TRACE],verbose detail for id 1718828806))
    ((2012-02-03,20:11:56,SampleClass3,[INFO],everything normal for id 530537821))

## <a name="troubleshooting"></a>Hibaelhárítás

### <a name="errors-when-running-jobs"></a>Ha a futó feladatok hibák

Hello hive feladat futtatásakor a következő szöveg egy hiba hasonló toohello merülhetnek fel:

    Caused by: org.apache.hadoop.hive.ql.metadata.HiveException: [Error 20001]: An error occurred while reading or writing tooyour custom script. It may have crashed with an error.

Ez a probléma hello sorvégződések hello Python-fájl okozhatja. Számos Windows szerkesztők toousing CRLF alapértelmezett befejezési hello sorként, de általában a várt LF Linux alkalmazások.

Hello PowerShell utasítások tooremove hello CR karakter után hello fájl tooHDInsight feltöltés előtt használható:

```powershell
$original_file ='c:\path\to\hiveudf.py'
$text = [IO.File]::ReadAllText($original_file) -replace "`r`n", "`n"
[IO.File]::WriteAllText($original_file, $text)
```

### <a name="powershell-scripts"></a>PowerShell-parancsfájlok

Mindkét hello példában PowerShell-parancsfájlok toorun hello példákat tartalmaz megjegyzésként sorokkal hello feladat hiba kimenet megjelenítése. Ha nem Ön hello feladathoz tartozó várt hello kimeneti, állítsa vissza a hello alábbi. sor, és tekintse meg, ha hello hibainformációk kapcsolatos problémát jelez.

```powershell
# Get-AzureRmHDInsightJobOutput `
        -Clustername $clusterName `
        -JobId $job.JobId `
        -HttpCredential $creds `
        -DisplayOutputType StandardError
```

hello hibainformációk (STDERR) és hello feladat (STDOUT) hello eredménye egyaránt naplózott tooyour HDInsight-tárolóba.

| A feladat... | Nézze meg a fájlok hello blob a tárolóban |
| --- | --- |
| Hive |/ HivePython/stderr<p>/ HivePython/stdout |
| Pig |/ PigPython/stderr<p>/ PigPython/stdout |

## <a name="next"></a>Következő lépések

Ha alapértelmezés szerint nem biztosított tooload Python-modulok van szüksége, tekintse meg [hogyan toodeploy egy modul tooAzure HDInsight](http://blogs.msdn.com/b/benjguin/archive/2014/03/03/how-to-deploy-a-python-module-to-windows-azure-hdinsight.aspx).

Más módokon toouse Pig, a Hive és a toolearn MapReduce használatáról tekintse meg a következő dokumentumok hello:

* [A Hive használata a HDInsightban](hdinsight-use-hive.md)
* [A Pig használata a HDInsightban](hdinsight-use-pig.md)
* [Use MapReduce with HDInsight](hdinsight-use-mapreduce.md)
