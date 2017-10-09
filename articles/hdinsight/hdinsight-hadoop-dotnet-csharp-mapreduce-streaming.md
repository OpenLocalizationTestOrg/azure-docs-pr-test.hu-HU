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
# <a name="use-c-with-mapreduce-streaming-on-hadoop-in-hdinsight"></a><span data-ttu-id="3b237-103">C# használata a hdinsight Hadoop streamelési MapReduce</span><span class="sxs-lookup"><span data-stu-id="3b237-103">Use C# with MapReduce streaming on Hadoop in HDInsight</span></span>

<span data-ttu-id="3b237-104">Megtudhatja, hogyan toouse C# toocreate hdinsight MapReduce megoldást.</span><span class="sxs-lookup"><span data-stu-id="3b237-104">Learn how toouse C# toocreate a MapReduce solution on HDInsight.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="3b237-105">Linux hello azt az egyetlen operációs rendszer, használja a HDInsight 3.4 vagy újabb verziója.</span><span class="sxs-lookup"><span data-stu-id="3b237-105">Linux is hello only operating system used on HDInsight version 3.4 or greater.</span></span> <span data-ttu-id="3b237-106">További információkért lásd: [HDInsight-összetevők verziószámozása](hdinsight-component-versioning.md).</span><span class="sxs-lookup"><span data-stu-id="3b237-106">For more information, see [HDInsight component versioning](hdinsight-component-versioning.md).</span></span>

<span data-ttu-id="3b237-107">Hadoop streamelési egy segédprogram, amely lehetővé teszi egy parancsfájl vagy végrehajtható fájllal toorun MapReduce-feladatok.</span><span class="sxs-lookup"><span data-stu-id="3b237-107">Hadoop streaming is a utility that allows you toorun MapReduce jobs using a script or executable.</span></span> <span data-ttu-id="3b237-108">Ebben a példában a .NET használt tooimplement hello hozzárendelést és a word-count megoldások nyomáscsökkentő.</span><span class="sxs-lookup"><span data-stu-id="3b237-108">In this example, .NET is used tooimplement hello mapper and reducer for a word count solution.</span></span>

## <a name="net-on-hdinsight"></a><span data-ttu-id="3b237-109">A HDInsight .NET</span><span class="sxs-lookup"><span data-stu-id="3b237-109">.NET on HDInsight</span></span>

<span data-ttu-id="3b237-110">__Linux-alapú HDInsight__ fürtök használata [monó (https://mono-project.com)](https://mono-project.com) toorun .NET-alkalmazások.</span><span class="sxs-lookup"><span data-stu-id="3b237-110">__Linux-based HDInsight__ clusters use [Mono (https://mono-project.com)](https://mono-project.com) toorun .NET applications.</span></span> <span data-ttu-id="3b237-111">Monó verzió 4.2.1 megtalálható HDInsight 3.5-ös verziója.</span><span class="sxs-lookup"><span data-stu-id="3b237-111">Mono version 4.2.1 is included with HDInsight version 3.5.</span></span> <span data-ttu-id="3b237-112">Monó részét képező HDInsight hello verzióján további információkért lásd: [HDInsight összetevő verziók](hdinsight-component-versioning.md).</span><span class="sxs-lookup"><span data-stu-id="3b237-112">For more information on hello version of Mono included with HDInsight, see [HDInsight component versions](hdinsight-component-versioning.md).</span></span> <span data-ttu-id="3b237-113">Monó, egy adott verziójához toouse lásd: hello [telepítés vagy frissítés monó](hdinsight-hadoop-install-mono.md) dokumentum.</span><span class="sxs-lookup"><span data-stu-id="3b237-113">toouse a specific version of Mono, see hello [Install or update Mono](hdinsight-hadoop-install-mono.md) document.</span></span>

<span data-ttu-id="3b237-114">A .NET-keretrendszer verzióival is kompatibilisek monó további információkért lásd: [monó kompatibilitási](http://www.mono-project.com/docs/about-mono/compatibility/).</span><span class="sxs-lookup"><span data-stu-id="3b237-114">For more information on Mono compatibility with .NET Framework versions, see [Mono compatibility](http://www.mono-project.com/docs/about-mono/compatibility/).</span></span>

## <a name="how-hadoop-streaming-works"></a><span data-ttu-id="3b237-115">Hadoop streamelési működése</span><span class="sxs-lookup"><span data-stu-id="3b237-115">How Hadoop streaming works</span></span>

<span data-ttu-id="3b237-116">hello a használt adatfolyam-ebben a dokumentumban folyamat alapvetően a következőképpen történik:</span><span class="sxs-lookup"><span data-stu-id="3b237-116">hello basic process used for streaming in this document is as follows:</span></span>

1. <span data-ttu-id="3b237-117">Hadoop STDIN adatleképező toohello (ebben a példában mapper.exe) továbbítja.</span><span class="sxs-lookup"><span data-stu-id="3b237-117">Hadoop passes data toohello mapper (mapper.exe in this example) on STDIN.</span></span>
2. <span data-ttu-id="3b237-118">hello leképező hello adatokat dolgozza fel, és tabulátorral tagolt kulcs/érték párok tooSTDOUT bocsát ki.</span><span class="sxs-lookup"><span data-stu-id="3b237-118">hello mapper processes hello data, and emits tab-delimited key/value pairs tooSTDOUT.</span></span>
3. <span data-ttu-id="3b237-119">hello kimeneti Hadoop olvasni, és ezután átadja STDIN toohello nyomáscsökkentő (reducer.exe ebben a példában).</span><span class="sxs-lookup"><span data-stu-id="3b237-119">hello output is read by Hadoop, and then passed toohello reducer (reducer.exe in this example) on STDIN.</span></span>
4. <span data-ttu-id="3b237-120">hello nyomáscsökkentő tabulátorral tagolt hello kulcs/érték párok beolvassa, feldolgozza a hello adatokat és majd bocsát ki hello eredmény tabulátorral tagolt kulcs/érték párként STDOUT-on.</span><span class="sxs-lookup"><span data-stu-id="3b237-120">hello reducer reads hello tab-delimited key/value pairs, processes hello data, and then emits hello result as tab-delimited key/value pairs on STDOUT.</span></span>
5. <span data-ttu-id="3b237-121">hello kimeneti Hadoop olvashatják és írt toohello kimeneti könyvtár.</span><span class="sxs-lookup"><span data-stu-id="3b237-121">hello output is read by Hadoop and written toohello output directory.</span></span>

<span data-ttu-id="3b237-122">A streaming további információkért lásd: [Hadoop Streamelési (https://hadoop.apache.org/docs/r2.7.1/hadoop-streaming/HadoopStreaming.html)](https://hadoop.apache.org/docs/r2.7.1/hadoop-streaming/HadoopStreaming.html).</span><span class="sxs-lookup"><span data-stu-id="3b237-122">For more information on streaming, see [Hadoop Streaming (https://hadoop.apache.org/docs/r2.7.1/hadoop-streaming/HadoopStreaming.html)](https://hadoop.apache.org/docs/r2.7.1/hadoop-streaming/HadoopStreaming.html).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="3b237-123">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="3b237-123">Prerequisites</span></span>

* <span data-ttu-id="3b237-124">Egy írást, és C#-kódban, amelynek célpontja a .NET-keretrendszer 4.5 felépítése ismeretét.</span><span class="sxs-lookup"><span data-stu-id="3b237-124">A familiarity with writing and building C# code that targets .NET Framework 4.5.</span></span> <span data-ttu-id="3b237-125">Ez a dokumentum használatát a Visual Studio 2017 hello szükséges lépések.</span><span class="sxs-lookup"><span data-stu-id="3b237-125">hello steps in this document use Visual Studio 2017.</span></span>

* <span data-ttu-id="3b237-126">Módon tooupload .exe fájlok toohello fürt.</span><span class="sxs-lookup"><span data-stu-id="3b237-126">A way tooupload .exe files toohello cluster.</span></span> <span data-ttu-id="3b237-127">hello jelen dokumentumban leírt lépések használata hello Data Lake Tools Visual Studio tooupload hello fájlok tooprimary hello fürt tárolóhelyét.</span><span class="sxs-lookup"><span data-stu-id="3b237-127">hello steps in this document use hello Data Lake Tools for Visual Studio tooupload hello files tooprimary storage for hello cluster.</span></span>

* <span data-ttu-id="3b237-128">Az Azure PowerShell vagy SSH-ügyfél.</span><span class="sxs-lookup"><span data-stu-id="3b237-128">Azure PowerShell or a SSH client.</span></span>

* <span data-ttu-id="3b237-129">A Hadoop on HDInsight-fürt.</span><span class="sxs-lookup"><span data-stu-id="3b237-129">A Hadoop on HDInsight cluster.</span></span> <span data-ttu-id="3b237-130">A fürtök létrehozásáról további információk: [HDInsight-fürtök létrehozása](hdinsight-provision-clusters.md).</span><span class="sxs-lookup"><span data-stu-id="3b237-130">For more information on creating a cluster, see [Create an HDInsight cluster](hdinsight-provision-clusters.md).</span></span>

## <a name="create-hello-mapper"></a><span data-ttu-id="3b237-131">Hozzon létre hello leképezője</span><span class="sxs-lookup"><span data-stu-id="3b237-131">Create hello mapper</span></span>

<span data-ttu-id="3b237-132">A Visual Studióban hozzon létre egy új __Konzolalkalmazás__ nevű __leképező__.</span><span class="sxs-lookup"><span data-stu-id="3b237-132">In Visual Studio, create a new __Console application__ named __mapper__.</span></span> <span data-ttu-id="3b237-133">A következő kód hello alkalmazás hello használata:</span><span class="sxs-lookup"><span data-stu-id="3b237-133">Use hello following code for hello application:</span></span>

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

<span data-ttu-id="3b237-134">Hello alkalmazás létrehozása után összeállítani, tooproduce hello `/bin/Debug/mapper.exe` hello projekt fájl.</span><span class="sxs-lookup"><span data-stu-id="3b237-134">After creating hello application, build it tooproduce hello `/bin/Debug/mapper.exe` file in hello project directory.</span></span>

## <a name="create-hello-reducer"></a><span data-ttu-id="3b237-135">Hello nyomáscsökkentő létrehozása</span><span class="sxs-lookup"><span data-stu-id="3b237-135">Create hello reducer</span></span>

<span data-ttu-id="3b237-136">A Visual Studióban hozzon létre egy új __Konzolalkalmazás__ nevű __nyomáscsökkentő__.</span><span class="sxs-lookup"><span data-stu-id="3b237-136">In Visual Studio, create a new __Console application__ named __reducer__.</span></span> <span data-ttu-id="3b237-137">A következő kód hello alkalmazás hello használata:</span><span class="sxs-lookup"><span data-stu-id="3b237-137">Use hello following code for hello application:</span></span>

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

<span data-ttu-id="3b237-138">Hello alkalmazás létrehozása után összeállítani, tooproduce hello `/bin/Debug/reducer.exe` hello projekt fájl.</span><span class="sxs-lookup"><span data-stu-id="3b237-138">After creating hello application, build it tooproduce hello `/bin/Debug/reducer.exe` file in hello project directory.</span></span>

## <a name="upload-toostorage"></a><span data-ttu-id="3b237-139">Toostorage feltöltése</span><span class="sxs-lookup"><span data-stu-id="3b237-139">Upload toostorage</span></span>

1. <span data-ttu-id="3b237-140">A Visual Studióban nyissa meg a **Server Explorer**.</span><span class="sxs-lookup"><span data-stu-id="3b237-140">In Visual Studio, open **Server Explorer**.</span></span>

2. <span data-ttu-id="3b237-141">Bontsa ki az **Azure** elemet, majd bontsa ki a **HDInsight** elemet.</span><span class="sxs-lookup"><span data-stu-id="3b237-141">Expand **Azure**, and then expand **HDInsight**.</span></span>

3. <span data-ttu-id="3b237-142">Ha a rendszer kéri, adja meg Azure-előfizetés hitelesítő adatait, és kattintson **bejelentkezés**.</span><span class="sxs-lookup"><span data-stu-id="3b237-142">If prompted, enter your Azure subscription credentials, and then click **Sign In**.</span></span>

4. <span data-ttu-id="3b237-143">Bontsa ki, hogy az alkalmazás kívánja toodeploy hello HDInsight-fürthöz.</span><span class="sxs-lookup"><span data-stu-id="3b237-143">Expand hello HDInsight cluster that you wish toodeploy this application to.</span></span> <span data-ttu-id="3b237-144">Hello szöveg bejegyzés __(alapértelmezett Tárfiók)__ szerepel.</span><span class="sxs-lookup"><span data-stu-id="3b237-144">An entry with hello text __(Default Storage Account)__ is listed.</span></span>

    ![Server Explorer hello tárfiók hello fürt megjelenítése](./media/hdinsight-hadoop-hive-pig-udf-dotnet-csharp/storage.png)

    * <span data-ttu-id="3b237-146">Ez a bejegyzés bővíthetők, ha az egy __Azure Storage-fiók__ hello fürt alapértelmezett tárolóként.</span><span class="sxs-lookup"><span data-stu-id="3b237-146">If this entry can be expanded, you are using an __Azure Storage Account__ as default storage for hello cluster.</span></span> <span data-ttu-id="3b237-147">tooview hello fájlok hello alapértelmezett tároló hello fürt, bontsa ki a hello bejegyzést, és kattintson duplán a hello __(alapértelmezett tároló)__.</span><span class="sxs-lookup"><span data-stu-id="3b237-147">tooview hello files on hello default storage for hello cluster, expand hello entry and then double-click hello __(Default Container)__.</span></span>

    * <span data-ttu-id="3b237-148">Ez a bejegyzés nem bonthatók ki, ha az __Azure Data Lake Store__ hello fürt hello alapértelmezett tárolóként.</span><span class="sxs-lookup"><span data-stu-id="3b237-148">If this entry cannot be expanded, you are using __Azure Data Lake Store__ as hello default storage for hello cluster.</span></span> <span data-ttu-id="3b237-149">tooview hello fájlok hello alapértelmezett tároló hello fürt, kattintson duplán a hello __(alapértelmezett Tárfiók)__ bejegyzés.</span><span class="sxs-lookup"><span data-stu-id="3b237-149">tooview hello files on hello default storage for hello cluster, double-click hello __(Default Storage Account)__ entry.</span></span>

5. <span data-ttu-id="3b237-150">tooupload hello .exe fájlok, a hello a következő módszerek valamelyikével:</span><span class="sxs-lookup"><span data-stu-id="3b237-150">tooupload hello .exe files, use one of hello following methods:</span></span>

    * <span data-ttu-id="3b237-151">Ha használ egy __Azure Storage-fiók__, hello Feltöltés ikonra, és tallózással keressen toohello **bin\debug** hello mappa **leképező** projekt.</span><span class="sxs-lookup"><span data-stu-id="3b237-151">If using an __Azure Storage Account__, click hello upload icon, and then browse toohello **bin\debug** folder for hello **mapper** project.</span></span> <span data-ttu-id="3b237-152">Végül válassza ki a hello **mapper.exe** fájlt, és kattintson a **Ok**.</span><span class="sxs-lookup"><span data-stu-id="3b237-152">Finally, select hello **mapper.exe** file and click **Ok**.</span></span>

        ![Töltse fel az ikon](./media/hdinsight-hadoop-hive-pig-udf-dotnet-csharp/upload.png)
    
    * <span data-ttu-id="3b237-154">Ha használ __Azure Data Lake Store__, kattintson a jobb gombbal egy üres területre a hello felsoroló, és válassza __feltöltése__.</span><span class="sxs-lookup"><span data-stu-id="3b237-154">If using __Azure Data Lake Store__, right-click an empty area in hello file listing, and then select __Upload__.</span></span> <span data-ttu-id="3b237-155">Végül válassza ki a hello **mapper.exe** fájlt, és kattintson a **nyitott**.</span><span class="sxs-lookup"><span data-stu-id="3b237-155">Finally, select hello **mapper.exe** file and click **Open**.</span></span>

    <span data-ttu-id="3b237-156">Egyszer hello __mapper.exe__ feltöltése befejeződött, ismétlődő hello feltöltési folyamat a hello __reducer.exe__ fájlt.</span><span class="sxs-lookup"><span data-stu-id="3b237-156">Once hello __mapper.exe__ upload has finished, repeat hello upload process for hello __reducer.exe__ file.</span></span>

## <a name="run-a-job-using-an-ssh-session"></a><span data-ttu-id="3b237-157">Egy feladat futtatása: SSH-munkamenetet használatával</span><span class="sxs-lookup"><span data-stu-id="3b237-157">Run a job: Using an SSH session</span></span>

1. <span data-ttu-id="3b237-158">SSH tooconnect toohello HDInsight-fürt használatára.</span><span class="sxs-lookup"><span data-stu-id="3b237-158">Use SSH tooconnect toohello HDInsight cluster.</span></span> <span data-ttu-id="3b237-159">További információ: [Az SSH használata HDInsighttal](hdinsight-hadoop-linux-use-ssh-unix.md).</span><span class="sxs-lookup"><span data-stu-id="3b237-159">For more information, see [Use SSH with HDInsight](hdinsight-hadoop-linux-use-ssh-unix.md).</span></span>

2. <span data-ttu-id="3b237-160">A következő parancs toostart hello MapReduce feladatot hello egyikét használja:</span><span class="sxs-lookup"><span data-stu-id="3b237-160">Use one of hello following command toostart hello MapReduce job:</span></span>

    * <span data-ttu-id="3b237-161">Ha használ __Data Lake Store__ alapértelmezett tárolóként:</span><span class="sxs-lookup"><span data-stu-id="3b237-161">If using __Data Lake Store__ as default storage:</span></span>

        ```bash
        yarn jar /usr/hdp/current/hadoop-mapreduce-client/hadoop-streaming.jar -files adl:///mapper.exe,adl:///reducer.exe -mapper mapper.exe -reducer reducer.exe -input /example/data/gutenberg/davinci.txt -output /example/wordcountout
        ```
    
    * <span data-ttu-id="3b237-162">Ha használ __Azure Storage__ alapértelmezett tárolóként:</span><span class="sxs-lookup"><span data-stu-id="3b237-162">If using __Azure Storage__ as default storage:</span></span>

        ```bash
        yarn jar /usr/hdp/current/hadoop-mapreduce-client/hadoop-streaming.jar -files wasb:///mapper.exe,wasb:///reducer.exe -mapper mapper.exe -reducer reducer.exe -input /example/data/gutenberg/davinci.txt -output /example/wordcountout
        ```

    <span data-ttu-id="3b237-163">hello a következő lista ismerteti, hogy minden paraméter funkciója:</span><span class="sxs-lookup"><span data-stu-id="3b237-163">hello following list describes what each parameter does:</span></span>

    * <span data-ttu-id="3b237-164">`hadoop-streaming.jar`: hello jar-fájlra, amely a MapReduce funkció streaming hello tartalmazza.</span><span class="sxs-lookup"><span data-stu-id="3b237-164">`hadoop-streaming.jar`: hello jar file that contains hello streaming MapReduce functionality.</span></span>
    * <span data-ttu-id="3b237-165">`-files`: Hello hozzáadja `mapper.exe` és `reducer.exe` fájlok toothis feladat.</span><span class="sxs-lookup"><span data-stu-id="3b237-165">`-files`: Adds hello `mapper.exe` and `reducer.exe` files toothis job.</span></span> <span data-ttu-id="3b237-166">Hello `adl:///` vagy `wasb:///` előtt minden fájl hello elérési toohello hello fürt tárolóhelyét alapértelmezett gyökérmappájában.</span><span class="sxs-lookup"><span data-stu-id="3b237-166">hello `adl:///` or `wasb:///` before each file is hello path toohello root of default storage for hello cluster.</span></span>
    * <span data-ttu-id="3b237-167">`-mapper`: Adja meg, melyik fájl hello leképező valósítja meg.</span><span class="sxs-lookup"><span data-stu-id="3b237-167">`-mapper`: Specifies which file implements hello mapper.</span></span>
    * <span data-ttu-id="3b237-168">`-reducer`: Adja meg, melyik fájl hello nyomáscsökkentő valósítja meg.</span><span class="sxs-lookup"><span data-stu-id="3b237-168">`-reducer`: Specifies which file implements hello reducer.</span></span>
    * <span data-ttu-id="3b237-169">`-input`: hello bemeneti adatai.</span><span class="sxs-lookup"><span data-stu-id="3b237-169">`-input`: hello input data.</span></span>
    * <span data-ttu-id="3b237-170">`-output`: hello kimeneti könyvtár.</span><span class="sxs-lookup"><span data-stu-id="3b237-170">`-output`: hello output directory.</span></span>

3. <span data-ttu-id="3b237-171">Amikor hello MapReduce feladat befejeződik, használja a következő tooview hello eredmények hello:</span><span class="sxs-lookup"><span data-stu-id="3b237-171">Once hello MapReduce job completes, use hello following tooview hello results:</span></span>

    ```bash
    hdfs dfs -text /example/wordcountout/part-00000
    ```

    <span data-ttu-id="3b237-172">hello következő szöveg látható egy példa a parancs által visszaadott hello adatok:</span><span class="sxs-lookup"><span data-stu-id="3b237-172">hello following text is an example of hello data returned by this command:</span></span>

        you     1128
        young   38
        younger 1
        youngest        1
        your    338
        yours   4
        yourself        34
        yourselves      3
        youth   17

## <a name="run-a-job-using-powershell"></a><span data-ttu-id="3b237-173">Egy feladat futtatása: PowerShell-lel</span><span class="sxs-lookup"><span data-stu-id="3b237-173">Run a job: Using PowerShell</span></span>

<span data-ttu-id="3b237-174">A következő PowerShell-parancsfájl toorun MapReduce feladatot hello használata és hello eredmények letöltéséhez.</span><span class="sxs-lookup"><span data-stu-id="3b237-174">Use hello following PowerShell script toorun a MapReduce job and download hello results.</span></span>

[!code-powershell[main](../../powershell_scripts/hdinsight/use-csharp-mapreduce/use-csharp-mapreduce.ps1?range=5-87)]

<span data-ttu-id="3b237-175">A parancsprogram kéri hello fürt bejelentkezési nevét és jelszavát, valamint hello HDInsight-fürt neve.</span><span class="sxs-lookup"><span data-stu-id="3b237-175">This script prompts you for hello cluster login account name and password, along with hello HDInsight cluster name.</span></span> <span data-ttu-id="3b237-176">Miután hello feladat befejeződött, hello eredménye a letöltött toohello `output.txt` fájl hello directory hello parancsfájlban a van futott.</span><span class="sxs-lookup"><span data-stu-id="3b237-176">Once hello job completes, hello output is downloaded toohello `output.txt` file in hello directory hello script is ran from.</span></span> <span data-ttu-id="3b237-177">hello következő szöveg látható egy példa hello hello adatok `output.txt` fájlt:</span><span class="sxs-lookup"><span data-stu-id="3b237-177">hello following text is an example of hello data in hello `output.txt` file:</span></span>

    you     1128
    young   38
    younger 1
    youngest        1
    your    338
    yours   4
    yourself        34
    yourselves      3
    youth   17

## <a name="next-steps"></a><span data-ttu-id="3b237-178">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="3b237-178">Next steps</span></span>

<span data-ttu-id="3b237-179">A MapReduce használata a hdinsight eszközzel további információkért lásd: [és a HDInsight együttes használata MapReduce](hdinsight-use-mapreduce.md).</span><span class="sxs-lookup"><span data-stu-id="3b237-179">For more information on using MapReduce with HDInsight, see [Use MapReduce with HDInsight](hdinsight-use-mapreduce.md).</span></span>

<span data-ttu-id="3b237-180">A C# és Hive és a Pig használatával további információkért lásd: [egy C# felhasználó által definiált függvény használata a Hive és a Pig](hdinsight-hadoop-hive-pig-udf-dotnet-csharp.md).</span><span class="sxs-lookup"><span data-stu-id="3b237-180">For information on using C# with Hive and Pig, see [Use a C# user defined function with Hive and Pig](hdinsight-hadoop-hive-pig-udf-dotnet-csharp.md).</span></span>

<span data-ttu-id="3b237-181">A HDInsight alatt futó Storm a C# használatával további információkért lásd: [fejlesztésére C#-topológiák HDInsight alatt futó Storm](hdinsight-storm-develop-csharp-visual-studio-topology.md).</span><span class="sxs-lookup"><span data-stu-id="3b237-181">For information on using C# with Storm on HDInsight, see [Develop C# topologies for Storm on HDInsight](hdinsight-storm-develop-csharp-visual-studio-topology.md).</span></span>