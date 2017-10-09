---
title: "a HDInsight tools aaaMigrate tooAzure erőforrás-kezelő |} Microsoft Docs"
description: "Hogyan HDInsight-fürtök toomigrate tooAzure fejlesztői erőforrás-kezelő eszközei"
services: hdinsight
editor: cgronlun
manager: jhubbard
author: nitinme
documentationcenter: 
ms.assetid: 05efedb5-6456-4552-87ff-156d77fbe2e1
ms.service: hdinsight
ms.custom: hdinsightactive
ms.workload: big-data
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/10/2017
ms.author: nitinme
ms.openlocfilehash: c087ae63d2544e5badae6be9c258f783aa92e2ef
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="migrating-tooazure-resource-manager-based-development-tools-for-hdinsight-clusters"></a><span data-ttu-id="1e560-103">A HDInsight-fürtök áttelepítése tooAzure fejlesztési Resource Manager-alapú eszközök</span><span class="sxs-lookup"><span data-stu-id="1e560-103">Migrating tooAzure Resource Manager-based development tools for HDInsight clusters</span></span>

<span data-ttu-id="1e560-104">A HDInsight-ból kivezettük való Azure Service Manager ASM-alapú eszközök hdinsight.</span><span class="sxs-lookup"><span data-stu-id="1e560-104">HDInsight is deprecating Azure Service Manager (ASM)-based tools for HDInsight.</span></span> <span data-ttu-id="1e560-105">Ha használta az Azure PowerShell, az Azure parancssori felület vagy hello HDInsight .NET SDK toowork HDInsight-fürtökkel, javasolt toouse hello Azure Resource Manager ARM-alapú verzióiban PowerShell parancssori felület és továbbítja a .NET SDK áll.</span><span class="sxs-lookup"><span data-stu-id="1e560-105">If you have been using Azure PowerShell, Azure CLI, or hello HDInsight .NET SDK toowork with HDInsight clusters, you are encouraged toouse hello Azure Resource Manager (ARM)-based versions of PowerShell, CLI, and .NET SDK going forward.</span></span> <span data-ttu-id="1e560-106">Ez a cikk ismerteti a mutatók hogyan toomigrate toohello új ARM-alapú módszer.</span><span class="sxs-lookup"><span data-stu-id="1e560-106">This article provides pointers on how toomigrate toohello new ARM-based approach.</span></span> <span data-ttu-id="1e560-107">Megfelelő esetben ez a cikk is rámutat hello ASM és ARM megközelítések hdinsight hello különbségei.</span><span class="sxs-lookup"><span data-stu-id="1e560-107">Wherever applicable, this article also points out hello differences between hello ASM and ARM approaches for HDInsight.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="1e560-108">a címterület-kezelés hello támogatása PowerShell parancssori felület, és a .NET SDK nem küld a **2017. január 1.**.</span><span class="sxs-lookup"><span data-stu-id="1e560-108">hello support for ASM based PowerShell, CLI, and .NET SDK will discontinue on **January 1, 2017**.</span></span>
> 
> 

## <a name="migrating-azure-cli-tooazure-resource-manager"></a><span data-ttu-id="1e560-109">Áttelepítése az Azure parancssori felület tooAzure erőforrás-kezelő</span><span class="sxs-lookup"><span data-stu-id="1e560-109">Migrating Azure CLI tooAzure Resource Manager</span></span>
<span data-ttu-id="1e560-110">hello Azure CLI most alapértelmezett tooAzure a Resource Manager (ARM) módban, kivéve, ha frissíti a korábbi telepítés; Ebben az esetben szüksége lehet toouse hello `azure config mode arm` tooswitch tooARM üzemmód.</span><span class="sxs-lookup"><span data-stu-id="1e560-110">hello Azure CLI now defaults tooAzure Resource Manager (ARM) mode, unless you are upgrading from a previous installation; in this case, you may need toouse hello `azure config mode arm` command tooswitch tooARM mode.</span></span>

<span data-ttu-id="1e560-111">a parancsok alapszintű hello adott hello Azure CLI toowork megadott HDInsight Azure Service Management (ASM) a rendszer hello azonos ARM; használatakor azonban bizonyos paraméterek és kapcsolók előfordulhat, hogy új neve lehet, és nincsenek sok új paraméter elérhető ARM használatakor.</span><span class="sxs-lookup"><span data-stu-id="1e560-111">hello basic commands that hello Azure CLI provided toowork with HDInsight using Azure Service Management (ASM) are hello same when using ARM; however some parameters and switches may have new names, and there are many new parameters available when using ARM.</span></span> <span data-ttu-id="1e560-112">Például használhatja `azure hdinsight cluster create` toospecify hello Azure virtuális hálózat, amely egy fürt, létre kell hozni, vagy Hive- és Oozie metaadattárhoz.</span><span class="sxs-lookup"><span data-stu-id="1e560-112">For example, you can now use `azure hdinsight cluster create` toospecify hello Azure Virtual Network that a cluster should be created in, or Hive and Oozie metastore information.</span></span>

<span data-ttu-id="1e560-113">A HDInsight Azure Resource Manageren keresztül használatához alapvető parancsok a következők:</span><span class="sxs-lookup"><span data-stu-id="1e560-113">Basic commands for working with HDInsight through Azure Resource Manager are:</span></span>

* <span data-ttu-id="1e560-114">`azure hdinsight cluster create`-hoz létre egy új HDInsight-fürt</span><span class="sxs-lookup"><span data-stu-id="1e560-114">`azure hdinsight cluster create` - creates a new HDInsight cluster</span></span>
* <span data-ttu-id="1e560-115">`azure hdinsight cluster delete`– egy meglévő HDInsight-fürt törlése</span><span class="sxs-lookup"><span data-stu-id="1e560-115">`azure hdinsight cluster delete` - deletes an existing HDInsight cluster</span></span>
* <span data-ttu-id="1e560-116">`azure hdinsight cluster show`– egy meglévő fürthöz kapcsolatos információk megjelenítése</span><span class="sxs-lookup"><span data-stu-id="1e560-116">`azure hdinsight cluster show` - display information about an existing cluster</span></span>
* <span data-ttu-id="1e560-117">`azure hdinsight cluster list`– ismerteti az Azure-előfizetés a HDInsight-fürtök</span><span class="sxs-lookup"><span data-stu-id="1e560-117">`azure hdinsight cluster list` - lists HDInsight clusters for your Azure subscription</span></span>

<span data-ttu-id="1e560-118">Használjon hello `-h` tooinspect hello paraméterek és kapcsolók érhető el minden egyes parancsnál.</span><span class="sxs-lookup"><span data-stu-id="1e560-118">Use hello `-h` switch tooinspect hello parameters and switches available for each command.</span></span>

### <a name="new-commands"></a><span data-ttu-id="1e560-119">Új parancsok</span><span class="sxs-lookup"><span data-stu-id="1e560-119">New commands</span></span>
<span data-ttu-id="1e560-120">Elérhető az Azure Resource Manager új parancsok a következők:</span><span class="sxs-lookup"><span data-stu-id="1e560-120">New commands available with Azure Resource Manager are:</span></span>

* <span data-ttu-id="1e560-121">`azure hdinsight cluster resize`-dinamikusan módosítások hello hello fürt feldolgozó csomópontok száma</span><span class="sxs-lookup"><span data-stu-id="1e560-121">`azure hdinsight cluster resize` - dynamically changes hello number of worker nodes in hello cluster</span></span>
* <span data-ttu-id="1e560-122">`azure hdinsight cluster enable-http-access`-lehetővé teszi, hogy a HTTPs hozzáférés toohello fürtöt (az alapértelmezés)</span><span class="sxs-lookup"><span data-stu-id="1e560-122">`azure hdinsight cluster enable-http-access` - enables HTTPs access toohello cluster (on by default)</span></span>
* <span data-ttu-id="1e560-123">`azure hdinsight cluster disable-http-access`– HTTPs hozzáférés toohello fürt letiltása</span><span class="sxs-lookup"><span data-stu-id="1e560-123">`azure hdinsight cluster disable-http-access` - disables HTTPs access toohello cluster</span></span>
* <span data-ttu-id="1e560-124">`azure hdinsight script-action`-parancsokat biztosít létrehozása vagy kezelése Parancsfájlműveletek egy fürtön</span><span class="sxs-lookup"><span data-stu-id="1e560-124">`azure hdinsight script-action` - provides commands for creating/managing Script Actions on a cluster</span></span>
* <span data-ttu-id="1e560-125">`azure hdinsight config`-parancsok biztosít a konfigurációs fájl létrehozásakor használható hello `hdinsight cluster create` parancs tooprovide konfigurációs adatait.</span><span class="sxs-lookup"><span data-stu-id="1e560-125">`azure hdinsight config` - provides commands for creating a configuration file that can be used with hello `hdinsight cluster create` command tooprovide configuration information.</span></span>

### <a name="deprecated-commands"></a><span data-ttu-id="1e560-126">Elavult parancsok</span><span class="sxs-lookup"><span data-stu-id="1e560-126">Deprecated commands</span></span>
<span data-ttu-id="1e560-127">Ha hello `azure hdinsight job` parancsok toosubmit feladatok tooyour HDInsight-fürtjéhez, ezek nincsenek hello ARM-parancsok keresztül érhető el.</span><span class="sxs-lookup"><span data-stu-id="1e560-127">If you use hello `azure hdinsight job` commands toosubmit jobs tooyour HDInsight cluster, these are not available through hello ARM commands.</span></span> <span data-ttu-id="1e560-128">Ha tooprogrammatically küldés feladatok tooHDInsight parancsfájlok, Ehelyett használjon hello REST API-k, a HDInsight által biztosított.</span><span class="sxs-lookup"><span data-stu-id="1e560-128">If you need tooprogrammatically submit jobs tooHDInsight from scripts, you should instead use hello REST APIs provided by HDInsight.</span></span> <span data-ttu-id="1e560-129">A REST API-k használatával feladatok elküldésekor további információkért lásd: a következő dokumentumok hello.</span><span class="sxs-lookup"><span data-stu-id="1e560-129">For more information on submitting jobs using REST APIs, see hello following documents.</span></span>

* [<span data-ttu-id="1e560-130">Hadoop MapReduce-feladatok a HDInsight használata cURL használatával futtassa</span><span class="sxs-lookup"><span data-stu-id="1e560-130">Run MapReduce jobs with Hadoop on HDInsight using cURL</span></span>](hdinsight-hadoop-use-mapreduce-curl.md)
* [<span data-ttu-id="1e560-131">A Hadoop Hive-lekérdezések futtatása a HDInsight használata cURL használatával</span><span class="sxs-lookup"><span data-stu-id="1e560-131">Run Hive queries with Hadoop on HDInsight using cURL</span></span>](hdinsight-hadoop-use-hive-curl.md)
* [<span data-ttu-id="1e560-132">Pig-feladatokhoz Hadoop on HDInsight használata cURL használatával futtassa</span><span class="sxs-lookup"><span data-stu-id="1e560-132">Run Pig jobs with Hadoop on HDInsight using cURL</span></span>](hdinsight-hadoop-use-pig-curl.md)

<span data-ttu-id="1e560-133">Más módokon toorun MapReduce olvashat, struktúra, és interaktív módon sertésfelmérés, lásd: [használata MapReduce a Hadoop on HDInsight](hdinsight-use-mapreduce.md), [használata a hdinsight Hadoop Hive](hdinsight-use-hive.md), és [a Pig használata a hadooppal HDInsight](hdinsight-use-pig.md).</span><span class="sxs-lookup"><span data-stu-id="1e560-133">For information on other ways toorun MapReduce, Hive, and Pig interactively, see [Use MapReduce with Hadoop on HDInsight](hdinsight-use-mapreduce.md), [Use Hive with Hadoop on HDInsight](hdinsight-use-hive.md), and [Use Pig with Hadoop on HDInsight](hdinsight-use-pig.md).</span></span>

### <a name="examples"></a><span data-ttu-id="1e560-134">Példák</span><span class="sxs-lookup"><span data-stu-id="1e560-134">Examples</span></span>
<span data-ttu-id="1e560-135">**Fürt létrehozása**</span><span class="sxs-lookup"><span data-stu-id="1e560-135">**Creating a cluster**</span></span>

* <span data-ttu-id="1e560-136">Régi parancs (ASM)-`azure hdinsight cluster create myhdicluster --location northeurope --osType linux --storageAccountName mystorage --storageAccountKey <storagekey> --storageContainer mycontainer --userName admin --password mypassword --sshUserName sshuser --sshPassword mypassword`</span><span class="sxs-lookup"><span data-stu-id="1e560-136">Old command (ASM) - `azure hdinsight cluster create myhdicluster --location northeurope --osType linux --storageAccountName mystorage --storageAccountKey <storagekey> --storageContainer mycontainer --userName admin --password mypassword --sshUserName sshuser --sshPassword mypassword`</span></span>
* <span data-ttu-id="1e560-137">Új parancs (ARM)-`azure hdinsight cluster create myhdicluster -g myresourcegroup --location northeurope --osType linux --clusterType hadoop --defaultStorageAccountName mystorage --defaultStorageAccountKey <storagekey> --defaultStorageContainer mycontainer --userName admin -password mypassword --sshUserName sshuser --sshPassword mypassword`</span><span class="sxs-lookup"><span data-stu-id="1e560-137">New command (ARM) - `azure hdinsight cluster create myhdicluster -g myresourcegroup --location northeurope --osType linux --clusterType hadoop --defaultStorageAccountName mystorage --defaultStorageAccountKey <storagekey> --defaultStorageContainer mycontainer --userName admin -password mypassword --sshUserName sshuser --sshPassword mypassword`</span></span>

<span data-ttu-id="1e560-138">**A fürtök törlésével**</span><span class="sxs-lookup"><span data-stu-id="1e560-138">**Deleting a cluster**</span></span>

* <span data-ttu-id="1e560-139">Régi parancs (ASM)-`azure hdinsight cluster delete myhdicluster`</span><span class="sxs-lookup"><span data-stu-id="1e560-139">Old command (ASM) - `azure hdinsight cluster delete myhdicluster`</span></span>
* <span data-ttu-id="1e560-140">Új parancs (ARM)-`azure hdinsight cluster delete mycluster -g myresourcegroup`</span><span class="sxs-lookup"><span data-stu-id="1e560-140">New command (ARM) - `azure hdinsight cluster delete mycluster -g myresourcegroup`</span></span>

<span data-ttu-id="1e560-141">**Lista fürtök**</span><span class="sxs-lookup"><span data-stu-id="1e560-141">**List clusters**</span></span>

* <span data-ttu-id="1e560-142">Régi parancs (ASM)-`azure hdinsight cluster list`</span><span class="sxs-lookup"><span data-stu-id="1e560-142">Old command (ASM) - `azure hdinsight cluster list`</span></span>
* <span data-ttu-id="1e560-143">Új parancs (ARM)-`azure hdinsight cluster list`</span><span class="sxs-lookup"><span data-stu-id="1e560-143">New command (ARM) - `azure hdinsight cluster list`</span></span>

> [!NOTE]
> <span data-ttu-id="1e560-144">A hello parancs megadásával hello erőforrás csoport használatával `-g` csak hello fürtök visszaadható hello megadott erőforráscsoportban.</span><span class="sxs-lookup"><span data-stu-id="1e560-144">For hello list command, specifying hello resource group using `-g` will return only hello clusters in hello specified resource group.</span></span>
> 
> 

<span data-ttu-id="1e560-145">**Fürt információ megjelenítése**</span><span class="sxs-lookup"><span data-stu-id="1e560-145">**Show cluster information**</span></span>

* <span data-ttu-id="1e560-146">Régi parancs (ASM)-`azure hdinsight cluster show myhdicluster`</span><span class="sxs-lookup"><span data-stu-id="1e560-146">Old command (ASM) - `azure hdinsight cluster show myhdicluster`</span></span>
* <span data-ttu-id="1e560-147">Új parancs (ARM)-`azure hdinsight cluster show myhdicluster -g myresourcegroup`</span><span class="sxs-lookup"><span data-stu-id="1e560-147">New command (ARM) - `azure hdinsight cluster show myhdicluster -g myresourcegroup`</span></span>

## <a name="migrating-azure-powershell-tooazure-resource-manager"></a><span data-ttu-id="1e560-148">Áttelepítése az Azure PowerShell tooAzure erőforrás-kezelő</span><span class="sxs-lookup"><span data-stu-id="1e560-148">Migrating Azure PowerShell tooAzure Resource Manager</span></span>
<span data-ttu-id="1e560-149">hello általános információk az Azure PowerShell hello Azure Resource Managerrel (ARM) módban található [az Azure PowerShell használata Azure Resource Managerrel](../powershell-azure-resource-manager.md).</span><span class="sxs-lookup"><span data-stu-id="1e560-149">hello general information about Azure PowerShell in hello Azure Resource Manager (ARM) mode can be found at [Using Azure PowerShell with Azure Resource Manager](../powershell-azure-resource-manager.md).</span></span>

<span data-ttu-id="1e560-150">hello Azure PowerShell ARM parancsmagok telepített-mellé a címterület-kezelési parancsmagok hello lehet.</span><span class="sxs-lookup"><span data-stu-id="1e560-150">hello Azure PowerShell ARM cmdlets can be installed side-by-side with hello ASM cmdlets.</span></span> <span data-ttu-id="1e560-151">hello két mód hello parancsmagjait elkülönítsék névvel.</span><span class="sxs-lookup"><span data-stu-id="1e560-151">hello cmdlets from hello two modes can be distinguished by their names.</span></span>  <span data-ttu-id="1e560-152">hello ARM üzemmódban van *AzureRmHDInsight* a hello parancsmag neve túl összehasonlításával*AzureHDInsight* hello ASM módban.</span><span class="sxs-lookup"><span data-stu-id="1e560-152">hello ARM mode has *AzureRmHDInsight* in hello cmdlet names comparing too*AzureHDInsight* in hello ASM mode.</span></span>  <span data-ttu-id="1e560-153">Például *New-AzureRmHDInsightCluster* vs. *Új AzureHDInsightCluster*.</span><span class="sxs-lookup"><span data-stu-id="1e560-153">For example, *New-AzureRmHDInsightCluster* vs. *New-AzureHDInsightCluster*.</span></span> <span data-ttu-id="1e560-154">Paraméterek és kapcsolók hírek nevük lehet, hogy legyen, és nincsenek sok új paraméter elérhető ARM használatakor.</span><span class="sxs-lookup"><span data-stu-id="1e560-154">Parameters and switches may have news names, and there are many new parameters available when using ARM.</span></span>  <span data-ttu-id="1e560-155">Például több parancsmagok szükséges nevű új kapcsoló *- ResourceGroupName*.</span><span class="sxs-lookup"><span data-stu-id="1e560-155">For example, several cmdlets require a new switch called *-ResourceGroupName*.</span></span> 

<span data-ttu-id="1e560-156">Hello HDInsight-parancsmagokat használhatja, csatlakozzon a tooyour Azure-fiókra, és az új erőforráscsoport létrehozása:</span><span class="sxs-lookup"><span data-stu-id="1e560-156">Before you can use hello HDInsight cmdlets, you must connect tooyour Azure account, and create a new resource group:</span></span>

* <span data-ttu-id="1e560-157">Login-AzureRmAccount vagy [válasszon-AzureRmProfile](https://msdn.microsoft.com/library/mt619310.aspx).</span><span class="sxs-lookup"><span data-stu-id="1e560-157">Login-AzureRmAccount or [Select-AzureRmProfile](https://msdn.microsoft.com/library/mt619310.aspx).</span></span> <span data-ttu-id="1e560-158">Lásd: [hitelesítéséhez az Azure Resource Manager egyszerű szolgáltatás](../azure-resource-manager/resource-group-authenticate-service-principal.md)</span><span class="sxs-lookup"><span data-stu-id="1e560-158">See [Authenticating a service principal with Azure Resource Manager](../azure-resource-manager/resource-group-authenticate-service-principal.md)</span></span>
* [<span data-ttu-id="1e560-159">Új-AzureRmResourceGroup</span><span class="sxs-lookup"><span data-stu-id="1e560-159">New-AzureRmResourceGroup</span></span>](https://msdn.microsoft.com/library/mt603739.aspx)

### <a name="renamed-cmdlets"></a><span data-ttu-id="1e560-160">Átnevezett parancsmagok</span><span class="sxs-lookup"><span data-stu-id="1e560-160">Renamed cmdlets</span></span>
<span data-ttu-id="1e560-161">toolist hello HDInsight címterület-kezelési parancsmagok a Windows PowerShell-konzolon:</span><span class="sxs-lookup"><span data-stu-id="1e560-161">toolist hello HDInsight ASM cmdlets in Windows PowerShell console:</span></span>

    help *azurermhdinsight*

<span data-ttu-id="1e560-162">hello következő táblázatban hello címterület-kezelési parancsmagok és a nevek hello ARM üzemmódban:</span><span class="sxs-lookup"><span data-stu-id="1e560-162">hello following table lists hello ASM cmdlets and their names in hello ARM mode:</span></span>

| <span data-ttu-id="1e560-163">Címterület-kezelési parancsmagok</span><span class="sxs-lookup"><span data-stu-id="1e560-163">ASM cmdlets</span></span> | <span data-ttu-id="1e560-164">ARM-parancsmagok</span><span class="sxs-lookup"><span data-stu-id="1e560-164">ARM cmdlets</span></span> |
| --- | --- |
| <span data-ttu-id="1e560-165">Add-AzureHDInsightConfigValues</span><span class="sxs-lookup"><span data-stu-id="1e560-165">Add-AzureHDInsightConfigValues</span></span> |[<span data-ttu-id="1e560-166">Adja hozzá AzureRmHDInsightConfigValues</span><span class="sxs-lookup"><span data-stu-id="1e560-166">Add-AzureRmHDInsightConfigValues</span></span>](https://msdn.microsoft.com/library/mt603530.aspx) |
| <span data-ttu-id="1e560-167">Add-AzureHDInsightMetastore</span><span class="sxs-lookup"><span data-stu-id="1e560-167">Add-AzureHDInsightMetastore</span></span> |[<span data-ttu-id="1e560-168">Adja hozzá AzureRmHDInsightMetastore</span><span class="sxs-lookup"><span data-stu-id="1e560-168">Add-AzureRmHDInsightMetastore</span></span>](https://msdn.microsoft.com/library/mt603670.aspx) |
| <span data-ttu-id="1e560-169">Add-AzureHDInsightScriptAction</span><span class="sxs-lookup"><span data-stu-id="1e560-169">Add-AzureHDInsightScriptAction</span></span> |[<span data-ttu-id="1e560-170">Adja hozzá AzureRmHDInsightScriptAction</span><span class="sxs-lookup"><span data-stu-id="1e560-170">Add-AzureRmHDInsightScriptAction</span></span>](https://msdn.microsoft.com/library/mt603527.aspx) |
| <span data-ttu-id="1e560-171">Add-AzureHDInsightStorage</span><span class="sxs-lookup"><span data-stu-id="1e560-171">Add-AzureHDInsightStorage</span></span> |[<span data-ttu-id="1e560-172">Adja hozzá AzureRmHDInsightStorage</span><span class="sxs-lookup"><span data-stu-id="1e560-172">Add-AzureRmHDInsightStorage</span></span>](https://msdn.microsoft.com/library/mt619445.aspx) |
| <span data-ttu-id="1e560-173">Get-AzureHDInsightCluster</span><span class="sxs-lookup"><span data-stu-id="1e560-173">Get-AzureHDInsightCluster</span></span> |[<span data-ttu-id="1e560-174">Get-AzureRmHDInsightCluster</span><span class="sxs-lookup"><span data-stu-id="1e560-174">Get-AzureRmHDInsightCluster</span></span>](https://msdn.microsoft.com/library/mt619371.aspx) |
| <span data-ttu-id="1e560-175">Get-AzureHDInsightJob</span><span class="sxs-lookup"><span data-stu-id="1e560-175">Get-AzureHDInsightJob</span></span> |[<span data-ttu-id="1e560-176">Get-AzureRmHDInsightJob</span><span class="sxs-lookup"><span data-stu-id="1e560-176">Get-AzureRmHDInsightJob</span></span>](https://msdn.microsoft.com/library/mt603590.aspx) |
| <span data-ttu-id="1e560-177">Get-AzureHDInsightJobOutput</span><span class="sxs-lookup"><span data-stu-id="1e560-177">Get-AzureHDInsightJobOutput</span></span> |[<span data-ttu-id="1e560-178">Get-AzureRmHDInsightJobOutput</span><span class="sxs-lookup"><span data-stu-id="1e560-178">Get-AzureRmHDInsightJobOutput</span></span>](https://msdn.microsoft.com/library/mt603793.aspx) |
| <span data-ttu-id="1e560-179">Get-AzureHDInsightProperties</span><span class="sxs-lookup"><span data-stu-id="1e560-179">Get-AzureHDInsightProperties</span></span> |[<span data-ttu-id="1e560-180">Get-AzureRmHDInsightProperties</span><span class="sxs-lookup"><span data-stu-id="1e560-180">Get-AzureRmHDInsightProperties</span></span>](https://msdn.microsoft.com/library/mt603546.aspx) |
| <span data-ttu-id="1e560-181">Grant-AzureHDInsightHttpServicesAccess</span><span class="sxs-lookup"><span data-stu-id="1e560-181">Grant-AzureHDInsightHttpServicesAccess</span></span> |[<span data-ttu-id="1e560-182">Támogatás-AzureRmHDInsightHttpServicesAccess</span><span class="sxs-lookup"><span data-stu-id="1e560-182">Grant-AzureRmHDInsightHttpServicesAccess</span></span>](https://msdn.microsoft.com/library/mt619407.aspx) |
| <span data-ttu-id="1e560-183">Grant-AzureHdinsightRdpAccess</span><span class="sxs-lookup"><span data-stu-id="1e560-183">Grant-AzureHdinsightRdpAccess</span></span> |[<span data-ttu-id="1e560-184">Támogatás-AzureRmHDInsightRdpServicesAccess</span><span class="sxs-lookup"><span data-stu-id="1e560-184">Grant-AzureRmHDInsightRdpServicesAccess</span></span>](https://msdn.microsoft.com/library/mt603717.aspx) |
| <span data-ttu-id="1e560-185">Invoke-AzureHDInsightHiveJob</span><span class="sxs-lookup"><span data-stu-id="1e560-185">Invoke-AzureHDInsightHiveJob</span></span> |[<span data-ttu-id="1e560-186">Invoke-AzureRmHDInsightHiveJob</span><span class="sxs-lookup"><span data-stu-id="1e560-186">Invoke-AzureRmHDInsightHiveJob</span></span>](https://msdn.microsoft.com/library/mt603593.aspx) |
| <span data-ttu-id="1e560-187">New-AzureHDInsightCluster</span><span class="sxs-lookup"><span data-stu-id="1e560-187">New-AzureHDInsightCluster</span></span> |[<span data-ttu-id="1e560-188">Új AzureRmHDInsightCluster</span><span class="sxs-lookup"><span data-stu-id="1e560-188">New-AzureRmHDInsightCluster</span></span>](https://msdn.microsoft.com/library/mt619331.aspx) |
| <span data-ttu-id="1e560-189">New-AzureHDInsightClusterConfig</span><span class="sxs-lookup"><span data-stu-id="1e560-189">New-AzureHDInsightClusterConfig</span></span> |[<span data-ttu-id="1e560-190">Új AzureRmHDInsightClusterConfig</span><span class="sxs-lookup"><span data-stu-id="1e560-190">New-AzureRmHDInsightClusterConfig</span></span>](https://msdn.microsoft.com/library/mt603700.aspx) |
| <span data-ttu-id="1e560-191">New-AzureHDInsightHiveJobDefinition</span><span class="sxs-lookup"><span data-stu-id="1e560-191">New-AzureHDInsightHiveJobDefinition</span></span> |[<span data-ttu-id="1e560-192">Új AzureRmHDInsightHiveJobDefinition</span><span class="sxs-lookup"><span data-stu-id="1e560-192">New-AzureRmHDInsightHiveJobDefinition</span></span>](https://msdn.microsoft.com/library/mt619448.aspx) |
| <span data-ttu-id="1e560-193">New-AzureHDInsightMapReduceJobDefinition</span><span class="sxs-lookup"><span data-stu-id="1e560-193">New-AzureHDInsightMapReduceJobDefinition</span></span> |[<span data-ttu-id="1e560-194">Új AzureRmHDInsightMapReduceJobDefinition</span><span class="sxs-lookup"><span data-stu-id="1e560-194">New-AzureRmHDInsightMapReduceJobDefinition</span></span>](https://msdn.microsoft.com/library/mt603626.aspx) |
| <span data-ttu-id="1e560-195">New-AzureHDInsightPigJobDefinition</span><span class="sxs-lookup"><span data-stu-id="1e560-195">New-AzureHDInsightPigJobDefinition</span></span> |[<span data-ttu-id="1e560-196">Új AzureRmHDInsightPigJobDefinition</span><span class="sxs-lookup"><span data-stu-id="1e560-196">New-AzureRmHDInsightPigJobDefinition</span></span>](https://msdn.microsoft.com/library/mt603671.aspx) |
| <span data-ttu-id="1e560-197">New-AzureHDInsightSqoopJobDefinition</span><span class="sxs-lookup"><span data-stu-id="1e560-197">New-AzureHDInsightSqoopJobDefinition</span></span> |[<span data-ttu-id="1e560-198">Új AzureRmHDInsightSqoopJobDefinition</span><span class="sxs-lookup"><span data-stu-id="1e560-198">New-AzureRmHDInsightSqoopJobDefinition</span></span>](https://msdn.microsoft.com/library/mt608551.aspx) |
| <span data-ttu-id="1e560-199">New-AzureHDInsightStreamingMapReduceJobDefinition</span><span class="sxs-lookup"><span data-stu-id="1e560-199">New-AzureHDInsightStreamingMapReduceJobDefinition</span></span> |[<span data-ttu-id="1e560-200">Új AzureRmHDInsightStreamingMapReduceJobDefinition</span><span class="sxs-lookup"><span data-stu-id="1e560-200">New-AzureRmHDInsightStreamingMapReduceJobDefinition</span></span>](https://msdn.microsoft.com/library/mt603626.aspx) |
| <span data-ttu-id="1e560-201">Remove-AzureHDInsightCluster</span><span class="sxs-lookup"><span data-stu-id="1e560-201">Remove-AzureHDInsightCluster</span></span> |[<span data-ttu-id="1e560-202">Remove-AzureRmHDInsightCluster</span><span class="sxs-lookup"><span data-stu-id="1e560-202">Remove-AzureRmHDInsightCluster</span></span>](https://msdn.microsoft.com/library/mt619431.aspx) |
| <span data-ttu-id="1e560-203">Revoke-AzureHDInsightHttpServicesAccess</span><span class="sxs-lookup"><span data-stu-id="1e560-203">Revoke-AzureHDInsightHttpServicesAccess</span></span> |[<span data-ttu-id="1e560-204">A visszavonási-AzureRmHDInsightHttpServicesAccess</span><span class="sxs-lookup"><span data-stu-id="1e560-204">Revoke-AzureRmHDInsightHttpServicesAccess</span></span>](https://msdn.microsoft.com/library/mt619375.aspx) |
| <span data-ttu-id="1e560-205">Revoke-AzureHdinsightRdpAccess</span><span class="sxs-lookup"><span data-stu-id="1e560-205">Revoke-AzureHdinsightRdpAccess</span></span> |[<span data-ttu-id="1e560-206">A visszavonási-AzureRmHDInsightRdpServicesAccess</span><span class="sxs-lookup"><span data-stu-id="1e560-206">Revoke-AzureRmHDInsightRdpServicesAccess</span></span>](https://msdn.microsoft.com/library/mt603523.aspx) |
| <span data-ttu-id="1e560-207">Set-AzureHDInsightClusterSize</span><span class="sxs-lookup"><span data-stu-id="1e560-207">Set-AzureHDInsightClusterSize</span></span> |[<span data-ttu-id="1e560-208">Set-AzureRmHDInsightClusterSize</span><span class="sxs-lookup"><span data-stu-id="1e560-208">Set-AzureRmHDInsightClusterSize</span></span>](https://msdn.microsoft.com/library/mt603513.aspx) |
| <span data-ttu-id="1e560-209">Set-AzureHDInsightDefaultStorage</span><span class="sxs-lookup"><span data-stu-id="1e560-209">Set-AzureHDInsightDefaultStorage</span></span> |[<span data-ttu-id="1e560-210">Set-AzureRmHDInsightDefaultStorage</span><span class="sxs-lookup"><span data-stu-id="1e560-210">Set-AzureRmHDInsightDefaultStorage</span></span>](https://msdn.microsoft.com/library/mt603486.aspx) |
| <span data-ttu-id="1e560-211">Start-AzureHDInsightJob</span><span class="sxs-lookup"><span data-stu-id="1e560-211">Start-AzureHDInsightJob</span></span> |[<span data-ttu-id="1e560-212">Start-AzureRmHDInsightJob</span><span class="sxs-lookup"><span data-stu-id="1e560-212">Start-AzureRmHDInsightJob</span></span>](https://msdn.microsoft.com/library/mt603798.aspx) |
| <span data-ttu-id="1e560-213">Stop-AzureHDInsightJob</span><span class="sxs-lookup"><span data-stu-id="1e560-213">Stop-AzureHDInsightJob</span></span> |[<span data-ttu-id="1e560-214">STOP-AzureRmHDInsightJob</span><span class="sxs-lookup"><span data-stu-id="1e560-214">Stop-AzureRmHDInsightJob</span></span>](https://msdn.microsoft.com/library/mt619424.aspx) |
| <span data-ttu-id="1e560-215">Use-AzureHDInsightCluster</span><span class="sxs-lookup"><span data-stu-id="1e560-215">Use-AzureHDInsightCluster</span></span> |[<span data-ttu-id="1e560-216">Használjon-AzureRmHDInsightCluster</span><span class="sxs-lookup"><span data-stu-id="1e560-216">Use-AzureRmHDInsightCluster</span></span>](https://msdn.microsoft.com/library/mt619442.aspx) |
| <span data-ttu-id="1e560-217">Wait-AzureHDInsightJob</span><span class="sxs-lookup"><span data-stu-id="1e560-217">Wait-AzureHDInsightJob</span></span> |[<span data-ttu-id="1e560-218">Várjon, amíg-AzureRmHDInsightJob</span><span class="sxs-lookup"><span data-stu-id="1e560-218">Wait-AzureRmHDInsightJob</span></span>](https://msdn.microsoft.com/library/mt603834.aspx) |

### <a name="new-cmdlets"></a><span data-ttu-id="1e560-219">Új parancsmagok</span><span class="sxs-lookup"><span data-stu-id="1e560-219">New cmdlets</span></span>
<span data-ttu-id="1e560-220">Az alábbiakban hello hello új parancsmagokat csak hello ARM módban érhető el.</span><span class="sxs-lookup"><span data-stu-id="1e560-220">hello following are hello new cmdlets that are only available in hello ARM mode.</span></span> 

<span data-ttu-id="1e560-221">**A kapcsolódó parancsmagok parancsfájlművelet:**</span><span class="sxs-lookup"><span data-stu-id="1e560-221">**Script action related cmdlets:**</span></span>

* <span data-ttu-id="1e560-222">**Get-AzureRmHDInsightPersistedScriptAction**: lekérdezi hello megőrzött Parancsfájlműveletek fürt és időrendben sorolja fel azokat, vagy egy megadott megőrzött parancsfájl művelet lekérdezi a részletek.</span><span class="sxs-lookup"><span data-stu-id="1e560-222">**Get-AzureRmHDInsightPersistedScriptAction**: Gets hello persisted script actions for a cluster and lists them in chronological order, or gets details for a specified persisted script action.</span></span> 
* <span data-ttu-id="1e560-223">**Get-AzureRmHDInsightScriptActionHistory**: hello parancsfájlművelet-előzmény lekérdezi a fürt fordított időrendben sorolja fel, és lekérdezi a korábban végrehajtott parancsfájlművelet részleteit.</span><span class="sxs-lookup"><span data-stu-id="1e560-223">**Get-AzureRmHDInsightScriptActionHistory**: Gets hello script action history for a cluster and lists it in reverse chronological order, or gets details of a previously executed script action.</span></span> 
* <span data-ttu-id="1e560-224">**Remove-AzureRmHDInsightPersistedScriptAction**: egy megőrzött parancsfájl művelet eltávolítja a HDInsight-fürtöt.</span><span class="sxs-lookup"><span data-stu-id="1e560-224">**Remove-AzureRmHDInsightPersistedScriptAction**: Removes a persisted script action from an HDInsight cluster.</span></span>
* <span data-ttu-id="1e560-225">**Set-AzureRmHDInsightPersistedScriptAction**: állítja be a korábban végrehajtott parancsfájl művelet toobe egy megőrzött parancsfájl műveletet.</span><span class="sxs-lookup"><span data-stu-id="1e560-225">**Set-AzureRmHDInsightPersistedScriptAction**: Sets a previously executed script action toobe a persisted script action.</span></span>
* <span data-ttu-id="1e560-226">**Küldje el AzureRmHDInsightScriptAction**: egy új parancsfájl művelet tooan Azure HDInsight-fürtöt ad meg.</span><span class="sxs-lookup"><span data-stu-id="1e560-226">**Submit-AzureRmHDInsightScriptAction**: Submits a new script action tooan Azure HDInsight cluster.</span></span> 

<span data-ttu-id="1e560-227">További használati információkért lásd: [testreszabása Linux-alapú HDInsight-fürtök használata parancsfájlművelet](hdinsight-hadoop-customize-cluster-linux.md).</span><span class="sxs-lookup"><span data-stu-id="1e560-227">For additional usage information, see [Customize Linux-based HDInsight clusters using Script Action](hdinsight-hadoop-customize-cluster-linux.md).</span></span>

<span data-ttu-id="1e560-228">**A kapcsolódó parancsmagok Clsuter identitás:**</span><span class="sxs-lookup"><span data-stu-id="1e560-228">**Clsuter identity related cmdlets:**</span></span>

* <span data-ttu-id="1e560-229">**Adja hozzá AzureRmHDInsightClusterIdentity**: hozzáadja a fürt identitását tooa konfigurációs objektum érdekében, hogy a hello HDInsight-fürt Azure Data Lake tárolja.</span><span class="sxs-lookup"><span data-stu-id="1e560-229">**Add-AzureRmHDInsightClusterIdentity**: Adds a cluster identity tooa cluster configuration object so that hello HDInsight cluster can access Azure Data Lake Stores.</span></span> <span data-ttu-id="1e560-230">Lásd: [HDInsight-fürtök létrehozása az Azure PowerShell használatával a Data Lake Store](../data-lake-store/data-lake-store-hdinsight-hadoop-use-powershell.md).</span><span class="sxs-lookup"><span data-stu-id="1e560-230">See [Create an HDInsight cluster with Data Lake Store using Azure PowerShell](../data-lake-store/data-lake-store-hdinsight-hadoop-use-powershell.md).</span></span>

### <a name="examples"></a><span data-ttu-id="1e560-231">Példák</span><span class="sxs-lookup"><span data-stu-id="1e560-231">Examples</span></span>
<span data-ttu-id="1e560-232">**Fürt létrehozása**</span><span class="sxs-lookup"><span data-stu-id="1e560-232">**Create cluster**</span></span>

<span data-ttu-id="1e560-233">Régi parancs (ASM):</span><span class="sxs-lookup"><span data-stu-id="1e560-233">Old command (ASM):</span></span> 

    New-AzureHDInsightCluster `
        -Name $clusterName `
        -Location $location `
        -DefaultStorageAccountName "$storageAccountName.blob.core.windows.net" `
        -DefaultStorageAccountKey $storageAccountKey `
        -DefaultStorageContainerName $containerName `
        -ClusterSizeInNodes 2 `
        -ClusterType Hadoop `
        -OSType Linux `
        -Version "3.2" `
        -Credential $httpCredential `
        -SshCredential $sshCredential

<span data-ttu-id="1e560-234">Új parancs (ARM):</span><span class="sxs-lookup"><span data-stu-id="1e560-234">New command (ARM):</span></span>

    New-AzureRmHDInsightCluster `
        -ClusterName $clusterName `
        -ResourceGroupName $resourceGroupName `
        -Location $location `
        -DefaultStorageAccountName "$storageAccountName.blob.core.windows.net" `
        -DefaultStorageAccountKey $storageAccountKey `
        -DefaultStorageContainer $containerName  `
        -ClusterSizeInNodes 2 `
        -ClusterType Hadoop `
        -OSType Linux `
        -Version "3.2" `
        -HttpCredential $httpcredentials `
        -SshCredential $sshCredentials


<span data-ttu-id="1e560-235">**Fürt törlése**</span><span class="sxs-lookup"><span data-stu-id="1e560-235">**Delete cluster**</span></span>

<span data-ttu-id="1e560-236">Régi parancs (ASM):</span><span class="sxs-lookup"><span data-stu-id="1e560-236">Old command (ASM):</span></span>

    Remove-AzureHDInsightCluster -name $clusterName 

<span data-ttu-id="1e560-237">Új parancs (ARM):</span><span class="sxs-lookup"><span data-stu-id="1e560-237">New command (ARM):</span></span>

    Remove-AzureRmHDInsightCluster -ResourceGroupName $resourceGroupName -ClusterName $clusterName 

<span data-ttu-id="1e560-238">**Lista fürt**</span><span class="sxs-lookup"><span data-stu-id="1e560-238">**List cluster**</span></span>

<span data-ttu-id="1e560-239">Régi parancs (ASM):</span><span class="sxs-lookup"><span data-stu-id="1e560-239">Old command (ASM):</span></span>

    Get-AzureHDInsightCluster

<span data-ttu-id="1e560-240">Új parancs (ARM):</span><span class="sxs-lookup"><span data-stu-id="1e560-240">New command (ARM):</span></span>

    Get-AzureRmHDInsightCluster 

<span data-ttu-id="1e560-241">**Fürt megjelenítése**</span><span class="sxs-lookup"><span data-stu-id="1e560-241">**Show cluster**</span></span>

<span data-ttu-id="1e560-242">Régi parancs (ASM):</span><span class="sxs-lookup"><span data-stu-id="1e560-242">Old command (ASM):</span></span>

    Get-AzureHDInsightCluster -Name $clusterName

<span data-ttu-id="1e560-243">Új parancs (ARM):</span><span class="sxs-lookup"><span data-stu-id="1e560-243">New command (ARM):</span></span>

    Get-AzureRmHDInsightCluster -ResourceGroupName $resourceGroupName -clusterName $clusterName


#### <a name="other-samples"></a><span data-ttu-id="1e560-244">Más minták</span><span class="sxs-lookup"><span data-stu-id="1e560-244">Other samples</span></span>
* [<span data-ttu-id="1e560-245">A HDInsight-fürtök létrehozása</span><span class="sxs-lookup"><span data-stu-id="1e560-245">Create HDInsight clusters</span></span>](hdinsight-hadoop-create-linux-clusters-azure-powershell.md)
* [<span data-ttu-id="1e560-246">Küldje el a Hive-feladatok</span><span class="sxs-lookup"><span data-stu-id="1e560-246">Submit Hive jobs</span></span>](hdinsight-hadoop-use-hive-powershell.md)
* [<span data-ttu-id="1e560-247">Küldje el a Pig-feladatokhoz</span><span class="sxs-lookup"><span data-stu-id="1e560-247">Submit Pig jobs</span></span>](hdinsight-hadoop-use-pig-powershell.md)
* [<span data-ttu-id="1e560-248">Sqoop feladatok elküldéséhez</span><span class="sxs-lookup"><span data-stu-id="1e560-248">Submit Sqoop jobs</span></span>](hdinsight-hadoop-use-sqoop-powershell.md)

## <a name="migrating-toohello-arm-based-hdinsight-net-sdk"></a><span data-ttu-id="1e560-249">Áttelepítése toohello ARM-alapú HDInsight .NET SDK</span><span class="sxs-lookup"><span data-stu-id="1e560-249">Migrating toohello ARM-based HDInsight .NET SDK</span></span>
<span data-ttu-id="1e560-250">hello Azure Szolgáltatáskezelés-alapú [(ASM) HDInsight .NET SDK](https://msdn.microsoft.com/library/azure/mt416619.aspx) elavult.</span><span class="sxs-lookup"><span data-stu-id="1e560-250">hello Azure Service Management-based [(ASM) HDInsight .NET SDK](https://msdn.microsoft.com/library/azure/mt416619.aspx) is now deprecated.</span></span> <span data-ttu-id="1e560-251">Javasoljuk, toouse áll hello Azure Resource Manager-alapú [(ARM) a HDInsight .NET SDK](https://msdn.microsoft.com/library/azure/mt271028.aspx).</span><span class="sxs-lookup"><span data-stu-id="1e560-251">You are encouraged toouse hello Azure Resource Management-based [(ARM) HDInsight .NET SDK](https://msdn.microsoft.com/library/azure/mt271028.aspx).</span></span> <span data-ttu-id="1e560-252">hello következő ASM-alapú HDInsight-csomagok elavulttá válnak.</span><span class="sxs-lookup"><span data-stu-id="1e560-252">hello following ASM-based HDInsight packages are being deprecated.</span></span>

* `Microsoft.WindowsAzure.Management.HDInsight`
* `Microsoft.Hadoop.Client`

<span data-ttu-id="1e560-253">Ez a szakasz ismerteti a mutatók toomore tooperform bizonyos feladatok hello SDK ARM-alapú.</span><span class="sxs-lookup"><span data-stu-id="1e560-253">This section provides pointers toomore information on how tooperform certain tasks using hello ARM-based SDK.</span></span>

| <span data-ttu-id="1e560-254">Hogyan... ARM-alapú HDInsight SDK használatával hello</span><span class="sxs-lookup"><span data-stu-id="1e560-254">How to... using hello ARM-based HDInsight SDK</span></span> | <span data-ttu-id="1e560-255">Hivatkozások</span><span class="sxs-lookup"><span data-stu-id="1e560-255">Links</span></span> |
| --- | --- |
| <span data-ttu-id="1e560-256">.NET SDK használatával a HDInsight-fürtök létrehozása</span><span class="sxs-lookup"><span data-stu-id="1e560-256">Create HDInsight clusters using .NET SDK</span></span> |<span data-ttu-id="1e560-257">Lásd: [HDInsight-fürtök létrehozása .NET SDK használatával](hdinsight-hadoop-create-linux-clusters-dotnet-sdk.md)</span><span class="sxs-lookup"><span data-stu-id="1e560-257">See [Create HDInsight clusters using .NET SDK](hdinsight-hadoop-create-linux-clusters-dotnet-sdk.md)</span></span> |
| <span data-ttu-id="1e560-258">A fürt parancsfájlművelet .NET SDK-val testreszabása</span><span class="sxs-lookup"><span data-stu-id="1e560-258">Customize a cluster using Script Action with .NET SDK</span></span> |<span data-ttu-id="1e560-259">Lásd: [testreszabása HDInsight Linux clusters parancsfájlművelet használatával](hdinsight-hadoop-create-linux-clusters-dotnet-sdk.md#use-script-action)</span><span class="sxs-lookup"><span data-stu-id="1e560-259">See [Customize HDInsight Linux clusters using Script Action](hdinsight-hadoop-create-linux-clusters-dotnet-sdk.md#use-script-action)</span></span> |
| <span data-ttu-id="1e560-260">Alkalmazások interaktív .NET SDK-val az Azure Active Directory használatával hitelesíti</span><span class="sxs-lookup"><span data-stu-id="1e560-260">Authenticate applications interactively using Azure Active Directory with .NET SDK</span></span> |<span data-ttu-id="1e560-261">Lásd: [.NET SDK használatával futtassa Hive lekérdezések](hdinsight-hadoop-use-hive-dotnet-sdk.md).</span><span class="sxs-lookup"><span data-stu-id="1e560-261">See [Run Hive queries using .NET SDK](hdinsight-hadoop-use-hive-dotnet-sdk.md).</span></span> <span data-ttu-id="1e560-262">Ebben a cikkben hello kódrészletet hello interaktív hitelesítési módszert használja.</span><span class="sxs-lookup"><span data-stu-id="1e560-262">hello code snippet in this article uses hello interactive authentication approach.</span></span> |
| <span data-ttu-id="1e560-263">Alkalmazások nem interaktív .NET SDK-val az Azure Active Directory használatával hitelesíti</span><span class="sxs-lookup"><span data-stu-id="1e560-263">Authenticate applications non-interactively using Azure Active Directory with .NET SDK</span></span> |<span data-ttu-id="1e560-264">Lásd: [a HDInsight nem interaktív alkalmazások létrehozása](hdinsight-create-non-interactive-authentication-dotnet-applications.md)</span><span class="sxs-lookup"><span data-stu-id="1e560-264">See [Create non-interactive applications for HDInsight](hdinsight-create-non-interactive-authentication-dotnet-applications.md)</span></span> |
| <span data-ttu-id="1e560-265">.NET SDK használatával Hive feladat elküldése</span><span class="sxs-lookup"><span data-stu-id="1e560-265">Submit a Hive job using .NET SDK</span></span> |<span data-ttu-id="1e560-266">Lásd: [elküldeni a Hive-feladatok](hdinsight-hadoop-use-hive-dotnet-sdk.md)</span><span class="sxs-lookup"><span data-stu-id="1e560-266">See [Submit Hive jobs](hdinsight-hadoop-use-hive-dotnet-sdk.md)</span></span> |
| <span data-ttu-id="1e560-267">Elküldeni a Pig feladatot .NET SDK használatával</span><span class="sxs-lookup"><span data-stu-id="1e560-267">Submit a Pig job using .NET SDK</span></span> |<span data-ttu-id="1e560-268">Lásd: [elküldeni a Pig-feladatokhoz](hdinsight-hadoop-use-pig-dotnet-sdk.md)</span><span class="sxs-lookup"><span data-stu-id="1e560-268">See [Submit Pig jobs](hdinsight-hadoop-use-pig-dotnet-sdk.md)</span></span> |
| <span data-ttu-id="1e560-269">.NET SDK használatával Sqoop feladat elküldése</span><span class="sxs-lookup"><span data-stu-id="1e560-269">Submit a Sqoop job using .NET SDK</span></span> |<span data-ttu-id="1e560-270">Lásd: [nyújt Sqoop feladatok](hdinsight-hadoop-use-sqoop-dotnet-sdk.md)</span><span class="sxs-lookup"><span data-stu-id="1e560-270">See [Submit Sqoop jobs](hdinsight-hadoop-use-sqoop-dotnet-sdk.md)</span></span> |
| <span data-ttu-id="1e560-271">Lista HDInsight-fürtök .NET SDK használatával</span><span class="sxs-lookup"><span data-stu-id="1e560-271">List HDInsight clusters using .NET SDK</span></span> |<span data-ttu-id="1e560-272">Lásd: [lista HDInsight-fürtök](hdinsight-administer-use-dotnet-sdk.md#list-clusters)</span><span class="sxs-lookup"><span data-stu-id="1e560-272">See [List HDInsight clusters](hdinsight-administer-use-dotnet-sdk.md#list-clusters)</span></span> |
| <span data-ttu-id="1e560-273">A HDInsight-fürtök .NET SDK használatával méretezése</span><span class="sxs-lookup"><span data-stu-id="1e560-273">Scale HDInsight clusters using .NET SDK</span></span> |<span data-ttu-id="1e560-274">Lásd: [méretezési HDInsight-fürtök](hdinsight-administer-use-dotnet-sdk.md#scale-clusters)</span><span class="sxs-lookup"><span data-stu-id="1e560-274">See [Scale HDInsight clusters](hdinsight-administer-use-dotnet-sdk.md#scale-clusters)</span></span> |
| <span data-ttu-id="1e560-275">GRANT/revoke access tooHDInsight fürtök .NET SDK használatával</span><span class="sxs-lookup"><span data-stu-id="1e560-275">Grant/revoke access tooHDInsight clusters using .NET SDK</span></span> |<span data-ttu-id="1e560-276">Lásd: [Grant/revoke access tooHDInsight fürtök](hdinsight-administer-use-dotnet-sdk.md#grantrevoke-access)</span><span class="sxs-lookup"><span data-stu-id="1e560-276">See [Grant/revoke access tooHDInsight clusters](hdinsight-administer-use-dotnet-sdk.md#grantrevoke-access)</span></span> |
| <span data-ttu-id="1e560-277">HTTP felhasználó hitelesítő adatait a HDInsight-fürtök .NET SDK használatával</span><span class="sxs-lookup"><span data-stu-id="1e560-277">Update HTTP user credentials for HDInsight clusters using .NET SDK</span></span> |<span data-ttu-id="1e560-278">Lásd: [frissítés HTTP felhasználói hitelesítő adatokat a HDInsight-fürtök](hdinsight-administer-use-dotnet-sdk.md#update-http-user-credentials)</span><span class="sxs-lookup"><span data-stu-id="1e560-278">See [Update HTTP user credentials for HDInsight clusters](hdinsight-administer-use-dotnet-sdk.md#update-http-user-credentials)</span></span> |
| <span data-ttu-id="1e560-279">Hello alapértelmezett tárfiók keresése a HDInsight-fürtök .NET SDK használatával</span><span class="sxs-lookup"><span data-stu-id="1e560-279">Find hello default storage account for HDInsight clusters using .NET SDK</span></span> |<span data-ttu-id="1e560-280">Lásd: [hello alapértelmezett tárfiók keresése a HDInsight-fürtök](hdinsight-administer-use-dotnet-sdk.md#find-the-default-storage-account)</span><span class="sxs-lookup"><span data-stu-id="1e560-280">See [Find hello default storage account for HDInsight clusters](hdinsight-administer-use-dotnet-sdk.md#find-the-default-storage-account)</span></span> |
| <span data-ttu-id="1e560-281">.NET SDK használatával a HDInsight-fürtök törlése</span><span class="sxs-lookup"><span data-stu-id="1e560-281">Delete HDInsight clusters using .NET SDK</span></span> |<span data-ttu-id="1e560-282">Lásd: [törlése HDInsight-fürtök](hdinsight-administer-use-dotnet-sdk.md#delete-clusters)</span><span class="sxs-lookup"><span data-stu-id="1e560-282">See [Delete HDInsight clusters](hdinsight-administer-use-dotnet-sdk.md#delete-clusters)</span></span> |

### <a name="examples"></a><span data-ttu-id="1e560-283">Példák</span><span class="sxs-lookup"><span data-stu-id="1e560-283">Examples</span></span>
<span data-ttu-id="1e560-284">A következő példák a hogyan van egy művelet hello ARM-alapú SDK a hello ASM-alapú SDK és hello egyenértékű kódrészletet használatával történik.</span><span class="sxs-lookup"><span data-stu-id="1e560-284">Following are some examples on how an operation is performed using hello ASM-based SDK and hello equivalent code snippet for hello ARM-based SDK.</span></span>

<span data-ttu-id="1e560-285">**A fürt CRUD-ügyfél létrehozása**</span><span class="sxs-lookup"><span data-stu-id="1e560-285">**Creating a cluster CRUD client**</span></span>

* <span data-ttu-id="1e560-286">Régi parancs (ASM)</span><span class="sxs-lookup"><span data-stu-id="1e560-286">Old command (ASM)</span></span>
  
        //Certificate auth
        //This logs hello application in using a subscription administration certificate, which is not offered in Azure Resource Manager (ARM)
  
        const string subid = "454467d4-60ca-4dfd-a556-216eeeeeeee1";
        var cred = new HDInsightCertificateCredential(new Guid(subid), new X509Certificate2(@"path\to\certificate.cer"));
        var client = HDInsightClient.Connect(cred);
* <span data-ttu-id="1e560-287">Új parancs (ARM) (egyszerű szolgáltatás engedélyezése)</span><span class="sxs-lookup"><span data-stu-id="1e560-287">New command (ARM) (Service principal authorization)</span></span>
  
        //Service principal auth
        //This will log hello application in as itself, rather than on behalf of a specific user.
        //For details, including how tooset up hello application, see:
        //   https://azure.microsoft.com/en-us/documentation/articles/hdinsight-create-non-interactive-authentication-dotnet-applications/
  
        var authFactory = new AuthenticationFactory();
  
        var account = new AzureAccount { Type = AzureAccount.AccountType.ServicePrincipal, Id = clientId };
  
        var env = AzureEnvironment.PublicEnvironments[EnvironmentName.AzureCloud];
  
        var accessToken = authFactory.Authenticate(account, env, tenantId, secretKey, ShowDialog.Never).AccessToken;
  
        var creds = new TokenCloudCredentials(subId.ToString(), accessToken);
  
        _hdiManagementClient = new HDInsightManagementClient(creds);
* <span data-ttu-id="1e560-288">Új parancs (ARM) (felhasználói engedélyezési)</span><span class="sxs-lookup"><span data-stu-id="1e560-288">New command (ARM) (User authorization)</span></span>
  
        //User auth
        //This will log hello application in on behalf of hello user.
        //hello end-user will see a login popup.
  
        var authFactory = new AuthenticationFactory();
  
        var account = new AzureAccount { Type = AzureAccount.AccountType.User, Id = username };
  
        var env = AzureEnvironment.PublicEnvironments[EnvironmentName.AzureCloud];
  
        var accessToken = authFactory.Authenticate(account, env, AuthenticationFactory.CommonAdTenant, password, ShowDialog.Auto).AccessToken;
  
        var creds = new TokenCloudCredentials(subId.ToString(), accessToken);
  
        _hdiManagementClient = new HDInsightManagementClient(creds);

<span data-ttu-id="1e560-289">**Fürt létrehozása**</span><span class="sxs-lookup"><span data-stu-id="1e560-289">**Creating a cluster**</span></span>

* <span data-ttu-id="1e560-290">Régi parancs (ASM)</span><span class="sxs-lookup"><span data-stu-id="1e560-290">Old command (ASM)</span></span>
  
        var clusterInfo = new ClusterCreateParameters
                    {
                        Name = dnsName,
                        DefaultStorageAccountKey = key,
                        DefaultStorageContainer = defaultStorageContainer,
                        DefaultStorageAccountName = storageAccountDnsName,
                        ClusterSizeInNodes = 1,
                        ClusterType = type,
                        Location = "West US",
                        UserName = "admin",
                        Password = "*******",
                        Version = version,
                        HeadNodeSize = NodeVMSize.Large,
                    };
        clusterInfo.CoreConfiguration.Add(new KeyValuePair<string, string>("config1", "value1"));
        client.CreateCluster(clusterInfo);
* <span data-ttu-id="1e560-291">Új parancs (ARM)</span><span class="sxs-lookup"><span data-stu-id="1e560-291">New command (ARM)</span></span>
  
        var clusterCreateParameters = new ClusterCreateParameters
            {
                Location = "West US",
                ClusterType = "Hadoop",
                Version = "3.1",
                OSType = OSType.Linux,
                DefaultStorageAccountName = "mystorage.blob.core.windows.net",
                DefaultStorageAccountKey =
                    "O9EQvp3A3AjXq/W27rst1GQfLllhp0gUeiUUn2D8zX2lU3taiXSSfqkZlcPv+nQcYUxYw==",
                UserName = "hadoopuser",
                Password = "*******",
                HeadNodeSize = "ExtraLarge",
                RdpUsername = "hdirp",
                RdpPassword = ""*******",
                RdpAccessExpiry = new DateTime(2025, 3, 1),
                ClusterSizeInNodes = 5
            };
        var coreConfigs = new Dictionary<string, string> {{"config1", "value1"}};
        clusterCreateParameters.Configurations.Add(ConfigurationKey.CoreSite, coreConfigs);

<span data-ttu-id="1e560-292">**HTTP-hozzáférés engedélyezése**</span><span class="sxs-lookup"><span data-stu-id="1e560-292">**Enabling HTTP access**</span></span>

* <span data-ttu-id="1e560-293">Régi parancs (ASM)</span><span class="sxs-lookup"><span data-stu-id="1e560-293">Old command (ASM)</span></span>
  
        client.EnableHttp(dnsName, "West US", "admin", "*******");
* <span data-ttu-id="1e560-294">Új parancs (ARM)</span><span class="sxs-lookup"><span data-stu-id="1e560-294">New command (ARM)</span></span>
  
        var httpParams = new HttpSettingsParameters
        {
               HttpUserEnabled = true,
               HttpUsername = "admin",
               HttpPassword = "*******",
        };
        client.Clusters.ConfigureHttpSettings(resourceGroup, dnsname, httpParams);

<span data-ttu-id="1e560-295">**A fürtök törlésével**</span><span class="sxs-lookup"><span data-stu-id="1e560-295">**Deleting a cluster**</span></span>

* <span data-ttu-id="1e560-296">Régi parancs (ASM)</span><span class="sxs-lookup"><span data-stu-id="1e560-296">Old command (ASM)</span></span>
  
        client.DeleteCluster(dnsName);
* <span data-ttu-id="1e560-297">Új parancs (ARM)</span><span class="sxs-lookup"><span data-stu-id="1e560-297">New command (ARM)</span></span>
  
        client.Clusters.Delete(resourceGroup, dnsname);

