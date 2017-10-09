---
title: a HDInsight - Azure feladatok streaming MapReduce Python aaaDevelop |} Microsoft Docs
description: "Megtudhatja, hogyan toouse Python MapReduce-feladatok adatfolyam. Hadoop streamelési API biztosít a Java nyelven írt a MapReduce."
services: hdinsight
keyword: mapreduce python,python map reduce,python mapreduce
documentationcenter: 
author: Blackmist
manager: jhubbard
editor: cgronlun
tags: azure-portal
ms.assetid: 7631d8d9-98ae-42ec-b9ec-ee3cf7e57fb3
ms.service: hdinsight
ms.custom: hdinsightactive,hdiseo17may2017
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 07/31/2017
ms.author: larryfr
ms.openlocfilehash: a6ae3ba650b665ecc5839a4ddf5282f8ccfb6bd6
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="develop-python-streaming-mapreduce-programs-for-hdinsight"></a>A HDInsight MapReduce programok streaming Python fejlesztése

Megtudhatja, hogyan toouse Python MapReduce műveletek adatfolyam. Hadoop streamelési API biztosít, amely lehetővé teszi, hogy a toowrite térkép, és csökkentheti a funkciók nem Java nyelven MapReduce. hello jelen dokumentumban leírt lépések végrehajtása hello térkép és csökkentse az összetevők a Python.

## <a name="prerequisites"></a>Előfeltételek

* A Linux-based Hadoop on HDInsight-fürt

  > [!IMPORTANT]
  > hello jelen dokumentumban leírt lépések egy HDInsight-fürt által használt Linux igényelnek. Linux hello azt az egyetlen operációs rendszer, használja a HDInsight 3.4 vagy újabb verziója. További tudnivalókért lásd: [A HDInsight elavulása Windows rendszeren](hdinsight-component-versioning.md#hdinsight-windows-retirement).

* Egy szövegszerkesztőben

  > [!IMPORTANT]
  > hello szövegszerkesztőben LF hello sor befejezési karakterként használjon. Egy sor vége CRLF használata okoz hibák hello MapReduce feladatot futó Linux-alapú HDInsight-fürtökön.

* Hello `ssh` és `scp` parancsok, vagy [Azure PowerShell](https://docs.microsoft.com/powershell/azure/overview?view=azurermps-3.8.0)

## <a name="word-count"></a>Word száma

Ebben a példában az alapvető szószámot python megvalósítva, a hozzárendelést és nyomáscsökkentő. hello leképező mondat bontja egyes szavak és hello nyomáscsökkentő hello szavak összesíti és tooproduce hello kimeneti száma.

hello a következő folyamatábra bemutatja, mi történik a hello leképezés során, és csökkentheti a fázisok.

![hello mapreduce folyamat ábrája](./media/hdinsight-hadoop-streaming-python/HDI.WordCountDiagram.png)

## <a name="streaming-mapreduce"></a>Adatfolyam-továbbítási MapReduce

Hadoop lehetővé teszi egy fájlt, amely hello térkép tartalmaz, és csökkenti a feladat által használt logikai toospecify. konkrét követelmények hello hello rendelve, és csökkentse a logikai vannak:

* **Bemeneti**: hello térkép, így csökkentheti összetevők bemeneti adatokat STDIN kell olvasni.
* **Kimeneti**: hello térkép, így csökkentheti összetevők kell írnia a kimeneti adatok tooSTDOUT.
* **Az adatformátum**: hello adatok felhasznált és előállított egy kulcs/érték pár, karakterrel elválasztó karakterláncként lapon kell lennie.

Python könnyen kezelhető követelménynek hello segítségével `sys` modul tooread STDIN és használatával `print` tooprint tooSTDOUT. hello fennmaradó feladat van egyszerűen hello adatok formázása a lap (`\t`) karakter közötti hello kulcs-érték.

## <a name="create-hello-mapper-and-reducer"></a>Hello leképező és nyomáscsökkentő létrehozása

1. Hozzon létre egy fájlt `mapper.py` és hello használja a következő kód hello tartalmat:

   ```python
   #!/usr/bin/env python

   # Use hello sys module
   import sys

   # 'file' in this case is STDIN
   def read_input(file):
       # Split each line into words
       for line in file:
           yield line.split()

   def main(separator='\t'):
       # Read hello data using read_input
       data = read_input(sys.stdin)
       # Process each word returned from read_input
       for words in data:
           # Process each word
           for word in words:
               # Write tooSTDOUT
               print '%s%s%d' % (word, separator, 1)

   if __name__ == "__main__":
       main()
   ```

2. Hozzon létre egy fájlt **reducer.py** és hello használja a következő kód hello tartalmat:

   ```python
   #!/usr/bin/env python

   # import modules
   from itertools import groupby
   from operator import itemgetter
   import sys

   # 'file' in this case is STDIN
   def read_mapper_output(file, separator='\t'):
       # Go through each line
       for line in file:
           # Strip out hello separator character
           yield line.rstrip().split(separator, 1)

   def main(separator='\t'):
       # Read hello data using read_mapper_output
       data = read_mapper_output(sys.stdin, separator=separator)
       # Group words and counts into 'group'
       #   Since MapReduce is a distributed process, each word
       #   may have multiple counts. 'group' will have all counts
       #   which can be retrieved using hello word as hello key.
       for current_word, group in groupby(data, itemgetter(0)):
           try:
               # For each word, pull hello count(s) for hello word
               #   from 'group' and create a total count
               total_count = sum(int(count) for current_word, count in group)
               # Write toostdout
               print "%s%s%d" % (current_word, separator, total_count)
           except ValueError:
               # Count was not a number, so do nothing
               pass

   if __name__ == "__main__":
       main()
   ```

## <a name="run-using-powershell"></a>Futtatás a PowerShell használatával

tooensure, hogy az megfelelő hello jobb sorvégeket, a következő PowerShell-parancsfájl használata hello:

[!code-powershell[main](../../powershell_scripts/hdinsight/streaming-python/streaming-python.ps1?range=138-140)]

Használja a következő PowerShell parancsfájl tooupload hello fájlok hello hello feladat futtatása és hello kimeneti megtekintése:

[!code-powershell[main](../../powershell_scripts/hdinsight/streaming-python/streaming-python.ps1?range=5-134)]

## <a name="run-from-an-ssh-session"></a>Az SSH-munkamenetet futtatása

1. A fejlesztési környezetet az a hello azonos könyvtárhoz, mint a `mapper.py` és `reducer.py` fájlok, a következő parancs hello használata:

    ```bash
    scp mapper.py reducer.py username@clustername-ssh.azurehdinsight.net:
    ```

    Cserélje le `username` a hello SSH-felhasználónév a fürtöt, és `clustername` hello néven a fürt.

    Ez a parancs hello helyi rendszer toohello átjárócsomópont hello fájlokat másolja át.

    > [!NOTE]
    > Ha a jelszó toosecure SSH-fiókját, hello jelszót kéri. Ha SSH-kulcsot használt, előfordulhat, hogy toouse hello `-i` paraméter és hello elérési személyes toohello-kulcsot. Például: `scp -i /path/to/private/key mapper.py reducer.py username@clustername-ssh.azurehdinsight.net:`.

2. Csatlakozás toohello fürt SSH segítségével:

    ```bash
    ssh username@clustername-ssh.azurehdinsight.net`
    ```

    További információkért lásd: [az SSH a Hdinsighttal](hdinsight-hadoop-linux-use-ssh-unix.md).

3. tooensure hello mapper.py és reducer.py rendelkezik hello sorvégződések javítsa ki, a következő parancsok hello használata:

    ```bash
    perl -pi -e 's/\r\n/\n/g' mapper.py
    perl -pi -e 's/\r\n/\n/g' reducer.py
    ```

4. A következő parancs toostart hello MapReduce feladatot hello használata.

    ```bash
    yarn jar /usr/hdp/current/hadoop-mapreduce-client/hadoop-streaming.jar -files mapper.py,reducer.py -mapper mapper.py -reducer reducer.py -input /example/data/gutenberg/davinci.txt -output /example/wordcountout
    ```

    Ez a parancs a következő részek hello rendelkezik:

   * **hadoop-streaming.jar**: használt adatfolyam-továbbítási MapReduce műveletek végrehajtása során. Az illesztők Hadoop kóddal hello külső MapReduce megadnia.

   * **-fájlok**: hozzáadja a megadott hello fájlok toohello MapReduce feladatot.

   * **-leképező**: toouse fájl leképező hello, amely közli Hadoop.

   * **-Nyomáscsökkentő**: közli Hadoop, amely toouse fájlt, mert nyomáscsökkentő hello.

   * **-bemeneti**: hello bemeneti fájl, amely azt számolja a jelenti.

   * **-kimeneti**: hello hello kimeneti könyvtár nevével.

    Hello MapReduce feladatot működik, mint hello folyamat százalékként jelenik meg.

        15-02-05 19:01:04 információ mapreduce. Feladat: a térkép 0 % csökkentheti a 0 % 15-02-05 19:01:16 információ mapreduce. Feladat: a térkép 100 %-os csökkentése 0 % 15-02-05 19:01:27 információ mapreduce. Feladat: a térkép 100 %-os csökkentheti a 100 %-os


5. tooview hello kimeneti, használja a következő parancs hello:

    ```bash
    hdfs dfs -text /example/wordcountout/part-00000
    ```

    Ez a parancs megjeleníti szavak és hányszor hello word történt.

## <a name="next-steps"></a>Következő lépések

Most, hogy megtanulta, hogyan streaming MapRedcue toouse feladatok a hdinsight eszközzel, használja a következő hivatkozások tooexplore hello más módokon toowork Azure HDInsight.

* [A Hive használata a HDInsightban](hdinsight-use-hive.md)
* [A Pig használata a HDInsightban](hdinsight-use-pig.md)
* [MapReduce-feladatok használata a HDInsightban](hdinsight-use-mapreduce.md)
