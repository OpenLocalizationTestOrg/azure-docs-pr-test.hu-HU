---
title: "során HDInsight Hive-könyvtárakhoz aaaAdd fürtlétrehozás - Azure |} Microsoft Docs"
description: "Ismerje meg, hogyan tooadd Hive-könyvtárakhoz (jar-fájlok), tooan HDInsight fürt fürt létrehozása során."
services: hdinsight
documentationcenter: 
author: Blackmist
manager: jhubbard
editor: cgronlun
ms.assetid: 2fd74b8d-c006-45c6-a9e2-72ff5d2d978a
ms.service: hdinsight
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 07/12/2017
ms.author: larryfr
ms.custom: H1Hack27Feb2017,hdinsightactive
ms.openlocfilehash: 2e028a07c3248205def0789af2c262a0774a8f19
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="add-custom-hive-libraries-when-creating-your-hdinsight-cluster"></a><span data-ttu-id="31334-103">Adja hozzá az egyedi Hive-könyvtárakhoz, ha a HDInsight-fürt létrehozása</span><span class="sxs-lookup"><span data-stu-id="31334-103">Add custom Hive libraries when creating your HDInsight cluster</span></span>

<span data-ttu-id="31334-104">Ha a HDInsight Hive a gyakran használt könyvtárak, ez a dokumentum ismerteti a parancsfájlművelet toopre terhelésű hello könyvtárak használata a fürt létrehozása során.</span><span class="sxs-lookup"><span data-stu-id="31334-104">If you have libraries that you use frequently with Hive on HDInsight, this document contains information on using a Script Action toopre-load hello libraries during cluster creation.</span></span> <span data-ttu-id="31334-105">Ebben a dokumentumban hello lépések segítségével hozzáadott szalagtárak világszerte elérhető a Hive - van nincs szükség toouse [hozzáadása JAR](https://cwiki.apache.org/confluence/display/Hive/LanguageManual+Cli) tooload őket.</span><span class="sxs-lookup"><span data-stu-id="31334-105">Libraries added using hello steps in this document are globally available in Hive - there is no need toouse [ADD JAR](https://cwiki.apache.org/confluence/display/Hive/LanguageManual+Cli) tooload them.</span></span>

## <a name="how-it-works"></a><span data-ttu-id="31334-106">Működés</span><span class="sxs-lookup"><span data-stu-id="31334-106">How it works</span></span>

<span data-ttu-id="31334-107">Amikor egy fürtöt hoz létre, opcionálisan megadhat egy parancsfájlt futtató művelet parancsfájl hello fürtcsomópontokon azok létrehozása közben.</span><span class="sxs-lookup"><span data-stu-id="31334-107">When creating a cluster, you can optionally specify a Script Action that runs a script on hello cluster nodes while they are being created.</span></span> <span data-ttu-id="31334-108">Ebben a dokumentumban hello parancsfájl egyetlen paramétert, és előre betöltött hello (tárolt jar-fájlok formájában) szalagtárak toobe tartalmazó WASB hely fogad el.</span><span class="sxs-lookup"><span data-stu-id="31334-108">hello script in this document accepts a single parameter, which is a WASB location that contains hello libraries (stored as jar files) toobe pre-loaded.</span></span>

<span data-ttu-id="31334-109">Fürt létrehozása során hello parancsfájl hello fájlok enumerálása, másolja őket toohello `/usr/lib/customhivelibs/` directory head és a munkavégző csomópont, majd hozzáadja őket toohello `hive.aux.jars.path` hello tulajdonság `core-site.xml` fájlt.</span><span class="sxs-lookup"><span data-stu-id="31334-109">During cluster creation, hello script enumerates hello files, copies them toohello `/usr/lib/customhivelibs/` directory on head and worker nodes, then adds them toohello `hive.aux.jars.path` property in hello `core-site.xml` file.</span></span> <span data-ttu-id="31334-110">Linux-alapú fürtökön is frissíti hello `hive-env.sh` hello fájlok hello helye fájlt.</span><span class="sxs-lookup"><span data-stu-id="31334-110">On Linux-based clusters, it also updates hello `hive-env.sh` file with hello location of hello files.</span></span>

> [!NOTE]
> <span data-ttu-id="31334-111">Ebben a cikkben hello Parancsfájlműveletek segítségével hello szalagtárak elérhetővé teszi a következő forgatókönyvek hello:</span><span class="sxs-lookup"><span data-stu-id="31334-111">Using hello script actions in this article makes hello libraries available in hello following scenarios:</span></span>
>
> * <span data-ttu-id="31334-112">**Linux-alapú HDInsight** – Ha egy Hive ügyfél használatával hello **WebHCat**, és **hiveserver2-n**.</span><span class="sxs-lookup"><span data-stu-id="31334-112">**Linux-based HDInsight** - when using hello a Hive client, **WebHCat**, and **HiveServer2**.</span></span>
> * <span data-ttu-id="31334-113">**Windows-alapú HDInsight** – hello Hive ügyfél használatakor és **WebHCat**.</span><span class="sxs-lookup"><span data-stu-id="31334-113">**Windows-based HDInsight** - when using hello Hive client and **WebHCat**.</span></span>

## <a name="hello-script"></a><span data-ttu-id="31334-114">hello parancsfájl</span><span class="sxs-lookup"><span data-stu-id="31334-114">hello script</span></span>

<span data-ttu-id="31334-115">**Parancsfájl helyét**</span><span class="sxs-lookup"><span data-stu-id="31334-115">**Script location**</span></span>

<span data-ttu-id="31334-116">A **Linux-alapú fürtökön**: [https://hdiconfigactions.blob.core.windows.net/linuxsetupcustomhivelibsv01/setup-customhivelibs-v01.sh](https://hdiconfigactions.blob.core.windows.net/linuxsetupcustomhivelibsv01/setup-customhivelibs-v01.sh)</span><span class="sxs-lookup"><span data-stu-id="31334-116">For **Linux-based clusters**: [https://hdiconfigactions.blob.core.windows.net/linuxsetupcustomhivelibsv01/setup-customhivelibs-v01.sh](https://hdiconfigactions.blob.core.windows.net/linuxsetupcustomhivelibsv01/setup-customhivelibs-v01.sh)</span></span>

<span data-ttu-id="31334-117">A **Windows-alapú fürtök**: [https://hdiconfigactions.blob.core.windows.net/setupcustomhivelibsv01/setup-customhivelibs-v01.ps1](https://hdiconfigactions.blob.core.windows.net/setupcustomhivelibsv01/setup-customhivelibs-v01.ps1)</span><span class="sxs-lookup"><span data-stu-id="31334-117">For **Windows-based clusters**: [https://hdiconfigactions.blob.core.windows.net/setupcustomhivelibsv01/setup-customhivelibs-v01.ps1](https://hdiconfigactions.blob.core.windows.net/setupcustomhivelibsv01/setup-customhivelibs-v01.ps1)</span></span>

> [!IMPORTANT]
> <span data-ttu-id="31334-118">Linux hello azt az egyetlen operációs rendszer, használja a HDInsight 3.4 vagy újabb verziója.</span><span class="sxs-lookup"><span data-stu-id="31334-118">Linux is hello only operating system used on HDInsight version 3.4 or greater.</span></span> <span data-ttu-id="31334-119">További tudnivalókért lásd: [A HDInsight elavulása Windows rendszeren](hdinsight-component-versioning.md#hdinsight-windows-retirement).</span><span class="sxs-lookup"><span data-stu-id="31334-119">For more information, see [HDInsight retirement on Windows](hdinsight-component-versioning.md#hdinsight-windows-retirement).</span></span>

<span data-ttu-id="31334-120">**Követelmények**</span><span class="sxs-lookup"><span data-stu-id="31334-120">**Requirements**</span></span>

* <span data-ttu-id="31334-121">hello parancsfájlok kell alkalmazott tooboth hello **Átjárócsomópontokat** és **munkavégző csomópontokhoz**.</span><span class="sxs-lookup"><span data-stu-id="31334-121">hello scripts must be applied tooboth hello **Head nodes** and **Worker nodes**.</span></span>

* <span data-ttu-id="31334-122">hello JAR-fájlok kivételével tooinstall Azure Blob Storage-ban kell tárolni kívánja a **egyetlen tároló**.</span><span class="sxs-lookup"><span data-stu-id="31334-122">hello jars you wish tooinstall must be stored in Azure Blob Storage in a **single container**.</span></span>

* <span data-ttu-id="31334-123">hello tárfiókot tartalmazó jar-fájlok hello könyvtár **kell** kell csatolt toohello HDInsight-fürt létrehozása során.</span><span class="sxs-lookup"><span data-stu-id="31334-123">hello storage account containing hello library of jar files **must** be linked toohello HDInsight cluster during creation.</span></span> <span data-ttu-id="31334-124">Vagy hello alapértelmezett tárfiókot kell lennie, vagy a fiók hozzáadva __opcionális konfigurációs__.</span><span class="sxs-lookup"><span data-stu-id="31334-124">It must either be hello default storage account, or an account added through __optional configuration__.</span></span>

* <span data-ttu-id="31334-125">a paraméter toohello parancsfájlművelet hello WASB elérési toohello tárolót kell megadni.</span><span class="sxs-lookup"><span data-stu-id="31334-125">hello WASB path toohello container must be specified as a parameter toohello Script Action.</span></span> <span data-ttu-id="31334-126">Például ha hello JAR-fájlok kivételével tárolódnak nevű tárolót **függvénytárak** egy tárfiókon nevű **mystorage**, hello paraméter lenne  **wasb://libs@mystorage.blob.core.windows.net/** .</span><span class="sxs-lookup"><span data-stu-id="31334-126">For example, if hello jars are stored in a container named **libs** on a storage account named **mystorage**, hello parameter would be **wasb://libs@mystorage.blob.core.windows.net/**.</span></span>

  > [!NOTE]
  > <span data-ttu-id="31334-127">Jelen dokumentum céljából feltételezzük, hogy rendelkezik már hozzon létre egy tárfiókot, a blob-tároló és a feltöltött hello fájlok tooit.</span><span class="sxs-lookup"><span data-stu-id="31334-127">This document assumes that you have already create a storage account, blob container, and uploaded hello files tooit.</span></span>
  >
  > <span data-ttu-id="31334-128">Ha nem hozott létre egy tárfiókot, ehhez keresztül hello [Azure-portálon](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="31334-128">If you have not created a storage account, you can do so through hello [Azure portal](https://portal.azure.com).</span></span> <span data-ttu-id="31334-129">Ezután egy segédprogramot használhatja például a [Azure Tártallózó](http://storageexplorer.com/) toocreate egy tároló hello fiók- és feltöltése az fájlok tooit.</span><span class="sxs-lookup"><span data-stu-id="31334-129">You can then use a utility such as [Azure Storage Explorer](http://storageexplorer.com/) toocreate a container in hello account and upload files tooit.</span></span>

## <a name="create-a-cluster-using-hello-script"></a><span data-ttu-id="31334-130">Hozzon létre egy fürtöt hello parancsfájl használatával</span><span class="sxs-lookup"><span data-stu-id="31334-130">Create a cluster using hello script</span></span>

> [!NOTE]
> <span data-ttu-id="31334-131">a lépéseket követve hello hozzon létre egy Linux-alapú HDInsight-fürtöt.</span><span class="sxs-lookup"><span data-stu-id="31334-131">hello following steps create a Linux-based HDInsight cluster.</span></span> <span data-ttu-id="31334-132">egy Windows-alapú fürt toocreate kiválasztása **Windows** , hello fürt operációs rendszer hello fürt létrehozásakor, és hello (PowerShell) a Windows parancsfájl használata helyett hello bash parancsfájlok.</span><span class="sxs-lookup"><span data-stu-id="31334-132">toocreate a Windows-based cluster, select **Windows** as hello cluster OS when creating hello cluster, and use hello Windows (PowerShell) script instead of hello bash script.</span></span>
>
> <span data-ttu-id="31334-133">Azure PowerShell vagy hello HDInsight .NET SDK toocreate ezt a parancsfájlt a fürt is használja.</span><span class="sxs-lookup"><span data-stu-id="31334-133">You can also use Azure PowerShell or hello HDInsight .NET SDK toocreate a cluster using this script.</span></span> <span data-ttu-id="31334-134">A következő módszerekkel további információkért lásd: [testreszabása HDInsight-fürtök parancsfájlműveletekkel](hdinsight-hadoop-customize-cluster-linux.md).</span><span class="sxs-lookup"><span data-stu-id="31334-134">For more information on using these methods, see [Customize HDInsight clusters with Script Actions](hdinsight-hadoop-customize-cluster-linux.md).</span></span>

1. <span data-ttu-id="31334-135">Fürt elkezdhessen a hello lépések segítségével [Provision HDInsight-fürtök Linux rendszeren](hdinsight-hadoop-provision-linux-clusters.md), de kiépítése nem fejeződött be.</span><span class="sxs-lookup"><span data-stu-id="31334-135">Start provisioning a cluster by using hello steps in [Provision HDInsight clusters on Linux](hdinsight-hadoop-provision-linux-clusters.md), but do not complete provisioning.</span></span>

2. <span data-ttu-id="31334-136">A hello **opcionális konfigurációs** panelen válassza **Parancsfájlműveletek**, és adja meg a következő információ hello:</span><span class="sxs-lookup"><span data-stu-id="31334-136">On hello **Optional Configuration** blade, select **Script Actions**, and provide hello following information:</span></span>

   * <span data-ttu-id="31334-137">**NÉV**: Adjon meg egy rövid nevet hello parancsfájlművelet.</span><span class="sxs-lookup"><span data-stu-id="31334-137">**NAME**: Enter a friendly name for hello script action.</span></span>

   * <span data-ttu-id="31334-138">**PARANCSFÁJL URI azonosítója**: https://hdiconfigactions.blob.core.windows.net/linuxsetupcustomhivelibsv01/setup-customhivelibs-v01.sh</span><span class="sxs-lookup"><span data-stu-id="31334-138">**SCRIPT URI**: https://hdiconfigactions.blob.core.windows.net/linuxsetupcustomhivelibsv01/setup-customhivelibs-v01.sh</span></span>

   * <span data-ttu-id="31334-139">**HEAD**: ezt a beállítást.</span><span class="sxs-lookup"><span data-stu-id="31334-139">**HEAD**: Check this option.</span></span>

   * <span data-ttu-id="31334-140">**MUNKAVÉGZŐ**: ezt a beállítást.</span><span class="sxs-lookup"><span data-stu-id="31334-140">**WORKER**: Check this option.</span></span>

   * <span data-ttu-id="31334-141">**ZOOKEEPER**: hagyja üresen a mezőt.</span><span class="sxs-lookup"><span data-stu-id="31334-141">**ZOOKEEPER**: Leave this blank.</span></span>

   * <span data-ttu-id="31334-142">**PARAMÉTEREK**: Adja meg a hello WASB cím toohello tároló és a tárolási fiók, amely tartalmazza a hello JAR-fájlok kivételével.</span><span class="sxs-lookup"><span data-stu-id="31334-142">**PARAMETERS**: Enter hello WASB address toohello container and storage account that contains hello jars.</span></span> <span data-ttu-id="31334-143">Például  **wasb://libs@mystorage.blob.core.windows.net/** .</span><span class="sxs-lookup"><span data-stu-id="31334-143">For example, **wasb://libs@mystorage.blob.core.windows.net/**.</span></span>

3. <span data-ttu-id="31334-144">Hello hello alján **Parancsfájlműveletek**, hello használata **válasszon** gombok toosave hello beállítása.</span><span class="sxs-lookup"><span data-stu-id="31334-144">At hello bottom of hello **Script Actions**, use hello **Select** button toosave hello configuration.</span></span>

4. <span data-ttu-id="31334-145">A hello **opcionális konfigurációs** panelen válassza **a társított Tárfiókokban** és select hello **tárolási kulcs hozzáadása** hivatkozásra.</span><span class="sxs-lookup"><span data-stu-id="31334-145">On hello **Optional Configuration** blade, select **Linked Storage Accounts** and select hello **Add a storage key** link.</span></span> <span data-ttu-id="31334-146">Jelöljön ki, amely tartalmazza a hello JAR-fájlok kivételével hello tárfiók, és aztán hello **válasszon** gombok toosave beállításokat és a visszatérési hello **opcionális konfigurációs** panelen.</span><span class="sxs-lookup"><span data-stu-id="31334-146">Select hello storage account that contains hello jars, and then use hello **select** buttons toosave settings and return hello **Optional Configuration** blade.</span></span>

5. <span data-ttu-id="31334-147">Használja hello **kiválasztása** hello hello alján gomb **opcionális konfigurációs** panel toosave hello opcionális konfigurációs adatait.</span><span class="sxs-lookup"><span data-stu-id="31334-147">Use hello **Select** button at hello bottom of hello **Optional Configuration** blade toosave hello optional configuration information.</span></span>

6. <span data-ttu-id="31334-148">Kiépítés hello fürt, a folytatáshoz [Provision HDInsight-fürtök Linux rendszeren](hdinsight-hadoop-provision-linux-clusters.md).</span><span class="sxs-lookup"><span data-stu-id="31334-148">Continue provisioning hello cluster as described in [Provision HDInsight clusters on Linux](hdinsight-hadoop-provision-linux-clusters.md).</span></span>

<span data-ttu-id="31334-149">Miután befejeződött a fürt létrehozása,-e képes toouse hello JAR-fájlok kivételével Hive hozzá ezt a parancsfájlt keresztül anélkül, hogy toouse hello `ADD JAR` utasítást.</span><span class="sxs-lookup"><span data-stu-id="31334-149">Once cluster creation finishes, you are able toouse hello jars added through this script from Hive without having toouse hello `ADD JAR` statement.</span></span>

## <a name="next-steps"></a><span data-ttu-id="31334-150">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="31334-150">Next steps</span></span>

<span data-ttu-id="31334-151">Hive munkavégzés további információkért lásd: [hdinsight Hive használata](hdinsight-use-hive.md)</span><span class="sxs-lookup"><span data-stu-id="31334-151">For more information on working with Hive, see [Use Hive with HDInsight](hdinsight-use-hive.md)</span></span>
