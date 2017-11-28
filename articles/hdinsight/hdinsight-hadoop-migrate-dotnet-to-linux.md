---
title: "a Hadoop-MapReduce a Linux-alapú HDInsight - Azure .NET aaaUse |} Microsoft Docs"
description: "Megtudhatja, hogyan toouse .NET-alkalmazások a Linux-alapú HDInsight MapReduce streaming."
services: hdinsight
documentationCenter: 
author: Blackmist
manager: jhubbard
editor: cgronlun
tags: azure-portal
ms.assetid: 
ms.service: hdinsight
ms.custom: hdinsightactive
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 07/12/2017
ms.author: larryfr
ms.openlocfilehash: 5a4e6dc1b4dafa8cc40780e3371fa4b8ba3e3d48
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="migrate-net-solutions-for-windows-based-hdinsight-toolinux-based-hdinsight"></a><span data-ttu-id="18928-103">Telepítse át a .NET-megoldások a Windows-alapú HDInsight tooLinux-alapú HDInsight</span><span class="sxs-lookup"><span data-stu-id="18928-103">Migrate .NET solutions for Windows-based HDInsight tooLinux-based HDInsight</span></span>

<span data-ttu-id="18928-104">Linux-alapú HDInsight-fürtök használata [monó (https://mono-project.com)](https://mono-project.com) toorun .NET-alkalmazások.</span><span class="sxs-lookup"><span data-stu-id="18928-104">Linux-based HDInsight clusters use [Mono (https://mono-project.com)](https://mono-project.com) toorun .NET applications.</span></span> <span data-ttu-id="18928-105">Monó toouse .NET-összetevők, például a Linux-alapú hdinsight eszközzel MapReduce-alkalmazások lehetővé teszi.</span><span class="sxs-lookup"><span data-stu-id="18928-105">Mono allows you toouse .NET components such as MapReduce applications with Linux-based HDInsight.</span></span> <span data-ttu-id="18928-106">Ebből a dokumentumból megtudhatja, hogyan toomigrate .NET megoldások monó a Linux-alapú HDInsight a Windows-alapú HDInsight-fürtök toowork hozták létre.</span><span class="sxs-lookup"><span data-stu-id="18928-106">In this document, learn how toomigrate .NET solutions created for Windows-based HDInsight clusters toowork with Mono on Linux-based HDInsight.</span></span>

## <a name="mono-compatibility-with-net"></a><span data-ttu-id="18928-107">A .NET monó kompatibilitási</span><span class="sxs-lookup"><span data-stu-id="18928-107">Mono compatibility with .NET</span></span>

<span data-ttu-id="18928-108">Monó verzió 4.2.1 megtalálható HDInsight 3.5-ös verziója.</span><span class="sxs-lookup"><span data-stu-id="18928-108">Mono version 4.2.1 is included with HDInsight version 3.5.</span></span> <span data-ttu-id="18928-109">Monó részét képező HDInsight hello verzióján további információkért lásd: [HDInsight összetevő verziók](hdinsight-component-versioning.md).</span><span class="sxs-lookup"><span data-stu-id="18928-109">For more information on hello version of Mono included with HDInsight, see [HDInsight component versions](hdinsight-component-versioning.md).</span></span> <span data-ttu-id="18928-110">Monó, egy adott verziójához tooinstall lásd: hello [telepítés vagy frissítés monó](hdinsight-hadoop-install-mono.md) dokumentum.</span><span class="sxs-lookup"><span data-stu-id="18928-110">tooinstall a specific version of Mono, see hello [Install or update Mono](hdinsight-hadoop-install-mono.md) document.</span></span>

<span data-ttu-id="18928-111">Monó és a .NET között részletes információkért lásd: hello [monó kompatibilitási (http://www.mono-project.com/docs/about-mono/compatibility/)](http://www.mono-project.com/docs/about-mono/compatibility/) dokumentum.</span><span class="sxs-lookup"><span data-stu-id="18928-111">For detailed information on compatibility between Mono and .NET, see hello [Mono compatibility (http://www.mono-project.com/docs/about-mono/compatibility/)](http://www.mono-project.com/docs/about-mono/compatibility/) document.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="18928-112">Monó hello SCP.NET keretrendszer esetén.</span><span class="sxs-lookup"><span data-stu-id="18928-112">hello SCP.NET framework is compatible with Mono.</span></span> <span data-ttu-id="18928-113">További tájékoztatást SCP.NET Monó, lásd: [használja a Visual Studio toodevelop C#-topológiák HDInsight alatt futó Apache Storm](hdinsight-storm-develop-csharp-visual-studio-topology.md).</span><span class="sxs-lookup"><span data-stu-id="18928-113">For more information on using SCP.NET with Mono, see [Use Visual Studio toodevelop C# topologies for Apache Storm on HDInsight](hdinsight-storm-develop-csharp-visual-studio-topology.md).</span></span>

## <a name="automated-portability-analysis"></a><span data-ttu-id="18928-114">Automatizált hordozhatóság elemzés</span><span class="sxs-lookup"><span data-stu-id="18928-114">Automated portability analysis</span></span>

<span data-ttu-id="18928-115">Hello [.NET hordozhatóság Analyzer](https://marketplace.visualstudio.com/items?itemName=ConnieYau.NETPortabilityAnalyzer) lehet használt toogenerate inkompatibilitási problémák, az alkalmazás és az monó között jelentést.</span><span class="sxs-lookup"><span data-stu-id="18928-115">hello [.NET Portability Analyzer](https://marketplace.visualstudio.com/items?itemName=ConnieYau.NETPortabilityAnalyzer) can be used toogenerate a report of incompatibilities between your application and Mono.</span></span> <span data-ttu-id="18928-116">Használja a következő lépéseket tooconfigure hello analyzer toocheck hello az alkalmazás monó hordozhatóság:</span><span class="sxs-lookup"><span data-stu-id="18928-116">Use hello following steps tooconfigure hello analyzer toocheck your application for Mono portability:</span></span>

1. <span data-ttu-id="18928-117">Telepítse a hello [.NET hordozhatóság Analyzer](https://marketplace.visualstudio.com/items?itemName=ConnieYau.NETPortabilityAnalyzer).</span><span class="sxs-lookup"><span data-stu-id="18928-117">Install hello [.NET Portability Analyzer](https://marketplace.visualstudio.com/items?itemName=ConnieYau.NETPortabilityAnalyzer).</span></span> <span data-ttu-id="18928-118">A telepítés során válassza ki a Visual Studio toouse hello verzióját.</span><span class="sxs-lookup"><span data-stu-id="18928-118">During installation, select hello version of Visual Studio toouse.</span></span>

2. <span data-ttu-id="18928-119">Visual Studio 2015-öt, válassza ki __elemzés__ > __Hordozhatósága Analyzer beállítások__, és győződjön meg arról, hogy __4.5__ hello beadása __monó__  szakasz.</span><span class="sxs-lookup"><span data-stu-id="18928-119">From Visual Studio 2015, select __Analyze__ > __Portability Analyzer Settings__, and make sure that __4.5__ is checked in hello __Mono__ section.</span></span>

    ![4.5 monó részében hello analyzer beállítások be van jelölve](./media/hdinsight-hadoop-migrate-dotnet-to-linux/portability-analyzer-settings.png)

    <span data-ttu-id="18928-121">Válassza ki __OK__ toosave hello konfigurációs.</span><span class="sxs-lookup"><span data-stu-id="18928-121">Select __OK__ toosave hello configuration.</span></span>

3. <span data-ttu-id="18928-122">Válassza ki __elemzése__ > __szerelvény hordozhatóság elemzése__.</span><span class="sxs-lookup"><span data-stu-id="18928-122">Select __Analyze__ > __Analyze Assembly Portability__.</span></span> <span data-ttu-id="18928-123">A megoldás tartalmazó hello szerelvény, majd válassza ki és __nyitott__ toobegin elemzés.</span><span class="sxs-lookup"><span data-stu-id="18928-123">Select hello assembly that contains your solution, and then select __Open__ toobegin analysis.</span></span>

4. <span data-ttu-id="18928-124">Elemzés végrehajtása után válassza ki a __elemzés__ > __elemzési jelentések megtekintése__.</span><span class="sxs-lookup"><span data-stu-id="18928-124">Once analysis is complete, select __Analyze__ > __View analysis reports__.</span></span> <span data-ttu-id="18928-125">A __Hordozhatósága az elemzés eredményeinek__, jelölje be __nyissa meg a jelentés__ tooopen jelentést.</span><span class="sxs-lookup"><span data-stu-id="18928-125">In __Portability Analysis Results__, select __Open report__ tooopen a report.</span></span>

    ![Hordozhatóság analyzer eredmények párbeszédpanel](./media/hdinsight-hadoop-migrate-dotnet-to-linux/portability-analyzer-results.png)

> [!IMPORTANT]
> <span data-ttu-id="18928-127">hello elemző eszköz nem dolgozza fel a megoldással minden probléma.</span><span class="sxs-lookup"><span data-stu-id="18928-127">hello analyzer cannot catch every problem with your solution.</span></span> <span data-ttu-id="18928-128">Például egy fájl elérési útja `c:\temp\file.txt` OK magas, mert a Windows rendszeren futtatott monó és hello elérési út érvényes, ebben a környezetben.</span><span class="sxs-lookup"><span data-stu-id="18928-128">For example, a file path of `c:\temp\file.txt` is considered OK because Mono runs on Windows and hello path is valid in that context.</span></span> <span data-ttu-id="18928-129">Azonban hello elérési út nem egy Linux platformon érvényes.</span><span class="sxs-lookup"><span data-stu-id="18928-129">However, hello path is not valid on a Linux platform.</span></span>

## <a name="manual-portability-analysis"></a><span data-ttu-id="18928-130">Manuális hordozhatóság elemzés</span><span class="sxs-lookup"><span data-stu-id="18928-130">Manual portability analysis</span></span>

<span data-ttu-id="18928-131">Hajtsa végre a kód használatával hello információi hello manuális naplózási [alkalmazások hordozhatóságát (http://www.mono-project.com/docs/getting-started/application-portability/)](http://www.mono-project.com/docs/getting-started/application-portability/) dokumentum.</span><span class="sxs-lookup"><span data-stu-id="18928-131">Perform a manual audit of your code using hello information in hello [Application Portability (http://www.mono-project.com/docs/getting-started/application-portability/)](http://www.mono-project.com/docs/getting-started/application-portability/) document.</span></span>

## <a name="modify-and-build"></a><span data-ttu-id="18928-132">Módosítsa és létrehozása</span><span class="sxs-lookup"><span data-stu-id="18928-132">Modify and build</span></span>

<span data-ttu-id="18928-133">Folytatás toouse Visual Studio toobuild a .NET-megoldások a HDInsight.</span><span class="sxs-lookup"><span data-stu-id="18928-133">You can continue toouse Visual Studio toobuild your .NET solutions for HDInsight.</span></span> <span data-ttu-id="18928-134">Azonban meg kell győződnie arról, hogy hello projekt konfigurált toouse .NET-keretrendszer 4.5.</span><span class="sxs-lookup"><span data-stu-id="18928-134">However, you must ensure that hello project is configured toouse .NET Framework 4.5.</span></span>

## <a name="deploy-and-test"></a><span data-ttu-id="18928-135">Telepítse és tesztelje</span><span class="sxs-lookup"><span data-stu-id="18928-135">Deploy and test</span></span>

<span data-ttu-id="18928-136">Ha módosította a hello javaslatok hello .NET hordozhatóság Analyzer vagy egy manuális elemzés használó megoldás, a hdinsight eszközzel kell tesztelni.</span><span class="sxs-lookup"><span data-stu-id="18928-136">Once you have modified your solution using hello recommendations from hello .NET Portability Analyzer or from a manual analysis, you must test it with HDInsight.</span></span> <span data-ttu-id="18928-137">A Linux-alapú HDInsight-fürt hello megoldás tesztelési felfedheti finom javítani toobe igénylő problémák.</span><span class="sxs-lookup"><span data-stu-id="18928-137">Testing hello solution on a Linux-based HDInsight cluster may reveal subtle problems that need toobe corrected.</span></span> <span data-ttu-id="18928-138">Azt javasoljuk, hogy további naplózás engedélyezése az alkalmazás azt tesztelése során.</span><span class="sxs-lookup"><span data-stu-id="18928-138">We recommend that you enable additional logging in your application while testing it.</span></span>

<span data-ttu-id="18928-139">A naplók elérése további információkért lásd: a következő dokumentumok hello:</span><span class="sxs-lookup"><span data-stu-id="18928-139">For more information on accessing logs, see hello following documents:</span></span>

* [<span data-ttu-id="18928-140">YARN-alkalmazásnaplók elérése Linux-alapú HDInsighton</span><span class="sxs-lookup"><span data-stu-id="18928-140">Access YARN application logs on Linux-based HDInsight</span></span>](hdinsight-hadoop-access-yarn-app-logs-linux.md)

## <a name="next-steps"></a><span data-ttu-id="18928-141">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="18928-141">Next steps</span></span>

* [<span data-ttu-id="18928-142">C# használata a HDInsight MapReduce</span><span class="sxs-lookup"><span data-stu-id="18928-142">Use C# with MapReduce on HDInsight</span></span>](hdinsight-hadoop-dotnet-csharp-mapreduce-streaming.md)

* [<span data-ttu-id="18928-143">C# felhasználó által megadott függvények használata a Hive és a Pig használatával</span><span class="sxs-lookup"><span data-stu-id="18928-143">Use C# user defined functions with Hive and Pig</span></span>](hdinsight-hadoop-hive-pig-udf-dotnet-csharp.md)

* [<span data-ttu-id="18928-144">A HDInsight alatt futó Storm a C#-topológiák fejlesztése</span><span class="sxs-lookup"><span data-stu-id="18928-144">Develop C# topologies for Storm on HDInsight</span></span>](hdinsight-storm-develop-csharp-visual-studio-topology.md)