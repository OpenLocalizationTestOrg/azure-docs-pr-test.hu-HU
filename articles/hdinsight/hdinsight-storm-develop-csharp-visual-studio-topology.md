---
title: "a Visual Studio és a C# - Azure HDInsight Storm-topológiák aaaApache |} Microsoft Docs"
description: "Ismerje meg, hogy a C# Storm-topológiák toocreate. Hozzon létre egy egyszerű word-count topológiához a Visual Studio által hello Hadoop tools for Visual Studio használatával."
services: hdinsight
documentationcenter: 
author: Blackmist
manager: jhubbard
editor: cgronlun
tags: azure-portal
ms.assetid: 380d804f-a8c5-4b20-9762-593ec4da5a0d
ms.service: hdinsight
ms.custom: hdinsightactive
ms.devlang: java
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 08/02/2017
ms.author: larryfr
ms.openlocfilehash: b3fb01a4dda144fd7fb4141e624e31e667f93753
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="develop-c-topologies-for-apache-storm-by-using-hello-data-lake-tools-for-visual-studio"></a><span data-ttu-id="b9e47-104">C#-topológiák fejlesztése az Apache Storm által hello Data Lake tools for Visual Studio használatával</span><span class="sxs-lookup"><span data-stu-id="b9e47-104">Develop C# topologies for Apache Storm by using hello Data Lake tools for Visual Studio</span></span>

<span data-ttu-id="b9e47-105">Ismerje meg, hogyan toocreate a C# Storm-topológia hello Azure Data Lake (Hadoop) használatával tools for Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="b9e47-105">Learn how toocreate a C# Storm topology by using hello Azure Data Lake (Hadoop) tools for Visual Studio.</span></span> <span data-ttu-id="b9e47-106">Ebből a dokumentumból hello folyamatot, amely egy Storm-projekt létrehozása a Visual Studio helyi tesztelés és tooan alatt futó Apache Storm on Azure HDInsight-fürt üzembe helyezése.</span><span class="sxs-lookup"><span data-stu-id="b9e47-106">This document walks through hello process of creating a Storm project in Visual Studio, testing it locally, and deploying it tooan Apache Storm on Azure HDInsight cluster.</span></span>

<span data-ttu-id="b9e47-107">Azt is megtudhatja, hogyan C#-és Java használó hibrid topológiák toocreate.</span><span class="sxs-lookup"><span data-stu-id="b9e47-107">You also learn how toocreate hybrid topologies that use C# and Java components.</span></span>

> [!NOTE]
> <span data-ttu-id="b9e47-108">Hello jelen dokumentumban leírt lépések a Visual Studio Windows fejlesztői környezetre támaszkodnak, amíg a hello lefordított projekt lehet elküldött tooeither egy Linux vagy a Windows-alapú HDInsight-fürt.</span><span class="sxs-lookup"><span data-stu-id="b9e47-108">While hello steps in this document rely on a Windows development environment with Visual Studio, hello compiled project can be submitted tooeither a Linux or Windows-based HDInsight cluster.</span></span> <span data-ttu-id="b9e47-109">Csak a Linux-alapú fürtök létrehozása után 28 2016 októberétől kezdve, SCP.NET topológiákat támogatja.</span><span class="sxs-lookup"><span data-stu-id="b9e47-109">Only Linux-based clusters created after October 28, 2016, support SCP.NET topologies.</span></span>

<span data-ttu-id="b9e47-110">a Linux-alapú fürt toouse egy C# topológia, frissítenie kell a hello Microsoft.SCP.Net.SDK NuGet-csomagot a projekt tooversion 0.10.0.6 által használt vagy újabb.</span><span class="sxs-lookup"><span data-stu-id="b9e47-110">toouse a C# topology with a Linux-based cluster, you must update hello Microsoft.SCP.Net.SDK NuGet package used by your project tooversion 0.10.0.6 or later.</span></span> <span data-ttu-id="b9e47-111">hello csomag hello verziójának is egyeznie kell telepíteni a HDInsight alatt futó Storm hello főverziója.</span><span class="sxs-lookup"><span data-stu-id="b9e47-111">hello version of hello package must also match hello major version of Storm installed on HDInsight.</span></span>

| <span data-ttu-id="b9e47-112">HDInsight-verzió</span><span class="sxs-lookup"><span data-stu-id="b9e47-112">HDInsight version</span></span> | <span data-ttu-id="b9e47-113">A Storm verzióját</span><span class="sxs-lookup"><span data-stu-id="b9e47-113">Storm version</span></span> | <span data-ttu-id="b9e47-114">SCP.NET verzió</span><span class="sxs-lookup"><span data-stu-id="b9e47-114">SCP.NET version</span></span> | <span data-ttu-id="b9e47-115">Alapértelmezett monó verzió</span><span class="sxs-lookup"><span data-stu-id="b9e47-115">Default Mono version</span></span> |
|:-----------------:|:-------------:|:---------------:|:--------------------:|
| <span data-ttu-id="b9e47-116">3.3</span><span class="sxs-lookup"><span data-stu-id="b9e47-116">3.3</span></span> |<span data-ttu-id="b9e47-117">0.10.x</span><span class="sxs-lookup"><span data-stu-id="b9e47-117">0.10.x</span></span> |<span data-ttu-id="b9e47-118">0.10.x.x</span><span class="sxs-lookup"><span data-stu-id="b9e47-118">0.10.x.x</span></span></br><span data-ttu-id="b9e47-119">(csak a Windows-alapú HDInsight)</span><span class="sxs-lookup"><span data-stu-id="b9e47-119">(only on Windows-based HDInsight)</span></span> | <span data-ttu-id="b9e47-120">NA</span><span class="sxs-lookup"><span data-stu-id="b9e47-120">NA</span></span> |
| <span data-ttu-id="b9e47-121">3.4</span><span class="sxs-lookup"><span data-stu-id="b9e47-121">3.4</span></span> | <span data-ttu-id="b9e47-122">0.10.0.x</span><span class="sxs-lookup"><span data-stu-id="b9e47-122">0.10.0.x</span></span> | <span data-ttu-id="b9e47-123">0.10.0.x</span><span class="sxs-lookup"><span data-stu-id="b9e47-123">0.10.0.x</span></span> | <span data-ttu-id="b9e47-124">3.2.8</span><span class="sxs-lookup"><span data-stu-id="b9e47-124">3.2.8</span></span> |
| <span data-ttu-id="b9e47-125">3.5</span><span class="sxs-lookup"><span data-stu-id="b9e47-125">3.5</span></span> | <span data-ttu-id="b9e47-126">1.0.2.x</span><span class="sxs-lookup"><span data-stu-id="b9e47-126">1.0.2.x</span></span> | <span data-ttu-id="b9e47-127">1.0.0.x</span><span class="sxs-lookup"><span data-stu-id="b9e47-127">1.0.0.x</span></span> | <span data-ttu-id="b9e47-128">4.2.1</span><span class="sxs-lookup"><span data-stu-id="b9e47-128">4.2.1</span></span> |
| <span data-ttu-id="b9e47-129">3.6</span><span class="sxs-lookup"><span data-stu-id="b9e47-129">3.6</span></span> | <span data-ttu-id="b9e47-130">1.1.0.x</span><span class="sxs-lookup"><span data-stu-id="b9e47-130">1.1.0.x</span></span> | <span data-ttu-id="b9e47-131">1.0.0.x</span><span class="sxs-lookup"><span data-stu-id="b9e47-131">1.0.0.x</span></span> | <span data-ttu-id="b9e47-132">4.2.8</span><span class="sxs-lookup"><span data-stu-id="b9e47-132">4.2.8</span></span> |

> [!IMPORTANT]
> <span data-ttu-id="b9e47-133">C#-topológiák Linux-alapú fürtökön kell használni a .NET 4.5, és monó toorun hello HDInsight-fürt használatára.</span><span class="sxs-lookup"><span data-stu-id="b9e47-133">C# topologies on Linux-based clusters must use .NET 4.5, and use Mono toorun on hello HDInsight cluster.</span></span> <span data-ttu-id="b9e47-134">Ellenőrizze [monó kompatibilitási](http://www.mono-project.com/docs/about-mono/compatibility/) az esetleges kompatibilitási problémák.</span><span class="sxs-lookup"><span data-stu-id="b9e47-134">Check [Mono compatibility](http://www.mono-project.com/docs/about-mono/compatibility/) for potential incompatibilities.</span></span>

## <a name="install-visual-studio"></a><span data-ttu-id="b9e47-135">A Visual Studio telepítése</span><span class="sxs-lookup"><span data-stu-id="b9e47-135">Install Visual Studio</span></span>

<span data-ttu-id="b9e47-136">C#-topológiák rendelkező SCP.NET fejleszthet követően a Visual Studio verziójának hello egyikének használatával:</span><span class="sxs-lookup"><span data-stu-id="b9e47-136">You can develop C# topologies with SCP.NET by using one of hello following versions of Visual Studio:</span></span>

* <span data-ttu-id="b9e47-137">A Visual Studio 2012 [4. frissítés](http://www.microsoft.com/download/details.aspx?id=39305)</span><span class="sxs-lookup"><span data-stu-id="b9e47-137">Visual Studio 2012 with [Update 4](http://www.microsoft.com/download/details.aspx?id=39305)</span></span>

* <span data-ttu-id="b9e47-138">A Visual Studio 2013-as verziójának [4. frissítés](http://www.microsoft.com/download/details.aspx?id=44921) vagy [Visual Studio 2013 Community](http://go.microsoft.com/fwlink/?LinkId=517284)</span><span class="sxs-lookup"><span data-stu-id="b9e47-138">Visual Studio 2013 with [Update 4](http://www.microsoft.com/download/details.aspx?id=44921) or [Visual Studio 2013 Community](http://go.microsoft.com/fwlink/?LinkId=517284)</span></span>

* <span data-ttu-id="b9e47-139">Visual Studio 2015-öt vagy [Visual Studio 2015-Közösség](https://go.microsoft.com/fwlink/?LinkId=532606)</span><span class="sxs-lookup"><span data-stu-id="b9e47-139">Visual Studio 2015 or [Visual Studio 2015 Community](https://go.microsoft.com/fwlink/?LinkId=532606)</span></span>

* <span data-ttu-id="b9e47-140">A Visual Studio 2017 (minden kiadás)</span><span class="sxs-lookup"><span data-stu-id="b9e47-140">Visual Studio 2017 (any edition)</span></span>

## <a name="install-data-lake-tools-for-visual-studio"></a><span data-ttu-id="b9e47-141">Telepítse a Data Lake Visual Studio eszközök</span><span class="sxs-lookup"><span data-stu-id="b9e47-141">Install Data Lake tools for Visual Studio</span></span>

<span data-ttu-id="b9e47-142">tooinstall Data Lake tools for Visual Studio kövesse hello [Ismerkedés a Data Lake tools for Visual Studio használatával](hdinsight-hadoop-visual-studio-tools-get-started.md).</span><span class="sxs-lookup"><span data-stu-id="b9e47-142">tooinstall Data Lake tools for Visual Studio, follow hello steps in [Get started using Data Lake tools for Visual Studio](hdinsight-hadoop-visual-studio-tools-get-started.md).</span></span>

## <a name="install-java"></a><span data-ttu-id="b9e47-143">Java telepítése</span><span class="sxs-lookup"><span data-stu-id="b9e47-143">Install Java</span></span>

<span data-ttu-id="b9e47-144">A Storm-topológia a Visual Studio eszközből elküldésekor SCP.NET hello topológia és a függőségek tartalmazó zip-fájl állít elő.</span><span class="sxs-lookup"><span data-stu-id="b9e47-144">When you submit a Storm topology from Visual Studio, SCP.NET generates a zip file that contains hello topology and dependencies.</span></span> <span data-ttu-id="b9e47-145">Java használt toocreate ezek zip-fájlokhoz, mert több kompatibilis a Linux-alapú fürtökön formátuma nem használ.</span><span class="sxs-lookup"><span data-stu-id="b9e47-145">Java is used toocreate these zip files, because it uses a format that is more compatible with Linux-based clusters.</span></span>

1. <span data-ttu-id="b9e47-146">Hello Java fejlesztői készlet (JDK) 7 vagy újabb a fejlesztési környezet telepítése.</span><span class="sxs-lookup"><span data-stu-id="b9e47-146">Install hello Java Developer Kit (JDK) 7 or later on your development environment.</span></span> <span data-ttu-id="b9e47-147">Kaphat az Oracle JDK hello [Oracle](http://www.oracle.com/technetwork/java/javase/downloads/index.html).</span><span class="sxs-lookup"><span data-stu-id="b9e47-147">You can get hello Oracle JDK from [Oracle](http://www.oracle.com/technetwork/java/javase/downloads/index.html).</span></span> <span data-ttu-id="b9e47-148">Is [más Java terjesztéseket](http://openjdk.java.net/).</span><span class="sxs-lookup"><span data-stu-id="b9e47-148">You can also use [other Java distributions](http://openjdk.java.net/).</span></span>

2. <span data-ttu-id="b9e47-149">Hello `JAVA_HOME` környezeti változó kell pont toohello tartalmazó könyvtár Java.</span><span class="sxs-lookup"><span data-stu-id="b9e47-149">hello `JAVA_HOME` environment variable must point toohello directory that contains Java.</span></span>

3. <span data-ttu-id="b9e47-150">Hello `PATH` környezeti változó tartalmaznia kell hello `%JAVA_HOME%\bin` könyvtár.</span><span class="sxs-lookup"><span data-stu-id="b9e47-150">hello `PATH` environment variable must include hello `%JAVA_HOME%\bin` directory.</span></span>

<span data-ttu-id="b9e47-151">C# console application tooverify, hogy Java és hello JDK helyesen vannak-e telepítve a következő hello használhatja:</span><span class="sxs-lookup"><span data-stu-id="b9e47-151">You can use hello following C# console application tooverify that Java and hello JDK are correctly installed:</span></span>

```csharp
using System;
using System.IO;
namespace ConsoleApplication2
{
   class Program
   {
       static void Main(string[] args)
       {
           string javaHome = Environment.GetEnvironmentVariable(“JAVA_HOME”);
           if (!string.IsNullOrEmpty(javaHome))
           {
               string jarExe = Path.Combine(javaHome + @”\bin”, “jar.exe”);
               if (File.Exists(jarExe))
               {
                   Console.WriteLine(“JAVA Is Installed properly”);
                    return;
               }
               else
               {
                   Console.WriteLine(“A valid JAVA JDK is not found. Looks like JRE is installed instead of JDK.”);
               }
           }
           else
           {
             Console.WriteLine(“A valid JAVA JDK is not found. JAVA_HOME environment variable is not set.”);
           }
       }  
   }
}
```

## <a name="storm-templates"></a><span data-ttu-id="b9e47-152">A Storm-sablonok</span><span class="sxs-lookup"><span data-stu-id="b9e47-152">Storm templates</span></span>

<span data-ttu-id="b9e47-153">hello Data Lake tools for Visual Studio adja meg a következő sablonok hello:</span><span class="sxs-lookup"><span data-stu-id="b9e47-153">hello Data Lake tools for Visual Studio provide hello following templates:</span></span>

| <span data-ttu-id="b9e47-154">Projekt típusa</span><span class="sxs-lookup"><span data-stu-id="b9e47-154">Project type</span></span> | <span data-ttu-id="b9e47-155">Azt mutatja be</span><span class="sxs-lookup"><span data-stu-id="b9e47-155">Demonstrates</span></span> |
| --- | --- |
| <span data-ttu-id="b9e47-156">Storm-alkalmazás</span><span class="sxs-lookup"><span data-stu-id="b9e47-156">Storm Application</span></span> |<span data-ttu-id="b9e47-157">Egy üres Storm-topológia projektet.</span><span class="sxs-lookup"><span data-stu-id="b9e47-157">An empty Storm topology project.</span></span> |
| <span data-ttu-id="b9e47-158">A Storm Azure SQL Writer minta</span><span class="sxs-lookup"><span data-stu-id="b9e47-158">Storm Azure SQL Writer Sample</span></span> |<span data-ttu-id="b9e47-159">Hogyan toowrite tooAzure SQL-adatbázis.</span><span class="sxs-lookup"><span data-stu-id="b9e47-159">How toowrite tooAzure SQL Database.</span></span> |
| <span data-ttu-id="b9e47-160">A Storm Azure Cosmos DB olvasó minta</span><span class="sxs-lookup"><span data-stu-id="b9e47-160">Storm Azure Cosmos DB Reader Sample</span></span> |<span data-ttu-id="b9e47-161">Hogyan tooread a Azure Cosmos-Adatbázisból.</span><span class="sxs-lookup"><span data-stu-id="b9e47-161">How tooread from Azure Cosmos DB.</span></span> |
| <span data-ttu-id="b9e47-162">A Storm Azure Cosmos DB író minta</span><span class="sxs-lookup"><span data-stu-id="b9e47-162">Storm Azure Cosmos DB Writer Sample</span></span> |<span data-ttu-id="b9e47-163">Hogyan toowrite tooAzure Cosmos DB.</span><span class="sxs-lookup"><span data-stu-id="b9e47-163">How toowrite tooAzure Cosmos DB.</span></span> |
| <span data-ttu-id="b9e47-164">A Storm EventHub olvasó minta</span><span class="sxs-lookup"><span data-stu-id="b9e47-164">Storm EventHub Reader Sample</span></span> |<span data-ttu-id="b9e47-165">Hogyan Azure Event hubs tooread.</span><span class="sxs-lookup"><span data-stu-id="b9e47-165">How tooread from Azure Event Hubs.</span></span> |
| <span data-ttu-id="b9e47-166">A Storm EventHub író minta</span><span class="sxs-lookup"><span data-stu-id="b9e47-166">Storm EventHub Writer Sample</span></span> |<span data-ttu-id="b9e47-167">Hogyan toowrite tooAzure Event Hubs.</span><span class="sxs-lookup"><span data-stu-id="b9e47-167">How toowrite tooAzure Event Hubs.</span></span> |
| <span data-ttu-id="b9e47-168">A Storm HBase olvasó minta</span><span class="sxs-lookup"><span data-stu-id="b9e47-168">Storm HBase Reader Sample</span></span> |<span data-ttu-id="b9e47-169">Hogyan tooread a HBase a HDInsight-fürtök.</span><span class="sxs-lookup"><span data-stu-id="b9e47-169">How tooread from HBase on HDInsight clusters.</span></span> |
| <span data-ttu-id="b9e47-170">A Storm HBase író minta</span><span class="sxs-lookup"><span data-stu-id="b9e47-170">Storm HBase Writer Sample</span></span> |<span data-ttu-id="b9e47-171">Hogyan toowrite tooHBase a HDInsight-fürtök.</span><span class="sxs-lookup"><span data-stu-id="b9e47-171">How toowrite tooHBase on HDInsight clusters.</span></span> |
| <span data-ttu-id="b9e47-172">A Storm hibrid minta</span><span class="sxs-lookup"><span data-stu-id="b9e47-172">Storm Hybrid Sample</span></span> |<span data-ttu-id="b9e47-173">Hogyan toouse egy Java-összetevő.</span><span class="sxs-lookup"><span data-stu-id="b9e47-173">How toouse a Java component.</span></span> |
| <span data-ttu-id="b9e47-174">A Storm-minta</span><span class="sxs-lookup"><span data-stu-id="b9e47-174">Storm Sample</span></span> |<span data-ttu-id="b9e47-175">Alapszintű word-count topológiához.</span><span class="sxs-lookup"><span data-stu-id="b9e47-175">A basic word count topology.</span></span> |

> [!WARNING]
> <span data-ttu-id="b9e47-176">Nem minden sablonok a Linux-alapú hdinsight eszközzel fog működni.</span><span class="sxs-lookup"><span data-stu-id="b9e47-176">Not all templates will work with Linux-based HDInsight.</span></span> <span data-ttu-id="b9e47-177">Hello sablonok által használt Nuget-csomagok nem lehet monó kompatibilis.</span><span class="sxs-lookup"><span data-stu-id="b9e47-177">Nuget packages used by hello templates may not be compatible with Mono.</span></span> <span data-ttu-id="b9e47-178">Ellenőrizze a hello [monó kompatibilitási](http://www.mono-project.com/docs/about-mono/compatibility/) hello használata és dokumentálása [.NET hordozhatóság Analyzer](hdinsight-hadoop-migrate-dotnet-to-linux.md#automated-portability-analysis) tooidentify potenciális problémákat.</span><span class="sxs-lookup"><span data-stu-id="b9e47-178">Check hello [Mono compatibility](http://www.mono-project.com/docs/about-mono/compatibility/) document and use hello [.NET Portability Analyzer](hdinsight-hadoop-migrate-dotnet-to-linux.md#automated-portability-analysis) tooidentify potential problems.</span></span>

<span data-ttu-id="b9e47-179">A jelen dokumentumban leírt lépések hello hello alapszintű Storm-alkalmazás projekt típus toocreate a topológia használja.</span><span class="sxs-lookup"><span data-stu-id="b9e47-179">In hello steps in this document, you use hello basic Storm Application project type toocreate a topology.</span></span>

### <a name="hbase-templates-notes"></a><span data-ttu-id="b9e47-180">A HBase sablonok megjegyzések</span><span class="sxs-lookup"><span data-stu-id="b9e47-180">HBase templates notes</span></span>

<span data-ttu-id="b9e47-181">hello HBase olvasó és író sablonok használata hello HBase REST API-t nem hello HBase Java API-val a HDInsight-fürt HBase toocommunicate.</span><span class="sxs-lookup"><span data-stu-id="b9e47-181">hello HBase reader and writer templates use hello HBase REST API, not hello HBase Java API, toocommunicate with an HBase on HDInsight cluster.</span></span>

### <a name="eventhub-templates-notes"></a><span data-ttu-id="b9e47-182">Az EventHub sablonok megjegyzések</span><span class="sxs-lookup"><span data-stu-id="b9e47-182">EventHub templates notes</span></span>

> [!IMPORTANT]
> <span data-ttu-id="b9e47-183">Java-alapú EventHub hello spout hello alatt futó Storm példatopológiái 3.5-ös vagy újabb verziója nem működik az EventHub-olvasó sablon részét képező összetevő.</span><span class="sxs-lookup"><span data-stu-id="b9e47-183">hello Java-based EventHub spout component included with hello EventHub Reader template may not work with Storm on HDInsight version 3.5 or later.</span></span> <span data-ttu-id="b9e47-184">Ez az összetevő frissített verziója érhető el, [GitHub](https://github.com/hdinsight/hdinsight-storm-examples/tree/master/HDI3.5/lib).</span><span class="sxs-lookup"><span data-stu-id="b9e47-184">An updated version of this component is available at [GitHub](https://github.com/hdinsight/hdinsight-storm-examples/tree/master/HDI3.5/lib).</span></span>

<span data-ttu-id="b9e47-185">Ezt példatopológia összetevő, és együttműködik Storm on HDInsight 3.5, lásd: [GitHub](https://github.com/Azure-Samples/hdinsight-dotnet-java-storm-eventhub).</span><span class="sxs-lookup"><span data-stu-id="b9e47-185">For an example topology that uses this component and works with Storm on HDInsight 3.5, see [GitHub](https://github.com/Azure-Samples/hdinsight-dotnet-java-storm-eventhub).</span></span>

## <a name="create-a-c-topology"></a><span data-ttu-id="b9e47-186">Hozzon létre egy C#-topológiák</span><span class="sxs-lookup"><span data-stu-id="b9e47-186">Create a C# topology</span></span>

1. <span data-ttu-id="b9e47-187">Nyissa meg a Visual Studio, válassza ki **fájl** > **új**, majd válassza ki **projekt**.</span><span class="sxs-lookup"><span data-stu-id="b9e47-187">Open Visual Studio, select **File** > **New**, and then select **Project**.</span></span>

2. <span data-ttu-id="b9e47-188">A hello **új projekt** ablakában bontsa ki a **telepített** > **sablonok**, és válassza ki **Azure Data Lake**.</span><span class="sxs-lookup"><span data-stu-id="b9e47-188">From hello **New Project** window, expand **Installed** > **Templates**, and select **Azure Data Lake**.</span></span> <span data-ttu-id="b9e47-189">Válassza ki a sablonok hello listáról **Storm-alkalmazás**.</span><span class="sxs-lookup"><span data-stu-id="b9e47-189">From hello list of templates, select **Storm Application**.</span></span> <span data-ttu-id="b9e47-190">Hello a hello képernyő aljára, adja meg a **WordCount** hello alkalmazás hello neveként.</span><span class="sxs-lookup"><span data-stu-id="b9e47-190">At hello bottom of hello screen, enter **WordCount** as hello name of hello application.</span></span>

    ![Képernyőfelvétel az új projekt ablakról](./media/hdinsight-storm-develop-csharp-visual-studio-topology/new-project.png)

3. <span data-ttu-id="b9e47-192">Hello projekt létrehozása után rendelkeznie kell a következő fájlok hello:</span><span class="sxs-lookup"><span data-stu-id="b9e47-192">After you have created hello project, you should have hello following files:</span></span>

   * <span data-ttu-id="b9e47-193">**Program.cs**: ezt a fájlt a projekthez hello topológia meghatározása.</span><span class="sxs-lookup"><span data-stu-id="b9e47-193">**Program.cs**: This file defines hello topology for your project.</span></span> <span data-ttu-id="b9e47-194">Alapértelmezés szerint létrejön egy alapértelmezett topológia, amely egy spout és egy bolt áll.</span><span class="sxs-lookup"><span data-stu-id="b9e47-194">A default topology that consists of one spout and one bolt is created by default.</span></span>

   * <span data-ttu-id="b9e47-195">**Spout.cs**: egy példa spout, amely véletlenszerű számok bocsát ki.</span><span class="sxs-lookup"><span data-stu-id="b9e47-195">**Spout.cs**: An example spout that emits random numbers.</span></span>

   * <span data-ttu-id="b9e47-196">**Bolt.cs**: egy példa bolthoz, amely hello spout által kibocsátott számok számát követi.</span><span class="sxs-lookup"><span data-stu-id="b9e47-196">**Bolt.cs**: An example bolt that keeps a count of numbers emitted by hello spout.</span></span>

     <span data-ttu-id="b9e47-197">Hello projekt létrehozásakor NuGet letölti a legújabb hello [SCP.NET csomag](https://www.nuget.org/packages/Microsoft.SCP.Net.SDK/).</span><span class="sxs-lookup"><span data-stu-id="b9e47-197">When you create hello project, NuGet downloads hello latest [SCP.NET package](https://www.nuget.org/packages/Microsoft.SCP.Net.SDK/).</span></span>

     [!INCLUDE [scp.net version important](../../includes/hdinsight-storm-scpdotnet-version.md)]

### <a name="implement-hello-spout"></a><span data-ttu-id="b9e47-198">Alkalmazzon hello spout</span><span class="sxs-lookup"><span data-stu-id="b9e47-198">Implement hello spout</span></span>

1. <span data-ttu-id="b9e47-199">Nyissa meg **Spout.cs**.</span><span class="sxs-lookup"><span data-stu-id="b9e47-199">Open **Spout.cs**.</span></span> <span data-ttu-id="b9e47-200">Spoutokkal kapcsolatban állnak a használt tooread adatok külső forrásból topológiában.</span><span class="sxs-lookup"><span data-stu-id="b9e47-200">Spouts are used tooread data in a topology from an external source.</span></span> <span data-ttu-id="b9e47-201">egy spout hello fő összetevőket a következők:</span><span class="sxs-lookup"><span data-stu-id="b9e47-201">hello main components for a spout are:</span></span>

   * <span data-ttu-id="b9e47-202">**NextTuple**: amikor hello spout engedélyezett tooemit új rekordokat Storm hívja.</span><span class="sxs-lookup"><span data-stu-id="b9e47-202">**NextTuple**: Called by Storm when hello spout is allowed tooemit new tuples.</span></span>

   * <span data-ttu-id="b9e47-203">**Nyugtázási** (csak tranzakciós topológia): a nyugtázás hello topológia hello spout küldött rekordokat más összetevői által kezdeményezett kezeli.</span><span class="sxs-lookup"><span data-stu-id="b9e47-203">**Ack** (transactional topology only): Handles acknowledgements initiated by other components in hello topology for tuples sent from hello spout.</span></span> <span data-ttu-id="b9e47-204">Egy rekord igazolása lehetővé teszi, hogy tudja, hogy megtörtént a feldolgozása sikeresen alsóbb rétegbeli összetevők hello spout.</span><span class="sxs-lookup"><span data-stu-id="b9e47-204">Acknowledging a tuple lets hello spout know that it was processed successfully by downstream components.</span></span>

   * <span data-ttu-id="b9e47-205">**Sikertelen** (csak tranzakciós topológia): rekordokat, amelyek vannak sikertelen – a feldolgozás más összetevői hello topológia kezeli.</span><span class="sxs-lookup"><span data-stu-id="b9e47-205">**Fail** (transactional topology only): Handles tuples that are fail-processing other components in hello topology.</span></span> <span data-ttu-id="b9e47-206">A sikertelen metódusának lehetővé teszi a toore-kibocsátás hello rekordot, hogy újból feldolgozva.</span><span class="sxs-lookup"><span data-stu-id="b9e47-206">Implementing a Fail method allows you toore-emit hello tuple so that it can be processed again.</span></span>

2. <span data-ttu-id="b9e47-207">Cserélje le a hello hello tartalmát **Spout** a következő szöveg hello osztályt.</span><span class="sxs-lookup"><span data-stu-id="b9e47-207">Replace hello contents of hello **Spout** class with hello following text.</span></span> <span data-ttu-id="b9e47-208">A spout véletlenszerűen hello topológia be bocsát ki egy mondat helyett szerepel.</span><span class="sxs-lookup"><span data-stu-id="b9e47-208">This spout randomly emits a sentence into hello topology.</span></span>

    ```csharp
    private Context ctx;
    private Random r = new Random();
    string[] sentences = new string[] {
        "hello cow jumped over hello moon",
        "an apple a day keeps hello doctor away",
        "four score and seven years ago",
        "snow white and hello seven dwarfs",
        "i am at two with nature"
    };

    public Spout(Context ctx)
    {
        // Set hello instance context
        this.ctx = ctx;

        Context.Logger.Info("Generator constructor called");

        // Declare Output schema
        Dictionary<string, List<Type>> outputSchema = new Dictionary<string, List<Type>>();
        // hello schema for hello default output stream is
        // a tuple that contains a string field
        outputSchema.Add("default", new List<Type>() { typeof(string) });
        this.ctx.DeclareComponentSchema(new ComponentStreamSchema(null, outputSchema));
    }

    // Get an instance of hello spout
    public static Spout Get(Context ctx, Dictionary<string, Object> parms)
    {
        return new Spout(ctx);
    }

    public void NextTuple(Dictionary<string, Object> parms)
    {
        Context.Logger.Info("NextTuple enter");
        // hello sentence toobe emitted
        string sentence;

        // Get a random sentence
        sentence = sentences[r.Next(0, sentences.Length - 1)];
        Context.Logger.Info("Emit: {0}", sentence);
        // Emit it
        this.ctx.Emit(new Values(sentence));

        Context.Logger.Info("NextTuple exit");
    }

    public void Ack(long seqId, Dictionary<string, Object> parms)
    {
        // Only used for transactional topologies
    }

    public void Fail(long seqId, Dictionary<string, Object> parms)
    {
        // Only used for transactional topologies
    }
    ```

### <a name="implement-hello-bolts"></a><span data-ttu-id="b9e47-209">Hello boltokhoz megvalósítása</span><span class="sxs-lookup"><span data-stu-id="b9e47-209">Implement hello bolts</span></span>

1. <span data-ttu-id="b9e47-210">Törölje a meglévő hello **Bolt.cs** fájl hello projektből.</span><span class="sxs-lookup"><span data-stu-id="b9e47-210">Delete hello existing **Bolt.cs** file from hello project.</span></span>

2. <span data-ttu-id="b9e47-211">A **Megoldáskezelőben**, kattintson a jobb gombbal a hello projektet, és válassza ki **Hozzáadás** > **új elem**.</span><span class="sxs-lookup"><span data-stu-id="b9e47-211">In **Solution Explorer**, right-click hello project, and select **Add** > **New item**.</span></span> <span data-ttu-id="b9e47-212">Hello listájából válassza ki **Storm Bolt**, és írja be **Splitter.cs** hello neveként.</span><span class="sxs-lookup"><span data-stu-id="b9e47-212">From hello list, select **Storm Bolt**, and enter **Splitter.cs** as hello name.</span></span> <span data-ttu-id="b9e47-213">Ismételje meg a második bolt nevű folyamat toocreate **Counter.cs**.</span><span class="sxs-lookup"><span data-stu-id="b9e47-213">Repeat this process toocreate a second bolt named **Counter.cs**.</span></span>

   * <span data-ttu-id="b9e47-214">**Splitter.cs**: egy olyan bolthoz, amely felosztja a mondatok egyedi szót, és megfelelően kibocsát egy új adatfolyam szavak valósítja meg.</span><span class="sxs-lookup"><span data-stu-id="b9e47-214">**Splitter.cs**: Implements a bolt that splits sentences into individual words, and emits a new stream of words.</span></span>

   * <span data-ttu-id="b9e47-215">**Counter.cs**: valósít meg olyan bolthoz, amely megjeleníti minden szó, és megfelelően kibocsát egy új adatfolyam szavak és minden szó hello száma.</span><span class="sxs-lookup"><span data-stu-id="b9e47-215">**Counter.cs**: Implements a bolt that counts each word, and emits a new stream of words and hello count for each word.</span></span>

     > [!NOTE]
     > <span data-ttu-id="b9e47-216">Ezek a boltokhoz írási és olvasási toostreams, de egy bolt toocommunicate is használható adatforrások, például egy adatbázis vagy a szolgáltatás.</span><span class="sxs-lookup"><span data-stu-id="b9e47-216">These bolts read and write toostreams, but you can also use a bolt toocommunicate with sources such as a database or service.</span></span>

3. <span data-ttu-id="b9e47-217">Nyissa meg **Splitter.cs**.</span><span class="sxs-lookup"><span data-stu-id="b9e47-217">Open **Splitter.cs**.</span></span> <span data-ttu-id="b9e47-218">Alapértelmezés szerint csak egyetlen módszer van: **Execute**.</span><span class="sxs-lookup"><span data-stu-id="b9e47-218">It has only one method by default: **Execute**.</span></span> <span data-ttu-id="b9e47-219">hello Execute metódus neve feldolgozásra rekordot hello bolt fogadásakor.</span><span class="sxs-lookup"><span data-stu-id="b9e47-219">hello Execute method is called when hello bolt receives a tuple for processing.</span></span> <span data-ttu-id="b9e47-220">Itt el tudja olvasni és feldolgozni a bejövő rekordokat, és a kibocsátás kimenő rekordokat.</span><span class="sxs-lookup"><span data-stu-id="b9e47-220">Here, you can read and process incoming tuples, and emit outbound tuples.</span></span>

4. <span data-ttu-id="b9e47-221">Cserélje le a hello hello tartalmát **elválasztó** hello kód a következő osztályra:</span><span class="sxs-lookup"><span data-stu-id="b9e47-221">Replace hello contents of hello **Splitter** class with hello following code:</span></span>

    ```csharp
    private Context ctx;

    // Constructor
    public Splitter(Context ctx)
    {
        Context.Logger.Info("Splitter constructor called");
        this.ctx = ctx;

        // Declare Input and Output schemas
        Dictionary<string, List<Type>> inputSchema = new Dictionary<string, List<Type>>();
        // Input contains a tuple with a string field (hello sentence)
        inputSchema.Add("default", new List<Type>() { typeof(string) });
        Dictionary<string, List<Type>> outputSchema = new Dictionary<string, List<Type>>();
        // Outbound contains a tuple with a string field (hello word)
        outputSchema.Add("default", new List<Type>() { typeof(string) });
        this.ctx.DeclareComponentSchema(new ComponentStreamSchema(inputSchema, outputSchema));
    }

    // Get a new instance of hello bolt
    public static Splitter Get(Context ctx, Dictionary<string, Object> parms)
    {
        return new Splitter(ctx);
    }

    // Called when a new tuple is available
    public void Execute(SCPTuple tuple)
    {
        Context.Logger.Info("Execute enter");

        // Get hello sentence from hello tuple
        string sentence = tuple.GetString(0);
        // Split at space characters
        foreach (string word in sentence.Split(' '))
        {
            Context.Logger.Info("Emit: {0}", word);
            //Emit each word
            this.ctx.Emit(new Values(word));
        }

        Context.Logger.Info("Execute exit");
    }
    ```

5. <span data-ttu-id="b9e47-222">Nyissa meg **Counter.cs**, és cserélje ki hello osztály tartalmát hello következőre:</span><span class="sxs-lookup"><span data-stu-id="b9e47-222">Open **Counter.cs**, and replace hello class contents with hello following:</span></span>

    ```csharp
    private Context ctx;

    // Dictionary for holding words and counts
    private Dictionary<string, int> counts = new Dictionary<string, int>();

    // Constructor
    public Counter(Context ctx)
    {
        Context.Logger.Info("Counter constructor called");
        // Set instance context
        this.ctx = ctx;

        // Declare Input and Output schemas
        Dictionary<string, List<Type>> inputSchema = new Dictionary<string, List<Type>>();
        // A tuple containing a string field - hello word
        inputSchema.Add("default", new List<Type>() { typeof(string) });

        Dictionary<string, List<Type>> outputSchema = new Dictionary<string, List<Type>>();
        // A tuple containing a string and integer field - hello word and hello word count
        outputSchema.Add("default", new List<Type>() { typeof(string), typeof(int) });
        this.ctx.DeclareComponentSchema(new ComponentStreamSchema(inputSchema, outputSchema));
    }

    // Get a new instance
    public static Counter Get(Context ctx, Dictionary<string, Object> parms)
    {
        return new Counter(ctx);
    }

    // Called when a new tuple is available
    public void Execute(SCPTuple tuple)
    {
        Context.Logger.Info("Execute enter");

        // Get hello word from hello tuple
        string word = tuple.GetString(0);
        // Do we already have an entry for hello word in hello dictionary?
        // If no, create one with a count of 0
        int count = counts.ContainsKey(word) ? counts[word] : 0;
        // Increment hello count
        count++;
        // Update hello count in hello dictionary
        counts[word] = count;

        Context.Logger.Info("Emit: {0}, count: {1}", word, count);
        // Emit hello word and count information
        this.ctx.Emit(Constants.DEFAULT_STREAM_ID, new List<SCPTuple> { tuple }, new Values(word, count));
        Context.Logger.Info("Execute exit");
    }
    ```

### <a name="define-hello-topology"></a><span data-ttu-id="b9e47-223">Hello topológia meghatározása</span><span class="sxs-lookup"><span data-stu-id="b9e47-223">Define hello topology</span></span>

<span data-ttu-id="b9e47-224">A grafikus, amely meghatározza, hogyan hello közötti adatáramlás összetevők spoutokkal kapcsolatban és boltokhoz vannak rendezve.</span><span class="sxs-lookup"><span data-stu-id="b9e47-224">Spouts and bolts are arranged in a graph, which defines how hello data flows between components.</span></span> <span data-ttu-id="b9e47-225">Az ebben a topológiában hello graph a következőképpen történik:</span><span class="sxs-lookup"><span data-stu-id="b9e47-225">For this topology, hello graph is as follows:</span></span>

![Hogyan vannak elrendezve összetevők ábrája](./media/hdinsight-storm-develop-csharp-visual-studio-topology/wordcount-topology.png)

<span data-ttu-id="b9e47-227">A mondatok hello spout a kibocsátott, és a hello elválasztó bolt elosztott tooinstances.</span><span class="sxs-lookup"><span data-stu-id="b9e47-227">Sentences are emitted from hello spout, and are distributed tooinstances of hello Splitter bolt.</span></span> <span data-ttu-id="b9e47-228">hello elválasztó bolt bontja szavakat, amelyek elosztott toohello számláló bolt hello mondatokat.</span><span class="sxs-lookup"><span data-stu-id="b9e47-228">hello Splitter bolt breaks hello sentences into words, which are distributed toohello Counter bolt.</span></span>

<span data-ttu-id="b9e47-229">Szószámot helyileg tárolt hello teljesítményszámláló-példány, mert azt szeretnénk, ha meg arról, hogy a szavakat toohello flow toomake számláló bolt példányt.</span><span class="sxs-lookup"><span data-stu-id="b9e47-229">Because word count is held locally in hello Counter instance, we want toomake sure that specific words flow toohello same Counter bolt instance.</span></span> <span data-ttu-id="b9e47-230">Minden példány nyomon követi a szavakat.</span><span class="sxs-lookup"><span data-stu-id="b9e47-230">Each instance keeps track of specific words.</span></span> <span data-ttu-id="b9e47-231">Hello elválasztó bolt nem tart fenn, mert tényleg mindegy hello elválasztó példányok kap mely mondat helyett szerepel.</span><span class="sxs-lookup"><span data-stu-id="b9e47-231">Since hello Splitter bolt maintains no state, it really doesn't matter which instance of hello splitter receives which sentence.</span></span>

<span data-ttu-id="b9e47-232">Nyissa meg **Program.cs**.</span><span class="sxs-lookup"><span data-stu-id="b9e47-232">Open **Program.cs**.</span></span> <span data-ttu-id="b9e47-233">hello fontos módszer **GetTopologyBuilder**, ami által használt toodefine hello topológia beküldött tooStorm.</span><span class="sxs-lookup"><span data-stu-id="b9e47-233">hello important method is **GetTopologyBuilder**, which is used toodefine hello topology that is submitted tooStorm.</span></span> <span data-ttu-id="b9e47-234">Cserélje le a hello tartalmát **GetTopologyBuilder** a következő kód tooimplement hello topológia a fentiekben ismertetett hello:</span><span class="sxs-lookup"><span data-stu-id="b9e47-234">Replace hello contents of **GetTopologyBuilder** with hello following code tooimplement hello topology described previously:</span></span>

```csharp
// Create a new topology named 'WordCount'
TopologyBuilder topologyBuilder = new TopologyBuilder("WordCount" + DateTime.Now.ToString("yyyyMMddHHmmss"));

// Add hello spout toohello topology.
// Name hello component 'sentences'
// Name hello field that is emitted as 'sentence'
topologyBuilder.SetSpout(
    "sentences",
    Spout.Get,
    new Dictionary<string, List<string>>()
    {
        {Constants.DEFAULT_STREAM_ID, new List<string>(){"sentence"}}
    },
    1);
// Add hello splitter bolt toohello topology.
// Name hello component 'splitter'
// Name hello field that is emitted 'word'
// Use suffleGrouping toodistribute incoming tuples
//   from hello 'sentences' spout across instances
//   of hello splitter
topologyBuilder.SetBolt(
    "splitter",
    Splitter.Get,
    new Dictionary<string, List<string>>()
    {
        {Constants.DEFAULT_STREAM_ID, new List<string>(){"word"}}
    },
    1).shuffleGrouping("sentences");

// Add hello counter bolt toohello topology.
// Name hello component 'counter'
// Name hello fields that are emitted 'word' and 'count'
// Use fieldsGrouping tooensure that tuples are routed
//   toocounter instances based on hello contents of field
//   position 0 (hello word). This could also have been
//   List<string>(){"word"}.
//   This ensures that hello word 'jumped', for example, will always
//   go toohello same instance
topologyBuilder.SetBolt(
    "counter",
    Counter.Get,
    new Dictionary<string, List<string>>()
    {
        {Constants.DEFAULT_STREAM_ID, new List<string>(){"word", "count"}}
    },
    1).fieldsGrouping("splitter", new List<int>() { 0 });

// Add topology config
topologyBuilder.SetTopologyConfig(new Dictionary<string, string>()
{
    {"topology.kryo.register","[\"[B\"]"}
});

return topologyBuilder;
```

## <a name="submit-hello-topology"></a><span data-ttu-id="b9e47-235">Küldje el a hello topológia</span><span class="sxs-lookup"><span data-stu-id="b9e47-235">Submit hello topology</span></span>

1. <span data-ttu-id="b9e47-236">A **Megoldáskezelőben**, kattintson a jobb gombbal a hello projektet, és válassza ki **elküldeni a HDInsight tooStorm**.</span><span class="sxs-lookup"><span data-stu-id="b9e47-236">In **Solution Explorer**, right-click hello project, and select **Submit tooStorm on HDInsight**.</span></span>

   > [!NOTE]
   > <span data-ttu-id="b9e47-237">Ha a rendszer kéri, adja meg hello hitelesítő adatait az Azure-előfizetéshez.</span><span class="sxs-lookup"><span data-stu-id="b9e47-237">If prompted, enter hello credentials for your Azure subscription.</span></span> <span data-ttu-id="b9e47-238">Ha egynél több előfizetéssel rendelkezik, jelentkezzen be a Storm on HDInsight-fürt tartalmazó toohello.</span><span class="sxs-lookup"><span data-stu-id="b9e47-238">If you have more than one subscription, sign in toohello one that contains your Storm on HDInsight cluster.</span></span>

2. <span data-ttu-id="b9e47-239">Válassza ki a Storm on HDInsight-fürt hello **Storm-fürt** legördülő listából válassza ki, és válassza **Submit**.</span><span class="sxs-lookup"><span data-stu-id="b9e47-239">Select your Storm on HDInsight cluster from hello **Storm Cluster** drop-down list, and then select **Submit**.</span></span> <span data-ttu-id="b9e47-240">Ha hello elküldése sikeres hello segítségével figyelheti **kimeneti** ablak.</span><span class="sxs-lookup"><span data-stu-id="b9e47-240">You can monitor if hello submission is successful by using hello **Output** window.</span></span>

3. <span data-ttu-id="b9e47-241">Amikor hello topológia sikeresen elküldte, hello **Storm-topológiák** a hello fürt megjelenjen-e.</span><span class="sxs-lookup"><span data-stu-id="b9e47-241">When hello topology has been successfully submitted, hello **Storm Topologies** for hello cluster should appear.</span></span> <span data-ttu-id="b9e47-242">Jelölje be hello **WordCount** hello lista tooview információt a futtató topológia hello topológia.</span><span class="sxs-lookup"><span data-stu-id="b9e47-242">Select hello **WordCount** topology from hello list tooview information about hello running topology.</span></span>

   > [!NOTE]
   > <span data-ttu-id="b9e47-243">Megtekintheti továbbá **Storm-topológiák** a **Server Explorer**.</span><span class="sxs-lookup"><span data-stu-id="b9e47-243">You can also view **Storm Topologies** from **Server Explorer**.</span></span> <span data-ttu-id="b9e47-244">Bontsa ki a **Azure** > **HDInsight**, kattintson a jobb gombbal a Storm on HDInsight-fürt, majd válassza ki **nézet Storm-topológiák**.</span><span class="sxs-lookup"><span data-stu-id="b9e47-244">Expand **Azure** > **HDInsight**, right-click a Storm on HDInsight cluster, and then select **View Storm Topologies**.</span></span>

    <span data-ttu-id="b9e47-245">hello összetevők hello topológia tooview adatait hello összetevő hello diagramban kattintson duplán.</span><span class="sxs-lookup"><span data-stu-id="b9e47-245">tooview information about hello components in hello topology, double-click hello component in hello diagram.</span></span>

4. <span data-ttu-id="b9e47-246">A hello **topológia összegzése** megtekintéséhez kattintson az **Kill** toostop hello topológia.</span><span class="sxs-lookup"><span data-stu-id="b9e47-246">From hello **Topology Summary** view, click **Kill** toostop hello topology.</span></span>

   > [!NOTE]
   > <span data-ttu-id="b9e47-247">Storm-topológiák folytatható toorun, amíg azok inaktiválása vagy hello fürtök törlése.</span><span class="sxs-lookup"><span data-stu-id="b9e47-247">Storm topologies continue toorun until they are deactivated, or hello cluster is deleted.</span></span>

## <a name="transactional-topology"></a><span data-ttu-id="b9e47-248">Tranzakciós topológia</span><span class="sxs-lookup"><span data-stu-id="b9e47-248">Transactional topology</span></span>

<span data-ttu-id="b9e47-249">hello előző topológia nem tranzakciós.</span><span class="sxs-lookup"><span data-stu-id="b9e47-249">hello previous topology is non-transactional.</span></span> <span data-ttu-id="b9e47-250">hello összetevők hello topológiában nem valósítja meg a funkciók tooreplaying üzeneteket.</span><span class="sxs-lookup"><span data-stu-id="b9e47-250">hello components in hello topology do not implement functionality tooreplaying messages.</span></span> <span data-ttu-id="b9e47-251">Például egy tranzakciós topológia, hozzon létre egy projektet, és válassza **Storm minta** hello projekt típusként.</span><span class="sxs-lookup"><span data-stu-id="b9e47-251">For an example of a transactional topology, create a project and select **Storm Sample** as hello project type.</span></span>

<span data-ttu-id="b9e47-252">Tranzakciós topológiák valósítja meg a következő adatok toosupport ismétlés hello:</span><span class="sxs-lookup"><span data-stu-id="b9e47-252">Transactional topologies implement hello following toosupport replay of data:</span></span>

* <span data-ttu-id="b9e47-253">**Metaadatok gyorsítótárazása**: hello spout kell tárolnia, így hello adatokat beolvasni, és újra kibocsátott, ha hiba történik a kibocsátott hello adatokkal kapcsolatos metaadatokat.</span><span class="sxs-lookup"><span data-stu-id="b9e47-253">**Metadata caching**: hello spout must store metadata about hello data emitted, so that hello data can be retrieved and emitted again if a failure occurs.</span></span> <span data-ttu-id="b9e47-254">Mert hello minta által kibocsátott hello adatok kicsi, a táblakonstruktor minden rekordjának hello nyers adatok a visszajátszás dictionary tárolja.</span><span class="sxs-lookup"><span data-stu-id="b9e47-254">Because hello data emitted by hello sample is small, hello raw data for each tuple is stored in a dictionary for replay.</span></span>

* <span data-ttu-id="b9e47-255">**Nyugtázási**: hello topológiában minden bolt meghívhatja `this.ctx.Ack(tuple)` tooacknowledge, hogy sikeresen feldolgozta-rekordot.</span><span class="sxs-lookup"><span data-stu-id="b9e47-255">**Ack**: Each bolt in hello topology can call `this.ctx.Ack(tuple)` tooacknowledge that it has successfully processed a tuple.</span></span> <span data-ttu-id="b9e47-256">Ha az összes boltokhoz korrektúrák hello rekordot, hello `Ack` hello spout metódusában meghívták.</span><span class="sxs-lookup"><span data-stu-id="b9e47-256">When all bolts have acked hello tuple, hello `Ack` method of hello spout is invoked.</span></span> <span data-ttu-id="b9e47-257">Hello `Ack` módszer lehetővé teszi, hogy a hello spout tooremove adatok, amelyek a visszajátszás gyorsítótárazva lett.</span><span class="sxs-lookup"><span data-stu-id="b9e47-257">hello `Ack` method allows hello spout tooremove data that was cached for replay.</span></span>

* <span data-ttu-id="b9e47-258">**Sikertelen**: minden bolt meghívhatja `this.ctx.Fail(tuple)` tooindicate a feldolgozása sikertelen volt a rekordot.</span><span class="sxs-lookup"><span data-stu-id="b9e47-258">**Fail**: Each bolt can call `this.ctx.Fail(tuple)` tooindicate that processing has failed for a tuple.</span></span> <span data-ttu-id="b9e47-259">hello hiba lejjebb toohello `Fail` metódusában hello spout, ahol hello rekordot is bejegyzéseit meg kell ismételni a gyorsítótárazott metaadatok.</span><span class="sxs-lookup"><span data-stu-id="b9e47-259">hello failure propagates toohello `Fail` method of hello spout, where hello tuple can be replayed by using cached metadata.</span></span>

* <span data-ttu-id="b9e47-260">**A feladatütemezési azonosító**: egy rekord kibocsátó, amikor egy egyedi Sorozatazonosító adható meg.</span><span class="sxs-lookup"><span data-stu-id="b9e47-260">**Sequence ID**: When emitting a tuple, a unique sequence ID can be specified.</span></span> <span data-ttu-id="b9e47-261">Ez az érték hello rekordot a visszajátszás (Ack és sikertelen) feldolgozás azonosítja.</span><span class="sxs-lookup"><span data-stu-id="b9e47-261">This value identifies hello tuple for replay (Ack and Fail) processing.</span></span> <span data-ttu-id="b9e47-262">Például hello a hello spout **Storm minta** projekt hello következő használja, amikor adatokat kibocsátó:</span><span class="sxs-lookup"><span data-stu-id="b9e47-262">For example, hello spout in hello **Storm Sample** project uses hello following when emitting data:</span></span>

        this.ctx.Emit(Constants.DEFAULT_STREAM_ID, new Values(sentence), lastSeqId);

    <span data-ttu-id="b9e47-263">Ez a kód megfelelően kibocsát egy rekord, amely tartalmazza a mondatok toohello alapértelmezett adatfolyam hello feladatütemezési azonosítóérték található **lastSeqId**.</span><span class="sxs-lookup"><span data-stu-id="b9e47-263">This code emits a tuple that contains a sentence toohello default stream, with hello sequence ID value contained in **lastSeqId**.</span></span> <span data-ttu-id="b9e47-264">Ehhez a példához **lastSeqId** értéke eggyel növekszik, a kibocsátott minden rekordot.</span><span class="sxs-lookup"><span data-stu-id="b9e47-264">For this example, **lastSeqId** is incremented for every tuple emitted.</span></span>

<span data-ttu-id="b9e47-265">Ahogyan az hello **Storm minta** projekt, a tranzakciós-e egy összetevő lehet futásidőben, a konfiguráció alapján állítsa be.</span><span class="sxs-lookup"><span data-stu-id="b9e47-265">As demonstrated in hello **Storm Sample** project, whether a component is transactional can be set at runtime, based on configuration.</span></span>

## <a name="hybrid-topology-with-c-and-java"></a><span data-ttu-id="b9e47-266">Hibrid C# és Java topológia</span><span class="sxs-lookup"><span data-stu-id="b9e47-266">Hybrid topology with C# and Java</span></span>

<span data-ttu-id="b9e47-267">Data Lake tools is használhatja a Visual Studio toocreate hibrid topológiák, ha néhány összetevőt C# és Java.</span><span class="sxs-lookup"><span data-stu-id="b9e47-267">You can also use Data Lake tools for Visual Studio toocreate hybrid topologies, where some components are C# and others are Java.</span></span>

<span data-ttu-id="b9e47-268">Példa egy hibrid topológia, hozzon létre egy projektet, és jelölje ki **Storm hibrid minta**.</span><span class="sxs-lookup"><span data-stu-id="b9e47-268">For an example of a hybrid topology, create a project and select **Storm Hybrid Sample**.</span></span> <span data-ttu-id="b9e47-269">Ez a minta-típus a következő fogalmak hello mutatja be:</span><span class="sxs-lookup"><span data-stu-id="b9e47-269">This sample type demonstrates hello following concepts:</span></span>

* <span data-ttu-id="b9e47-270">**Java spout** és **C# bolt**: az **HybridTopology_javaSpout_csharpBolt**.</span><span class="sxs-lookup"><span data-stu-id="b9e47-270">**Java spout** and **C# bolt**: Defined in **HybridTopology_javaSpout_csharpBolt**.</span></span>

    * <span data-ttu-id="b9e47-271">A tranzakciós verzió van megadva **HybridTopologyTx_javaSpout_csharpBolt**.</span><span class="sxs-lookup"><span data-stu-id="b9e47-271">A transactional version is defined in **HybridTopologyTx_javaSpout_csharpBolt**.</span></span>

* <span data-ttu-id="b9e47-272">**C# spout** és **Java bolt**: az **HybridTopology_csharpSpout_javaBolt**.</span><span class="sxs-lookup"><span data-stu-id="b9e47-272">**C# spout** and **Java bolt**: Defined in **HybridTopology_csharpSpout_javaBolt**.</span></span>

    * <span data-ttu-id="b9e47-273">A tranzakciós verzió van megadva **HybridTopologyTx_csharpSpout_javaBolt**.</span><span class="sxs-lookup"><span data-stu-id="b9e47-273">A transactional version is defined in **HybridTopologyTx_csharpSpout_javaBolt**.</span></span>

  > [!NOTE]
  > <span data-ttu-id="b9e47-274">Ebben a verzióban is bemutatja, hogyan toouse Clojure code Java összetevőjeként szövegfájlból.</span><span class="sxs-lookup"><span data-stu-id="b9e47-274">This version also demonstrates how toouse Clojure code from a text file as a Java component.</span></span>


<span data-ttu-id="b9e47-275">hello projekt elküldésekor használt tooswitch hello topológia egyszerűen át hello `[Active(true)]` utasítás toohello topológiáját toouse, mielőtt elküldi azt toohello fürt.</span><span class="sxs-lookup"><span data-stu-id="b9e47-275">tooswitch hello topology that is used when hello project is submitted, simply move hello `[Active(true)]` statement toohello topology you want toouse, before submitting it toohello cluster.</span></span>

> [!NOTE]
> <span data-ttu-id="b9e47-276">A projekt hello részeként biztosított összes hello Java szükséges fájlok **JavaDependency** mappa.</span><span class="sxs-lookup"><span data-stu-id="b9e47-276">All hello Java files that are required are provided as part of this project in hello **JavaDependency** folder.</span></span>

<span data-ttu-id="b9e47-277">Vegye figyelembe hello következőket létrehozása és elküldése a hibrid topológia:</span><span class="sxs-lookup"><span data-stu-id="b9e47-277">Consider hello following when you are creating and submitting a hybrid topology:</span></span>

* <span data-ttu-id="b9e47-278">Használjon **JavaComponentConstructor** toocreate hello adott spout vagy bolt a Java-osztály példánya.</span><span class="sxs-lookup"><span data-stu-id="b9e47-278">You must use **JavaComponentConstructor** toocreate an instance of hello Java class for a spout or bolt.</span></span>

* <span data-ttu-id="b9e47-279">Használjon **microsoft.scp.storm.multilang.CustomizedInteropJSONSerializer** tooserialize virtuális gépbe vagy onnan Java-összetevőt Java-adatobjektumok tooJSON.</span><span class="sxs-lookup"><span data-stu-id="b9e47-279">You should use **microsoft.scp.storm.multilang.CustomizedInteropJSONSerializer** tooserialize data into or out of Java components from Java objects tooJSON.</span></span>

* <span data-ttu-id="b9e47-280">Hello topológia toohello server elküldésekor használnia kell a hello **további konfigurációs** beállítás toospecify hello **Java elérési utat**.</span><span class="sxs-lookup"><span data-stu-id="b9e47-280">When submitting hello topology toohello server, you must use hello **Additional configurations** option toospecify hello **Java File paths**.</span></span> <span data-ttu-id="b9e47-281">hello elérési út van megadva a Java-osztályok tartalmazó JAR fájlok hello tartalmazó hello könyvtár kell lennie.</span><span class="sxs-lookup"><span data-stu-id="b9e47-281">hello path specified should be hello directory that contains hello JAR files that contain your Java classes.</span></span>

### <a name="azure-event-hubs"></a><span data-ttu-id="b9e47-282">Azure Event Hubs</span><span class="sxs-lookup"><span data-stu-id="b9e47-282">Azure Event Hubs</span></span>

<span data-ttu-id="b9e47-283">SCP.NET 0.9.4.203 tartalmazza egy új osztályt és metódus kifejezetten a hello Event Hub spout (a Java spout az Event Hubs olvasó) használata.</span><span class="sxs-lookup"><span data-stu-id="b9e47-283">SCP.NET version 0.9.4.203 introduces a new class and method specifically for working with hello Event Hub spout (a Java spout that reads from Event Hubs).</span></span> <span data-ttu-id="b9e47-284">A topológia egy Event Hub spout használó létrehozásakor a következő módszerek hello használata:</span><span class="sxs-lookup"><span data-stu-id="b9e47-284">When you create a topology that uses an Event Hub spout, use hello following methods:</span></span>

* <span data-ttu-id="b9e47-285">**EventHubSpoutConfig** osztály: hoz létre egy objektumot, amely hello spout összetevő hello konfigurációját tartalmazza.</span><span class="sxs-lookup"><span data-stu-id="b9e47-285">**EventHubSpoutConfig** class: Creates an object that contains hello configuration for hello spout component.</span></span>

* <span data-ttu-id="b9e47-286">**TopologyBuilder.SetEventHubSpout** metódus: hello Event Hub spout összetevő toohello topológia hozzáadja.</span><span class="sxs-lookup"><span data-stu-id="b9e47-286">**TopologyBuilder.SetEventHubSpout** method: Adds hello Event Hub spout component toohello topology.</span></span>

> [!NOTE]
> <span data-ttu-id="b9e47-287">Továbbra is használnia kell hello **CustomizedInteropJSONSerializer** hello spout által létrehozott tooserialize adatok.</span><span class="sxs-lookup"><span data-stu-id="b9e47-287">You must still use hello **CustomizedInteropJSONSerializer** tooserialize data produced by hello spout.</span></span>

## <span data-ttu-id="b9e47-288"><a id="configurationmanager"></a>ConfigurationManager használata</span><span class="sxs-lookup"><span data-stu-id="b9e47-288"><a id="configurationmanager"></a>Use ConfigurationManager</span></span>

<span data-ttu-id="b9e47-289">Ne használjon **ConfigurationManager** tooretrieve konfigurációs értékei a boltok és összetevők spout.</span><span class="sxs-lookup"><span data-stu-id="b9e47-289">Don't use **ConfigurationManager** tooretrieve configuration values from bolt and spout components.</span></span> <span data-ttu-id="b9e47-290">Így okozhat, null mutató által-kivétel.</span><span class="sxs-lookup"><span data-stu-id="b9e47-290">Doing so can cause a null pointer exception.</span></span> <span data-ttu-id="b9e47-291">Ehelyett a projekthez hello konfigurációs átad a Storm-topológia hello hello topológia környezetben kulcs-érték párban.</span><span class="sxs-lookup"><span data-stu-id="b9e47-291">Instead, hello configuration for your project is passed into hello Storm topology as a key and value pair in hello topology context.</span></span> <span data-ttu-id="b9e47-292">Minden egyes összetevő, amely támaszkodik a konfigurációs értékeket kell kérheti le azokat a hello környezetből inicializálása során.</span><span class="sxs-lookup"><span data-stu-id="b9e47-292">Each component that relies on configuration values must retrieve them from hello context during initialization.</span></span>

<span data-ttu-id="b9e47-293">hello következő kód bemutatja, hogyan tooretrieve ezeket az értékeket:</span><span class="sxs-lookup"><span data-stu-id="b9e47-293">hello following code demonstrates how tooretrieve these values:</span></span>

```csharp
public class MyComponent : ISCPBolt
{
    // toohold configuration information loaded from context
    Configuration configuration;
    ...
    public MyComponent(Context ctx, Dictionary<string, Object> parms)
    {
        // Save a copy of hello context for this component instance
        this.ctx = ctx;
        // If it exists, load hello configuration for hello component
        if(parms.ContainsKey(Constants.USER_CONFIG))
        {
            this.configuration = parms[Constants.USER_CONFIG] as System.Configuration.Configuration;
        }
        // Retrieve hello value of "Foo" from configuration
        var foo = this.configuration.AppSettings.Settings["Foo"].Value;
    }
    ...
}
```

<span data-ttu-id="b9e47-294">Ha egy `Get` metódus tooreturn egy példányát az összetevőt, győződjön meg arról, hogy mindkét hello haladnak `Context` és `Dictionary<string, Object>` paraméterek toohello konstruktor.</span><span class="sxs-lookup"><span data-stu-id="b9e47-294">If you use a `Get` method tooreturn an instance of your component, you must ensure that it passes both hello `Context` and `Dictionary<string, Object>` parameters toohello constructor.</span></span> <span data-ttu-id="b9e47-295">hello alábbi példa az alapvető `Get` módszer, amelynek megfelelően továbbítja ezeket az értékeket:</span><span class="sxs-lookup"><span data-stu-id="b9e47-295">hello following example is a basic `Get` method that properly passes these values:</span></span>

```csharp
public static MyComponent Get(Context ctx, Dictionary<string, Object> parms)
{
    return new MyComponent(ctx, parms);
}
```

## <a name="how-tooupdate-scpnet"></a><span data-ttu-id="b9e47-296">Hogyan tooupdate SCP.NET</span><span class="sxs-lookup"><span data-stu-id="b9e47-296">How tooupdate SCP.NET</span></span>

<span data-ttu-id="b9e47-297">SCP.NET újabb kiadásaiban a Nugeten keresztül Csomagfrissítés támogatja.</span><span class="sxs-lookup"><span data-stu-id="b9e47-297">Recent releases of SCP.NET support package upgrade through NuGet.</span></span> <span data-ttu-id="b9e47-298">Ha egy új frissítés érhető el, egy frissítési értesítést kap.</span><span class="sxs-lookup"><span data-stu-id="b9e47-298">When a new update is available, you receive an upgrade notification.</span></span> <span data-ttu-id="b9e47-299">a frissítés keresése toomanually kövesse az alábbi lépéseket:</span><span class="sxs-lookup"><span data-stu-id="b9e47-299">toomanually check for an upgrade, follow these steps:</span></span>

1. <span data-ttu-id="b9e47-300">A **Megoldáskezelőben**, kattintson a jobb gombbal a hello projektet, és válassza ki **NuGet-csomagok kezelése**.</span><span class="sxs-lookup"><span data-stu-id="b9e47-300">In **Solution Explorer**, right-click hello project, and select **Manage NuGet Packages**.</span></span>

2. <span data-ttu-id="b9e47-301">Hello Csomagkezelő, válassza ki **frissítések**.</span><span class="sxs-lookup"><span data-stu-id="b9e47-301">From hello package manager, select **Updates**.</span></span> <span data-ttu-id="b9e47-302">Frissítés érhető el, ha szerepel.</span><span class="sxs-lookup"><span data-stu-id="b9e47-302">If an update is available, it is listed.</span></span> <span data-ttu-id="b9e47-303">Kattintson a **frissítés** a hello csomag tooinstall azt.</span><span class="sxs-lookup"><span data-stu-id="b9e47-303">Click **Update** for hello package tooinstall it.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="b9e47-304">Ha a projekt, amely nem NuGet SCP.NET korábbi verziójával lett létrehozva, a következő lépéseket tooupdate tooa újabb verzióra hello kell elvégeznie:</span><span class="sxs-lookup"><span data-stu-id="b9e47-304">If your project was created with an earlier version of SCP.NET that did not use NuGet, you must perform hello following steps tooupdate tooa newer version:</span></span>
>
> 1. <span data-ttu-id="b9e47-305">A **Megoldáskezelőben**, kattintson a jobb gombbal a hello projektet, és válassza ki **NuGet-csomagok kezelése**.</span><span class="sxs-lookup"><span data-stu-id="b9e47-305">In **Solution Explorer**, right-click hello project, and select **Manage NuGet Packages**.</span></span>
> 2. <span data-ttu-id="b9e47-306">Hello segítségével **keresési** mezőben, keresse meg, és adja hozzá, **Microsoft.SCP.Net.SDK** toohello projekt.</span><span class="sxs-lookup"><span data-stu-id="b9e47-306">Using hello **Search** field, search for, and then add, **Microsoft.SCP.Net.SDK** toohello project.</span></span>

## <a name="troubleshoot-common-issues-with-topologies"></a><span data-ttu-id="b9e47-307">Topológiák termékkel kapcsolatos gyakori hibák elhárítása</span><span class="sxs-lookup"><span data-stu-id="b9e47-307">Troubleshoot common issues with topologies</span></span>

### <a name="null-pointer-exceptions"></a><span data-ttu-id="b9e47-308">NULL mutatót kivételek</span><span class="sxs-lookup"><span data-stu-id="b9e47-308">Null pointer exceptions</span></span>

<span data-ttu-id="b9e47-309">C#-topológiák használatakor a Linux-alapú HDInsight-fürthöz boltok és összetevőket spout **ConfigurationManager** tooread konfigurációs beállításokat futásidőben adhat vissza null mutatót kivételek.</span><span class="sxs-lookup"><span data-stu-id="b9e47-309">When you are using a C# topology with a Linux-based HDInsight cluster, bolt and spout components that use **ConfigurationManager** tooread configuration settings at runtime may return null pointer exceptions.</span></span>

<span data-ttu-id="b9e47-310">a projekt hello konfigurációs hello Storm-topológia átadott hello topológia környezetben kulcs-érték párban.</span><span class="sxs-lookup"><span data-stu-id="b9e47-310">hello configuration for your project is passed into hello Storm topology as a key and value pair in hello topology context.</span></span> <span data-ttu-id="b9e47-311">Tooyour összetevők átadása akkor inicializálásakor hello szótárobjektum lekérhetők.</span><span class="sxs-lookup"><span data-stu-id="b9e47-311">It can be retrieved from hello dictionary object that is passed tooyour components when they are initialized.</span></span>

<span data-ttu-id="b9e47-312">További információkért lásd: hello [ConfigurationManager](#configurationmanager) szakasz ebben a dokumentumban.</span><span class="sxs-lookup"><span data-stu-id="b9e47-312">For more information, see hello [ConfigurationManager](#configurationmanager) section of this document.</span></span>

### <a name="systemtypeloadexception"></a><span data-ttu-id="b9e47-313">System.TypeLoadException</span><span class="sxs-lookup"><span data-stu-id="b9e47-313">System.TypeLoadException</span></span>

<span data-ttu-id="b9e47-314">C#-topológiák használatakor a Linux-alapú HDInsight-fürthöz felmerülhet a következő hiba hello:</span><span class="sxs-lookup"><span data-stu-id="b9e47-314">When you are using a C# topology with a Linux-based HDInsight cluster, you may encounter hello following error:</span></span>

    System.TypeLoadException: Failure has occurred while loading a type.

<span data-ttu-id="b9e47-315">Ez a hiba akkor fordul elő, egy bináris, amely nem kompatibilis a monó támogató .NET hello verziójának használatakor.</span><span class="sxs-lookup"><span data-stu-id="b9e47-315">This error occurs when you use a binary that is not compatible with hello version of .NET that Mono supports.</span></span>

<span data-ttu-id="b9e47-316">Linux-alapú HDInsight-fürtök győződjön meg arról, hogy a projekt használ-e a bináris fájlok a .NET 4.5 számára.</span><span class="sxs-lookup"><span data-stu-id="b9e47-316">For Linux-based HDInsight clusters, make sure that your project uses binaries compiled for .NET 4.5.</span></span>

### <a name="test-a-topology-locally"></a><span data-ttu-id="b9e47-317">A topológia helyi tesztelése</span><span class="sxs-lookup"><span data-stu-id="b9e47-317">Test a topology locally</span></span>

<span data-ttu-id="b9e47-318">Könnyen toodeploy topológia tooa fürtökhöz, néhány esetben azonban szükség lehet a topológia helyileg tootest.</span><span class="sxs-lookup"><span data-stu-id="b9e47-318">Although it is easy toodeploy a topology tooa cluster, in some cases, you may need tootest a topology locally.</span></span> <span data-ttu-id="b9e47-319">A következő lépéseket toorun hello használja, és tesztelje a hello példa topológia ebben az oktatóanyagban helyileg a fejlesztői környezetben.</span><span class="sxs-lookup"><span data-stu-id="b9e47-319">Use hello following steps toorun and test hello example topology in this tutorial locally in your development environment.</span></span>

> [!WARNING]
> <span data-ttu-id="b9e47-320">Helyi tesztelése csak akkor működik, a basic, a C#-topológiák csak.</span><span class="sxs-lookup"><span data-stu-id="b9e47-320">Local testing only works for basic, C#-only topologies.</span></span> <span data-ttu-id="b9e47-321">A hibrid topológiák vagy topológiák több adatfolyam használó helyi tesztelése nem használható.</span><span class="sxs-lookup"><span data-stu-id="b9e47-321">You cannot use local testing for hybrid topologies or topologies that use multiple streams.</span></span>

1. <span data-ttu-id="b9e47-322">A **Megoldáskezelőben**, kattintson a jobb gombbal a hello projektet, és válassza ki **tulajdonságok**.</span><span class="sxs-lookup"><span data-stu-id="b9e47-322">In **Solution Explorer**, right-click hello project, and select **Properties**.</span></span> <span data-ttu-id="b9e47-323">A projekt tulajdonságai hello módosítása hello **kimeneti típus** túl**Konzolalkalmazás**.</span><span class="sxs-lookup"><span data-stu-id="b9e47-323">In hello project properties, change hello **Output type** too**Console Application**.</span></span>

    ![Képernyőfelvétel a projekt tulajdonságait, a kiemelt kimeneti típusú](./media/hdinsight-storm-develop-csharp-visual-studio-topology/outputtype.png)

   > [!NOTE]
   > <span data-ttu-id="b9e47-325">Ne feledje toochange hello **kimeneti típus** túl biztonsági**Class Library** hello topológia tooa fürt központi telepítése előtt.</span><span class="sxs-lookup"><span data-stu-id="b9e47-325">Remember toochange hello **Output type** back too**Class Library** before you deploy hello topology tooa cluster.</span></span>

2. <span data-ttu-id="b9e47-326">A **Megoldáskezelőben**, kattintson a jobb gombbal a hello projektet, és válassza **Hozzáadás** > **új elem**.</span><span class="sxs-lookup"><span data-stu-id="b9e47-326">In **Solution Explorer**, right-click hello project, and then select **Add** > **New Item**.</span></span> <span data-ttu-id="b9e47-327">Válassza ki **osztály**, és írja be **LocalTest.cs** hello osztály neve.</span><span class="sxs-lookup"><span data-stu-id="b9e47-327">Select **Class**, and enter **LocalTest.cs** as hello class name.</span></span> <span data-ttu-id="b9e47-328">Végezetül kattintson **Hozzáadás**.</span><span class="sxs-lookup"><span data-stu-id="b9e47-328">Finally, click **Add**.</span></span>

3. <span data-ttu-id="b9e47-329">Nyissa meg **LocalTest.cs**, és adja hozzá a következő hello **használatával** hello felső utasítást:</span><span class="sxs-lookup"><span data-stu-id="b9e47-329">Open **LocalTest.cs**, and add hello following **using** statement at hello top:</span></span>

    ```csharp
    using Microsoft.SCP;
    ```

4. <span data-ttu-id="b9e47-330">Hello hello tartalmát kód használata hello következő **LocalTest** osztály:</span><span class="sxs-lookup"><span data-stu-id="b9e47-330">Use hello following code as hello contents of hello **LocalTest** class:</span></span>

    ```csharp
    // Drives hello topology components
    public void RunTestCase()
    {
        // An empty dictionary for use when creating components
        Dictionary<string, Object> emptyDictionary = new Dictionary<string, object>();

        #region Test hello spout
        {
            Console.WriteLine("Starting spout");
            // LocalContext is a local-mode context that can be used tooinitialize
            // components in hello development environment.
            LocalContext spoutCtx = LocalContext.Get();
            // Get a new instance of hello spout, using hello local context
            Spout sentences = Spout.Get(spoutCtx, emptyDictionary);

            // Emit 10 tuples
            for (int i = 0; i < 10; i++)
            {
                sentences.NextTuple(emptyDictionary);
            }
            // Use LocalContext toopersist hello data stream toofile
            spoutCtx.WriteMsgQueueToFile("sentences.txt");
            Console.WriteLine("Spout finished");
        }
        #endregion

        #region Test hello splitter bolt
        {
            Console.WriteLine("Starting splitter bolt");
            // LocalContext is a local-mode context that can be used tooinitialize
            // components in hello development environment.
            LocalContext splitterCtx = LocalContext.Get();
            // Get a new instance of hello bolt
            Splitter splitter = Splitter.Get(splitterCtx, emptyDictionary);

            // Set hello data stream toohello data created by hello spout
            splitterCtx.ReadFromFileToMsgQueue("sentences.txt");
            // Get a batch of tuples from hello stream
            List<SCPTuple> batch = splitterCtx.RecvFromMsgQueue();
            // Process each tuple in hello batch
            foreach (SCPTuple tuple in batch)
            {
                splitter.Execute(tuple);
            }
            // Use LocalContext toopersist hello data stream toofile
            splitterCtx.WriteMsgQueueToFile("splitter.txt");
            Console.WriteLine("Splitter bolt finished");
        }
        #endregion

        #region Test hello counter bolt
        {
            Console.WriteLine("Starting counter bolt");
            // LocalContext is a local-mode context that can be used tooinitialize
            // components in hello development environment.
            LocalContext counterCtx = LocalContext.Get();
            // Get a new instance of hello bolt
            Counter counter = Counter.Get(counterCtx, emptyDictionary);

            // Set hello data stream toohello data created by splitter bolt
            counterCtx.ReadFromFileToMsgQueue("splitter.txt");
            // Get a batch of tuples from hello stream
            List<SCPTuple> batch = counterCtx.RecvFromMsgQueue();
            // Process each tuple in hello batch
            foreach (SCPTuple tuple in batch)
            {
                counter.Execute(tuple);
            }
            // Use LocalContext toopersist hello data stream toofile
            counterCtx.WriteMsgQueueToFile("counter.txt");
            Console.WriteLine("Counter bolt finished");
        }
        #endregion
    }
    ```

    <span data-ttu-id="b9e47-331">Eltarthat egy rövid ideig tooread keresztül hello kód megjegyzéseket.</span><span class="sxs-lookup"><span data-stu-id="b9e47-331">Take a moment tooread through hello code comments.</span></span> <span data-ttu-id="b9e47-332">Ezt a kódot használja **LocalContext** toorun hello összetevők hello fejlesztési környezetet, és továbbra is fennáll, hello adatfolyam hello helyi meghajtón lévő összetevők tootext fájlok között.</span><span class="sxs-lookup"><span data-stu-id="b9e47-332">This code uses **LocalContext** toorun hello components in hello development environment, and it persists hello data stream between components tootext files on hello local drive.</span></span>

1. <span data-ttu-id="b9e47-333">Nyissa meg **Program.cs**, és adja hozzá a következő toohello hello **fő** módszert:</span><span class="sxs-lookup"><span data-stu-id="b9e47-333">Open **Program.cs**, and add hello following toohello **Main** method:</span></span>

    ```csharp
    Console.WriteLine("Starting tests");
    System.Environment.SetEnvironmentVariable("microsoft.scp.logPrefix", "WordCount-LocalTest");
    // Initialize hello runtime
    SCPRuntime.Initialize();

    //If we are not running under hello local context, throw an error
    if (Context.pluginType != SCPPluginType.SCP_NET_LOCAL)
    {
        throw new Exception(string.Format("unexpected pluginType: {0}", Context.pluginType));
    }
    // Create test instance
    LocalTest tests = new LocalTest();
    // Run tests
    tests.RunTestCase();
    Console.WriteLine("Tests finished");
    Console.ReadKey();
    ```

2. <span data-ttu-id="b9e47-334">Hello módosítások mentéséhez, és kattintson a **F5** , vagy válasszon **Debug** > **Start Debugging** toostart hello projekt.</span><span class="sxs-lookup"><span data-stu-id="b9e47-334">Save hello changes, and then click **F5** or select **Debug** > **Start Debugging** toostart hello project.</span></span> <span data-ttu-id="b9e47-335">A konzolablakban kell jelenik meg, és állapot naplózása hello tesztek állapotát.</span><span class="sxs-lookup"><span data-stu-id="b9e47-335">A console window should appear, and log status as hello tests progress.</span></span> <span data-ttu-id="b9e47-336">Ha **tesztek befejeződött** jelenik meg, nyomja meg bármelyik kulcs tooclose hello ablakban.</span><span class="sxs-lookup"><span data-stu-id="b9e47-336">When **Tests finished** appears, press any key tooclose hello window.</span></span>

3. <span data-ttu-id="b9e47-337">Használjon **Windows Explorer** toolocate hello a projektet tartalmazó könyvtárat.</span><span class="sxs-lookup"><span data-stu-id="b9e47-337">Use **Windows Explorer** toolocate hello directory that contains your project.</span></span> <span data-ttu-id="b9e47-338">Például: **C:\Users\<your_user_name > \Documents\Visual Studio 2013\Projects\WordCount\WordCount**.</span><span class="sxs-lookup"><span data-stu-id="b9e47-338">For example: **C:\Users\<your_user_name>\Documents\Visual Studio 2013\Projects\WordCount\WordCount**.</span></span> <span data-ttu-id="b9e47-339">Ebben a könyvtárban, nyissa meg a **Bin**, és kattintson a **Debug**.</span><span class="sxs-lookup"><span data-stu-id="b9e47-339">In this directory, open **Bin**, and then click **Debug**.</span></span> <span data-ttu-id="b9e47-340">Hello szöveges fájlok, amelyeket a hello tesztek futtatásakor kell megjelennie: sentences.txt counter.txt és splitter.txt.</span><span class="sxs-lookup"><span data-stu-id="b9e47-340">You should see hello text files that were produced when hello tests ran: sentences.txt, counter.txt, and splitter.txt.</span></span> <span data-ttu-id="b9e47-341">Nyissa meg minden egyes szöveges fájlt, és vizsgálja meg a hello adatok.</span><span class="sxs-lookup"><span data-stu-id="b9e47-341">Open each text file and inspect hello data.</span></span>

   > [!NOTE]
   > <span data-ttu-id="b9e47-342">Karakterlánc típusú adatok továbbra is fennáll, ezeket a fájlokat a decimális értékek ábrázolva.</span><span class="sxs-lookup"><span data-stu-id="b9e47-342">String data persists as an array of decimal values in these files.</span></span> <span data-ttu-id="b9e47-343">Például \[[97,103,111]] hello a **splitter.txt** fájl hello word *és*.</span><span class="sxs-lookup"><span data-stu-id="b9e47-343">For example, \[[97,103,111]] in hello **splitter.txt** file is hello word *and*.</span></span>

> [!NOTE]
> <span data-ttu-id="b9e47-344">Lehet, hogy tooset hello **projekttípus** túl biztonsági**Class Library** tooa Storm on HDInsight-fürt üzembe helyezése előtt.</span><span class="sxs-lookup"><span data-stu-id="b9e47-344">Be sure tooset hello **Project type** back too**Class Library** before deploying tooa Storm on HDInsight cluster.</span></span>

### <a name="log-information"></a><span data-ttu-id="b9e47-345">Naplóadatok</span><span class="sxs-lookup"><span data-stu-id="b9e47-345">Log information</span></span>

<span data-ttu-id="b9e47-346">Könnyen bejelentkezhet információkat a topológia összetevői használatával `Context.Logger`.</span><span class="sxs-lookup"><span data-stu-id="b9e47-346">You can easily log information from your topology components by using `Context.Logger`.</span></span> <span data-ttu-id="b9e47-347">Hello következő például egy tájékoztató naplóbejegyzés hoz létre:</span><span class="sxs-lookup"><span data-stu-id="b9e47-347">For example, hello following creates an informational log entry:</span></span>

```csharp
Context.Logger.Info("Component started");
```

<span data-ttu-id="b9e47-348">A naplózott információk is megtekinthetők hello **Hadoop Szolgáltatásnaplót**, amely megtalálható **Server Explorer**.</span><span class="sxs-lookup"><span data-stu-id="b9e47-348">Logged information can be viewed from hello **Hadoop Service Log**, which is found in **Server Explorer**.</span></span> <span data-ttu-id="b9e47-349">Bontsa ki a hello bejegyzést a Storm on HDInsight-fürt számára, és végül **Hadoop Szolgáltatásnaplót**.</span><span class="sxs-lookup"><span data-stu-id="b9e47-349">Expand hello entry for your Storm on HDInsight cluster, and then expand **Hadoop Service Log**.</span></span> <span data-ttu-id="b9e47-350">Végül válassza ki a hello napló fájl tooview.</span><span class="sxs-lookup"><span data-stu-id="b9e47-350">Finally, select hello log file tooview.</span></span>

> [!NOTE]
> <span data-ttu-id="b9e47-351">hello naplók hello Azure storage-fiók, amely a fürt által használt vannak tárolva.</span><span class="sxs-lookup"><span data-stu-id="b9e47-351">hello logs are stored in hello Azure storage account that is used by your cluster.</span></span> <span data-ttu-id="b9e47-352">tooview hello naplók a Visual Studio alkalmazásban, be kell jelentkeznie toohello Azure-előfizetéssel, amely hello tárfiók tulajdonosa.</span><span class="sxs-lookup"><span data-stu-id="b9e47-352">tooview hello logs in Visual Studio, you must sign in toohello Azure subscription that owns hello storage account.</span></span>

### <a name="view-error-information"></a><span data-ttu-id="b9e47-353">Hiba történt adatainak megtekintése</span><span class="sxs-lookup"><span data-stu-id="b9e47-353">View error information</span></span>

<span data-ttu-id="b9e47-354">a futó topológiát az előfordult tooview hibák lépések hello használata:</span><span class="sxs-lookup"><span data-stu-id="b9e47-354">tooview errors that have occurred in a running topology, use hello following steps:</span></span>

1. <span data-ttu-id="b9e47-355">A **Server Explorer**, kattintson a jobb gombbal a hello Storm on HDInsight-fürt, és válassza ki **nézet Storm-topológiák**.</span><span class="sxs-lookup"><span data-stu-id="b9e47-355">From **Server Explorer**, right-click hello Storm on HDInsight cluster, and select **View Storm topologies**.</span></span>

2. <span data-ttu-id="b9e47-356">A hello **Spout** és **boltok**, hello **utolsó hiba** oszlop hello utolsó hiba információkat tartalmaz.</span><span class="sxs-lookup"><span data-stu-id="b9e47-356">For hello **Spout** and **Bolts**, hello **Last Error** column contains information on hello last error.</span></span>

3. <span data-ttu-id="b9e47-357">Jelölje be hello **Spout azonosító** vagy **Bolt azonosító** hello összetevő, amely rendelkezik a felsorolt hiba.</span><span class="sxs-lookup"><span data-stu-id="b9e47-357">Select hello **Spout Id** or **Bolt Id** for hello component that has an error listed.</span></span> <span data-ttu-id="b9e47-358">Hello részletes lapon, amely hiba jelenik meg, további információt hello szerepel **hibák** alján hello hello szakasz.</span><span class="sxs-lookup"><span data-stu-id="b9e47-358">On hello details page that is displayed, additional error information is listed in hello **Errors** section at hello bottom of hello page.</span></span>

4. <span data-ttu-id="b9e47-359">tooobtain további információt, jelölje be a **Port** a hello **végrehajtója** hello lap részében, toosee hello Storm munkavégző naplóban hello utolsó néhány percig.</span><span class="sxs-lookup"><span data-stu-id="b9e47-359">tooobtain more information, select a **Port** from hello **Executors** section of hello page, toosee hello Storm worker log for hello last few minutes.</span></span>

### <a name="errors-submitting-topologies"></a><span data-ttu-id="b9e47-360">Hibák elküldése a topológiák</span><span class="sxs-lookup"><span data-stu-id="b9e47-360">Errors submitting topologies</span></span>

<span data-ttu-id="b9e47-361">Ha hibákba ütközik egy topológia tooHDInsight elküldése, a naplók hello kiszolgálóoldali összetevők, amelyek kezelik a HDInsight-fürt topológiája küldésének is megtalálhatja.</span><span class="sxs-lookup"><span data-stu-id="b9e47-361">If you encounter errors submitting a topology tooHDInsight, you can find logs for hello server-side components that handle topology submission on your HDInsight cluster.</span></span> <span data-ttu-id="b9e47-362">tooretrieve ezek naplózza, használjon hello következő parancsot a parancssorból:</span><span class="sxs-lookup"><span data-stu-id="b9e47-362">tooretrieve these logs, use hello following command from a command line:</span></span>

    scp sshuser@clustername-ssh.azurehdinsight.net:/var/log/hdinsight-scpwebapi/hdinsight-scpwebapi.out .

<span data-ttu-id="b9e47-363">Cserélje le __sshuser__ a hello hello fürthöz SSH-felhasználói fiókhoz.</span><span class="sxs-lookup"><span data-stu-id="b9e47-363">Replace __sshuser__ with hello SSH user account for hello cluster.</span></span> <span data-ttu-id="b9e47-364">Cserélje le __clustername__ hello nevű hello HDInsight-fürt.</span><span class="sxs-lookup"><span data-stu-id="b9e47-364">Replace __clustername__ with hello name of hello HDInsight cluster.</span></span> <span data-ttu-id="b9e47-365">További információk az `scp` és `ssh` a hdinsight eszközzel, lásd: [az SSH a Hdinsighttal](hdinsight-hadoop-linux-use-ssh-unix.md).</span><span class="sxs-lookup"><span data-stu-id="b9e47-365">For more information on using `scp` and `ssh` with HDInsight, see [Use SSH with HDInsight](hdinsight-hadoop-linux-use-ssh-unix.md).</span></span>

<span data-ttu-id="b9e47-366">Több okból sikertelen lehet a jelentések:</span><span class="sxs-lookup"><span data-stu-id="b9e47-366">Submissions can fail for multiple reasons:</span></span>

* <span data-ttu-id="b9e47-367">JDK nincs telepítve, vagy nincs hello elérési úton.</span><span class="sxs-lookup"><span data-stu-id="b9e47-367">JDK is not installed or is not in hello path.</span></span>
* <span data-ttu-id="b9e47-368">Java szükséges függőség nem szerepelnek a hello küldésének.</span><span class="sxs-lookup"><span data-stu-id="b9e47-368">Required Java dependencies are not included in hello submission.</span></span>
* <span data-ttu-id="b9e47-369">Nem kompatibilis függőségek.</span><span class="sxs-lookup"><span data-stu-id="b9e47-369">Incompatible dependencies.</span></span>
* <span data-ttu-id="b9e47-370">Ismétlődő topológia nevek.</span><span class="sxs-lookup"><span data-stu-id="b9e47-370">Duplicate topology names.</span></span>

<span data-ttu-id="b9e47-371">Ha hello `hdinsight-scpwebapi.out` a napló tartalmaz egy `FileNotFoundException`, ennek oka lehet a következő feltételek hello:</span><span class="sxs-lookup"><span data-stu-id="b9e47-371">If hello `hdinsight-scpwebapi.out` log contains a `FileNotFoundException`, this might be caused by hello following conditions:</span></span>

* <span data-ttu-id="b9e47-372">hello JDK nincs hello fejlesztőkörnyezet hello elérési útját.</span><span class="sxs-lookup"><span data-stu-id="b9e47-372">hello JDK is not in hello path on hello development environment.</span></span> <span data-ttu-id="b9e47-373">Győződjön meg arról, hogy hello fejlesztési környezetet, és hogy telepítve van a JDK hello `%JAVA_HOME%/bin` hello elérési út része.</span><span class="sxs-lookup"><span data-stu-id="b9e47-373">Verify that hello JDK is installed in hello development environment, and that `%JAVA_HOME%/bin` is in hello path.</span></span>
* <span data-ttu-id="b9e47-374">Java függősége hiányzik.</span><span class="sxs-lookup"><span data-stu-id="b9e47-374">You are missing a Java dependency.</span></span> <span data-ttu-id="b9e47-375">Győződjön meg arról, beleértve minden szükséges .jar fájlt hello küldésének részeként is.</span><span class="sxs-lookup"><span data-stu-id="b9e47-375">Make sure you are including any required .jar files as part of hello submission.</span></span>

## <a name="next-steps"></a><span data-ttu-id="b9e47-376">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="b9e47-376">Next steps</span></span>

<span data-ttu-id="b9e47-377">Például az adatok feldolgozása az Event Hubs, [feldolgozni az eseményeket az Azure Event Hubs a HDInsight alatt futó Storm](hdinsight-storm-develop-csharp-event-hub-topology.md).</span><span class="sxs-lookup"><span data-stu-id="b9e47-377">For an example of processing data from Event Hubs, see [Process events from Azure Event Hubs with Storm on HDInsight](hdinsight-storm-develop-csharp-event-hub-topology.md).</span></span>

<span data-ttu-id="b9e47-378">Például egy C#-topológiák, amely felosztja a több adatfolyam adatfolyam-adatokat, lásd: [C# Storm példa](https://github.com/Blackmist/csharp-storm-example).</span><span class="sxs-lookup"><span data-stu-id="b9e47-378">For an example of a C# topology that splits stream data into multiple streams, see [C# Storm example](https://github.com/Blackmist/csharp-storm-example).</span></span>

<span data-ttu-id="b9e47-379">toodiscover C#-topológiák, létrehozásával kapcsolatos további információért lásd: [GitHub](https://github.com/hdinsight/hdinsight-storm-examples/blob/master/SCPNet-GettingStarted.md).</span><span class="sxs-lookup"><span data-stu-id="b9e47-379">toodiscover more information about creating C# topologies, see [GitHub](https://github.com/hdinsight/hdinsight-storm-examples/blob/master/SCPNet-GettingStarted.md).</span></span>

<span data-ttu-id="b9e47-380">További módszereket toowork a hdinsight eszközzel, és minták a HDInsight alatt futó Storm további tekintse meg a következő dokumentumok hello:</span><span class="sxs-lookup"><span data-stu-id="b9e47-380">For more ways toowork with HDInsight and more Storm on HDInsight samples, see hello following documents:</span></span>

<span data-ttu-id="b9e47-381">**Microsoft SCP.NET**</span><span class="sxs-lookup"><span data-stu-id="b9e47-381">**Microsoft SCP.NET**</span></span>

* [<span data-ttu-id="b9e47-382">Szolgáltatáskapcsolódási pont programozási útmutató</span><span class="sxs-lookup"><span data-stu-id="b9e47-382">SCP programming guide</span></span>](hdinsight-storm-scp-programming-guide.md)

<span data-ttu-id="b9e47-383">**A HDInsight alatt futó Apache Storm**</span><span class="sxs-lookup"><span data-stu-id="b9e47-383">**Apache Storm on HDInsight**</span></span>

* [<span data-ttu-id="b9e47-384">Telepítheti és figyelheti a HDInsight alatt futó Apache Storm topológiák</span><span class="sxs-lookup"><span data-stu-id="b9e47-384">Deploy and monitor topologies with Apache Storm on HDInsight</span></span>](hdinsight-storm-deploy-monitor-topology.md)
* [<span data-ttu-id="b9e47-385">HDInsight alatt futó Storm példatopológiái</span><span class="sxs-lookup"><span data-stu-id="b9e47-385">Example topologies for Storm on HDInsight</span></span>](hdinsight-storm-example-topology.md)

<span data-ttu-id="b9e47-386">**Apache Hadoop on HDInsight**</span><span class="sxs-lookup"><span data-stu-id="b9e47-386">**Apache Hadoop on HDInsight**</span></span>

* [<span data-ttu-id="b9e47-387">A Hive használata a hdinsight Hadoop</span><span class="sxs-lookup"><span data-stu-id="b9e47-387">Use Hive with Hadoop on HDInsight</span></span>](hdinsight-use-hive.md)
* [<span data-ttu-id="b9e47-388">A Pig használata a HDInsight Hadoop</span><span class="sxs-lookup"><span data-stu-id="b9e47-388">Use Pig with Hadoop on HDInsight</span></span>](hdinsight-use-pig.md)
* [<span data-ttu-id="b9e47-389">A HDInsight Hadoop MapReduce használata</span><span class="sxs-lookup"><span data-stu-id="b9e47-389">Use MapReduce with Hadoop on HDInsight</span></span>](hdinsight-use-mapreduce.md)

<span data-ttu-id="b9e47-390">**Apache HBase a HDInsight-on**</span><span class="sxs-lookup"><span data-stu-id="b9e47-390">**Apache HBase on HDInsight**</span></span>

* [<span data-ttu-id="b9e47-391">Bevezetés a hbase a HDInsight eszközzel</span><span class="sxs-lookup"><span data-stu-id="b9e47-391">Getting started with HBase on HDInsight</span></span>](hdinsight-hbase-tutorial-get-started.md)
