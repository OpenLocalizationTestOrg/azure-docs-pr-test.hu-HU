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
# <a name="use-c-with-mapreduce-streaming-on-hadoop-in-hdinsight"></a><span data-ttu-id="68343-103">C# használata a hdinsight Hadoop streamelési MapReduce</span><span class="sxs-lookup"><span data-stu-id="68343-103">Use C# with MapReduce streaming on Hadoop in HDInsight</span></span>

<span data-ttu-id="68343-104">Ismerje meg, hogyan használható a C# MapReduce megoldás létrehozása a HDInsight.</span><span class="sxs-lookup"><span data-stu-id="68343-104">Learn how to use C# to create a MapReduce solution on HDInsight.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="68343-105">A Linux az egyetlen operációs rendszer, amely a HDInsight 3.4-es vagy újabb verziói esetében használható.</span><span class="sxs-lookup"><span data-stu-id="68343-105">Linux is the only operating system used on HDInsight version 3.4 or greater.</span></span> <span data-ttu-id="68343-106">További információkért lásd: [HDInsight-összetevők verziószámozása](hdinsight-component-versioning.md).</span><span class="sxs-lookup"><span data-stu-id="68343-106">For more information, see [HDInsight component versioning](hdinsight-component-versioning.md).</span></span>

<span data-ttu-id="68343-107">Hadoop streamelési egy segédprogram, amely lehetővé teszi egy parancsfájl vagy végrehajtható fájllal MapReduce-feladatok futtatásához.</span><span class="sxs-lookup"><span data-stu-id="68343-107">Hadoop streaming is a utility that allows you to run MapReduce jobs using a script or executable.</span></span> <span data-ttu-id="68343-108">Ebben a példában a .NET a hozzárendelést és a word-count megoldások nyomáscsökkentő végrehajtásához használatos.</span><span class="sxs-lookup"><span data-stu-id="68343-108">In this example, .NET is used to implement the mapper and reducer for a word count solution.</span></span>

## <a name="net-on-hdinsight"></a><span data-ttu-id="68343-109">A HDInsight .NET</span><span class="sxs-lookup"><span data-stu-id="68343-109">.NET on HDInsight</span></span>

<span data-ttu-id="68343-110">__Linux-alapú HDInsight__ fürtök használata [monó (https://mono-project.com)](https://mono-project.com) .NET-alkalmazások futtatására.</span><span class="sxs-lookup"><span data-stu-id="68343-110">__Linux-based HDInsight__ clusters use [Mono (https://mono-project.com)](https://mono-project.com) to run .NET applications.</span></span> <span data-ttu-id="68343-111">Monó verzió 4.2.1 megtalálható HDInsight 3.5-ös verziója.</span><span class="sxs-lookup"><span data-stu-id="68343-111">Mono version 4.2.1 is included with HDInsight version 3.5.</span></span> <span data-ttu-id="68343-112">Monó részét képező HDInsight-verzión további információkért lásd: [HDInsight összetevő verziók](hdinsight-component-versioning.md).</span><span class="sxs-lookup"><span data-stu-id="68343-112">For more information on the version of Mono included with HDInsight, see [HDInsight component versions](hdinsight-component-versioning.md).</span></span> <span data-ttu-id="68343-113">Monó adott verzióját használja, tekintse meg a [telepítés vagy frissítés monó](hdinsight-hadoop-install-mono.md) dokumentum.</span><span class="sxs-lookup"><span data-stu-id="68343-113">To use a specific version of Mono, see the [Install or update Mono](hdinsight-hadoop-install-mono.md) document.</span></span>

<span data-ttu-id="68343-114">A .NET-keretrendszer verzióival is kompatibilisek monó további információkért lásd: [monó kompatibilitási](http://www.mono-project.com/docs/about-mono/compatibility/).</span><span class="sxs-lookup"><span data-stu-id="68343-114">For more information on Mono compatibility with .NET Framework versions, see [Mono compatibility](http://www.mono-project.com/docs/about-mono/compatibility/).</span></span>

## <a name="how-hadoop-streaming-works"></a><span data-ttu-id="68343-115">Hadoop streamelési működése</span><span class="sxs-lookup"><span data-stu-id="68343-115">How Hadoop streaming works</span></span>

<span data-ttu-id="68343-116">Az adatfolyamként történő ebben a dokumentumban használt folyamat alapvetően a következőképpen történik:</span><span class="sxs-lookup"><span data-stu-id="68343-116">The basic process used for streaming in this document is as follows:</span></span>

1. <span data-ttu-id="68343-117">Hadoop STDIN az adatok továbbítja a leképező (mapper.exe ebben a példában).</span><span class="sxs-lookup"><span data-stu-id="68343-117">Hadoop passes data to the mapper (mapper.exe in this example) on STDIN.</span></span>
2. <span data-ttu-id="68343-118">A hozzárendelést feldolgozza az adatokat, és tabulátorral tagolt kulcs/érték párok az STDOUT bocsát ki.</span><span class="sxs-lookup"><span data-stu-id="68343-118">The mapper processes the data, and emits tab-delimited key/value pairs to STDOUT.</span></span>
3. <span data-ttu-id="68343-119">A kimeneti Hadoop olvasni, és ezután átadja a nyomáscsökkentő (ebben a példában reducer.exe) a STDIN.</span><span class="sxs-lookup"><span data-stu-id="68343-119">The output is read by Hadoop, and then passed to the reducer (reducer.exe in this example) on STDIN.</span></span>
4. <span data-ttu-id="68343-120">A nyomáscsökkentő tabulátorral tagolt kulcs/érték párok beolvassa, feldolgozza az adatokat, és majd bocsát ki az eredmény tabulátorral tagolt kulcs/érték párként STDOUT-on.</span><span class="sxs-lookup"><span data-stu-id="68343-120">The reducer reads the tab-delimited key/value pairs, processes the data, and then emits the result as tab-delimited key/value pairs on STDOUT.</span></span>
5. <span data-ttu-id="68343-121">A kimeneti Hadoop olvasni és írni a kimeneti könyvtár.</span><span class="sxs-lookup"><span data-stu-id="68343-121">The output is read by Hadoop and written to the output directory.</span></span>

<span data-ttu-id="68343-122">A streaming további információkért lásd: [Hadoop Streamelési (https://hadoop.apache.org/docs/r2.7.1/hadoop-streaming/HadoopStreaming.html)](https://hadoop.apache.org/docs/r2.7.1/hadoop-streaming/HadoopStreaming.html).</span><span class="sxs-lookup"><span data-stu-id="68343-122">For more information on streaming, see [Hadoop Streaming (https://hadoop.apache.org/docs/r2.7.1/hadoop-streaming/HadoopStreaming.html)](https://hadoop.apache.org/docs/r2.7.1/hadoop-streaming/HadoopStreaming.html).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="68343-123">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="68343-123">Prerequisites</span></span>

* <span data-ttu-id="68343-124">Egy írást, és C#-kódban, amelynek célpontja a .NET-keretrendszer 4.5 felépítése ismeretét.</span><span class="sxs-lookup"><span data-stu-id="68343-124">A familiarity with writing and building C# code that targets .NET Framework 4.5.</span></span> <span data-ttu-id="68343-125">A jelen dokumentumban leírt lépések a Visual Studio 2017 használja.</span><span class="sxs-lookup"><span data-stu-id="68343-125">The steps in this document use Visual Studio 2017.</span></span>

* <span data-ttu-id="68343-126">A fürt .exe fájlok feltöltése módja.</span><span class="sxs-lookup"><span data-stu-id="68343-126">A way to upload .exe files to the cluster.</span></span> <span data-ttu-id="68343-127">A jelen dokumentumban leírt lépések a Data Lake Tools for Visual Studio használatával a fájlok feltöltése a fürt elsődleges tárhelyére.</span><span class="sxs-lookup"><span data-stu-id="68343-127">The steps in this document use the Data Lake Tools for Visual Studio to upload the files to primary storage for the cluster.</span></span>

* <span data-ttu-id="68343-128">Az Azure PowerShell vagy SSH-ügyfél.</span><span class="sxs-lookup"><span data-stu-id="68343-128">Azure PowerShell or a SSH client.</span></span>

* <span data-ttu-id="68343-129">A Hadoop on HDInsight-fürt.</span><span class="sxs-lookup"><span data-stu-id="68343-129">A Hadoop on HDInsight cluster.</span></span> <span data-ttu-id="68343-130">A fürtök létrehozásáról további információk: [HDInsight-fürtök létrehozása](hdinsight-provision-clusters.md).</span><span class="sxs-lookup"><span data-stu-id="68343-130">For more information on creating a cluster, see [Create an HDInsight cluster](hdinsight-provision-clusters.md).</span></span>

## <a name="create-the-mapper"></a><span data-ttu-id="68343-131">Hozzon létre a leképezője</span><span class="sxs-lookup"><span data-stu-id="68343-131">Create the mapper</span></span>

<span data-ttu-id="68343-132">A Visual Studióban hozzon létre egy új __Konzolalkalmazás__ nevű __leképező__.</span><span class="sxs-lookup"><span data-stu-id="68343-132">In Visual Studio, create a new __Console application__ named __mapper__.</span></span> <span data-ttu-id="68343-133">Az alkalmazás a következő kódot használhatja:</span><span class="sxs-lookup"><span data-stu-id="68343-133">Use the following code for the application:</span></span>

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

<span data-ttu-id="68343-134">Miután létrehozta az alkalmazást, összeállítani, eredményezett a `/bin/Debug/mapper.exe` fájlt a projekt könyvtárában.</span><span class="sxs-lookup"><span data-stu-id="68343-134">After creating the application, build it to produce the `/bin/Debug/mapper.exe` file in the project directory.</span></span>

## <a name="create-the-reducer"></a><span data-ttu-id="68343-135">A nyomáscsökkentő létrehozása</span><span class="sxs-lookup"><span data-stu-id="68343-135">Create the reducer</span></span>

<span data-ttu-id="68343-136">A Visual Studióban hozzon létre egy új __Konzolalkalmazás__ nevű __nyomáscsökkentő__.</span><span class="sxs-lookup"><span data-stu-id="68343-136">In Visual Studio, create a new __Console application__ named __reducer__.</span></span> <span data-ttu-id="68343-137">Az alkalmazás a következő kódot használhatja:</span><span class="sxs-lookup"><span data-stu-id="68343-137">Use the following code for the application:</span></span>

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

<span data-ttu-id="68343-138">Miután létrehozta az alkalmazást, összeállítani, eredményezett a `/bin/Debug/reducer.exe` fájlt a projekt könyvtárában.</span><span class="sxs-lookup"><span data-stu-id="68343-138">After creating the application, build it to produce the `/bin/Debug/reducer.exe` file in the project directory.</span></span>

## <a name="upload-to-storage"></a><span data-ttu-id="68343-139">Töltse fel a tároló</span><span class="sxs-lookup"><span data-stu-id="68343-139">Upload to storage</span></span>

1. <span data-ttu-id="68343-140">A Visual Studióban nyissa meg a **Server Explorer**.</span><span class="sxs-lookup"><span data-stu-id="68343-140">In Visual Studio, open **Server Explorer**.</span></span>

2. <span data-ttu-id="68343-141">Bontsa ki az **Azure** elemet, majd bontsa ki a **HDInsight** elemet.</span><span class="sxs-lookup"><span data-stu-id="68343-141">Expand **Azure**, and then expand **HDInsight**.</span></span>

3. <span data-ttu-id="68343-142">Ha a rendszer kéri, adja meg Azure-előfizetés hitelesítő adatait, és kattintson **bejelentkezés**.</span><span class="sxs-lookup"><span data-stu-id="68343-142">If prompted, enter your Azure subscription credentials, and then click **Sign In**.</span></span>

4. <span data-ttu-id="68343-143">Bontsa ki a kívánja telepíteni az alkalmazást a HDInsight-fürthöz.</span><span class="sxs-lookup"><span data-stu-id="68343-143">Expand the HDInsight cluster that you wish to deploy this application to.</span></span> <span data-ttu-id="68343-144">A szöveg bejegyzés __(alapértelmezett Tárfiók)__ szerepel.</span><span class="sxs-lookup"><span data-stu-id="68343-144">An entry with the text __(Default Storage Account)__ is listed.</span></span>

    ![A tárfiók a fürt megjelenítő Server Explorer](./media/hdinsight-hadoop-hive-pig-udf-dotnet-csharp/storage.png)

    * <span data-ttu-id="68343-146">Ez a bejegyzés bővíthetők, ha az egy __Azure Storage-fiók__ a fürt alapértelmezett tárolóként.</span><span class="sxs-lookup"><span data-stu-id="68343-146">If this entry can be expanded, you are using an __Azure Storage Account__ as default storage for the cluster.</span></span> <span data-ttu-id="68343-147">A fürt az alapértelmezett tároló a fájlok megtekintéséhez bontsa ki a bejegyzést, és majd kattintson duplán a __(alapértelmezett tároló)__.</span><span class="sxs-lookup"><span data-stu-id="68343-147">To view the files on the default storage for the cluster, expand the entry and then double-click the __(Default Container)__.</span></span>

    * <span data-ttu-id="68343-148">Ez a bejegyzés nem bonthatók ki, ha az __Azure Data Lake Store__ a fürt alapértelmezett tárolóként.</span><span class="sxs-lookup"><span data-stu-id="68343-148">If this entry cannot be expanded, you are using __Azure Data Lake Store__ as the default storage for the cluster.</span></span> <span data-ttu-id="68343-149">A fürt az alapértelmezett tároló a fájlok megtekintéséhez kattintson duplán a __(alapértelmezett Tárfiók)__ bejegyzés.</span><span class="sxs-lookup"><span data-stu-id="68343-149">To view the files on the default storage for the cluster, double-click the __(Default Storage Account)__ entry.</span></span>

5. <span data-ttu-id="68343-150">Az .exe fájlok feltöltéséhez használja a következő módszerek egyikét:</span><span class="sxs-lookup"><span data-stu-id="68343-150">To upload the .exe files, use one of the following methods:</span></span>

    * <span data-ttu-id="68343-151">Ha használ egy __Azure Storage-fiók__, kattintson a Feltöltés ikonra, és keresse a **bin\debug** mappát a **leképező** projekt.</span><span class="sxs-lookup"><span data-stu-id="68343-151">If using an __Azure Storage Account__, click the upload icon, and then browse to the **bin\debug** folder for the **mapper** project.</span></span> <span data-ttu-id="68343-152">Végül válassza ki a **mapper.exe** fájlt, és kattintson a **Ok**.</span><span class="sxs-lookup"><span data-stu-id="68343-152">Finally, select the **mapper.exe** file and click **Ok**.</span></span>

        ![Töltse fel az ikon](./media/hdinsight-hadoop-hive-pig-udf-dotnet-csharp/upload.png)
    
    * <span data-ttu-id="68343-154">Ha használ __Azure Data Lake Store__, kattintson a jobb gombbal egy üres területre a fájl az átjáróra a listában, majd válassza ki __feltöltése__.</span><span class="sxs-lookup"><span data-stu-id="68343-154">If using __Azure Data Lake Store__, right-click an empty area in the file listing, and then select __Upload__.</span></span> <span data-ttu-id="68343-155">Végül válassza ki a **mapper.exe** fájlt, és kattintson a **nyitott**.</span><span class="sxs-lookup"><span data-stu-id="68343-155">Finally, select the **mapper.exe** file and click **Open**.</span></span>

    <span data-ttu-id="68343-156">Egyszer a __mapper.exe__ befejezte a feltöltést, ismételje meg a feltöltési folyamat számára a __reducer.exe__ fájlt.</span><span class="sxs-lookup"><span data-stu-id="68343-156">Once the __mapper.exe__ upload has finished, repeat the upload process for the __reducer.exe__ file.</span></span>

## <a name="run-a-job-using-an-ssh-session"></a><span data-ttu-id="68343-157">Egy feladat futtatása: SSH-munkamenetet használatával</span><span class="sxs-lookup"><span data-stu-id="68343-157">Run a job: Using an SSH session</span></span>

1. <span data-ttu-id="68343-158">Az SSH használata a HDInsight-fürthöz való csatlakozáshoz.</span><span class="sxs-lookup"><span data-stu-id="68343-158">Use SSH to connect to the HDInsight cluster.</span></span> <span data-ttu-id="68343-159">További információ: [Az SSH használata HDInsighttal](hdinsight-hadoop-linux-use-ssh-unix.md).</span><span class="sxs-lookup"><span data-stu-id="68343-159">For more information, see [Use SSH with HDInsight](hdinsight-hadoop-linux-use-ssh-unix.md).</span></span>

2. <span data-ttu-id="68343-160">A következő parancs használatával indítsa el a MapReduce feladatot:</span><span class="sxs-lookup"><span data-stu-id="68343-160">Use one of the following command to start the MapReduce job:</span></span>

    * <span data-ttu-id="68343-161">Ha használ __Data Lake Store__ alapértelmezett tárolóként:</span><span class="sxs-lookup"><span data-stu-id="68343-161">If using __Data Lake Store__ as default storage:</span></span>

        ```bash
        yarn jar /usr/hdp/current/hadoop-mapreduce-client/hadoop-streaming.jar -files adl:///mapper.exe,adl:///reducer.exe -mapper mapper.exe -reducer reducer.exe -input /example/data/gutenberg/davinci.txt -output /example/wordcountout
        ```
    
    * <span data-ttu-id="68343-162">Ha használ __Azure Storage__ alapértelmezett tárolóként:</span><span class="sxs-lookup"><span data-stu-id="68343-162">If using __Azure Storage__ as default storage:</span></span>

        ```bash
        yarn jar /usr/hdp/current/hadoop-mapreduce-client/hadoop-streaming.jar -files wasb:///mapper.exe,wasb:///reducer.exe -mapper mapper.exe -reducer reducer.exe -input /example/data/gutenberg/davinci.txt -output /example/wordcountout
        ```

    <span data-ttu-id="68343-163">Az alábbi lista ismerteti, hogy minden paraméter funkciója:</span><span class="sxs-lookup"><span data-stu-id="68343-163">The following list describes what each parameter does:</span></span>

    * <span data-ttu-id="68343-164">`hadoop-streaming.jar`: A jar-fájlra, amely tartalmazza az adatfolyam-továbbítási MapReduce-funkciókat.</span><span class="sxs-lookup"><span data-stu-id="68343-164">`hadoop-streaming.jar`: The jar file that contains the streaming MapReduce functionality.</span></span>
    * <span data-ttu-id="68343-165">`-files`: Hozzáadja a `mapper.exe` és `reducer.exe` fájlok ebben a projektben.</span><span class="sxs-lookup"><span data-stu-id="68343-165">`-files`: Adds the `mapper.exe` and `reducer.exe` files to this job.</span></span> <span data-ttu-id="68343-166">A `adl:///` vagy `wasb:///` előtt minden fájl elérési útját a fürt tárolóhelyét alapértelmezett gyökérmappájában.</span><span class="sxs-lookup"><span data-stu-id="68343-166">The `adl:///` or `wasb:///` before each file is the path to the root of default storage for the cluster.</span></span>
    * <span data-ttu-id="68343-167">`-mapper`: Meghatározza, hogy melyik fájlt a leképező valósítja meg.</span><span class="sxs-lookup"><span data-stu-id="68343-167">`-mapper`: Specifies which file implements the mapper.</span></span>
    * <span data-ttu-id="68343-168">`-reducer`: Meghatározza, hogy melyik fájlt a nyomáscsökkentő valósítja meg.</span><span class="sxs-lookup"><span data-stu-id="68343-168">`-reducer`: Specifies which file implements the reducer.</span></span>
    * <span data-ttu-id="68343-169">`-input`: A bemeneti adatok.</span><span class="sxs-lookup"><span data-stu-id="68343-169">`-input`: The input data.</span></span>
    * <span data-ttu-id="68343-170">`-output`: A kimeneti könyvtár.</span><span class="sxs-lookup"><span data-stu-id="68343-170">`-output`: The output directory.</span></span>

3. <span data-ttu-id="68343-171">Miután befejeződött a MapReduce feladatot, használja az eredmények megtekintéséhez a következő:</span><span class="sxs-lookup"><span data-stu-id="68343-171">Once the MapReduce job completes, use the following to view the results:</span></span>

    ```bash
    hdfs dfs -text /example/wordcountout/part-00000
    ```

    <span data-ttu-id="68343-172">A következő szöveget a parancs által visszaadott adatok példája:</span><span class="sxs-lookup"><span data-stu-id="68343-172">The following text is an example of the data returned by this command:</span></span>

        you     1128
        young   38
        younger 1
        youngest        1
        your    338
        yours   4
        yourself        34
        yourselves      3
        youth   17

## <a name="run-a-job-using-powershell"></a><span data-ttu-id="68343-173">Egy feladat futtatása: PowerShell-lel</span><span class="sxs-lookup"><span data-stu-id="68343-173">Run a job: Using PowerShell</span></span>

<span data-ttu-id="68343-174">A következő PowerShell-parancsfájl segítségével MapReduce feladatot futtatni, és töltse le az eredményeket.</span><span class="sxs-lookup"><span data-stu-id="68343-174">Use the following PowerShell script to run a MapReduce job and download the results.</span></span>

<span data-ttu-id="68343-175">[!code-powershell[fő](../../powershell_scripts/hdinsight/use-csharp-mapreduce/use-csharp-mapreduce.ps1?range=5-87)]</span><span class="sxs-lookup"><span data-stu-id="68343-175">[!code-powershell[main](../../powershell_scripts/hdinsight/use-csharp-mapreduce/use-csharp-mapreduce.ps1?range=5-87)]</span></span>

<span data-ttu-id="68343-176">A parancsprogram kéri a fürt bejelentkezési nevét és jelszavát, és a HDInsight-fürt nevét.</span><span class="sxs-lookup"><span data-stu-id="68343-176">This script prompts you for the cluster login account name and password, along with the HDInsight cluster name.</span></span> <span data-ttu-id="68343-177">Ha a feladat befejeződik, a rendszer letölti a kimenet a `output.txt` fájl a könyvtárban, a parancsfájl futtatása a.</span><span class="sxs-lookup"><span data-stu-id="68343-177">Once the job completes, the output is downloaded to the `output.txt` file in the directory the script is ran from.</span></span> <span data-ttu-id="68343-178">A következő szöveg látható egy példa az adatokat a `output.txt` fájlt:</span><span class="sxs-lookup"><span data-stu-id="68343-178">The following text is an example of the data in the `output.txt` file:</span></span>

    you     1128
    young   38
    younger 1
    youngest        1
    your    338
    yours   4
    yourself        34
    yourselves      3
    youth   17

## <a name="next-steps"></a><span data-ttu-id="68343-179">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="68343-179">Next steps</span></span>

<span data-ttu-id="68343-180">A MapReduce használata a hdinsight eszközzel további információkért lásd: [és a HDInsight együttes használata MapReduce](hdinsight-use-mapreduce.md).</span><span class="sxs-lookup"><span data-stu-id="68343-180">For more information on using MapReduce with HDInsight, see [Use MapReduce with HDInsight](hdinsight-use-mapreduce.md).</span></span>

<span data-ttu-id="68343-181">A C# és Hive és a Pig használatával további információkért lásd: [egy C# felhasználó által definiált függvény használata a Hive és a Pig](hdinsight-hadoop-hive-pig-udf-dotnet-csharp.md).</span><span class="sxs-lookup"><span data-stu-id="68343-181">For information on using C# with Hive and Pig, see [Use a C# user defined function with Hive and Pig](hdinsight-hadoop-hive-pig-udf-dotnet-csharp.md).</span></span>

<span data-ttu-id="68343-182">A HDInsight alatt futó Storm a C# használatával további információkért lásd: [fejlesztésére C#-topológiák HDInsight alatt futó Storm](hdinsight-storm-develop-csharp-visual-studio-topology.md).</span><span class="sxs-lookup"><span data-stu-id="68343-182">For information on using C# with Storm on HDInsight, see [Develop C# topologies for Storm on HDInsight](hdinsight-storm-develop-csharp-visual-studio-topology.md).</span></span>