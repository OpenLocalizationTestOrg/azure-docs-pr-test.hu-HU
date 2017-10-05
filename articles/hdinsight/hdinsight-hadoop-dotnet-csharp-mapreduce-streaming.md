---
title: "C# használata a hdinsight - Azure Hadoop MapReduce |} Microsoft Docs"
description: "Megtudhatja, hogyan használja a C# Azure HDInsight Hadoop MapReduce megoldások létrehozásához."
services: hdinsight
documentationcenter: 
author: Blackmist
manager: jhubbard
editor: cgronlun
tags: azure-portal
ms.assetid: d83def76-12ad-4538-bb8e-3ba3542b7211
ms.custom: hdinsightactive
ms.service: hdinsight
ms.workload: big-data
ms.tgt_pltfrm: na
ms.devlang: dotnet
ms.topic: article
ms.date: 07/12/2017
ms.author: larryfr
ms.openlocfilehash: adb454e56378a800c671614735aec78b6851aeb2
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/03/2017
---
# <a name="use-c-with-mapreduce-streaming-on-hadoop-in-hdinsight"></a>C# használata a hdinsight Hadoop streamelési MapReduce

Ismerje meg, hogyan használható a C# MapReduce megoldás létrehozása a HDInsight.

> [!IMPORTANT]
> A Linux az egyetlen operációs rendszer, amely a HDInsight 3.4-es vagy újabb verziói esetében használható. További információkért lásd: [HDInsight-összetevők verziószámozása](hdinsight-component-versioning.md).

Hadoop streamelési egy segédprogram, amely lehetővé teszi egy parancsfájl vagy végrehajtható fájllal MapReduce-feladatok futtatásához. Ebben a példában a .NET a hozzárendelést és a word-count megoldások nyomáscsökkentő végrehajtásához használatos.

## <a name="net-on-hdinsight"></a>A HDInsight .NET

__Linux-alapú HDInsight__ fürtök használata [monó (https://mono-project.com)](https://mono-project.com) .NET-alkalmazások futtatására. Monó verzió 4.2.1 megtalálható HDInsight 3.5-ös verziója. Monó részét képező HDInsight-verzión további információkért lásd: [HDInsight összetevő verziók](hdinsight-component-versioning.md). Monó adott verzióját használja, tekintse meg a [telepítés vagy frissítés monó](hdinsight-hadoop-install-mono.md) dokumentum.

A .NET-keretrendszer verzióival is kompatibilisek monó további információkért lásd: [monó kompatibilitási](http://www.mono-project.com/docs/about-mono/compatibility/).

## <a name="how-hadoop-streaming-works"></a>Hadoop streamelési működése

Az adatfolyamként történő ebben a dokumentumban használt folyamat alapvetően a következőképpen történik:

1. Hadoop STDIN az adatok továbbítja a leképező (mapper.exe ebben a példában).
2. A hozzárendelést feldolgozza az adatokat, és tabulátorral tagolt kulcs/érték párok az STDOUT bocsát ki.
3. A kimeneti Hadoop olvasni, és ezután átadja a nyomáscsökkentő (ebben a példában reducer.exe) a STDIN.
4. A nyomáscsökkentő tabulátorral tagolt kulcs/érték párok beolvassa, feldolgozza az adatokat, és majd bocsát ki az eredmény tabulátorral tagolt kulcs/érték párként STDOUT-on.
5. A kimeneti Hadoop olvasni és írni a kimeneti könyvtár.

A streaming további információkért lásd: [Hadoop Streamelési (https://hadoop.apache.org/docs/r2.7.1/hadoop-streaming/HadoopStreaming.html)](https://hadoop.apache.org/docs/r2.7.1/hadoop-streaming/HadoopStreaming.html).

## <a name="prerequisites"></a>Előfeltételek

* Egy írást, és C#-kódban, amelynek célpontja a .NET-keretrendszer 4.5 felépítése ismeretét. A jelen dokumentumban leírt lépések a Visual Studio 2017 használja.

* A fürt .exe fájlok feltöltése módja. A jelen dokumentumban leírt lépések a Data Lake Tools for Visual Studio használatával a fájlok feltöltése a fürt elsődleges tárhelyére.

* Az Azure PowerShell vagy SSH-ügyfél.

* A Hadoop on HDInsight-fürt. A fürtök létrehozásáról további információk: [HDInsight-fürtök létrehozása](hdinsight-provision-clusters.md).

## <a name="create-the-mapper"></a>Hozzon létre a leképezője

A Visual Studióban hozzon létre egy új __Konzolalkalmazás__ nevű __leképező__. Az alkalmazás a következő kódot használhatja:

```csharp
using System;
using System.Text.RegularExpressions;

namespace mapper
{
    class Program
    {
        static void Main(string[] args)
        {
            string line;
            //Hadoop passes data to the mapper on STDIN
            while((line = Console.ReadLine()) != null)
            {
                // We only want words, so strip out punctuation, numbers, etc.
                var onlyText = Regex.Replace(line, @"\.|;|:|,|[0-9]|'", "");
                // Split at whitespace.
                var words = Regex.Matches(onlyText, @"[\w]+");
                // Loop over the words
                foreach(var word in words)
                {
                    //Emit tab-delimited key/value pairs.
                    //In this case, a word and a count of 1.
                    Console.WriteLine("{0}\t1",word);
                }
            }
        }
    }
}
```

Miután létrehozta az alkalmazást, összeállítani, eredményezett a `/bin/Debug/mapper.exe` fájlt a projekt könyvtárában.

## <a name="create-the-reducer"></a>A nyomáscsökkentő létrehozása

A Visual Studióban hozzon létre egy új __Konzolalkalmazás__ nevű __nyomáscsökkentő__. Az alkalmazás a következő kódot használhatja:

```csharp
using System;
using System.Collections.Generic;

namespace reducer
{
    class Program
    {
        static void Main(string[] args)
        {
            //Dictionary for holding a count of words
            Dictionary<string, int> words = new Dictionary<string, int>();

            string line;
            //Read from STDIN
            while ((line = Console.ReadLine()) != null)
            {
                // Data from Hadoop is tab-delimited key/value pairs
                var sArr = line.Split('\t');
                // Get the word
                string word = sArr[0];
                // Get the count
                int count = Convert.ToInt32(sArr[1]);

                //Do we already have a count for the word?
                if(words.ContainsKey(word))
                {
                    //If so, increment the count
                    words[word] += count;
                } else
                {
                    //Add the key to the collection
                    words.Add(word, count);
                }
            }
            //Finally, emit each word and count
            foreach (var word in words)
            {
                //Emit tab-delimited key/value pairs.
                //In this case, a word and a count of 1.
                Console.WriteLine("{0}\t{1}", word.Key, word.Value);
            }
        }
    }
}
```

Miután létrehozta az alkalmazást, összeállítani, eredményezett a `/bin/Debug/reducer.exe` fájlt a projekt könyvtárában.

## <a name="upload-to-storage"></a>Töltse fel a tároló

1. A Visual Studióban nyissa meg a **Server Explorer**.

2. Bontsa ki az **Azure** elemet, majd bontsa ki a **HDInsight** elemet.

3. Ha a rendszer kéri, adja meg Azure-előfizetés hitelesítő adatait, és kattintson **bejelentkezés**.

4. Bontsa ki a kívánja telepíteni az alkalmazást a HDInsight-fürthöz. A szöveg bejegyzés __(alapértelmezett Tárfiók)__ szerepel.

    ![A tárfiók a fürt megjelenítő Server Explorer](./media/hdinsight-hadoop-hive-pig-udf-dotnet-csharp/storage.png)

    * Ez a bejegyzés bővíthetők, ha az egy __Azure Storage-fiók__ a fürt alapértelmezett tárolóként. A fürt az alapértelmezett tároló a fájlok megtekintéséhez bontsa ki a bejegyzést, és majd kattintson duplán a __(alapértelmezett tároló)__.

    * Ez a bejegyzés nem bonthatók ki, ha az __Azure Data Lake Store__ a fürt alapértelmezett tárolóként. A fürt az alapértelmezett tároló a fájlok megtekintéséhez kattintson duplán a __(alapértelmezett Tárfiók)__ bejegyzés.

5. Az .exe fájlok feltöltéséhez használja a következő módszerek egyikét:

    * Ha használ egy __Azure Storage-fiók__, kattintson a Feltöltés ikonra, és keresse a **bin\debug** mappát a **leképező** projekt. Végül válassza ki a **mapper.exe** fájlt, és kattintson a **Ok**.

        ![Töltse fel az ikon](./media/hdinsight-hadoop-hive-pig-udf-dotnet-csharp/upload.png)
    
    * Ha használ __Azure Data Lake Store__, kattintson a jobb gombbal egy üres területre a fájl az átjáróra a listában, majd válassza ki __feltöltése__. Végül válassza ki a **mapper.exe** fájlt, és kattintson a **nyitott**.

    Egyszer a __mapper.exe__ befejezte a feltöltést, ismételje meg a feltöltési folyamat számára a __reducer.exe__ fájlt.

## <a name="run-a-job-using-an-ssh-session"></a>Egy feladat futtatása: SSH-munkamenetet használatával

1. Az SSH használata a HDInsight-fürthöz való csatlakozáshoz. További információ: [Az SSH használata HDInsighttal](hdinsight-hadoop-linux-use-ssh-unix.md).

2. A következő parancs használatával indítsa el a MapReduce feladatot:

    * Ha használ __Data Lake Store__ alapértelmezett tárolóként:

        ```bash
        yarn jar /usr/hdp/current/hadoop-mapreduce-client/hadoop-streaming.jar -files adl:///mapper.exe,adl:///reducer.exe -mapper mapper.exe -reducer reducer.exe -input /example/data/gutenberg/davinci.txt -output /example/wordcountout
        ```
    
    * Ha használ __Azure Storage__ alapértelmezett tárolóként:

        ```bash
        yarn jar /usr/hdp/current/hadoop-mapreduce-client/hadoop-streaming.jar -files wasb:///mapper.exe,wasb:///reducer.exe -mapper mapper.exe -reducer reducer.exe -input /example/data/gutenberg/davinci.txt -output /example/wordcountout
        ```

    Az alábbi lista ismerteti, hogy minden paraméter funkciója:

    * `hadoop-streaming.jar`: A jar-fájlra, amely tartalmazza az adatfolyam-továbbítási MapReduce-funkciókat.
    * `-files`: Hozzáadja a `mapper.exe` és `reducer.exe` fájlok ebben a projektben. A `adl:///` vagy `wasb:///` előtt minden fájl elérési útját a fürt tárolóhelyét alapértelmezett gyökérmappájában.
    * `-mapper`: Meghatározza, hogy melyik fájlt a leképező valósítja meg.
    * `-reducer`: Meghatározza, hogy melyik fájlt a nyomáscsökkentő valósítja meg.
    * `-input`: A bemeneti adatok.
    * `-output`: A kimeneti könyvtár.

3. Miután befejeződött a MapReduce feladatot, használja az eredmények megtekintéséhez a következő:

    ```bash
    hdfs dfs -text /example/wordcountout/part-00000
    ```

    A következő szöveget a parancs által visszaadott adatok példája:

        you     1128
        young   38
        younger 1
        youngest        1
        your    338
        yours   4
        yourself        34
        yourselves      3
        youth   17

## <a name="run-a-job-using-powershell"></a>Egy feladat futtatása: PowerShell-lel

A következő PowerShell-parancsfájl segítségével MapReduce feladatot futtatni, és töltse le az eredményeket.

[!code-powershell[fő](../../powershell_scripts/hdinsight/use-csharp-mapreduce/use-csharp-mapreduce.ps1?range=5-87)]

A parancsprogram kéri a fürt bejelentkezési nevét és jelszavát, és a HDInsight-fürt nevét. Ha a feladat befejeződik, a rendszer letölti a kimenet a `output.txt` fájl a könyvtárban, a parancsfájl futtatása a. A következő szöveg látható egy példa az adatokat a `output.txt` fájlt:

    you     1128
    young   38
    younger 1
    youngest        1
    your    338
    yours   4
    yourself        34
    yourselves      3
    youth   17

## <a name="next-steps"></a>Következő lépések

A MapReduce használata a hdinsight eszközzel további információkért lásd: [és a HDInsight együttes használata MapReduce](hdinsight-use-mapreduce.md).

A C# és Hive és a Pig használatával további információkért lásd: [egy C# felhasználó által definiált függvény használata a Hive és a Pig](hdinsight-hadoop-hive-pig-udf-dotnet-csharp.md).

A HDInsight alatt futó Storm a C# használatával további információkért lásd: [fejlesztésére C#-topológiák HDInsight alatt futó Storm](hdinsight-storm-develop-csharp-visual-studio-topology.md).