---
title: "Parancsfájl-művelet fejlesztése a Linux-alapú HDInsight - Azure |} Microsoft Docs"
description: "Megtudhatja, hogyan Bash parancsfájlok használata a Linux-alapú HDInsight-fürtök testreszabása. A HDInsight a parancsfájl művelet a szolgáltatás lehetővé teszi a parancsfájlok futtatása közben, vagy a fürt létrehozása után. Parancsfájlok segítségével fürt konfigurációs beállításokat módosítaná, vagy további szoftvereket telepíteni."
services: hdinsight
documentationcenter: 
author: Blackmist
manager: jhubbard
editor: cgronlun
ms.assetid: cf4c89cd-f7da-4a10-857f-838004965d3e
ms.service: hdinsight
ms.custom: hdinsightactive
ms.workload: big-data
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/31/2017
ms.author: larryfr
ms.openlocfilehash: 7f1a0bd8c7e60770d376f10eaea136a55c632c5e
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/03/2017
---
# <a name="script-action-development-with-hdinsight"></a><span data-ttu-id="7ea3b-105">Parancsfájl művelet fejlesztése a Hdinsighttal</span><span class="sxs-lookup"><span data-stu-id="7ea3b-105">Script action development with HDInsight</span></span>

<span data-ttu-id="7ea3b-106">Ismerje meg, hogyan szabhatja testre a Bash parancsfájlok használata a HDInsight-fürthöz.</span><span class="sxs-lookup"><span data-stu-id="7ea3b-106">Learn how to customize your HDInsight cluster using Bash scripts.</span></span> <span data-ttu-id="7ea3b-107">A Parancsfájlműveletek olyan testreszabhatja a HDInsight alatt vagy a fürt létrehozása után.</span><span class="sxs-lookup"><span data-stu-id="7ea3b-107">Script actions are a way to customize HDInsight during or after cluster creation.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="7ea3b-108">A jelen dokumentumban leírt lépések egy HDInsight-fürt által használt Linux igényelnek.</span><span class="sxs-lookup"><span data-stu-id="7ea3b-108">The steps in this document require an HDInsight cluster that uses Linux.</span></span> <span data-ttu-id="7ea3b-109">A Linux az egyetlen operációs rendszer, amely a HDInsight 3.4-es vagy újabb verziói esetében használható.</span><span class="sxs-lookup"><span data-stu-id="7ea3b-109">Linux is the only operating system used on HDInsight version 3.4 or greater.</span></span> <span data-ttu-id="7ea3b-110">További tudnivalókért lásd: [A HDInsight elavulása Windows rendszeren](hdinsight-component-versioning.md#hdinsight-windows-retirement).</span><span class="sxs-lookup"><span data-stu-id="7ea3b-110">For more information, see [HDInsight retirement on Windows](hdinsight-component-versioning.md#hdinsight-windows-retirement).</span></span>

## <a name="what-are-script-actions"></a><span data-ttu-id="7ea3b-111">Mik azok a Parancsfájlműveletek</span><span class="sxs-lookup"><span data-stu-id="7ea3b-111">What are script actions</span></span>

<span data-ttu-id="7ea3b-112">A Parancsfájlműveletek olyan Bash parancsfájlok futó Azure a fürtcsomópontokon ellenőrizze konfigurációs módosításokat, vagy a szoftver telepítése.</span><span class="sxs-lookup"><span data-stu-id="7ea3b-112">Script actions are Bash scripts that Azure runs on the cluster nodes to make configuration changes or install software.</span></span> <span data-ttu-id="7ea3b-113">A parancsfájlművelet legfelső szintű végrehajtásakor, és a fürtcsomópontok teljes körű hozzáférési jogosultságokat biztosít.</span><span class="sxs-lookup"><span data-stu-id="7ea3b-113">A script action is executed as root, and provides full access rights to the cluster nodes.</span></span>

<span data-ttu-id="7ea3b-114">A Parancsfájlműveletek az alábbi eljárások alkalmazhatók:</span><span class="sxs-lookup"><span data-stu-id="7ea3b-114">Script actions can be applied through the following methods:</span></span>

| <span data-ttu-id="7ea3b-115">Ezt a módszert használja egy parancsfájl alkalmazandó...</span><span class="sxs-lookup"><span data-stu-id="7ea3b-115">Use this method to apply a script...</span></span> | <span data-ttu-id="7ea3b-116">Során fürt létrehozása...</span><span class="sxs-lookup"><span data-stu-id="7ea3b-116">During cluster creation...</span></span> | <span data-ttu-id="7ea3b-117">Egy futó fürtön...</span><span class="sxs-lookup"><span data-stu-id="7ea3b-117">On a running cluster...</span></span> |
| --- |:---:|:---:|
| <span data-ttu-id="7ea3b-118">Azure Portal</span><span class="sxs-lookup"><span data-stu-id="7ea3b-118">Azure portal</span></span> |<span data-ttu-id="7ea3b-119">✓</span><span class="sxs-lookup"><span data-stu-id="7ea3b-119">✓</span></span> |<span data-ttu-id="7ea3b-120">✓</span><span class="sxs-lookup"><span data-stu-id="7ea3b-120">✓</span></span> |
| <span data-ttu-id="7ea3b-121">Azure PowerShell</span><span class="sxs-lookup"><span data-stu-id="7ea3b-121">Azure PowerShell</span></span> |<span data-ttu-id="7ea3b-122">✓</span><span class="sxs-lookup"><span data-stu-id="7ea3b-122">✓</span></span> |<span data-ttu-id="7ea3b-123">✓</span><span class="sxs-lookup"><span data-stu-id="7ea3b-123">✓</span></span> |
| <span data-ttu-id="7ea3b-124">Azure CLI</span><span class="sxs-lookup"><span data-stu-id="7ea3b-124">Azure CLI</span></span> |&nbsp; |<span data-ttu-id="7ea3b-125">✓</span><span class="sxs-lookup"><span data-stu-id="7ea3b-125">✓</span></span> |
| <span data-ttu-id="7ea3b-126">HDInsight .NET SDK</span><span class="sxs-lookup"><span data-stu-id="7ea3b-126">HDInsight .NET SDK</span></span> |<span data-ttu-id="7ea3b-127">✓</span><span class="sxs-lookup"><span data-stu-id="7ea3b-127">✓</span></span> |<span data-ttu-id="7ea3b-128">✓</span><span class="sxs-lookup"><span data-stu-id="7ea3b-128">✓</span></span> |
| <span data-ttu-id="7ea3b-129">Az Azure Resource Manager-sablon</span><span class="sxs-lookup"><span data-stu-id="7ea3b-129">Azure Resource Manager Template</span></span> |<span data-ttu-id="7ea3b-130">✓</span><span class="sxs-lookup"><span data-stu-id="7ea3b-130">✓</span></span> |&nbsp; |

<span data-ttu-id="7ea3b-131">Ezekkel a módszerekkel Parancsfájlműveletek alkalmazandó további információkért lásd: [testreszabása HDInsight-fürtök Parancsfájlműveletek segítségével](hdinsight-hadoop-customize-cluster-linux.md).</span><span class="sxs-lookup"><span data-stu-id="7ea3b-131">For more information on using these methods to apply script actions, see [Customize HDInsight clusters using script actions](hdinsight-hadoop-customize-cluster-linux.md).</span></span>

## <span data-ttu-id="7ea3b-132"><a name="bestPracticeScripting"></a>Parancsfájl fejlesztési ajánlott eljárásai</span><span class="sxs-lookup"><span data-stu-id="7ea3b-132"><a name="bestPracticeScripting"></a>Best practices for script development</span></span>

<span data-ttu-id="7ea3b-133">A HDInsight-fürtök egyéni parancsfájl fejlesztésekor van több bevált gyakorlatokat, amelyekkel tartsa szem előtt:</span><span class="sxs-lookup"><span data-stu-id="7ea3b-133">When you develop a custom script for an HDInsight cluster, there are several best practices to keep in mind:</span></span>

* [<span data-ttu-id="7ea3b-134">A cél a Hadoop-verzió</span><span class="sxs-lookup"><span data-stu-id="7ea3b-134">Target the Hadoop version</span></span>](#bPS1)
* [<span data-ttu-id="7ea3b-135">A cél az operációs rendszer verziója</span><span class="sxs-lookup"><span data-stu-id="7ea3b-135">Target the OS Version</span></span>](#bps10)
* [<span data-ttu-id="7ea3b-136">Tartalmaznak egy stabil parancsfájl erőforrások</span><span class="sxs-lookup"><span data-stu-id="7ea3b-136">Provide stable links to script resources</span></span>](#bPS2)
* [<span data-ttu-id="7ea3b-137">Előre lefordított erőforrások használata</span><span class="sxs-lookup"><span data-stu-id="7ea3b-137">Use pre-compiled resources</span></span>](#bPS4)
* [<span data-ttu-id="7ea3b-138">Győződjön meg arról, hogy a fürt testreszabási parancsfájl idempotent</span><span class="sxs-lookup"><span data-stu-id="7ea3b-138">Ensure that the cluster customization script is idempotent</span></span>](#bPS3)
* [<span data-ttu-id="7ea3b-139">Magas rendelkezésre állásának a fürt-architektúra</span><span class="sxs-lookup"><span data-stu-id="7ea3b-139">Ensure high availability of the cluster architecture</span></span>](#bPS5)
* [<span data-ttu-id="7ea3b-140">Az Azure Blob storage használata egyéni összetevők konfigurálása</span><span class="sxs-lookup"><span data-stu-id="7ea3b-140">Configure the custom components to use Azure Blob storage</span></span>](#bPS6)
* [<span data-ttu-id="7ea3b-141">Kapcsolatos adatokat ír az STDOUT és az STDERR</span><span class="sxs-lookup"><span data-stu-id="7ea3b-141">Write information to STDOUT and STDERR</span></span>](#bPS7)
* [<span data-ttu-id="7ea3b-142">ASCII LF sorvégződések rendelkező fájlok mentése</span><span class="sxs-lookup"><span data-stu-id="7ea3b-142">Save files as ASCII with LF line endings</span></span>](#bps8)
* [<span data-ttu-id="7ea3b-143">Használja az újrapróbálkozási logika átmeneti hibák elhárítása</span><span class="sxs-lookup"><span data-stu-id="7ea3b-143">Use retry logic to recover from transient errors</span></span>](#bps9)

> [!IMPORTANT]
> <span data-ttu-id="7ea3b-144">A Parancsfájlműveletek 60 percen belül kell végrehajtani, vagy a folyamat sikertelen lesz.</span><span class="sxs-lookup"><span data-stu-id="7ea3b-144">Script actions must complete within 60 minutes or the process fails.</span></span> <span data-ttu-id="7ea3b-145">Csomópont telepítése során, a parancsfájl fut egyidejűleg más beállítás és konfiguráció folyamatok.</span><span class="sxs-lookup"><span data-stu-id="7ea3b-145">During node provisioning, the script runs concurrently with other setup and configuration processes.</span></span> <span data-ttu-id="7ea3b-146">Erőforrások, például a CPU-idő vagy a hálózati sávszélesség erőforrásokért való versengés okozhat a parancsfájl a befejezéshez, mint a fejlesztési környezetet az hosszabb időt vesz igénybe.</span><span class="sxs-lookup"><span data-stu-id="7ea3b-146">Competition for resources such as CPU time or network bandwidth may cause the script to take longer to finish than it does in your development environment.</span></span>

### <span data-ttu-id="7ea3b-147"><a name="bPS1"></a>A cél a Hadoop-verzió</span><span class="sxs-lookup"><span data-stu-id="7ea3b-147"><a name="bPS1"></a>Target the Hadoop version</span></span>

<span data-ttu-id="7ea3b-148">Különböző verzióit a HDInsight Hadoop-szolgáltatások és telepített összetevők különböző verziói működnek.</span><span class="sxs-lookup"><span data-stu-id="7ea3b-148">Different versions of HDInsight have different versions of Hadoop services and components installed.</span></span> <span data-ttu-id="7ea3b-149">Ha a parancsfájl egy szolgáltatás vagy összetevő adott verziójának vár, csak akkor ajánlott a parancsfájl, amely tartalmazza a szükséges összetevők HDInsight verziójával.</span><span class="sxs-lookup"><span data-stu-id="7ea3b-149">If your script expects a specific version of a service or component, you should only use the script with the version of HDInsight that includes the required components.</span></span> <span data-ttu-id="7ea3b-150">HDInsight használatával mellékelt összetevő verzióin információt a [HDInsight-összetevők verziószámozása](hdinsight-component-versioning.md) dokumentum.</span><span class="sxs-lookup"><span data-stu-id="7ea3b-150">You can find information on component versions included with HDInsight using the [HDInsight component versioning](hdinsight-component-versioning.md) document.</span></span>

### <span data-ttu-id="7ea3b-151"><a name="bps10"></a>A cél az operációs rendszer verziója</span><span class="sxs-lookup"><span data-stu-id="7ea3b-151"><a name="bps10"></a> Target the OS version</span></span>

<span data-ttu-id="7ea3b-152">Linux-alapú HDInsight az Ubuntu Linux terjesztési alapul.</span><span class="sxs-lookup"><span data-stu-id="7ea3b-152">Linux-based HDInsight is based on the Ubuntu Linux distribution.</span></span> <span data-ttu-id="7ea3b-153">HDInsight különböző verziói különböző verzióinak Ubuntu, módosíthatja a parancsfájl működését támaszkodnak.</span><span class="sxs-lookup"><span data-stu-id="7ea3b-153">Different versions of HDInsight rely on different versions of Ubuntu, which may change how your script behaves.</span></span> <span data-ttu-id="7ea3b-154">HDInsight 3.4 és korábbi például Ubuntu verziók Upstart használó alapul.</span><span class="sxs-lookup"><span data-stu-id="7ea3b-154">For example, HDInsight 3.4 and earlier are based on Ubuntu versions that use Upstart.</span></span> <span data-ttu-id="7ea3b-155">3.5-ös verziója Ubuntu 16.04, amely használja Systemd alapul.</span><span class="sxs-lookup"><span data-stu-id="7ea3b-155">Version 3.5 is based on Ubuntu 16.04, which uses Systemd.</span></span> <span data-ttu-id="7ea3b-156">Systemd és Upstart támaszkodnak különböző parancsok, így a parancsfájlt is együttműködve kell írni.</span><span class="sxs-lookup"><span data-stu-id="7ea3b-156">Systemd and Upstart rely on different commands, so your script should be written to work with both.</span></span>

<span data-ttu-id="7ea3b-157">HDInsight 3.4 és 3.5-ös közötti másik fontos különbség, hogy `JAVA_HOME` Java 8 most mutat.</span><span class="sxs-lookup"><span data-stu-id="7ea3b-157">Another important difference between HDInsight 3.4 and 3.5 is that `JAVA_HOME` now points to Java 8.</span></span>

<span data-ttu-id="7ea3b-158">Az operációs rendszer verzióját használatával ellenőrizheti `lsb_release`.</span><span class="sxs-lookup"><span data-stu-id="7ea3b-158">You can check the OS version by using `lsb_release`.</span></span> <span data-ttu-id="7ea3b-159">A következő kód bemutatja, hogyan állapítható meg, ha a parancsfájl Ubuntu 14 vagy 16 fut:</span><span class="sxs-lookup"><span data-stu-id="7ea3b-159">The following code demonstrates how to determine if the script is running on Ubuntu 14 or 16:</span></span>

```bash
OS_VERSION=$(lsb_release -sr)
if [[ $OS_VERSION == 14* ]]; then
    echo "OS verion is $OS_VERSION. Using hue-binaries-14-04."
    HUE_TARFILE=hue-binaries-14-04.tgz
elif [[ $OS_VERSION == 16* ]]; then
    echo "OS verion is $OS_VERSION. Using hue-binaries-16-04."
    HUE_TARFILE=hue-binaries-16-04.tgz
fi
...
if [[ $OS_VERSION == 16* ]]; then
    echo "Using systemd configuration"
    systemctl daemon-reload
    systemctl stop webwasb.service    
    systemctl start webwasb.service
else
    echo "Using upstart configuration"
    initctl reload-configuration
    stop webwasb
    start webwasb
fi
...
if [[ $OS_VERSION == 14* ]]; then
    export JAVA_HOME=/usr/lib/jvm/java-7-openjdk-amd64
elif [[ $OS_VERSION == 16* ]]; then
    export JAVA_HOME=/usr/lib/jvm/java-8-openjdk-amd64
fi
```

<span data-ttu-id="7ea3b-160">A teljes parancsfájl, amely tartalmazza ezeket a https://hdiconfigactions.blob.core.windows.net/linuxhueconfigactionv02/install-hue-uber-v02.sh kódtöredékek található.</span><span class="sxs-lookup"><span data-stu-id="7ea3b-160">You can find the full script that contains these snippets at https://hdiconfigactions.blob.core.windows.net/linuxhueconfigactionv02/install-hue-uber-v02.sh.</span></span>

<span data-ttu-id="7ea3b-161">Ubuntu, a HDInsight által használt verziója: a [HDInsight összetevő verziószáma](hdinsight-component-versioning.md) dokumentum.</span><span class="sxs-lookup"><span data-stu-id="7ea3b-161">For the version of Ubuntu that is used by HDInsight, see the [HDInsight component version](hdinsight-component-versioning.md) document.</span></span>

<span data-ttu-id="7ea3b-162">Systemd és Upstart közötti különbségek ismertetése: [Systemd Upstart felhasználók](https://wiki.ubuntu.com/SystemdForUpstartUsers).</span><span class="sxs-lookup"><span data-stu-id="7ea3b-162">To understand the differences between Systemd and Upstart, see [Systemd for Upstart users](https://wiki.ubuntu.com/SystemdForUpstartUsers).</span></span>

### <span data-ttu-id="7ea3b-163"><a name="bPS2"></a>Tartalmaznak egy stabil parancsfájl erőforrások</span><span class="sxs-lookup"><span data-stu-id="7ea3b-163"><a name="bPS2"></a>Provide stable links to script resources</span></span>

<span data-ttu-id="7ea3b-164">A parancsfájl és a kapcsolódó erőforrások a fürt élettartama során rendelkezésre kell állnia.</span><span class="sxs-lookup"><span data-stu-id="7ea3b-164">The script and associated resources must remain available throughout the lifetime of the cluster.</span></span> <span data-ttu-id="7ea3b-165">Ezeket az erőforrásokat szükség, ha új csomópontokat ad hozzá a fürthöz skálázás műveletek során.</span><span class="sxs-lookup"><span data-stu-id="7ea3b-165">These resources are required if new nodes are added to the cluster during scaling operations.</span></span>

<span data-ttu-id="7ea3b-166">Az ajánlott eljárás, hogy töltse le és archiválni mindent az Azure Storage-fiók előfizetéséhez.</span><span class="sxs-lookup"><span data-stu-id="7ea3b-166">The best practice is to download and archive everything in an Azure Storage account on your subscription.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="7ea3b-167">A használt tárfiók más storage-fiók a fürt vagy egy nyilvános, csak olvasható tároló alapértelmezett tárfiókot kell lennie.</span><span class="sxs-lookup"><span data-stu-id="7ea3b-167">The storage account used must be the default storage account for the cluster or a public, read-only container on any other storage account.</span></span>

<span data-ttu-id="7ea3b-168">Például a Microsoft által biztosított minták tárolódnak a [https://hdiconfigactions.blob.core.windows.net/](https://hdiconfigactions.blob.core.windows.net/) tárfiók.</span><span class="sxs-lookup"><span data-stu-id="7ea3b-168">For example, the samples provided by Microsoft are stored in the [https://hdiconfigactions.blob.core.windows.net/](https://hdiconfigactions.blob.core.windows.net/) storage account.</span></span> <span data-ttu-id="7ea3b-169">Ez egy olyan nyilvános, csak olvasható tároló, a HDInsight csapat tartja fenn.</span><span class="sxs-lookup"><span data-stu-id="7ea3b-169">This is a public, read-only container maintained by the HDInsight team.</span></span>

### <span data-ttu-id="7ea3b-170"><a name="bPS4"></a>Előre lefordított erőforrások használata</span><span class="sxs-lookup"><span data-stu-id="7ea3b-170"><a name="bPS4"></a>Use pre-compiled resources</span></span>

<span data-ttu-id="7ea3b-171">A parancsfájl futtatásához szükséges idő csökkentése érdekében ne erőforrások a forráskód fordítási műveletek.</span><span class="sxs-lookup"><span data-stu-id="7ea3b-171">To reduce the time it takes to run the script, avoid operations that compile resources from source code.</span></span> <span data-ttu-id="7ea3b-172">Például előre lefordítani az erőforrásokat, és tárolja őket egy Azure Storage-fiók BLOB a HDInsight azonos adatközpontba.</span><span class="sxs-lookup"><span data-stu-id="7ea3b-172">For example, pre-compile resources and store them in an Azure Storage account blob in the same data center as HDInsight.</span></span>

### <span data-ttu-id="7ea3b-173"><a name="bPS3"></a>Győződjön meg arról, hogy a fürt testreszabási parancsfájl idempotent</span><span class="sxs-lookup"><span data-stu-id="7ea3b-173"><a name="bPS3"></a>Ensure that the cluster customization script is idempotent</span></span>

<span data-ttu-id="7ea3b-174">Parancsfájlok idempotent kell lennie.</span><span class="sxs-lookup"><span data-stu-id="7ea3b-174">Scripts must be idempotent.</span></span> <span data-ttu-id="7ea3b-175">Ha a parancsfájl több alkalommal fut, akkor térjen vissza a fürt olyan állapotban minden alkalommal.</span><span class="sxs-lookup"><span data-stu-id="7ea3b-175">If the script runs multiple times, it should return the cluster to the same state every time.</span></span>

<span data-ttu-id="7ea3b-176">Például egy parancsfájlt, amely módosítja a konfigurációs fájlok ne adja hozzá duplikált bejegyzéseket Ha több alkalommal lefutott.</span><span class="sxs-lookup"><span data-stu-id="7ea3b-176">For example, a script that modifies configuration files should not add duplicate entries if ran multiple times.</span></span>

### <span data-ttu-id="7ea3b-177"><a name="bPS5"></a>Magas rendelkezésre állásának a fürt-architektúra</span><span class="sxs-lookup"><span data-stu-id="7ea3b-177"><a name="bPS5"></a>Ensure high availability of the cluster architecture</span></span>

<span data-ttu-id="7ea3b-178">Linux-alapú HDInsight-fürtök meg két átjárócsomópontokkal aktív, a fürtön belül, és Parancsfájlműveletek mindkét csomópontján futtatni.</span><span class="sxs-lookup"><span data-stu-id="7ea3b-178">Linux-based HDInsight clusters provide two head nodes that are active within the cluster, and script actions run on both nodes.</span></span> <span data-ttu-id="7ea3b-179">Ha az összetevők telepítése csak egy átjárócsomópont fognak, az összetevők telepítésének mindkét központi csomópontján.</span><span class="sxs-lookup"><span data-stu-id="7ea3b-179">If the components you install expect only one head node, do not install the components on both head nodes.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="7ea3b-180">HDInsight részeként szolgáltatást tervezték, hogy igény szerint, a két központi csomópontok közötti feladatátvételt.</span><span class="sxs-lookup"><span data-stu-id="7ea3b-180">Services provided as part of HDInsight are designed to fail over between the two head nodes as needed.</span></span> <span data-ttu-id="7ea3b-181">Ez a funkció nincs kibővítve a Parancsfájlműveletek segítségével egyéni összetevőit.</span><span class="sxs-lookup"><span data-stu-id="7ea3b-181">This functionality is not extended to custom components installed through script actions.</span></span> <span data-ttu-id="7ea3b-182">Ha magas rendelkezésre állású egyéni összetevők, meg kell valósítani a saját feladatátvételi mechanizmus.</span><span class="sxs-lookup"><span data-stu-id="7ea3b-182">If you need high availability for custom components, you must implement your own failover mechanism.</span></span>

### <span data-ttu-id="7ea3b-183"><a name="bPS6"></a>Az Azure Blob storage használata egyéni összetevők konfigurálása</span><span class="sxs-lookup"><span data-stu-id="7ea3b-183"><a name="bPS6"></a>Configure the custom components to use Azure Blob storage</span></span>

<span data-ttu-id="7ea3b-184">A fürt telepített összetevők előfordulhat olyan alapértelmezett konfigurációt, amely a Hadoop elosztott fájlrendszerrel (HDFS) tárolót használ.</span><span class="sxs-lookup"><span data-stu-id="7ea3b-184">Components that you install on the cluster might have a default configuration that uses Hadoop Distributed File System (HDFS) storage.</span></span> <span data-ttu-id="7ea3b-185">HDInsight használja, mint az alapértelmezett tároló Azure Storage vagy a Data Lake Store.</span><span class="sxs-lookup"><span data-stu-id="7ea3b-185">HDInsight uses either Azure Storage or Data Lake Store as the default storage.</span></span> <span data-ttu-id="7ea3b-186">Mindkét egy HDFS kompatibilis fájlrendszert biztosít, amely az adatok továbbra is fennáll, akkor is, ha a fürtök törlése.</span><span class="sxs-lookup"><span data-stu-id="7ea3b-186">Both provide an HDFS compatible file system that persists data even if the cluster is deleted.</span></span> <span data-ttu-id="7ea3b-187">Szükség lehet az összetevők telepítését a WASB vagy ADL HDFS helyett beállításához.</span><span class="sxs-lookup"><span data-stu-id="7ea3b-187">You may need to configure components you install to use WASB or ADL instead of HDFS.</span></span>

<span data-ttu-id="7ea3b-188">A legtöbb műveleteket nem kell megadnia a fájlrendszerben.</span><span class="sxs-lookup"><span data-stu-id="7ea3b-188">For most operations, you do not need to specify the file system.</span></span> <span data-ttu-id="7ea3b-189">Például a következő másolja át a giraph-examples.jar fájlt a helyi fájlrendszer fürtre tárhelyre:</span><span class="sxs-lookup"><span data-stu-id="7ea3b-189">For example, the following copies the giraph-examples.jar file from the local file system to cluster storage:</span></span>

```bash
hdfs dfs -put /usr/hdp/current/giraph/giraph-examples.jar /example/jars/
```

<span data-ttu-id="7ea3b-190">Ebben a példában a `hdfs` parancs átlátható módon használja az alapértelmezett fürttároló.</span><span class="sxs-lookup"><span data-stu-id="7ea3b-190">In this example, the `hdfs` command transparently uses the default cluster storage.</span></span> <span data-ttu-id="7ea3b-191">Egyes műveletek esetében meg kell adnia az URI azonosító.</span><span class="sxs-lookup"><span data-stu-id="7ea3b-191">For some operations, you may need to specify the URI.</span></span> <span data-ttu-id="7ea3b-192">Például `adl:///example/jars` a Data Lake Store vagy `wasb:///example/jars` az Azure Storage.</span><span class="sxs-lookup"><span data-stu-id="7ea3b-192">For example, `adl:///example/jars` for Data Lake Store or `wasb:///example/jars` for Azure Storage.</span></span>

### <span data-ttu-id="7ea3b-193"><a name="bPS7"></a>Kapcsolatos adatokat ír az STDOUT és az STDERR</span><span class="sxs-lookup"><span data-stu-id="7ea3b-193"><a name="bPS7"></a>Write information to STDOUT and STDERR</span></span>

<span data-ttu-id="7ea3b-194">HDInsight naplózza az STDOUT és az STDERR írt parancsfájl kimenete.</span><span class="sxs-lookup"><span data-stu-id="7ea3b-194">HDInsight logs script output that is written to STDOUT and STDERR.</span></span> <span data-ttu-id="7ea3b-195">Ezt az információt az Ambari webes felhasználói felület használatával tekintheti meg.</span><span class="sxs-lookup"><span data-stu-id="7ea3b-195">You can view this information using the Ambari web UI.</span></span>

> [!NOTE]
> <span data-ttu-id="7ea3b-196">Ambari csak akkor használható, ha a fürt sikeresen létrehozva.</span><span class="sxs-lookup"><span data-stu-id="7ea3b-196">Ambari is only available if the cluster is successfully created.</span></span> <span data-ttu-id="7ea3b-197">Ha Ön egy parancsfájlművelettel és a fürt létrehozása, létrehozása sikertelen, tekintse meg a hibaelhárítási rész [testreszabása HDInsight-fürtök használata parancsfájlművelet](hdinsight-hadoop-customize-cluster-linux.md#troubleshooting) az egyéb módjai férnek hozzá a naplózott információk.</span><span class="sxs-lookup"><span data-stu-id="7ea3b-197">If you use a script action during cluster creation, and creation fails, see the troubleshooting section [Customize HDInsight clusters using script action](hdinsight-hadoop-customize-cluster-linux.md#troubleshooting) for other ways of accessing logged information.</span></span>

<span data-ttu-id="7ea3b-198">A legtöbb segédprogramok és telepítőcsomagok már kapcsolatos adatokat ír az STDOUT és az STDERR, azonban előfordulhat, hogy hozzáadandó további naplózás.</span><span class="sxs-lookup"><span data-stu-id="7ea3b-198">Most utilities and installation packages already write information to STDOUT and STDERR, however you may want to add additional logging.</span></span> <span data-ttu-id="7ea3b-199">Szöveg STDOUT küldeni, használja a `echo`.</span><span class="sxs-lookup"><span data-stu-id="7ea3b-199">To send text to STDOUT, use `echo`.</span></span> <span data-ttu-id="7ea3b-200">Példa:</span><span class="sxs-lookup"><span data-stu-id="7ea3b-200">For example:</span></span>

```bash
echo "Getting ready to install Foo"
```

<span data-ttu-id="7ea3b-201">Alapértelmezés szerint `echo` STDOUT elküldi a karakterláncot.</span><span class="sxs-lookup"><span data-stu-id="7ea3b-201">By default, `echo` sends the string to STDOUT.</span></span> <span data-ttu-id="7ea3b-202">Az STDERR irányítani, vegye fel `>&2` előtt `echo`.</span><span class="sxs-lookup"><span data-stu-id="7ea3b-202">To direct it to STDERR, add `>&2` before `echo`.</span></span> <span data-ttu-id="7ea3b-203">Példa:</span><span class="sxs-lookup"><span data-stu-id="7ea3b-203">For example:</span></span>

```bash
>&2 echo "An error occurred installing Foo"
```

<span data-ttu-id="7ea3b-204">Ez a STDERR (2) az STDOUT helyette írt adatok irányítja át.</span><span class="sxs-lookup"><span data-stu-id="7ea3b-204">This redirects information written to STDOUT to STDERR (2) instead.</span></span> <span data-ttu-id="7ea3b-205">IO átirányítással kapcsolatos további információkért lásd: [http://www.tldp.org/LDP/abs/html/io-redirection.html](http://www.tldp.org/LDP/abs/html/io-redirection.html).</span><span class="sxs-lookup"><span data-stu-id="7ea3b-205">For more information on IO redirection, see [http://www.tldp.org/LDP/abs/html/io-redirection.html](http://www.tldp.org/LDP/abs/html/io-redirection.html).</span></span>

<span data-ttu-id="7ea3b-206">A Parancsfájlműveletek által naplózott adatok megtekintéséhez használatos időkategóriát további információkért lásd: [testreszabása HDInsight-fürtök parancsfájlművelet használatával](hdinsight-hadoop-customize-cluster-linux.md#troubleshooting)</span><span class="sxs-lookup"><span data-stu-id="7ea3b-206">For more information on viewing information logged by script actions, see [Customize HDInsight clusters using script action](hdinsight-hadoop-customize-cluster-linux.md#troubleshooting)</span></span>

### <span data-ttu-id="7ea3b-207"><a name="bps8"></a>ASCII LF sorvégződések rendelkező fájlok mentése</span><span class="sxs-lookup"><span data-stu-id="7ea3b-207"><a name="bps8"></a> Save files as ASCII with LF line endings</span></span>

<span data-ttu-id="7ea3b-208">Bash parancsfájlok megszakította a LF vonallal ASCII formátumban kell tárolni.</span><span class="sxs-lookup"><span data-stu-id="7ea3b-208">Bash scripts should be stored as ASCII format, with lines terminated by LF.</span></span> <span data-ttu-id="7ea3b-209">A következő hiba miatt sikertelen lehet a fájlokat, UTF-8 tárolják, vagy a sor befejezési CRLF használja:</span><span class="sxs-lookup"><span data-stu-id="7ea3b-209">Files that are stored as UTF-8, or use CRLF as the line ending may fail with the following error:</span></span>

```
$'\r': command not found
line 1: #!/usr/bin/env: No such file or directory
```

### <span data-ttu-id="7ea3b-210"><a name="bps9"></a>Használja az újrapróbálkozási logika átmeneti hibák elhárítása</span><span class="sxs-lookup"><span data-stu-id="7ea3b-210"><a name="bps9"></a> Use retry logic to recover from transient errors</span></span>

<span data-ttu-id="7ea3b-211">Fájlok apt get vagy egyéb adatot az interneten keresztül továbbítani művelettel csomagok telepítése letöltése a művelet átmeneti hálózati hibák miatt sikertelenek lehetnek.</span><span class="sxs-lookup"><span data-stu-id="7ea3b-211">When downloading files, installing packages using apt-get, or other actions that transmit data over the internet, the action may fail due to transient networking errors.</span></span> <span data-ttu-id="7ea3b-212">A távoli erőforrás kommunikációra például folyamatban van egy biztonsági mentési csomópont feladatátvételét lehet.</span><span class="sxs-lookup"><span data-stu-id="7ea3b-212">For example, the remote resource you are communicating with may be in the process of failing over to a backup node.</span></span>

<span data-ttu-id="7ea3b-213">Ahhoz, hogy a parancsfájl rugalmas átmeneti hibáinak, újrapróbálkozási logikát is létrehozható.</span><span class="sxs-lookup"><span data-stu-id="7ea3b-213">To make your script resilient to transient errors, you can implement retry logic.</span></span> <span data-ttu-id="7ea3b-214">A következő függvény végrehajtása újrapróbálkozási logika mutatja be.</span><span class="sxs-lookup"><span data-stu-id="7ea3b-214">The following function demonstrates how to implement retry logic.</span></span> <span data-ttu-id="7ea3b-215">Újbóli próbálkozások háromszor a műveletet.</span><span class="sxs-lookup"><span data-stu-id="7ea3b-215">It retries the operation three times before failing.</span></span>

```bash
#retry
MAXATTEMPTS=3

retry() {
    local -r CMD="$@"
    local -i ATTMEPTNUM=1
    local -i RETRYINTERVAL=2

    until $CMD
    do
        if (( ATTMEPTNUM == MAXATTEMPTS ))
        then
                echo "Attempt $ATTMEPTNUM failed. no more attempts left."
                return 1
        else
                echo "Attempt $ATTMEPTNUM failed! Retrying in $RETRYINTERVAL seconds..."
                sleep $(( RETRYINTERVAL ))
                ATTMEPTNUM=$ATTMEPTNUM+1
        fi
    done
}
```

<span data-ttu-id="7ea3b-216">Az alábbi példák bemutatják, hogyan lehet a funkció használatához.</span><span class="sxs-lookup"><span data-stu-id="7ea3b-216">The following examples demonstrate how to use this function.</span></span>

```bash
retry ls -ltr foo

retry wget -O ./tmpfile.sh https://hdiconfigactions.blob.core.windows.net/linuxhueconfigactionv02/install-hue-uber-v02.sh
```

## <span data-ttu-id="7ea3b-217"><a name="helpermethods"></a>Egyéni parancsfájlok segítő módszerei</span><span class="sxs-lookup"><span data-stu-id="7ea3b-217"><a name="helpermethods"></a>Helper methods for custom scripts</span></span>

<span data-ttu-id="7ea3b-218">Parancsfájl művelet segítő módszereket segédprogramok egyéni parancsfájlok írása közben használható.</span><span class="sxs-lookup"><span data-stu-id="7ea3b-218">Script action helper methods are utilities that you can use while writing custom scripts.</span></span> <span data-ttu-id="7ea3b-219">Ezek a módszerek tartalmazza a[https://hdiconfigactions.blob.core.windows.net/linuxconfigactionmodulev01/HDInsightUtilities-v01.sh](https://hdiconfigactions.blob.core.windows.net/linuxconfigactionmodulev01/HDInsightUtilities-v01.sh) parancsfájl.</span><span class="sxs-lookup"><span data-stu-id="7ea3b-219">These methods are contained in the[https://hdiconfigactions.blob.core.windows.net/linuxconfigactionmodulev01/HDInsightUtilities-v01.sh](https://hdiconfigactions.blob.core.windows.net/linuxconfigactionmodulev01/HDInsightUtilities-v01.sh) script.</span></span> <span data-ttu-id="7ea3b-220">Használja a következő letölteni és telepíteni azokat a parancsfájl részeként:</span><span class="sxs-lookup"><span data-stu-id="7ea3b-220">Use the following to download and use them as part of your script:</span></span>

```bash
# Import the helper method module.
wget -O /tmp/HDInsightUtilities-v01.sh -q https://hdiconfigactions.blob.core.windows.net/linuxconfigactionmodulev01/HDInsightUtilities-v01.sh && source /tmp/HDInsightUtilities-v01.sh && rm -f /tmp/HDInsightUtilities-v01.sh
```

<span data-ttu-id="7ea3b-221">A következő segítő a parancsfájlt használhatja:</span><span class="sxs-lookup"><span data-stu-id="7ea3b-221">The following helpers available for use in your script:</span></span>

| <span data-ttu-id="7ea3b-222">Segítő kihasználtsága</span><span class="sxs-lookup"><span data-stu-id="7ea3b-222">Helper usage</span></span> | <span data-ttu-id="7ea3b-223">Leírás</span><span class="sxs-lookup"><span data-stu-id="7ea3b-223">Description</span></span> |
| --- | --- |
| `download_file SOURCEURL DESTFILEPATH [OVERWRITE]` |<span data-ttu-id="7ea3b-224">A fájl a forrás URI letölti a fájl megadott elérési útja.</span><span class="sxs-lookup"><span data-stu-id="7ea3b-224">Downloads a file from the source URI to the specified file path.</span></span> <span data-ttu-id="7ea3b-225">Alapértelmezés szerint azt a meglévő fájl felülírásának nem.</span><span class="sxs-lookup"><span data-stu-id="7ea3b-225">By default, it does not overwrite an existing file.</span></span> |
| `untar_file TARFILE DESTDIR` |<span data-ttu-id="7ea3b-226">Kibontja a bont (használatával `-xf`) a cél könyvtárba.</span><span class="sxs-lookup"><span data-stu-id="7ea3b-226">Extracts a tar file (using `-xf`) to the destination directory.</span></span> |
| `test_is_headnode` |<span data-ttu-id="7ea3b-227">Ha a fürt átjárócsomópontjához, visszatérési 1; itt futott Ellenkező esetben 0.</span><span class="sxs-lookup"><span data-stu-id="7ea3b-227">If ran on a cluster head node, return 1; otherwise, 0.</span></span> |
| `test_is_datanode` |<span data-ttu-id="7ea3b-228">Ha az aktuális csomópont adatcsomóponton (munkavégző), a visszatérési érték egy 1. Ellenkező esetben 0.</span><span class="sxs-lookup"><span data-stu-id="7ea3b-228">If the current node is a data (worker) node, return a 1; otherwise, 0.</span></span> |
| `test_is_first_datanode` |<span data-ttu-id="7ea3b-229">Ha az aktuális csomópont az első adatok (munkavégző) csomópontra (elnevezett workernode0) adja vissza egy 1. Ellenkező esetben 0.</span><span class="sxs-lookup"><span data-stu-id="7ea3b-229">If the current node is the first data (worker) node (named workernode0) return a 1; otherwise, 0.</span></span> |
| `get_headnodes` |<span data-ttu-id="7ea3b-230">Térjen vissza a headnodes teljesen minősített tartományneve a fürtben.</span><span class="sxs-lookup"><span data-stu-id="7ea3b-230">Return the fully qualified domain name of the headnodes in the cluster.</span></span> <span data-ttu-id="7ea3b-231">Neveket vesszővel tagolt.</span><span class="sxs-lookup"><span data-stu-id="7ea3b-231">Names are comma delimited.</span></span> <span data-ttu-id="7ea3b-232">Üres karakterlánc hibát akkor adja vissza.</span><span class="sxs-lookup"><span data-stu-id="7ea3b-232">An empty string is returned on error.</span></span> |
| `get_primary_headnode` |<span data-ttu-id="7ea3b-233">Lekérdezi az elsődleges headnode teljesen minősített tartományneve.</span><span class="sxs-lookup"><span data-stu-id="7ea3b-233">Gets the fully qualified domain name of the primary headnode.</span></span> <span data-ttu-id="7ea3b-234">Üres karakterlánc hibát akkor adja vissza.</span><span class="sxs-lookup"><span data-stu-id="7ea3b-234">An empty string is returned on error.</span></span> |
| `get_secondary_headnode` |<span data-ttu-id="7ea3b-235">Lekérdezi a másodlagos headnode teljesen minősített tartományneve.</span><span class="sxs-lookup"><span data-stu-id="7ea3b-235">Gets the fully qualified domain name of the secondary headnode.</span></span> <span data-ttu-id="7ea3b-236">Üres karakterlánc hibát akkor adja vissza.</span><span class="sxs-lookup"><span data-stu-id="7ea3b-236">An empty string is returned on error.</span></span> |
| `get_primary_headnode_number` |<span data-ttu-id="7ea3b-237">A numerikus utótagját, az elsődleges headnode lekérdezi.</span><span class="sxs-lookup"><span data-stu-id="7ea3b-237">Gets the numeric suffix of the primary headnode.</span></span> <span data-ttu-id="7ea3b-238">Üres karakterlánc hibát akkor adja vissza.</span><span class="sxs-lookup"><span data-stu-id="7ea3b-238">An empty string is returned on error.</span></span> |
| `get_secondary_headnode_number` |<span data-ttu-id="7ea3b-239">A numerikus utótagját, a másodlagos headnode lekérdezi.</span><span class="sxs-lookup"><span data-stu-id="7ea3b-239">Gets the numeric suffix of the secondary headnode.</span></span> <span data-ttu-id="7ea3b-240">Üres karakterlánc hibát akkor adja vissza.</span><span class="sxs-lookup"><span data-stu-id="7ea3b-240">An empty string is returned on error.</span></span> |

## <span data-ttu-id="7ea3b-241"><a name="commonusage"></a>Gyakori használati minták</span><span class="sxs-lookup"><span data-stu-id="7ea3b-241"><a name="commonusage"></a>Common usage patterns</span></span>

<span data-ttu-id="7ea3b-242">Ez a szakasz néhány gyakori használati mintái, amelyek a saját egyéni parancsfájl írásakor mutatjuk be végrehajtási nyújt útmutatást.</span><span class="sxs-lookup"><span data-stu-id="7ea3b-242">This section provides guidance on implementing some of the common usage patterns that you might run into while writing your own custom script.</span></span>

### <a name="passing-parameters-to-a-script"></a><span data-ttu-id="7ea3b-243">A parancsfájl paraméterek átadása</span><span class="sxs-lookup"><span data-stu-id="7ea3b-243">Passing parameters to a script</span></span>

<span data-ttu-id="7ea3b-244">Bizonyos esetekben a parancsfájl paramétereit lehet szükség.</span><span class="sxs-lookup"><span data-stu-id="7ea3b-244">In some cases, your script may require parameters.</span></span> <span data-ttu-id="7ea3b-245">Például szükség lehet a rendszergazdai jelszó a fürt az Ambari REST API használata esetén.</span><span class="sxs-lookup"><span data-stu-id="7ea3b-245">For example, you may need the admin password for the cluster when using the Ambari REST API.</span></span>

<span data-ttu-id="7ea3b-246">A parancsfájlnak átadott paraméterek nevezik *pozícióparaméterek*, és a hozzárendelt `$1` az első paraméter `$2` a második, és így-on.</span><span class="sxs-lookup"><span data-stu-id="7ea3b-246">Parameters passed to the script are known as *positional parameters*, and are assigned to `$1` for the first parameter, `$2` for the second, and so-on.</span></span> <span data-ttu-id="7ea3b-247">`$0`a parancsfájl önmagában nevét tartalmazza.</span><span class="sxs-lookup"><span data-stu-id="7ea3b-247">`$0` contains the name of the script itself.</span></span>

<span data-ttu-id="7ea3b-248">Értékek átadott paraméterek közé kell tenni szimpla idézőjelben ('), a parancsfájl.</span><span class="sxs-lookup"><span data-stu-id="7ea3b-248">Values passed to the script as parameters should be enclosed by single quotes (').</span></span> <span data-ttu-id="7ea3b-249">Ezzel biztosítja, hogy az átadott értéknek literálként van-e kezelni.</span><span class="sxs-lookup"><span data-stu-id="7ea3b-249">Doing so ensures that the passed value is treated as a literal.</span></span>

### <a name="setting-environment-variables"></a><span data-ttu-id="7ea3b-250">Környezeti változók beállítása</span><span class="sxs-lookup"><span data-stu-id="7ea3b-250">Setting environment variables</span></span>

<span data-ttu-id="7ea3b-251">Egy környezeti változó beállítása hajtja végre a következő utasítást:</span><span class="sxs-lookup"><span data-stu-id="7ea3b-251">Setting an environment variable is performed by the following statement:</span></span>

    VARIABLENAME=value

<span data-ttu-id="7ea3b-252">VARIABLENAME esetén annak a változónak a nevét.</span><span class="sxs-lookup"><span data-stu-id="7ea3b-252">Where VARIABLENAME is the name of the variable.</span></span> <span data-ttu-id="7ea3b-253">A változó elérésére, `$VARIABLENAME`.</span><span class="sxs-lookup"><span data-stu-id="7ea3b-253">To access the variable, use `$VARIABLENAME`.</span></span> <span data-ttu-id="7ea3b-254">Például egy jelszó nevű környezeti változó pozicionális paraméter által megadott értéket hozzárendelni használhatja a következő utasítást:</span><span class="sxs-lookup"><span data-stu-id="7ea3b-254">For example, to assign a value provided by a positional parameter as an environment variable named PASSWORD, you would use the following statement:</span></span>

    PASSWORD=$1

<span data-ttu-id="7ea3b-255">További információk hozzáférésének aztán használhatja `$PASSWORD`.</span><span class="sxs-lookup"><span data-stu-id="7ea3b-255">Subsequent access to the information could then use `$PASSWORD`.</span></span>

<span data-ttu-id="7ea3b-256">Környezeti változók a parancsfájl belül csak a parancsfájl hatókörén belül található.</span><span class="sxs-lookup"><span data-stu-id="7ea3b-256">Environment variables set within the script only exist within the scope of the script.</span></span> <span data-ttu-id="7ea3b-257">Egyes esetekben szükség lehet rendszerszintű környezeti változókat, amelyek megmaradnak a parancsfájl hozzáadása.</span><span class="sxs-lookup"><span data-stu-id="7ea3b-257">In some cases, you may need to add system-wide environment variables that will persist after the script has finished.</span></span> <span data-ttu-id="7ea3b-258">Adja hozzá a rendszerszintű környezeti változókat, vegye fel a változó `/etc/environment`.</span><span class="sxs-lookup"><span data-stu-id="7ea3b-258">To add system-wide environment variables, add the variable to `/etc/environment`.</span></span> <span data-ttu-id="7ea3b-259">Például a következő utasítás létrehozza `HADOOP_CONF_DIR`:</span><span class="sxs-lookup"><span data-stu-id="7ea3b-259">For example, the following statement adds `HADOOP_CONF_DIR`:</span></span>

```bash
echo "HADOOP_CONF_DIR=/etc/hadoop/conf" | sudo tee -a /etc/environment
```

### <a name="access-to-locations-where-the-custom-scripts-are-stored"></a><span data-ttu-id="7ea3b-260">Hozzáférés az egyéni parancsfájlok tároló helyekhez</span><span class="sxs-lookup"><span data-stu-id="7ea3b-260">Access to locations where the custom scripts are stored</span></span>

<span data-ttu-id="7ea3b-261">Egy fürt testreszabásához használt parancsfájlok kell tárolni az alábbi helyek egyikét:</span><span class="sxs-lookup"><span data-stu-id="7ea3b-261">Scripts used to customize a cluster needs to be stored in one of the following locations:</span></span>

* <span data-ttu-id="7ea3b-262">Egy __Azure Storage-fiók__ , amely kapcsolódik a fürthöz.</span><span class="sxs-lookup"><span data-stu-id="7ea3b-262">An __Azure Storage account__ that is associated with the cluster.</span></span>

* <span data-ttu-id="7ea3b-263">Egy __további tárfiók__ a fürthöz rendelt.</span><span class="sxs-lookup"><span data-stu-id="7ea3b-263">An __additional storage account__ associated with the cluster.</span></span>

* <span data-ttu-id="7ea3b-264">A __nyilvánosan olvasható URI__.</span><span class="sxs-lookup"><span data-stu-id="7ea3b-264">A __publicly readable URI__.</span></span> <span data-ttu-id="7ea3b-265">Például egy URL-címe a onedrive-on, Dropbox vagy más szolgáltatást tartalmazó fájl tárolt adatokat.</span><span class="sxs-lookup"><span data-stu-id="7ea3b-265">For example, a URL to data stored on OneDrive, Dropbox, or other file hosting service.</span></span>

* <span data-ttu-id="7ea3b-266">Egy __Azure Data Lake Store-fiók__ a HDInsight-fürthöz társított.</span><span class="sxs-lookup"><span data-stu-id="7ea3b-266">An __Azure Data Lake Store account__ that is associated with the HDInsight cluster.</span></span> <span data-ttu-id="7ea3b-267">Az Azure Data Lake Store és a HDInsight együttes használatával további információkért lásd: [HDInsight-fürtök létrehozása a Data Lake Store](../data-lake-store/data-lake-store-hdinsight-hadoop-use-portal.md).</span><span class="sxs-lookup"><span data-stu-id="7ea3b-267">For more information on using Azure Data Lake Store with HDInsight, see [Create an HDInsight cluster with Data Lake Store](../data-lake-store/data-lake-store-hdinsight-hadoop-use-portal.md).</span></span>

    > [!NOTE]
    > <span data-ttu-id="7ea3b-268">A HDInsight a Data Lake Store elérésére használt egyszerű a parancsfájl olvasási hozzáféréssel kell rendelkeznie.</span><span class="sxs-lookup"><span data-stu-id="7ea3b-268">The service principal HDInsight uses to access Data Lake Store must have read access to the script.</span></span>

<span data-ttu-id="7ea3b-269">A parancsfájl által használt erőforrások is nyilvánosan hozzáférhetőnek kell lenniük.</span><span class="sxs-lookup"><span data-stu-id="7ea3b-269">Resources used by the script must also be publicly available.</span></span>

<span data-ttu-id="7ea3b-270">A fájlok tárolására az Azure Storage-fiók vagy az Azure Data Lake Store biztosítja a gyors hozzáférést, mind az Azure-hálózatot.</span><span class="sxs-lookup"><span data-stu-id="7ea3b-270">Storing the files in an Azure Storage account or Azure Data Lake Store provides fast access, as both within the Azure network.</span></span>

> [!NOTE]
> <span data-ttu-id="7ea3b-271">A mutató hivatkozás a parancsfájl URI-formátum eltér attól függően, hogy a szolgáltatás használt.</span><span class="sxs-lookup"><span data-stu-id="7ea3b-271">The URI format used to reference the script differs depending on the service being used.</span></span> <span data-ttu-id="7ea3b-272">A HDInsight-fürthöz társított tárfiókokat, használjon `wasb://` vagy `wasbs://`.</span><span class="sxs-lookup"><span data-stu-id="7ea3b-272">For storage accounts associated with the HDInsight cluster, use `wasb://` or `wasbs://`.</span></span> <span data-ttu-id="7ea3b-273">Nyilvánosan olvasható URI-k használata `http://` vagy `https://`.</span><span class="sxs-lookup"><span data-stu-id="7ea3b-273">For publicly readable URIs, use `http://` or `https://`.</span></span> <span data-ttu-id="7ea3b-274">A Data Lake Store, használjon `adl://`.</span><span class="sxs-lookup"><span data-stu-id="7ea3b-274">For Data Lake Store, use `adl://`.</span></span>

### <a name="checking-the-operating-system-version"></a><span data-ttu-id="7ea3b-275">Az operációs rendszer verziójának ellenőrzése</span><span class="sxs-lookup"><span data-stu-id="7ea3b-275">Checking the operating system version</span></span>

<span data-ttu-id="7ea3b-276">HDInsight különböző verzióinak Ubuntu verzióról támaszkodnak.</span><span class="sxs-lookup"><span data-stu-id="7ea3b-276">Different versions of HDInsight rely on specific versions of Ubuntu.</span></span> <span data-ttu-id="7ea3b-277">Előfordulhat, hogy ki kell választani az parancsfájlban operációsrendszer-verziók közötti különbséget.</span><span class="sxs-lookup"><span data-stu-id="7ea3b-277">There may be differences between OS versions that you must check for in your script.</span></span> <span data-ttu-id="7ea3b-278">Szükség lehet például Ubuntu verziója kötődik bináris telepítéséhez.</span><span class="sxs-lookup"><span data-stu-id="7ea3b-278">For example, you may need to install a binary that is tied to the version of Ubuntu.</span></span>

<span data-ttu-id="7ea3b-279">Az operációs rendszer verzióját ellenőrzéséhez használja `lsb_release`.</span><span class="sxs-lookup"><span data-stu-id="7ea3b-279">To check the OS version, use `lsb_release`.</span></span> <span data-ttu-id="7ea3b-280">Például az alábbi parancsfájl bemutatja, hogyan kell hivatkoznia, egy adott bont fájlt az operációs rendszer verziójától függően:</span><span class="sxs-lookup"><span data-stu-id="7ea3b-280">For example, the following script demonstrates how to reference a specific tar file depending on the OS version:</span></span>

```bash
OS_VERSION=$(lsb_release -sr)
if [[ $OS_VERSION == 14* ]]; then
    echo "OS verion is $OS_VERSION. Using hue-binaries-14-04."
    HUE_TARFILE=hue-binaries-14-04.tgz
elif [[ $OS_VERSION == 16* ]]; then
    echo "OS verion is $OS_VERSION. Using hue-binaries-16-04."
    HUE_TARFILE=hue-binaries-16-04.tgz
fi
```

## <span data-ttu-id="7ea3b-281"><a name="deployScript"></a>Ellenőrzőlista a üzembe helyezéséhez egy parancsfájlművelet</span><span class="sxs-lookup"><span data-stu-id="7ea3b-281"><a name="deployScript"></a>Checklist for deploying a script action</span></span>

<span data-ttu-id="7ea3b-282">Azt a parancsfájlok telepítendő előkészítésekor tartott lépései a következők:</span><span class="sxs-lookup"><span data-stu-id="7ea3b-282">Here are the steps we took when preparing to deploy these scripts:</span></span>

* <span data-ttu-id="7ea3b-283">Helyezze el az egyéni parancsfájlok, amely elérhető a fürt csomópontjai a telepítés során helyen tartalmazó fájlokat.</span><span class="sxs-lookup"><span data-stu-id="7ea3b-283">Put the files that contain the custom scripts in a place that is accessible by the cluster nodes during deployment.</span></span> <span data-ttu-id="7ea3b-284">Például az alapértelmezett a fürt tárolóhelyét.</span><span class="sxs-lookup"><span data-stu-id="7ea3b-284">For example, the default storage for the cluster.</span></span> <span data-ttu-id="7ea3b-285">Fájlok nyilvánosan olvasható üzemeltetési szolgáltatásokat is tárolhatja.</span><span class="sxs-lookup"><span data-stu-id="7ea3b-285">Files can also be stored in publicly readable hosting services.</span></span>
* <span data-ttu-id="7ea3b-286">Győződjön meg arról, hogy a parancsfájl impotent.</span><span class="sxs-lookup"><span data-stu-id="7ea3b-286">Verify that the script is impotent.</span></span> <span data-ttu-id="7ea3b-287">Így lehetővé teszi, hogy a parancsfájl hajthatnak végre több alkalommal ugyanazon a csomóponton.</span><span class="sxs-lookup"><span data-stu-id="7ea3b-287">Doing so allows the script to be executed multiple times on the same node.</span></span>
* <span data-ttu-id="7ea3b-288">Egy ideiglenes könyvtár /tmp használatával a letöltött fájlok, parancsfájlok által használt, majd eltávolítással parancsfájlok rendelkezik végrehajtása után.</span><span class="sxs-lookup"><span data-stu-id="7ea3b-288">Use a temporary file directory /tmp to keep the downloaded files used by the scripts and then clean them up after scripts have executed.</span></span>
* <span data-ttu-id="7ea3b-289">Ha az operációs rendszer szintű beállításokat és a Hadoop szolgáltatás konfigurációs fájlokat, módosulnak, érdemes lehet HDInsight a szolgáltatások újraindítására.</span><span class="sxs-lookup"><span data-stu-id="7ea3b-289">If OS-level settings or Hadoop service configuration files are changed, you may want to restart HDInsight services.</span></span>

## <span data-ttu-id="7ea3b-290"><a name="runScriptAction"></a>A parancsfájlművelet futtatása</span><span class="sxs-lookup"><span data-stu-id="7ea3b-290"><a name="runScriptAction"></a>How to run a script action</span></span>

<span data-ttu-id="7ea3b-291">A Parancsfájlműveletek segítségével testre szabhatja a HDInsight-fürtök az alábbi módszerekkel:</span><span class="sxs-lookup"><span data-stu-id="7ea3b-291">You can use script actions to customize HDInsight clusters using the following methods:</span></span>

* <span data-ttu-id="7ea3b-292">Azure Portal</span><span class="sxs-lookup"><span data-stu-id="7ea3b-292">Azure portal</span></span>
* <span data-ttu-id="7ea3b-293">Azure PowerShell</span><span class="sxs-lookup"><span data-stu-id="7ea3b-293">Azure PowerShell</span></span>
* <span data-ttu-id="7ea3b-294">Azure Resource Manager-sablonok</span><span class="sxs-lookup"><span data-stu-id="7ea3b-294">Azure Resource Manager templates</span></span>
* <span data-ttu-id="7ea3b-295">A HDInsight .NET SDK.</span><span class="sxs-lookup"><span data-stu-id="7ea3b-295">The HDInsight .NET SDK.</span></span>

<span data-ttu-id="7ea3b-296">További információ az egyes módszerek használatával, lásd: [parancsfájlművelet használata](hdinsight-hadoop-customize-cluster-linux.md).</span><span class="sxs-lookup"><span data-stu-id="7ea3b-296">For more information on using each method, see [How to use script action](hdinsight-hadoop-customize-cluster-linux.md).</span></span>

## <span data-ttu-id="7ea3b-297"><a name="sampleScripts"></a>Egyéni parancsfájl-minták</span><span class="sxs-lookup"><span data-stu-id="7ea3b-297"><a name="sampleScripts"></a>Custom script samples</span></span>

<span data-ttu-id="7ea3b-298">A Microsoft mintaparancsfájlok összetevőinek telepítése HDInsight-fürtöt biztosít.</span><span class="sxs-lookup"><span data-stu-id="7ea3b-298">Microsoft provides sample scripts to install components on an HDInsight cluster.</span></span> <span data-ttu-id="7ea3b-299">Tekintse meg a következő hivatkozások további példa Parancsfájlműveletek.</span><span class="sxs-lookup"><span data-stu-id="7ea3b-299">See the following links for more example script actions.</span></span>

* [<span data-ttu-id="7ea3b-300">Telepítheti és használhatja a HDInsight-fürtök Hue</span><span class="sxs-lookup"><span data-stu-id="7ea3b-300">Install and use Hue on HDInsight clusters</span></span>](hdinsight-hadoop-hue-linux.md)
* [<span data-ttu-id="7ea3b-301">Telepítheti és használhatja a HDInsight-fürtök Solr</span><span class="sxs-lookup"><span data-stu-id="7ea3b-301">Install and use Solr on HDInsight clusters</span></span>](hdinsight-hadoop-solr-install-linux.md)
* [<span data-ttu-id="7ea3b-302">Telepítheti és használhatja a HDInsight-fürtök Giraph</span><span class="sxs-lookup"><span data-stu-id="7ea3b-302">Install and use Giraph on HDInsight clusters</span></span>](hdinsight-hadoop-giraph-install-linux.md)
* [<span data-ttu-id="7ea3b-303">Telepítéséhez vagy frissítéséhez monó a HDInsight-fürtökön</span><span class="sxs-lookup"><span data-stu-id="7ea3b-303">Install or upgrade Mono on HDInsight clusters</span></span>](hdinsight-hadoop-install-mono.md)

## <a name="troubleshooting"></a><span data-ttu-id="7ea3b-304">Hibaelhárítás</span><span class="sxs-lookup"><span data-stu-id="7ea3b-304">Troubleshooting</span></span>

<span data-ttu-id="7ea3b-305">Szert parancsfájlok használata során felmerülő hibák a következők:</span><span class="sxs-lookup"><span data-stu-id="7ea3b-305">The following are errors you may encounter when using scripts you have developed:</span></span>

<span data-ttu-id="7ea3b-306">**Hiba**: `$'\r': command not found`.</span><span class="sxs-lookup"><span data-stu-id="7ea3b-306">**Error**: `$'\r': command not found`.</span></span> <span data-ttu-id="7ea3b-307">Egyes esetekben követ `syntax error: unexpected end of file`.</span><span class="sxs-lookup"><span data-stu-id="7ea3b-307">Sometimes followed by `syntax error: unexpected end of file`.</span></span>

<span data-ttu-id="7ea3b-308">*OK*: ezt a hibát az okozza, ha egy parancsfájlban a sorok CRLF végződik.</span><span class="sxs-lookup"><span data-stu-id="7ea3b-308">*Cause*: This error is caused when the lines in a script end with CRLF.</span></span> <span data-ttu-id="7ea3b-309">UNIX rendszerek, a sor befejezési csak LF várt.</span><span class="sxs-lookup"><span data-stu-id="7ea3b-309">Unix systems expect only LF as the line ending.</span></span>

<span data-ttu-id="7ea3b-310">A probléma leggyakrabban esetén a parancsfájl állította össze a Windows-környezetben, mert CRLF egy közös sor a Windows sok szöveg szerkesztők befejezési.</span><span class="sxs-lookup"><span data-stu-id="7ea3b-310">This problem most often occurs when the script is authored on a Windows environment, as CRLF is a common line ending for many text editors on Windows.</span></span>

<span data-ttu-id="7ea3b-311">*Megoldási*: Ha egy beállítást a szövegszerkesztőben, Unix-formátum vagy kiválasztása LF a sor befejezése.</span><span class="sxs-lookup"><span data-stu-id="7ea3b-311">*Resolution*: If it is an option in your text editor, select Unix format or LF for the line ending.</span></span> <span data-ttu-id="7ea3b-312">A CRLF átállítása egy LF UNIX operációs rendszeren is használhat a következő parancsokat:</span><span class="sxs-lookup"><span data-stu-id="7ea3b-312">You may also use the following commands on a Unix system to change the CRLF to an LF:</span></span>

> [!NOTE]
> <span data-ttu-id="7ea3b-313">Az alábbi parancsokat is nagyjából megfelel, abban, hogy LF CRLF sorvégződések módosítsa azokat.</span><span class="sxs-lookup"><span data-stu-id="7ea3b-313">The following commands are roughly equivalent in that they should change the CRLF line endings to LF.</span></span> <span data-ttu-id="7ea3b-314">Válasszon ki egy, a rendszer segédprogramok alapján.</span><span class="sxs-lookup"><span data-stu-id="7ea3b-314">Select one based on the utilities available on your system.</span></span>

| <span data-ttu-id="7ea3b-315">Parancs</span><span class="sxs-lookup"><span data-stu-id="7ea3b-315">Command</span></span> | <span data-ttu-id="7ea3b-316">Megjegyzések</span><span class="sxs-lookup"><span data-stu-id="7ea3b-316">Notes</span></span> |
| --- | --- |
| `unix2dos -b INFILE` |<span data-ttu-id="7ea3b-317">Az eredeti fájl biztonsági mentése a egy. BAK bővítmény</span><span class="sxs-lookup"><span data-stu-id="7ea3b-317">The original file is backed up with a .BAK extension</span></span> |
| `tr -d '\r' < INFILE > OUTFILE` |<span data-ttu-id="7ea3b-318">EREDMÉNYFÁJL csak LF végződések javítása rendelkező verzióját tartalmazza.</span><span class="sxs-lookup"><span data-stu-id="7ea3b-318">OUTFILE contains a version with only LF endings</span></span> |
| `perl -pi -e 's/\r\n/\n/g' INFILE` | <span data-ttu-id="7ea3b-319">A fájlt közvetlenül módosítja</span><span class="sxs-lookup"><span data-stu-id="7ea3b-319">Modifies the file directly</span></span> |
| ```sed 's/$'"/`echo \\\r`/" INFILE > OUTFILE``` |<span data-ttu-id="7ea3b-320">EREDMÉNYFÁJL csak LF végződések javítása rendelkező verzióját tartalmazza.</span><span class="sxs-lookup"><span data-stu-id="7ea3b-320">OUTFILE contains a version with only LF endings.</span></span> |

<span data-ttu-id="7ea3b-321">**Hiba**: `line 1: #!/usr/bin/env: No such file or directory`.</span><span class="sxs-lookup"><span data-stu-id="7ea3b-321">**Error**: `line 1: #!/usr/bin/env: No such file or directory`.</span></span>

<span data-ttu-id="7ea3b-322">*OK*: A hiba akkor fordul elő, ha a parancsfájl UTF-8 a bájt rendelés be van jelölve (AJ) néven lett mentve.</span><span class="sxs-lookup"><span data-stu-id="7ea3b-322">*Cause*: This error occurs when the script was saved as UTF-8 with a Byte Order Mark (BOM).</span></span>

<span data-ttu-id="7ea3b-323">*Megoldási*: mentse a fájlt, ASCII vagy UTF-8 AJ nélkül.</span><span class="sxs-lookup"><span data-stu-id="7ea3b-323">*Resolution*: Save the file either as ASCII, or as UTF-8 without a BOM.</span></span> <span data-ttu-id="7ea3b-324">Az Anyagjegyzék nélkül fájlt létrehozni a Linux és Unix rendszeren is használhat a következő parancsot:</span><span class="sxs-lookup"><span data-stu-id="7ea3b-324">You may also use the following command on a Linux or Unix system to create a file without the BOM:</span></span>

    awk 'NR==1{sub(/^\xef\xbb\xbf/,"")}{print}' INFILE > OUTFILE

<span data-ttu-id="7ea3b-325">Cserélje le `INFILE` az Anyagjegyzék tartalmazó fájl.</span><span class="sxs-lookup"><span data-stu-id="7ea3b-325">Replace `INFILE` with the file containing the BOM.</span></span> <span data-ttu-id="7ea3b-326">`OUTFILE`egy új fájlnevet, amely tartalmazza a parancsfájl az Anyagjegyzék nélkül kell lennie.</span><span class="sxs-lookup"><span data-stu-id="7ea3b-326">`OUTFILE` should be a new file name, which contains the script without the BOM.</span></span>

## <span data-ttu-id="7ea3b-327"><a name="seeAlso"></a>Következő lépések</span><span class="sxs-lookup"><span data-stu-id="7ea3b-327"><a name="seeAlso"></a>Next steps</span></span>

* <span data-ttu-id="7ea3b-328">Megtudhatja, hogyan [testreszabása HDInsight-fürtök parancsfájlművelet használatával](hdinsight-hadoop-customize-cluster-linux.md)</span><span class="sxs-lookup"><span data-stu-id="7ea3b-328">Learn how to [Customize HDInsight clusters using script action](hdinsight-hadoop-customize-cluster-linux.md)</span></span>
* <span data-ttu-id="7ea3b-329">Használja a [HDInsight .NET SDK referenciáit](https://msdn.microsoft.com/library/mt271028.aspx) tudhat meg többet kezelése a HDInsight .NET-alkalmazások létrehozása</span><span class="sxs-lookup"><span data-stu-id="7ea3b-329">Use the [HDInsight .NET SDK reference](https://msdn.microsoft.com/library/mt271028.aspx) to learn more about creating .NET applications that manage HDInsight</span></span>
* <span data-ttu-id="7ea3b-330">Használja a [HDInsight REST API](https://msdn.microsoft.com/library/azure/mt622197.aspx) a többi használata a HDInsight-fürtökön felügyeleti műveletek elvégzéséhez.</span><span class="sxs-lookup"><span data-stu-id="7ea3b-330">Use the [HDInsight REST API](https://msdn.microsoft.com/library/azure/mt622197.aspx) to learn how to use REST to perform management actions on HDInsight clusters.</span></span>
