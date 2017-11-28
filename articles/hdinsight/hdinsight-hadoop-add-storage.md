---
title: "aaaAdd további az Azure storage-fiókok tooHDInsight |} Microsoft Docs"
description: "Ismerje meg, hogyan tooadd további az Azure storage accounts tooan meglévő HDInsight-fürtre."
services: hdinsight
documentationCenter: 
author: Blackmist
manager: jhubbard
editor: cgronlun
tags: azure-portal
ms.service: hdinsight
ms.devlang: 
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 08/04/2017
ms.author: larryfr
ms.custom: H1Hack27Feb2017,hdinsightactive
ms.openlocfilehash: ce5acfa4b61bf7e83b1fb374d64a1eaa3182fbec
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="add-additional-storage-accounts-toohdinsight"></a><span data-ttu-id="492d4-103">További tárhely fiókok tooHDInsight hozzáadása</span><span class="sxs-lookup"><span data-stu-id="492d4-103">Add additional storage accounts tooHDInsight</span></span>

<span data-ttu-id="492d4-104">Ismerje meg, hogyan toouse parancsfájl műveletek tooadd további az Azure storage accounts tooHDInsight.</span><span class="sxs-lookup"><span data-stu-id="492d4-104">Learn how toouse script actions tooadd additional Azure storage accounts tooHDInsight.</span></span> <span data-ttu-id="492d4-105">hello jelen dokumentumban leírt lépések hozzáadása a tárolási fiók tooan meglévő Linux-alapú HDInsight-fürtre.</span><span class="sxs-lookup"><span data-stu-id="492d4-105">hello steps in this document add a storage account tooan existing Linux-based HDInsight cluster.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="492d4-106">a dokumentumban szereplő információk hello tárgya további tárhely tooa fürt hozzáadása után lett létrehozva.</span><span class="sxs-lookup"><span data-stu-id="492d4-106">hello information in this document is about adding additional storage tooa cluster after it has been created.</span></span> <span data-ttu-id="492d4-107">A storage-fiókok hozzáadása a fürt létrehozása során további információkért lásd: [állítsa be a HDInsight Hadoop, Spark, Kafka és több fürt](hdinsight-hadoop-provision-linux-clusters.md).</span><span class="sxs-lookup"><span data-stu-id="492d4-107">For information on adding storage accounts during cluster creation, see [Set up clusters in HDInsight with Hadoop, Spark, Kafka, and more](hdinsight-hadoop-provision-linux-clusters.md).</span></span>

## <a name="how-it-works"></a><span data-ttu-id="492d4-108">Működés</span><span class="sxs-lookup"><span data-stu-id="492d4-108">How it works</span></span>

<span data-ttu-id="492d4-109">Ezt a parancsfájlt hello a következő paramétereket fogadja:</span><span class="sxs-lookup"><span data-stu-id="492d4-109">This script takes hello following parameters:</span></span>

* <span data-ttu-id="492d4-110">__Az Azure storage-fiók neve__: hello hello tárolási fiók tooadd toohello HDInsight-fürt nevét.</span><span class="sxs-lookup"><span data-stu-id="492d4-110">__Azure storage account name__: hello name of hello storage account tooadd toohello HDInsight cluster.</span></span> <span data-ttu-id="492d4-111">Hello parancsprogram futtatása után HDInsight olvashat és írhat adatokat tárolja ezt a tárfiókot.</span><span class="sxs-lookup"><span data-stu-id="492d4-111">After running hello script, HDInsight can read and write data stored in this storage account.</span></span>

* <span data-ttu-id="492d4-112">__Azure storage-fiók kulcs__: egy kulcs, amely engedélyezi a hozzáférést toohello tárfiók.</span><span class="sxs-lookup"><span data-stu-id="492d4-112">__Azure storage account key__: A key that grants access toohello storage account.</span></span>

* <span data-ttu-id="492d4-113">__-p__ (nem kötelező): Ha meg van adva, hello kulcs nem titkosított, és egyszerű szövegként hello core-site.xml fájl tárolja.</span><span class="sxs-lookup"><span data-stu-id="492d4-113">__-p__ (optional): If specified, hello key is not encrypted and is stored in hello core-site.xml file as plain text.</span></span>

<span data-ttu-id="492d4-114">A feldolgozás során hello parancsfájl hello a következő műveleteket hajtja végre:</span><span class="sxs-lookup"><span data-stu-id="492d4-114">During processing, hello script performs hello following actions:</span></span>

* <span data-ttu-id="492d4-115">Ha hello tárfiók már létezik a hello fürt hello core-site.xml konfigurációjában, hello parancsfájl kilép, és nincs több teendő történik.</span><span class="sxs-lookup"><span data-stu-id="492d4-115">If hello storage account already exists in hello core-site.xml configuration for hello cluster, hello script exits and no further actions are performed.</span></span>

* <span data-ttu-id="492d4-116">Ellenőrzi, hogy hello storage-fiók létezik-e, és hello kulcs segítségével férhetők el.</span><span class="sxs-lookup"><span data-stu-id="492d4-116">Verifies that hello storage account exists and can be accessed using hello key.</span></span>

* <span data-ttu-id="492d4-117">Hello kulcs hello fürt hitelesítő adat segítségével titkosítja.</span><span class="sxs-lookup"><span data-stu-id="492d4-117">Encrypts hello key using hello cluster credential.</span></span>

* <span data-ttu-id="492d4-118">Hello tárolási fiók toohello core-site.xml fájl hozzáadása.</span><span class="sxs-lookup"><span data-stu-id="492d4-118">Adds hello storage account toohello core-site.xml file.</span></span>

* <span data-ttu-id="492d4-119">Leállítja és újraindítja hello Oozie, YARN, MapReduce2 és HDFS szolgáltatásokat.</span><span class="sxs-lookup"><span data-stu-id="492d4-119">Stops and restarts hello Oozie, YARN, MapReduce2, and HDFS services.</span></span> <span data-ttu-id="492d4-120">Ezek a szolgáltatások indítása és leállítása lehetővé teszi, hogy azok toouse hello új tárfiókot.</span><span class="sxs-lookup"><span data-stu-id="492d4-120">Stopping and starting these services allows them toouse hello new storage account.</span></span>

> [!WARNING]
> <span data-ttu-id="492d4-121">A storage-fiók egy másik helyen található mint hello HDInsight-fürt használata nem támogatott.</span><span class="sxs-lookup"><span data-stu-id="492d4-121">Using a storage account in a different location than hello HDInsight cluster is not supported.</span></span>

## <a name="hello-script"></a><span data-ttu-id="492d4-122">hello parancsfájl</span><span class="sxs-lookup"><span data-stu-id="492d4-122">hello script</span></span>

<span data-ttu-id="492d4-123">__Parancsfájl-hely__: [https://hdiconfigactions.blob.core.windows.net/linuxaddstorageaccountv01/add-storage-account-v01.sh](https://hdiconfigactions.blob.core.windows.net/linuxaddstorageaccountv01/add-storage-account-v01.sh)</span><span class="sxs-lookup"><span data-stu-id="492d4-123">__Script location__: [https://hdiconfigactions.blob.core.windows.net/linuxaddstorageaccountv01/add-storage-account-v01.sh](https://hdiconfigactions.blob.core.windows.net/linuxaddstorageaccountv01/add-storage-account-v01.sh)</span></span>

<span data-ttu-id="492d4-124">__Követelmények__:</span><span class="sxs-lookup"><span data-stu-id="492d4-124">__Requirements__:</span></span>

* <span data-ttu-id="492d4-125">hello parancsfájl kell alkalmazni a hello __Átjárócsomópontokat__.</span><span class="sxs-lookup"><span data-stu-id="492d4-125">hello script must be applied on hello __Head nodes__.</span></span>

## <a name="toouse-hello-script"></a><span data-ttu-id="492d4-126">toouse hello parancsfájl</span><span class="sxs-lookup"><span data-stu-id="492d4-126">toouse hello script</span></span>

<span data-ttu-id="492d4-127">Ezt a parancsfájlt a hello Azure-portálon az Azure PowerShell is használható, vagy Azure CLI 1.0 hello.</span><span class="sxs-lookup"><span data-stu-id="492d4-127">This script can be used from hello Azure portal, Azure PowerShell, or hello Azure CLI 1.0.</span></span> <span data-ttu-id="492d4-128">További információkért lásd: hello [testreszabása Linux-alapú HDInsight-fürtök használata parancsfájlművelet](hdinsight-hadoop-customize-cluster-linux.md#apply-a-script-action-to-a-running-cluster) dokumentum.</span><span class="sxs-lookup"><span data-stu-id="492d4-128">For more information, see hello [Customize Linux-based HDInsight clusters using script action](hdinsight-hadoop-customize-cluster-linux.md#apply-a-script-action-to-a-running-cluster) document.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="492d4-129">Ha hello itt hello testreszabási dokumentumban ismertetett lépéseket, használja ezt a parancsfájlt a következő információk tooapply hello:</span><span class="sxs-lookup"><span data-stu-id="492d4-129">When using hello steps provided in hello customization document, use hello following information tooapply this script:</span></span>
>
> * <span data-ttu-id="492d4-130">Cserélje le a példa parancsfájlművelet URI hello URI ehhez a parancsprogramhoz (https://hdiconfigactions.blob.core.windows.net/linuxaddstorageaccountv01/add-storage-account-v01.sh).</span><span class="sxs-lookup"><span data-stu-id="492d4-130">Replace any example script action URI with hello URI for this script (https://hdiconfigactions.blob.core.windows.net/linuxaddstorageaccountv01/add-storage-account-v01.sh).</span></span>
> * <span data-ttu-id="492d4-131">Cserélje le a példa paramétereket hello az Azure storage fióknevet és kulcsot hello tárolási fiók toobe hozzáadott toohello fürt.</span><span class="sxs-lookup"><span data-stu-id="492d4-131">Replace any example parameters with hello Azure storage account name and key of hello storage account toobe added toohello cluster.</span></span> <span data-ttu-id="492d4-132">Ha az Azure portál használatával hello, ezeket a paramétereket szóközzel kell elválasztani.</span><span class="sxs-lookup"><span data-stu-id="492d4-132">If using hello Azure portal, these parameters must be separated by a space.</span></span>
> * <span data-ttu-id="492d4-133">Nem kell toomark ezt a parancsfájlt, __megőrzött__, azt közvetlenül hello Ambari konfigurációs hello fürt frissítéséhez.</span><span class="sxs-lookup"><span data-stu-id="492d4-133">You do not need toomark this script as __Persisted__, as it directly updates hello Ambari configuration for hello cluster.</span></span>

## <a name="known-issues"></a><span data-ttu-id="492d4-134">Ismert problémák</span><span class="sxs-lookup"><span data-stu-id="492d4-134">Known issues</span></span>

### <a name="storage-accounts-not-displayed-in-azure-portal-or-tools"></a><span data-ttu-id="492d4-135">Nem jelenik meg az Azure portálon vagy az eszközök a Storage-fiókok</span><span class="sxs-lookup"><span data-stu-id="492d4-135">Storage accounts not displayed in Azure portal or tools</span></span>

<span data-ttu-id="492d4-136">Amikor megtekinti hello HDInsight fürthöz hello Azure-portálon, válassza a hello __Storage-fiókok__ bejegyzésre az __tulajdonságok__ nem jelennek meg a parancsfájlművelet hozzáadva storage-fiókok.</span><span class="sxs-lookup"><span data-stu-id="492d4-136">When viewing hello HDInsight cluster in hello Azure portal, selecting hello __Storage Accounts__ entry under __Properties__ does not display storage accounts added through this script action.</span></span> <span data-ttu-id="492d4-137">Az Azure PowerShell és az Azure parancssori felület ne jelenjen meg hello további tárfiók vagy.</span><span class="sxs-lookup"><span data-stu-id="492d4-137">Azure PowerShell and Azure CLI do not display hello additional storage account either.</span></span>

<span data-ttu-id="492d4-138">hello tárolással kapcsolatos nem jelenik meg, mert hello parancsfájl csak módosítja hello fürt hello core-site.xml konfigurációjában.</span><span class="sxs-lookup"><span data-stu-id="492d4-138">hello storage information isn't displayed because hello script only modifies hello core-site.xml configuration for hello cluster.</span></span> <span data-ttu-id="492d4-139">Ezeket az információkat nem használja az Azure felügyeleti API-k használatával hello fürt információk beolvasása.</span><span class="sxs-lookup"><span data-stu-id="492d4-139">This information is not used when retrieving hello cluster information using Azure management APIs.</span></span>

<span data-ttu-id="492d4-140">tooview tárfiókadatok hozzáadott toohello fürt ezt a parancsfájlt használja hello Ambari REST API használatával.</span><span class="sxs-lookup"><span data-stu-id="492d4-140">tooview storage account information added toohello cluster using this script, use hello Ambari REST API.</span></span> <span data-ttu-id="492d4-141">A következő parancsok tooretrieve hello ezt az információt használja a fürt:</span><span class="sxs-lookup"><span data-stu-id="492d4-141">Use hello following commands tooretrieve this information for your cluster:</span></span>

```PowerShell
$creds = Get-Credential -UserName "admin" -Message "Enter hello cluster login credentials"
$resp = Invoke-WebRequest -Uri "https://$clusterName.azurehdinsight.net/api/v1/clusters/$clusterName/configurations/service_config_versions?service_name=HDFS&service_config_version=1" `
    -Credential $creds
$respObj = ConvertFrom-Json $resp.Content
$respObj.items.configurations.properties."fs.azure.account.key.$storageAccountName.blob.core.windows.net"
```

> [!NOTE]
> <span data-ttu-id="492d4-142">Állítsa be `$clusterName` toohello hello HDInsight-fürt nevét.</span><span class="sxs-lookup"><span data-stu-id="492d4-142">Set `$clusterName` toohello name of hello HDInsight cluster.</span></span> <span data-ttu-id="492d4-143">Állítsa be `$storageAccountName` toohello hello storage-fiók nevét.</span><span class="sxs-lookup"><span data-stu-id="492d4-143">Set `$storageAccountName` toohello name of hello storage account.</span></span> <span data-ttu-id="492d4-144">Amikor a rendszer kéri, adja meg a hello fürt bejelentkezési (rendszergazda) és jelszavát.</span><span class="sxs-lookup"><span data-stu-id="492d4-144">When prompted, enter hello cluster login (admin) and password.</span></span>

```Bash
curl -u admin:PASSWORD -G "https://CLUSTERNAME.azurehdinsight.net/api/v1/clusters/CLUSTERNAME/configurations/service_config_versions?service_name=HDFS&service_config_version=1" | jq '.items[].configurations[].properties["fs.azure.account.key.$STORAGEACCOUNTNAME.blob.core.windows.net"] | select(. != null)'
```

> [!NOTE]
> <span data-ttu-id="492d4-145">Állítsa be `$PASSWORD` toohello fürt (rendszergazda) bejelentkezési fiók jelszavát.</span><span class="sxs-lookup"><span data-stu-id="492d4-145">Set `$PASSWORD` toohello cluster login (admin) account password.</span></span> <span data-ttu-id="492d4-146">Állítsa be `$CLUSTERNAME` toohello hello HDInsight-fürt nevét.</span><span class="sxs-lookup"><span data-stu-id="492d4-146">Set `$CLUSTERNAME` toohello name of hello HDInsight cluster.</span></span> <span data-ttu-id="492d4-147">Állítsa be `$STORAGEACCOUNTNAME` toohello hello storage-fiók nevét.</span><span class="sxs-lookup"><span data-stu-id="492d4-147">Set `$STORAGEACCOUNTNAME` toohello name of hello storage account.</span></span>
>
> <span data-ttu-id="492d4-148">Ez a példa [curl (http://curl.haxx.se/)](http://curl.haxx.se/) és [jq (https://stedolan.github.io/jq/)](https://stedolan.github.io/jq/) tooretrieve és elemzési JSON-adatokat.</span><span class="sxs-lookup"><span data-stu-id="492d4-148">This example uses [curl (http://curl.haxx.se/)](http://curl.haxx.se/) and [jq (https://stedolan.github.io/jq/)](https://stedolan.github.io/jq/) tooretrieve and parse JSON data.</span></span>

<span data-ttu-id="492d4-149">Ha ezzel a paranccsal cserélje le __CLUSTERNAME__ hello nevű hello HDInsight-fürt.</span><span class="sxs-lookup"><span data-stu-id="492d4-149">When using this command, replace __CLUSTERNAME__ with hello name of hello HDInsight cluster.</span></span> <span data-ttu-id="492d4-150">Cserélje le __jelszó__ hello fürt hello HTTP bejelentkezési jelszót.</span><span class="sxs-lookup"><span data-stu-id="492d4-150">Replace __PASSWORD__ with hello HTTP login password for hello cluster.</span></span> <span data-ttu-id="492d4-151">Cserélje le __STORAGEACCOUNT__ hello nevű hello tárfiók hozzáadása parancsfájlművelet használatával.</span><span class="sxs-lookup"><span data-stu-id="492d4-151">Replace __STORAGEACCOUNT__ with hello name of hello storage account added using script action.</span></span> <span data-ttu-id="492d4-152">Ez a parancs által visszaadott adatokat a következő szöveg hasonló toohello jelenik meg:</span><span class="sxs-lookup"><span data-stu-id="492d4-152">Information returned from this command appears similar toohello following text:</span></span>

    "MIIB+gYJKoZIhvcNAQcDoIIB6zCCAecCAQAxggFaMIIBVgIBADA+MCoxKDAmBgNVBAMTH2RiZW5jcnlwdGlvbi5henVyZWhkaW5zaWdodC5uZXQCEA6GDZMW1oiESKFHFOOEgjcwDQYJKoZIhvcNAQEBBQAEggEATIuO8MJ45KEQAYBQld7WaRkJOWqaCLwFub9zNpscrquA2f3o0emy9Vr6vu5cD3GTt7PmaAF0pvssbKVMf/Z8yRpHmeezSco2y7e9Qd7xJKRLYtRHm80fsjiBHSW9CYkQwxHaOqdR7DBhZyhnj+DHhODsIO2FGM8MxWk4fgBRVO6CZ5eTmZ6KVR8wYbFLi8YZXb7GkUEeSn2PsjrKGiQjtpXw1RAyanCagr5vlg8CicZg1HuhCHWf/RYFWM3EBbVz+uFZPR3BqTgbvBhWYXRJaISwssvxotppe0ikevnEgaBYrflB2P+PVrwPTZ7f36HQcn4ifY1WRJQ4qRaUxdYEfzCBgwYJKoZIhvcNAQcBMBQGCCqGSIb3DQMHBAhRdscgRV3wmYBg3j/T1aEnO3wLWCRpgZa16MWqmfQPuansKHjLwbZjTpeirqUAQpZVyXdK/w4gKlK+t1heNsNo1Wwqu+Y47bSAX1k9Ud7+Ed2oETDI7724IJ213YeGxvu4Ngcf2eHW+FRK"

<span data-ttu-id="492d4-153">Ez a szöveg látható egy példa egy titkosított kulcsot, amely tooaccess hello storage-fiók.</span><span class="sxs-lookup"><span data-stu-id="492d4-153">This text is an example of an encrypted key, which is used tooaccess hello storage account.</span></span>

### <a name="unable-tooaccess-storage-after-changing-key"></a><span data-ttu-id="492d4-154">Nem lehet tooaccess tárolási kulcs módosítása után</span><span class="sxs-lookup"><span data-stu-id="492d4-154">Unable tooaccess storage after changing key</span></span>

<span data-ttu-id="492d4-155">Ha módosítja egy hello kulcsának, HDInsight már nem tud hozzáférni a hello tárfiók.</span><span class="sxs-lookup"><span data-stu-id="492d4-155">If you change hello key for a storage account, HDInsight can no longer access hello storage account.</span></span> <span data-ttu-id="492d4-156">HDInsight kulcs gyorsítótárazott másolatának hello core-site.xml hello fürt használja.</span><span class="sxs-lookup"><span data-stu-id="492d4-156">HDInsight uses a cached copy of key in hello core-site.xml for hello cluster.</span></span> <span data-ttu-id="492d4-157">A gyorsítótárban található példányát a frissített toomatch hello új kulcsot kell lennie.</span><span class="sxs-lookup"><span data-stu-id="492d4-157">This cached copy must be updated toomatch hello new key.</span></span>

<span data-ttu-id="492d4-158">Hello parancsfájlművelet újra fut does __nem__ hello kulcs frissíti hello parancsfájl toosee ellenőrzi, hogy hello storage-fiókhoz tartozó bejegyzés már létezik.</span><span class="sxs-lookup"><span data-stu-id="492d4-158">Running hello script action again does __not__ update hello key, as hello script checks toosee if an entry for hello storage account already exists.</span></span> <span data-ttu-id="492d4-159">Ha már létezik egy bejegyzést, tegye a módosításokat.</span><span class="sxs-lookup"><span data-stu-id="492d4-159">If an entry already exists, it does not make any changes.</span></span>

<span data-ttu-id="492d4-160">a probléma megoldásához toowork, el kell távolítania hello meglévő bejegyzés hello tárfiók.</span><span class="sxs-lookup"><span data-stu-id="492d4-160">toowork around this problem, you must remove hello existing entry for hello storage account.</span></span> <span data-ttu-id="492d4-161">A következő lépéseket tooremove hello meglévő bejegyzés hello használata:</span><span class="sxs-lookup"><span data-stu-id="492d4-161">Use hello following steps tooremove hello existing entry:</span></span>

1. <span data-ttu-id="492d4-162">Egy böngészőben nyissa meg a hello Ambari webes felhasználói felületén a HDInsight-fürthöz.</span><span class="sxs-lookup"><span data-stu-id="492d4-162">In a web browser, open hello Ambari Web UI for your HDInsight cluster.</span></span> <span data-ttu-id="492d4-163">hello URI https://CLUSTERNAME.azurehdinsight.net.</span><span class="sxs-lookup"><span data-stu-id="492d4-163">hello URI is https://CLUSTERNAME.azurehdinsight.net.</span></span> <span data-ttu-id="492d4-164">Cserélje le __CLUSTERNAME__ hello néven a fürt.</span><span class="sxs-lookup"><span data-stu-id="492d4-164">Replace __CLUSTERNAME__ with hello name of your cluster.</span></span>

    <span data-ttu-id="492d4-165">Amikor a rendszer kéri, adja meg bejelentkezési felhasználói hello HTTP és a jelszavát a fürt.</span><span class="sxs-lookup"><span data-stu-id="492d4-165">When prompted, enter hello HTTP login user and password for your cluster.</span></span>

2. <span data-ttu-id="492d4-166">Szolgáltatások hello bal oldali hello lap hello listában jelölje ki __HDFS__.</span><span class="sxs-lookup"><span data-stu-id="492d4-166">From hello list of services on hello left of hello page, select __HDFS__.</span></span> <span data-ttu-id="492d4-167">Válassza ki hello __Configs__ hello center hello lap lapján.</span><span class="sxs-lookup"><span data-stu-id="492d4-167">Then select hello __Configs__ tab in hello center of hello page.</span></span>

3. <span data-ttu-id="492d4-168">A hello __szűrő...__  mezőbe írja be az érték __fs.azure.account__.</span><span class="sxs-lookup"><span data-stu-id="492d4-168">In hello __Filter...__ field, enter a value of __fs.azure.account__.</span></span> <span data-ttu-id="492d4-169">Ez visszaad minden további tárfiókok toohello fürt hozzáadott bejegyzéseket.</span><span class="sxs-lookup"><span data-stu-id="492d4-169">This returns entries for any additional storage accounts that have been added toohello cluster.</span></span> <span data-ttu-id="492d4-170">Két különböző bejegyzések; __keyprovider__ és __kulcs__.</span><span class="sxs-lookup"><span data-stu-id="492d4-170">There are two types of entries; __keyprovider__ and __key__.</span></span> <span data-ttu-id="492d4-171">Mindkettő rendelkezik hello tárfiók hello kulcsnév részeként hello nevére.</span><span class="sxs-lookup"><span data-stu-id="492d4-171">Both contain hello name of hello storage account as part of hello key name.</span></span>

    <span data-ttu-id="492d4-172">hello bejegyzések a következők példa egy nevű tárfiók __mystorage__:</span><span class="sxs-lookup"><span data-stu-id="492d4-172">hello following are example entries for a storage account named __mystorage__:</span></span>

        fs.azure.account.keyprovider.mystorage.blob.core.windows.net
        fs.azure.account.key.mystorage.blob.core.windows.net

4. <span data-ttu-id="492d4-173">Miután azonosította hello kulcsokat hello tooremove van szüksége, a vörös hello '-' ikon toohello sarkában hello bejegyzés toodelete azt.</span><span class="sxs-lookup"><span data-stu-id="492d4-173">After you have identified hello keys for hello storage account you need tooremove, use hello red '-' icon toohello right of hello entry toodelete it.</span></span> <span data-ttu-id="492d4-174">Ezután használja az hello __mentése__ toosave gombra a módosítások.</span><span class="sxs-lookup"><span data-stu-id="492d4-174">Then use hello __Save__ button toosave your changes.</span></span>

5. <span data-ttu-id="492d4-175">Módosítások mentése után hello parancsfájl művelet tooadd hello tárfiókot és az új kulcs értéke toohello fürt használja.</span><span class="sxs-lookup"><span data-stu-id="492d4-175">After changes have been saved, use hello script action tooadd hello storage account and new key value toohello cluster.</span></span>

### <a name="poor-performance"></a><span data-ttu-id="492d4-176">Gyenge teljesítményt</span><span class="sxs-lookup"><span data-stu-id="492d4-176">Poor performance</span></span>

<span data-ttu-id="492d4-177">Ha hello tárfiók mint hello HDInsight-fürt egy másik régióban, gyenge teljesítményt tapasztalhat.</span><span class="sxs-lookup"><span data-stu-id="492d4-177">If hello storage account is in a different region than hello HDInsight cluster, you may experience poor performance.</span></span> <span data-ttu-id="492d4-178">Egy másik régióban küld a hálózati forgalom, hello regionális Azure adatközponton kívül és keresztben elérése során adatokat a nyilvános internethez, ami vethet fel késés hello.</span><span class="sxs-lookup"><span data-stu-id="492d4-178">Accessing data in a different region sends network traffic outside hello regional Azure data center and across hello public internet, which can introduce latency.</span></span>

> [!WARNING]
> <span data-ttu-id="492d4-179">A storage-fiók egy másik régióban, mint a HDInsight-fürt hello használata nem támogatott.</span><span class="sxs-lookup"><span data-stu-id="492d4-179">Using a storage account in a different region than hello HDInsight cluster is not supported.</span></span>

### <a name="additional-charges"></a><span data-ttu-id="492d4-180">További díjak</span><span class="sxs-lookup"><span data-stu-id="492d4-180">Additional charges</span></span>

<span data-ttu-id="492d4-181">Ha hello tárfiók más régióban, mint a HDInsight-fürt hello, további kilépő díjak Észreveheti az Azure számlázás a.</span><span class="sxs-lookup"><span data-stu-id="492d4-181">If hello storage account is in a different region than hello HDInsight cluster, you may notice additional egress charges on your Azure billing.</span></span> <span data-ttu-id="492d4-182">Egy kimenő kell fizetni akkor érvényes, ha az adatok egy regionális adatközpont hagyja.</span><span class="sxs-lookup"><span data-stu-id="492d4-182">An egress charge is applied when data leaves a regional data center.</span></span> <span data-ttu-id="492d4-183">Ez a díj lesz alkalmazva, akkor is, ha egy másik Azure-adatközpont egy másik régióban szánt hello forgalmat.</span><span class="sxs-lookup"><span data-stu-id="492d4-183">This charge is applied even if hello traffic is destined for another Azure data center in a different region.</span></span>

> [!WARNING]
> <span data-ttu-id="492d4-184">A storage-fiók egy másik régióban, mint a HDInsight-fürt hello használata nem támogatott.</span><span class="sxs-lookup"><span data-stu-id="492d4-184">Using a storage account in a different region than hello HDInsight cluster is not supported.</span></span>

## <a name="next-steps"></a><span data-ttu-id="492d4-185">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="492d4-185">Next steps</span></span>

<span data-ttu-id="492d4-186">Megtanulta, hogyan tooadd további tárfiókok tooan meglévő HDInsight-fürtre.</span><span class="sxs-lookup"><span data-stu-id="492d4-186">You have learned how tooadd additional storage accounts tooan existing HDInsight cluster.</span></span> <span data-ttu-id="492d4-187">A Parancsfájlműveletek további információkért lásd: [testreszabása Linux-alapú HDInsight-fürtök parancsfájlművelet használatával](hdinsight-hadoop-customize-cluster-linux.md)</span><span class="sxs-lookup"><span data-stu-id="492d4-187">For more information on script actions, see [Customize Linux-based HDInsight clusters using script action](hdinsight-hadoop-customize-cluster-linux.md)</span></span>
