---
title: "Parancsfájl-művelet fejlesztése a HDInsight - Azure |} Microsoft Docs"
description: "Ismerje meg a parancsfájlművelet Hadoop-fürtök testreszabása. Parancsfájl művelet fut a Hadoop-fürthöz további szoftvereket telepíteni, vagy módosítsa a fürt telepített alkalmazások használható."
services: hdinsight
documentationcenter: 
tags: azure-portal
author: mumian
manager: jhubbard
editor: cgronlun
ms.assetid: 836d68a8-8b21-4d69-8b61-281a7fe67f21
ms.service: hdinsight
ms.workload: big-data
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/25/2017
ms.author: jgao
ROBOTS: NOINDEX
ms.openlocfilehash: 0e182e6b43fd2d17524c1da36cf4c204bb1b865a
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/03/2017
---
# <a name="develop-script-action-scripts-for-hdinsight-windows-based-clusters"></a><span data-ttu-id="5556b-104">A HDInsight-Windows-alapú fürtök parancsfájlművelet-parancsfájlok fejlesztése</span><span class="sxs-lookup"><span data-stu-id="5556b-104">Develop Script Action scripts for HDInsight Windows-based clusters</span></span>
<span data-ttu-id="5556b-105">A HDInsight parancsfájlművelet parancsfájlok írásának ismertetése.</span><span class="sxs-lookup"><span data-stu-id="5556b-105">Learn how to write Script Action scripts for HDInsight.</span></span> <span data-ttu-id="5556b-106">Parancsfájlművelet-parancsfájlok használatával kapcsolatos információkért lásd: [testreszabása HDInsight-fürtök használata parancsfájlművelet](hdinsight-hadoop-customize-cluster.md).</span><span class="sxs-lookup"><span data-stu-id="5556b-106">For information on using Script Action scripts, see [Customize HDInsight clusters using Script Action](hdinsight-hadoop-customize-cluster.md).</span></span> <span data-ttu-id="5556b-107">A Linux-alapú HDInsight-fürtök írt ugyanazon cikk, lásd: [parancsfájlművelet fejlesztése parancsfájlok a HDInsight](hdinsight-hadoop-script-actions-linux.md).</span><span class="sxs-lookup"><span data-stu-id="5556b-107">For the same article written for Linux-based HDInsight clusters, see [Develop Script Action scripts for HDInsight](hdinsight-hadoop-script-actions-linux.md).</span></span>



> [!IMPORTANT]
> <span data-ttu-id="5556b-108">Az ebben a dokumentumban csak a lépések Windows-alapú HDInsight-fürtök.</span><span class="sxs-lookup"><span data-stu-id="5556b-108">The steps in this document only work for Windows-based HDInsight clusters.</span></span> <span data-ttu-id="5556b-109">HDInsight csak érhető el a Windows korábbi, mint a HDInsight 3.4-es verziójához.</span><span class="sxs-lookup"><span data-stu-id="5556b-109">HDInsight is only available on Windows for versions lower than HDInsight 3.4.</span></span> <span data-ttu-id="5556b-110">A Linux az egyetlen operációs rendszer, amely a HDInsight 3.4-es vagy újabb verziói esetében használható.</span><span class="sxs-lookup"><span data-stu-id="5556b-110">Linux is the only operating system used on HDInsight version 3.4 or greater.</span></span> <span data-ttu-id="5556b-111">További tudnivalókért lásd: [A HDInsight elavulása Windows rendszeren](hdinsight-component-versioning.md#hdinsight-windows-retirement).</span><span class="sxs-lookup"><span data-stu-id="5556b-111">For more information, see [HDInsight retirement on Windows](hdinsight-component-versioning.md#hdinsight-windows-retirement).</span></span> <span data-ttu-id="5556b-112">A Parancsfájlműveletek használata a Linux-alapú fürtökön információkért lásd: [parancsfájl-művelet fejlesztése a HDInsight (Linux)](hdinsight-hadoop-script-actions-linux.md).</span><span class="sxs-lookup"><span data-stu-id="5556b-112">For information on using script actions with Linux-based clusters, see [Script action development with HDInsight (Linux)](hdinsight-hadoop-script-actions-linux.md).</span></span>
>
>



<span data-ttu-id="5556b-113">Parancsfájl művelet fut a Hadoop-fürthöz további szoftvereket telepíteni, vagy módosítsa a fürt telepített alkalmazások használható.</span><span class="sxs-lookup"><span data-stu-id="5556b-113">Script Action can be used to install additional software running on a Hadoop cluster or to change the configuration of applications installed on a cluster.</span></span> <span data-ttu-id="5556b-114">A Parancsfájlműveletek olyan parancsfájlok, futtassa a fürtcsomópontokon, a HDInsight-fürtök telepítésekor, és a fürt csomópontjai HDInsight konfigurálásának befejezése után végrehajtás.</span><span class="sxs-lookup"><span data-stu-id="5556b-114">Script actions are scripts that run on the cluster nodes when HDInsight clusters are deployed, and they are executed once nodes in the cluster complete HDInsight configuration.</span></span> <span data-ttu-id="5556b-115">A parancsfájlművelet hajtja végre a fiók rendszergazdai jogosultságokkal, és teljes hozzáférési jogosultsága ahhoz, hogy a fürt csomópontjai biztosít.</span><span class="sxs-lookup"><span data-stu-id="5556b-115">A script action is executed under system admin account privileges and provides full access rights to the cluster nodes.</span></span> <span data-ttu-id="5556b-116">Az egyes fürtökön megadható Parancsfájlműveletek hajthatnak végre, amely a megadott sorrendben listáját.</span><span class="sxs-lookup"><span data-stu-id="5556b-116">Each cluster can be provided with a list of script actions to be executed in the order in which they are specified.</span></span>

> [!NOTE]
> <span data-ttu-id="5556b-117">Ha a következő hibaüzenet:</span><span class="sxs-lookup"><span data-stu-id="5556b-117">If you experience the following error message:</span></span>
>
> <span data-ttu-id="5556b-118">System.Management.Automation.CommandNotFoundException; ExceptionMessage: A kifejezés "Save-HDIFile" nem ismerhető fel egy parancsmag, a függvény, a parancsfájl vagy a futtatható program nevét.</span><span class="sxs-lookup"><span data-stu-id="5556b-118">System.Management.Automation.CommandNotFoundException; ExceptionMessage : The term 'Save-HDIFile' is not recognized as the name of a cmdlet, function, script file, or operable program.</span></span> <span data-ttu-id="5556b-119">Ellenőrizze a helyesírást, a név, vagy ha egy elérési út része, ellenőrizze, hogy az elérési út helyességét, és próbálkozzon újra.</span><span class="sxs-lookup"><span data-stu-id="5556b-119">Check the spelling of the name, or if a path was included, verify that the path is correct and try again.</span></span>
> <span data-ttu-id="5556b-120">Mivel az nem tartozik a segédmódszereket van.</span><span class="sxs-lookup"><span data-stu-id="5556b-120">It is because you didn't include the helper methods.</span></span>  <span data-ttu-id="5556b-121">Lásd: [segédmódszereket egyéni parancsfájlok](hdinsight-hadoop-script-actions.md#helper-methods-for-custom-scripts).</span><span class="sxs-lookup"><span data-stu-id="5556b-121">See [Helper methods for custom scripts](hdinsight-hadoop-script-actions.md#helper-methods-for-custom-scripts).</span></span>
>
>

## <a name="sample-scripts"></a><span data-ttu-id="5556b-122">Mintaszkriptek</span><span class="sxs-lookup"><span data-stu-id="5556b-122">Sample scripts</span></span>
<span data-ttu-id="5556b-123">A HDInsight-fürtök létrehozása a Windows operációs rendszeren, a parancsfájl művelete Azure PowerShell-parancsfájlt.</span><span class="sxs-lookup"><span data-stu-id="5556b-123">For creating HDInsight clusters on Windows operating system, the Script Action is Azure PowerShell script.</span></span> <span data-ttu-id="5556b-124">A következő parancsfájlt a hely konfigurációs fájljainak konfigurálásához végrehajtott minta:</span><span class="sxs-lookup"><span data-stu-id="5556b-124">The following script is a sample for configuring the site configuration files:</span></span>

[!INCLUDE [upgrade-powershell](../../includes/hdinsight-use-latest-powershell.md)]

    param (
        [parameter(Mandatory)][string] $ConfigFileName,
        [parameter(Mandatory)][string] $Name,
        [parameter(Mandatory)][string] $Value,
        [parameter()][string] $Description
    )

    if (!$Description) {
        $Description = ""
    }

    $hdiConfigFiles = @{
        "hive-site.xml" = "$env:HIVE_HOME\conf\hive-site.xml";
        "core-site.xml" = "$env:HADOOP_HOME\etc\hadoop\core-site.xml";
        "hdfs-site.xml" = "$env:HADOOP_HOME\etc\hadoop\hdfs-site.xml";
        "mapred-site.xml" = "$env:HADOOP_HOME\etc\hadoop\mapred-site.xml";
        "yarn-site.xml" = "$env:HADOOP_HOME\etc\hadoop\yarn-site.xml"
    }

    if (!($hdiConfigFiles[$ConfigFileName])) {
        Write-HDILog "Unable to configure $ConfigFileName because it is not part of the HDI configuration files."
        return
    }

    [xml]$configFile = Get-Content $hdiConfigFiles[$ConfigFileName]

    $existingproperty = $configFile.configuration.property | where {$_.Name -eq $Name}

    if ($existingproperty) {
        $existingproperty.Value = $Value
        $existingproperty.Description = $Description
    } else {
        $newproperty = @($configFile.configuration.property)[0].Clone()
        $newproperty.Name = $Name
        $newproperty.Value = $Value
        $newproperty.Description = $Description
        $configFile.configuration.AppendChild($newproperty)
    }

    $configFile.Save($hdiConfigFiles[$ConfigFileName])

    Write-HDILog "$configFileName has been configured."

<span data-ttu-id="5556b-125">A parancsfájl fogadja el a négy paraméter, a konfigurációs fájl nevét, a tulajdonságot kívánja módosítani, az értéket be szeretné állítani, és egy leírást.</span><span class="sxs-lookup"><span data-stu-id="5556b-125">The script takes four parameters, the configuration file name, the property you want to modify, the value you want to set, and a description.</span></span> <span data-ttu-id="5556b-126">Példa:</span><span class="sxs-lookup"><span data-stu-id="5556b-126">For example:</span></span>

    hive-site.xml hive.metastore.client.socket.timeout 90

<span data-ttu-id="5556b-127">Ezeket a paramétereket állítja be a hive.metastore.client.socket.timeout érték 90 a hive-site.xml fájlban.</span><span class="sxs-lookup"><span data-stu-id="5556b-127">These parameters sets the hive.metastore.client.socket.timeout value to 90 in the hive-site.xml file.</span></span>  <span data-ttu-id="5556b-128">Az alapértelmezett értéke 60 másodperc.</span><span class="sxs-lookup"><span data-stu-id="5556b-128">The default value is 60 seconds.</span></span>

<span data-ttu-id="5556b-129">A parancsfájlpéldát is találhatók [https://hditutorialdata.blob.core.windows.net/customizecluster/editSiteConfig.ps1](https://hditutorialdata.blob.core.windows.net/customizecluster/editSiteConfig.ps1).</span><span class="sxs-lookup"><span data-stu-id="5556b-129">This sample script can also be found at [https://hditutorialdata.blob.core.windows.net/customizecluster/editSiteConfig.ps1](https://hditutorialdata.blob.core.windows.net/customizecluster/editSiteConfig.ps1).</span></span>

<span data-ttu-id="5556b-130">HDInsight több parancsfájlok további összetevők telepíthetők a HDInsight-fürtök biztosítja:</span><span class="sxs-lookup"><span data-stu-id="5556b-130">HDInsight provides several scripts to install additional components on HDInsight clusters:</span></span>

| <span data-ttu-id="5556b-131">Név</span><span class="sxs-lookup"><span data-stu-id="5556b-131">Name</span></span> | <span data-ttu-id="5556b-132">Szkript</span><span class="sxs-lookup"><span data-stu-id="5556b-132">Script</span></span> |
| --- | --- |
| <span data-ttu-id="5556b-133">**Spark telepítése**</span><span class="sxs-lookup"><span data-stu-id="5556b-133">**Install Spark**</span></span> |<span data-ttu-id="5556b-134">https://hdiconfigactions.BLOB.Core.Windows.NET/sparkconfigactionv03/Spark-Installer-v03.ps1.</span><span class="sxs-lookup"><span data-stu-id="5556b-134">https://hdiconfigactions.blob.core.windows.net/sparkconfigactionv03/spark-installer-v03.ps1.</span></span> <span data-ttu-id="5556b-135">Lásd: [telepítése és használata a HDInsight Spark-fürtök][hdinsight-install-spark].</span><span class="sxs-lookup"><span data-stu-id="5556b-135">See [Install and use Spark on HDInsight clusters][hdinsight-install-spark].</span></span> |
| <span data-ttu-id="5556b-136">**R telepítéséhez**</span><span class="sxs-lookup"><span data-stu-id="5556b-136">**Install R**</span></span> |<span data-ttu-id="5556b-137">https://hdiconfigactions.BLOB.Core.Windows.NET/rconfigactionv02/r-Installer-v02.ps1.</span><span class="sxs-lookup"><span data-stu-id="5556b-137">https://hdiconfigactions.blob.core.windows.net/rconfigactionv02/r-installer-v02.ps1.</span></span> <span data-ttu-id="5556b-138">Lásd: [telepítése és használata R HDInsight-fürtök][hdinsight-r-scripts].</span><span class="sxs-lookup"><span data-stu-id="5556b-138">See [Install and use R on HDInsight clusters][hdinsight-r-scripts].</span></span> |
| <span data-ttu-id="5556b-139">**Solr telepítése**</span><span class="sxs-lookup"><span data-stu-id="5556b-139">**Install Solr**</span></span> |<span data-ttu-id="5556b-140">https://hdiconfigactions.BLOB.Core.Windows.NET/solrconfigactionv01/solr-Installer-v01.ps1.</span><span class="sxs-lookup"><span data-stu-id="5556b-140">https://hdiconfigactions.blob.core.windows.net/solrconfigactionv01/solr-installer-v01.ps1.</span></span> <span data-ttu-id="5556b-141">Lásd: [telepítése és használata Solr a HDInsight-fürtök](hdinsight-hadoop-solr-install.md).</span><span class="sxs-lookup"><span data-stu-id="5556b-141">See [Install and use Solr on HDInsight clusters](hdinsight-hadoop-solr-install.md).</span></span> |
| <span data-ttu-id="5556b-142">- **Giraph telepítése**</span><span class="sxs-lookup"><span data-stu-id="5556b-142">- **Install Giraph**</span></span> |<span data-ttu-id="5556b-143">https://hdiconfigactions.BLOB.Core.Windows.NET/giraphconfigactionv01/giraph-Installer-v01.ps1.</span><span class="sxs-lookup"><span data-stu-id="5556b-143">https://hdiconfigactions.blob.core.windows.net/giraphconfigactionv01/giraph-installer-v01.ps1.</span></span> <span data-ttu-id="5556b-144">Lásd: [telepítése és használata Giraph a HDInsight-fürtök](hdinsight-hadoop-giraph-install.md).</span><span class="sxs-lookup"><span data-stu-id="5556b-144">See [Install and use Giraph on HDInsight clusters](hdinsight-hadoop-giraph-install.md).</span></span> |

<span data-ttu-id="5556b-145">Parancsfájlművelet is telepíthető, az Azure-portálon az Azure PowerShell vagy a HDInsight .NET SDK használatával.</span><span class="sxs-lookup"><span data-stu-id="5556b-145">Script Action can be deployed from the Azure portal, Azure PowerShell or by using the HDInsight .NET SDK.</span></span>  <span data-ttu-id="5556b-146">További információkért lásd: [testreszabása HDInsight-fürtök használata parancsfájlművelet][hdinsight-cluster-customize].</span><span class="sxs-lookup"><span data-stu-id="5556b-146">For more information, see [Customize HDInsight clusters using Script Action][hdinsight-cluster-customize].</span></span>

> [!NOTE]
> <span data-ttu-id="5556b-147">A minta parancsfájlok csak a HDInsight-fürt verziószáma 3.1-es vagy újabb működik.</span><span class="sxs-lookup"><span data-stu-id="5556b-147">The sample scripts work only with HDInsight cluster version 3.1 or above.</span></span> <span data-ttu-id="5556b-148">A HDInsight-fürt verziókról további információkért lásd: [HDInsight-fürt verziókról](hdinsight-component-versioning.md).</span><span class="sxs-lookup"><span data-stu-id="5556b-148">For more information on HDInsight cluster versions, see [HDInsight cluster versions](hdinsight-component-versioning.md).</span></span>
>
>

## <a name="helper-methods-for-custom-scripts"></a><span data-ttu-id="5556b-149">Egyéni parancsfájlok segítő módszerei</span><span class="sxs-lookup"><span data-stu-id="5556b-149">Helper methods for custom scripts</span></span>
<span data-ttu-id="5556b-150">Parancsfájl művelet segítő módszereket segédprogramok egyéni parancsfájlok írása közben használható.</span><span class="sxs-lookup"><span data-stu-id="5556b-150">Script Action helper methods are utilities that you can use while writing custom scripts.</span></span> <span data-ttu-id="5556b-151">Ezek a módszerek definiált [https://hdiconfigactions.blob.core.windows.net/configactionmodulev05/HDInsightUtilities-v05.psm1](https://hdiconfigactions.blob.core.windows.net/configactionmodulev05/HDInsightUtilities-v05.psm1), és a használatával a következő minta parancsfájlokat is szerepelhet:</span><span class="sxs-lookup"><span data-stu-id="5556b-151">These methods are defined in [https://hdiconfigactions.blob.core.windows.net/configactionmodulev05/HDInsightUtilities-v05.psm1](https://hdiconfigactions.blob.core.windows.net/configactionmodulev05/HDInsightUtilities-v05.psm1), and can be included in your scripts using the following sample:</span></span>

    # Download config action module from a well-known directory.
    $CONFIGACTIONURI = "https://hdiconfigactions.blob.core.windows.net/configactionmodulev05/HDInsightUtilities-v05.psm1";
    $CONFIGACTIONMODULE = "C:\apps\dist\HDInsightUtilities.psm1";
    $webclient = New-Object System.Net.WebClient;
    $webclient.DownloadFile($CONFIGACTIONURI, $CONFIGACTIONMODULE);

    # (TIP) Import config action helper method module to make writing config action easy.
    if (Test-Path ($CONFIGACTIONMODULE))
    {
        Import-Module $CONFIGACTIONMODULE;
    }
    else
    {
        Write-Output "Failed to load HDInsightUtilities module, exiting ...";
        exit;
    }

<span data-ttu-id="5556b-152">Ez a parancsfájl által biztosított segítő módszerek a következők:</span><span class="sxs-lookup"><span data-stu-id="5556b-152">Here are the helper methods that are provided by this script:</span></span>

| <span data-ttu-id="5556b-153">Segédmetódus</span><span class="sxs-lookup"><span data-stu-id="5556b-153">Helper method</span></span> | <span data-ttu-id="5556b-154">Leírás</span><span class="sxs-lookup"><span data-stu-id="5556b-154">Description</span></span> |
| --- | --- |
| <span data-ttu-id="5556b-155">**Mentés-HDIFile**</span><span class="sxs-lookup"><span data-stu-id="5556b-155">**Save-HDIFile**</span></span> |<span data-ttu-id="5556b-156">Töltse le a fájlt a a megadott egységes erőforrás-azonosító (URI) a helyi lemezen, amely az Azure virtuális gép csomópontot a fürthöz rendelt helyre.</span><span class="sxs-lookup"><span data-stu-id="5556b-156">Download a file from the specified Uniform Resource Identifier (URI) to a location on the local disk that is associated with the Azure VM node assigned to the cluster.</span></span> |
| <span data-ttu-id="5556b-157">**Bontsa ki a HDIZippedFile**</span><span class="sxs-lookup"><span data-stu-id="5556b-157">**Expand-HDIZippedFile**</span></span> |<span data-ttu-id="5556b-158">Bontsa ki a tömörített fájlt.</span><span class="sxs-lookup"><span data-stu-id="5556b-158">Unzip a zipped file.</span></span> |
| <span data-ttu-id="5556b-159">**Invoke-HDICmdScript**</span><span class="sxs-lookup"><span data-stu-id="5556b-159">**Invoke-HDICmdScript**</span></span> |<span data-ttu-id="5556b-160">Futtassa a parancsfájlt a cmd.exe.</span><span class="sxs-lookup"><span data-stu-id="5556b-160">Run a script from cmd.exe.</span></span> |
| <span data-ttu-id="5556b-161">**Írási-HDILog**</span><span class="sxs-lookup"><span data-stu-id="5556b-161">**Write-HDILog**</span></span> |<span data-ttu-id="5556b-162">Kimeneti írása egy parancsfájl műveletéhez használt egyéni parancsfájl.</span><span class="sxs-lookup"><span data-stu-id="5556b-162">Write output from the custom script used for a script action.</span></span> |
| <span data-ttu-id="5556b-163">**Get-szolgáltatások**</span><span class="sxs-lookup"><span data-stu-id="5556b-163">**Get-Services**</span></span> |<span data-ttu-id="5556b-164">Ha a parancsfájl végrehajtása a gépen futó szolgáltatásokat listájának lekérése.</span><span class="sxs-lookup"><span data-stu-id="5556b-164">Get a list of services running on the machine where the script executes.</span></span> |
| <span data-ttu-id="5556b-165">**Get-szolgáltatás**</span><span class="sxs-lookup"><span data-stu-id="5556b-165">**Get-Service**</span></span> |<span data-ttu-id="5556b-166">Az adott szolgáltatás nevű bemeneti adatként, részletes információ egy adott szolgáltatáshoz (a szolgáltatás neve, folyamatazonosító, állapot, stb.) Ha a parancsfájl végrehajtása a gépen.</span><span class="sxs-lookup"><span data-stu-id="5556b-166">With the specific service name as input, get detailed information for a specific service (service name, process ID, state, etc.) on the machine where the script executes.</span></span> |
| <span data-ttu-id="5556b-167">**Get-HDIServices**</span><span class="sxs-lookup"><span data-stu-id="5556b-167">**Get-HDIServices**</span></span> |<span data-ttu-id="5556b-168">HDInsight services fut a számítógépen, ahol a parancsfájl végrehajtása listájának lekérése.</span><span class="sxs-lookup"><span data-stu-id="5556b-168">Get a list of HDInsight services running on the computer where the script executes.</span></span> |
| <span data-ttu-id="5556b-169">**Get-HDIService**</span><span class="sxs-lookup"><span data-stu-id="5556b-169">**Get-HDIService**</span></span> |<span data-ttu-id="5556b-170">Az adott HDInsight szolgáltatásnévvel bemeneti adatként, részletes információ egy adott szolgáltatáshoz (a szolgáltatás neve, folyamatazonosító, állapot, stb.) Ha a parancsfájl végrehajtása a gépen.</span><span class="sxs-lookup"><span data-stu-id="5556b-170">With the specific HDInsight service name as input, get detailed information for a specific service (service name, process ID, state, etc.) on the machine where the script executes.</span></span> |
| <span data-ttu-id="5556b-171">**Get-ServicesRunning**</span><span class="sxs-lookup"><span data-stu-id="5556b-171">**Get-ServicesRunning**</span></span> |<span data-ttu-id="5556b-172">Futó szolgáltatásokat a számítógépen a parancsfájl végrehajtása ahol listájának lekérése.</span><span class="sxs-lookup"><span data-stu-id="5556b-172">Get a list of services that are running on the computer where the script executes.</span></span> |
| <span data-ttu-id="5556b-173">**Get-ServiceRunning**</span><span class="sxs-lookup"><span data-stu-id="5556b-173">**Get-ServiceRunning**</span></span> |<span data-ttu-id="5556b-174">Ellenőrizze, hogy egy adott szolgáltatáshoz (a neve szerint) fut a számítógépen ahol a parancsfájl végrehajtása.</span><span class="sxs-lookup"><span data-stu-id="5556b-174">Check if a specific service (by name) is running on the computer where the script executes.</span></span> |
| <span data-ttu-id="5556b-175">**Get-HDIServicesRunning**</span><span class="sxs-lookup"><span data-stu-id="5556b-175">**Get-HDIServicesRunning**</span></span> |<span data-ttu-id="5556b-176">HDInsight services fut a számítógépen, ahol a parancsfájl végrehajtása listájának lekérése.</span><span class="sxs-lookup"><span data-stu-id="5556b-176">Get a list of HDInsight services running on the computer where the script executes.</span></span> |
| <span data-ttu-id="5556b-177">**Get-HDIServiceRunning**</span><span class="sxs-lookup"><span data-stu-id="5556b-177">**Get-HDIServiceRunning**</span></span> |<span data-ttu-id="5556b-178">Ellenőrizze, hogy egy adott HDInsight-szolgáltatás (név) a számítógépen. Ha a parancsfájl végrehajtása.</span><span class="sxs-lookup"><span data-stu-id="5556b-178">Check if a specific HDInsight service (by name) is running on the computer where the script executes.</span></span> |
| <span data-ttu-id="5556b-179">**Get-HDIHadoopVersion**</span><span class="sxs-lookup"><span data-stu-id="5556b-179">**Get-HDIHadoopVersion**</span></span> |<span data-ttu-id="5556b-180">A verziója telepítve azon a számítógépen, ahol a parancsfájl végrehajtása Hadoop beolvasása.</span><span class="sxs-lookup"><span data-stu-id="5556b-180">Get the version of Hadoop installed on the computer where the script executes.</span></span> |
| <span data-ttu-id="5556b-181">**Teszt-IsHDIHeadNode**</span><span class="sxs-lookup"><span data-stu-id="5556b-181">**Test-IsHDIHeadNode**</span></span> |<span data-ttu-id="5556b-182">Ellenőrizze, hogy a számítógépen, ahol a parancsfájl végrehajtása egy átjárócsomóponttal.</span><span class="sxs-lookup"><span data-stu-id="5556b-182">Check if the computer where the script executes is a head node.</span></span> |
| <span data-ttu-id="5556b-183">**Teszt-IsActiveHDIHeadNode**</span><span class="sxs-lookup"><span data-stu-id="5556b-183">**Test-IsActiveHDIHeadNode**</span></span> |<span data-ttu-id="5556b-184">Ellenőrizze, hogy a számítógépen, ahol a parancsfájl végrehajtása az aktív központi csomópont.</span><span class="sxs-lookup"><span data-stu-id="5556b-184">Check if the computer where the script executes is an active head node.</span></span> |
| <span data-ttu-id="5556b-185">**Teszt-IsHDIDataNode**</span><span class="sxs-lookup"><span data-stu-id="5556b-185">**Test-IsHDIDataNode**</span></span> |<span data-ttu-id="5556b-186">Ellenőrizze, hogy a számítógépen, ahol a parancsfájl végrehajtása egy adatcsomóponton.</span><span class="sxs-lookup"><span data-stu-id="5556b-186">Check if the computer where the script executes is a data node.</span></span> |
| <span data-ttu-id="5556b-187">**Szerkesztés-HDIConfigFile**</span><span class="sxs-lookup"><span data-stu-id="5556b-187">**Edit-HDIConfigFile**</span></span> |<span data-ttu-id="5556b-188">A konfigurációs fájlok hive-site.xml, a core-site.xml, a hdfs-site.xml, a mapred-site.xml vagy a yarn-site.xml szerkesztéséhez.</span><span class="sxs-lookup"><span data-stu-id="5556b-188">Edit the config files hive-site.xml, core-site.xml, hdfs-site.xml, mapred-site.xml, or yarn-site.xml.</span></span> |

## <a name="best-practices-for-script-development"></a><span data-ttu-id="5556b-189">Parancsfájl fejlesztési ajánlott eljárásai</span><span class="sxs-lookup"><span data-stu-id="5556b-189">Best practices for script development</span></span>
<span data-ttu-id="5556b-190">A HDInsight-fürtök egyéni parancsfájl fejlesztésekor van több bevált gyakorlatokat, amelyekkel tartsa szem előtt:</span><span class="sxs-lookup"><span data-stu-id="5556b-190">When you develop a custom script for an HDInsight cluster, there are several best practices to keep in mind:</span></span>

* <span data-ttu-id="5556b-191">A Hadoop-verziójának ellenőrzése</span><span class="sxs-lookup"><span data-stu-id="5556b-191">Check for the Hadoop version</span></span>

    <span data-ttu-id="5556b-192">Csak a HDInsight (Hadoop 2.4) 3.1-es verzióját vagy újabb támogatás parancsfájlművelet használatával egyéni összetevőinek telepítése egy fürt.</span><span class="sxs-lookup"><span data-stu-id="5556b-192">Only HDInsight version 3.1 (Hadoop 2.4) and above support using Script Action to install custom components on a cluster.</span></span> <span data-ttu-id="5556b-193">Az egyéni parancsfájlt kell használnia a **Get-HDIHadoopVersion** segédmetódus Hadoop verziójának más feladatok végrehajtása a parancsfájl a folytatása előtt.</span><span class="sxs-lookup"><span data-stu-id="5556b-193">In your custom script, you must use the **Get-HDIHadoopVersion** helper method to check the Hadoop version before proceeding with performing other tasks in the script.</span></span>
* <span data-ttu-id="5556b-194">Tartalmaznak egy stabil parancsfájl erőforrások</span><span class="sxs-lookup"><span data-stu-id="5556b-194">Provide stable links to script resources</span></span>

    <span data-ttu-id="5556b-195">Felhasználók győződjön meg arról, hogy a parancsfájlok és egyéb összetevők szerepel a testreszabás, a fürt teljes élettartama alatt a fürt elérhetők maradnak, és, hogy a fájlok verzióinak ne változtassa meg az az időtartam.</span><span class="sxs-lookup"><span data-stu-id="5556b-195">Users should make sure that all the scripts and other artifacts used in the customization of a cluster remain available throughout the lifetime of the cluster and that the versions of these files do not change for the duration.</span></span> <span data-ttu-id="5556b-196">Ezeket az erőforrásokat szükség, ha a fürtben található csomópontok különösen szükség.</span><span class="sxs-lookup"><span data-stu-id="5556b-196">These resources are required if the reimaging of nodes in the cluster is required.</span></span> <span data-ttu-id="5556b-197">Az ajánlott eljárás, hogy töltse le és archiválni egy tárfiókot, amely a felhasználó a tartalmát.</span><span class="sxs-lookup"><span data-stu-id="5556b-197">The best practice is to download and archive everything in a Storage account that the user controls.</span></span> <span data-ttu-id="5556b-198">Az alapértelmezett tárfiókot, illetve a megadott időpontban a központi telepítés testreszabott fürt további tárfiókok is lehet.</span><span class="sxs-lookup"><span data-stu-id="5556b-198">This can be the default Storage account or any of the additional Storage accounts specified at the time of deployment for a customized cluster.</span></span>
    <span data-ttu-id="5556b-199">A Spark és R testreszabott fürt minták a dokumentáció, például hajtottunk helyi másolat készítése az erőforrások ehhez a tárfiókhoz megadott: https://hdiconfigactions.blob.core.windows.net/.</span><span class="sxs-lookup"><span data-stu-id="5556b-199">In the Spark and R customized cluster samples provided in the documentation, for example, we have made a local copy of the resources in this Storage account: https://hdiconfigactions.blob.core.windows.net/.</span></span>
* <span data-ttu-id="5556b-200">Győződjön meg arról, hogy a fürt testreszabási parancsfájl idempotent</span><span class="sxs-lookup"><span data-stu-id="5556b-200">Ensure that the cluster customization script is idempotent</span></span>

    <span data-ttu-id="5556b-201">Várt kell, hogy a csomópontok egy HDInsight-fürt rendszerképének fürt élettartama során.</span><span class="sxs-lookup"><span data-stu-id="5556b-201">You must expect that the nodes of an HDInsight cluster is reimaged during the cluster lifetime.</span></span> <span data-ttu-id="5556b-202">Amikor a fürt rendszerképének a fürt testreszabási parancsfájl futtatása.</span><span class="sxs-lookup"><span data-stu-id="5556b-202">The cluster customization script is run whenever a cluster is reimaged.</span></span> <span data-ttu-id="5556b-203">Ezt a parancsfájlt kell megtervezni, abban az értelemben, hogy szerepkörpéldány rendszerképét, akkor a parancsfájl győződjön meg arról, hogy a fürt küld vissza a ugyanaz az idempotent testreszabott állapotát az imént, miután a parancsfájl lefutott, a fürt kezdetben létrehozásának első alkalommal kell.</span><span class="sxs-lookup"><span data-stu-id="5556b-203">This script must be designed to be idempotent in the sense that upon reimaging, the script should ensure that the cluster is returned to the same customized state that it was in just after the script ran for the first time when the cluster was initially created.</span></span> <span data-ttu-id="5556b-204">Például ha egyéni parancsfájl telepítették az alkalmazást D:\AppLocation első futtassa, és minden ezt követő futtatáskor, különösen, akkor a parancsfájl ellenőrizze, hogy a az alkalmazás D:\AppLocation helyen van, a parancsfájlban szereplő lépések más folytatása előtt.</span><span class="sxs-lookup"><span data-stu-id="5556b-204">For example, if a custom script installed an application at D:\AppLocation on its first run, then on each subsequent run, upon reimaging, the script should check whether the application exists at the D:\AppLocation location before proceeding with other steps in the script.</span></span>
* <span data-ttu-id="5556b-205">Az optimális helyen egyéni összetevők telepítéséhez</span><span class="sxs-lookup"><span data-stu-id="5556b-205">Install custom components in the optimal location</span></span>

    <span data-ttu-id="5556b-206">Ha a fürtcsomópontok vannak lemezképet, a C:\ meghajtó-erőforrás és a D:\ rendszermeghajtón is újraformázza, adatokat és alkalmazásokat, amelyek adott meghajtókon telepítette elvesztését eredményezi.</span><span class="sxs-lookup"><span data-stu-id="5556b-206">When cluster nodes are reimaged, the C:\ resource drive and D:\ system drive can be reformatted, resulting in the loss of data and applications that had been installed on those drives.</span></span> <span data-ttu-id="5556b-207">Ez is fordulhat, ha egy Azure virtuális gép (VM) csomópontot, amely a fürt része leáll, és olyan új csomópont cseréli le.</span><span class="sxs-lookup"><span data-stu-id="5556b-207">This could also happen if an Azure virtual machine (VM) node that is part of the cluster goes down and is replaced by a new node.</span></span> <span data-ttu-id="5556b-208">Összetevők a D:\ meghajtóra, vagy a fürt C:\apps helyen telepíthető.</span><span class="sxs-lookup"><span data-stu-id="5556b-208">You can install components on the D:\ drive or in the C:\apps location on the cluster.</span></span> <span data-ttu-id="5556b-209">A C:\ meghajtón más helyeken vannak fenntartva.</span><span class="sxs-lookup"><span data-stu-id="5556b-209">All other locations on the C:\ drive are reserved.</span></span> <span data-ttu-id="5556b-210">Adja meg a helyet Ha alkalmazások vagy a könyvtárak a fürt testreszabási parancsfájl telepíteni.</span><span class="sxs-lookup"><span data-stu-id="5556b-210">Specify the location where applications or libraries are to be installed in the cluster customization script.</span></span>
* <span data-ttu-id="5556b-211">Magas rendelkezésre állásának a fürt-architektúra</span><span class="sxs-lookup"><span data-stu-id="5556b-211">Ensure high availability of the cluster architecture</span></span>

    <span data-ttu-id="5556b-212">HDInsight felépítésű egy aktív-passzív a magas rendelkezésre állás érdekében egy átjárócsomóponttal van aktív módot (ahol a HDInsight-szolgáltatás fut) és a más átjárócsomópont (mely a hdinsight szolgáltatások nem futnak) készenléti kiszolgálói módban van.</span><span class="sxs-lookup"><span data-stu-id="5556b-212">HDInsight has an active-passive architecture for high availability, in which one head node is in active mode (where the HDInsight services are running) and the other head node is in standby mode (in which HDInsight services are not running).</span></span> <span data-ttu-id="5556b-213">A csomópontok aktív és passzív módban vált, ha a HDInsight szolgáltatások megszakadnak.</span><span class="sxs-lookup"><span data-stu-id="5556b-213">The nodes switch active and passive modes if HDInsight services are interrupted.</span></span> <span data-ttu-id="5556b-214">A parancsfájlművelet kiszolgálók telepítését mindkét központi csomópont a magas rendelkezésre állású használata esetén vegye figyelembe, hogy a HDInsight feladatátvételi mechanizmus nem tudja, hogy automatikusan áthelyezze a feladatokat a felhasználók által telepített szolgáltatások.</span><span class="sxs-lookup"><span data-stu-id="5556b-214">If a script action is used to install services on both head nodes for high availability, note that the HDInsight failover mechanism is not able to automatically fail over these user-installed services.</span></span> <span data-ttu-id="5556b-215">HDInsight központi csomópontokra, amelyeket várhatóan magas rendelkezésre állású legyen, felhasználók által telepített szolgáltatások vagy a saját feladatátvételi mechanizmus, ha az aktív-passzív módban van vagy kell aktív-aktív módban.</span><span class="sxs-lookup"><span data-stu-id="5556b-215">So user-installed services on HDInsight head nodes that are expected to be highly available must either have their own failover mechanism if in active-passive mode or be in active-active mode.</span></span>

    <span data-ttu-id="5556b-216">Egy HDInsight-parancsfájlművelet parancs mindkét központi csomópontján fut, amikor az átjárócsomópont szerepkör van megadva értékként a *ClusterRoleCollection* paraméter.</span><span class="sxs-lookup"><span data-stu-id="5556b-216">An HDInsight Script Action command runs on both head nodes when the head-node role is specified as a value in the *ClusterRoleCollection* parameter.</span></span> <span data-ttu-id="5556b-217">Egyéni parancsfájl tervezésekor ügyeljen, hogy, hogy a telepítő tisztában-e a parancsfájlt.</span><span class="sxs-lookup"><span data-stu-id="5556b-217">So when you design a custom script, make sure that your script is aware of this setup.</span></span> <span data-ttu-id="5556b-218">A problémák, ahol ugyanazok a szolgáltatások telepítve, és mindkét központi csomópont elindult és azok egymással versengő végül nem kell futtatnia.</span><span class="sxs-lookup"><span data-stu-id="5556b-218">You should not run into problems where the same services are installed and started on both of the head nodes and they end up competing with each other.</span></span> <span data-ttu-id="5556b-219">Is vegye figyelembe, hogy adatok nem vesztek el során, különösen, tehát parancsfájlművelet keresztül telepített szoftverek ezek az események rugalmasak lehetnek.</span><span class="sxs-lookup"><span data-stu-id="5556b-219">Also, be aware that data is lost during reimaging, so software installed via Script Action has to be resilient to such events.</span></span> <span data-ttu-id="5556b-220">Alkalmazások úgy kell megtervezni, magas rendelkezésre állású adatait, amely van osztva sok csomópontjai között.</span><span class="sxs-lookup"><span data-stu-id="5556b-220">Applications should be designed to work with highly available data that is distributed across many nodes.</span></span> <span data-ttu-id="5556b-221">Vegye figyelembe, hogy képes-e akár 1/5 egy fürt csomópontja lemezképet egy időben.</span><span class="sxs-lookup"><span data-stu-id="5556b-221">Note that as many as 1/5 of the nodes in a cluster can be reimaged at the same time.</span></span>
* <span data-ttu-id="5556b-222">Az Azure Blob storage használata egyéni összetevők konfigurálása</span><span class="sxs-lookup"><span data-stu-id="5556b-222">Configure the custom components to use Azure Blob storage</span></span>

    <span data-ttu-id="5556b-223">Az egyéni összetevők, amelyek telepítése a fürtcsomópontokon előfordulhat, hogy rendelkezik a Hadoop elosztott fájlrendszerrel (HDFS) tárolót használjanak alapértelmezett konfigurációval.</span><span class="sxs-lookup"><span data-stu-id="5556b-223">The custom components that you install on the cluster nodes might have a default configuration to use Hadoop Distributed File System (HDFS) storage.</span></span> <span data-ttu-id="5556b-224">Módosítania kell a konfigurációt használja helyette az Azure Blob Storage tárolóban.</span><span class="sxs-lookup"><span data-stu-id="5556b-224">You should change the configuration to use Azure Blob storage instead.</span></span> <span data-ttu-id="5556b-225">A fürt lemezkép alaphelyzetbe a HDFS fájlrendszer formázott lekérdezi és elveszítik az ott tárolt adatokat.</span><span class="sxs-lookup"><span data-stu-id="5556b-225">On a cluster reimage, the HDFS file system gets formatted and you would lose any data that is stored there.</span></span> <span data-ttu-id="5556b-226">Azure Blob storage használatával helyette biztosítja, hogy az adatok őrződnek meg.</span><span class="sxs-lookup"><span data-stu-id="5556b-226">Using Azure Blob storage instead ensures that your data is retained.</span></span>

## <a name="common-usage-patterns"></a><span data-ttu-id="5556b-227">Gyakori használati minták</span><span class="sxs-lookup"><span data-stu-id="5556b-227">Common usage patterns</span></span>
<span data-ttu-id="5556b-228">Ez a szakasz néhány gyakori használati mintái, amelyek a saját egyéni parancsfájl írásakor mutatjuk be végrehajtási nyújt útmutatást.</span><span class="sxs-lookup"><span data-stu-id="5556b-228">This section provides guidance on implementing some of the common usage patterns that you might run into while writing your own custom script.</span></span>

### <a name="configure-environment-variables"></a><span data-ttu-id="5556b-229">Környezeti változók konfigurálása</span><span class="sxs-lookup"><span data-stu-id="5556b-229">Configure environment variables</span></span>
<span data-ttu-id="5556b-230">Gyakran a parancsfájl művelet fejlesztési, úgy érzi, hogy a környezeti változók megadása szükséges.</span><span class="sxs-lookup"><span data-stu-id="5556b-230">Often in script action development, you feel the need to set environment variables.</span></span> <span data-ttu-id="5556b-231">Például legvalószínűbb forgatókönyv esetén bináris egy külső webhelyről töltheti le, telepítse azt a fürt, és adja hozzá a "PATH" környezeti változó a telepítés helyét.</span><span class="sxs-lookup"><span data-stu-id="5556b-231">For instance, a most likely scenario is when you download a binary from an external site, install it on the cluster, and add the location of where it is installed to your ‘PATH’ environment variable.</span></span> <span data-ttu-id="5556b-232">Az alábbi kódrészletben bemutatja, hogyan környezeti változókat beállítani az egyéni parancsprogramok futtatására.</span><span class="sxs-lookup"><span data-stu-id="5556b-232">The following snippet shows you how to set environment variables in the custom script.</span></span>

    Write-HDILog "Starting environment variable setting at: $(Get-Date)";
    [Environment]::SetEnvironmentVariable('MDS_RUNNER_CUSTOM_CLUSTER', 'true', 'Machine');

<span data-ttu-id="5556b-233">A jelen nyilatkozat beállítja a környezeti változó **MDS_RUNNER_CUSTOM_CLUSTER** értékre a "true", továbbá beállítja ezt a változót kell gépre kiterjedő hatóköre.</span><span class="sxs-lookup"><span data-stu-id="5556b-233">This statement sets the environment variable **MDS_RUNNER_CUSTOM_CLUSTER** to the value 'true' and also sets the scope of this variable to be machine-wide.</span></span> <span data-ttu-id="5556b-234">Esetenként fontos, hogy a környezeti változók a megfelelő hatókörben – számítógép vagy felhasználó van beállítva.</span><span class="sxs-lookup"><span data-stu-id="5556b-234">At times it is important that environment variables are set at the appropriate scope – machine or user.</span></span> <span data-ttu-id="5556b-235">Tekintse meg a [Itt] [ 1] környezeti változók beállításával kapcsolatos további információt.</span><span class="sxs-lookup"><span data-stu-id="5556b-235">Refer [here][1] for more information on setting environment variables.</span></span>

### <a name="access-to-locations-where-the-custom-scripts-are-stored"></a><span data-ttu-id="5556b-236">Hozzáférés az egyéni parancsfájlok tároló helyekhez</span><span class="sxs-lookup"><span data-stu-id="5556b-236">Access to locations where the custom scripts are stored</span></span>
<span data-ttu-id="5556b-237">A fürt testreszabásához használt parancsfájlok kell bármelyik kell az alapértelmezett tárfiókot, a fürt vagy egy nyilvános csak olvasható tároló bármely más tárfiók.</span><span class="sxs-lookup"><span data-stu-id="5556b-237">Scripts used to customize a cluster needs to either be in the default storage account for the cluster or in a public read-only container on any other storage account.</span></span> <span data-ttu-id="5556b-238">Ha a parancsfájl máshol található erőforrásokhoz fér hozzá ezek kell lenniük egy nyilvánosan elérhető (legalább nyilvános csak olvasható).</span><span class="sxs-lookup"><span data-stu-id="5556b-238">If your script accesses resources located elsewhere these need to be in a publicly accessible (at least public read-only).</span></span> <span data-ttu-id="5556b-239">Például előfordulhat, hogy szeretne hozzáférni egy fájlhoz, és mentse a SaveFile-HDI-paranccsal.</span><span class="sxs-lookup"><span data-stu-id="5556b-239">For instance you might want to access a file and save it using the SaveFile-HDI command.</span></span>

    Save-HDIFile -SrcUri 'https://somestorageaccount.blob.core.windows.net/somecontainer/some-file.jar' -DestFile 'C:\apps\dist\hadoop-2.4.0.2.1.9.0-2196\share\hadoop\mapreduce\some-file.jar'

<span data-ttu-id="5556b-240">Ebben a példában meg kell győződnie arról, hogy a tároló "somecontainer" tárfiókban "somestorageaccount" nyilvánosan elérhető.</span><span class="sxs-lookup"><span data-stu-id="5556b-240">In this example, you must ensure that the container 'somecontainer' in storage account 'somestorageaccount' is publicly accessible.</span></span> <span data-ttu-id="5556b-241">Ellenkező esetben a parancsfájl "Nem található" kivételt okoz, és sikertelen lesz.</span><span class="sxs-lookup"><span data-stu-id="5556b-241">Otherwise, the script throws a ‘Not Found’ exception and fail.</span></span>

### <a name="pass-parameters-to-the-add-azurermhdinsightscriptaction-cmdlet"></a><span data-ttu-id="5556b-242">Az Add-AzureRmHDInsightScriptAction parancsmagnak paraméterekkel</span><span class="sxs-lookup"><span data-stu-id="5556b-242">Pass parameters to the Add-AzureRmHDInsightScriptAction cmdlet</span></span>
<span data-ttu-id="5556b-243">Az Add-AzureRmHDInsightScriptAction parancsmag több paraméterekkel, magában foglalja a parancsfájl az összes paraméter értékeként karakterlánc formázni kell.</span><span class="sxs-lookup"><span data-stu-id="5556b-243">To pass multiple parameters to the Add-AzureRmHDInsightScriptAction cmdlet, you need to format the string value to contain all parameters for the script.</span></span> <span data-ttu-id="5556b-244">Példa:</span><span class="sxs-lookup"><span data-stu-id="5556b-244">For example:</span></span>

    "-CertifcateUri wasb:///abc.pfx -CertificatePassword 123456 -InstallFolderName MyFolder"

<span data-ttu-id="5556b-245">vagy</span><span class="sxs-lookup"><span data-stu-id="5556b-245">or</span></span>

    $parameters = '-Parameters "{0};{1};{2}"' -f $CertificateName,$certUriWithSasToken,$CertificatePassword


### <a name="throw-exception-for-failed-cluster-deployment"></a><span data-ttu-id="5556b-246">Sikertelen fürttelepítés kivétel throw</span><span class="sxs-lookup"><span data-stu-id="5556b-246">Throw exception for failed cluster deployment</span></span>
<span data-ttu-id="5556b-247">Ha pontosan értesítés szeretné, hogy a fürt testreszabási sikertelen volt a vártnak, fontos, hogy kivételt jelez, és a fürt létrehozása sikertelen.</span><span class="sxs-lookup"><span data-stu-id="5556b-247">If you want to get accurately notified of the fact that cluster customization did not succeed as expected, it is important to throw an exception and fail the cluster creation.</span></span> <span data-ttu-id="5556b-248">Például előfordulhat, hogy szeretne feldolgozni egy fájlt, ha létezik, és kezelni a hiba eset, ha a fájl nem létezik.</span><span class="sxs-lookup"><span data-stu-id="5556b-248">For instance, you might want to process a file if it exists and handle the error case where the file does not exist.</span></span> <span data-ttu-id="5556b-249">Ez biztosítja, hogy a parancsfájl szabályosan kilép, és a fürt állapota megfelelően van azonosítva.</span><span class="sxs-lookup"><span data-stu-id="5556b-249">This would ensure that the script exits gracefully and the state of the cluster is correctly known.</span></span> <span data-ttu-id="5556b-250">A következő példa bemutatja, hogyan ennek érdekében a következő kódrészletet:</span><span class="sxs-lookup"><span data-stu-id="5556b-250">The following snippet gives an example of how to achieve this:</span></span>

    If(Test-Path($SomePath)) {
        #Process file in some way
    } else {
        # File does not exist; handle error case
        # Print error message
    exit
    }

<span data-ttu-id="5556b-251">Ezt a kódrészletet a Ha a fájl nem létezett, vezet, olyan állapotban, ahol a parancsfájl ténylegesen kilép szabályosan a hibaüzenet a következő nyomtatás után, és a fürt eléri futó állapotban, feltéve, hogy a "sikeres" fürt szabásának fejeződött be.</span><span class="sxs-lookup"><span data-stu-id="5556b-251">In this snippet, if the file did not exist, it would lead to a state where the script actually exits gracefully after printing the error message, and the cluster reaches running state assuming it "successfully" completed cluster customization process.</span></span> <span data-ttu-id="5556b-252">Pontosan értesíti arról, hogy a fürt lényegében testreszabási Ha nem sikerült egy hiányzó fájlok miatt várt módon, hogy jobban megfelelő kivételt jelez, és a fürt testreszabási lépés sikertelen lesz.</span><span class="sxs-lookup"><span data-stu-id="5556b-252">If you want to be accurately notified of the fact that cluster customization essentially did not succeed as expected because of a missing file, it is more appropriate to throw an exception and fail the cluster customization step.</span></span> <span data-ttu-id="5556b-253">Ennek eléréséhez kell használnia a következő minta kódrészletet.</span><span class="sxs-lookup"><span data-stu-id="5556b-253">To achieve this you must use the following sample code snippet instead.</span></span>

    If(Test-Path($SomePath)) {
        #Process file in some way
    } else {
        # File does not exist; handle error case
        # Print error message
    throw
    }


## <a name="checklist-for-deploying-a-script-action"></a><span data-ttu-id="5556b-254">Ellenőrzőlista a üzembe helyezéséhez egy parancsfájlművelet</span><span class="sxs-lookup"><span data-stu-id="5556b-254">Checklist for deploying a script action</span></span>
<span data-ttu-id="5556b-255">Azt a parancsfájlok telepítendő előkészítésekor tartott lépései a következők:</span><span class="sxs-lookup"><span data-stu-id="5556b-255">Here are the steps we took when preparing to deploy these scripts:</span></span>

1. <span data-ttu-id="5556b-256">Helyezze el az egyéni parancsfájlok, amely elérhető a fürt csomópontjai a telepítés során helyen tartalmazó fájlokat.</span><span class="sxs-lookup"><span data-stu-id="5556b-256">Put the files that contain the custom scripts in a place that is accessible by the cluster nodes during deployment.</span></span> <span data-ttu-id="5556b-257">Ez az alapértelmezett vagy a fürtöt tartalmazó környezetben, vagy bármely más nyilvánosan elérhető tároló időpontjában megadott további tárfiókok lehet.</span><span class="sxs-lookup"><span data-stu-id="5556b-257">This can be any of the default or additional Storage accounts specified at the time of cluster deployment, or any other publicly accessible storage container.</span></span>
2. <span data-ttu-id="5556b-258">Győződjön meg arról, hogy azok végrehajtási idempotently, így a parancsfájl hajtható végre több alkalommal ugyanazon a csomóponton parancsfájlok ellenőrzést hozzáadása.</span><span class="sxs-lookup"><span data-stu-id="5556b-258">Add checks into scripts to make sure that they execute idempotently, so that the script can be executed multiple times on the same node.</span></span>
3. <span data-ttu-id="5556b-259">Használja a **Write-Output** Azure PowerShell-parancsmag segítségével STDOUT, valamint az STDERR dokumentumokat nyomtassanak azokon.</span><span class="sxs-lookup"><span data-stu-id="5556b-259">Use the **Write-Output** Azure PowerShell cmdlet to print to STDOUT as well as STDERR.</span></span> <span data-ttu-id="5556b-260">Ne használjon **Write-Host**.</span><span class="sxs-lookup"><span data-stu-id="5556b-260">Do not use **Write-Host**.</span></span>
4. <span data-ttu-id="5556b-261">Ideiglenes mappát, például a $env: ideiglenes, tartsa meg a letöltött parancsfájlok által használt, majd eltávolítással parancsfájlok rendelkezik végrehajtása után.</span><span class="sxs-lookup"><span data-stu-id="5556b-261">Use a temporary file folder, such as $env:TEMP, to keep the downloaded file used by the scripts and then clean them up after scripts have executed.</span></span>
5. <span data-ttu-id="5556b-262">Csak a D:\ vagy C:\apps egyéni szoftver telepítése.</span><span class="sxs-lookup"><span data-stu-id="5556b-262">Install custom software only at D:\ or C:\apps.</span></span> <span data-ttu-id="5556b-263">A C: meghajtón más helyeken nem használható, azok le foglalva.</span><span class="sxs-lookup"><span data-stu-id="5556b-263">Other locations on the C: drive should not be used as they are reserved.</span></span> <span data-ttu-id="5556b-264">Vegye figyelembe, hogy a C: meghajtón a C:\apps mappán kívüli fájlok telepítése azt eredményezheti, a telepítő hibákat reimages a csomópont alatt.</span><span class="sxs-lookup"><span data-stu-id="5556b-264">Note that installing files on the C: drive outside of the C:\apps folder may result in setup failures during reimages of the node.</span></span>
6. <span data-ttu-id="5556b-265">Abban az esetben, ha az operációs rendszer szintű beállításokat vagy Hadoop szolgáltatás konfigurációs fájlok módosultak, érdemes lehet indítsa újra a HDInsight-szolgáltatásokat, így kiválaszthatja a bármely az operációs rendszer szintű beállításokat, például a parancsfájlokban beállított környezeti változókat.</span><span class="sxs-lookup"><span data-stu-id="5556b-265">In the event that OS-level settings or Hadoop service configuration files were changed, you may want to restart HDInsight services so that they can pick up any OS-level settings, such as the environment variables set in the scripts.</span></span>

## <a name="debug-custom-scripts"></a><span data-ttu-id="5556b-266">Egyéni parancsfájlok hibakeresése</span><span class="sxs-lookup"><span data-stu-id="5556b-266">Debug custom scripts</span></span>
<span data-ttu-id="5556b-267">A parancsfájl hibanaplókat más kimeneti az alapértelmezett tárfiókot, ahol létrehozták a fürthöz megadott együtt tárolják.</span><span class="sxs-lookup"><span data-stu-id="5556b-267">The script error logs are stored, along with other output, in the default Storage account that you specified for the cluster at its creation.</span></span> <span data-ttu-id="5556b-268">A naplók tárolódnak a nevű tábla *u < \cluster-name-fragment >< \time-stamp > setuplog*.</span><span class="sxs-lookup"><span data-stu-id="5556b-268">The logs are stored in a table with the name *u<\cluster-name-fragment><\time-stamp>setuplog*.</span></span> <span data-ttu-id="5556b-269">Ezek a rekordok, az összes, amelyen a parancsprogram lefut a fürtben lévő csomópontok (átjárócsomópont és feldolgozó csomópontokat) összesített naplók.</span><span class="sxs-lookup"><span data-stu-id="5556b-269">These are aggregated logs that have records from all of the nodes (head node and worker nodes) on which the script runs in the cluster.</span></span>
<span data-ttu-id="5556b-270">Ellenőrizze a naplókat, egyszerűen a HDInsight Tools for Visual Studio használandó.</span><span class="sxs-lookup"><span data-stu-id="5556b-270">An easy way to check the logs is to use HDInsight Tools for Visual Studio.</span></span> <span data-ttu-id="5556b-271">Az eszközök telepítése, lásd: [első lépések a Visual Studio Hadoop tools for HDInsight használatával](hdinsight-hadoop-visual-studio-tools-get-started.md#install-data-lake-tools-for-visual-studio)</span><span class="sxs-lookup"><span data-stu-id="5556b-271">For installing the tools, see [Get started using Visual Studio Hadoop tools for HDInsight](hdinsight-hadoop-visual-studio-tools-get-started.md#install-data-lake-tools-for-visual-studio)</span></span>

<span data-ttu-id="5556b-272">**A részletek a naplóban a Visual Studio használatával**</span><span class="sxs-lookup"><span data-stu-id="5556b-272">**To check the log using Visual Studio**</span></span>

1. <span data-ttu-id="5556b-273">Nyissa meg a Visual Studiót.</span><span class="sxs-lookup"><span data-stu-id="5556b-273">Open Visual Studio.</span></span>
2. <span data-ttu-id="5556b-274">Kattintson a **nézet**, és kattintson a **Server Explorer**.</span><span class="sxs-lookup"><span data-stu-id="5556b-274">Click **View**, and then click **Server Explorer**.</span></span>
3. <span data-ttu-id="5556b-275">Kattintson a jobb gombbal az "Azure", kattintson a Csatlakozás gombra **Microsoft Azure-előfizetések**, és írja be a hitelesítő adatait.</span><span class="sxs-lookup"><span data-stu-id="5556b-275">Right-click "Azure", click Connect to **Microsoft Azure Subscriptions**, and then enter your credentials.</span></span>
4. <span data-ttu-id="5556b-276">Bontsa ki a **tárolási**, bontsa ki a rendszer az alapértelmezett fájlrendszert használja az Azure storage-fiók, **táblák**, majd kattintson duplán a tábla neve.</span><span class="sxs-lookup"><span data-stu-id="5556b-276">Expand **Storage**, expand the Azure storage account used as the default file system, expand **Tables**, and then double-click the table name.</span></span>

<span data-ttu-id="5556b-277">Akkor is távoli az STDOUT és az STDERR megjelenítéséhez egyéni parancsfájlok a fürt csomópontjai.</span><span class="sxs-lookup"><span data-stu-id="5556b-277">You can also remote into the cluster nodes to see both STDOUT and STDERR for custom scripts.</span></span> <span data-ttu-id="5556b-278">A naplók minden egyes csomóponton csak ahhoz a csomóponthoz, és be vannak jelentkezve **C:\HDInsightLogs\DeploymentAgent.log**.</span><span class="sxs-lookup"><span data-stu-id="5556b-278">The logs on each node are specific only to that node and are logged into **C:\HDInsightLogs\DeploymentAgent.log**.</span></span> <span data-ttu-id="5556b-279">Ezekben a naplófájlokban rögzíti az egyéni parancsfájl az összes kimenetének.</span><span class="sxs-lookup"><span data-stu-id="5556b-279">These log files record all outputs from the custom script.</span></span> <span data-ttu-id="5556b-280">A külső parancsfájl művelet egy példa napló részlet így néz ki:</span><span class="sxs-lookup"><span data-stu-id="5556b-280">An example log snippet for a Spark script action looks like this:</span></span>

    Microsoft.Hadoop.Deployment.Engine.CustomPowershellScriptCommand; Details : BEGIN: Invoking powershell script https://configactions.blob.core.windows.net/sparkconfigactions/spark-installer.ps1.;
    Version : 2.1.0.0;
    ActivityId : 739e61f5-aa22-4254-aafc-9faf56fc2692;
    AzureVMName : HEADNODE0;
    IsException : False;
    ExceptionType : ;
    ExceptionMessage : ;
    InnerExceptionType : ;
    InnerExceptionMessage : ;
    Exception : ;
    ...

    Starting Spark installation at: 09/04/2014 21:46:02 Done with Spark installation at: 09/04/2014 21:46:38;

    Version : 2.1.0.0;
    ActivityId : 739e61f5-aa22-4254-aafc-9faf56fc2692;
    AzureVMName : HEADNODE0;
    IsException : False;
    ExceptionType : ;
    ExceptionMessage : ;
    InnerExceptionType : ;
    InnerExceptionMessage : ;
    Exception : ;
    ...

    Microsoft.Hadoop.Deployment.Engine.CustomPowershellScriptCommand;
    Details : END: Invoking powershell script https://configactions.blob.core.windows.net/sparkconfigactions/spark-installer.ps1.;
    Version : 2.1.0.0;
    ActivityId : 739e61f5-aa22-4254-aafc-9faf56fc2692;
    AzureVMName : HEADNODE0;
    IsException : False;
    ExceptionType : ;
    ExceptionMessage : ;
    InnerExceptionType : ;
    InnerExceptionMessage : ;
    Exception : ;


<span data-ttu-id="5556b-281">Ez a napló az egyszerű, hogy a külső parancsfájl művelet lett végrehajtva a HEADNODE0 nevű virtuális Gépre, és, hogy a kivételek fordultak elő a végrehajtás során is.</span><span class="sxs-lookup"><span data-stu-id="5556b-281">In this log, it is clear that the Spark script action has been executed on the VM named HEADNODE0 and that no exceptions were thrown during the execution.</span></span>

<span data-ttu-id="5556b-282">Abban az esetben, ha végrehajtási hiba történik, a kimeneti leíró azt is ez a naplófájl tartalmazza.</span><span class="sxs-lookup"><span data-stu-id="5556b-282">In the event that an execution failure occurs, the output describing it is also contained in this log file.</span></span> <span data-ttu-id="5556b-283">Ezek a naplók található információk is segíthetnek megoldani az esetleg felmerülő problémákat parancsfájl.</span><span class="sxs-lookup"><span data-stu-id="5556b-283">The information provided in these logs should be helpful in debugging script problems that may arise.</span></span>

## <a name="see-also"></a><span data-ttu-id="5556b-284">Lásd még:</span><span class="sxs-lookup"><span data-stu-id="5556b-284">See also</span></span>
* <span data-ttu-id="5556b-285">[Parancsfájlművelet HDInsight-fürtök testreszabása][hdinsight-cluster-customize]</span><span class="sxs-lookup"><span data-stu-id="5556b-285">[Customize HDInsight clusters using Script Action][hdinsight-cluster-customize]</span></span>
* <span data-ttu-id="5556b-286">[Telepítse, és válassza a Spark on HDInsight-fürtök][hdinsight-install-spark]</span><span class="sxs-lookup"><span data-stu-id="5556b-286">[Install and use Spark on HDInsight clusters][hdinsight-install-spark]</span></span>
* <span data-ttu-id="5556b-287">[Telepítheti és használhatja a HDInsight-fürtök R][hdinsight-r-scripts]</span><span class="sxs-lookup"><span data-stu-id="5556b-287">[Install and use R on HDInsight clusters][hdinsight-r-scripts]</span></span>
* <span data-ttu-id="5556b-288">[Telepítheti és használhatja a HDInsight-fürtök Solr](hdinsight-hadoop-solr-install.md).</span><span class="sxs-lookup"><span data-stu-id="5556b-288">[Install and use Solr on HDInsight clusters](hdinsight-hadoop-solr-install.md).</span></span>
* <span data-ttu-id="5556b-289">[Telepítheti és használhatja a HDInsight-fürtök Giraph](hdinsight-hadoop-giraph-install.md).</span><span class="sxs-lookup"><span data-stu-id="5556b-289">[Install and use Giraph on HDInsight clusters](hdinsight-hadoop-giraph-install.md).</span></span>

[hdinsight-provision]: hdinsight-provision-clusters.md
[hdinsight-cluster-customize]: hdinsight-hadoop-customize-cluster.md
[hdinsight-install-spark]: hdinsight-hadoop-spark-install.md
[hdinsight-r-scripts]: hdinsight-hadoop-r-scripts.md
[powershell-install-configure]: install-configure-powershell.md

<!--Reference links in article-->
[1]: https://msdn.microsoft.com/library/96xafkes(v=vs.110).aspx
