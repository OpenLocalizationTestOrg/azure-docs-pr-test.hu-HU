---
title: "a Python comopnents - Azure HDInsight alatt futó Storm aaaApache |} Microsoft Docs"
description: "Megtudhatja, hogyan toocreate az Apache Storm-topológia Python-összetevőket."
services: hdinsight
documentationcenter: 
author: Blackmist
manager: jhubbard
editor: cgronlun
keywords: Apache storm python
ms.assetid: edd0ec4f-664d-4266-910c-6ecc94172ad8
ms.service: hdinsight
ms.custom: hdinsightactive,hdiseo17may2017
ms.devlang: python
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 08/04/2017
ms.author: larryfr
ms.openlocfilehash: 143c639623f1992f913900a7c52d6e3f03c701e2
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="develop-apache-storm-topologies-using-python-on-hdinsight"></a>Python használata a HDInsight alatt futó Apache Storm-topológiák fejlesztése

Megtudhatja, hogyan toocreate az Apache Storm-topológia Python-összetevőket. Apache Storm több nyelv használatát, még akkor is lehetővé teszi egy topológia több nyelv toocombine-összetevőt. hello fluxus keretrendszer (Storm 0.10.0-s bevezetett) lehetővé teszi a tooeasily megoldásokat Python-összetevőket használnak.

> [!IMPORTANT]
> a dokumentumban szereplő információk hello teszteltük a HDInsight 3.6 alatt futó Storm használatával. Linux hello azt az egyetlen operációs rendszer, használja a HDInsight 3.4 vagy újabb verziója. További tudnivalókért lásd: [A HDInsight elavulása Windows rendszeren](hdinsight-component-versioning.md#hdinsight-windows-retirement).

Ez a projekt kódját hello érhető el: [https://github.com/Azure-Samples/hdinsight-python-storm-wordcount](https://github.com/Azure-Samples/hdinsight-python-storm-wordcount).

## <a name="prerequisites"></a>Előfeltételek

* Python 2.7 vagy újabb

* Java JDK 1.8 vagy újabb

* 3 maven

* (Választható) A helyi Storm környezet. A helyi Storm-környezet csak akkor szükséges, ha azt szeretné, hogy helyileg toorun hello topológia. További információkért lásd: [a fejlesztési környezet létrehozása](http://storm.apache.org/releases/1.0.1/Setting-up-development-environment.html).

## <a name="storm-multi-language-support"></a>A Storm több nyelv támogatása

Apache Storm lett tervezett toowork összetevőkkel bármely programozási nyelv használatával készítettek. meg kell érteniük a hello összetevői hogyan toowork a hello [Thrift-definíciók alatt futó Storm](https://github.com/apache/storm/blob/master/storm-core/src/storm.thrift). Python a modul hello alatt futó Apache Storm projekt, amely lehetővé teszi a Storm tooeasily felület részeként biztosítja. A modul található [https://github.com/apache/storm/blob/master/storm-multilang/python/src/main/resources/resources/storm.py](https://github.com/apache/storm/blob/master/storm-multilang/python/src/main/resources/resources/storm.py).

A Storm egy Java lefutó folyamat, amely hello Java virtuális gép (JVM). Az egyéb nyelven írt összetevők vannak végre, mert magában. hello Storm ezek magában stdin/stdout keresztüli üzenetküldés JSON használatával kommunikál. További részleteket a összetevői közötti kommunikáció hello található [Multi-lang protokoll](https://storm.apache.org/documentation/Multilang-protocol.html) dokumentációját.

## <a name="python-with-hello-flux-framework"></a>Python hello fluxus keretrendszerrel

hello fluxus keretrendszer lehetővé teszi a Storm-topológiák toodefine elkülönítve hello összetevők. hello fluxus keretrendszer YAM toodefine hello Storm-topológia használja. hello következő szövege a következő: példa bemutatja, hogyan tooreference egy Python-összetevő hello YAM dokumentumban:

```yaml
# Spout definitions
spouts:
  - id: "sentence-spout"
    className: "org.apache.storm.flux.wrappers.spouts.FluxShellSpout"
    constructorArgs:
      # Command line
      - ["python", "sentencespout.py"]
      # Output field(s)
      - ["sentence"]
    # parallelism hint
    parallelism: 1
```

osztály hello `FluxShellSpout` használt toostart hello van `sentencespout.py` hello spout magában foglaló parancsprogram.

Fluxus hello Python parancsfájlok toobe vár hello `/resources` belül hello jar-fájlra, amely tartalmazza a hello topológia könyvtárába. Ezért ez a példa hello Python parancsfájlok tárol hello `/multilang/resources` könyvtár. Hello `pom.xml` tartalmazza ezt a fájlt a következő XML hello használata:

```xml
<!-- include hello Python components -->
<resource>
    <directory>${basedir}/multilang</directory>
    <filtering>false</filtering>
</resource>
```

A korábban említett van egy `storm.py` fájl, amely megvalósítja az alatt futó Storm hello Thrift-definíciók. hello fluxus tartalmazza `storm.py` automatikusan amikor hello projekt épül, így nem kell tooworry kapcsolatban, beleértve azt.

## <a name="build-hello-project"></a>Hello projekt létrehozása

Hello projekt gyökerében hello használja a következő parancs hello:

```bash
mvn clean compile package
```

Ezzel a paranccsal létrejön egy `target/WordCount-1.0-SNAPSHOT.jar` hello tartalmazó fájl fordítása topológia.

## <a name="run-hello-topology-locally"></a>Futtassa helyben a hello topológia

toorun hello topológia helyileg, a következő parancs hello használata:

```bash
storm jar WordCount-1.0-SNAPSHOT.jar org.apache.storm.flux.Flux -l -R /topology.yaml
```

> [!NOTE]
> Ez a parancs a helyi Storm környezet szükséges. További információkért lásd: [a fejlesztési környezet létrehozása](http://storm.apache.org/releases/1.0.1/Setting-up-development-environment.html)

Egyszer hello topológia indul el, akkor bocsát ki adatokat toohello helyi konzol hasonló toohello a következő szöveget:


    24302 [Thread-25-sentence-spout-executor[4 4]] INFO  o.a.s.s.ShellSpout - ShellLog pid:2436, name:sentence-spout Emiting hello cow jumped over hello moon
    24302 [Thread-30] INFO  o.a.s.t.ShellBolt - ShellLog pid:2438, name:splitter-bolt Emitting the
    24302 [Thread-28] INFO  o.a.s.t.ShellBolt - ShellLog pid:2437, name:counter-bolt Emitting years:160
    24302 [Thread-17-log-executor[3 3]] INFO  o.a.s.f.w.b.LogInfoBolt - {word=the, count=599}
    24303 [Thread-17-log-executor[3 3]] INFO  o.a.s.f.w.b.LogInfoBolt - {word=seven, count=302}
    24303 [Thread-17-log-executor[3 3]] INFO  o.a.s.f.w.b.LogInfoBolt - {word=dwarfs, count=143}
    24303 [Thread-25-sentence-spout-executor[4 4]] INFO  o.a.s.s.ShellSpout - ShellLog pid:2436, name:sentence-spout Emiting hello cow jumped over hello moon
    24303 [Thread-30] INFO  o.a.s.t.ShellBolt - ShellLog pid:2438, name:splitter-bolt Emitting cow
    24303 [Thread-17-log-executor[3 3]] INFO  o.a.s.f.w.b.LogInfoBolt - {word=four, count=160}


toostop hello topológia, használjon __Ctrl + C__.

## <a name="run-hello-storm-topology-on-hdinsight"></a>A HDInsight Storm-topológia hello futtatása

1. Használjon hello következő parancsot a toocopy hello `WordCount-1.0-SNAPSHOT.jar` tooyour Storm on HDInsight-fürt fájlt:

    ```bash
    scp target\WordCount-1.0-SNAPSHOT.jar sshuser@mycluster-ssh.azurehdinsight.net
    ```

    Cserélje le `sshuser` hello SSH-felhasználó a fürt számára. Cserélje le `mycluster` hello fürt névvel. Előfordulhat, hogy felszólító tooenter hello hello SSH felhasználó jelszavát.

    További információ az SSH és az SCP használatával, lásd: [az SSH a Hdinsighttal](hdinsight-hadoop-linux-use-ssh-unix.md).

2. Hello fájl feltöltése után csatlakoztassa az SSH használatával toohello fürtöt:

    ```bash
    ssh sshuser@mycluster-ssh.azurehdinsight.net
    ```

3. Hello SSH-munkamenetet, a parancs toostart hello topológia hello fürtön a következő hello használata:

    ```bash
    storm jar WordCount-1.0-SNAPSHOT.jar org.apache.storm.flux.Flux -r -R /topology.yaml
    ```

3. Hello Storm felhasználói felülete tooview hello topológia hello fürtben is használhatja. hello Storm felhasználói felülete https://mycluster.azurehdinsight.net/stormui helyezkedik el. Cserélje le `mycluster` elemet a fürt nevére.

> [!NOTE]
> Miután elindult, a Storm-topológia fut. csak a toostop hello topológia, hello a következő módszerek bármelyikét használhatja:
>
> * Hello `storm kill TOPOLOGYNAME` hello parancssori parancsot
> * Hello **Kill** hello Storm felhasználói felülete gombjára.


## <a name="next-steps"></a>Következő lépések

Tekintse meg a következő dokumentumok más módokon toouse Python és a HDInsight együttes hello:

* [Hogyan toouse Python az adatfolyamként történő MapReduce-feladatok](hdinsight-hadoop-streaming-python.md)
* [Hogyan toouse Python felhasználó definiált függvény (UDF) a Pig és Hive](hdinsight-python.md)
