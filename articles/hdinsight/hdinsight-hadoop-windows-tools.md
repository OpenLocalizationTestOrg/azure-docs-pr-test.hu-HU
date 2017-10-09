---
title: "a HDInsight - Azure hadooppal Windows rendszerű aaaUse |} Microsoft Docs"
description: "HDInsight Hadoop Windows PC működnek. Kezelése és lekérdezés fürtök PowerShell, a Visual Studio és a Linux-eszközökkel. A .NET big data-megoldások fejlesztése."
services: hdinsight
keywords: windows, a hadooppal Windows hadoop
author: cjgronlund
manager: jhubbard
ms.author: cgronlun
ms.date: 05/17/2017
ms.topic: article
ms.service: hdinsight
ms.custom: hdinsightactive,hdiseo17may2017
ms.openlocfilehash: 7c93f16e93349c0b8fb1abd55320c2c172b93aa9
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="work-in-hello-hadoop-ecosystem-on-hdinsight-from-a-windows-pc"></a>A Windows-számítógép HDInsight Hadoop ökoszisztémájának hello működik

További tudnivalók a fejlesztői és felügyeleti beállítások hello Windows rendszerű számítógépek a HDInsight Hadoop ökoszisztémájának hello használatához. 

HDInsight az Apache Hadoop és a Hadoop-összetevők, nyílt forráskódú technológiák fejlesztett Linux rendszeren alapul. HDInsight 3.4 vagy magasabb verziójú hello Ubuntu Linux terjesztési hello alapjául szolgáló operációs rendszer hello fürt használja. Azonban dolgozhat HDInsight egy Windows ügyfél vagy a Windows fejlesztői környezetre.

## <a name="use-powershell-for-deployment-and-management-tasks"></a>Használja a Powershellt üzembe helyezési és kezelési feladatok
Az Azure PowerShell egy parancsfájl-kezelési környezet, hogy toocontrol használ, és a HDInsight a Windows központi telepítési és felügyeleti feladatok automatizálására.

A PowerShell használatával elvégezhető feladatok példái:

* [PowerShell-fürtök létrehozása](hdinsight-hadoop-create-linux-clusters-azure-powershell.md)
* [PowerShell-lel Hive-lekérdezések futtatása](hdinsight-hadoop-use-hive-powershell.md)
* [A PowerShell-lel fürtök kezelése](hdinsight-administer-use-powershell.md)

Kövesse a lépéseket túl[Azure Powershell telepítése és konfigurálása](https://docs.microsoft.com/powershell/azure/install-azurerm-ps) tooget hello legújabb verziójára. Ha vannak olyan parancsprogramjai, amelyeket módosítani toobe toouse hello parancsmagjai az Azure Resource Manager, lásd: [tooAzure Resource Manager-alapú fejlesztési eszközöket a HDInsight-fürtök áttelepítése](hdinsight-hadoop-development-using-azure-resource-manager.md).

## <a name="utilities-you-can-run-in-a-browser"></a>Futtatása böngészőben segédprogramok
hello alábbi segédprogramokat rendelkezik egy webes felhasználói felület, amelyen a böngészőben:
* **[Azure Cloud rendszerhéj (előzetes verzió)](https://docs.microsoft.com/azure/cloud-shell/quickstart)**  egy interaktív, parancssori rendszerhéj a böngészőben futó és belül hello Azure-portálon.
* **[Ambari webes felhasználói felületén](hdinsight-hadoop-manage-ambari.md)**  van egy kezelési és figyelési segédprogram használható hello Azure-portál, amely használt toomanage különböző feladatok, például:
    * [Ambari hello REST API-t használja](hdinsight-hadoop-manage-ambari-rest-api.md)
    * [Az Ambari Hive nézete](hdinsight-hadoop-use-hive-ambari-view.md)
    * [Az Ambari Tez megtekintése](hdinsight-debug-ambari-tez-view.md)

## <a name="data-lake-hadoop-tools-for-visual-studio"></a>(Hadoop) a Data Lake Tools for Visual Studio
A Data Lake Tools használja a Visual Studio toodeploy, és kezelheti a Storm-topológiák. A Data Lake Tools hello SCP.NET SDK-t, amely lehetővé teszi a Visual Studio toodevelop C# Storm-topológiák is telepíti.

Mielőtt továbblép a következő példákban toohello [telepítése, és próbálja meg a Data Lake Tools for Visual Studio](hdinsight-hadoop-visual-studio-tools-get-started.md). 

A Visual Studio és a Data Lake Tools for Visual Studio elvégezhető feladatok példái:
* [Telepítését és kezelését a Storm-topológiák a Visual Studio eszközből](hdinsight-storm-deploy-monitor-topology-linux.md)
* [C#-topológiák fejlesztése a Visual Studio használatával alatt futó Storm](hdinsight-storm-develop-csharp-visual-studio-topology.md). hello bits közé tartoznak például sablonok a Storm-topológiák toodatabases, például az Azure Cosmos adatbázis és az SQL-adatbázis is elérheti.

## <a name="visual-studio-and-hello-net-sdk"></a>A Visual Studio és hello .NET SDK-val 

Visual Studio használata hello .NET SDK toomanage fürtök és a big data-alkalmazások fejlesztéséhez. A következő feladatok hello más IDEs használható, de példák a Visual Studio.

A Visual Studio .NET SDK hello elvégezhető feladatok példái:
* [Fürtök létrehozása és a HDInsight .NET-keretrendszer alkalmazás használata](hdinsight-hadoop-create-linux-clusters-dotnet-sdk.md)
* [Hello .NET SDK használatával Hive-lekérdezések futtatása](hdinsight-hadoop-use-hive-dotnet-sdk.md)
* [A Hive és a Hadoop streamelési Pig C# felhasználó által definiált függvények használata](hdinsight-hadoop-hive-pig-udf-dotnet-csharp.md)

> Tipp .NET megoldások a Windows-alapú HDInsight-fürtök használ, akkor egy időben tooplan áttelepítés tooLinux-alapú fürtökön. További információkért lásd: [át .NET Windows-alapú HDInsight-megoldást tooLinux-alapú HDInsight](hdinsight-hadoop-migrate-dotnet-to-linux.md).

## <a name="intellij-idea-and-eclipse-ide-for-spark-clusters"></a>Intellij IDEA és Eclipse IDE Spark-fürtök
Mindkét [Intellij IDEA](https://www.jetbrains.com/idea/download) és hello [Eclipse IDE](https://www.eclipse.org/downloads/) a következőkre használható:
* Fejleszthet, és küldje el a Scala Spark alkalmazás egy HDInsight Spark-fürtön.
* A Spark-fürt erőforrások eléréséhez.
* Fejleszthet, és egy Scala Spark alkalmazás helyileg történő futtatása.

Ezek a cikkek megjelenítése módját: 
* Intellij IDEA: [Create Spark-alkalmazások hello Azure eszköztára Intellij beépülő modul és hello Scala SDK használatával.](hdinsight-apache-spark-intellij-tool-plugin.md)
* IDE vagy az Eclipse Scala IDE Holdas: [létrehozása Spark-alkalmazások és hello Eclipse Azure eszköztára](hdinsight-apache-spark-eclipse-tool-plugin.md) 


## <a name="notebooks-on-spark-for-data-scientists"></a>A Spark on az adatszakértőkön notebookok 
Az Apache Spark on hdinsight fürtök Zeppelin notebookok és a Jupyter notebookok használható kernelt. 

* [Ismerje meg, hogyan toouse kernelek a Spark-fürtök a Jupyter notebookok tootest Spark-alkalmazások](hdinsight-apache-spark-zeppelin-notebook.md)
* [Ismerje meg, hogyan toouse Zeppelin notebookok a Spark-fürtök toorun Spark feladatok](hdinsight-apache-spark-jupyter-notebook-kernels.md) 


## <a name="run-linux-based-tools-and-technologies-on-windows"></a>Futtassa a Linux-alapú eszközök és technológiák a Windows

Ha olyan helyzet, amikor egy eszköz vagy technológia, amely csak akkor érhető el a Linux használni kell, vegye figyelembe az alábbi beállítások hello:

* **A Windows 10 (béta) bash** Ez a témakör a Linux alrendszer Windows. Bash lehetővé teszi a toodirectly Linux segédprogramok anélkül, hogy toomaintain egy dedikált Linux rendszerhez – telepítés futtatása. [Telepítse, hello Bash beta futtassa a Windows 10 rendszeren](https://msdn.microsoft.com/commandline/wsl/install_guide)
* **A Windows docker** toomany Linux-alapú eszközök hozzáférést biztosít, és a Windows-ről futtatható. Például a Hive közvetlenül a Windows Docker toorun hello Beeline ügyfél is használhatja. Docker toorun helyi Jupyter notebook is használhatja, és távolról kapcsolódhatnak a hdinsight tooSpark. [Ismerkedés a Windowshoz készült Docker](https://docs.docker.com/docker-for-windows/)
* **[MobaXTerm](http://mobaxterm.mobatek.net/)**  lehetővé teszi egy SSH-kapcsolaton keresztül toographically Tallózás hello fürt fájlrendszer.

## <a name="next-steps"></a>Következő lépések
Ha a Linux-alapú fürtök új tooworking, tekintse meg a cikkek kövesse hello:
* [Hadoop, Kafka, Spark vagy más fürtök beállítása](hdinsight-hadoop-provision-linux-clusters.md)
* [Tippek a HDInsight-fürtök Linux rendszeren](hdinsight-hadoop-linux-information.md)