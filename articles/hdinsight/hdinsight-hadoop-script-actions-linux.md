---
title: "a Linux-alapú HDInsight - Azure aaaScript művelet fejlesztési |} Microsoft Docs"
description: "Ismerje meg, hogyan toouse Bash parancsfájlok toocustomize Linux-alapú HDInsight-fürtökön. hello parancsfájl művelet a HDInsight funkcióval toorun parancsfájlok során vagy a fürt létrehozása után. Parancsfájlok használt toochange fürtbeállítások és további szoftvereket telepíteni."
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
ms.openlocfilehash: 1f504b00365df5f4cfb3ae19ad55ff7630342650
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="script-action-development-with-hdinsight"></a><span data-ttu-id="4a1b0-105">Parancsfájl művelet fejlesztése a Hdinsighttal</span><span class="sxs-lookup"><span data-stu-id="4a1b0-105">Script action development with HDInsight</span></span>

<span data-ttu-id="4a1b0-106">Megtudhatja, hogyan toocustomize a HDInsight fürt segítségével Bash parancsfájlok.</span><span class="sxs-lookup"><span data-stu-id="4a1b0-106">Learn how toocustomize your HDInsight cluster using Bash scripts.</span></span> <span data-ttu-id="4a1b0-107">A Parancsfájlműveletek olyan egy módon toocustomize HDInsight alatt vagy a fürt létrehozása után.</span><span class="sxs-lookup"><span data-stu-id="4a1b0-107">Script actions are a way toocustomize HDInsight during or after cluster creation.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="4a1b0-108">hello jelen dokumentumban leírt lépések egy HDInsight-fürt által használt Linux igényelnek.</span><span class="sxs-lookup"><span data-stu-id="4a1b0-108">hello steps in this document require an HDInsight cluster that uses Linux.</span></span> <span data-ttu-id="4a1b0-109">Linux hello azt az egyetlen operációs rendszer, használja a HDInsight 3.4 vagy újabb verziója.</span><span class="sxs-lookup"><span data-stu-id="4a1b0-109">Linux is hello only operating system used on HDInsight version 3.4 or greater.</span></span> <span data-ttu-id="4a1b0-110">További tudnivalókért lásd: [A HDInsight elavulása Windows rendszeren](hdinsight-component-versioning.md#hdinsight-windows-retirement).</span><span class="sxs-lookup"><span data-stu-id="4a1b0-110">For more information, see [HDInsight retirement on Windows](hdinsight-component-versioning.md#hdinsight-windows-retirement).</span></span>

## <a name="what-are-script-actions"></a><span data-ttu-id="4a1b0-111">Mik azok a Parancsfájlműveletek</span><span class="sxs-lookup"><span data-stu-id="4a1b0-111">What are script actions</span></span>

<span data-ttu-id="4a1b0-112">A Parancsfájlműveletek a Bash parancsfájlok futó Azure hello a fürt csomópontjai toomake konfigurációjának módosításai, vagy szoftvereket telepíteni.</span><span class="sxs-lookup"><span data-stu-id="4a1b0-112">Script actions are Bash scripts that Azure runs on hello cluster nodes toomake configuration changes or install software.</span></span> <span data-ttu-id="4a1b0-113">A parancsfájlművelet legfelső szintű végrehajtásakor, és a jogok toohello fürtcsomópontok teljes hozzáférést biztosít.</span><span class="sxs-lookup"><span data-stu-id="4a1b0-113">A script action is executed as root, and provides full access rights toohello cluster nodes.</span></span>

<span data-ttu-id="4a1b0-114">A Parancsfájlműveletek hello a következő módszerek alkalmazhatók:</span><span class="sxs-lookup"><span data-stu-id="4a1b0-114">Script actions can be applied through hello following methods:</span></span>

| <span data-ttu-id="4a1b0-115">A metódus tooapply parancsfájl használata...</span><span class="sxs-lookup"><span data-stu-id="4a1b0-115">Use this method tooapply a script...</span></span> | <span data-ttu-id="4a1b0-116">Során fürt létrehozása...</span><span class="sxs-lookup"><span data-stu-id="4a1b0-116">During cluster creation...</span></span> | <span data-ttu-id="4a1b0-117">Egy futó fürtön...</span><span class="sxs-lookup"><span data-stu-id="4a1b0-117">On a running cluster...</span></span> |
| --- |:---:|:---:|
| <span data-ttu-id="4a1b0-118">Azure Portal</span><span class="sxs-lookup"><span data-stu-id="4a1b0-118">Azure portal</span></span> |<span data-ttu-id="4a1b0-119">✓</span><span class="sxs-lookup"><span data-stu-id="4a1b0-119">✓</span></span> |<span data-ttu-id="4a1b0-120">✓</span><span class="sxs-lookup"><span data-stu-id="4a1b0-120">✓</span></span> |
| <span data-ttu-id="4a1b0-121">Azure PowerShell</span><span class="sxs-lookup"><span data-stu-id="4a1b0-121">Azure PowerShell</span></span> |<span data-ttu-id="4a1b0-122">✓</span><span class="sxs-lookup"><span data-stu-id="4a1b0-122">✓</span></span> |<span data-ttu-id="4a1b0-123">✓</span><span class="sxs-lookup"><span data-stu-id="4a1b0-123">✓</span></span> |
| <span data-ttu-id="4a1b0-124">Azure CLI</span><span class="sxs-lookup"><span data-stu-id="4a1b0-124">Azure CLI</span></span> |&nbsp; |<span data-ttu-id="4a1b0-125">✓</span><span class="sxs-lookup"><span data-stu-id="4a1b0-125">✓</span></span> |
| <span data-ttu-id="4a1b0-126">HDInsight .NET SDK</span><span class="sxs-lookup"><span data-stu-id="4a1b0-126">HDInsight .NET SDK</span></span> |<span data-ttu-id="4a1b0-127">✓</span><span class="sxs-lookup"><span data-stu-id="4a1b0-127">✓</span></span> |<span data-ttu-id="4a1b0-128">✓</span><span class="sxs-lookup"><span data-stu-id="4a1b0-128">✓</span></span> |
| <span data-ttu-id="4a1b0-129">Az Azure Resource Manager-sablon</span><span class="sxs-lookup"><span data-stu-id="4a1b0-129">Azure Resource Manager Template</span></span> |<span data-ttu-id="4a1b0-130">✓</span><span class="sxs-lookup"><span data-stu-id="4a1b0-130">✓</span></span> |&nbsp; |

<span data-ttu-id="4a1b0-131">Ezen módszerek tooapply Parancsfájlműveletek használatával kapcsolatos további információkért lásd: [testreszabása HDInsight-fürtök Parancsfájlműveletek segítségével](hdinsight-hadoop-customize-cluster-linux.md).</span><span class="sxs-lookup"><span data-stu-id="4a1b0-131">For more information on using these methods tooapply script actions, see [Customize HDInsight clusters using script actions](hdinsight-hadoop-customize-cluster-linux.md).</span></span>

## <span data-ttu-id="4a1b0-132"><a name="bestPracticeScripting"></a>Parancsfájl fejlesztési ajánlott eljárásai</span><span class="sxs-lookup"><span data-stu-id="4a1b0-132"><a name="bestPracticeScripting"></a>Best practices for script development</span></span>

<span data-ttu-id="4a1b0-133">A HDInsight-fürtök egyéni parancsfájl fejlesztésekor van néhány ajánlott eljárások tookeep figyelembe vételével:</span><span class="sxs-lookup"><span data-stu-id="4a1b0-133">When you develop a custom script for an HDInsight cluster, there are several best practices tookeep in mind:</span></span>

* [<span data-ttu-id="4a1b0-134">Hello Hadoop célverzió</span><span class="sxs-lookup"><span data-stu-id="4a1b0-134">Target hello Hadoop version</span></span>](#bPS1)
* [<span data-ttu-id="4a1b0-135">Cél hello operációs rendszer verziója</span><span class="sxs-lookup"><span data-stu-id="4a1b0-135">Target hello OS Version</span></span>](#bps10)
* [<span data-ttu-id="4a1b0-136">Adja meg a stabil tooscript erőforrások hivatkozásokat tartalmaz.</span><span class="sxs-lookup"><span data-stu-id="4a1b0-136">Provide stable links tooscript resources</span></span>](#bPS2)
* [<span data-ttu-id="4a1b0-137">Előre lefordított erőforrások használata</span><span class="sxs-lookup"><span data-stu-id="4a1b0-137">Use pre-compiled resources</span></span>](#bPS4)
* [<span data-ttu-id="4a1b0-138">Győződjön meg arról, hogy hello fürt testreszabási parancsfájl idempotent</span><span class="sxs-lookup"><span data-stu-id="4a1b0-138">Ensure that hello cluster customization script is idempotent</span></span>](#bPS3)
* [<span data-ttu-id="4a1b0-139">Magas rendelkezésre állásának hello fürt architektúrája</span><span class="sxs-lookup"><span data-stu-id="4a1b0-139">Ensure high availability of hello cluster architecture</span></span>](#bPS5)
* [<span data-ttu-id="4a1b0-140">Hello egyéni összetevők toouse Azure Blob-tároló konfigurálása</span><span class="sxs-lookup"><span data-stu-id="4a1b0-140">Configure hello custom components toouse Azure Blob storage</span></span>](#bPS6)
* [<span data-ttu-id="4a1b0-141">Információ tooSTDOUT és az STDERR írása</span><span class="sxs-lookup"><span data-stu-id="4a1b0-141">Write information tooSTDOUT and STDERR</span></span>](#bPS7)
* [<span data-ttu-id="4a1b0-142">ASCII LF sorvégződések rendelkező fájlok mentése</span><span class="sxs-lookup"><span data-stu-id="4a1b0-142">Save files as ASCII with LF line endings</span></span>](#bps8)
* [<span data-ttu-id="4a1b0-143">Újrapróbálkozási logika toorecover átmeneti hibák használata</span><span class="sxs-lookup"><span data-stu-id="4a1b0-143">Use retry logic toorecover from transient errors</span></span>](#bps9)

> [!IMPORTANT]
> <span data-ttu-id="4a1b0-144">A Parancsfájlműveletek 60 percen belül kell végrehajtani, vagy hello folyamat sikertelen lesz.</span><span class="sxs-lookup"><span data-stu-id="4a1b0-144">Script actions must complete within 60 minutes or hello process fails.</span></span> <span data-ttu-id="4a1b0-145">Csomópont telepítése során, hello parancsfájl egyidejűleg más beállítás és konfiguráció folyamatok fut.</span><span class="sxs-lookup"><span data-stu-id="4a1b0-145">During node provisioning, hello script runs concurrently with other setup and configuration processes.</span></span> <span data-ttu-id="4a1b0-146">Erőforrások, például a CPU-idő vagy a hálózati sávszélesség erőforrásokért való versengés hello parancsfájl tootake hosszabb toofinish azonban nem a fejlesztési környezetet, mint okozhat.</span><span class="sxs-lookup"><span data-stu-id="4a1b0-146">Competition for resources such as CPU time or network bandwidth may cause hello script tootake longer toofinish than it does in your development environment.</span></span>

### <span data-ttu-id="4a1b0-147"><a name="bPS1"></a>Hello Hadoop célverzió</span><span class="sxs-lookup"><span data-stu-id="4a1b0-147"><a name="bPS1"></a>Target hello Hadoop version</span></span>

<span data-ttu-id="4a1b0-148">Különböző verzióit a HDInsight Hadoop-szolgáltatások és telepített összetevők különböző verziói működnek.</span><span class="sxs-lookup"><span data-stu-id="4a1b0-148">Different versions of HDInsight have different versions of Hadoop services and components installed.</span></span> <span data-ttu-id="4a1b0-149">Ha a parancsfájl egy szolgáltatás vagy összetevő adott verziójának vár, csak akkor ajánlott hello parancsfájl, amely tartalmazza a szükséges hello összetevők HDInsight hello verziójával.</span><span class="sxs-lookup"><span data-stu-id="4a1b0-149">If your script expects a specific version of a service or component, you should only use hello script with hello version of HDInsight that includes hello required components.</span></span> <span data-ttu-id="4a1b0-150">Találhat információkat tartalmazza a hdinsight eszközzel összetevő verzióin hello segítségével [HDInsight-összetevők verziószámozása](hdinsight-component-versioning.md) dokumentum.</span><span class="sxs-lookup"><span data-stu-id="4a1b0-150">You can find information on component versions included with HDInsight using hello [HDInsight component versioning](hdinsight-component-versioning.md) document.</span></span>

### <span data-ttu-id="4a1b0-151"><a name="bps10"></a>Cél hello az operációs rendszer verziója</span><span class="sxs-lookup"><span data-stu-id="4a1b0-151"><a name="bps10"></a> Target hello OS version</span></span>

<span data-ttu-id="4a1b0-152">Linux-alapú HDInsight hello Ubuntu Linux-disztribúció alapul.</span><span class="sxs-lookup"><span data-stu-id="4a1b0-152">Linux-based HDInsight is based on hello Ubuntu Linux distribution.</span></span> <span data-ttu-id="4a1b0-153">HDInsight különböző verziói különböző verzióinak Ubuntu, módosíthatja a parancsfájl működését támaszkodnak.</span><span class="sxs-lookup"><span data-stu-id="4a1b0-153">Different versions of HDInsight rely on different versions of Ubuntu, which may change how your script behaves.</span></span> <span data-ttu-id="4a1b0-154">HDInsight 3.4 és korábbi például Ubuntu verziók Upstart használó alapul.</span><span class="sxs-lookup"><span data-stu-id="4a1b0-154">For example, HDInsight 3.4 and earlier are based on Ubuntu versions that use Upstart.</span></span> <span data-ttu-id="4a1b0-155">3.5-ös verziója Ubuntu 16.04, amely használja Systemd alapul.</span><span class="sxs-lookup"><span data-stu-id="4a1b0-155">Version 3.5 is based on Ubuntu 16.04, which uses Systemd.</span></span> <span data-ttu-id="4a1b0-156">Systemd és Upstart támaszkodnak különböző parancsok, a parancsfájl mindkét toowork kell írni.</span><span class="sxs-lookup"><span data-stu-id="4a1b0-156">Systemd and Upstart rely on different commands, so your script should be written toowork with both.</span></span>

<span data-ttu-id="4a1b0-157">HDInsight 3.4 és 3.5-ös közötti másik fontos különbség, hogy `JAVA_HOME` tooJava 8 most mutat.</span><span class="sxs-lookup"><span data-stu-id="4a1b0-157">Another important difference between HDInsight 3.4 and 3.5 is that `JAVA_HOME` now points tooJava 8.</span></span>

<span data-ttu-id="4a1b0-158">Hello az operációs rendszer verzió ellenőrzéséhez hajtsa végre `lsb_release`.</span><span class="sxs-lookup"><span data-stu-id="4a1b0-158">You can check hello OS version by using `lsb_release`.</span></span> <span data-ttu-id="4a1b0-159">hello következő kód bemutatja, hogyan toodetermine Ha hello parancsfájl Ubuntu 14 vagy 16 fut:</span><span class="sxs-lookup"><span data-stu-id="4a1b0-159">hello following code demonstrates how toodetermine if hello script is running on Ubuntu 14 or 16:</span></span>

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

<span data-ttu-id="4a1b0-160">Ezek a következő https://hdiconfigactions.blob.core.windows.net/linuxhueconfigactionv02/install-hue-uber-v02.sh szövegrészek tartalmazó hello teljes parancsfájl található.</span><span class="sxs-lookup"><span data-stu-id="4a1b0-160">You can find hello full script that contains these snippets at https://hdiconfigactions.blob.core.windows.net/linuxhueconfigactionv02/install-hue-uber-v02.sh.</span></span>

<span data-ttu-id="4a1b0-161">A HDInsight által használt Ubuntu hello-verziójáért lásd: hello [HDInsight összetevő verziószáma](hdinsight-component-versioning.md) dokumentum.</span><span class="sxs-lookup"><span data-stu-id="4a1b0-161">For hello version of Ubuntu that is used by HDInsight, see hello [HDInsight component version](hdinsight-component-versioning.md) document.</span></span>

<span data-ttu-id="4a1b0-162">toounderstand hello Systemd és különbségei Upstart, lásd: [Systemd Upstart felhasználók](https://wiki.ubuntu.com/SystemdForUpstartUsers).</span><span class="sxs-lookup"><span data-stu-id="4a1b0-162">toounderstand hello differences between Systemd and Upstart, see [Systemd for Upstart users](https://wiki.ubuntu.com/SystemdForUpstartUsers).</span></span>

### <span data-ttu-id="4a1b0-163"><a name="bPS2"></a>Adja meg a stabil tooscript erőforrások hivatkozásokat tartalmaz.</span><span class="sxs-lookup"><span data-stu-id="4a1b0-163"><a name="bPS2"></a>Provide stable links tooscript resources</span></span>

<span data-ttu-id="4a1b0-164">hello parancsfájlt és a kapcsolódó erőforrások rendelkezésre kell állnia hello fürt hello élettartama során.</span><span class="sxs-lookup"><span data-stu-id="4a1b0-164">hello script and associated resources must remain available throughout hello lifetime of hello cluster.</span></span> <span data-ttu-id="4a1b0-165">Ezeket az erőforrásokat szükség, ha új csomóponttal egészülnek toohello fürt skálázás műveletek során.</span><span class="sxs-lookup"><span data-stu-id="4a1b0-165">These resources are required if new nodes are added toohello cluster during scaling operations.</span></span>

<span data-ttu-id="4a1b0-166">hello célszerű toodownload, és archiválja mindent az Azure Storage-fiók előfizetéséhez.</span><span class="sxs-lookup"><span data-stu-id="4a1b0-166">hello best practice is toodownload and archive everything in an Azure Storage account on your subscription.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="4a1b0-167">hello használt tárfiók hello alapértelmezett tárfiók hello fürt vagy egy nyilvános, csak olvasható tároló bármely más tárfiók kell lennie.</span><span class="sxs-lookup"><span data-stu-id="4a1b0-167">hello storage account used must be hello default storage account for hello cluster or a public, read-only container on any other storage account.</span></span>

<span data-ttu-id="4a1b0-168">Például a Microsoft által biztosított hello minták hello tárolódnak [https://hdiconfigactions.blob.core.windows.net/](https://hdiconfigactions.blob.core.windows.net/) tárfiók.</span><span class="sxs-lookup"><span data-stu-id="4a1b0-168">For example, hello samples provided by Microsoft are stored in hello [https://hdiconfigactions.blob.core.windows.net/](https://hdiconfigactions.blob.core.windows.net/) storage account.</span></span> <span data-ttu-id="4a1b0-169">Ez egy olyan nyilvános, csak olvasható tároló, hello HDInsight csapat tartja fenn.</span><span class="sxs-lookup"><span data-stu-id="4a1b0-169">This is a public, read-only container maintained by hello HDInsight team.</span></span>

### <span data-ttu-id="4a1b0-170"><a name="bPS4"></a>Előre lefordított erőforrások használata</span><span class="sxs-lookup"><span data-stu-id="4a1b0-170"><a name="bPS4"></a>Use pre-compiled resources</span></span>

<span data-ttu-id="4a1b0-171">tooreduce hello alkalommal vesz toorun hello parancsfájl, erőforrások a forráskód fordítási műveletek elkerülése érdekében.</span><span class="sxs-lookup"><span data-stu-id="4a1b0-171">tooreduce hello time it takes toorun hello script, avoid operations that compile resources from source code.</span></span> <span data-ttu-id="4a1b0-172">Például előre lefordítani az erőforrásokat, és tárolja őket az Azure Storage-fiók blobot a hello azonos adatközpontba, ahol a HDInsight.</span><span class="sxs-lookup"><span data-stu-id="4a1b0-172">For example, pre-compile resources and store them in an Azure Storage account blob in hello same data center as HDInsight.</span></span>

### <span data-ttu-id="4a1b0-173"><a name="bPS3"></a>Győződjön meg arról, hogy hello fürt testreszabási parancsfájl idempotent</span><span class="sxs-lookup"><span data-stu-id="4a1b0-173"><a name="bPS3"></a>Ensure that hello cluster customization script is idempotent</span></span>

<span data-ttu-id="4a1b0-174">Parancsfájlok idempotent kell lennie.</span><span class="sxs-lookup"><span data-stu-id="4a1b0-174">Scripts must be idempotent.</span></span> <span data-ttu-id="4a1b0-175">Ha a hello parancsfájl több alkalommal fut, az kell visszaadnia minden egyes állapot azonos fürt toohello hello.</span><span class="sxs-lookup"><span data-stu-id="4a1b0-175">If hello script runs multiple times, it should return hello cluster toohello same state every time.</span></span>

<span data-ttu-id="4a1b0-176">Például egy parancsfájlt, amely módosítja a konfigurációs fájlok ne adja hozzá duplikált bejegyzéseket Ha több alkalommal lefutott.</span><span class="sxs-lookup"><span data-stu-id="4a1b0-176">For example, a script that modifies configuration files should not add duplicate entries if ran multiple times.</span></span>

### <span data-ttu-id="4a1b0-177"><a name="bPS5"></a>Magas rendelkezésre állásának hello fürt architektúrája</span><span class="sxs-lookup"><span data-stu-id="4a1b0-177"><a name="bPS5"></a>Ensure high availability of hello cluster architecture</span></span>

<span data-ttu-id="4a1b0-178">Linux-alapú HDInsight-fürtök meg két átjárócsomópontokkal aktív hello fürtön belül, és Parancsfájlműveletek mindkét csomópontján futtatni.</span><span class="sxs-lookup"><span data-stu-id="4a1b0-178">Linux-based HDInsight clusters provide two head nodes that are active within hello cluster, and script actions run on both nodes.</span></span> <span data-ttu-id="4a1b0-179">Ha csak egy átjárócsomópont várható hello összetevők telepítése hello összetevők telepítésének mindkét központi csomópontján.</span><span class="sxs-lookup"><span data-stu-id="4a1b0-179">If hello components you install expect only one head node, do not install hello components on both head nodes.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="4a1b0-180">Szolgáltatások HDInsight részeként kialakított toofail keresztül történik hello két központi csomópontok között szükség szerint.</span><span class="sxs-lookup"><span data-stu-id="4a1b0-180">Services provided as part of HDInsight are designed toofail over between hello two head nodes as needed.</span></span> <span data-ttu-id="4a1b0-181">Ez a funkció nem hosszabbodik toocustom összetevők Parancsfájlműveletek segítségével telepíthető.</span><span class="sxs-lookup"><span data-stu-id="4a1b0-181">This functionality is not extended toocustom components installed through script actions.</span></span> <span data-ttu-id="4a1b0-182">Ha magas rendelkezésre állású egyéni összetevők, meg kell valósítani a saját feladatátvételi mechanizmus.</span><span class="sxs-lookup"><span data-stu-id="4a1b0-182">If you need high availability for custom components, you must implement your own failover mechanism.</span></span>

### <span data-ttu-id="4a1b0-183"><a name="bPS6"></a>Hello egyéni összetevők toouse Azure Blob-tároló konfigurálása</span><span class="sxs-lookup"><span data-stu-id="4a1b0-183"><a name="bPS6"></a>Configure hello custom components toouse Azure Blob storage</span></span>

<span data-ttu-id="4a1b0-184">Összetevőket telepít a hello fürt Hadoop elosztott fájlrendszerrel (HDFS) tárolást használ alapértelmezett konfigurációja lehet.</span><span class="sxs-lookup"><span data-stu-id="4a1b0-184">Components that you install on hello cluster might have a default configuration that uses Hadoop Distributed File System (HDFS) storage.</span></span> <span data-ttu-id="4a1b0-185">HDInsight hello alapértelmezett tárolóként használ az Azure Storage vagy a Data Lake Store.</span><span class="sxs-lookup"><span data-stu-id="4a1b0-185">HDInsight uses either Azure Storage or Data Lake Store as hello default storage.</span></span> <span data-ttu-id="4a1b0-186">Mindkét egy HDFS kompatibilis fájlrendszert biztosít, amely az adatok továbbra is fennáll, akkor is, ha hello fürtök törlése.</span><span class="sxs-lookup"><span data-stu-id="4a1b0-186">Both provide an HDFS compatible file system that persists data even if hello cluster is deleted.</span></span> <span data-ttu-id="4a1b0-187">Szükség lehet tooconfigure összetevők telepítése toouse WASB vagy ADL HDFS helyett.</span><span class="sxs-lookup"><span data-stu-id="4a1b0-187">You may need tooconfigure components you install toouse WASB or ADL instead of HDFS.</span></span>

<span data-ttu-id="4a1b0-188">A legtöbb műveleteket nem kell toospecify hello fájlrendszer.</span><span class="sxs-lookup"><span data-stu-id="4a1b0-188">For most operations, you do not need toospecify hello file system.</span></span> <span data-ttu-id="4a1b0-189">Például hello következő hello giraph-examples.jar fájlt átmásolja hello helyi rendszer toocluster fájltároló:</span><span class="sxs-lookup"><span data-stu-id="4a1b0-189">For example, hello following copies hello giraph-examples.jar file from hello local file system toocluster storage:</span></span>

```bash
hdfs dfs -put /usr/hdp/current/giraph/giraph-examples.jar /example/jars/
```

<span data-ttu-id="4a1b0-190">Ebben a példában hello `hdfs` parancs átlátható módon használja a hello alapértelmezett fürttároló.</span><span class="sxs-lookup"><span data-stu-id="4a1b0-190">In this example, hello `hdfs` command transparently uses hello default cluster storage.</span></span> <span data-ttu-id="4a1b0-191">Egyes műveletek esetében szükség lehet a toospecify hello URI.</span><span class="sxs-lookup"><span data-stu-id="4a1b0-191">For some operations, you may need toospecify hello URI.</span></span> <span data-ttu-id="4a1b0-192">Például `adl:///example/jars` a Data Lake Store vagy `wasb:///example/jars` az Azure Storage.</span><span class="sxs-lookup"><span data-stu-id="4a1b0-192">For example, `adl:///example/jars` for Data Lake Store or `wasb:///example/jars` for Azure Storage.</span></span>

### <span data-ttu-id="4a1b0-193"><a name="bPS7"></a>Információ tooSTDOUT és az STDERR írása</span><span class="sxs-lookup"><span data-stu-id="4a1b0-193"><a name="bPS7"></a>Write information tooSTDOUT and STDERR</span></span>

<span data-ttu-id="4a1b0-194">HDInsight parancsfájl kimenete, amely írásbeli tooSTDOUT és az STDERR naplók.</span><span class="sxs-lookup"><span data-stu-id="4a1b0-194">HDInsight logs script output that is written tooSTDOUT and STDERR.</span></span> <span data-ttu-id="4a1b0-195">Ezt az információt hello Ambari webes felhasználói felület használatával tekintheti meg.</span><span class="sxs-lookup"><span data-stu-id="4a1b0-195">You can view this information using hello Ambari web UI.</span></span>

> [!NOTE]
> <span data-ttu-id="4a1b0-196">Ambari csak akkor használható, ha hello fürt sikeresen létrehozva.</span><span class="sxs-lookup"><span data-stu-id="4a1b0-196">Ambari is only available if hello cluster is successfully created.</span></span> <span data-ttu-id="4a1b0-197">Ha Ön egy parancsfájlművelettel és a fürt létrehozása, létrehozása sikertelen, tekintse meg a hibaelhárítási szakaszában hello [testreszabása HDInsight-fürtök parancsfájlművelet használatával](hdinsight-hadoop-customize-cluster-linux.md#troubleshooting) a más módon történő a naplózott információk eléréséhez.</span><span class="sxs-lookup"><span data-stu-id="4a1b0-197">If you use a script action during cluster creation, and creation fails, see hello troubleshooting section [Customize HDInsight clusters using script action](hdinsight-hadoop-customize-cluster-linux.md#troubleshooting) for other ways of accessing logged information.</span></span>

<span data-ttu-id="4a1b0-198">A legtöbb segédprogramok és telepítőcsomagok már írási információk tooSTDOUT és az STDERR, azonban szükség lehet további naplózás tooadd.</span><span class="sxs-lookup"><span data-stu-id="4a1b0-198">Most utilities and installation packages already write information tooSTDOUT and STDERR, however you may want tooadd additional logging.</span></span> <span data-ttu-id="4a1b0-199">toosend szöveg tooSTDOUT, használjon `echo`.</span><span class="sxs-lookup"><span data-stu-id="4a1b0-199">toosend text tooSTDOUT, use `echo`.</span></span> <span data-ttu-id="4a1b0-200">Példa:</span><span class="sxs-lookup"><span data-stu-id="4a1b0-200">For example:</span></span>

```bash
echo "Getting ready tooinstall Foo"
```

<span data-ttu-id="4a1b0-201">Alapértelmezés szerint `echo` küld hello karakterlánc tooSTDOUT.</span><span class="sxs-lookup"><span data-stu-id="4a1b0-201">By default, `echo` sends hello string tooSTDOUT.</span></span> <span data-ttu-id="4a1b0-202">toodirect azt tooSTDERR, adja hozzá `>&2` előtt `echo`.</span><span class="sxs-lookup"><span data-stu-id="4a1b0-202">toodirect it tooSTDERR, add `>&2` before `echo`.</span></span> <span data-ttu-id="4a1b0-203">Példa:</span><span class="sxs-lookup"><span data-stu-id="4a1b0-203">For example:</span></span>

```bash
>&2 echo "An error occurred installing Foo"
```

<span data-ttu-id="4a1b0-204">Ez a tooSTDOUT tooSTDERR (2) helyette írt adatok irányítja át.</span><span class="sxs-lookup"><span data-stu-id="4a1b0-204">This redirects information written tooSTDOUT tooSTDERR (2) instead.</span></span> <span data-ttu-id="4a1b0-205">IO átirányítással kapcsolatos további információkért lásd: [http://www.tldp.org/LDP/abs/html/io-redirection.html](http://www.tldp.org/LDP/abs/html/io-redirection.html).</span><span class="sxs-lookup"><span data-stu-id="4a1b0-205">For more information on IO redirection, see [http://www.tldp.org/LDP/abs/html/io-redirection.html](http://www.tldp.org/LDP/abs/html/io-redirection.html).</span></span>

<span data-ttu-id="4a1b0-206">A Parancsfájlműveletek által naplózott adatok megtekintéséhez használatos időkategóriát további információkért lásd: [testreszabása HDInsight-fürtök parancsfájlművelet használatával](hdinsight-hadoop-customize-cluster-linux.md#troubleshooting)</span><span class="sxs-lookup"><span data-stu-id="4a1b0-206">For more information on viewing information logged by script actions, see [Customize HDInsight clusters using script action](hdinsight-hadoop-customize-cluster-linux.md#troubleshooting)</span></span>

### <span data-ttu-id="4a1b0-207"><a name="bps8"></a>ASCII LF sorvégződések rendelkező fájlok mentése</span><span class="sxs-lookup"><span data-stu-id="4a1b0-207"><a name="bps8"></a> Save files as ASCII with LF line endings</span></span>

<span data-ttu-id="4a1b0-208">Bash parancsfájlok megszakította a LF vonallal ASCII formátumban kell tárolni.</span><span class="sxs-lookup"><span data-stu-id="4a1b0-208">Bash scripts should be stored as ASCII format, with lines terminated by LF.</span></span> <span data-ttu-id="4a1b0-209">UTF-8 tárolják, vagy használja CRLF hello sor befejezése sikertelen lehet a következő hiba hello fájlok:</span><span class="sxs-lookup"><span data-stu-id="4a1b0-209">Files that are stored as UTF-8, or use CRLF as hello line ending may fail with hello following error:</span></span>

```
$'\r': command not found
line 1: #!/usr/bin/env: No such file or directory
```

### <span data-ttu-id="4a1b0-210"><a name="bps9"></a>Újrapróbálkozási logika toorecover átmeneti hibák használata</span><span class="sxs-lookup"><span data-stu-id="4a1b0-210"><a name="bps9"></a> Use retry logic toorecover from transient errors</span></span>

<span data-ttu-id="4a1b0-211">Fájlok letöltése apt get vagy egyéb műveleteket adatátvitelhez csomagok telepítése hello internet, hello művelet tootransient hálózati hibák miatt sikertelenek lehetnek.</span><span class="sxs-lookup"><span data-stu-id="4a1b0-211">When downloading files, installing packages using apt-get, or other actions that transmit data over hello internet, hello action may fail due tootransient networking errors.</span></span> <span data-ttu-id="4a1b0-212">Például hello távoli erőforrás kommunikációra lehet hello folyamat sikertelen tooa biztonsági mentési csomópont.</span><span class="sxs-lookup"><span data-stu-id="4a1b0-212">For example, hello remote resource you are communicating with may be in hello process of failing over tooa backup node.</span></span>

<span data-ttu-id="4a1b0-213">toomake a parancsfájlhibák rugalmas tootransient újrapróbálkozási logikát Megvalósíthat.</span><span class="sxs-lookup"><span data-stu-id="4a1b0-213">toomake your script resilient tootransient errors, you can implement retry logic.</span></span> <span data-ttu-id="4a1b0-214">a következő függvény hello bemutatja, hogyan tooimplement újrapróbálkozási logika.</span><span class="sxs-lookup"><span data-stu-id="4a1b0-214">hello following function demonstrates how tooimplement retry logic.</span></span> <span data-ttu-id="4a1b0-215">Újbóli próbálkozások háromszor hello műveletet.</span><span class="sxs-lookup"><span data-stu-id="4a1b0-215">It retries hello operation three times before failing.</span></span>

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

<span data-ttu-id="4a1b0-216">hello alábbi példák bemutatják, hogyan toouse ezt a funkciót.</span><span class="sxs-lookup"><span data-stu-id="4a1b0-216">hello following examples demonstrate how toouse this function.</span></span>

```bash
retry ls -ltr foo

retry wget -O ./tmpfile.sh https://hdiconfigactions.blob.core.windows.net/linuxhueconfigactionv02/install-hue-uber-v02.sh
```

## <span data-ttu-id="4a1b0-217"><a name="helpermethods"></a>Egyéni parancsfájlok segítő módszerei</span><span class="sxs-lookup"><span data-stu-id="4a1b0-217"><a name="helpermethods"></a>Helper methods for custom scripts</span></span>

<span data-ttu-id="4a1b0-218">Parancsfájl művelet segítő módszereket segédprogramok egyéni parancsfájlok írása közben használható.</span><span class="sxs-lookup"><span data-stu-id="4a1b0-218">Script action helper methods are utilities that you can use while writing custom scripts.</span></span> <span data-ttu-id="4a1b0-219">Ezek a módszerek tartalmazza a[https://hdiconfigactions.blob.core.windows.net/linuxconfigactionmodulev01/HDInsightUtilities-v01.sh](https://hdiconfigactions.blob.core.windows.net/linuxconfigactionmodulev01/HDInsightUtilities-v01.sh) parancsfájl.</span><span class="sxs-lookup"><span data-stu-id="4a1b0-219">These methods are contained in the[https://hdiconfigactions.blob.core.windows.net/linuxconfigactionmodulev01/HDInsightUtilities-v01.sh](https://hdiconfigactions.blob.core.windows.net/linuxconfigactionmodulev01/HDInsightUtilities-v01.sh) script.</span></span> <span data-ttu-id="4a1b0-220">A következő toodownload hello használata, és felhasználja a parancsfájl részeként:</span><span class="sxs-lookup"><span data-stu-id="4a1b0-220">Use hello following toodownload and use them as part of your script:</span></span>

```bash
# Import hello helper method module.
wget -O /tmp/HDInsightUtilities-v01.sh -q https://hdiconfigactions.blob.core.windows.net/linuxconfigactionmodulev01/HDInsightUtilities-v01.sh && source /tmp/HDInsightUtilities-v01.sh && rm -f /tmp/HDInsightUtilities-v01.sh
```

<span data-ttu-id="4a1b0-221">a következő parancsfájlban használható segítő hello:</span><span class="sxs-lookup"><span data-stu-id="4a1b0-221">hello following helpers available for use in your script:</span></span>

| <span data-ttu-id="4a1b0-222">Segítő kihasználtsága</span><span class="sxs-lookup"><span data-stu-id="4a1b0-222">Helper usage</span></span> | <span data-ttu-id="4a1b0-223">Leírás</span><span class="sxs-lookup"><span data-stu-id="4a1b0-223">Description</span></span> |
| --- | --- |
| `download_file SOURCEURL DESTFILEPATH [OVERWRITE]` |<span data-ttu-id="4a1b0-224">Letölti a fájlt a hello forrás URI toohello fájl megadott elérési útja.</span><span class="sxs-lookup"><span data-stu-id="4a1b0-224">Downloads a file from hello source URI toohello specified file path.</span></span> <span data-ttu-id="4a1b0-225">Alapértelmezés szerint azt a meglévő fájl felülírásának nem.</span><span class="sxs-lookup"><span data-stu-id="4a1b0-225">By default, it does not overwrite an existing file.</span></span> |
| `untar_file TARFILE DESTDIR` |<span data-ttu-id="4a1b0-226">Kibontja a bont (használatával `-xf`) toohello célkönyvtáron.</span><span class="sxs-lookup"><span data-stu-id="4a1b0-226">Extracts a tar file (using `-xf`) toohello destination directory.</span></span> |
| `test_is_headnode` |<span data-ttu-id="4a1b0-227">Ha a fürt átjárócsomópontjához, visszatérési 1; itt futott Ellenkező esetben 0.</span><span class="sxs-lookup"><span data-stu-id="4a1b0-227">If ran on a cluster head node, return 1; otherwise, 0.</span></span> |
| `test_is_datanode` |<span data-ttu-id="4a1b0-228">Ha hello aktuális csomópont adatcsomóponton (munkavégző), a visszatérési érték egy 1. Ellenkező esetben 0.</span><span class="sxs-lookup"><span data-stu-id="4a1b0-228">If hello current node is a data (worker) node, return a 1; otherwise, 0.</span></span> |
| `test_is_first_datanode` |<span data-ttu-id="4a1b0-229">Ha hello aktuális csomópont hello első adatok (munkavégző) csomópontra (elnevezett workernode0) adja vissza egy 1. Ellenkező esetben 0.</span><span class="sxs-lookup"><span data-stu-id="4a1b0-229">If hello current node is hello first data (worker) node (named workernode0) return a 1; otherwise, 0.</span></span> |
| `get_headnodes` |<span data-ttu-id="4a1b0-230">Teljesen minősített tartománynevét hello hello headnodes hello fürt adja vissza.</span><span class="sxs-lookup"><span data-stu-id="4a1b0-230">Return hello fully qualified domain name of hello headnodes in hello cluster.</span></span> <span data-ttu-id="4a1b0-231">Neveket vesszővel tagolt.</span><span class="sxs-lookup"><span data-stu-id="4a1b0-231">Names are comma delimited.</span></span> <span data-ttu-id="4a1b0-232">Üres karakterlánc hibát akkor adja vissza.</span><span class="sxs-lookup"><span data-stu-id="4a1b0-232">An empty string is returned on error.</span></span> |
| `get_primary_headnode` |<span data-ttu-id="4a1b0-233">Lekérdezi a teljesen minősített tartománynevét hello hello elsődleges headnode.</span><span class="sxs-lookup"><span data-stu-id="4a1b0-233">Gets hello fully qualified domain name of hello primary headnode.</span></span> <span data-ttu-id="4a1b0-234">Üres karakterlánc hibát akkor adja vissza.</span><span class="sxs-lookup"><span data-stu-id="4a1b0-234">An empty string is returned on error.</span></span> |
| `get_secondary_headnode` |<span data-ttu-id="4a1b0-235">Lekérdezi a teljesen minősített tartománynevét hello hello másodlagos headnode.</span><span class="sxs-lookup"><span data-stu-id="4a1b0-235">Gets hello fully qualified domain name of hello secondary headnode.</span></span> <span data-ttu-id="4a1b0-236">Üres karakterlánc hibát akkor adja vissza.</span><span class="sxs-lookup"><span data-stu-id="4a1b0-236">An empty string is returned on error.</span></span> |
| `get_primary_headnode_number` |<span data-ttu-id="4a1b0-237">Lekérdezi a hello elsődleges headnode hello-utótagként.</span><span class="sxs-lookup"><span data-stu-id="4a1b0-237">Gets hello numeric suffix of hello primary headnode.</span></span> <span data-ttu-id="4a1b0-238">Üres karakterlánc hibát akkor adja vissza.</span><span class="sxs-lookup"><span data-stu-id="4a1b0-238">An empty string is returned on error.</span></span> |
| `get_secondary_headnode_number` |<span data-ttu-id="4a1b0-239">Lekérdezi a hello másodlagos headnode hello-utótagként.</span><span class="sxs-lookup"><span data-stu-id="4a1b0-239">Gets hello numeric suffix of hello secondary headnode.</span></span> <span data-ttu-id="4a1b0-240">Üres karakterlánc hibát akkor adja vissza.</span><span class="sxs-lookup"><span data-stu-id="4a1b0-240">An empty string is returned on error.</span></span> |

## <span data-ttu-id="4a1b0-241"><a name="commonusage"></a>Gyakori használati minták</span><span class="sxs-lookup"><span data-stu-id="4a1b0-241"><a name="commonusage"></a>Common usage patterns</span></span>

<span data-ttu-id="4a1b0-242">Ez a szakasz néhány hello gyakori használati szokásokról, amelyek a saját egyéni parancsfájl írásakor mutatjuk be végrehajtási nyújt útmutatást.</span><span class="sxs-lookup"><span data-stu-id="4a1b0-242">This section provides guidance on implementing some of hello common usage patterns that you might run into while writing your own custom script.</span></span>

### <a name="passing-parameters-tooa-script"></a><span data-ttu-id="4a1b0-243">Paraméterek átadása tooa parancsfájl</span><span class="sxs-lookup"><span data-stu-id="4a1b0-243">Passing parameters tooa script</span></span>

<span data-ttu-id="4a1b0-244">Bizonyos esetekben a parancsfájl paramétereit lehet szükség.</span><span class="sxs-lookup"><span data-stu-id="4a1b0-244">In some cases, your script may require parameters.</span></span> <span data-ttu-id="4a1b0-245">Például szükség lehet hello rendszergazdai jelszó hello fürt hello Ambari REST API használata esetén.</span><span class="sxs-lookup"><span data-stu-id="4a1b0-245">For example, you may need hello admin password for hello cluster when using hello Ambari REST API.</span></span>

<span data-ttu-id="4a1b0-246">Toohello parancsfájl átadott paraméterek nevezik *pozícióparaméterek*, és túl rendelt`$1` hello első paraméternek `$2` a második, és így a hello.</span><span class="sxs-lookup"><span data-stu-id="4a1b0-246">Parameters passed toohello script are known as *positional parameters*, and are assigned too`$1` for hello first parameter, `$2` for hello second, and so-on.</span></span> <span data-ttu-id="4a1b0-247">`$0`hello parancsfájl önmagában hello nevét tartalmazza.</span><span class="sxs-lookup"><span data-stu-id="4a1b0-247">`$0` contains hello name of hello script itself.</span></span>

<span data-ttu-id="4a1b0-248">Értékek toohello parancsfájl paraméterként szimpla idézőjelben (') közé kell tenni.</span><span class="sxs-lookup"><span data-stu-id="4a1b0-248">Values passed toohello script as parameters should be enclosed by single quotes (').</span></span> <span data-ttu-id="4a1b0-249">Ezzel biztosítja, hogy az átadott érték hello literálként kell kezelni.</span><span class="sxs-lookup"><span data-stu-id="4a1b0-249">Doing so ensures that hello passed value is treated as a literal.</span></span>

### <a name="setting-environment-variables"></a><span data-ttu-id="4a1b0-250">Környezeti változók beállítása</span><span class="sxs-lookup"><span data-stu-id="4a1b0-250">Setting environment variables</span></span>

<span data-ttu-id="4a1b0-251">Egy környezeti változó beállítása a következő utasítás hello végzi el:</span><span class="sxs-lookup"><span data-stu-id="4a1b0-251">Setting an environment variable is performed by hello following statement:</span></span>

    VARIABLENAME=value

<span data-ttu-id="4a1b0-252">Ahol VARIABLENAME az hello hello változó neve.</span><span class="sxs-lookup"><span data-stu-id="4a1b0-252">Where VARIABLENAME is hello name of hello variable.</span></span> <span data-ttu-id="4a1b0-253">tooaccess hello változó, használjon `$VARIABLENAME`.</span><span class="sxs-lookup"><span data-stu-id="4a1b0-253">tooaccess hello variable, use `$VARIABLENAME`.</span></span> <span data-ttu-id="4a1b0-254">Például tooassign egy pozícióparaméter, egy környezeti változó által megadott nevű jelszó, a következő utasítás hello szeretné használni:</span><span class="sxs-lookup"><span data-stu-id="4a1b0-254">For example, tooassign a value provided by a positional parameter as an environment variable named PASSWORD, you would use hello following statement:</span></span>

    PASSWORD=$1

<span data-ttu-id="4a1b0-255">További hozzáférési toohello adatait aztán használhatja `$PASSWORD`.</span><span class="sxs-lookup"><span data-stu-id="4a1b0-255">Subsequent access toohello information could then use `$PASSWORD`.</span></span>

<span data-ttu-id="4a1b0-256">Környezeti változók hello parancsfájl belül csak hello parancsfájl hello hatókörén belül található.</span><span class="sxs-lookup"><span data-stu-id="4a1b0-256">Environment variables set within hello script only exist within hello scope of hello script.</span></span> <span data-ttu-id="4a1b0-257">Egyes esetekben szükség lehet a tooadd rendszerszintű környezeti változókat, amelyek hello parancsfájl áll fenn.</span><span class="sxs-lookup"><span data-stu-id="4a1b0-257">In some cases, you may need tooadd system-wide environment variables that will persist after hello script has finished.</span></span> <span data-ttu-id="4a1b0-258">tooadd rendszerszintű környezeti változókat, adjon hozzá hello változó túl`/etc/environment`.</span><span class="sxs-lookup"><span data-stu-id="4a1b0-258">tooadd system-wide environment variables, add hello variable too`/etc/environment`.</span></span> <span data-ttu-id="4a1b0-259">Például a következő utasítás hello létrehozza `HADOOP_CONF_DIR`:</span><span class="sxs-lookup"><span data-stu-id="4a1b0-259">For example, hello following statement adds `HADOOP_CONF_DIR`:</span></span>

```bash
echo "HADOOP_CONF_DIR=/etc/hadoop/conf" | sudo tee -a /etc/environment
```

### <a name="access-toolocations-where-hello-custom-scripts-are-stored"></a><span data-ttu-id="4a1b0-260">Hozzáférés toolocations hello egyéni parancsfájlok tároló</span><span class="sxs-lookup"><span data-stu-id="4a1b0-260">Access toolocations where hello custom scripts are stored</span></span>

<span data-ttu-id="4a1b0-261">Használt toocustomize fürt kell toobe tárolva hello alábbi helyek egyikét:</span><span class="sxs-lookup"><span data-stu-id="4a1b0-261">Scripts used toocustomize a cluster needs toobe stored in one of hello following locations:</span></span>

* <span data-ttu-id="4a1b0-262">Egy __Azure Storage-fiók__ hello fürt társított.</span><span class="sxs-lookup"><span data-stu-id="4a1b0-262">An __Azure Storage account__ that is associated with hello cluster.</span></span>

* <span data-ttu-id="4a1b0-263">Egy __további tárfiók__ hello-fürthöz tartozó.</span><span class="sxs-lookup"><span data-stu-id="4a1b0-263">An __additional storage account__ associated with hello cluster.</span></span>

* <span data-ttu-id="4a1b0-264">A __nyilvánosan olvasható URI__.</span><span class="sxs-lookup"><span data-stu-id="4a1b0-264">A __publicly readable URI__.</span></span> <span data-ttu-id="4a1b0-265">Például egy URL-cím toodata a onedrive vállalati verzió, a Dropbox vagy más szolgáltatást tartalmazó fájl tárolja.</span><span class="sxs-lookup"><span data-stu-id="4a1b0-265">For example, a URL toodata stored on OneDrive, Dropbox, or other file hosting service.</span></span>

* <span data-ttu-id="4a1b0-266">Egy __Azure Data Lake Store-fiók__ hello HDInsight-fürthöz társított.</span><span class="sxs-lookup"><span data-stu-id="4a1b0-266">An __Azure Data Lake Store account__ that is associated with hello HDInsight cluster.</span></span> <span data-ttu-id="4a1b0-267">Az Azure Data Lake Store és a HDInsight együttes használatával további információkért lásd: [HDInsight-fürtök létrehozása a Data Lake Store](../data-lake-store/data-lake-store-hdinsight-hadoop-use-portal.md).</span><span class="sxs-lookup"><span data-stu-id="4a1b0-267">For more information on using Azure Data Lake Store with HDInsight, see [Create an HDInsight cluster with Data Lake Store](../data-lake-store/data-lake-store-hdinsight-hadoop-use-portal.md).</span></span>

    > [!NOTE]
    > <span data-ttu-id="4a1b0-268">hello szolgáltatás egyszerű HDInsight használ tooaccess Data Lake Store toohello parancsfájl olvasási hozzáféréssel kell rendelkeznie.</span><span class="sxs-lookup"><span data-stu-id="4a1b0-268">hello service principal HDInsight uses tooaccess Data Lake Store must have read access toohello script.</span></span>

<span data-ttu-id="4a1b0-269">Hello parancsfájlhoz használt erőforrásokhoz is nyilvánosan hozzáférhetőnek kell lenniük.</span><span class="sxs-lookup"><span data-stu-id="4a1b0-269">Resources used by hello script must also be publicly available.</span></span>

<span data-ttu-id="4a1b0-270">Hello fájlok tárolására az Azure Storage-fiók vagy az Azure Data Lake Store gyors hozzáférést, belül hello Azure-hálózatot is biztosít.</span><span class="sxs-lookup"><span data-stu-id="4a1b0-270">Storing hello files in an Azure Storage account or Azure Data Lake Store provides fast access, as both within hello Azure network.</span></span>

> [!NOTE]
> <span data-ttu-id="4a1b0-271">hello URI formátumának tooreference hello parancsfájl abban különbözik attól függően, hogy hello szolgáltatást használja.</span><span class="sxs-lookup"><span data-stu-id="4a1b0-271">hello URI format used tooreference hello script differs depending on hello service being used.</span></span> <span data-ttu-id="4a1b0-272">Hello HDInsight-fürthöz társított tárfiókokat, használjon `wasb://` vagy `wasbs://`.</span><span class="sxs-lookup"><span data-stu-id="4a1b0-272">For storage accounts associated with hello HDInsight cluster, use `wasb://` or `wasbs://`.</span></span> <span data-ttu-id="4a1b0-273">Nyilvánosan olvasható URI-k használata `http://` vagy `https://`.</span><span class="sxs-lookup"><span data-stu-id="4a1b0-273">For publicly readable URIs, use `http://` or `https://`.</span></span> <span data-ttu-id="4a1b0-274">A Data Lake Store, használjon `adl://`.</span><span class="sxs-lookup"><span data-stu-id="4a1b0-274">For Data Lake Store, use `adl://`.</span></span>

### <a name="checking-hello-operating-system-version"></a><span data-ttu-id="4a1b0-275">Hello operációs rendszer verziójának ellenőrzése</span><span class="sxs-lookup"><span data-stu-id="4a1b0-275">Checking hello operating system version</span></span>

<span data-ttu-id="4a1b0-276">HDInsight különböző verzióinak Ubuntu verzióról támaszkodnak.</span><span class="sxs-lookup"><span data-stu-id="4a1b0-276">Different versions of HDInsight rely on specific versions of Ubuntu.</span></span> <span data-ttu-id="4a1b0-277">Előfordulhat, hogy ki kell választani az parancsfájlban operációsrendszer-verziók közötti különbséget.</span><span class="sxs-lookup"><span data-stu-id="4a1b0-277">There may be differences between OS versions that you must check for in your script.</span></span> <span data-ttu-id="4a1b0-278">Ubuntu kapcsolt toohello verziója bináris tooinstall például szükség lehet.</span><span class="sxs-lookup"><span data-stu-id="4a1b0-278">For example, you may need tooinstall a binary that is tied toohello version of Ubuntu.</span></span>

<span data-ttu-id="4a1b0-279">toocheck hello az operációs rendszer verziója, használjon `lsb_release`.</span><span class="sxs-lookup"><span data-stu-id="4a1b0-279">toocheck hello OS version, use `lsb_release`.</span></span> <span data-ttu-id="4a1b0-280">Például a következő parancsfájl hello bemutatja, hogyan tooreference egy adott bont fájl attól függően, hogy az operációs rendszer verziója hello:</span><span class="sxs-lookup"><span data-stu-id="4a1b0-280">For example, hello following script demonstrates how tooreference a specific tar file depending on hello OS version:</span></span>

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

## <span data-ttu-id="4a1b0-281"><a name="deployScript"></a>Ellenőrzőlista a üzembe helyezéséhez egy parancsfájlművelet</span><span class="sxs-lookup"><span data-stu-id="4a1b0-281"><a name="deployScript"></a>Checklist for deploying a script action</span></span>

<span data-ttu-id="4a1b0-282">Az alábbiakban azt tartott, ezek a parancsfájlok toodeploy előkészítésekor hello lépéseket:</span><span class="sxs-lookup"><span data-stu-id="4a1b0-282">Here are hello steps we took when preparing toodeploy these scripts:</span></span>

* <span data-ttu-id="4a1b0-283">Hello egyéni parancsfájlok, amely elérhető a hello fürtcsomópontok üzembe helyezése során helyen tartalmazó hello fájlokat helyezze el.</span><span class="sxs-lookup"><span data-stu-id="4a1b0-283">Put hello files that contain hello custom scripts in a place that is accessible by hello cluster nodes during deployment.</span></span> <span data-ttu-id="4a1b0-284">Például hello alapértelmezett hello fürt tárolóhelyét.</span><span class="sxs-lookup"><span data-stu-id="4a1b0-284">For example, hello default storage for hello cluster.</span></span> <span data-ttu-id="4a1b0-285">Fájlok nyilvánosan olvasható üzemeltetési szolgáltatásokat is tárolhatja.</span><span class="sxs-lookup"><span data-stu-id="4a1b0-285">Files can also be stored in publicly readable hosting services.</span></span>
* <span data-ttu-id="4a1b0-286">Győződjön meg arról, hogy impotent hello parancsfájl.</span><span class="sxs-lookup"><span data-stu-id="4a1b0-286">Verify that hello script is impotent.</span></span> <span data-ttu-id="4a1b0-287">Így lehetővé teszi, hogy hello parancsfájl toobe végre több alkalommal hello azonos csomópont.</span><span class="sxs-lookup"><span data-stu-id="4a1b0-287">Doing so allows hello script toobe executed multiple times on hello same node.</span></span>
* <span data-ttu-id="4a1b0-288">Használjon egy ideiglenes könyvtár /tmp tookeep hello hello parancsfájlok által használt fájlokat, majd eltávolítással parancsfájlok rendelkezik végrehajtása után.</span><span class="sxs-lookup"><span data-stu-id="4a1b0-288">Use a temporary file directory /tmp tookeep hello downloaded files used by hello scripts and then clean them up after scripts have executed.</span></span>
* <span data-ttu-id="4a1b0-289">Ha az operációs rendszer szintű beállításokat és a Hadoop szolgáltatás konfigurációs fájlokat, módosulnak, érdemes lehet toorestart HDInsight szolgáltatások.</span><span class="sxs-lookup"><span data-stu-id="4a1b0-289">If OS-level settings or Hadoop service configuration files are changed, you may want toorestart HDInsight services.</span></span>

## <span data-ttu-id="4a1b0-290"><a name="runScriptAction"></a>Hogyan toorun egy parancsfájlművelet</span><span class="sxs-lookup"><span data-stu-id="4a1b0-290"><a name="runScriptAction"></a>How toorun a script action</span></span>

<span data-ttu-id="4a1b0-291">Parancsfájl műveletek toocustomize HDInsight-fürtök hello a következő módszerek használhatók:</span><span class="sxs-lookup"><span data-stu-id="4a1b0-291">You can use script actions toocustomize HDInsight clusters using hello following methods:</span></span>

* <span data-ttu-id="4a1b0-292">Azure Portal</span><span class="sxs-lookup"><span data-stu-id="4a1b0-292">Azure portal</span></span>
* <span data-ttu-id="4a1b0-293">Azure PowerShell</span><span class="sxs-lookup"><span data-stu-id="4a1b0-293">Azure PowerShell</span></span>
* <span data-ttu-id="4a1b0-294">Azure Resource Manager-sablonok</span><span class="sxs-lookup"><span data-stu-id="4a1b0-294">Azure Resource Manager templates</span></span>
* <span data-ttu-id="4a1b0-295">hello HDInsight .NET SDK-val.</span><span class="sxs-lookup"><span data-stu-id="4a1b0-295">hello HDInsight .NET SDK.</span></span>

<span data-ttu-id="4a1b0-296">További információ az egyes módszerek használatával, lásd: [hogyan toouse parancsfájl művelet](hdinsight-hadoop-customize-cluster-linux.md).</span><span class="sxs-lookup"><span data-stu-id="4a1b0-296">For more information on using each method, see [How toouse script action](hdinsight-hadoop-customize-cluster-linux.md).</span></span>

## <span data-ttu-id="4a1b0-297"><a name="sampleScripts"></a>Egyéni parancsfájl-minták</span><span class="sxs-lookup"><span data-stu-id="4a1b0-297"><a name="sampleScripts"></a>Custom script samples</span></span>

<span data-ttu-id="4a1b0-298">Microsoft tooinstall összetevők Ez a minta parancsfájlok HDInsight-fürtöt.</span><span class="sxs-lookup"><span data-stu-id="4a1b0-298">Microsoft provides sample scripts tooinstall components on an HDInsight cluster.</span></span> <span data-ttu-id="4a1b0-299">Tekintse meg a következő hivatkozások további példa Parancsfájlműveletek hello.</span><span class="sxs-lookup"><span data-stu-id="4a1b0-299">See hello following links for more example script actions.</span></span>

* [<span data-ttu-id="4a1b0-300">Telepítheti és használhatja a HDInsight-fürtök Hue</span><span class="sxs-lookup"><span data-stu-id="4a1b0-300">Install and use Hue on HDInsight clusters</span></span>](hdinsight-hadoop-hue-linux.md)
* [<span data-ttu-id="4a1b0-301">Telepítheti és használhatja a HDInsight-fürtök Solr</span><span class="sxs-lookup"><span data-stu-id="4a1b0-301">Install and use Solr on HDInsight clusters</span></span>](hdinsight-hadoop-solr-install-linux.md)
* [<span data-ttu-id="4a1b0-302">Telepítheti és használhatja a HDInsight-fürtök Giraph</span><span class="sxs-lookup"><span data-stu-id="4a1b0-302">Install and use Giraph on HDInsight clusters</span></span>](hdinsight-hadoop-giraph-install-linux.md)
* [<span data-ttu-id="4a1b0-303">Telepítéséhez vagy frissítéséhez monó a HDInsight-fürtökön</span><span class="sxs-lookup"><span data-stu-id="4a1b0-303">Install or upgrade Mono on HDInsight clusters</span></span>](hdinsight-hadoop-install-mono.md)

## <a name="troubleshooting"></a><span data-ttu-id="4a1b0-304">Hibaelhárítás</span><span class="sxs-lookup"><span data-stu-id="4a1b0-304">Troubleshooting</span></span>

<span data-ttu-id="4a1b0-305">Az alábbiakban hello szert parancsfájlok használata során felmerülő hibák:</span><span class="sxs-lookup"><span data-stu-id="4a1b0-305">hello following are errors you may encounter when using scripts you have developed:</span></span>

<span data-ttu-id="4a1b0-306">**Hiba**: `$'\r': command not found`.</span><span class="sxs-lookup"><span data-stu-id="4a1b0-306">**Error**: `$'\r': command not found`.</span></span> <span data-ttu-id="4a1b0-307">Egyes esetekben követ `syntax error: unexpected end of file`.</span><span class="sxs-lookup"><span data-stu-id="4a1b0-307">Sometimes followed by `syntax error: unexpected end of file`.</span></span>

<span data-ttu-id="4a1b0-308">*OK*: ezt a hibát az okozza, ha a parancsfájl hello sorainak CRLF végződik.</span><span class="sxs-lookup"><span data-stu-id="4a1b0-308">*Cause*: This error is caused when hello lines in a script end with CRLF.</span></span> <span data-ttu-id="4a1b0-309">UNIX rendszerek csak LF hello sor befejezési, várt.</span><span class="sxs-lookup"><span data-stu-id="4a1b0-309">Unix systems expect only LF as hello line ending.</span></span>

<span data-ttu-id="4a1b0-310">Ez a probléma leggyakrabban esetén hello parancsfájl állította össze a Windows-környezetben, mert CRLF egy közös sor a Windows sok szöveg szerkesztők befejezési.</span><span class="sxs-lookup"><span data-stu-id="4a1b0-310">This problem most often occurs when hello script is authored on a Windows environment, as CRLF is a common line ending for many text editors on Windows.</span></span>

<span data-ttu-id="4a1b0-311">*Megoldási*: Ha egy beállítást a szövegszerkesztőben, Unix-formátum vagy kiválasztása LF hello sor befejezése.</span><span class="sxs-lookup"><span data-stu-id="4a1b0-311">*Resolution*: If it is an option in your text editor, select Unix format or LF for hello line ending.</span></span> <span data-ttu-id="4a1b0-312">A Unix rendszer toochange hello CRLF tooan LF parancsok következő hello is használhatja:</span><span class="sxs-lookup"><span data-stu-id="4a1b0-312">You may also use hello following commands on a Unix system toochange hello CRLF tooan LF:</span></span>

> [!NOTE]
> <span data-ttu-id="4a1b0-313">hello következő parancsok abban nagyjából megfelel, hogy azok hello CRLF sor végződések javítása tooLF kell módosítani.</span><span class="sxs-lookup"><span data-stu-id="4a1b0-313">hello following commands are roughly equivalent in that they should change hello CRLF line endings tooLF.</span></span> <span data-ttu-id="4a1b0-314">Válasszon ki egy, a rendszer hello segédprogramok alapján.</span><span class="sxs-lookup"><span data-stu-id="4a1b0-314">Select one based on hello utilities available on your system.</span></span>

| <span data-ttu-id="4a1b0-315">Parancs</span><span class="sxs-lookup"><span data-stu-id="4a1b0-315">Command</span></span> | <span data-ttu-id="4a1b0-316">Megjegyzések</span><span class="sxs-lookup"><span data-stu-id="4a1b0-316">Notes</span></span> |
| --- | --- |
| `unix2dos -b INFILE` |<span data-ttu-id="4a1b0-317">hello eredeti fájlt a biztonsági mentése egy. BAK bővítmény</span><span class="sxs-lookup"><span data-stu-id="4a1b0-317">hello original file is backed up with a .BAK extension</span></span> |
| `tr -d '\r' < INFILE > OUTFILE` |<span data-ttu-id="4a1b0-318">EREDMÉNYFÁJL csak LF végződések javítása rendelkező verzióját tartalmazza.</span><span class="sxs-lookup"><span data-stu-id="4a1b0-318">OUTFILE contains a version with only LF endings</span></span> |
| `perl -pi -e 's/\r\n/\n/g' INFILE` | <span data-ttu-id="4a1b0-319">Közvetlenül a hello fájl módosítása</span><span class="sxs-lookup"><span data-stu-id="4a1b0-319">Modifies hello file directly</span></span> |
| ```sed 's/$'"/`echo \\\r`/" INFILE > OUTFILE``` |<span data-ttu-id="4a1b0-320">EREDMÉNYFÁJL csak LF végződések javítása rendelkező verzióját tartalmazza.</span><span class="sxs-lookup"><span data-stu-id="4a1b0-320">OUTFILE contains a version with only LF endings.</span></span> |

<span data-ttu-id="4a1b0-321">**Hiba**: `line 1: #!/usr/bin/env: No such file or directory`.</span><span class="sxs-lookup"><span data-stu-id="4a1b0-321">**Error**: `line 1: #!/usr/bin/env: No such file or directory`.</span></span>

<span data-ttu-id="4a1b0-322">*OK*: Ez a hiba jelentkezik hello parancsfájl UTF-8 a bájt rendelés be van jelölve (AJ) néven lett mentve.</span><span class="sxs-lookup"><span data-stu-id="4a1b0-322">*Cause*: This error occurs when hello script was saved as UTF-8 with a Byte Order Mark (BOM).</span></span>

<span data-ttu-id="4a1b0-323">*Megoldási*: mentés hello fájl mint ASCII vagy UTF-8 AJ nélkül.</span><span class="sxs-lookup"><span data-stu-id="4a1b0-323">*Resolution*: Save hello file either as ASCII, or as UTF-8 without a BOM.</span></span> <span data-ttu-id="4a1b0-324">Hello parancs követően egy Linux vagy Unix rendszer toocreate egy fájl hello AJ nélkül is használhatja:</span><span class="sxs-lookup"><span data-stu-id="4a1b0-324">You may also use hello following command on a Linux or Unix system toocreate a file without hello BOM:</span></span>

    awk 'NR==1{sub(/^\xef\xbb\xbf/,"")}{print}' INFILE > OUTFILE

<span data-ttu-id="4a1b0-325">Cserélje le `INFILE` hello AJ tartalmazó hello fájllal.</span><span class="sxs-lookup"><span data-stu-id="4a1b0-325">Replace `INFILE` with hello file containing hello BOM.</span></span> <span data-ttu-id="4a1b0-326">`OUTFILE`egy új fájlnevet, hello parancsfájl nélkül hello AJ tartalmazó kell lennie.</span><span class="sxs-lookup"><span data-stu-id="4a1b0-326">`OUTFILE` should be a new file name, which contains hello script without hello BOM.</span></span>

## <span data-ttu-id="4a1b0-327"><a name="seeAlso"></a>Következő lépések</span><span class="sxs-lookup"><span data-stu-id="4a1b0-327"><a name="seeAlso"></a>Next steps</span></span>

* <span data-ttu-id="4a1b0-328">Ismerje meg, hogyan túl[testreszabása HDInsight-fürtök parancsfájlművelet használatával](hdinsight-hadoop-customize-cluster-linux.md)</span><span class="sxs-lookup"><span data-stu-id="4a1b0-328">Learn how too[Customize HDInsight clusters using script action](hdinsight-hadoop-customize-cluster-linux.md)</span></span>
* <span data-ttu-id="4a1b0-329">Használjon hello [HDInsight .NET SDK referenciáit](https://msdn.microsoft.com/library/mt271028.aspx) toolearn kezelése a HDInsight .NET-alkalmazások létrehozásával kapcsolatos további</span><span class="sxs-lookup"><span data-stu-id="4a1b0-329">Use hello [HDInsight .NET SDK reference](https://msdn.microsoft.com/library/mt271028.aspx) toolearn more about creating .NET applications that manage HDInsight</span></span>
* <span data-ttu-id="4a1b0-330">Használjon hello [HDInsight REST API](https://msdn.microsoft.com/library/azure/mt622197.aspx) hogyan toouse REST tooperform felügyeleti műveletek a HDInsight-fürtök toolearn.</span><span class="sxs-lookup"><span data-stu-id="4a1b0-330">Use hello [HDInsight REST API](https://msdn.microsoft.com/library/azure/mt622197.aspx) toolearn how toouse REST tooperform management actions on HDInsight clusters.</span></span>
