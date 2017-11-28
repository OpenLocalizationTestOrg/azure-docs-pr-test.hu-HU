---
title: "A HDInsight az Azure Resource Manager eszközeinek át |} Microsoft Docs"
description: "Azure Resource Manager Fejlesztőeszközök a HDInsight-fürtök áttelepítése"
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
ms.openlocfilehash: 708d22b9ce53d4dbc07c6bcde0c46dcd238291bb
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="migrating-to-azure-resource-manager-based-development-tools-for-hdinsight-clusters"></a><span data-ttu-id="1219a-103">A Fejlesztőeszközök Azure Resource Manager-alapú HDInsight-fürtök áttelepítése</span><span class="sxs-lookup"><span data-stu-id="1219a-103">Migrating to Azure Resource Manager-based development tools for HDInsight clusters</span></span>

<span data-ttu-id="1219a-104">A HDInsight-ból kivezettük való Azure Service Manager ASM-alapú eszközök hdinsight.</span><span class="sxs-lookup"><span data-stu-id="1219a-104">HDInsight is deprecating Azure Service Manager (ASM)-based tools for HDInsight.</span></span> <span data-ttu-id="1219a-105">Ha használta az Azure PowerShell, az Azure parancssori felület vagy a HDInsight .NET SDK a HDInsight-fürtök együttműködni, hosszúan az Azure Resource Manager ARM-alapú verziói PowerShell parancssori felület és továbbítja a .NET SDK használatával.</span><span class="sxs-lookup"><span data-stu-id="1219a-105">If you have been using Azure PowerShell, Azure CLI, or the HDInsight .NET SDK to work with HDInsight clusters, you are encouraged to use the Azure Resource Manager (ARM)-based versions of PowerShell, CLI, and .NET SDK going forward.</span></span> <span data-ttu-id="1219a-106">Ez a cikk mutatók áttelepítése az új ARM-alapú módszert biztosít.</span><span class="sxs-lookup"><span data-stu-id="1219a-106">This article provides pointers on how to migrate to the new ARM-based approach.</span></span> <span data-ttu-id="1219a-107">Megfelelő esetben ez a cikk is rámutat, a címterület-kezelési és ARM közötti különbségekről megközelítések hdinsight.</span><span class="sxs-lookup"><span data-stu-id="1219a-107">Wherever applicable, this article also points out the differences between the ASM and ARM approaches for HDInsight.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="1219a-108">A címterület-kezelési támogatás PowerShell parancssori felület, és a .NET SDK nem küld a **2017. január 1.**.</span><span class="sxs-lookup"><span data-stu-id="1219a-108">The support for ASM based PowerShell, CLI, and .NET SDK will discontinue on **January 1, 2017**.</span></span>
> 
> 

## <a name="migrating-azure-cli-to-azure-resource-manager"></a><span data-ttu-id="1219a-109">Az Azure CLI áttelepítése az Azure Resource Manager számára</span><span class="sxs-lookup"><span data-stu-id="1219a-109">Migrating Azure CLI to Azure Resource Manager</span></span>
<span data-ttu-id="1219a-110">Az Azure parancssori felület most alapértelmezés szerint az Azure Resource Managerrel (ARM) módot, kivéve, ha frissíti a korábbi telepítés; Ebben az esetben előfordulhat, hogy kell használnia a `azure config mode arm` parancs futtatásával váltson az ARM üzemmódban.</span><span class="sxs-lookup"><span data-stu-id="1219a-110">The Azure CLI now defaults to Azure Resource Manager (ARM) mode, unless you are upgrading from a previous installation; in this case, you may need to use the `azure config mode arm` command to switch to ARM mode.</span></span>

<span data-ttu-id="1219a-111">Az alapszintű Azure Service Management (ASM) használatával történő együttműködésre a HDInsight az Azure parancssori felület megadott parancsok esetén ugyanazt az ARM; azonban bizonyos paraméterek és kapcsolók előfordulhat, hogy új neve lehet, és nincsenek sok új paraméter elérhető ARM használatakor.</span><span class="sxs-lookup"><span data-stu-id="1219a-111">The basic commands that the Azure CLI provided to work with HDInsight using Azure Service Management (ASM) are the same when using ARM; however some parameters and switches may have new names, and there are many new parameters available when using ARM.</span></span> <span data-ttu-id="1219a-112">Például használhatja `azure hdinsight cluster create` a Azure virtuális hálózat, amely egy fürt, létre kell hozni, vagy Hive- és Oozie metaadattárhoz megadásához.</span><span class="sxs-lookup"><span data-stu-id="1219a-112">For example, you can now use `azure hdinsight cluster create` to specify the Azure Virtual Network that a cluster should be created in, or Hive and Oozie metastore information.</span></span>

<span data-ttu-id="1219a-113">A HDInsight Azure Resource Manageren keresztül használatához alapvető parancsok a következők:</span><span class="sxs-lookup"><span data-stu-id="1219a-113">Basic commands for working with HDInsight through Azure Resource Manager are:</span></span>

* <span data-ttu-id="1219a-114">`azure hdinsight cluster create`-hoz létre egy új HDInsight-fürt</span><span class="sxs-lookup"><span data-stu-id="1219a-114">`azure hdinsight cluster create` - creates a new HDInsight cluster</span></span>
* <span data-ttu-id="1219a-115">`azure hdinsight cluster delete`– egy meglévő HDInsight-fürt törlése</span><span class="sxs-lookup"><span data-stu-id="1219a-115">`azure hdinsight cluster delete` - deletes an existing HDInsight cluster</span></span>
* <span data-ttu-id="1219a-116">`azure hdinsight cluster show`– egy meglévő fürthöz kapcsolatos információk megjelenítése</span><span class="sxs-lookup"><span data-stu-id="1219a-116">`azure hdinsight cluster show` - display information about an existing cluster</span></span>
* <span data-ttu-id="1219a-117">`azure hdinsight cluster list`– ismerteti az Azure-előfizetés a HDInsight-fürtök</span><span class="sxs-lookup"><span data-stu-id="1219a-117">`azure hdinsight cluster list` - lists HDInsight clusters for your Azure subscription</span></span>

<span data-ttu-id="1219a-118">Használja a `-h` kapcsolót, hogy a paraméterek és kapcsolók érhető el minden egyes parancsnál.</span><span class="sxs-lookup"><span data-stu-id="1219a-118">Use the `-h` switch to inspect the parameters and switches available for each command.</span></span>

### <a name="new-commands"></a><span data-ttu-id="1219a-119">Új parancsok</span><span class="sxs-lookup"><span data-stu-id="1219a-119">New commands</span></span>
<span data-ttu-id="1219a-120">Elérhető az Azure Resource Manager új parancsok a következők:</span><span class="sxs-lookup"><span data-stu-id="1219a-120">New commands available with Azure Resource Manager are:</span></span>

* <span data-ttu-id="1219a-121">`azure hdinsight cluster resize`-dinamikusan változik adhatja meg a fürt feldolgozó csomópontjainak számát</span><span class="sxs-lookup"><span data-stu-id="1219a-121">`azure hdinsight cluster resize` - dynamically changes the number of worker nodes in the cluster</span></span>
* <span data-ttu-id="1219a-122">`azure hdinsight cluster enable-http-access`-HTTPs-hozzáférést biztosít a fürt (az alapértelmezés)</span><span class="sxs-lookup"><span data-stu-id="1219a-122">`azure hdinsight cluster enable-http-access` - enables HTTPs access to the cluster (on by default)</span></span>
* <span data-ttu-id="1219a-123">`azure hdinsight cluster disable-http-access`-letiltja a HTTPs-hozzáférés a fürthöz</span><span class="sxs-lookup"><span data-stu-id="1219a-123">`azure hdinsight cluster disable-http-access` - disables HTTPs access to the cluster</span></span>
* <span data-ttu-id="1219a-124">`azure hdinsight script-action`-parancsokat biztosít létrehozása vagy kezelése Parancsfájlműveletek egy fürtön</span><span class="sxs-lookup"><span data-stu-id="1219a-124">`azure hdinsight script-action` - provides commands for creating/managing Script Actions on a cluster</span></span>
* <span data-ttu-id="1219a-125">`azure hdinsight config`-parancsok biztosít a konfigurációs fájl létrehozása, amely használható a `hdinsight cluster create` parancs használatával adja meg a konfigurációs adatokat.</span><span class="sxs-lookup"><span data-stu-id="1219a-125">`azure hdinsight config` - provides commands for creating a configuration file that can be used with the `hdinsight cluster create` command to provide configuration information.</span></span>

### <a name="deprecated-commands"></a><span data-ttu-id="1219a-126">Elavult parancsok</span><span class="sxs-lookup"><span data-stu-id="1219a-126">Deprecated commands</span></span>
<span data-ttu-id="1219a-127">Ha használja a `azure hdinsight job` parancsok elküldeni a HDInsight-fürtjét, feladatok ezek nem állnak rendelkezésre az ARM-parancsok használatával.</span><span class="sxs-lookup"><span data-stu-id="1219a-127">If you use the `azure hdinsight job` commands to submit jobs to your HDInsight cluster, these are not available through the ARM commands.</span></span> <span data-ttu-id="1219a-128">Ha programozottan feladatok elküldéséhez a HDInsight parancsfájlok van szüksége, helyette használja a HDInsight által biztosított REST API-k.</span><span class="sxs-lookup"><span data-stu-id="1219a-128">If you need to programmatically submit jobs to HDInsight from scripts, you should instead use the REST APIs provided by HDInsight.</span></span> <span data-ttu-id="1219a-129">A REST API-k használatával feladatok elküldésekor további információkért lásd a következő dokumentumokat.</span><span class="sxs-lookup"><span data-stu-id="1219a-129">For more information on submitting jobs using REST APIs, see the following documents.</span></span>

* [<span data-ttu-id="1219a-130">Hadoop MapReduce-feladatok a HDInsight használata cURL használatával futtassa</span><span class="sxs-lookup"><span data-stu-id="1219a-130">Run MapReduce jobs with Hadoop on HDInsight using cURL</span></span>](hdinsight-hadoop-use-mapreduce-curl.md)
* [<span data-ttu-id="1219a-131">A Hadoop Hive-lekérdezések futtatása a HDInsight használata cURL használatával</span><span class="sxs-lookup"><span data-stu-id="1219a-131">Run Hive queries with Hadoop on HDInsight using cURL</span></span>](hdinsight-hadoop-use-hive-curl.md)
* [<span data-ttu-id="1219a-132">Pig-feladatokhoz Hadoop on HDInsight használata cURL használatával futtassa</span><span class="sxs-lookup"><span data-stu-id="1219a-132">Run Pig jobs with Hadoop on HDInsight using cURL</span></span>](hdinsight-hadoop-use-pig-curl.md)

<span data-ttu-id="1219a-133">Információk más módjairól MapReduce futtatásához, struktúra, és interaktív módon sertésfelmérés, lásd: [használata MapReduce a Hadoop on HDInsight](hdinsight-use-mapreduce.md), [használata a hdinsight Hadoop Hive](hdinsight-use-hive.md), és [a Pig használata a hadooppal HDInsight](hdinsight-use-pig.md).</span><span class="sxs-lookup"><span data-stu-id="1219a-133">For information on other ways to run MapReduce, Hive, and Pig interactively, see [Use MapReduce with Hadoop on HDInsight](hdinsight-use-mapreduce.md), [Use Hive with Hadoop on HDInsight](hdinsight-use-hive.md), and [Use Pig with Hadoop on HDInsight](hdinsight-use-pig.md).</span></span>

### <a name="examples"></a><span data-ttu-id="1219a-134">Példák</span><span class="sxs-lookup"><span data-stu-id="1219a-134">Examples</span></span>
<span data-ttu-id="1219a-135">**Fürt létrehozása**</span><span class="sxs-lookup"><span data-stu-id="1219a-135">**Creating a cluster**</span></span>

* <span data-ttu-id="1219a-136">Régi parancs (ASM)-`azure hdinsight cluster create myhdicluster --location northeurope --osType linux --storageAccountName mystorage --storageAccountKey <storagekey> --storageContainer mycontainer --userName admin --password mypassword --sshUserName sshuser --sshPassword mypassword`</span><span class="sxs-lookup"><span data-stu-id="1219a-136">Old command (ASM) - `azure hdinsight cluster create myhdicluster --location northeurope --osType linux --storageAccountName mystorage --storageAccountKey <storagekey> --storageContainer mycontainer --userName admin --password mypassword --sshUserName sshuser --sshPassword mypassword`</span></span>
* <span data-ttu-id="1219a-137">Új parancs (ARM)-`azure hdinsight cluster create myhdicluster -g myresourcegroup --location northeurope --osType linux --clusterType hadoop --defaultStorageAccountName mystorage --defaultStorageAccountKey <storagekey> --defaultStorageContainer mycontainer --userName admin -password mypassword --sshUserName sshuser --sshPassword mypassword`</span><span class="sxs-lookup"><span data-stu-id="1219a-137">New command (ARM) - `azure hdinsight cluster create myhdicluster -g myresourcegroup --location northeurope --osType linux --clusterType hadoop --defaultStorageAccountName mystorage --defaultStorageAccountKey <storagekey> --defaultStorageContainer mycontainer --userName admin -password mypassword --sshUserName sshuser --sshPassword mypassword`</span></span>

<span data-ttu-id="1219a-138">**A fürtök törlésével**</span><span class="sxs-lookup"><span data-stu-id="1219a-138">**Deleting a cluster**</span></span>

* <span data-ttu-id="1219a-139">Régi parancs (ASM)-`azure hdinsight cluster delete myhdicluster`</span><span class="sxs-lookup"><span data-stu-id="1219a-139">Old command (ASM) - `azure hdinsight cluster delete myhdicluster`</span></span>
* <span data-ttu-id="1219a-140">Új parancs (ARM)-`azure hdinsight cluster delete mycluster -g myresourcegroup`</span><span class="sxs-lookup"><span data-stu-id="1219a-140">New command (ARM) - `azure hdinsight cluster delete mycluster -g myresourcegroup`</span></span>

<span data-ttu-id="1219a-141">**Lista fürtök**</span><span class="sxs-lookup"><span data-stu-id="1219a-141">**List clusters**</span></span>

* <span data-ttu-id="1219a-142">Régi parancs (ASM)-`azure hdinsight cluster list`</span><span class="sxs-lookup"><span data-stu-id="1219a-142">Old command (ASM) - `azure hdinsight cluster list`</span></span>
* <span data-ttu-id="1219a-143">Új parancs (ARM)-`azure hdinsight cluster list`</span><span class="sxs-lookup"><span data-stu-id="1219a-143">New command (ARM) - `azure hdinsight cluster list`</span></span>

> [!NOTE]
> <span data-ttu-id="1219a-144">A parancs megadásával az erőforrás csoport használatával `-g` csak a fürtök ad vissza a megadott erőforráscsoportban.</span><span class="sxs-lookup"><span data-stu-id="1219a-144">For the list command, specifying the resource group using `-g` will return only the clusters in the specified resource group.</span></span>
> 
> 

<span data-ttu-id="1219a-145">**Fürt információ megjelenítése**</span><span class="sxs-lookup"><span data-stu-id="1219a-145">**Show cluster information**</span></span>

* <span data-ttu-id="1219a-146">Régi parancs (ASM)-`azure hdinsight cluster show myhdicluster`</span><span class="sxs-lookup"><span data-stu-id="1219a-146">Old command (ASM) - `azure hdinsight cluster show myhdicluster`</span></span>
* <span data-ttu-id="1219a-147">Új parancs (ARM)-`azure hdinsight cluster show myhdicluster -g myresourcegroup`</span><span class="sxs-lookup"><span data-stu-id="1219a-147">New command (ARM) - `azure hdinsight cluster show myhdicluster -g myresourcegroup`</span></span>

## <a name="migrating-azure-powershell-to-azure-resource-manager"></a><span data-ttu-id="1219a-148">Az Azure PowerShell áttelepítése az Azure Resource Manager számára</span><span class="sxs-lookup"><span data-stu-id="1219a-148">Migrating Azure PowerShell to Azure Resource Manager</span></span>
<span data-ttu-id="1219a-149">Az Azure PowerShell általános információk az Azure Resource Managerrel (ARM) módban található [az Azure PowerShell használata Azure Resource Managerrel](../powershell-azure-resource-manager.md).</span><span class="sxs-lookup"><span data-stu-id="1219a-149">The general information about Azure PowerShell in the Azure Resource Manager (ARM) mode can be found at [Using Azure PowerShell with Azure Resource Manager](../powershell-azure-resource-manager.md).</span></span>

<span data-ttu-id="1219a-150">Az Azure PowerShell ARM-parancsmagok telepített-mellé a címterület-kezelési parancsmagokkal is lehet.</span><span class="sxs-lookup"><span data-stu-id="1219a-150">The Azure PowerShell ARM cmdlets can be installed side-by-side with the ASM cmdlets.</span></span> <span data-ttu-id="1219a-151">A két mód a parancsmag elkülönítsék névvel.</span><span class="sxs-lookup"><span data-stu-id="1219a-151">The cmdlets from the two modes can be distinguished by their names.</span></span>  <span data-ttu-id="1219a-152">Az ARM üzemmódban van *AzureRmHDInsight* való hasonlítás parancsmag nevében szereplő *AzureHDInsight* a címterület-kezelési módban.</span><span class="sxs-lookup"><span data-stu-id="1219a-152">The ARM mode has *AzureRmHDInsight* in the cmdlet names comparing to *AzureHDInsight* in the ASM mode.</span></span>  <span data-ttu-id="1219a-153">Például *New-AzureRmHDInsightCluster* vs. *Új AzureHDInsightCluster*.</span><span class="sxs-lookup"><span data-stu-id="1219a-153">For example, *New-AzureRmHDInsightCluster* vs. *New-AzureHDInsightCluster*.</span></span> <span data-ttu-id="1219a-154">Paraméterek és kapcsolók hírek nevük lehet, hogy legyen, és nincsenek sok új paraméter elérhető ARM használatakor.</span><span class="sxs-lookup"><span data-stu-id="1219a-154">Parameters and switches may have news names, and there are many new parameters available when using ARM.</span></span>  <span data-ttu-id="1219a-155">Például több parancsmagok szükséges nevű új kapcsoló *- ResourceGroupName*.</span><span class="sxs-lookup"><span data-stu-id="1219a-155">For example, several cmdlets require a new switch called *-ResourceGroupName*.</span></span> 

<span data-ttu-id="1219a-156">A HDInsight-parancsmagokat használhatja, csatlakozzon az Azure-fiókjával, és az új erőforráscsoport létrehozása:</span><span class="sxs-lookup"><span data-stu-id="1219a-156">Before you can use the HDInsight cmdlets, you must connect to your Azure account, and create a new resource group:</span></span>

* <span data-ttu-id="1219a-157">Login-AzureRmAccount vagy [válasszon-AzureRmProfile](https://msdn.microsoft.com/library/mt619310.aspx).</span><span class="sxs-lookup"><span data-stu-id="1219a-157">Login-AzureRmAccount or [Select-AzureRmProfile](https://msdn.microsoft.com/library/mt619310.aspx).</span></span> <span data-ttu-id="1219a-158">Lásd: [hitelesítéséhez az Azure Resource Manager egyszerű szolgáltatás](../azure-resource-manager/resource-group-authenticate-service-principal.md)</span><span class="sxs-lookup"><span data-stu-id="1219a-158">See [Authenticating a service principal with Azure Resource Manager](../azure-resource-manager/resource-group-authenticate-service-principal.md)</span></span>
* [<span data-ttu-id="1219a-159">Új-AzureRmResourceGroup</span><span class="sxs-lookup"><span data-stu-id="1219a-159">New-AzureRmResourceGroup</span></span>](https://msdn.microsoft.com/library/mt603739.aspx)

### <a name="renamed-cmdlets"></a><span data-ttu-id="1219a-160">Átnevezett parancsmagok</span><span class="sxs-lookup"><span data-stu-id="1219a-160">Renamed cmdlets</span></span>
<span data-ttu-id="1219a-161">A HDInsight ASM-parancsmagok a Windows PowerShell-konzolban listázásához:</span><span class="sxs-lookup"><span data-stu-id="1219a-161">To list the HDInsight ASM cmdlets in Windows PowerShell console:</span></span>

    help *azurermhdinsight*

<span data-ttu-id="1219a-162">A következő táblázat a címterület-kezelési parancsmagok és a nevek a ARM üzemmódban.</span><span class="sxs-lookup"><span data-stu-id="1219a-162">The following table lists the ASM cmdlets and their names in the ARM mode:</span></span>

| <span data-ttu-id="1219a-163">Címterület-kezelési parancsmagok</span><span class="sxs-lookup"><span data-stu-id="1219a-163">ASM cmdlets</span></span> | <span data-ttu-id="1219a-164">ARM-parancsmagok</span><span class="sxs-lookup"><span data-stu-id="1219a-164">ARM cmdlets</span></span> |
| --- | --- |
| <span data-ttu-id="1219a-165">Add-AzureHDInsightConfigValues</span><span class="sxs-lookup"><span data-stu-id="1219a-165">Add-AzureHDInsightConfigValues</span></span> |[<span data-ttu-id="1219a-166">Adja hozzá AzureRmHDInsightConfigValues</span><span class="sxs-lookup"><span data-stu-id="1219a-166">Add-AzureRmHDInsightConfigValues</span></span>](https://msdn.microsoft.com/library/mt603530.aspx) |
| <span data-ttu-id="1219a-167">Add-AzureHDInsightMetastore</span><span class="sxs-lookup"><span data-stu-id="1219a-167">Add-AzureHDInsightMetastore</span></span> |[<span data-ttu-id="1219a-168">Adja hozzá AzureRmHDInsightMetastore</span><span class="sxs-lookup"><span data-stu-id="1219a-168">Add-AzureRmHDInsightMetastore</span></span>](https://msdn.microsoft.com/library/mt603670.aspx) |
| <span data-ttu-id="1219a-169">Add-AzureHDInsightScriptAction</span><span class="sxs-lookup"><span data-stu-id="1219a-169">Add-AzureHDInsightScriptAction</span></span> |[<span data-ttu-id="1219a-170">Adja hozzá AzureRmHDInsightScriptAction</span><span class="sxs-lookup"><span data-stu-id="1219a-170">Add-AzureRmHDInsightScriptAction</span></span>](https://msdn.microsoft.com/library/mt603527.aspx) |
| <span data-ttu-id="1219a-171">Add-AzureHDInsightStorage</span><span class="sxs-lookup"><span data-stu-id="1219a-171">Add-AzureHDInsightStorage</span></span> |[<span data-ttu-id="1219a-172">Adja hozzá AzureRmHDInsightStorage</span><span class="sxs-lookup"><span data-stu-id="1219a-172">Add-AzureRmHDInsightStorage</span></span>](https://msdn.microsoft.com/library/mt619445.aspx) |
| <span data-ttu-id="1219a-173">Get-AzureHDInsightCluster</span><span class="sxs-lookup"><span data-stu-id="1219a-173">Get-AzureHDInsightCluster</span></span> |[<span data-ttu-id="1219a-174">Get-AzureRmHDInsightCluster</span><span class="sxs-lookup"><span data-stu-id="1219a-174">Get-AzureRmHDInsightCluster</span></span>](https://msdn.microsoft.com/library/mt619371.aspx) |
| <span data-ttu-id="1219a-175">Get-AzureHDInsightJob</span><span class="sxs-lookup"><span data-stu-id="1219a-175">Get-AzureHDInsightJob</span></span> |[<span data-ttu-id="1219a-176">Get-AzureRmHDInsightJob</span><span class="sxs-lookup"><span data-stu-id="1219a-176">Get-AzureRmHDInsightJob</span></span>](https://msdn.microsoft.com/library/mt603590.aspx) |
| <span data-ttu-id="1219a-177">Get-AzureHDInsightJobOutput</span><span class="sxs-lookup"><span data-stu-id="1219a-177">Get-AzureHDInsightJobOutput</span></span> |[<span data-ttu-id="1219a-178">Get-AzureRmHDInsightJobOutput</span><span class="sxs-lookup"><span data-stu-id="1219a-178">Get-AzureRmHDInsightJobOutput</span></span>](https://msdn.microsoft.com/library/mt603793.aspx) |
| <span data-ttu-id="1219a-179">Get-AzureHDInsightProperties</span><span class="sxs-lookup"><span data-stu-id="1219a-179">Get-AzureHDInsightProperties</span></span> |[<span data-ttu-id="1219a-180">Get-AzureRmHDInsightProperties</span><span class="sxs-lookup"><span data-stu-id="1219a-180">Get-AzureRmHDInsightProperties</span></span>](https://msdn.microsoft.com/library/mt603546.aspx) |
| <span data-ttu-id="1219a-181">Grant-AzureHDInsightHttpServicesAccess</span><span class="sxs-lookup"><span data-stu-id="1219a-181">Grant-AzureHDInsightHttpServicesAccess</span></span> |[<span data-ttu-id="1219a-182">Támogatás-AzureRmHDInsightHttpServicesAccess</span><span class="sxs-lookup"><span data-stu-id="1219a-182">Grant-AzureRmHDInsightHttpServicesAccess</span></span>](https://msdn.microsoft.com/library/mt619407.aspx) |
| <span data-ttu-id="1219a-183">Grant-AzureHdinsightRdpAccess</span><span class="sxs-lookup"><span data-stu-id="1219a-183">Grant-AzureHdinsightRdpAccess</span></span> |[<span data-ttu-id="1219a-184">Támogatás-AzureRmHDInsightRdpServicesAccess</span><span class="sxs-lookup"><span data-stu-id="1219a-184">Grant-AzureRmHDInsightRdpServicesAccess</span></span>](https://msdn.microsoft.com/library/mt603717.aspx) |
| <span data-ttu-id="1219a-185">Invoke-AzureHDInsightHiveJob</span><span class="sxs-lookup"><span data-stu-id="1219a-185">Invoke-AzureHDInsightHiveJob</span></span> |[<span data-ttu-id="1219a-186">Invoke-AzureRmHDInsightHiveJob</span><span class="sxs-lookup"><span data-stu-id="1219a-186">Invoke-AzureRmHDInsightHiveJob</span></span>](https://msdn.microsoft.com/library/mt603593.aspx) |
| <span data-ttu-id="1219a-187">New-AzureHDInsightCluster</span><span class="sxs-lookup"><span data-stu-id="1219a-187">New-AzureHDInsightCluster</span></span> |[<span data-ttu-id="1219a-188">Új AzureRmHDInsightCluster</span><span class="sxs-lookup"><span data-stu-id="1219a-188">New-AzureRmHDInsightCluster</span></span>](https://msdn.microsoft.com/library/mt619331.aspx) |
| <span data-ttu-id="1219a-189">New-AzureHDInsightClusterConfig</span><span class="sxs-lookup"><span data-stu-id="1219a-189">New-AzureHDInsightClusterConfig</span></span> |[<span data-ttu-id="1219a-190">Új AzureRmHDInsightClusterConfig</span><span class="sxs-lookup"><span data-stu-id="1219a-190">New-AzureRmHDInsightClusterConfig</span></span>](https://msdn.microsoft.com/library/mt603700.aspx) |
| <span data-ttu-id="1219a-191">New-AzureHDInsightHiveJobDefinition</span><span class="sxs-lookup"><span data-stu-id="1219a-191">New-AzureHDInsightHiveJobDefinition</span></span> |[<span data-ttu-id="1219a-192">Új AzureRmHDInsightHiveJobDefinition</span><span class="sxs-lookup"><span data-stu-id="1219a-192">New-AzureRmHDInsightHiveJobDefinition</span></span>](https://msdn.microsoft.com/library/mt619448.aspx) |
| <span data-ttu-id="1219a-193">New-AzureHDInsightMapReduceJobDefinition</span><span class="sxs-lookup"><span data-stu-id="1219a-193">New-AzureHDInsightMapReduceJobDefinition</span></span> |[<span data-ttu-id="1219a-194">Új AzureRmHDInsightMapReduceJobDefinition</span><span class="sxs-lookup"><span data-stu-id="1219a-194">New-AzureRmHDInsightMapReduceJobDefinition</span></span>](https://msdn.microsoft.com/library/mt603626.aspx) |
| <span data-ttu-id="1219a-195">New-AzureHDInsightPigJobDefinition</span><span class="sxs-lookup"><span data-stu-id="1219a-195">New-AzureHDInsightPigJobDefinition</span></span> |[<span data-ttu-id="1219a-196">Új AzureRmHDInsightPigJobDefinition</span><span class="sxs-lookup"><span data-stu-id="1219a-196">New-AzureRmHDInsightPigJobDefinition</span></span>](https://msdn.microsoft.com/library/mt603671.aspx) |
| <span data-ttu-id="1219a-197">New-AzureHDInsightSqoopJobDefinition</span><span class="sxs-lookup"><span data-stu-id="1219a-197">New-AzureHDInsightSqoopJobDefinition</span></span> |[<span data-ttu-id="1219a-198">Új AzureRmHDInsightSqoopJobDefinition</span><span class="sxs-lookup"><span data-stu-id="1219a-198">New-AzureRmHDInsightSqoopJobDefinition</span></span>](https://msdn.microsoft.com/library/mt608551.aspx) |
| <span data-ttu-id="1219a-199">New-AzureHDInsightStreamingMapReduceJobDefinition</span><span class="sxs-lookup"><span data-stu-id="1219a-199">New-AzureHDInsightStreamingMapReduceJobDefinition</span></span> |[<span data-ttu-id="1219a-200">Új AzureRmHDInsightStreamingMapReduceJobDefinition</span><span class="sxs-lookup"><span data-stu-id="1219a-200">New-AzureRmHDInsightStreamingMapReduceJobDefinition</span></span>](https://msdn.microsoft.com/library/mt603626.aspx) |
| <span data-ttu-id="1219a-201">Remove-AzureHDInsightCluster</span><span class="sxs-lookup"><span data-stu-id="1219a-201">Remove-AzureHDInsightCluster</span></span> |[<span data-ttu-id="1219a-202">Remove-AzureRmHDInsightCluster</span><span class="sxs-lookup"><span data-stu-id="1219a-202">Remove-AzureRmHDInsightCluster</span></span>](https://msdn.microsoft.com/library/mt619431.aspx) |
| <span data-ttu-id="1219a-203">Revoke-AzureHDInsightHttpServicesAccess</span><span class="sxs-lookup"><span data-stu-id="1219a-203">Revoke-AzureHDInsightHttpServicesAccess</span></span> |[<span data-ttu-id="1219a-204">A visszavonási-AzureRmHDInsightHttpServicesAccess</span><span class="sxs-lookup"><span data-stu-id="1219a-204">Revoke-AzureRmHDInsightHttpServicesAccess</span></span>](https://msdn.microsoft.com/library/mt619375.aspx) |
| <span data-ttu-id="1219a-205">Revoke-AzureHdinsightRdpAccess</span><span class="sxs-lookup"><span data-stu-id="1219a-205">Revoke-AzureHdinsightRdpAccess</span></span> |[<span data-ttu-id="1219a-206">A visszavonási-AzureRmHDInsightRdpServicesAccess</span><span class="sxs-lookup"><span data-stu-id="1219a-206">Revoke-AzureRmHDInsightRdpServicesAccess</span></span>](https://msdn.microsoft.com/library/mt603523.aspx) |
| <span data-ttu-id="1219a-207">Set-AzureHDInsightClusterSize</span><span class="sxs-lookup"><span data-stu-id="1219a-207">Set-AzureHDInsightClusterSize</span></span> |[<span data-ttu-id="1219a-208">Set-AzureRmHDInsightClusterSize</span><span class="sxs-lookup"><span data-stu-id="1219a-208">Set-AzureRmHDInsightClusterSize</span></span>](https://msdn.microsoft.com/library/mt603513.aspx) |
| <span data-ttu-id="1219a-209">Set-AzureHDInsightDefaultStorage</span><span class="sxs-lookup"><span data-stu-id="1219a-209">Set-AzureHDInsightDefaultStorage</span></span> |[<span data-ttu-id="1219a-210">Set-AzureRmHDInsightDefaultStorage</span><span class="sxs-lookup"><span data-stu-id="1219a-210">Set-AzureRmHDInsightDefaultStorage</span></span>](https://msdn.microsoft.com/library/mt603486.aspx) |
| <span data-ttu-id="1219a-211">Start-AzureHDInsightJob</span><span class="sxs-lookup"><span data-stu-id="1219a-211">Start-AzureHDInsightJob</span></span> |[<span data-ttu-id="1219a-212">Start-AzureRmHDInsightJob</span><span class="sxs-lookup"><span data-stu-id="1219a-212">Start-AzureRmHDInsightJob</span></span>](https://msdn.microsoft.com/library/mt603798.aspx) |
| <span data-ttu-id="1219a-213">Stop-AzureHDInsightJob</span><span class="sxs-lookup"><span data-stu-id="1219a-213">Stop-AzureHDInsightJob</span></span> |[<span data-ttu-id="1219a-214">STOP-AzureRmHDInsightJob</span><span class="sxs-lookup"><span data-stu-id="1219a-214">Stop-AzureRmHDInsightJob</span></span>](https://msdn.microsoft.com/library/mt619424.aspx) |
| <span data-ttu-id="1219a-215">Use-AzureHDInsightCluster</span><span class="sxs-lookup"><span data-stu-id="1219a-215">Use-AzureHDInsightCluster</span></span> |[<span data-ttu-id="1219a-216">Használjon-AzureRmHDInsightCluster</span><span class="sxs-lookup"><span data-stu-id="1219a-216">Use-AzureRmHDInsightCluster</span></span>](https://msdn.microsoft.com/library/mt619442.aspx) |
| <span data-ttu-id="1219a-217">Wait-AzureHDInsightJob</span><span class="sxs-lookup"><span data-stu-id="1219a-217">Wait-AzureHDInsightJob</span></span> |[<span data-ttu-id="1219a-218">Várjon, amíg-AzureRmHDInsightJob</span><span class="sxs-lookup"><span data-stu-id="1219a-218">Wait-AzureRmHDInsightJob</span></span>](https://msdn.microsoft.com/library/mt603834.aspx) |

### <a name="new-cmdlets"></a><span data-ttu-id="1219a-219">Új parancsmagok</span><span class="sxs-lookup"><span data-stu-id="1219a-219">New cmdlets</span></span>
<span data-ttu-id="1219a-220">Az alábbiakban az új parancsmagok, amelyek csak a ARM módban érhető el.</span><span class="sxs-lookup"><span data-stu-id="1219a-220">The following are the new cmdlets that are only available in the ARM mode.</span></span> 

<span data-ttu-id="1219a-221">**A kapcsolódó parancsmagok parancsfájlművelet:**</span><span class="sxs-lookup"><span data-stu-id="1219a-221">**Script action related cmdlets:**</span></span>

* <span data-ttu-id="1219a-222">**Get-AzureRmHDInsightPersistedScriptAction**: lekérdezi a megőrzött Parancsfájlműveletek fürt és időrendben sorolja fel azokat, vagy egy megadott megőrzött parancsfájl művelet lekérdezi a részletek.</span><span class="sxs-lookup"><span data-stu-id="1219a-222">**Get-AzureRmHDInsightPersistedScriptAction**: Gets the persisted script actions for a cluster and lists them in chronological order, or gets details for a specified persisted script action.</span></span> 
* <span data-ttu-id="1219a-223">**Get-AzureRmHDInsightScriptActionHistory**: a parancsfájlművelet előzményeinek lekérdezi a fürt, fordított időrendben sorolja fel, és lekérdezi a korábban végrehajtott parancsfájlművelet részleteit.</span><span class="sxs-lookup"><span data-stu-id="1219a-223">**Get-AzureRmHDInsightScriptActionHistory**: Gets the script action history for a cluster and lists it in reverse chronological order, or gets details of a previously executed script action.</span></span> 
* <span data-ttu-id="1219a-224">**Remove-AzureRmHDInsightPersistedScriptAction**: egy megőrzött parancsfájl művelet eltávolítja a HDInsight-fürtöt.</span><span class="sxs-lookup"><span data-stu-id="1219a-224">**Remove-AzureRmHDInsightPersistedScriptAction**: Removes a persisted script action from an HDInsight cluster.</span></span>
* <span data-ttu-id="1219a-225">**Set-AzureRmHDInsightPersistedScriptAction**: egy megőrzött parancsfájl műveletet kell azelőtti parancsfájlművelet állítja be.</span><span class="sxs-lookup"><span data-stu-id="1219a-225">**Set-AzureRmHDInsightPersistedScriptAction**: Sets a previously executed script action to be a persisted script action.</span></span>
* <span data-ttu-id="1219a-226">**Küldje el AzureRmHDInsightScriptAction**: egy új parancsfájlművelet Azure HDInsight-fürtöt ad meg.</span><span class="sxs-lookup"><span data-stu-id="1219a-226">**Submit-AzureRmHDInsightScriptAction**: Submits a new script action to an Azure HDInsight cluster.</span></span> 

<span data-ttu-id="1219a-227">További használati információkért lásd: [testreszabása Linux-alapú HDInsight-fürtök használata parancsfájlművelet](hdinsight-hadoop-customize-cluster-linux.md).</span><span class="sxs-lookup"><span data-stu-id="1219a-227">For additional usage information, see [Customize Linux-based HDInsight clusters using Script Action](hdinsight-hadoop-customize-cluster-linux.md).</span></span>

<span data-ttu-id="1219a-228">**A kapcsolódó parancsmagok Clsuter identitás:**</span><span class="sxs-lookup"><span data-stu-id="1219a-228">**Clsuter identity related cmdlets:**</span></span>

* <span data-ttu-id="1219a-229">**Adja hozzá AzureRmHDInsightClusterIdentity**: ad hozzá egy fürt identitását egy fürt konfigurációs objektumot, hogy a HDInsight-fürt elérhessék az Azure Data Lake tárolja.</span><span class="sxs-lookup"><span data-stu-id="1219a-229">**Add-AzureRmHDInsightClusterIdentity**: Adds a cluster identity to a cluster configuration object so that the HDInsight cluster can access Azure Data Lake Stores.</span></span> <span data-ttu-id="1219a-230">Lásd: [HDInsight-fürtök létrehozása az Azure PowerShell használatával a Data Lake Store](../data-lake-store/data-lake-store-hdinsight-hadoop-use-powershell.md).</span><span class="sxs-lookup"><span data-stu-id="1219a-230">See [Create an HDInsight cluster with Data Lake Store using Azure PowerShell](../data-lake-store/data-lake-store-hdinsight-hadoop-use-powershell.md).</span></span>

### <a name="examples"></a><span data-ttu-id="1219a-231">Példák</span><span class="sxs-lookup"><span data-stu-id="1219a-231">Examples</span></span>
<span data-ttu-id="1219a-232">**Fürt létrehozása**</span><span class="sxs-lookup"><span data-stu-id="1219a-232">**Create cluster**</span></span>

<span data-ttu-id="1219a-233">Régi parancs (ASM):</span><span class="sxs-lookup"><span data-stu-id="1219a-233">Old command (ASM):</span></span> 

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

<span data-ttu-id="1219a-234">Új parancs (ARM):</span><span class="sxs-lookup"><span data-stu-id="1219a-234">New command (ARM):</span></span>

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


<span data-ttu-id="1219a-235">**Fürt törlése**</span><span class="sxs-lookup"><span data-stu-id="1219a-235">**Delete cluster**</span></span>

<span data-ttu-id="1219a-236">Régi parancs (ASM):</span><span class="sxs-lookup"><span data-stu-id="1219a-236">Old command (ASM):</span></span>

    Remove-AzureHDInsightCluster -name $clusterName 

<span data-ttu-id="1219a-237">Új parancs (ARM):</span><span class="sxs-lookup"><span data-stu-id="1219a-237">New command (ARM):</span></span>

    Remove-AzureRmHDInsightCluster -ResourceGroupName $resourceGroupName -ClusterName $clusterName 

<span data-ttu-id="1219a-238">**Lista fürt**</span><span class="sxs-lookup"><span data-stu-id="1219a-238">**List cluster**</span></span>

<span data-ttu-id="1219a-239">Régi parancs (ASM):</span><span class="sxs-lookup"><span data-stu-id="1219a-239">Old command (ASM):</span></span>

    Get-AzureHDInsightCluster

<span data-ttu-id="1219a-240">Új parancs (ARM):</span><span class="sxs-lookup"><span data-stu-id="1219a-240">New command (ARM):</span></span>

    Get-AzureRmHDInsightCluster 

<span data-ttu-id="1219a-241">**Fürt megjelenítése**</span><span class="sxs-lookup"><span data-stu-id="1219a-241">**Show cluster**</span></span>

<span data-ttu-id="1219a-242">Régi parancs (ASM):</span><span class="sxs-lookup"><span data-stu-id="1219a-242">Old command (ASM):</span></span>

    Get-AzureHDInsightCluster -Name $clusterName

<span data-ttu-id="1219a-243">Új parancs (ARM):</span><span class="sxs-lookup"><span data-stu-id="1219a-243">New command (ARM):</span></span>

    Get-AzureRmHDInsightCluster -ResourceGroupName $resourceGroupName -clusterName $clusterName


#### <a name="other-samples"></a><span data-ttu-id="1219a-244">Más minták</span><span class="sxs-lookup"><span data-stu-id="1219a-244">Other samples</span></span>
* [<span data-ttu-id="1219a-245">A HDInsight-fürtök létrehozása</span><span class="sxs-lookup"><span data-stu-id="1219a-245">Create HDInsight clusters</span></span>](hdinsight-hadoop-create-linux-clusters-azure-powershell.md)
* [<span data-ttu-id="1219a-246">Küldje el a Hive-feladatok</span><span class="sxs-lookup"><span data-stu-id="1219a-246">Submit Hive jobs</span></span>](hdinsight-hadoop-use-hive-powershell.md)
* [<span data-ttu-id="1219a-247">Küldje el a Pig-feladatokhoz</span><span class="sxs-lookup"><span data-stu-id="1219a-247">Submit Pig jobs</span></span>](hdinsight-hadoop-use-pig-powershell.md)
* [<span data-ttu-id="1219a-248">Sqoop feladatok elküldéséhez</span><span class="sxs-lookup"><span data-stu-id="1219a-248">Submit Sqoop jobs</span></span>](hdinsight-hadoop-use-sqoop-powershell.md)

## <a name="migrating-to-the-arm-based-hdinsight-net-sdk"></a><span data-ttu-id="1219a-249">Az ARM-alapú HDInsight .NET SDK áttelepítése</span><span class="sxs-lookup"><span data-stu-id="1219a-249">Migrating to the ARM-based HDInsight .NET SDK</span></span>
<span data-ttu-id="1219a-250">Az Azure Szolgáltatáskezelés-alapú [(ASM) HDInsight .NET SDK](https://msdn.microsoft.com/library/azure/mt416619.aspx) elavult.</span><span class="sxs-lookup"><span data-stu-id="1219a-250">The Azure Service Management-based [(ASM) HDInsight .NET SDK](https://msdn.microsoft.com/library/azure/mt416619.aspx) is now deprecated.</span></span> <span data-ttu-id="1219a-251">Hosszúan használja az Azure Resource Manager-alapú [(ARM) a HDInsight .NET SDK](https://msdn.microsoft.com/library/azure/mt271028.aspx).</span><span class="sxs-lookup"><span data-stu-id="1219a-251">You are encouraged to use the Azure Resource Management-based [(ARM) HDInsight .NET SDK](https://msdn.microsoft.com/library/azure/mt271028.aspx).</span></span> <span data-ttu-id="1219a-252">A következő ASM-alapú HDInsight-csomagok elavulttá válnak.</span><span class="sxs-lookup"><span data-stu-id="1219a-252">The following ASM-based HDInsight packages are being deprecated.</span></span>

* `Microsoft.WindowsAzure.Management.HDInsight`
* `Microsoft.Hadoop.Client`

<span data-ttu-id="1219a-253">Ez a témakör mutatókat biztosít a további információt az ARM-alapú SDK segítségével bizonyos feladatok elvégzéséhez.</span><span class="sxs-lookup"><span data-stu-id="1219a-253">This section provides pointers to more information on how to perform certain tasks using the ARM-based SDK.</span></span>

| <span data-ttu-id="1219a-254">Hogyan... az ARM-alapú HDInsight SDK használatával</span><span class="sxs-lookup"><span data-stu-id="1219a-254">How to... using the ARM-based HDInsight SDK</span></span> | <span data-ttu-id="1219a-255">Hivatkozások</span><span class="sxs-lookup"><span data-stu-id="1219a-255">Links</span></span> |
| --- | --- |
| <span data-ttu-id="1219a-256">.NET SDK használatával a HDInsight-fürtök létrehozása</span><span class="sxs-lookup"><span data-stu-id="1219a-256">Create HDInsight clusters using .NET SDK</span></span> |<span data-ttu-id="1219a-257">Lásd: [HDInsight-fürtök létrehozása .NET SDK használatával](hdinsight-hadoop-create-linux-clusters-dotnet-sdk.md)</span><span class="sxs-lookup"><span data-stu-id="1219a-257">See [Create HDInsight clusters using .NET SDK](hdinsight-hadoop-create-linux-clusters-dotnet-sdk.md)</span></span> |
| <span data-ttu-id="1219a-258">A fürt parancsfájlművelet .NET SDK-val testreszabása</span><span class="sxs-lookup"><span data-stu-id="1219a-258">Customize a cluster using Script Action with .NET SDK</span></span> |<span data-ttu-id="1219a-259">Lásd: [testreszabása HDInsight Linux clusters parancsfájlművelet használatával](hdinsight-hadoop-create-linux-clusters-dotnet-sdk.md#use-script-action)</span><span class="sxs-lookup"><span data-stu-id="1219a-259">See [Customize HDInsight Linux clusters using Script Action](hdinsight-hadoop-create-linux-clusters-dotnet-sdk.md#use-script-action)</span></span> |
| <span data-ttu-id="1219a-260">Alkalmazások interaktív .NET SDK-val az Azure Active Directory használatával hitelesíti</span><span class="sxs-lookup"><span data-stu-id="1219a-260">Authenticate applications interactively using Azure Active Directory with .NET SDK</span></span> |<span data-ttu-id="1219a-261">Lásd: [.NET SDK használatával futtassa Hive lekérdezések](hdinsight-hadoop-use-hive-dotnet-sdk.md).</span><span class="sxs-lookup"><span data-stu-id="1219a-261">See [Run Hive queries using .NET SDK](hdinsight-hadoop-use-hive-dotnet-sdk.md).</span></span> <span data-ttu-id="1219a-262">Ebben a cikkben a kódrészletet használja az interaktív hitelesítési módszerrel.</span><span class="sxs-lookup"><span data-stu-id="1219a-262">The code snippet in this article uses the interactive authentication approach.</span></span> |
| <span data-ttu-id="1219a-263">Alkalmazások nem interaktív .NET SDK-val az Azure Active Directory használatával hitelesíti</span><span class="sxs-lookup"><span data-stu-id="1219a-263">Authenticate applications non-interactively using Azure Active Directory with .NET SDK</span></span> |<span data-ttu-id="1219a-264">Lásd: [a HDInsight nem interaktív alkalmazások létrehozása](hdinsight-create-non-interactive-authentication-dotnet-applications.md)</span><span class="sxs-lookup"><span data-stu-id="1219a-264">See [Create non-interactive applications for HDInsight](hdinsight-create-non-interactive-authentication-dotnet-applications.md)</span></span> |
| <span data-ttu-id="1219a-265">.NET SDK használatával Hive feladat elküldése</span><span class="sxs-lookup"><span data-stu-id="1219a-265">Submit a Hive job using .NET SDK</span></span> |<span data-ttu-id="1219a-266">Lásd: [elküldeni a Hive-feladatok](hdinsight-hadoop-use-hive-dotnet-sdk.md)</span><span class="sxs-lookup"><span data-stu-id="1219a-266">See [Submit Hive jobs](hdinsight-hadoop-use-hive-dotnet-sdk.md)</span></span> |
| <span data-ttu-id="1219a-267">Elküldeni a Pig feladatot .NET SDK használatával</span><span class="sxs-lookup"><span data-stu-id="1219a-267">Submit a Pig job using .NET SDK</span></span> |<span data-ttu-id="1219a-268">Lásd: [elküldeni a Pig-feladatokhoz](hdinsight-hadoop-use-pig-dotnet-sdk.md)</span><span class="sxs-lookup"><span data-stu-id="1219a-268">See [Submit Pig jobs](hdinsight-hadoop-use-pig-dotnet-sdk.md)</span></span> |
| <span data-ttu-id="1219a-269">.NET SDK használatával Sqoop feladat elküldése</span><span class="sxs-lookup"><span data-stu-id="1219a-269">Submit a Sqoop job using .NET SDK</span></span> |<span data-ttu-id="1219a-270">Lásd: [nyújt Sqoop feladatok](hdinsight-hadoop-use-sqoop-dotnet-sdk.md)</span><span class="sxs-lookup"><span data-stu-id="1219a-270">See [Submit Sqoop jobs](hdinsight-hadoop-use-sqoop-dotnet-sdk.md)</span></span> |
| <span data-ttu-id="1219a-271">Lista HDInsight-fürtök .NET SDK használatával</span><span class="sxs-lookup"><span data-stu-id="1219a-271">List HDInsight clusters using .NET SDK</span></span> |<span data-ttu-id="1219a-272">Lásd: [lista HDInsight-fürtök](hdinsight-administer-use-dotnet-sdk.md#list-clusters)</span><span class="sxs-lookup"><span data-stu-id="1219a-272">See [List HDInsight clusters](hdinsight-administer-use-dotnet-sdk.md#list-clusters)</span></span> |
| <span data-ttu-id="1219a-273">A HDInsight-fürtök .NET SDK használatával méretezése</span><span class="sxs-lookup"><span data-stu-id="1219a-273">Scale HDInsight clusters using .NET SDK</span></span> |<span data-ttu-id="1219a-274">Lásd: [méretezési HDInsight-fürtök](hdinsight-administer-use-dotnet-sdk.md#scale-clusters)</span><span class="sxs-lookup"><span data-stu-id="1219a-274">See [Scale HDInsight clusters](hdinsight-administer-use-dotnet-sdk.md#scale-clusters)</span></span> |
| <span data-ttu-id="1219a-275">A HDInsight-fürtök .NET SDK használatával engedélyezéshez/visszavonáshoz elérésére</span><span class="sxs-lookup"><span data-stu-id="1219a-275">Grant/revoke access to HDInsight clusters using .NET SDK</span></span> |<span data-ttu-id="1219a-276">Lásd: [Grant/revoke access HDInsight-fürtök](hdinsight-administer-use-dotnet-sdk.md#grantrevoke-access)</span><span class="sxs-lookup"><span data-stu-id="1219a-276">See [Grant/revoke access to HDInsight clusters](hdinsight-administer-use-dotnet-sdk.md#grantrevoke-access)</span></span> |
| <span data-ttu-id="1219a-277">HTTP felhasználó hitelesítő adatait a HDInsight-fürtök .NET SDK használatával</span><span class="sxs-lookup"><span data-stu-id="1219a-277">Update HTTP user credentials for HDInsight clusters using .NET SDK</span></span> |<span data-ttu-id="1219a-278">Lásd: [frissítés HTTP felhasználói hitelesítő adatokat a HDInsight-fürtök](hdinsight-administer-use-dotnet-sdk.md#update-http-user-credentials)</span><span class="sxs-lookup"><span data-stu-id="1219a-278">See [Update HTTP user credentials for HDInsight clusters](hdinsight-administer-use-dotnet-sdk.md#update-http-user-credentials)</span></span> |
| <span data-ttu-id="1219a-279">Az alapértelmezett tárfiók keresése a HDInsight-fürtök .NET SDK használatával</span><span class="sxs-lookup"><span data-stu-id="1219a-279">Find the default storage account for HDInsight clusters using .NET SDK</span></span> |<span data-ttu-id="1219a-280">Lásd: [található az alapértelmezett tárfiókot, a HDInsight-fürtök](hdinsight-administer-use-dotnet-sdk.md#find-the-default-storage-account)</span><span class="sxs-lookup"><span data-stu-id="1219a-280">See [Find the default storage account for HDInsight clusters](hdinsight-administer-use-dotnet-sdk.md#find-the-default-storage-account)</span></span> |
| <span data-ttu-id="1219a-281">.NET SDK használatával a HDInsight-fürtök törlése</span><span class="sxs-lookup"><span data-stu-id="1219a-281">Delete HDInsight clusters using .NET SDK</span></span> |<span data-ttu-id="1219a-282">Lásd: [törlése HDInsight-fürtök](hdinsight-administer-use-dotnet-sdk.md#delete-clusters)</span><span class="sxs-lookup"><span data-stu-id="1219a-282">See [Delete HDInsight clusters](hdinsight-administer-use-dotnet-sdk.md#delete-clusters)</span></span> |

### <a name="examples"></a><span data-ttu-id="1219a-283">Példák</span><span class="sxs-lookup"><span data-stu-id="1219a-283">Examples</span></span>
<span data-ttu-id="1219a-284">A következő példák a hogyan van egy művelet alapján történik a címterület-kezelési alapú SDK és az azzal egyenértékű kódrészletet az ARM-alapú SDK-ban.</span><span class="sxs-lookup"><span data-stu-id="1219a-284">Following are some examples on how an operation is performed using the ASM-based SDK and the equivalent code snippet for the ARM-based SDK.</span></span>

<span data-ttu-id="1219a-285">**A fürt CRUD-ügyfél létrehozása**</span><span class="sxs-lookup"><span data-stu-id="1219a-285">**Creating a cluster CRUD client**</span></span>

* <span data-ttu-id="1219a-286">Régi parancs (ASM)</span><span class="sxs-lookup"><span data-stu-id="1219a-286">Old command (ASM)</span></span>
  
        //Certificate auth
        //This logs the application in using a subscription administration certificate, which is not offered in Azure Resource Manager (ARM)
  
        const string subid = "454467d4-60ca-4dfd-a556-216eeeeeeee1";
        var cred = new HDInsightCertificateCredential(new Guid(subid), new X509Certificate2(@"path\to\certificate.cer"));
        var client = HDInsightClient.Connect(cred);
* <span data-ttu-id="1219a-287">Új parancs (ARM) (egyszerű szolgáltatás engedélyezése)</span><span class="sxs-lookup"><span data-stu-id="1219a-287">New command (ARM) (Service principal authorization)</span></span>
  
        //Service principal auth
        //This will log the application in as itself, rather than on behalf of a specific user.
        //For details, including how to set up the application, see:
        //   https://azure.microsoft.com/en-us/documentation/articles/hdinsight-create-non-interactive-authentication-dotnet-applications/
  
        var authFactory = new AuthenticationFactory();
  
        var account = new AzureAccount { Type = AzureAccount.AccountType.ServicePrincipal, Id = clientId };
  
        var env = AzureEnvironment.PublicEnvironments[EnvironmentName.AzureCloud];
  
        var accessToken = authFactory.Authenticate(account, env, tenantId, secretKey, ShowDialog.Never).AccessToken;
  
        var creds = new TokenCloudCredentials(subId.ToString(), accessToken);
  
        _hdiManagementClient = new HDInsightManagementClient(creds);
* <span data-ttu-id="1219a-288">Új parancs (ARM) (felhasználói engedélyezési)</span><span class="sxs-lookup"><span data-stu-id="1219a-288">New command (ARM) (User authorization)</span></span>
  
        //User auth
        //This will log the application in on behalf of the user.
        //The end-user will see a login popup.
  
        var authFactory = new AuthenticationFactory();
  
        var account = new AzureAccount { Type = AzureAccount.AccountType.User, Id = username };
  
        var env = AzureEnvironment.PublicEnvironments[EnvironmentName.AzureCloud];
  
        var accessToken = authFactory.Authenticate(account, env, AuthenticationFactory.CommonAdTenant, password, ShowDialog.Auto).AccessToken;
  
        var creds = new TokenCloudCredentials(subId.ToString(), accessToken);
  
        _hdiManagementClient = new HDInsightManagementClient(creds);

<span data-ttu-id="1219a-289">**Fürt létrehozása**</span><span class="sxs-lookup"><span data-stu-id="1219a-289">**Creating a cluster**</span></span>

* <span data-ttu-id="1219a-290">Régi parancs (ASM)</span><span class="sxs-lookup"><span data-stu-id="1219a-290">Old command (ASM)</span></span>
  
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
* <span data-ttu-id="1219a-291">Új parancs (ARM)</span><span class="sxs-lookup"><span data-stu-id="1219a-291">New command (ARM)</span></span>
  
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

<span data-ttu-id="1219a-292">**HTTP-hozzáférés engedélyezése**</span><span class="sxs-lookup"><span data-stu-id="1219a-292">**Enabling HTTP access**</span></span>

* <span data-ttu-id="1219a-293">Régi parancs (ASM)</span><span class="sxs-lookup"><span data-stu-id="1219a-293">Old command (ASM)</span></span>
  
        client.EnableHttp(dnsName, "West US", "admin", "*******");
* <span data-ttu-id="1219a-294">Új parancs (ARM)</span><span class="sxs-lookup"><span data-stu-id="1219a-294">New command (ARM)</span></span>
  
        var httpParams = new HttpSettingsParameters
        {
               HttpUserEnabled = true,
               HttpUsername = "admin",
               HttpPassword = "*******",
        };
        client.Clusters.ConfigureHttpSettings(resourceGroup, dnsname, httpParams);

<span data-ttu-id="1219a-295">**A fürtök törlésével**</span><span class="sxs-lookup"><span data-stu-id="1219a-295">**Deleting a cluster**</span></span>

* <span data-ttu-id="1219a-296">Régi parancs (ASM)</span><span class="sxs-lookup"><span data-stu-id="1219a-296">Old command (ASM)</span></span>
  
        client.DeleteCluster(dnsName);
* <span data-ttu-id="1219a-297">Új parancs (ARM)</span><span class="sxs-lookup"><span data-stu-id="1219a-297">New command (ARM)</span></span>
  
        client.Clusters.Delete(resourceGroup, dnsname);

