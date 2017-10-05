---
title: "A Visual Studio és a C# - Azure HDInsight alatt futó Apache Storm-topológiák |} Microsoft Docs"
description: "Ismerje meg a C# Storm-topológiák létrehozása. Hozzon létre egy egyszerű word-count topológiához a Visual Studio által a Hadoop tools for Visual Studio használatával."
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
ms.openlocfilehash: 3ee89b6644ba395e0a6c28ecc2c082c2f7393ac8
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/03/2017
---
# <a name="develop-c-topologies-for-apache-storm-by-using-the-data-lake-tools-for-visual-studio"></a><span data-ttu-id="f40a4-104">C#-topológiák fejlesztése az Apache Storm által a Data Lake tools for Visual Studio használatával</span><span class="sxs-lookup"><span data-stu-id="f40a4-104">Develop C# topologies for Apache Storm by using the Data Lake tools for Visual Studio</span></span>

<span data-ttu-id="f40a4-105">Megtudhatja, hogyan hozhat létre egy C# Storm-topológia az Azure Data Lake (Hadoop) tools for Visual Studio használatával.</span><span class="sxs-lookup"><span data-stu-id="f40a4-105">Learn how to create a C# Storm topology by using the Azure Data Lake (Hadoop) tools for Visual Studio.</span></span> <span data-ttu-id="f40a4-106">Ebből a dokumentumból a folyamatot, amely egy Storm-projekt létrehozása a Visual Studio helyi tesztelés és való az Apache Storm on Azure HDInsight-fürt üzembe helyezése.</span><span class="sxs-lookup"><span data-stu-id="f40a4-106">This document walks through the process of creating a Storm project in Visual Studio, testing it locally, and deploying it to an Apache Storm on Azure HDInsight cluster.</span></span>

<span data-ttu-id="f40a4-107">Azt is megtudhatja, hogyan C# és Java összetevők használó hibrid topológiák létrehozásához.</span><span class="sxs-lookup"><span data-stu-id="f40a4-107">You also learn how to create hybrid topologies that use C# and Java components.</span></span>

> [!NOTE]
> <span data-ttu-id="f40a4-108">A jelen dokumentumban leírt lépések a Visual Studio Windows fejlesztői környezetre támaszkodnak, amíg a lefordított projekt küldheti el a Linux vagy a Windows-alapú HDInsight-fürthöz.</span><span class="sxs-lookup"><span data-stu-id="f40a4-108">While the steps in this document rely on a Windows development environment with Visual Studio, the compiled project can be submitted to either a Linux or Windows-based HDInsight cluster.</span></span> <span data-ttu-id="f40a4-109">Csak a Linux-alapú fürtök létrehozása után 28 2016 októberétől kezdve, SCP.NET topológiákat támogatja.</span><span class="sxs-lookup"><span data-stu-id="f40a4-109">Only Linux-based clusters created after October 28, 2016, support SCP.NET topologies.</span></span>

<span data-ttu-id="f40a4-110">Egy Linux-alapú fürttel C#-topológiák használatához frissítenie kell a Microsoft.SCP.Net.SDK NuGet-csomagot a projekt által használt 0.10.0.6 verzió vagy újabb.</span><span class="sxs-lookup"><span data-stu-id="f40a4-110">To use a C# topology with a Linux-based cluster, you must update the Microsoft.SCP.Net.SDK NuGet package used by your project to version 0.10.0.6 or later.</span></span> <span data-ttu-id="f40a4-111">A csomag verziójának a HDInsightban telepített Storm főverziójával is egyeznie kell.</span><span class="sxs-lookup"><span data-stu-id="f40a4-111">The version of the package must also match the major version of Storm installed on HDInsight.</span></span>

| <span data-ttu-id="f40a4-112">HDInsight-verzió</span><span class="sxs-lookup"><span data-stu-id="f40a4-112">HDInsight version</span></span> | <span data-ttu-id="f40a4-113">A Storm verzióját</span><span class="sxs-lookup"><span data-stu-id="f40a4-113">Storm version</span></span> | <span data-ttu-id="f40a4-114">SCP.NET verzió</span><span class="sxs-lookup"><span data-stu-id="f40a4-114">SCP.NET version</span></span> | <span data-ttu-id="f40a4-115">Alapértelmezett monó verzió</span><span class="sxs-lookup"><span data-stu-id="f40a4-115">Default Mono version</span></span> |
|:-----------------:|:-------------:|:---------------:|:--------------------:|
| <span data-ttu-id="f40a4-116">3.3</span><span class="sxs-lookup"><span data-stu-id="f40a4-116">3.3</span></span> |<span data-ttu-id="f40a4-117">0.10.x</span><span class="sxs-lookup"><span data-stu-id="f40a4-117">0.10.x</span></span> |<span data-ttu-id="f40a4-118">0.10.x.x</span><span class="sxs-lookup"><span data-stu-id="f40a4-118">0.10.x.x</span></span></br><span data-ttu-id="f40a4-119">(csak a Windows-alapú HDInsight)</span><span class="sxs-lookup"><span data-stu-id="f40a4-119">(only on Windows-based HDInsight)</span></span> | <span data-ttu-id="f40a4-120">NA</span><span class="sxs-lookup"><span data-stu-id="f40a4-120">NA</span></span> |
| <span data-ttu-id="f40a4-121">3.4</span><span class="sxs-lookup"><span data-stu-id="f40a4-121">3.4</span></span> | <span data-ttu-id="f40a4-122">0.10.0.x</span><span class="sxs-lookup"><span data-stu-id="f40a4-122">0.10.0.x</span></span> | <span data-ttu-id="f40a4-123">0.10.0.x</span><span class="sxs-lookup"><span data-stu-id="f40a4-123">0.10.0.x</span></span> | <span data-ttu-id="f40a4-124">3.2.8</span><span class="sxs-lookup"><span data-stu-id="f40a4-124">3.2.8</span></span> |
| <span data-ttu-id="f40a4-125">3.5</span><span class="sxs-lookup"><span data-stu-id="f40a4-125">3.5</span></span> | <span data-ttu-id="f40a4-126">1.0.2.x</span><span class="sxs-lookup"><span data-stu-id="f40a4-126">1.0.2.x</span></span> | <span data-ttu-id="f40a4-127">1.0.0.x</span><span class="sxs-lookup"><span data-stu-id="f40a4-127">1.0.0.x</span></span> | <span data-ttu-id="f40a4-128">4.2.1</span><span class="sxs-lookup"><span data-stu-id="f40a4-128">4.2.1</span></span> |
| <span data-ttu-id="f40a4-129">3.6</span><span class="sxs-lookup"><span data-stu-id="f40a4-129">3.6</span></span> | <span data-ttu-id="f40a4-130">1.1.0.x</span><span class="sxs-lookup"><span data-stu-id="f40a4-130">1.1.0.x</span></span> | <span data-ttu-id="f40a4-131">1.0.0.x</span><span class="sxs-lookup"><span data-stu-id="f40a4-131">1.0.0.x</span></span> | <span data-ttu-id="f40a4-132">4.2.8</span><span class="sxs-lookup"><span data-stu-id="f40a4-132">4.2.8</span></span> |

> [!IMPORTANT]
> <span data-ttu-id="f40a4-133">A Linux-alapú fürtök C#-topológiáinak a .NET 4.5-öt kell használnia, és a Mono segítségével futhatnak a HDInsight-fürtön.</span><span class="sxs-lookup"><span data-stu-id="f40a4-133">C# topologies on Linux-based clusters must use .NET 4.5, and use Mono to run on the HDInsight cluster.</span></span> <span data-ttu-id="f40a4-134">Ellenőrizze [monó kompatibilitási](http://www.mono-project.com/docs/about-mono/compatibility/) az esetleges kompatibilitási problémák.</span><span class="sxs-lookup"><span data-stu-id="f40a4-134">Check [Mono compatibility](http://www.mono-project.com/docs/about-mono/compatibility/) for potential incompatibilities.</span></span>

## <a name="install-visual-studio"></a><span data-ttu-id="f40a4-135">A Visual Studio telepítése</span><span class="sxs-lookup"><span data-stu-id="f40a4-135">Install Visual Studio</span></span>

<span data-ttu-id="f40a4-136">C#-topológiák SCP.NET a Visual Studio következő verzióinak egyikét használva fejleszthet:</span><span class="sxs-lookup"><span data-stu-id="f40a4-136">You can develop C# topologies with SCP.NET by using one of the following versions of Visual Studio:</span></span>

* <span data-ttu-id="f40a4-137">A Visual Studio 2012 [4. frissítés](http://www.microsoft.com/download/details.aspx?id=39305)</span><span class="sxs-lookup"><span data-stu-id="f40a4-137">Visual Studio 2012 with [Update 4](http://www.microsoft.com/download/details.aspx?id=39305)</span></span>

* <span data-ttu-id="f40a4-138">A Visual Studio 2013-as verziójának [4. frissítés](http://www.microsoft.com/download/details.aspx?id=44921) vagy [Visual Studio 2013 Community](http://go.microsoft.com/fwlink/?LinkId=517284)</span><span class="sxs-lookup"><span data-stu-id="f40a4-138">Visual Studio 2013 with [Update 4](http://www.microsoft.com/download/details.aspx?id=44921) or [Visual Studio 2013 Community](http://go.microsoft.com/fwlink/?LinkId=517284)</span></span>

* <span data-ttu-id="f40a4-139">Visual Studio 2015-öt vagy [Visual Studio 2015-Közösség](https://go.microsoft.com/fwlink/?LinkId=532606)</span><span class="sxs-lookup"><span data-stu-id="f40a4-139">Visual Studio 2015 or [Visual Studio 2015 Community](https://go.microsoft.com/fwlink/?LinkId=532606)</span></span>

* <span data-ttu-id="f40a4-140">A Visual Studio 2017 (minden kiadás)</span><span class="sxs-lookup"><span data-stu-id="f40a4-140">Visual Studio 2017 (any edition)</span></span>

## <a name="install-data-lake-tools-for-visual-studio"></a><span data-ttu-id="f40a4-141">Telepítse a Data Lake Visual Studio eszközök</span><span class="sxs-lookup"><span data-stu-id="f40a4-141">Install Data Lake tools for Visual Studio</span></span>

<span data-ttu-id="f40a4-142">Telepítheti a Data Lake tools for Visual Studio, kövesse a lépéseket a [Ismerkedés a Data Lake tools for Visual Studio használatával](hdinsight-hadoop-visual-studio-tools-get-started.md).</span><span class="sxs-lookup"><span data-stu-id="f40a4-142">To install Data Lake tools for Visual Studio, follow the steps in [Get started using Data Lake tools for Visual Studio](hdinsight-hadoop-visual-studio-tools-get-started.md).</span></span>

## <a name="install-java"></a><span data-ttu-id="f40a4-143">Java telepítése</span><span class="sxs-lookup"><span data-stu-id="f40a4-143">Install Java</span></span>

<span data-ttu-id="f40a4-144">A Storm-topológia a Visual Studio eszközből elküldésekor SCP.NET állít elő, a topológia és a függőségeit tartalmazó zip-fájl.</span><span class="sxs-lookup"><span data-stu-id="f40a4-144">When you submit a Storm topology from Visual Studio, SCP.NET generates a zip file that contains the topology and dependencies.</span></span> <span data-ttu-id="f40a4-145">Java a zip-fájlok létrehozására szolgál, mert több kompatibilis a Linux-alapú fürtökön formátuma nem használ.</span><span class="sxs-lookup"><span data-stu-id="f40a4-145">Java is used to create these zip files, because it uses a format that is more compatible with Linux-based clusters.</span></span>

1. <span data-ttu-id="f40a4-146">A Java fejlesztői készlet (JDK) 7 vagy újabb a fejlesztési környezet telepítése.</span><span class="sxs-lookup"><span data-stu-id="f40a4-146">Install the Java Developer Kit (JDK) 7 or later on your development environment.</span></span> <span data-ttu-id="f40a4-147">Az Oracle JDK a kaphat [Oracle](http://www.oracle.com/technetwork/java/javase/downloads/index.html).</span><span class="sxs-lookup"><span data-stu-id="f40a4-147">You can get the Oracle JDK from [Oracle](http://www.oracle.com/technetwork/java/javase/downloads/index.html).</span></span> <span data-ttu-id="f40a4-148">Is [más Java terjesztéseket](http://openjdk.java.net/).</span><span class="sxs-lookup"><span data-stu-id="f40a4-148">You can also use [other Java distributions](http://openjdk.java.net/).</span></span>

2. <span data-ttu-id="f40a4-149">A `JAVA_HOME` környezeti változó Java tartalmazó könyvtárat kell mutatnia.</span><span class="sxs-lookup"><span data-stu-id="f40a4-149">The `JAVA_HOME` environment variable must point to the directory that contains Java.</span></span>

3. <span data-ttu-id="f40a4-150">A `PATH` környezeti változó tartalmaznia kell a `%JAVA_HOME%\bin` könyvtár.</span><span class="sxs-lookup"><span data-stu-id="f40a4-150">The `PATH` environment variable must include the `%JAVA_HOME%\bin` directory.</span></span>

<span data-ttu-id="f40a4-151">A következő C# Konzolalkalmazás használatával ellenőrizze, hogy Java és a JDK megfelelően vannak-e telepítve:</span><span class="sxs-lookup"><span data-stu-id="f40a4-151">You can use the following C# console application to verify that Java and the JDK are correctly installed:</span></span>

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

## <a name="storm-templates"></a><span data-ttu-id="f40a4-152">A Storm-sablonok</span><span class="sxs-lookup"><span data-stu-id="f40a4-152">Storm templates</span></span>

<span data-ttu-id="f40a4-153">A Data Lake tools for Visual Studio adja meg a következő sablonokat:</span><span class="sxs-lookup"><span data-stu-id="f40a4-153">The Data Lake tools for Visual Studio provide the following templates:</span></span>

| <span data-ttu-id="f40a4-154">Projekt típusa</span><span class="sxs-lookup"><span data-stu-id="f40a4-154">Project type</span></span> | <span data-ttu-id="f40a4-155">Azt mutatja be</span><span class="sxs-lookup"><span data-stu-id="f40a4-155">Demonstrates</span></span> |
| --- | --- |
| <span data-ttu-id="f40a4-156">Storm-alkalmazás</span><span class="sxs-lookup"><span data-stu-id="f40a4-156">Storm Application</span></span> |<span data-ttu-id="f40a4-157">Egy üres Storm-topológia projektet.</span><span class="sxs-lookup"><span data-stu-id="f40a4-157">An empty Storm topology project.</span></span> |
| <span data-ttu-id="f40a4-158">A Storm Azure SQL Writer minta</span><span class="sxs-lookup"><span data-stu-id="f40a4-158">Storm Azure SQL Writer Sample</span></span> |<span data-ttu-id="f40a4-159">Megtudhatja, hogyan lehet írni az Azure SQL Database.</span><span class="sxs-lookup"><span data-stu-id="f40a4-159">How to write to Azure SQL Database.</span></span> |
| <span data-ttu-id="f40a4-160">A Storm Azure Cosmos DB olvasó minta</span><span class="sxs-lookup"><span data-stu-id="f40a4-160">Storm Azure Cosmos DB Reader Sample</span></span> |<span data-ttu-id="f40a4-161">How Azure Cosmos DB olvasni.</span><span class="sxs-lookup"><span data-stu-id="f40a4-161">How to read from Azure Cosmos DB.</span></span> |
| <span data-ttu-id="f40a4-162">A Storm Azure Cosmos DB író minta</span><span class="sxs-lookup"><span data-stu-id="f40a4-162">Storm Azure Cosmos DB Writer Sample</span></span> |<span data-ttu-id="f40a4-163">How Azure Cosmos DB írni.</span><span class="sxs-lookup"><span data-stu-id="f40a4-163">How to write to Azure Cosmos DB.</span></span> |
| <span data-ttu-id="f40a4-164">A Storm EventHub olvasó minta</span><span class="sxs-lookup"><span data-stu-id="f40a4-164">Storm EventHub Reader Sample</span></span> |<span data-ttu-id="f40a4-165">How Azure Event Hubs olvasni.</span><span class="sxs-lookup"><span data-stu-id="f40a4-165">How to read from Azure Event Hubs.</span></span> |
| <span data-ttu-id="f40a4-166">A Storm EventHub író minta</span><span class="sxs-lookup"><span data-stu-id="f40a4-166">Storm EventHub Writer Sample</span></span> |<span data-ttu-id="f40a4-167">How Azure Event Hubs írni.</span><span class="sxs-lookup"><span data-stu-id="f40a4-167">How to write to Azure Event Hubs.</span></span> |
| <span data-ttu-id="f40a4-168">A Storm HBase olvasó minta</span><span class="sxs-lookup"><span data-stu-id="f40a4-168">Storm HBase Reader Sample</span></span> |<span data-ttu-id="f40a4-169">Hogyan lehet olvasni a HBase a HDInsight-fürtökön.</span><span class="sxs-lookup"><span data-stu-id="f40a4-169">How to read from HBase on HDInsight clusters.</span></span> |
| <span data-ttu-id="f40a4-170">A Storm HBase író minta</span><span class="sxs-lookup"><span data-stu-id="f40a4-170">Storm HBase Writer Sample</span></span> |<span data-ttu-id="f40a4-171">Hogyan lehet írni a HBase a HDInsight-fürtökön.</span><span class="sxs-lookup"><span data-stu-id="f40a4-171">How to write to HBase on HDInsight clusters.</span></span> |
| <span data-ttu-id="f40a4-172">A Storm hibrid minta</span><span class="sxs-lookup"><span data-stu-id="f40a4-172">Storm Hybrid Sample</span></span> |<span data-ttu-id="f40a4-173">Hogyan használható egy Java-összetevő.</span><span class="sxs-lookup"><span data-stu-id="f40a4-173">How to use a Java component.</span></span> |
| <span data-ttu-id="f40a4-174">A Storm-minta</span><span class="sxs-lookup"><span data-stu-id="f40a4-174">Storm Sample</span></span> |<span data-ttu-id="f40a4-175">Alapszintű word-count topológiához.</span><span class="sxs-lookup"><span data-stu-id="f40a4-175">A basic word count topology.</span></span> |

> [!WARNING]
> <span data-ttu-id="f40a4-176">Nem minden sablonok a Linux-alapú hdinsight eszközzel fog működni.</span><span class="sxs-lookup"><span data-stu-id="f40a4-176">Not all templates will work with Linux-based HDInsight.</span></span> <span data-ttu-id="f40a4-177">A sablonok által használt Nuget-csomagok nem lehet monó kompatibilis.</span><span class="sxs-lookup"><span data-stu-id="f40a4-177">Nuget packages used by the templates may not be compatible with Mono.</span></span> <span data-ttu-id="f40a4-178">Ellenőrizze a [monó kompatibilitási](http://www.mono-project.com/docs/about-mono/compatibility/) dokumentum, és használja a [.NET hordozhatóság Analyzer](hdinsight-hadoop-migrate-dotnet-to-linux.md#automated-portability-analysis) potenciális problémák azonosításához.</span><span class="sxs-lookup"><span data-stu-id="f40a4-178">Check the [Mono compatibility](http://www.mono-project.com/docs/about-mono/compatibility/) document and use the [.NET Portability Analyzer](hdinsight-hadoop-migrate-dotnet-to-linux.md#automated-portability-analysis) to identify potential problems.</span></span>

<span data-ttu-id="f40a4-179">A jelen dokumentumban leírt lépések segítségével a alapszintű Storm-alkalmazás projekt típus olyan topológiák létrehozását is.</span><span class="sxs-lookup"><span data-stu-id="f40a4-179">In the steps in this document, you use the basic Storm Application project type to create a topology.</span></span>

### <a name="hbase-templates-notes"></a><span data-ttu-id="f40a4-180">A HBase sablonok megjegyzések</span><span class="sxs-lookup"><span data-stu-id="f40a4-180">HBase templates notes</span></span>

<span data-ttu-id="f40a4-181">A HBase írási és olvasási szerepkörökhöz sablonok a HBase REST API-t nem a HBase Java API használatával kommunikálnak a HBase a HDInsight-fürthöz.</span><span class="sxs-lookup"><span data-stu-id="f40a4-181">The HBase reader and writer templates use the HBase REST API, not the HBase Java API, to communicate with an HBase on HDInsight cluster.</span></span>

### <a name="eventhub-templates-notes"></a><span data-ttu-id="f40a4-182">Az EventHub sablonok megjegyzések</span><span class="sxs-lookup"><span data-stu-id="f40a4-182">EventHub templates notes</span></span>

> [!IMPORTANT]
> <span data-ttu-id="f40a4-183">A Java-alapú EventHub spout-összetevő az EventHub-olvasó sablon nem feltétlenül Storm on HDInsight 3.5-ös vagy újabb verziója.</span><span class="sxs-lookup"><span data-stu-id="f40a4-183">The Java-based EventHub spout component included with the EventHub Reader template may not work with Storm on HDInsight version 3.5 or later.</span></span> <span data-ttu-id="f40a4-184">Ez az összetevő frissített verziója érhető el, [GitHub](https://github.com/hdinsight/hdinsight-storm-examples/tree/master/HDI3.5/lib).</span><span class="sxs-lookup"><span data-stu-id="f40a4-184">An updated version of this component is available at [GitHub](https://github.com/hdinsight/hdinsight-storm-examples/tree/master/HDI3.5/lib).</span></span>

<span data-ttu-id="f40a4-185">Ezt példatopológia összetevő, és együttműködik Storm on HDInsight 3.5, lásd: [GitHub](https://github.com/Azure-Samples/hdinsight-dotnet-java-storm-eventhub).</span><span class="sxs-lookup"><span data-stu-id="f40a4-185">For an example topology that uses this component and works with Storm on HDInsight 3.5, see [GitHub](https://github.com/Azure-Samples/hdinsight-dotnet-java-storm-eventhub).</span></span>

## <a name="create-a-c-topology"></a><span data-ttu-id="f40a4-186">Hozzon létre egy C#-topológiák</span><span class="sxs-lookup"><span data-stu-id="f40a4-186">Create a C# topology</span></span>

1. <span data-ttu-id="f40a4-187">Nyissa meg a Visual Studio, válassza ki **fájl** > **új**, majd válassza ki **projekt**.</span><span class="sxs-lookup"><span data-stu-id="f40a4-187">Open Visual Studio, select **File** > **New**, and then select **Project**.</span></span>

2. <span data-ttu-id="f40a4-188">Az a **új projekt** ablakában bontsa ki a **telepített** > **sablonok**, és válassza ki **Azure Data Lake**.</span><span class="sxs-lookup"><span data-stu-id="f40a4-188">From the **New Project** window, expand **Installed** > **Templates**, and select **Azure Data Lake**.</span></span> <span data-ttu-id="f40a4-189">Válassza ki a listáról a sablonok **Storm-alkalmazás**.</span><span class="sxs-lookup"><span data-stu-id="f40a4-189">From the list of templates, select **Storm Application**.</span></span> <span data-ttu-id="f40a4-190">Adja meg a képernyő alján **WordCount** az alkalmazás nevével.</span><span class="sxs-lookup"><span data-stu-id="f40a4-190">At the bottom of the screen, enter **WordCount** as the name of the application.</span></span>

    ![Képernyőfelvétel az új projekt ablakról](./media/hdinsight-storm-develop-csharp-visual-studio-topology/new-project.png)

3. <span data-ttu-id="f40a4-192">Miután létrehozta a projekt, rendelkeznie kell a következő fájlokat:</span><span class="sxs-lookup"><span data-stu-id="f40a4-192">After you have created the project, you should have the following files:</span></span>

   * <span data-ttu-id="f40a4-193">**Program.cs**: A fájl határozza meg a topológia a projekthez.</span><span class="sxs-lookup"><span data-stu-id="f40a4-193">**Program.cs**: This file defines the topology for your project.</span></span> <span data-ttu-id="f40a4-194">Alapértelmezés szerint létrejön egy alapértelmezett topológia, amely egy spout és egy bolt áll.</span><span class="sxs-lookup"><span data-stu-id="f40a4-194">A default topology that consists of one spout and one bolt is created by default.</span></span>

   * <span data-ttu-id="f40a4-195">**Spout.cs**: egy példa spout, amely véletlenszerű számok bocsát ki.</span><span class="sxs-lookup"><span data-stu-id="f40a4-195">**Spout.cs**: An example spout that emits random numbers.</span></span>

   * <span data-ttu-id="f40a4-196">**Bolt.cs**: egy példa bolthoz, amely a spout által kibocsátott számok számát követi.</span><span class="sxs-lookup"><span data-stu-id="f40a4-196">**Bolt.cs**: An example bolt that keeps a count of numbers emitted by the spout.</span></span>

     <span data-ttu-id="f40a4-197">A projekt létrehozásához NuGet letölti a legújabb [SCP.NET csomag](https://www.nuget.org/packages/Microsoft.SCP.Net.SDK/).</span><span class="sxs-lookup"><span data-stu-id="f40a4-197">When you create the project, NuGet downloads the latest [SCP.NET package](https://www.nuget.org/packages/Microsoft.SCP.Net.SDK/).</span></span>

     [!INCLUDE [scp.net version important](../../includes/hdinsight-storm-scpdotnet-version.md)]

### <a name="implement-the-spout"></a><span data-ttu-id="f40a4-198">A spout megvalósítása</span><span class="sxs-lookup"><span data-stu-id="f40a4-198">Implement the spout</span></span>

1. <span data-ttu-id="f40a4-199">Nyissa meg **Spout.cs**.</span><span class="sxs-lookup"><span data-stu-id="f40a4-199">Open **Spout.cs**.</span></span> <span data-ttu-id="f40a4-200">Spoutokkal kapcsolatban használt külső forrásból topológiában adatokat olvasni.</span><span class="sxs-lookup"><span data-stu-id="f40a4-200">Spouts are used to read data in a topology from an external source.</span></span> <span data-ttu-id="f40a4-201">Egy spout a fő elemei a következők:</span><span class="sxs-lookup"><span data-stu-id="f40a4-201">The main components for a spout are:</span></span>

   * <span data-ttu-id="f40a4-202">**NextTuple**: Storm által meghívva, amikor a spout engedélyezett hozható létre új rekordokat.</span><span class="sxs-lookup"><span data-stu-id="f40a4-202">**NextTuple**: Called by Storm when the spout is allowed to emit new tuples.</span></span>

   * <span data-ttu-id="f40a4-203">**Nyugtázási** (csak tranzakciós topológia): a nyugtázás a rekordokat a spout küldött topológiájának más összetevői által kezdeményezett kezeli.</span><span class="sxs-lookup"><span data-stu-id="f40a4-203">**Ack** (transactional topology only): Handles acknowledgements initiated by other components in the topology for tuples sent from the spout.</span></span> <span data-ttu-id="f40a4-204">Egy rekord igazolása lehetővé teszi, hogy a spout tudja, hogy megtörtént a feldolgozása sikeresen alsóbb rétegbeli összetevők.</span><span class="sxs-lookup"><span data-stu-id="f40a4-204">Acknowledging a tuple lets the spout know that it was processed successfully by downstream components.</span></span>

   * <span data-ttu-id="f40a4-205">**Sikertelen** (csak tranzakciós topológia): rekordokat, amelyek vannak sikertelen – a feldolgozás a topológia más összetevők kezeli.</span><span class="sxs-lookup"><span data-stu-id="f40a4-205">**Fail** (transactional topology only): Handles tuples that are fail-processing other components in the topology.</span></span> <span data-ttu-id="f40a4-206">A sikertelen metódusának lehetővé teszi, hogy újra dolgozhatók újra létrehozza a rekordban.</span><span class="sxs-lookup"><span data-stu-id="f40a4-206">Implementing a Fail method allows you to re-emit the tuple so that it can be processed again.</span></span>

2. <span data-ttu-id="f40a4-207">Cserélje le a tartalmát a **Spout** osztály a következő szöveggel.</span><span class="sxs-lookup"><span data-stu-id="f40a4-207">Replace the contents of the **Spout** class with the following text.</span></span> <span data-ttu-id="f40a4-208">A spout véletlenszerűen azokat a topológia bocsát ki egy mondat helyett szerepel.</span><span class="sxs-lookup"><span data-stu-id="f40a4-208">This spout randomly emits a sentence into the topology.</span></span>

    ```csharp
    private Context ctx;
    private Random r = new Random();
    string[] sentences = new string[] {
        "the cow jumped over the moon",
        "an apple a day keeps the doctor away",
        "four score and seven years ago",
        "snow white and the seven dwarfs",
        "i am at two with nature"
    };

    public Spout(Context ctx)
    {
        // Set the instance context
        this.ctx = ctx;

        Context.Logger.Info("Generator constructor called");

        // Declare Output schema
        Dictionary<string, List<Type>> outputSchema = new Dictionary<string, List<Type>>();
        // The schema for the default output stream is
        // a tuple that contains a string field
        outputSchema.Add("default", new List<Type>() { typeof(string) });
        this.ctx.DeclareComponentSchema(new ComponentStreamSchema(null, outputSchema));
    }

    // Get an instance of the spout
    public static Spout Get(Context ctx, Dictionary<string, Object> parms)
    {
        return new Spout(ctx);
    }

    public void NextTuple(Dictionary<string, Object> parms)
    {
        Context.Logger.Info("NextTuple enter");
        // The sentence to be emitted
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

### <a name="implement-the-bolts"></a><span data-ttu-id="f40a4-209">A boltokhoz megvalósítása</span><span class="sxs-lookup"><span data-stu-id="f40a4-209">Implement the bolts</span></span>

1. <span data-ttu-id="f40a4-210">Törölje a meglévő **Bolt.cs** fájlt a projektben.</span><span class="sxs-lookup"><span data-stu-id="f40a4-210">Delete the existing **Bolt.cs** file from the project.</span></span>

2. <span data-ttu-id="f40a4-211">A **Megoldáskezelőben**, kattintson jobb gombbal a projektre, és válassza ki **Hozzáadás** > **új elem**.</span><span class="sxs-lookup"><span data-stu-id="f40a4-211">In **Solution Explorer**, right-click the project, and select **Add** > **New item**.</span></span> <span data-ttu-id="f40a4-212">Válassza ki a listáról, **Storm Bolt**, és írja be **Splitter.cs** neveként.</span><span class="sxs-lookup"><span data-stu-id="f40a4-212">From the list, select **Storm Bolt**, and enter **Splitter.cs** as the name.</span></span> <span data-ttu-id="f40a4-213">Ismételje meg a folyamat hozzon létre egy második bolt nevű **Counter.cs**.</span><span class="sxs-lookup"><span data-stu-id="f40a4-213">Repeat this process to create a second bolt named **Counter.cs**.</span></span>

   * <span data-ttu-id="f40a4-214">**Splitter.cs**: egy olyan bolthoz, amely felosztja a mondatok egyedi szót, és megfelelően kibocsát egy új adatfolyam szavak valósítja meg.</span><span class="sxs-lookup"><span data-stu-id="f40a4-214">**Splitter.cs**: Implements a bolt that splits sentences into individual words, and emits a new stream of words.</span></span>

   * <span data-ttu-id="f40a4-215">**Counter.cs**: valósít meg olyan bolthoz, amely megjeleníti minden szó, és megfelelően kibocsát egy új adatfolyam szavak és minden szó számát.</span><span class="sxs-lookup"><span data-stu-id="f40a4-215">**Counter.cs**: Implements a bolt that counts each word, and emits a new stream of words and the count for each word.</span></span>

     > [!NOTE]
     > <span data-ttu-id="f40a4-216">Ezek boltokhoz írási és olvasási adatfolyamokat, de kommunikálni az adatforrások, például egy adatbázis vagy a szolgáltatás egy olyan bolthoz is használhatja.</span><span class="sxs-lookup"><span data-stu-id="f40a4-216">These bolts read and write to streams, but you can also use a bolt to communicate with sources such as a database or service.</span></span>

3. <span data-ttu-id="f40a4-217">Nyissa meg **Splitter.cs**.</span><span class="sxs-lookup"><span data-stu-id="f40a4-217">Open **Splitter.cs**.</span></span> <span data-ttu-id="f40a4-218">Alapértelmezés szerint csak egyetlen módszer van: **Execute**.</span><span class="sxs-lookup"><span data-stu-id="f40a4-218">It has only one method by default: **Execute**.</span></span> <span data-ttu-id="f40a4-219">Az Execute metódus neve feldolgozásra rekordot tartalmaz a bolt fogadásakor.</span><span class="sxs-lookup"><span data-stu-id="f40a4-219">The Execute method is called when the bolt receives a tuple for processing.</span></span> <span data-ttu-id="f40a4-220">Itt el tudja olvasni és feldolgozni a bejövő rekordokat, és a kibocsátás kimenő rekordokat.</span><span class="sxs-lookup"><span data-stu-id="f40a4-220">Here, you can read and process incoming tuples, and emit outbound tuples.</span></span>

4. <span data-ttu-id="f40a4-221">Cserélje le a tartalmát a **elválasztó** osztály az alábbi kódra:</span><span class="sxs-lookup"><span data-stu-id="f40a4-221">Replace the contents of the **Splitter** class with the following code:</span></span>

    ```csharp
    private Context ctx;

    // Constructor
    public Splitter(Context ctx)
    {
        Context.Logger.Info("Splitter constructor called");
        this.ctx = ctx;

        // Declare Input and Output schemas
        Dictionary<string, List<Type>> inputSchema = new Dictionary<string, List<Type>>();
        // Input contains a tuple with a string field (the sentence)
        inputSchema.Add("default", new List<Type>() { typeof(string) });
        Dictionary<string, List<Type>> outputSchema = new Dictionary<string, List<Type>>();
        // Outbound contains a tuple with a string field (the word)
        outputSchema.Add("default", new List<Type>() { typeof(string) });
        this.ctx.DeclareComponentSchema(new ComponentStreamSchema(inputSchema, outputSchema));
    }

    // Get a new instance of the bolt
    public static Splitter Get(Context ctx, Dictionary<string, Object> parms)
    {
        return new Splitter(ctx);
    }

    // Called when a new tuple is available
    public void Execute(SCPTuple tuple)
    {
        Context.Logger.Info("Execute enter");

        // Get the sentence from the tuple
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

5. <span data-ttu-id="f40a4-222">Nyissa meg **Counter.cs**, és cserélje ki a osztály tartalmát a következőre:</span><span class="sxs-lookup"><span data-stu-id="f40a4-222">Open **Counter.cs**, and replace the class contents with the following:</span></span>

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
        // A tuple containing a string field - the word
        inputSchema.Add("default", new List<Type>() { typeof(string) });

        Dictionary<string, List<Type>> outputSchema = new Dictionary<string, List<Type>>();
        // A tuple containing a string and integer field - the word and the word count
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

        // Get the word from the tuple
        string word = tuple.GetString(0);
        // Do we already have an entry for the word in the dictionary?
        // If no, create one with a count of 0
        int count = counts.ContainsKey(word) ? counts[word] : 0;
        // Increment the count
        count++;
        // Update the count in the dictionary
        counts[word] = count;

        Context.Logger.Info("Emit: {0}, count: {1}", word, count);
        // Emit the word and count information
        this.ctx.Emit(Constants.DEFAULT_STREAM_ID, new List<SCPTuple> { tuple }, new Values(word, count));
        Context.Logger.Info("Execute exit");
    }
    ```

### <a name="define-the-topology"></a><span data-ttu-id="f40a4-223">A topológia meghatározása</span><span class="sxs-lookup"><span data-stu-id="f40a4-223">Define the topology</span></span>

<span data-ttu-id="f40a4-224">A grafikus, amely meghatározza, hogyan a közötti adatáramlás összetevők spoutokkal kapcsolatban és boltokhoz vannak rendezve.</span><span class="sxs-lookup"><span data-stu-id="f40a4-224">Spouts and bolts are arranged in a graph, which defines how the data flows between components.</span></span> <span data-ttu-id="f40a4-225">Ez a topológia a diagram a következőképpen történik:</span><span class="sxs-lookup"><span data-stu-id="f40a4-225">For this topology, the graph is as follows:</span></span>

![Hogyan vannak elrendezve összetevők ábrája](./media/hdinsight-storm-develop-csharp-visual-studio-topology/wordcount-topology.png)

<span data-ttu-id="f40a4-227">A mondatok a spout a kibocsátott, és továbbítja őket a elválasztó bolt példányai.</span><span class="sxs-lookup"><span data-stu-id="f40a4-227">Sentences are emitted from the spout, and are distributed to instances of the Splitter bolt.</span></span> <span data-ttu-id="f40a4-228">Az elválasztó bolt bontja szavakat, amely továbbítja őket a számláló bolt a mondatok.</span><span class="sxs-lookup"><span data-stu-id="f40a4-228">The Splitter bolt breaks the sentences into words, which are distributed to the Counter bolt.</span></span>

<span data-ttu-id="f40a4-229">Szószámot helyileg használatban van a teljesítményszámláló-példány, mert azt szeretnénk, győződjön meg arról, hogy szavak flow ugyanazon teljesítményszámláló bolt-példányra végzi el.</span><span class="sxs-lookup"><span data-stu-id="f40a4-229">Because word count is held locally in the Counter instance, we want to make sure that specific words flow to the same Counter bolt instance.</span></span> <span data-ttu-id="f40a4-230">Minden példány nyomon követi a szavakat.</span><span class="sxs-lookup"><span data-stu-id="f40a4-230">Each instance keeps track of specific words.</span></span> <span data-ttu-id="f40a4-231">Az elválasztó bolt nem tart fenn, mert tényleg nem számít, az elválasztó példányok kap mely mondat helyett szerepel.</span><span class="sxs-lookup"><span data-stu-id="f40a4-231">Since the Splitter bolt maintains no state, it really doesn't matter which instance of the splitter receives which sentence.</span></span>

<span data-ttu-id="f40a4-232">Nyissa meg **Program.cs**.</span><span class="sxs-lookup"><span data-stu-id="f40a4-232">Open **Program.cs**.</span></span> <span data-ttu-id="f40a4-233">A fontos módszer **GetTopologyBuilder**, amely segítségével meghatározhatja, hogy a topológia elküldött Storm legyen.</span><span class="sxs-lookup"><span data-stu-id="f40a4-233">The important method is **GetTopologyBuilder**, which is used to define the topology that is submitted to Storm.</span></span> <span data-ttu-id="f40a4-234">Cserélje le a tartalmát **GetTopologyBuilder** a fentiekben említett topológia végrehajtásához a következő kóddal:</span><span class="sxs-lookup"><span data-stu-id="f40a4-234">Replace the contents of **GetTopologyBuilder** with the following code to implement the topology described previously:</span></span>

```csharp
// Create a new topology named 'WordCount'
TopologyBuilder topologyBuilder = new TopologyBuilder("WordCount" + DateTime.Now.ToString("yyyyMMddHHmmss"));

// Add the spout to the topology.
// Name the component 'sentences'
// Name the field that is emitted as 'sentence'
topologyBuilder.SetSpout(
    "sentences",
    Spout.Get,
    new Dictionary<string, List<string>>()
    {
        {Constants.DEFAULT_STREAM_ID, new List<string>(){"sentence"}}
    },
    1);
// Add the splitter bolt to the topology.
// Name the component 'splitter'
// Name the field that is emitted 'word'
// Use suffleGrouping to distribute incoming tuples
//   from the 'sentences' spout across instances
//   of the splitter
topologyBuilder.SetBolt(
    "splitter",
    Splitter.Get,
    new Dictionary<string, List<string>>()
    {
        {Constants.DEFAULT_STREAM_ID, new List<string>(){"word"}}
    },
    1).shuffleGrouping("sentences");

// Add the counter bolt to the topology.
// Name the component 'counter'
// Name the fields that are emitted 'word' and 'count'
// Use fieldsGrouping to ensure that tuples are routed
//   to counter instances based on the contents of field
//   position 0 (the word). This could also have been
//   List<string>(){"word"}.
//   This ensures that the word 'jumped', for example, will always
//   go to the same instance
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

## <a name="submit-the-topology"></a><span data-ttu-id="f40a4-235">Küldje el a topológia</span><span class="sxs-lookup"><span data-stu-id="f40a4-235">Submit the topology</span></span>

1. <span data-ttu-id="f40a4-236">A **Megoldáskezelőben**, kattintson jobb gombbal a projektre, és válassza ki **Submit a HDInsight alatt futó Storm**.</span><span class="sxs-lookup"><span data-stu-id="f40a4-236">In **Solution Explorer**, right-click the project, and select **Submit to Storm on HDInsight**.</span></span>

   > [!NOTE]
   > <span data-ttu-id="f40a4-237">Ha a rendszer kéri, adja meg a hitelesítő adatait az Azure-előfizetéshez.</span><span class="sxs-lookup"><span data-stu-id="f40a4-237">If prompted, enter the credentials for your Azure subscription.</span></span> <span data-ttu-id="f40a4-238">Ha egynél több előfizetéssel rendelkezik, jelentkezzen be, amely tartalmazza a Storm on HDInsight-fürt.</span><span class="sxs-lookup"><span data-stu-id="f40a4-238">If you have more than one subscription, sign in to the one that contains your Storm on HDInsight cluster.</span></span>

2. <span data-ttu-id="f40a4-239">Válassza ki a Storm on HDInsight-fürt a a **Storm-fürt** legördülő listából válassza ki, és válassza **Submit**.</span><span class="sxs-lookup"><span data-stu-id="f40a4-239">Select your Storm on HDInsight cluster from the **Storm Cluster** drop-down list, and then select **Submit**.</span></span> <span data-ttu-id="f40a4-240">Ha a küldése sikeres használatával figyelheti a **kimeneti** ablak.</span><span class="sxs-lookup"><span data-stu-id="f40a4-240">You can monitor if the submission is successful by using the **Output** window.</span></span>

3. <span data-ttu-id="f40a4-241">Ha a topológia küldése sikeres volt, a **Storm-topológiák** megjelenjen-e a fürt számára.</span><span class="sxs-lookup"><span data-stu-id="f40a4-241">When the topology has been successfully submitted, the **Storm Topologies** for the cluster should appear.</span></span> <span data-ttu-id="f40a4-242">Válassza ki a **WordCount** topológia a futó topológiát vonatkozó információk megtekintése a listából.</span><span class="sxs-lookup"><span data-stu-id="f40a4-242">Select the **WordCount** topology from the list to view information about the running topology.</span></span>

   > [!NOTE]
   > <span data-ttu-id="f40a4-243">Megtekintheti továbbá **Storm-topológiák** a **Server Explorer**.</span><span class="sxs-lookup"><span data-stu-id="f40a4-243">You can also view **Storm Topologies** from **Server Explorer**.</span></span> <span data-ttu-id="f40a4-244">Bontsa ki a **Azure** > **HDInsight**, kattintson a jobb gombbal a Storm on HDInsight-fürt, majd válassza ki **nézet Storm-topológiák**.</span><span class="sxs-lookup"><span data-stu-id="f40a4-244">Expand **Azure** > **HDInsight**, right-click a Storm on HDInsight cluster, and then select **View Storm Topologies**.</span></span>

    <span data-ttu-id="f40a4-245">A topológia a összetevőivel kapcsolatos információk megtekintéséhez kattintson duplán a diagramban.</span><span class="sxs-lookup"><span data-stu-id="f40a4-245">To view information about the components in the topology, double-click the component in the diagram.</span></span>

4. <span data-ttu-id="f40a4-246">Az a **topológia összegzése** megtekintéséhez kattintson az **Kill** a topológia leállítása.</span><span class="sxs-lookup"><span data-stu-id="f40a4-246">From the **Topology Summary** view, click **Kill** to stop the topology.</span></span>

   > [!NOTE]
   > <span data-ttu-id="f40a4-247">Storm-topológiák továbbra is fut, amíg azok inaktiválása, vagy a fürt törlésekor.</span><span class="sxs-lookup"><span data-stu-id="f40a4-247">Storm topologies continue to run until they are deactivated, or the cluster is deleted.</span></span>

## <a name="transactional-topology"></a><span data-ttu-id="f40a4-248">Tranzakciós topológia</span><span class="sxs-lookup"><span data-stu-id="f40a4-248">Transactional topology</span></span>

<span data-ttu-id="f40a4-249">Az előző topológia nem tranzakciós.</span><span class="sxs-lookup"><span data-stu-id="f40a4-249">The previous topology is non-transactional.</span></span> <span data-ttu-id="f40a4-250">Az összetevők a topológia nem valósítja meg a funkciót a üzenetek visszajátszását.</span><span class="sxs-lookup"><span data-stu-id="f40a4-250">The components in the topology do not implement functionality to replaying messages.</span></span> <span data-ttu-id="f40a4-251">Például egy tranzakciós topológia, hozzon létre egy projektet, és válassza **Storm minta** a projekt típusként.</span><span class="sxs-lookup"><span data-stu-id="f40a4-251">For an example of a transactional topology, create a project and select **Storm Sample** as the project type.</span></span>

<span data-ttu-id="f40a4-252">Tranzakciós topológiák valósítja meg a következő adatok ismétlés támogatásához:</span><span class="sxs-lookup"><span data-stu-id="f40a4-252">Transactional topologies implement the following to support replay of data:</span></span>

* <span data-ttu-id="f40a4-253">**Metaadatok gyorsítótárazása**: A spout kell tárolni a metaadatokat, így az adatok lekérése, és újra kibocsátott, ha hiba történik a kibocsátott adatokról.</span><span class="sxs-lookup"><span data-stu-id="f40a4-253">**Metadata caching**: The spout must store metadata about the data emitted, so that the data can be retrieved and emitted again if a failure occurs.</span></span> <span data-ttu-id="f40a4-254">Mivel a minta által kibocsátott adatokról kisméretű, a nyers adatok a táblakonstruktor minden rekordjának a visszajátszás dictionary tárolja.</span><span class="sxs-lookup"><span data-stu-id="f40a4-254">Because the data emitted by the sample is small, the raw data for each tuple is stored in a dictionary for replay.</span></span>

* <span data-ttu-id="f40a4-255">**Nyugtázási**: minden bolt a topológia meghívhatja `this.ctx.Ack(tuple)` való megerősíti, hogy sikeresen feldolgozta-rekordot.</span><span class="sxs-lookup"><span data-stu-id="f40a4-255">**Ack**: Each bolt in the topology can call `this.ctx.Ack(tuple)` to acknowledge that it has successfully processed a tuple.</span></span> <span data-ttu-id="f40a4-256">Ha az összes boltokhoz korrektúrák a rekord a `Ack` a spout metódusában meghívták.</span><span class="sxs-lookup"><span data-stu-id="f40a4-256">When all bolts have acked the tuple, the `Ack` method of the spout is invoked.</span></span> <span data-ttu-id="f40a4-257">A `Ack` módszer lehetővé teszi, hogy eltávolítja a visszajátszás gyorsítótárazott adatokat spout.</span><span class="sxs-lookup"><span data-stu-id="f40a4-257">The `Ack` method allows the spout to remove data that was cached for replay.</span></span>

* <span data-ttu-id="f40a4-258">**Sikertelen**: minden bolt meghívhatja `this.ctx.Fail(tuple)` annak jelzésére, hogy a feldolgozás sikertelen volt a rekordot.</span><span class="sxs-lookup"><span data-stu-id="f40a4-258">**Fail**: Each bolt can call `this.ctx.Fail(tuple)` to indicate that processing has failed for a tuple.</span></span> <span data-ttu-id="f40a4-259">A hiba propagálás a a `Fail` módja a spout, ahol a rekordot is bejegyzéseit meg kell ismételni a gyorsítótárazott metaadatok.</span><span class="sxs-lookup"><span data-stu-id="f40a4-259">The failure propagates to the `Fail` method of the spout, where the tuple can be replayed by using cached metadata.</span></span>

* <span data-ttu-id="f40a4-260">**A feladatütemezési azonosító**: egy rekord kibocsátó, amikor egy egyedi Sorozatazonosító adható meg.</span><span class="sxs-lookup"><span data-stu-id="f40a4-260">**Sequence ID**: When emitting a tuple, a unique sequence ID can be specified.</span></span> <span data-ttu-id="f40a4-261">Ez az érték a rekord a visszajátszás (Ack és sikertelen) feldolgozás azonosítja.</span><span class="sxs-lookup"><span data-stu-id="f40a4-261">This value identifies the tuple for replay (Ack and Fail) processing.</span></span> <span data-ttu-id="f40a4-262">Például a spout a **Storm minta** kibocsátó adatainak használja a következő projekt:</span><span class="sxs-lookup"><span data-stu-id="f40a4-262">For example, the spout in the **Storm Sample** project uses the following when emitting data:</span></span>

        this.ctx.Emit(Constants.DEFAULT_STREAM_ID, new Values(sentence), lastSeqId);

    <span data-ttu-id="f40a4-263">Ez a kód megfelelően kibocsát egy rekord, amely tartalmazza a feladatütemezési azonosítóérték található alapértelmezett adatfolyam mondat **lastSeqId**.</span><span class="sxs-lookup"><span data-stu-id="f40a4-263">This code emits a tuple that contains a sentence to the default stream, with the sequence ID value contained in **lastSeqId**.</span></span> <span data-ttu-id="f40a4-264">Ehhez a példához **lastSeqId** értéke eggyel növekszik, a kibocsátott minden rekordot.</span><span class="sxs-lookup"><span data-stu-id="f40a4-264">For this example, **lastSeqId** is incremented for every tuple emitted.</span></span>

<span data-ttu-id="f40a4-265">Ahogyan az a **Storm minta** projekt, a tranzakciós-e egy összetevő lehet futásidőben, a konfiguráció alapján állítsa be.</span><span class="sxs-lookup"><span data-stu-id="f40a4-265">As demonstrated in the **Storm Sample** project, whether a component is transactional can be set at runtime, based on configuration.</span></span>

## <a name="hybrid-topology-with-c-and-java"></a><span data-ttu-id="f40a4-266">Hibrid C# és Java topológia</span><span class="sxs-lookup"><span data-stu-id="f40a4-266">Hybrid topology with C# and Java</span></span>

<span data-ttu-id="f40a4-267">Data Lake tools for Visual Studio segítségével hibrid topológiák, ha néhány összetevőt C# és Java.</span><span class="sxs-lookup"><span data-stu-id="f40a4-267">You can also use Data Lake tools for Visual Studio to create hybrid topologies, where some components are C# and others are Java.</span></span>

<span data-ttu-id="f40a4-268">Példa egy hibrid topológia, hozzon létre egy projektet, és jelölje ki **Storm hibrid minta**.</span><span class="sxs-lookup"><span data-stu-id="f40a4-268">For an example of a hybrid topology, create a project and select **Storm Hybrid Sample**.</span></span> <span data-ttu-id="f40a4-269">Ez a minta-típus a következő fogalmakat mutatja be:</span><span class="sxs-lookup"><span data-stu-id="f40a4-269">This sample type demonstrates the following concepts:</span></span>

* <span data-ttu-id="f40a4-270">**Java spout** és **C# bolt**: az **HybridTopology_javaSpout_csharpBolt**.</span><span class="sxs-lookup"><span data-stu-id="f40a4-270">**Java spout** and **C# bolt**: Defined in **HybridTopology_javaSpout_csharpBolt**.</span></span>

    * <span data-ttu-id="f40a4-271">A tranzakciós verzió van megadva **HybridTopologyTx_javaSpout_csharpBolt**.</span><span class="sxs-lookup"><span data-stu-id="f40a4-271">A transactional version is defined in **HybridTopologyTx_javaSpout_csharpBolt**.</span></span>

* <span data-ttu-id="f40a4-272">**C# spout** és **Java bolt**: az **HybridTopology_csharpSpout_javaBolt**.</span><span class="sxs-lookup"><span data-stu-id="f40a4-272">**C# spout** and **Java bolt**: Defined in **HybridTopology_csharpSpout_javaBolt**.</span></span>

    * <span data-ttu-id="f40a4-273">A tranzakciós verzió van megadva **HybridTopologyTx_csharpSpout_javaBolt**.</span><span class="sxs-lookup"><span data-stu-id="f40a4-273">A transactional version is defined in **HybridTopologyTx_csharpSpout_javaBolt**.</span></span>

  > [!NOTE]
  > <span data-ttu-id="f40a4-274">Ebben a verzióban is bemutatja, hogyan használják ki egy szövegfájlból Clojure kód egy Java-összetevő.</span><span class="sxs-lookup"><span data-stu-id="f40a4-274">This version also demonstrates how to use Clojure code from a text file as a Java component.</span></span>


<span data-ttu-id="f40a4-275">Váltás a topológia, amikor a projekthez használt, egyszerűen helyezze át a `[Active(true)]` nyilatkozatot, így a topológia szeretné használni, a fürt való továbbítás előtt.</span><span class="sxs-lookup"><span data-stu-id="f40a4-275">To switch the topology that is used when the project is submitted, simply move the `[Active(true)]` statement to the topology you want to use, before submitting it to the cluster.</span></span>

> [!NOTE]
> <span data-ttu-id="f40a4-276">A projekt részeként biztosított összes a Java-fájlokat, amelyek szükségesek a **JavaDependency** mappa.</span><span class="sxs-lookup"><span data-stu-id="f40a4-276">All the Java files that are required are provided as part of this project in the **JavaDependency** folder.</span></span>

<span data-ttu-id="f40a4-277">Amikor létrehozása és elküldése a hibrid topológia, vegye figyelembe a következőket:</span><span class="sxs-lookup"><span data-stu-id="f40a4-277">Consider the following when you are creating and submitting a hybrid topology:</span></span>

* <span data-ttu-id="f40a4-278">Használjon **JavaComponentConstructor** egy spout a Java-osztály példányának létrehozása vagy boltok esetében.</span><span class="sxs-lookup"><span data-stu-id="f40a4-278">You must use **JavaComponentConstructor** to create an instance of the Java class for a spout or bolt.</span></span>

* <span data-ttu-id="f40a4-279">Használjon **microsoft.scp.storm.multilang.CustomizedInteropJSONSerializer** adatok szerializálása a virtuális gépbe vagy onnan Pojo objektumait JSON-Java összetevőt.</span><span class="sxs-lookup"><span data-stu-id="f40a4-279">You should use **microsoft.scp.storm.multilang.CustomizedInteropJSONSerializer** to serialize data into or out of Java components from Java objects to JSON.</span></span>

* <span data-ttu-id="f40a4-280">Ha a topológia a kiszolgálóra történő elküldésekor kell használnia a **további konfigurációs** beállítással adhatja meg a **Java elérési utat**.</span><span class="sxs-lookup"><span data-stu-id="f40a4-280">When submitting the topology to the server, you must use the **Additional configurations** option to specify the **Java File paths**.</span></span> <span data-ttu-id="f40a4-281">A megadott elérési utat kell lennie a Java-osztályok tartalmazó JAR-fájlokat tartalmazó könyvtárat.</span><span class="sxs-lookup"><span data-stu-id="f40a4-281">The path specified should be the directory that contains the JAR files that contain your Java classes.</span></span>

### <a name="azure-event-hubs"></a><span data-ttu-id="f40a4-282">Azure Event Hubs</span><span class="sxs-lookup"><span data-stu-id="f40a4-282">Azure Event Hubs</span></span>

<span data-ttu-id="f40a4-283">SCP.NET 0.9.4.203 tartalmazza egy új osztályt és metódus kimondottan az Event Hub spout (a Java spout az Event Hubs olvasó) használata.</span><span class="sxs-lookup"><span data-stu-id="f40a4-283">SCP.NET version 0.9.4.203 introduces a new class and method specifically for working with the Event Hub spout (a Java spout that reads from Event Hubs).</span></span> <span data-ttu-id="f40a4-284">A topológia egy Event Hub spout használó létrehozásakor az alábbi módszerekkel:</span><span class="sxs-lookup"><span data-stu-id="f40a4-284">When you create a topology that uses an Event Hub spout, use the following methods:</span></span>

* <span data-ttu-id="f40a4-285">**EventHubSpoutConfig** osztály: hoz létre egy objektumot, amely tartalmazza a spout összetevő konfigurációját.</span><span class="sxs-lookup"><span data-stu-id="f40a4-285">**EventHubSpoutConfig** class: Creates an object that contains the configuration for the spout component.</span></span>

* <span data-ttu-id="f40a4-286">**TopologyBuilder.SetEventHubSpout** módszer: az Event Hub spout összetevő felvétele a topológiát.</span><span class="sxs-lookup"><span data-stu-id="f40a4-286">**TopologyBuilder.SetEventHubSpout** method: Adds the Event Hub spout component to the topology.</span></span>

> [!NOTE]
> <span data-ttu-id="f40a4-287">Továbbra is használnia kell a **CustomizedInteropJSONSerializer** szerializálni a spout által visszaadott adatokat.</span><span class="sxs-lookup"><span data-stu-id="f40a4-287">You must still use the **CustomizedInteropJSONSerializer** to serialize data produced by the spout.</span></span>

## <span data-ttu-id="f40a4-288"><a id="configurationmanager"></a>ConfigurationManager használata</span><span class="sxs-lookup"><span data-stu-id="f40a4-288"><a id="configurationmanager"></a>Use ConfigurationManager</span></span>

<span data-ttu-id="f40a4-289">Ne használjon **ConfigurationManager** konfigurációs értékek lekérése bolt és összetevők spout.</span><span class="sxs-lookup"><span data-stu-id="f40a4-289">Don't use **ConfigurationManager** to retrieve configuration values from bolt and spout components.</span></span> <span data-ttu-id="f40a4-290">Így okozhat, null mutató által-kivétel.</span><span class="sxs-lookup"><span data-stu-id="f40a4-290">Doing so can cause a null pointer exception.</span></span> <span data-ttu-id="f40a4-291">Ehelyett a projekthez konfigurációs átad a a Storm-topológia a topológia környezetben kulcs-érték párban.</span><span class="sxs-lookup"><span data-stu-id="f40a4-291">Instead, the configuration for your project is passed into the Storm topology as a key and value pair in the topology context.</span></span> <span data-ttu-id="f40a4-292">Minden egyes összetevő, amely támaszkodik a konfigurációs értékeket kell lekérnie őket a környezetben inicializálása során.</span><span class="sxs-lookup"><span data-stu-id="f40a4-292">Each component that relies on configuration values must retrieve them from the context during initialization.</span></span>

<span data-ttu-id="f40a4-293">A következő kód bemutatja, hogyan lehet lekérni a ezeket az értékeket:</span><span class="sxs-lookup"><span data-stu-id="f40a4-293">The following code demonstrates how to retrieve these values:</span></span>

```csharp
public class MyComponent : ISCPBolt
{
    // To hold configuration information loaded from context
    Configuration configuration;
    ...
    public MyComponent(Context ctx, Dictionary<string, Object> parms)
    {
        // Save a copy of the context for this component instance
        this.ctx = ctx;
        // If it exists, load the configuration for the component
        if(parms.ContainsKey(Constants.USER_CONFIG))
        {
            this.configuration = parms[Constants.USER_CONFIG] as System.Configuration.Configuration;
        }
        // Retrieve the value of "Foo" from configuration
        var foo = this.configuration.AppSettings.Settings["Foo"].Value;
    }
    ...
}
```

<span data-ttu-id="f40a4-294">Ha egy `Get` metódus egy példány a összetevő visszaadása meg kell győződnie arról, hogy mindkét átadja a `Context` és `Dictionary<string, Object>` a konstruktor paramétereit.</span><span class="sxs-lookup"><span data-stu-id="f40a4-294">If you use a `Get` method to return an instance of your component, you must ensure that it passes both the `Context` and `Dictionary<string, Object>` parameters to the constructor.</span></span> <span data-ttu-id="f40a4-295">A következő példa egy alapvető `Get` módszer, amelynek megfelelően továbbítja ezeket az értékeket:</span><span class="sxs-lookup"><span data-stu-id="f40a4-295">The following example is a basic `Get` method that properly passes these values:</span></span>

```csharp
public static MyComponent Get(Context ctx, Dictionary<string, Object> parms)
{
    return new MyComponent(ctx, parms);
}
```

## <a name="how-to-update-scpnet"></a><span data-ttu-id="f40a4-296">SCP.NET frissítése</span><span class="sxs-lookup"><span data-stu-id="f40a4-296">How to update SCP.NET</span></span>

<span data-ttu-id="f40a4-297">SCP.NET újabb kiadásaiban a Nugeten keresztül Csomagfrissítés támogatja.</span><span class="sxs-lookup"><span data-stu-id="f40a4-297">Recent releases of SCP.NET support package upgrade through NuGet.</span></span> <span data-ttu-id="f40a4-298">Ha egy új frissítés érhető el, egy frissítési értesítést kap.</span><span class="sxs-lookup"><span data-stu-id="f40a4-298">When a new update is available, you receive an upgrade notification.</span></span> <span data-ttu-id="f40a4-299">Frissítés manuálisan ellenőrzéséhez kövesse az alábbi lépéseket:</span><span class="sxs-lookup"><span data-stu-id="f40a4-299">To manually check for an upgrade, follow these steps:</span></span>

1. <span data-ttu-id="f40a4-300">A **Megoldáskezelőben**, kattintson jobb gombbal a projektre, és válassza ki **NuGet-csomagok kezelése**.</span><span class="sxs-lookup"><span data-stu-id="f40a4-300">In **Solution Explorer**, right-click the project, and select **Manage NuGet Packages**.</span></span>

2. <span data-ttu-id="f40a4-301">Válassza ki a package manager **frissítések**.</span><span class="sxs-lookup"><span data-stu-id="f40a4-301">From the package manager, select **Updates**.</span></span> <span data-ttu-id="f40a4-302">Frissítés érhető el, ha szerepel.</span><span class="sxs-lookup"><span data-stu-id="f40a4-302">If an update is available, it is listed.</span></span> <span data-ttu-id="f40a4-303">Kattintson a **frissítés** a csomag telepítéséhez.</span><span class="sxs-lookup"><span data-stu-id="f40a4-303">Click **Update** for the package to install it.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="f40a4-304">Ha a projekt, amely nem NuGet SCP.NET korábbi verziójával lett létrehozva, újabb verzióra való frissítése a következő lépéseket kell elvégeznie:</span><span class="sxs-lookup"><span data-stu-id="f40a4-304">If your project was created with an earlier version of SCP.NET that did not use NuGet, you must perform the following steps to update to a newer version:</span></span>
>
> 1. <span data-ttu-id="f40a4-305">A **Megoldáskezelőben**, kattintson jobb gombbal a projektre, és válassza ki **NuGet-csomagok kezelése**.</span><span class="sxs-lookup"><span data-stu-id="f40a4-305">In **Solution Explorer**, right-click the project, and select **Manage NuGet Packages**.</span></span>
> 2. <span data-ttu-id="f40a4-306">Használja a **keresési** mezőben, keresse meg, és adja hozzá, **Microsoft.SCP.Net.SDK** a projekthez.</span><span class="sxs-lookup"><span data-stu-id="f40a4-306">Using the **Search** field, search for, and then add, **Microsoft.SCP.Net.SDK** to the project.</span></span>

## <a name="troubleshoot-common-issues-with-topologies"></a><span data-ttu-id="f40a4-307">Topológiák termékkel kapcsolatos gyakori hibák elhárítása</span><span class="sxs-lookup"><span data-stu-id="f40a4-307">Troubleshoot common issues with topologies</span></span>

### <a name="null-pointer-exceptions"></a><span data-ttu-id="f40a4-308">NULL mutatót kivételek</span><span class="sxs-lookup"><span data-stu-id="f40a4-308">Null pointer exceptions</span></span>

<span data-ttu-id="f40a4-309">C#-topológiák használatakor a Linux-alapú HDInsight-fürthöz boltok és összetevőket spout **ConfigurationManager** olvasni a konfigurációs beállításokat futásidőben adhat vissza null mutatót kivételek.</span><span class="sxs-lookup"><span data-stu-id="f40a4-309">When you are using a C# topology with a Linux-based HDInsight cluster, bolt and spout components that use **ConfigurationManager** to read configuration settings at runtime may return null pointer exceptions.</span></span>

<span data-ttu-id="f40a4-310">A konfiguráció a projekthez átad a Storm-topológia a topológia környezetében kulcs-érték párban.</span><span class="sxs-lookup"><span data-stu-id="f40a4-310">The configuration for your project is passed into the Storm topology as a key and value pair in the topology context.</span></span> <span data-ttu-id="f40a4-311">A rendszer inicializálásakor a összetevők átadott szótárobjektum lekérhetők.</span><span class="sxs-lookup"><span data-stu-id="f40a4-311">It can be retrieved from the dictionary object that is passed to your components when they are initialized.</span></span>

<span data-ttu-id="f40a4-312">További információkért lásd: a [ConfigurationManager](#configurationmanager) szakasz ebben a dokumentumban.</span><span class="sxs-lookup"><span data-stu-id="f40a4-312">For more information, see the [ConfigurationManager](#configurationmanager) section of this document.</span></span>

### <a name="systemtypeloadexception"></a><span data-ttu-id="f40a4-313">System.TypeLoadException</span><span class="sxs-lookup"><span data-stu-id="f40a4-313">System.TypeLoadException</span></span>

<span data-ttu-id="f40a4-314">Amikor egy Linux-alapú HDInsight-fürthöz C#-topológiák használ, a következő hiba merülhetnek fel:</span><span class="sxs-lookup"><span data-stu-id="f40a4-314">When you are using a C# topology with a Linux-based HDInsight cluster, you may encounter the following error:</span></span>

    System.TypeLoadException: Failure has occurred while loading a type.

<span data-ttu-id="f40a4-315">Ez akkor fordul elő, ha egy bináris, amely nem kompatibilis a monó támogató .NET verzióját használja.</span><span class="sxs-lookup"><span data-stu-id="f40a4-315">This error occurs when you use a binary that is not compatible with the version of .NET that Mono supports.</span></span>

<span data-ttu-id="f40a4-316">Linux-alapú HDInsight-fürtök győződjön meg arról, hogy a projekt használ-e a bináris fájlok a .NET 4.5 számára.</span><span class="sxs-lookup"><span data-stu-id="f40a4-316">For Linux-based HDInsight clusters, make sure that your project uses binaries compiled for .NET 4.5.</span></span>

### <a name="test-a-topology-locally"></a><span data-ttu-id="f40a4-317">A topológia helyi tesztelése</span><span class="sxs-lookup"><span data-stu-id="f40a4-317">Test a topology locally</span></span>

<span data-ttu-id="f40a4-318">Egyszerűen a topológia telepíthető a fürtre, néhány esetben azonban szükség lehet a topológia helyi tesztelése.</span><span class="sxs-lookup"><span data-stu-id="f40a4-318">Although it is easy to deploy a topology to a cluster, in some cases, you may need to test a topology locally.</span></span> <span data-ttu-id="f40a4-319">Az alábbi lépések segítségével futtathatja és tesztelheti a példa topológia ebben az oktatóanyagban helyileg a fejlesztői környezetben.</span><span class="sxs-lookup"><span data-stu-id="f40a4-319">Use the following steps to run and test the example topology in this tutorial locally in your development environment.</span></span>

> [!WARNING]
> <span data-ttu-id="f40a4-320">Helyi tesztelése csak akkor működik, a basic, a C#-topológiák csak.</span><span class="sxs-lookup"><span data-stu-id="f40a4-320">Local testing only works for basic, C#-only topologies.</span></span> <span data-ttu-id="f40a4-321">A hibrid topológiák vagy topológiák több adatfolyam használó helyi tesztelése nem használható.</span><span class="sxs-lookup"><span data-stu-id="f40a4-321">You cannot use local testing for hybrid topologies or topologies that use multiple streams.</span></span>

1. <span data-ttu-id="f40a4-322">A **Megoldáskezelőben**, kattintson jobb gombbal a projektre, és válassza ki **tulajdonságok**.</span><span class="sxs-lookup"><span data-stu-id="f40a4-322">In **Solution Explorer**, right-click the project, and select **Properties**.</span></span> <span data-ttu-id="f40a4-323">A projekt tulajdonságait, módosítsa a **kimeneti típus** való **Konzolalkalmazás**.</span><span class="sxs-lookup"><span data-stu-id="f40a4-323">In the project properties, change the **Output type** to **Console Application**.</span></span>

    ![Képernyőfelvétel a projekt tulajdonságait, a kiemelt kimeneti típusú](./media/hdinsight-storm-develop-csharp-visual-studio-topology/outputtype.png)

   > [!NOTE]
   > <span data-ttu-id="f40a4-325">Ne felejtse el módosítani a **kimeneti típus** vissza **Class Library** a topológia egy fürt központi telepítése előtt.</span><span class="sxs-lookup"><span data-stu-id="f40a4-325">Remember to change the **Output type** back to **Class Library** before you deploy the topology to a cluster.</span></span>

2. <span data-ttu-id="f40a4-326">A **Megoldáskezelőben**, kattintson jobb gombbal a projektre, majd válassza ki **Hozzáadás** > **új elem**.</span><span class="sxs-lookup"><span data-stu-id="f40a4-326">In **Solution Explorer**, right-click the project, and then select **Add** > **New Item**.</span></span> <span data-ttu-id="f40a4-327">Válassza ki **osztály**, és írja be **LocalTest.cs** osztály neveként.</span><span class="sxs-lookup"><span data-stu-id="f40a4-327">Select **Class**, and enter **LocalTest.cs** as the class name.</span></span> <span data-ttu-id="f40a4-328">Végezetül kattintson **Hozzáadás**.</span><span class="sxs-lookup"><span data-stu-id="f40a4-328">Finally, click **Add**.</span></span>

3. <span data-ttu-id="f40a4-329">Nyissa meg **LocalTest.cs**, és adja hozzá a következő **használatával** felső utasítást:</span><span class="sxs-lookup"><span data-stu-id="f40a4-329">Open **LocalTest.cs**, and add the following **using** statement at the top:</span></span>

    ```csharp
    using Microsoft.SCP;
    ```

4. <span data-ttu-id="f40a4-330">A következő kódot használja, a tartalmát a **LocalTest** osztály:</span><span class="sxs-lookup"><span data-stu-id="f40a4-330">Use the following code as the contents of the **LocalTest** class:</span></span>

    ```csharp
    // Drives the topology components
    public void RunTestCase()
    {
        // An empty dictionary for use when creating components
        Dictionary<string, Object> emptyDictionary = new Dictionary<string, object>();

        #region Test the spout
        {
            Console.WriteLine("Starting spout");
            // LocalContext is a local-mode context that can be used to initialize
            // components in the development environment.
            LocalContext spoutCtx = LocalContext.Get();
            // Get a new instance of the spout, using the local context
            Spout sentences = Spout.Get(spoutCtx, emptyDictionary);

            // Emit 10 tuples
            for (int i = 0; i < 10; i++)
            {
                sentences.NextTuple(emptyDictionary);
            }
            // Use LocalContext to persist the data stream to file
            spoutCtx.WriteMsgQueueToFile("sentences.txt");
            Console.WriteLine("Spout finished");
        }
        #endregion

        #region Test the splitter bolt
        {
            Console.WriteLine("Starting splitter bolt");
            // LocalContext is a local-mode context that can be used to initialize
            // components in the development environment.
            LocalContext splitterCtx = LocalContext.Get();
            // Get a new instance of the bolt
            Splitter splitter = Splitter.Get(splitterCtx, emptyDictionary);

            // Set the data stream to the data created by the spout
            splitterCtx.ReadFromFileToMsgQueue("sentences.txt");
            // Get a batch of tuples from the stream
            List<SCPTuple> batch = splitterCtx.RecvFromMsgQueue();
            // Process each tuple in the batch
            foreach (SCPTuple tuple in batch)
            {
                splitter.Execute(tuple);
            }
            // Use LocalContext to persist the data stream to file
            splitterCtx.WriteMsgQueueToFile("splitter.txt");
            Console.WriteLine("Splitter bolt finished");
        }
        #endregion

        #region Test the counter bolt
        {
            Console.WriteLine("Starting counter bolt");
            // LocalContext is a local-mode context that can be used to initialize
            // components in the development environment.
            LocalContext counterCtx = LocalContext.Get();
            // Get a new instance of the bolt
            Counter counter = Counter.Get(counterCtx, emptyDictionary);

            // Set the data stream to the data created by splitter bolt
            counterCtx.ReadFromFileToMsgQueue("splitter.txt");
            // Get a batch of tuples from the stream
            List<SCPTuple> batch = counterCtx.RecvFromMsgQueue();
            // Process each tuple in the batch
            foreach (SCPTuple tuple in batch)
            {
                counter.Execute(tuple);
            }
            // Use LocalContext to persist the data stream to file
            counterCtx.WriteMsgQueueToFile("counter.txt");
            Console.WriteLine("Counter bolt finished");
        }
        #endregion
    }
    ```

    <span data-ttu-id="f40a4-331">A kód megjegyzéseket keresztül néhány percet igénybe vehet.</span><span class="sxs-lookup"><span data-stu-id="f40a4-331">Take a moment to read through the code comments.</span></span> <span data-ttu-id="f40a4-332">Ezt a kódot használja **LocalContext** futtatni a a fejlesztési környezetet, és továbbra is fennáll, a szöveges fájlt a helyi meghajtón összetevők közötti adatfolyamban.</span><span class="sxs-lookup"><span data-stu-id="f40a4-332">This code uses **LocalContext** to run the components in the development environment, and it persists the data stream between components to text files on the local drive.</span></span>

1. <span data-ttu-id="f40a4-333">Nyissa meg **Program.cs**, és adja hozzá a következőt a **fő** módszert:</span><span class="sxs-lookup"><span data-stu-id="f40a4-333">Open **Program.cs**, and add the following to the **Main** method:</span></span>

    ```csharp
    Console.WriteLine("Starting tests");
    System.Environment.SetEnvironmentVariable("microsoft.scp.logPrefix", "WordCount-LocalTest");
    // Initialize the runtime
    SCPRuntime.Initialize();

    //If we are not running under the local context, throw an error
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

2. <span data-ttu-id="f40a4-334">Menti a módosításokat, és kattintson a **F5** , vagy válasszon **Debug** > **Start Debugging** a projekt indításához.</span><span class="sxs-lookup"><span data-stu-id="f40a4-334">Save the changes, and then click **F5** or select **Debug** > **Start Debugging** to start the project.</span></span> <span data-ttu-id="f40a4-335">A konzolablakban kell jelenik meg, és a tesztek állapotát állapot naplózása.</span><span class="sxs-lookup"><span data-stu-id="f40a4-335">A console window should appear, and log status as the tests progress.</span></span> <span data-ttu-id="f40a4-336">Ha **tesztek befejeződött** jelenik meg, nyomja le bármelyik billentyűt az ablak bezárásához.</span><span class="sxs-lookup"><span data-stu-id="f40a4-336">When **Tests finished** appears, press any key to close the window.</span></span>

3. <span data-ttu-id="f40a4-337">Használjon **Windows Explorer** kereséséhez a könyvtárban, amely tartalmazza a projekthez.</span><span class="sxs-lookup"><span data-stu-id="f40a4-337">Use **Windows Explorer** to locate the directory that contains your project.</span></span> <span data-ttu-id="f40a4-338">Például: **C:\Users\<your_user_name > \Documents\Visual Studio 2013\Projects\WordCount\WordCount**.</span><span class="sxs-lookup"><span data-stu-id="f40a4-338">For example: **C:\Users\<your_user_name>\Documents\Visual Studio 2013\Projects\WordCount\WordCount**.</span></span> <span data-ttu-id="f40a4-339">Ebben a könyvtárban, nyissa meg a **Bin**, és kattintson a **Debug**.</span><span class="sxs-lookup"><span data-stu-id="f40a4-339">In this directory, open **Bin**, and then click **Debug**.</span></span> <span data-ttu-id="f40a4-340">A szövegfájlok, amelyeket a vizsgálatok futtatásakor kell megjelennie: sentences.txt counter.txt és splitter.txt.</span><span class="sxs-lookup"><span data-stu-id="f40a4-340">You should see the text files that were produced when the tests ran: sentences.txt, counter.txt, and splitter.txt.</span></span> <span data-ttu-id="f40a4-341">Nyissa meg minden egyes szöveges fájlt, és vizsgálja meg az adatokat.</span><span class="sxs-lookup"><span data-stu-id="f40a4-341">Open each text file and inspect the data.</span></span>

   > [!NOTE]
   > <span data-ttu-id="f40a4-342">Karakterlánc típusú adatok továbbra is fennáll, ezeket a fájlokat a decimális értékek ábrázolva.</span><span class="sxs-lookup"><span data-stu-id="f40a4-342">String data persists as an array of decimal values in these files.</span></span> <span data-ttu-id="f40a4-343">Például \[[97,103,111]] az az **splitter.txt** fájl szó *és*.</span><span class="sxs-lookup"><span data-stu-id="f40a4-343">For example, \[[97,103,111]] in the **splitter.txt** file is the word *and*.</span></span>

> [!NOTE]
> <span data-ttu-id="f40a4-344">Meg kell adni a **projekttípus** vissza **Class Library** a Storm on HDInsight-fürt üzembe helyezése előtt.</span><span class="sxs-lookup"><span data-stu-id="f40a4-344">Be sure to set the **Project type** back to **Class Library** before deploying to a Storm on HDInsight cluster.</span></span>

### <a name="log-information"></a><span data-ttu-id="f40a4-345">Naplóadatok</span><span class="sxs-lookup"><span data-stu-id="f40a4-345">Log information</span></span>

<span data-ttu-id="f40a4-346">Könnyen bejelentkezhet információkat a topológia összetevői használatával `Context.Logger`.</span><span class="sxs-lookup"><span data-stu-id="f40a4-346">You can easily log information from your topology components by using `Context.Logger`.</span></span> <span data-ttu-id="f40a4-347">Például a következő létrehoz egy tájékoztató naplóbejegyzés:</span><span class="sxs-lookup"><span data-stu-id="f40a4-347">For example, the following creates an informational log entry:</span></span>

```csharp
Context.Logger.Info("Component started");
```

<span data-ttu-id="f40a4-348">A naplózott információk is megtekinthetők a **Hadoop Szolgáltatásnaplót**, amely megtalálható **Server Explorer**.</span><span class="sxs-lookup"><span data-stu-id="f40a4-348">Logged information can be viewed from the **Hadoop Service Log**, which is found in **Server Explorer**.</span></span> <span data-ttu-id="f40a4-349">Bontsa ki a bejegyzést a Storm on HDInsight-fürt számára, és végül **Hadoop Szolgáltatásnaplót**.</span><span class="sxs-lookup"><span data-stu-id="f40a4-349">Expand the entry for your Storm on HDInsight cluster, and then expand **Hadoop Service Log**.</span></span> <span data-ttu-id="f40a4-350">Végül válassza ki a naplófájl megtekintése.</span><span class="sxs-lookup"><span data-stu-id="f40a4-350">Finally, select the log file to view.</span></span>

> [!NOTE]
> <span data-ttu-id="f40a4-351">A naplók az Azure storage-fiók, amely a fürt által használt vannak tárolva.</span><span class="sxs-lookup"><span data-stu-id="f40a4-351">The logs are stored in the Azure storage account that is used by your cluster.</span></span> <span data-ttu-id="f40a4-352">A naplók megtekintéséhez a Visual Studio be kell jelentkeznie Azure-előfizetést, amely a tárfiók tulajdonosa számára.</span><span class="sxs-lookup"><span data-stu-id="f40a4-352">To view the logs in Visual Studio, you must sign in to the Azure subscription that owns the storage account.</span></span>

### <a name="view-error-information"></a><span data-ttu-id="f40a4-353">Hiba történt adatainak megtekintése</span><span class="sxs-lookup"><span data-stu-id="f40a4-353">View error information</span></span>

<span data-ttu-id="f40a4-354">A futó topológiát a bekövetkezett hibák megtekintéséhez használja a következő lépéseket:</span><span class="sxs-lookup"><span data-stu-id="f40a4-354">To view errors that have occurred in a running topology, use the following steps:</span></span>

1. <span data-ttu-id="f40a4-355">A **Server Explorer**, kattintson a jobb gombbal a Storm on HDInsight-fürt, és válassza ki **nézet Storm-topológiák**.</span><span class="sxs-lookup"><span data-stu-id="f40a4-355">From **Server Explorer**, right-click the Storm on HDInsight cluster, and select **View Storm topologies**.</span></span>

2. <span data-ttu-id="f40a4-356">Az a **Spout** és **boltok**, a **utolsó hiba** oszlop az utolsó hiba információkat tartalmaz.</span><span class="sxs-lookup"><span data-stu-id="f40a4-356">For the **Spout** and **Bolts**, the **Last Error** column contains information on the last error.</span></span>

3. <span data-ttu-id="f40a4-357">Válassza ki a **Spout azonosító** vagy **Bolt azonosító** felsorolt hibás összetevő.</span><span class="sxs-lookup"><span data-stu-id="f40a4-357">Select the **Spout Id** or **Bolt Id** for the component that has an error listed.</span></span> <span data-ttu-id="f40a4-358">Amely hiba jelenik meg, további részleteit megjelenítő oldalon információ szerepel a **hibák** szakasz az oldal alján.</span><span class="sxs-lookup"><span data-stu-id="f40a4-358">On the details page that is displayed, additional error information is listed in the **Errors** section at the bottom of the page.</span></span>

4. <span data-ttu-id="f40a4-359">További információk beszerzéséhez válassza ki egy **Port** a a **végrehajtója** lap, a munkavégző Storm napló megtekintéséhez az elmúlt néhány perc részében.</span><span class="sxs-lookup"><span data-stu-id="f40a4-359">To obtain more information, select a **Port** from the **Executors** section of the page, to see the Storm worker log for the last few minutes.</span></span>

### <a name="errors-submitting-topologies"></a><span data-ttu-id="f40a4-360">Hibák elküldése a topológiák</span><span class="sxs-lookup"><span data-stu-id="f40a4-360">Errors submitting topologies</span></span>

<span data-ttu-id="f40a4-361">Ha hibákba ütközik egy HDInsight-topológiát elküldése, naplók a kiszolgálóoldali összetevők, amelyek kezelik a HDInsight-fürt topológiája küldésének találja.</span><span class="sxs-lookup"><span data-stu-id="f40a4-361">If you encounter errors submitting a topology to HDInsight, you can find logs for the server-side components that handle topology submission on your HDInsight cluster.</span></span> <span data-ttu-id="f40a4-362">Ezek a naplók lekéréséhez használja a következő parancsot a parancssorból:</span><span class="sxs-lookup"><span data-stu-id="f40a4-362">To retrieve these logs, use the following command from a command line:</span></span>

    scp sshuser@clustername-ssh.azurehdinsight.net:/var/log/hdinsight-scpwebapi/hdinsight-scpwebapi.out .

<span data-ttu-id="f40a4-363">Cserélje le __sshuser__ a fürthöz SSH felhasználói fiókkal.</span><span class="sxs-lookup"><span data-stu-id="f40a4-363">Replace __sshuser__ with the SSH user account for the cluster.</span></span> <span data-ttu-id="f40a4-364">Cserélje le __clustername__ a HDInsight-fürt nevét.</span><span class="sxs-lookup"><span data-stu-id="f40a4-364">Replace __clustername__ with the name of the HDInsight cluster.</span></span> <span data-ttu-id="f40a4-365">További információk az `scp` és `ssh` a hdinsight eszközzel, lásd: [az SSH a Hdinsighttal](hdinsight-hadoop-linux-use-ssh-unix.md).</span><span class="sxs-lookup"><span data-stu-id="f40a4-365">For more information on using `scp` and `ssh` with HDInsight, see [Use SSH with HDInsight](hdinsight-hadoop-linux-use-ssh-unix.md).</span></span>

<span data-ttu-id="f40a4-366">Több okból sikertelen lehet a jelentések:</span><span class="sxs-lookup"><span data-stu-id="f40a4-366">Submissions can fail for multiple reasons:</span></span>

* <span data-ttu-id="f40a4-367">JDK nincs telepítve, vagy az elérési út nem tartalmazza.</span><span class="sxs-lookup"><span data-stu-id="f40a4-367">JDK is not installed or is not in the path.</span></span>
* <span data-ttu-id="f40a4-368">Java szükséges függőség nem szerepelnek a küldése.</span><span class="sxs-lookup"><span data-stu-id="f40a4-368">Required Java dependencies are not included in the submission.</span></span>
* <span data-ttu-id="f40a4-369">Nem kompatibilis függőségek.</span><span class="sxs-lookup"><span data-stu-id="f40a4-369">Incompatible dependencies.</span></span>
* <span data-ttu-id="f40a4-370">Ismétlődő topológia nevek.</span><span class="sxs-lookup"><span data-stu-id="f40a4-370">Duplicate topology names.</span></span>

<span data-ttu-id="f40a4-371">Ha a `hdinsight-scpwebapi.out` a napló tartalmaz egy `FileNotFoundException`, ennek oka lehet az alábbi feltételek:</span><span class="sxs-lookup"><span data-stu-id="f40a4-371">If the `hdinsight-scpwebapi.out` log contains a `FileNotFoundException`, this might be caused by the following conditions:</span></span>

* <span data-ttu-id="f40a4-372">A JDK nincs a fejlesztési környezet elérési útját.</span><span class="sxs-lookup"><span data-stu-id="f40a4-372">The JDK is not in the path on the development environment.</span></span> <span data-ttu-id="f40a4-373">Győződjön meg arról, hogy a JDK telepítve van-e a fejlesztési környezetet, és hogy `%JAVA_HOME%/bin` az elérési út része.</span><span class="sxs-lookup"><span data-stu-id="f40a4-373">Verify that the JDK is installed in the development environment, and that `%JAVA_HOME%/bin` is in the path.</span></span>
* <span data-ttu-id="f40a4-374">Java függősége hiányzik.</span><span class="sxs-lookup"><span data-stu-id="f40a4-374">You are missing a Java dependency.</span></span> <span data-ttu-id="f40a4-375">Ellenőrizze, hogy a küldése részeként beleértve a minden szükséges .jar fájlt.</span><span class="sxs-lookup"><span data-stu-id="f40a4-375">Make sure you are including any required .jar files as part of the submission.</span></span>

## <a name="next-steps"></a><span data-ttu-id="f40a4-376">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="f40a4-376">Next steps</span></span>

<span data-ttu-id="f40a4-377">Például az adatok feldolgozása az Event Hubs, [feldolgozni az eseményeket az Azure Event Hubs a HDInsight alatt futó Storm](hdinsight-storm-develop-csharp-event-hub-topology.md).</span><span class="sxs-lookup"><span data-stu-id="f40a4-377">For an example of processing data from Event Hubs, see [Process events from Azure Event Hubs with Storm on HDInsight](hdinsight-storm-develop-csharp-event-hub-topology.md).</span></span>

<span data-ttu-id="f40a4-378">Például egy C#-topológiák, amely felosztja a több adatfolyam adatfolyam-adatokat, lásd: [C# Storm példa](https://github.com/Blackmist/csharp-storm-example).</span><span class="sxs-lookup"><span data-stu-id="f40a4-378">For an example of a C# topology that splits stream data into multiple streams, see [C# Storm example](https://github.com/Blackmist/csharp-storm-example).</span></span>

<span data-ttu-id="f40a4-379">C#-topológiák létrehozásával kapcsolatos további információk felderítésére, lásd: [GitHub](https://github.com/hdinsight/hdinsight-storm-examples/blob/master/SCPNet-GettingStarted.md).</span><span class="sxs-lookup"><span data-stu-id="f40a4-379">To discover more information about creating C# topologies, see [GitHub](https://github.com/hdinsight/hdinsight-storm-examples/blob/master/SCPNet-GettingStarted.md).</span></span>

<span data-ttu-id="f40a4-380">További részleteket a HDInsight használata és minták a HDInsight alatt futó Storm további tekintse meg a következő dokumentumokat:</span><span class="sxs-lookup"><span data-stu-id="f40a4-380">For more ways to work with HDInsight and more Storm on HDInsight samples, see the following documents:</span></span>

<span data-ttu-id="f40a4-381">**Microsoft SCP.NET**</span><span class="sxs-lookup"><span data-stu-id="f40a4-381">**Microsoft SCP.NET**</span></span>

* [<span data-ttu-id="f40a4-382">Szolgáltatáskapcsolódási pont programozási útmutató</span><span class="sxs-lookup"><span data-stu-id="f40a4-382">SCP programming guide</span></span>](hdinsight-storm-scp-programming-guide.md)

<span data-ttu-id="f40a4-383">**A HDInsight alatt futó Apache Storm**</span><span class="sxs-lookup"><span data-stu-id="f40a4-383">**Apache Storm on HDInsight**</span></span>

* [<span data-ttu-id="f40a4-384">Telepítheti és figyelheti a HDInsight alatt futó Apache Storm topológiák</span><span class="sxs-lookup"><span data-stu-id="f40a4-384">Deploy and monitor topologies with Apache Storm on HDInsight</span></span>](hdinsight-storm-deploy-monitor-topology.md)
* [<span data-ttu-id="f40a4-385">HDInsight alatt futó Storm példatopológiái</span><span class="sxs-lookup"><span data-stu-id="f40a4-385">Example topologies for Storm on HDInsight</span></span>](hdinsight-storm-example-topology.md)

<span data-ttu-id="f40a4-386">**Apache Hadoop on HDInsight**</span><span class="sxs-lookup"><span data-stu-id="f40a4-386">**Apache Hadoop on HDInsight**</span></span>

* [<span data-ttu-id="f40a4-387">A Hive használata a hdinsight Hadoop</span><span class="sxs-lookup"><span data-stu-id="f40a4-387">Use Hive with Hadoop on HDInsight</span></span>](hdinsight-use-hive.md)
* [<span data-ttu-id="f40a4-388">A Pig használata a HDInsight Hadoop</span><span class="sxs-lookup"><span data-stu-id="f40a4-388">Use Pig with Hadoop on HDInsight</span></span>](hdinsight-use-pig.md)
* [<span data-ttu-id="f40a4-389">A HDInsight Hadoop MapReduce használata</span><span class="sxs-lookup"><span data-stu-id="f40a4-389">Use MapReduce with Hadoop on HDInsight</span></span>](hdinsight-use-mapreduce.md)

<span data-ttu-id="f40a4-390">**Apache HBase a HDInsight-on**</span><span class="sxs-lookup"><span data-stu-id="f40a4-390">**Apache HBase on HDInsight**</span></span>

* [<span data-ttu-id="f40a4-391">Bevezetés a hbase a HDInsight eszközzel</span><span class="sxs-lookup"><span data-stu-id="f40a4-391">Getting started with HBase on HDInsight</span></span>](hdinsight-hbase-tutorial-get-started.md)
