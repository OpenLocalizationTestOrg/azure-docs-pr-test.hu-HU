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
# <a name="migrate-net-solutions-for-windows-based-hdinsight-toolinux-based-hdinsight"></a>Telepítse át a .NET-megoldások a Windows-alapú HDInsight tooLinux-alapú HDInsight

Linux-alapú HDInsight-fürtök használata [monó (https://mono-project.com)](https://mono-project.com) toorun .NET-alkalmazások. Monó toouse .NET-összetevők, például a Linux-alapú hdinsight eszközzel MapReduce-alkalmazások lehetővé teszi. Ebből a dokumentumból megtudhatja, hogyan toomigrate .NET megoldások monó a Linux-alapú HDInsight a Windows-alapú HDInsight-fürtök toowork hozták létre.

## <a name="mono-compatibility-with-net"></a>A .NET monó kompatibilitási

Monó verzió 4.2.1 megtalálható HDInsight 3.5-ös verziója. Monó részét képező HDInsight hello verzióján további információkért lásd: [HDInsight összetevő verziók](hdinsight-component-versioning.md). Monó, egy adott verziójához tooinstall lásd: hello [telepítés vagy frissítés monó](hdinsight-hadoop-install-mono.md) dokumentum.

Monó és a .NET között részletes információkért lásd: hello [monó kompatibilitási (http://www.mono-project.com/docs/about-mono/compatibility/)](http://www.mono-project.com/docs/about-mono/compatibility/) dokumentum.

> [!IMPORTANT]
> Monó hello SCP.NET keretrendszer esetén. További tájékoztatást SCP.NET Monó, lásd: [használja a Visual Studio toodevelop C#-topológiák HDInsight alatt futó Apache Storm](hdinsight-storm-develop-csharp-visual-studio-topology.md).

## <a name="automated-portability-analysis"></a>Automatizált hordozhatóság elemzés

Hello [.NET hordozhatóság Analyzer](https://marketplace.visualstudio.com/items?itemName=ConnieYau.NETPortabilityAnalyzer) lehet használt toogenerate inkompatibilitási problémák, az alkalmazás és az monó között jelentést. Használja a következő lépéseket tooconfigure hello analyzer toocheck hello az alkalmazás monó hordozhatóság:

1. Telepítse a hello [.NET hordozhatóság Analyzer](https://marketplace.visualstudio.com/items?itemName=ConnieYau.NETPortabilityAnalyzer). A telepítés során válassza ki a Visual Studio toouse hello verzióját.

2. Visual Studio 2015-öt, válassza ki __elemzés__ > __Hordozhatósága Analyzer beállítások__, és győződjön meg arról, hogy __4.5__ hello beadása __monó__  szakasz.

    ![4.5 monó részében hello analyzer beállítások be van jelölve](./media/hdinsight-hadoop-migrate-dotnet-to-linux/portability-analyzer-settings.png)

    Válassza ki __OK__ toosave hello konfigurációs.

3. Válassza ki __elemzése__ > __szerelvény hordozhatóság elemzése__. A megoldás tartalmazó hello szerelvény, majd válassza ki és __nyitott__ toobegin elemzés.

4. Elemzés végrehajtása után válassza ki a __elemzés__ > __elemzési jelentések megtekintése__. A __Hordozhatósága az elemzés eredményeinek__, jelölje be __nyissa meg a jelentés__ tooopen jelentést.

    ![Hordozhatóság analyzer eredmények párbeszédpanel](./media/hdinsight-hadoop-migrate-dotnet-to-linux/portability-analyzer-results.png)

> [!IMPORTANT]
> hello elemző eszköz nem dolgozza fel a megoldással minden probléma. Például egy fájl elérési útja `c:\temp\file.txt` OK magas, mert a Windows rendszeren futtatott monó és hello elérési út érvényes, ebben a környezetben. Azonban hello elérési út nem egy Linux platformon érvényes.

## <a name="manual-portability-analysis"></a>Manuális hordozhatóság elemzés

Hajtsa végre a kód használatával hello információi hello manuális naplózási [alkalmazások hordozhatóságát (http://www.mono-project.com/docs/getting-started/application-portability/)](http://www.mono-project.com/docs/getting-started/application-portability/) dokumentum.

## <a name="modify-and-build"></a>Módosítsa és létrehozása

Folytatás toouse Visual Studio toobuild a .NET-megoldások a HDInsight. Azonban meg kell győződnie arról, hogy hello projekt konfigurált toouse .NET-keretrendszer 4.5.

## <a name="deploy-and-test"></a>Telepítse és tesztelje

Ha módosította a hello javaslatok hello .NET hordozhatóság Analyzer vagy egy manuális elemzés használó megoldás, a hdinsight eszközzel kell tesztelni. A Linux-alapú HDInsight-fürt hello megoldás tesztelési felfedheti finom javítani toobe igénylő problémák. Azt javasoljuk, hogy további naplózás engedélyezése az alkalmazás azt tesztelése során.

A naplók elérése további információkért lásd: a következő dokumentumok hello:

* [YARN-alkalmazásnaplók elérése Linux-alapú HDInsighton](hdinsight-hadoop-access-yarn-app-logs-linux.md)

## <a name="next-steps"></a>Következő lépések

* [C# használata a HDInsight MapReduce](hdinsight-hadoop-dotnet-csharp-mapreduce-streaming.md)

* [C# felhasználó által megadott függvények használata a Hive és a Pig használatával](hdinsight-hadoop-hive-pig-udf-dotnet-csharp.md)

* [A HDInsight alatt futó Storm a C#-topológiák fejlesztése](hdinsight-storm-develop-csharp-visual-studio-topology.md)