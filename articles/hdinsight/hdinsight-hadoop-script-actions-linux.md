---
title: "a Linux-alapú HDInsight - Azure aaaScript művelet fejlesztési |} Microsoft Docs"
description: "Ismerje meg, hogyan toouse Bash parancsfájlok toocustomize Linux-alapú HDInsight-fürtökön. hello parancsfájl művelet a HDInsight funkcióval toorun parancsfájlok során vagy a fürt létrehozása után. Parancsfájlok használt toochange fürtbeállítások és további szoftvereket telepíteni."
services: hdinsight
documentationcenter: 
author: Blackmist
manager: jhubbard
editor: cgronlun
ms.assetid: cf4c89cd-f7da-4a10-857f-838004965d3e
ms.service: hdinsight
ms.custom: hdinsightactive
ms.workload: big-data
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/31/2017
ms.author: larryfr
ms.openlocfilehash: 1f504b00365df5f4cfb3ae19ad55ff7630342650
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="script-action-development-with-hdinsight"></a>Parancsfájl művelet fejlesztése a Hdinsighttal

Megtudhatja, hogyan toocustomize a HDInsight fürt segítségével Bash parancsfájlok. A Parancsfájlműveletek olyan egy módon toocustomize HDInsight alatt vagy a fürt létrehozása után.

> [!IMPORTANT]
> hello jelen dokumentumban leírt lépések egy HDInsight-fürt által használt Linux igényelnek. Linux hello azt az egyetlen operációs rendszer, használja a HDInsight 3.4 vagy újabb verziója. További tudnivalókért lásd: [A HDInsight elavulása Windows rendszeren](hdinsight-component-versioning.md#hdinsight-windows-retirement).

## <a name="what-are-script-actions"></a>Mik azok a Parancsfájlműveletek

A Parancsfájlműveletek a Bash parancsfájlok futó Azure hello a fürt csomópontjai toomake konfigurációjának módosításai, vagy szoftvereket telepíteni. A parancsfájlművelet legfelső szintű végrehajtásakor, és a jogok toohello fürtcsomópontok teljes hozzáférést biztosít.

A Parancsfájlműveletek hello a következő módszerek alkalmazhatók:

| A metódus tooapply parancsfájl használata... | Során fürt létrehozása... | Egy futó fürtön... |
| --- |:---:|:---:|
| Azure Portal |✓ |✓ |
| Azure PowerShell |✓ |✓ |
| Azure CLI |&nbsp; |✓ |
| HDInsight .NET SDK |✓ |✓ |
| Az Azure Resource Manager-sablon |✓ |&nbsp; |

Ezen módszerek tooapply Parancsfájlműveletek használatával kapcsolatos további információkért lásd: [testreszabása HDInsight-fürtök Parancsfájlműveletek segítségével](hdinsight-hadoop-customize-cluster-linux.md).

## <a name="bestPracticeScripting"></a>Parancsfájl fejlesztési ajánlott eljárásai

A HDInsight-fürtök egyéni parancsfájl fejlesztésekor van néhány ajánlott eljárások tookeep figyelembe vételével:

* [Hello Hadoop célverzió](#bPS1)
* [Cél hello operációs rendszer verziója](#bps10)
* [Adja meg a stabil tooscript erőforrások hivatkozásokat tartalmaz.](#bPS2)
* [Előre lefordított erőforrások használata](#bPS4)
* [Győződjön meg arról, hogy hello fürt testreszabási parancsfájl idempotent](#bPS3)
* [Magas rendelkezésre állásának hello fürt architektúrája](#bPS5)
* [Hello egyéni összetevők toouse Azure Blob-tároló konfigurálása](#bPS6)
* [Információ tooSTDOUT és az STDERR írása](#bPS7)
* [ASCII LF sorvégződések rendelkező fájlok mentése](#bps8)
* [Újrapróbálkozási logika toorecover átmeneti hibák használata](#bps9)

> [!IMPORTANT]
> A Parancsfájlműveletek 60 percen belül kell végrehajtani, vagy hello folyamat sikertelen lesz. Csomópont telepítése során, hello parancsfájl egyidejűleg más beállítás és konfiguráció folyamatok fut. Erőforrások, például a CPU-idő vagy a hálózati sávszélesség erőforrásokért való versengés hello parancsfájl tootake hosszabb toofinish azonban nem a fejlesztési környezetet, mint okozhat.

### <a name="bPS1"></a>Hello Hadoop célverzió

Különböző verzióit a HDInsight Hadoop-szolgáltatások és telepített összetevők különböző verziói működnek. Ha a parancsfájl egy szolgáltatás vagy összetevő adott verziójának vár, csak akkor ajánlott hello parancsfájl, amely tartalmazza a szükséges hello összetevők HDInsight hello verziójával. Találhat információkat tartalmazza a hdinsight eszközzel összetevő verzióin hello segítségével [HDInsight-összetevők verziószámozása](hdinsight-component-versioning.md) dokumentum.

### <a name="bps10"></a>Cél hello az operációs rendszer verziója

Linux-alapú HDInsight hello Ubuntu Linux-disztribúció alapul. HDInsight különböző verziói különböző verzióinak Ubuntu, módosíthatja a parancsfájl működését támaszkodnak. HDInsight 3.4 és korábbi például Ubuntu verziók Upstart használó alapul. 3.5-ös verziója Ubuntu 16.04, amely használja Systemd alapul. Systemd és Upstart támaszkodnak különböző parancsok, a parancsfájl mindkét toowork kell írni.

HDInsight 3.4 és 3.5-ös közötti másik fontos különbség, hogy `JAVA_HOME` tooJava 8 most mutat.

Hello az operációs rendszer verzió ellenőrzéséhez hajtsa végre `lsb_release`. hello következő kód bemutatja, hogyan toodetermine Ha hello parancsfájl Ubuntu 14 vagy 16 fut:

```bash
OS_VERSION=$(lsb_release -sr)
if [[ $OS_VERSION == 14* ]]; then
    echo "OS verion is $OS_VERSION. Using hue-binaries-14-04."
    HUE_TARFILE=hue-binaries-14-04.tgz
elif [[ $OS_VERSION == 16* ]]; then
    echo "OS verion is $OS_VERSION. Using hue-binaries-16-04."
    HUE_TARFILE=hue-binaries-16-04.tgz
fi
...
if [[ $OS_VERSION == 16* ]]; then
    echo "Using systemd configuration"
    systemctl daemon-reload
    systemctl stop webwasb.service    
    systemctl start webwasb.service
else
    echo "Using upstart configuration"
    initctl reload-configuration
    stop webwasb
    start webwasb
fi
...
if [[ $OS_VERSION == 14* ]]; then
    export JAVA_HOME=/usr/lib/jvm/java-7-openjdk-amd64
elif [[ $OS_VERSION == 16* ]]; then
    export JAVA_HOME=/usr/lib/jvm/java-8-openjdk-amd64
fi
```

Ezek a következő https://hdiconfigactions.blob.core.windows.net/linuxhueconfigactionv02/install-hue-uber-v02.sh szövegrészek tartalmazó hello teljes parancsfájl található.

A HDInsight által használt Ubuntu hello-verziójáért lásd: hello [HDInsight összetevő verziószáma](hdinsight-component-versioning.md) dokumentum.

toounderstand hello Systemd és különbségei Upstart, lásd: [Systemd Upstart felhasználók](https://wiki.ubuntu.com/SystemdForUpstartUsers).

### <a name="bPS2"></a>Adja meg a stabil tooscript erőforrások hivatkozásokat tartalmaz.

hello parancsfájlt és a kapcsolódó erőforrások rendelkezésre kell állnia hello fürt hello élettartama során. Ezeket az erőforrásokat szükség, ha új csomóponttal egészülnek toohello fürt skálázás műveletek során.

hello célszerű toodownload, és archiválja mindent az Azure Storage-fiók előfizetéséhez.

> [!IMPORTANT]
> hello használt tárfiók hello alapértelmezett tárfiók hello fürt vagy egy nyilvános, csak olvasható tároló bármely más tárfiók kell lennie.

Például a Microsoft által biztosított hello minták hello tárolódnak [https://hdiconfigactions.blob.core.windows.net/](https://hdiconfigactions.blob.core.windows.net/) tárfiók. Ez egy olyan nyilvános, csak olvasható tároló, hello HDInsight csapat tartja fenn.

### <a name="bPS4"></a>Előre lefordított erőforrások használata

tooreduce hello alkalommal vesz toorun hello parancsfájl, erőforrások a forráskód fordítási műveletek elkerülése érdekében. Például előre lefordítani az erőforrásokat, és tárolja őket az Azure Storage-fiók blobot a hello azonos adatközpontba, ahol a HDInsight.

### <a name="bPS3"></a>Győződjön meg arról, hogy hello fürt testreszabási parancsfájl idempotent

Parancsfájlok idempotent kell lennie. Ha a hello parancsfájl több alkalommal fut, az kell visszaadnia minden egyes állapot azonos fürt toohello hello.

Például egy parancsfájlt, amely módosítja a konfigurációs fájlok ne adja hozzá duplikált bejegyzéseket Ha több alkalommal lefutott.

### <a name="bPS5"></a>Magas rendelkezésre állásának hello fürt architektúrája

Linux-alapú HDInsight-fürtök meg két átjárócsomópontokkal aktív hello fürtön belül, és Parancsfájlműveletek mindkét csomópontján futtatni. Ha csak egy átjárócsomópont várható hello összetevők telepítése hello összetevők telepítésének mindkét központi csomópontján.

> [!IMPORTANT]
> Szolgáltatások HDInsight részeként kialakított toofail keresztül történik hello két központi csomópontok között szükség szerint. Ez a funkció nem hosszabbodik toocustom összetevők Parancsfájlműveletek segítségével telepíthető. Ha magas rendelkezésre állású egyéni összetevők, meg kell valósítani a saját feladatátvételi mechanizmus.

### <a name="bPS6"></a>Hello egyéni összetevők toouse Azure Blob-tároló konfigurálása

Összetevőket telepít a hello fürt Hadoop elosztott fájlrendszerrel (HDFS) tárolást használ alapértelmezett konfigurációja lehet. HDInsight hello alapértelmezett tárolóként használ az Azure Storage vagy a Data Lake Store. Mindkét egy HDFS kompatibilis fájlrendszert biztosít, amely az adatok továbbra is fennáll, akkor is, ha hello fürtök törlése. Szükség lehet tooconfigure összetevők telepítése toouse WASB vagy ADL HDFS helyett.

A legtöbb műveleteket nem kell toospecify hello fájlrendszer. Például hello következő hello giraph-examples.jar fájlt átmásolja hello helyi rendszer toocluster fájltároló:

```bash
hdfs dfs -put /usr/hdp/current/giraph/giraph-examples.jar /example/jars/
```

Ebben a példában hello `hdfs` parancs átlátható módon használja a hello alapértelmezett fürttároló. Egyes műveletek esetében szükség lehet a toospecify hello URI. Például `adl:///example/jars` a Data Lake Store vagy `wasb:///example/jars` az Azure Storage.

### <a name="bPS7"></a>Információ tooSTDOUT és az STDERR írása

HDInsight parancsfájl kimenete, amely írásbeli tooSTDOUT és az STDERR naplók. Ezt az információt hello Ambari webes felhasználói felület használatával tekintheti meg.

> [!NOTE]
> Ambari csak akkor használható, ha hello fürt sikeresen létrehozva. Ha Ön egy parancsfájlművelettel és a fürt létrehozása, létrehozása sikertelen, tekintse meg a hibaelhárítási szakaszában hello [testreszabása HDInsight-fürtök parancsfájlművelet használatával](hdinsight-hadoop-customize-cluster-linux.md#troubleshooting) a más módon történő a naplózott információk eléréséhez.

A legtöbb segédprogramok és telepítőcsomagok már írási információk tooSTDOUT és az STDERR, azonban szükség lehet további naplózás tooadd. toosend szöveg tooSTDOUT, használjon `echo`. Példa:

```bash
echo "Getting ready tooinstall Foo"
```

Alapértelmezés szerint `echo` küld hello karakterlánc tooSTDOUT. toodirect azt tooSTDERR, adja hozzá `>&2` előtt `echo`. Példa:

```bash
>&2 echo "An error occurred installing Foo"
```

Ez a tooSTDOUT tooSTDERR (2) helyette írt adatok irányítja át. IO átirányítással kapcsolatos további információkért lásd: [http://www.tldp.org/LDP/abs/html/io-redirection.html](http://www.tldp.org/LDP/abs/html/io-redirection.html).

A Parancsfájlműveletek által naplózott adatok megtekintéséhez használatos időkategóriát további információkért lásd: [testreszabása HDInsight-fürtök parancsfájlművelet használatával](hdinsight-hadoop-customize-cluster-linux.md#troubleshooting)

### <a name="bps8"></a>ASCII LF sorvégződések rendelkező fájlok mentése

Bash parancsfájlok megszakította a LF vonallal ASCII formátumban kell tárolni. UTF-8 tárolják, vagy használja CRLF hello sor befejezése sikertelen lehet a következő hiba hello fájlok:

```
$'\r': command not found
line 1: #!/usr/bin/env: No such file or directory
```

### <a name="bps9"></a>Újrapróbálkozási logika toorecover átmeneti hibák használata

Fájlok letöltése apt get vagy egyéb műveleteket adatátvitelhez csomagok telepítése hello internet, hello művelet tootransient hálózati hibák miatt sikertelenek lehetnek. Például hello távoli erőforrás kommunikációra lehet hello folyamat sikertelen tooa biztonsági mentési csomópont.

toomake a parancsfájlhibák rugalmas tootransient újrapróbálkozási logikát Megvalósíthat. a következő függvény hello bemutatja, hogyan tooimplement újrapróbálkozási logika. Újbóli próbálkozások háromszor hello műveletet.

```bash
#retry
MAXATTEMPTS=3

retry() {
    local -r CMD="$@"
    local -i ATTMEPTNUM=1
    local -i RETRYINTERVAL=2

    until $CMD
    do
        if (( ATTMEPTNUM == MAXATTEMPTS ))
        then
                echo "Attempt $ATTMEPTNUM failed. no more attempts left."
                return 1
        else
                echo "Attempt $ATTMEPTNUM failed! Retrying in $RETRYINTERVAL seconds..."
                sleep $(( RETRYINTERVAL ))
                ATTMEPTNUM=$ATTMEPTNUM+1
        fi
    done
}
```

hello alábbi példák bemutatják, hogyan toouse ezt a funkciót.

```bash
retry ls -ltr foo

retry wget -O ./tmpfile.sh https://hdiconfigactions.blob.core.windows.net/linuxhueconfigactionv02/install-hue-uber-v02.sh
```

## <a name="helpermethods"></a>Egyéni parancsfájlok segítő módszerei

Parancsfájl művelet segítő módszereket segédprogramok egyéni parancsfájlok írása közben használható. Ezek a módszerek tartalmazza a[https://hdiconfigactions.blob.core.windows.net/linuxconfigactionmodulev01/HDInsightUtilities-v01.sh](https://hdiconfigactions.blob.core.windows.net/linuxconfigactionmodulev01/HDInsightUtilities-v01.sh) parancsfájl. A következő toodownload hello használata, és felhasználja a parancsfájl részeként:

```bash
# Import hello helper method module.
wget -O /tmp/HDInsightUtilities-v01.sh -q https://hdiconfigactions.blob.core.windows.net/linuxconfigactionmodulev01/HDInsightUtilities-v01.sh && source /tmp/HDInsightUtilities-v01.sh && rm -f /tmp/HDInsightUtilities-v01.sh
```

a következő parancsfájlban használható segítő hello:

| Segítő kihasználtsága | Leírás |
| --- | --- |
| `download_file SOURCEURL DESTFILEPATH [OVERWRITE]` |Letölti a fájlt a hello forrás URI toohello fájl megadott elérési útja. Alapértelmezés szerint azt a meglévő fájl felülírásának nem. |
| `untar_file TARFILE DESTDIR` |Kibontja a bont (használatával `-xf`) toohello célkönyvtáron. |
| `test_is_headnode` |Ha a fürt átjárócsomópontjához, visszatérési 1; itt futott Ellenkező esetben 0. |
| `test_is_datanode` |Ha hello aktuális csomópont adatcsomóponton (munkavégző), a visszatérési érték egy 1. Ellenkező esetben 0. |
| `test_is_first_datanode` |Ha hello aktuális csomópont hello első adatok (munkavégző) csomópontra (elnevezett workernode0) adja vissza egy 1. Ellenkező esetben 0. |
| `get_headnodes` |Teljesen minősített tartománynevét hello hello headnodes hello fürt adja vissza. Neveket vesszővel tagolt. Üres karakterlánc hibát akkor adja vissza. |
| `get_primary_headnode` |Lekérdezi a teljesen minősített tartománynevét hello hello elsődleges headnode. Üres karakterlánc hibát akkor adja vissza. |
| `get_secondary_headnode` |Lekérdezi a teljesen minősített tartománynevét hello hello másodlagos headnode. Üres karakterlánc hibát akkor adja vissza. |
| `get_primary_headnode_number` |Lekérdezi a hello elsődleges headnode hello-utótagként. Üres karakterlánc hibát akkor adja vissza. |
| `get_secondary_headnode_number` |Lekérdezi a hello másodlagos headnode hello-utótagként. Üres karakterlánc hibát akkor adja vissza. |

## <a name="commonusage"></a>Gyakori használati minták

Ez a szakasz néhány hello gyakori használati szokásokról, amelyek a saját egyéni parancsfájl írásakor mutatjuk be végrehajtási nyújt útmutatást.

### <a name="passing-parameters-tooa-script"></a>Paraméterek átadása tooa parancsfájl

Bizonyos esetekben a parancsfájl paramétereit lehet szükség. Például szükség lehet hello rendszergazdai jelszó hello fürt hello Ambari REST API használata esetén.

Toohello parancsfájl átadott paraméterek nevezik *pozícióparaméterek*, és túl rendelt`$1` hello első paraméternek `$2` a második, és így a hello. `$0`hello parancsfájl önmagában hello nevét tartalmazza.

Értékek toohello parancsfájl paraméterként szimpla idézőjelben (') közé kell tenni. Ezzel biztosítja, hogy az átadott érték hello literálként kell kezelni.

### <a name="setting-environment-variables"></a>Környezeti változók beállítása

Egy környezeti változó beállítása a következő utasítás hello végzi el:

    VARIABLENAME=value

Ahol VARIABLENAME az hello hello változó neve. tooaccess hello változó, használjon `$VARIABLENAME`. Például tooassign egy pozícióparaméter, egy környezeti változó által megadott nevű jelszó, a következő utasítás hello szeretné használni:

    PASSWORD=$1

További hozzáférési toohello adatait aztán használhatja `$PASSWORD`.

Környezeti változók hello parancsfájl belül csak hello parancsfájl hello hatókörén belül található. Egyes esetekben szükség lehet a tooadd rendszerszintű környezeti változókat, amelyek hello parancsfájl áll fenn. tooadd rendszerszintű környezeti változókat, adjon hozzá hello változó túl`/etc/environment`. Például a következő utasítás hello létrehozza `HADOOP_CONF_DIR`:

```bash
echo "HADOOP_CONF_DIR=/etc/hadoop/conf" | sudo tee -a /etc/environment
```

### <a name="access-toolocations-where-hello-custom-scripts-are-stored"></a>Hozzáférés toolocations hello egyéni parancsfájlok tároló

Használt toocustomize fürt kell toobe tárolva hello alábbi helyek egyikét:

* Egy __Azure Storage-fiók__ hello fürt társított.

* Egy __további tárfiók__ hello-fürthöz tartozó.

* A __nyilvánosan olvasható URI__. Például egy URL-cím toodata a onedrive vállalati verzió, a Dropbox vagy más szolgáltatást tartalmazó fájl tárolja.

* Egy __Azure Data Lake Store-fiók__ hello HDInsight-fürthöz társított. Az Azure Data Lake Store és a HDInsight együttes használatával további információkért lásd: [HDInsight-fürtök létrehozása a Data Lake Store](../data-lake-store/data-lake-store-hdinsight-hadoop-use-portal.md).

    > [!NOTE]
    > hello szolgáltatás egyszerű HDInsight használ tooaccess Data Lake Store toohello parancsfájl olvasási hozzáféréssel kell rendelkeznie.

Hello parancsfájlhoz használt erőforrásokhoz is nyilvánosan hozzáférhetőnek kell lenniük.

Hello fájlok tárolására az Azure Storage-fiók vagy az Azure Data Lake Store gyors hozzáférést, belül hello Azure-hálózatot is biztosít.

> [!NOTE]
> hello URI formátumának tooreference hello parancsfájl abban különbözik attól függően, hogy hello szolgáltatást használja. Hello HDInsight-fürthöz társított tárfiókokat, használjon `wasb://` vagy `wasbs://`. Nyilvánosan olvasható URI-k használata `http://` vagy `https://`. A Data Lake Store, használjon `adl://`.

### <a name="checking-hello-operating-system-version"></a>Hello operációs rendszer verziójának ellenőrzése

HDInsight különböző verzióinak Ubuntu verzióról támaszkodnak. Előfordulhat, hogy ki kell választani az parancsfájlban operációsrendszer-verziók közötti különbséget. Ubuntu kapcsolt toohello verziója bináris tooinstall például szükség lehet.

toocheck hello az operációs rendszer verziója, használjon `lsb_release`. Például a következő parancsfájl hello bemutatja, hogyan tooreference egy adott bont fájl attól függően, hogy az operációs rendszer verziója hello:

```bash
OS_VERSION=$(lsb_release -sr)
if [[ $OS_VERSION == 14* ]]; then
    echo "OS verion is $OS_VERSION. Using hue-binaries-14-04."
    HUE_TARFILE=hue-binaries-14-04.tgz
elif [[ $OS_VERSION == 16* ]]; then
    echo "OS verion is $OS_VERSION. Using hue-binaries-16-04."
    HUE_TARFILE=hue-binaries-16-04.tgz
fi
```

## <a name="deployScript"></a>Ellenőrzőlista a üzembe helyezéséhez egy parancsfájlművelet

Az alábbiakban azt tartott, ezek a parancsfájlok toodeploy előkészítésekor hello lépéseket:

* Hello egyéni parancsfájlok, amely elérhető a hello fürtcsomópontok üzembe helyezése során helyen tartalmazó hello fájlokat helyezze el. Például hello alapértelmezett hello fürt tárolóhelyét. Fájlok nyilvánosan olvasható üzemeltetési szolgáltatásokat is tárolhatja.
* Győződjön meg arról, hogy impotent hello parancsfájl. Így lehetővé teszi, hogy hello parancsfájl toobe végre több alkalommal hello azonos csomópont.
* Használjon egy ideiglenes könyvtár /tmp tookeep hello hello parancsfájlok által használt fájlokat, majd eltávolítással parancsfájlok rendelkezik végrehajtása után.
* Ha az operációs rendszer szintű beállításokat és a Hadoop szolgáltatás konfigurációs fájlokat, módosulnak, érdemes lehet toorestart HDInsight szolgáltatások.

## <a name="runScriptAction"></a>Hogyan toorun egy parancsfájlművelet

Parancsfájl műveletek toocustomize HDInsight-fürtök hello a következő módszerek használhatók:

* Azure Portal
* Azure PowerShell
* Azure Resource Manager-sablonok
* hello HDInsight .NET SDK-val.

További információ az egyes módszerek használatával, lásd: [hogyan toouse parancsfájl művelet](hdinsight-hadoop-customize-cluster-linux.md).

## <a name="sampleScripts"></a>Egyéni parancsfájl-minták

Microsoft tooinstall összetevők Ez a minta parancsfájlok HDInsight-fürtöt. Tekintse meg a következő hivatkozások további példa Parancsfájlműveletek hello.

* [Telepítheti és használhatja a HDInsight-fürtök Hue](hdinsight-hadoop-hue-linux.md)
* [Telepítheti és használhatja a HDInsight-fürtök Solr](hdinsight-hadoop-solr-install-linux.md)
* [Telepítheti és használhatja a HDInsight-fürtök Giraph](hdinsight-hadoop-giraph-install-linux.md)
* [Telepítéséhez vagy frissítéséhez monó a HDInsight-fürtökön](hdinsight-hadoop-install-mono.md)

## <a name="troubleshooting"></a>Hibaelhárítás

Az alábbiakban hello szert parancsfájlok használata során felmerülő hibák:

**Hiba**: `$'\r': command not found`. Egyes esetekben követ `syntax error: unexpected end of file`.

*OK*: ezt a hibát az okozza, ha a parancsfájl hello sorainak CRLF végződik. UNIX rendszerek csak LF hello sor befejezési, várt.

Ez a probléma leggyakrabban esetén hello parancsfájl állította össze a Windows-környezetben, mert CRLF egy közös sor a Windows sok szöveg szerkesztők befejezési.

*Megoldási*: Ha egy beállítást a szövegszerkesztőben, Unix-formátum vagy kiválasztása LF hello sor befejezése. A Unix rendszer toochange hello CRLF tooan LF parancsok következő hello is használhatja:

> [!NOTE]
> hello következő parancsok abban nagyjából megfelel, hogy azok hello CRLF sor végződések javítása tooLF kell módosítani. Válasszon ki egy, a rendszer hello segédprogramok alapján.

| Parancs | Megjegyzések |
| --- | --- |
| `unix2dos -b INFILE` |hello eredeti fájlt a biztonsági mentése egy. BAK bővítmény |
| `tr -d '\r' < INFILE > OUTFILE` |EREDMÉNYFÁJL csak LF végződések javítása rendelkező verzióját tartalmazza. |
| `perl -pi -e 's/\r\n/\n/g' INFILE` | Közvetlenül a hello fájl módosítása |
| ```sed 's/$'"/`echo \\\r`/" INFILE > OUTFILE``` |EREDMÉNYFÁJL csak LF végződések javítása rendelkező verzióját tartalmazza. |

**Hiba**: `line 1: #!/usr/bin/env: No such file or directory`.

*OK*: Ez a hiba jelentkezik hello parancsfájl UTF-8 a bájt rendelés be van jelölve (AJ) néven lett mentve.

*Megoldási*: mentés hello fájl mint ASCII vagy UTF-8 AJ nélkül. Hello parancs követően egy Linux vagy Unix rendszer toocreate egy fájl hello AJ nélkül is használhatja:

    awk 'NR==1{sub(/^\xef\xbb\xbf/,"")}{print}' INFILE > OUTFILE

Cserélje le `INFILE` hello AJ tartalmazó hello fájllal. `OUTFILE`egy új fájlnevet, hello parancsfájl nélkül hello AJ tartalmazó kell lennie.

## <a name="seeAlso"></a>Következő lépések

* Ismerje meg, hogyan túl[testreszabása HDInsight-fürtök parancsfájlművelet használatával](hdinsight-hadoop-customize-cluster-linux.md)
* Használjon hello [HDInsight .NET SDK referenciáit](https://msdn.microsoft.com/library/mt271028.aspx) toolearn kezelése a HDInsight .NET-alkalmazások létrehozásával kapcsolatos további
* Használjon hello [HDInsight REST API](https://msdn.microsoft.com/library/azure/mt622197.aspx) hogyan toouse REST tooperform felügyeleti műveletek a HDInsight-fürtök toolearn.
