---
title: a HDInsight - Azure hadoop MapReduce C# aaaUse |} Microsoft Docs
description: "Megtudhatja, hogyan toouse C# toocreate MapReduce-megoldások Azure HDInsight Hadoop."
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
ms.openlocfilehash: dd8b684e74155bc1a37d4ab8d6f9033276ef5aa3
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="use-c-with-mapreduce-streaming-on-hadoop-in-hdinsight"></a>C# használata a hdinsight Hadoop streamelési MapReduce

Megtudhatja, hogyan toouse C# toocreate hdinsight MapReduce megoldást.

> [!IMPORTANT]
> Linux hello azt az egyetlen operációs rendszer, használja a HDInsight 3.4 vagy újabb verziója. További információkért lásd: [HDInsight-összetevők verziószámozása](hdinsight-component-versioning.md).

Hadoop streamelési egy segédprogram, amely lehetővé teszi egy parancsfájl vagy végrehajtható fájllal toorun MapReduce-feladatok. Ebben a példában a .NET használt tooimplement hello hozzárendelést és a word-count megoldások nyomáscsökkentő.

## <a name="net-on-hdinsight"></a>A HDInsight .NET

__Linux-alapú HDInsight__ fürtök használata [monó (https://mono-project.com)](https://mono-project.com) toorun .NET-alkalmazások. Monó verzió 4.2.1 megtalálható HDInsight 3.5-ös verziója. Monó részét képező HDInsight hello verzióján további információkért lásd: [HDInsight összetevő verziók](hdinsight-component-versioning.md). Monó, egy adott verziójához toouse lásd: hello [telepítés vagy frissítés monó](hdinsight-hadoop-install-mono.md) dokumentum.

A .NET-keretrendszer verzióival is kompatibilisek monó további információkért lásd: [monó kompatibilitási](http://www.mono-project.com/docs/about-mono/compatibility/).

## <a name="how-hadoop-streaming-works"></a>Hadoop streamelési működése

hello a használt adatfolyam-ebben a dokumentumban folyamat alapvetően a következőképpen történik:

1. Hadoop STDIN adatleképező toohello (ebben a példában mapper.exe) továbbítja.
2. hello leképező hello adatokat dolgozza fel, és tabulátorral tagolt kulcs/érték párok tooSTDOUT bocsát ki.
3. hello kimeneti Hadoop olvasni, és ezután átadja STDIN toohello nyomáscsökkentő (reducer.exe ebben a példában).
4. hello nyomáscsökkentő tabulátorral tagolt hello kulcs/érték párok beolvassa, feldolgozza a hello adatokat és majd bocsát ki hello eredmény tabulátorral tagolt kulcs/érték párként STDOUT-on.
5. hello kimeneti Hadoop olvashatják és írt toohello kimeneti könyvtár.

A streaming további információkért lásd: [Hadoop Streamelési (https://hadoop.apache.org/docs/r2.7.1/hadoop-streaming/HadoopStreaming.html)](https://hadoop.apache.org/docs/r2.7.1/hadoop-streaming/HadoopStreaming.html).

## <a name="prerequisites"></a>Előfeltételek

* Egy írást, és C#-kódban, amelynek célpontja a .NET-keretrendszer 4.5 felépítése ismeretét. Ez a dokumentum használatát a Visual Studio 2017 hello szükséges lépések.

* Módon tooupload .exe fájlok toohello fürt. hello jelen dokumentumban leírt lépések használata hello Data Lake Tools Visual Studio tooupload hello fájlok tooprimary hello fürt tárolóhelyét.

* Az Azure PowerShell vagy SSH-ügyfél.

* A Hadoop on HDInsight-fürt. A fürtök létrehozásáról további információk: [HDInsight-fürtök létrehozása](hdinsight-provision-clusters.md).

## <a name="create-hello-mapper"></a>Hozzon létre hello leképezője

A Visual Studióban hozzon létre egy új __Konzolalkalmazás__ nevű __leképező__. A következő kód hello alkalmazás hello használata:

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
            //Hadoop passes data toohello mapper on STDIN
            while((line = Console.ReadLine()) != null)
            {
                // We only want words, so strip out punctuation, numbers, etc.
                var onlyText = Regex.Replace(line, @"\.|;|:|,|[0-9]|'", "");
                // Split at whitespace.
                var words = Regex.Matches(onlyText, @"[\w]+");
                // Loop over hello words
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

Hello alkalmazás létrehozása után összeállítani, tooproduce hello `/bin/Debug/mapper.exe` hello projekt fájl.

## <a name="create-hello-reducer"></a>Hello nyomáscsökkentő létrehozása

A Visual Studióban hozzon létre egy új __Konzolalkalmazás__ nevű __nyomáscsökkentő__. A következő kód hello alkalmazás hello használata:

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
                // Get hello word
                string word = sArr[0];
                // Get hello count
                int count = Convert.ToInt32(sArr[1]);

                //Do we already have a count for hello word?
                if(words.ContainsKey(word))
                {
                    //If so, increment hello count
                    words[word] += count;
                } else
                {
                    //Add hello key toohello collection
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

Hello alkalmazás létrehozása után összeállítani, tooproduce hello `/bin/Debug/reducer.exe` hello projekt fájl.

## <a name="upload-toostorage"></a>Toostorage feltöltése

1. A Visual Studióban nyissa meg a **Server Explorer**.

2. Bontsa ki az **Azure** elemet, majd bontsa ki a **HDInsight** elemet.

3. Ha a rendszer kéri, adja meg Azure-előfizetés hitelesítő adatait, és kattintson **bejelentkezés**.

4. Bontsa ki, hogy az alkalmazás kívánja toodeploy hello HDInsight-fürthöz. Hello szöveg bejegyzés __(alapértelmezett Tárfiók)__ szerepel.

    ![Server Explorer hello tárfiók hello fürt megjelenítése](./media/hdinsight-hadoop-hive-pig-udf-dotnet-csharp/storage.png)

    * Ez a bejegyzés bővíthetők, ha az egy __Azure Storage-fiók__ hello fürt alapértelmezett tárolóként. tooview hello fájlok hello alapértelmezett tároló hello fürt, bontsa ki a hello bejegyzést, és kattintson duplán a hello __(alapértelmezett tároló)__.

    * Ez a bejegyzés nem bonthatók ki, ha az __Azure Data Lake Store__ hello fürt hello alapértelmezett tárolóként. tooview hello fájlok hello alapértelmezett tároló hello fürt, kattintson duplán a hello __(alapértelmezett Tárfiók)__ bejegyzés.

5. tooupload hello .exe fájlok, a hello a következő módszerek valamelyikével:

    * Ha használ egy __Azure Storage-fiók__, hello Feltöltés ikonra, és tallózással keressen toohello **bin\debug** hello mappa **leképező** projekt. Végül válassza ki a hello **mapper.exe** fájlt, és kattintson a **Ok**.

        ![Töltse fel az ikon](./media/hdinsight-hadoop-hive-pig-udf-dotnet-csharp/upload.png)
    
    * Ha használ __Azure Data Lake Store__, kattintson a jobb gombbal egy üres területre a hello felsoroló, és válassza __feltöltése__. Végül válassza ki a hello **mapper.exe** fájlt, és kattintson a **nyitott**.

    Egyszer hello __mapper.exe__ feltöltése befejeződött, ismétlődő hello feltöltési folyamat a hello __reducer.exe__ fájlt.

## <a name="run-a-job-using-an-ssh-session"></a>Egy feladat futtatása: SSH-munkamenetet használatával

1. SSH tooconnect toohello HDInsight-fürt használatára. További információ: [Az SSH használata HDInsighttal](hdinsight-hadoop-linux-use-ssh-unix.md).

2. A következő parancs toostart hello MapReduce feladatot hello egyikét használja:

    * Ha használ __Data Lake Store__ alapértelmezett tárolóként:

        ```bash
        yarn jar /usr/hdp/current/hadoop-mapreduce-client/hadoop-streaming.jar -files adl:///mapper.exe,adl:///reducer.exe -mapper mapper.exe -reducer reducer.exe -input /example/data/gutenberg/davinci.txt -output /example/wordcountout
        ```
    
    * Ha használ __Azure Storage__ alapértelmezett tárolóként:

        ```bash
        yarn jar /usr/hdp/current/hadoop-mapreduce-client/hadoop-streaming.jar -files wasb:///mapper.exe,wasb:///reducer.exe -mapper mapper.exe -reducer reducer.exe -input /example/data/gutenberg/davinci.txt -output /example/wordcountout
        ```

    hello a következő lista ismerteti, hogy minden paraméter funkciója:

    * `hadoop-streaming.jar`: hello jar-fájlra, amely a MapReduce funkció streaming hello tartalmazza.
    * `-files`: Hello hozzáadja `mapper.exe` és `reducer.exe` fájlok toothis feladat. Hello `adl:///` vagy `wasb:///` előtt minden fájl hello elérési toohello hello fürt tárolóhelyét alapértelmezett gyökérmappájában.
    * `-mapper`: Adja meg, melyik fájl hello leképező valósítja meg.
    * `-reducer`: Adja meg, melyik fájl hello nyomáscsökkentő valósítja meg.
    * `-input`: hello bemeneti adatai.
    * `-output`: hello kimeneti könyvtár.

3. Amikor hello MapReduce feladat befejeződik, használja a következő tooview hello eredmények hello:

    ```bash
    hdfs dfs -text /example/wordcountout/part-00000
    ```

    hello következő szöveg látható egy példa a parancs által visszaadott hello adatok:

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

A következő PowerShell-parancsfájl toorun MapReduce feladatot hello használata és hello eredmények letöltéséhez.

[!code-powershell[main](../../powershell_scripts/hdinsight/use-csharp-mapreduce/use-csharp-mapreduce.ps1?range=5-87)]

A parancsprogram kéri hello fürt bejelentkezési nevét és jelszavát, valamint hello HDInsight-fürt neve. Miután hello feladat befejeződött, hello eredménye a letöltött toohello `output.txt` fájl hello directory hello parancsfájlban a van futott. hello következő szöveg látható egy példa hello hello adatok `output.txt` fájlt:

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