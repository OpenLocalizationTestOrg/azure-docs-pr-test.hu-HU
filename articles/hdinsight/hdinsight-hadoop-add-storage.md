---
title: "További Azure storage-fiókok hozzáadása HDInsight |} Microsoft Docs"
description: "Ismerje meg, hogy további Azure storage-fiókok hozzáadása egy meglévő HDInsight-fürtre."
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
ms.openlocfilehash: 0853e8605e07c28867676e9c13b89263ade67c88
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/18/2017
---
# <a name="add-additional-storage-accounts-to-hdinsight"></a><span data-ttu-id="43128-103">HDInsight további storage-fiókok hozzáadása</span><span class="sxs-lookup"><span data-stu-id="43128-103">Add additional storage accounts to HDInsight</span></span>

<span data-ttu-id="43128-104">A Parancsfájlműveletek segítségével további Azure storage-fiókok hozzáadása HDInsight útmutató.</span><span class="sxs-lookup"><span data-stu-id="43128-104">Learn how to use script actions to add additional Azure storage accounts to HDInsight.</span></span> <span data-ttu-id="43128-105">A jelen dokumentumban leírt lépések tárfiók hozzáadása egy meglévő Linux-alapú HDInsight-fürthöz.</span><span class="sxs-lookup"><span data-stu-id="43128-105">The steps in this document add a storage account to an existing Linux-based HDInsight cluster.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="43128-106">A jelen dokumentumban szereplő információk további tárhely hozzáadása a fürt létrehozása után van.</span><span class="sxs-lookup"><span data-stu-id="43128-106">The information in this document is about adding additional storage to a cluster after it has been created.</span></span> <span data-ttu-id="43128-107">A storage-fiókok hozzáadása a fürt létrehozása során további információkért lásd: [állítsa be a HDInsight Hadoop, Spark, Kafka és több fürt](hdinsight-hadoop-provision-linux-clusters.md).</span><span class="sxs-lookup"><span data-stu-id="43128-107">For information on adding storage accounts during cluster creation, see [Set up clusters in HDInsight with Hadoop, Spark, Kafka, and more](hdinsight-hadoop-provision-linux-clusters.md).</span></span>

## <a name="how-it-works"></a><span data-ttu-id="43128-108">Működés</span><span class="sxs-lookup"><span data-stu-id="43128-108">How it works</span></span>

<span data-ttu-id="43128-109">Ezt a parancsfájlt a következő paramétereket fogadja:</span><span class="sxs-lookup"><span data-stu-id="43128-109">This script takes the following parameters:</span></span>

* <span data-ttu-id="43128-110">__Az Azure storage-fiók neve__: a HDInsight-fürt hozzáadása a tárfiók nevét.</span><span class="sxs-lookup"><span data-stu-id="43128-110">__Azure storage account name__: The name of the storage account to add to the HDInsight cluster.</span></span> <span data-ttu-id="43128-111">A parancsfájl futtatása után HDInsight olvashat és írhat adatokat tárolja ezt a tárfiókot.</span><span class="sxs-lookup"><span data-stu-id="43128-111">After running the script, HDInsight can read and write data stored in this storage account.</span></span>

* <span data-ttu-id="43128-112">__Azure storage-fiók kulcs__: A kulcs, amely hozzáférést biztosít a tárfiókhoz.</span><span class="sxs-lookup"><span data-stu-id="43128-112">__Azure storage account key__: A key that grants access to the storage account.</span></span>

* <span data-ttu-id="43128-113">__-p__ (nem kötelező): Ha meg van adva, a kulcs nem titkosított, és egyszerű szövegként a core-site.xml fájlban tárolja.</span><span class="sxs-lookup"><span data-stu-id="43128-113">__-p__ (optional): If specified, the key is not encrypted and is stored in the core-site.xml file as plain text.</span></span>

<span data-ttu-id="43128-114">A feldolgozás során a parancsfájl a következő műveleteket hajtja végre:</span><span class="sxs-lookup"><span data-stu-id="43128-114">During processing, the script performs the following actions:</span></span>

* <span data-ttu-id="43128-115">Ha a tárfiók már létezik a core-site.xml konfigurációban a fürt számára, a parancsfájl kilép, és nincs több teendő történik.</span><span class="sxs-lookup"><span data-stu-id="43128-115">If the storage account already exists in the core-site.xml configuration for the cluster, the script exits and no further actions are performed.</span></span>

* <span data-ttu-id="43128-116">Ellenőrzi, hogy a storage-fiók létezik-e, és a kulcs segítségével férhetők el.</span><span class="sxs-lookup"><span data-stu-id="43128-116">Verifies that the storage account exists and can be accessed using the key.</span></span>

* <span data-ttu-id="43128-117">Titkosítja a kulcsot, a fürt hitelesítő adatokkal.</span><span class="sxs-lookup"><span data-stu-id="43128-117">Encrypts the key using the cluster credential.</span></span>

* <span data-ttu-id="43128-118">A storage-fiók hozzáadása a core-site.xml fájl módosítása.</span><span class="sxs-lookup"><span data-stu-id="43128-118">Adds the storage account to the core-site.xml file.</span></span>

* <span data-ttu-id="43128-119">Leállítja és újraindítja a Oozie, YARN, MapReduce2 és HDFS szolgáltatásokat.</span><span class="sxs-lookup"><span data-stu-id="43128-119">Stops and restarts the Oozie, YARN, MapReduce2, and HDFS services.</span></span> <span data-ttu-id="43128-120">Ezek a szolgáltatások indítása és leállítása lehetővé teszi az új tárfiókot használja.</span><span class="sxs-lookup"><span data-stu-id="43128-120">Stopping and starting these services allows them to use the new storage account.</span></span>

> [!WARNING]
> <span data-ttu-id="43128-121">A storage-fiók egy másik helyen, mint a HDInsight-fürt használata nem támogatott.</span><span class="sxs-lookup"><span data-stu-id="43128-121">Using a storage account in a different location than the HDInsight cluster is not supported.</span></span>

## <a name="the-script"></a><span data-ttu-id="43128-122">A parancsfájl</span><span class="sxs-lookup"><span data-stu-id="43128-122">The script</span></span>

<span data-ttu-id="43128-123">__Parancsfájl-hely__: [https://hdiconfigactions.blob.core.windows.net/linuxaddstorageaccountv01/add-storage-account-v01.sh](https://hdiconfigactions.blob.core.windows.net/linuxaddstorageaccountv01/add-storage-account-v01.sh)</span><span class="sxs-lookup"><span data-stu-id="43128-123">__Script location__: [https://hdiconfigactions.blob.core.windows.net/linuxaddstorageaccountv01/add-storage-account-v01.sh](https://hdiconfigactions.blob.core.windows.net/linuxaddstorageaccountv01/add-storage-account-v01.sh)</span></span>

<span data-ttu-id="43128-124">__Követelmények__:</span><span class="sxs-lookup"><span data-stu-id="43128-124">__Requirements__:</span></span>

* <span data-ttu-id="43128-125">A parancsfájl azon kell alkalmazni a __Átjárócsomópontokat__.</span><span class="sxs-lookup"><span data-stu-id="43128-125">The script must be applied on the __Head nodes__.</span></span>

## <a name="to-use-the-script"></a><span data-ttu-id="43128-126">A parancsfájl használata</span><span class="sxs-lookup"><span data-stu-id="43128-126">To use the script</span></span>

<span data-ttu-id="43128-127">Ez a parancsfájl az Azure portál, Azure PowerShell vagy az Azure CLI 1.0 használható.</span><span class="sxs-lookup"><span data-stu-id="43128-127">This script can be used from the Azure portal, Azure PowerShell, or the Azure CLI 1.0.</span></span> <span data-ttu-id="43128-128">További információkért lásd: a [testreszabása Linux-alapú HDInsight-fürtök használata parancsfájlművelet](hdinsight-hadoop-customize-cluster-linux.md#apply-a-script-action-to-a-running-cluster) dokumentum.</span><span class="sxs-lookup"><span data-stu-id="43128-128">For more information, see the [Customize Linux-based HDInsight clusters using script action](hdinsight-hadoop-customize-cluster-linux.md#apply-a-script-action-to-a-running-cluster) document.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="43128-129">A testreszabási dokumentumban leírt lépések használata esetén olvassa el következő alkalmazni ezt a parancsfájlt:</span><span class="sxs-lookup"><span data-stu-id="43128-129">When using the steps provided in the customization document, use the following information to apply this script:</span></span>
>
> * <span data-ttu-id="43128-130">Cserélje le ezt a parancsfájlt (https://hdiconfigactions.blob.core.windows.net/linuxaddstorageaccountv01/add-storage-account-v01.sh) URI-JÁNAK bármely példa parancsfájlművelet URI.</span><span class="sxs-lookup"><span data-stu-id="43128-130">Replace any example script action URI with the URI for this script (https://hdiconfigactions.blob.core.windows.net/linuxaddstorageaccountv01/add-storage-account-v01.sh).</span></span>
> * <span data-ttu-id="43128-131">A példa paramétereket cserélje le a az Azure storage-fiók nevét és a kulcsot a tárfiók hozzá kell adni a fürthöz.</span><span class="sxs-lookup"><span data-stu-id="43128-131">Replace any example parameters with the Azure storage account name and key of the storage account to be added to the cluster.</span></span> <span data-ttu-id="43128-132">Az Azure portál használatával, ha ezeket a paramétereket szóközzel kell elválasztani.</span><span class="sxs-lookup"><span data-stu-id="43128-132">If using the Azure portal, these parameters must be separated by a space.</span></span>
> * <span data-ttu-id="43128-133">Nem kell megjelölni a parancsprogramot __megőrzött__, ahogy azt közvetlenül frissíti az Ambari konfigurációját a fürt számára.</span><span class="sxs-lookup"><span data-stu-id="43128-133">You do not need to mark this script as __Persisted__, as it directly updates the Ambari configuration for the cluster.</span></span>

## <a name="known-issues"></a><span data-ttu-id="43128-134">Ismert problémák</span><span class="sxs-lookup"><span data-stu-id="43128-134">Known issues</span></span>

### <a name="storage-accounts-not-displayed-in-azure-portal-or-tools"></a><span data-ttu-id="43128-135">Nem jelenik meg az Azure portálon vagy az eszközök a Storage-fiókok</span><span class="sxs-lookup"><span data-stu-id="43128-135">Storage accounts not displayed in Azure portal or tools</span></span>

<span data-ttu-id="43128-136">A HDInsight-fürthöz az Azure portálon megtekintésekor kiválasztása a __Tárfiókok__ bejegyzésre az __tulajdonságok__ nem jelennek meg a parancsfájlművelet hozzáadva storage-fiókok.</span><span class="sxs-lookup"><span data-stu-id="43128-136">When viewing the HDInsight cluster in the Azure portal, selecting the __Storage Accounts__ entry under __Properties__ does not display storage accounts added through this script action.</span></span> <span data-ttu-id="43128-137">Az Azure PowerShell és az Azure parancssori felület ne jelenjen meg a további tárfiók vagy.</span><span class="sxs-lookup"><span data-stu-id="43128-137">Azure PowerShell and Azure CLI do not display the additional storage account either.</span></span>

<span data-ttu-id="43128-138">A tárolással kapcsolatos nem jelenik meg, mert a parancsfájl csak módosítja a core-site.xml konfigurációját a fürtben.</span><span class="sxs-lookup"><span data-stu-id="43128-138">The storage information isn't displayed because the script only modifies the core-site.xml configuration for the cluster.</span></span> <span data-ttu-id="43128-139">Ezeket az információkat nem használja az Azure felügyeleti API-k használata a fürt-adatok beolvasása.</span><span class="sxs-lookup"><span data-stu-id="43128-139">This information is not used when retrieving the cluster information using Azure management APIs.</span></span>

<span data-ttu-id="43128-140">A parancsfájl a fürthöz hozzáadott tárfiókadatok megtekintéséhez használja az Ambari REST API-t.</span><span class="sxs-lookup"><span data-stu-id="43128-140">To view storage account information added to the cluster using this script, use the Ambari REST API.</span></span> <span data-ttu-id="43128-141">Az alábbi parancsokat használja ezt az információt talál a fürt beolvasásához:</span><span class="sxs-lookup"><span data-stu-id="43128-141">Use the following commands to retrieve this information for your cluster:</span></span>

```PowerShell
$creds = Get-Credential -UserName "admin" -Message "Enter the cluster login credentials"
$resp = Invoke-WebRequest -Uri "https://$clusterName.azurehdinsight.net/api/v1/clusters/$clusterName/configurations/service_config_versions?service_name=HDFS&service_config_version=1" `
    -Credential $creds
$respObj = ConvertFrom-Json $resp.Content
$respObj.items.configurations.properties."fs.azure.account.key.$storageAccountName.blob.core.windows.net"
```

> [!NOTE]
> <span data-ttu-id="43128-142">Állítsa be `$clusterName` a HDInsight-fürt nevére.</span><span class="sxs-lookup"><span data-stu-id="43128-142">Set `$clusterName` to the name of the HDInsight cluster.</span></span> <span data-ttu-id="43128-143">Állítsa be `$storageAccountName` a tárfiók nevére.</span><span class="sxs-lookup"><span data-stu-id="43128-143">Set `$storageAccountName` to the name of the storage account.</span></span> <span data-ttu-id="43128-144">Amikor a rendszer kéri, adja meg a fürt bejelentkezési (rendszergazda) és a jelszót.</span><span class="sxs-lookup"><span data-stu-id="43128-144">When prompted, enter the cluster login (admin) and password.</span></span>

```Bash
curl -u admin:PASSWORD -G "https://CLUSTERNAME.azurehdinsight.net/api/v1/clusters/CLUSTERNAME/configurations/service_config_versions?service_name=HDFS&service_config_version=1" | jq '.items[].configurations[].properties["fs.azure.account.key.$STORAGEACCOUNTNAME.blob.core.windows.net"] | select(. != null)'
```

> [!NOTE]
> <span data-ttu-id="43128-145">Állítsa be `$PASSWORD` az a fürt (rendszergazda) bejelentkezési fiók jelszavát.</span><span class="sxs-lookup"><span data-stu-id="43128-145">Set `$PASSWORD` to the cluster login (admin) account password.</span></span> <span data-ttu-id="43128-146">Állítsa be `$CLUSTERNAME` a HDInsight-fürt nevére.</span><span class="sxs-lookup"><span data-stu-id="43128-146">Set `$CLUSTERNAME` to the name of the HDInsight cluster.</span></span> <span data-ttu-id="43128-147">Állítsa be `$STORAGEACCOUNTNAME` a tárfiók nevére.</span><span class="sxs-lookup"><span data-stu-id="43128-147">Set `$STORAGEACCOUNTNAME` to the name of the storage account.</span></span>
>
> <span data-ttu-id="43128-148">Ez a példa [curl (http://curl.haxx.se/)](http://curl.haxx.se/) és [jq (https://stedolan.github.io/jq/)](https://stedolan.github.io/jq/) beolvasása és elemzése JSON-adatokat.</span><span class="sxs-lookup"><span data-stu-id="43128-148">This example uses [curl (http://curl.haxx.se/)](http://curl.haxx.se/) and [jq (https://stedolan.github.io/jq/)](https://stedolan.github.io/jq/) to retrieve and parse JSON data.</span></span>

<span data-ttu-id="43128-149">Ha ezzel a paranccsal cserélje le __CLUSTERNAME__ a HDInsight-fürt nevét.</span><span class="sxs-lookup"><span data-stu-id="43128-149">When using this command, replace __CLUSTERNAME__ with the name of the HDInsight cluster.</span></span> <span data-ttu-id="43128-150">Cserélje le __jelszó__ a fürt HTTP bejelentkezési jelszavával.</span><span class="sxs-lookup"><span data-stu-id="43128-150">Replace __PASSWORD__ with the HTTP login password for the cluster.</span></span> <span data-ttu-id="43128-151">Cserélje le __STORAGEACCOUNT__ parancsfájlművelet segítségével adhatók hozzá a tárfiók nevével.</span><span class="sxs-lookup"><span data-stu-id="43128-151">Replace __STORAGEACCOUNT__ with the name of the storage account added using script action.</span></span> <span data-ttu-id="43128-152">Ez a parancs által visszaadott információ jelenik meg az alábbihoz hasonló:</span><span class="sxs-lookup"><span data-stu-id="43128-152">Information returned from this command appears similar to the following text:</span></span>

    "MIIB+gYJKoZIhvcNAQcDoIIB6zCCAecCAQAxggFaMIIBVgIBADA+MCoxKDAmBgNVBAMTH2RiZW5jcnlwdGlvbi5henVyZWhkaW5zaWdodC5uZXQCEA6GDZMW1oiESKFHFOOEgjcwDQYJKoZIhvcNAQEBBQAEggEATIuO8MJ45KEQAYBQld7WaRkJOWqaCLwFub9zNpscrquA2f3o0emy9Vr6vu5cD3GTt7PmaAF0pvssbKVMf/Z8yRpHmeezSco2y7e9Qd7xJKRLYtRHm80fsjiBHSW9CYkQwxHaOqdR7DBhZyhnj+DHhODsIO2FGM8MxWk4fgBRVO6CZ5eTmZ6KVR8wYbFLi8YZXb7GkUEeSn2PsjrKGiQjtpXw1RAyanCagr5vlg8CicZg1HuhCHWf/RYFWM3EBbVz+uFZPR3BqTgbvBhWYXRJaISwssvxotppe0ikevnEgaBYrflB2P+PVrwPTZ7f36HQcn4ifY1WRJQ4qRaUxdYEfzCBgwYJKoZIhvcNAQcBMBQGCCqGSIb3DQMHBAhRdscgRV3wmYBg3j/T1aEnO3wLWCRpgZa16MWqmfQPuansKHjLwbZjTpeirqUAQpZVyXdK/w4gKlK+t1heNsNo1Wwqu+Y47bSAX1k9Ud7+Ed2oETDI7724IJ213YeGxvu4Ngcf2eHW+FRK"

<span data-ttu-id="43128-153">Ez a szöveg egy titkosított kulcsot, amely a tárfiók eléréséhez használt példája.</span><span class="sxs-lookup"><span data-stu-id="43128-153">This text is an example of an encrypted key, which is used to access the storage account.</span></span>

### <a name="unable-to-access-storage-after-changing-key"></a><span data-ttu-id="43128-154">Nem érhető el tárhely kulcs módosítása után</span><span class="sxs-lookup"><span data-stu-id="43128-154">Unable to access storage after changing key</span></span>

<span data-ttu-id="43128-155">Ha módosítja a tárfiók kulcsa, HDInsight már nem tud hozzáférni a tárfiókhoz.</span><span class="sxs-lookup"><span data-stu-id="43128-155">If you change the key for a storage account, HDInsight can no longer access the storage account.</span></span> <span data-ttu-id="43128-156">HDInsight a core-site.xml kulcs gyorsítótárazott másolatának a fürt használja.</span><span class="sxs-lookup"><span data-stu-id="43128-156">HDInsight uses a cached copy of key in the core-site.xml for the cluster.</span></span> <span data-ttu-id="43128-157">A gyorsítótárban található példányát frissíteni kell, hogy meg az új.</span><span class="sxs-lookup"><span data-stu-id="43128-157">This cached copy must be updated to match the new key.</span></span>

<span data-ttu-id="43128-158">A parancsfájlművelet újra fut does __nem__ frissíti a kulcsot, a parancsfájl ellenőrzi, hogy ha egy bejegyzést a tárfiók már létezik.</span><span class="sxs-lookup"><span data-stu-id="43128-158">Running the script action again does __not__ update the key, as the script checks to see if an entry for the storage account already exists.</span></span> <span data-ttu-id="43128-159">Ha már létezik egy bejegyzést, tegye a módosításokat.</span><span class="sxs-lookup"><span data-stu-id="43128-159">If an entry already exists, it does not make any changes.</span></span>

<span data-ttu-id="43128-160">Ez a probléma megoldása érdekében el kell távolítania a meglévő bejegyzést a tárfiók.</span><span class="sxs-lookup"><span data-stu-id="43128-160">To work around this problem, you must remove the existing entry for the storage account.</span></span> <span data-ttu-id="43128-161">Az alábbi lépések segítségével távolítsa el a meglévő bejegyzést:</span><span class="sxs-lookup"><span data-stu-id="43128-161">Use the following steps to remove the existing entry:</span></span>

1. <span data-ttu-id="43128-162">Egy webböngészőben nyissa meg az Ambari webes felhasználói felülete a HDInsight-fürthöz.</span><span class="sxs-lookup"><span data-stu-id="43128-162">In a web browser, open the Ambari Web UI for your HDInsight cluster.</span></span> <span data-ttu-id="43128-163">Az URI https://CLUSTERNAME.azurehdinsight.net.</span><span class="sxs-lookup"><span data-stu-id="43128-163">The URI is https://CLUSTERNAME.azurehdinsight.net.</span></span> <span data-ttu-id="43128-164">Cserélje le a __CLUSTERNAME__ elemet a fürt nevére.</span><span class="sxs-lookup"><span data-stu-id="43128-164">Replace __CLUSTERNAME__ with the name of your cluster.</span></span>

    <span data-ttu-id="43128-165">Amikor a rendszer kéri, adja meg a bejelentkezési felhasználói HTTP és a jelszót a fürt számára.</span><span class="sxs-lookup"><span data-stu-id="43128-165">When prompted, enter the HTTP login user and password for your cluster.</span></span>

2. <span data-ttu-id="43128-166">Válassza ki a listáról a lap bal oldalon található szolgáltatások __HDFS__.</span><span class="sxs-lookup"><span data-stu-id="43128-166">From the list of services on the left of the page, select __HDFS__.</span></span> <span data-ttu-id="43128-167">Válassza ki a __Configs__ fülre az oldal közepére.</span><span class="sxs-lookup"><span data-stu-id="43128-167">Then select the __Configs__ tab in the center of the page.</span></span>

3. <span data-ttu-id="43128-168">Az a __szűrő...__  mezőbe írja be az érték __fs.azure.account__.</span><span class="sxs-lookup"><span data-stu-id="43128-168">In the __Filter...__ field, enter a value of __fs.azure.account__.</span></span> <span data-ttu-id="43128-169">Ez visszaad minden további tárfiókok a fürthöz hozzáadott bejegyzéseket.</span><span class="sxs-lookup"><span data-stu-id="43128-169">This returns entries for any additional storage accounts that have been added to the cluster.</span></span> <span data-ttu-id="43128-170">Két különböző bejegyzések; __keyprovider__ és __kulcs__.</span><span class="sxs-lookup"><span data-stu-id="43128-170">There are two types of entries; __keyprovider__ and __key__.</span></span> <span data-ttu-id="43128-171">Mindkettő rendelkezik a kulcsnév részeként a tárfiók nevét.</span><span class="sxs-lookup"><span data-stu-id="43128-171">Both contain the name of the storage account as part of the key name.</span></span>

    <span data-ttu-id="43128-172">A bejegyzések a következők példa egy nevű tárfiók __mystorage__:</span><span class="sxs-lookup"><span data-stu-id="43128-172">The following are example entries for a storage account named __mystorage__:</span></span>

        fs.azure.account.keyprovider.mystorage.blob.core.windows.net
        fs.azure.account.key.mystorage.blob.core.windows.net

4. <span data-ttu-id="43128-173">Miután azonosította a kulcsokat a tárfiók, el kell távolítania, használja a vörös "-" jobb oldalán a bejegyzés törli-e ikon.</span><span class="sxs-lookup"><span data-stu-id="43128-173">After you have identified the keys for the storage account you need to remove, use the red '-' icon to the right of the entry to delete it.</span></span> <span data-ttu-id="43128-174">Ezután a __mentése__ gombra a módosítások mentéséhez.</span><span class="sxs-lookup"><span data-stu-id="43128-174">Then use the __Save__ button to save your changes.</span></span>

5. <span data-ttu-id="43128-175">Miután menti, a parancsfájl művelettel új kulcs értékét és a tárolási fiók hozzáadása a fürthöz.</span><span class="sxs-lookup"><span data-stu-id="43128-175">After changes have been saved, use the script action to add the storage account and new key value to the cluster.</span></span>

### <a name="poor-performance"></a><span data-ttu-id="43128-176">Gyenge teljesítményt</span><span class="sxs-lookup"><span data-stu-id="43128-176">Poor performance</span></span>

<span data-ttu-id="43128-177">Ha a tárfiók egy másik régióban, mint a HDInsight-fürthöz, gyenge teljesítményt tapasztalhat.</span><span class="sxs-lookup"><span data-stu-id="43128-177">If the storage account is in a different region than the HDInsight cluster, you may experience poor performance.</span></span> <span data-ttu-id="43128-178">Egy másik régióban található adatokhoz hozzáférő elküldi a hálózati forgalom, a regionális Azure adatközponton kívül és vethet fel várakozási ideje a nyilvános interneten keresztül történő.</span><span class="sxs-lookup"><span data-stu-id="43128-178">Accessing data in a different region sends network traffic outside the regional Azure data center and across the public internet, which can introduce latency.</span></span>

> [!WARNING]
> <span data-ttu-id="43128-179">A storage-fiók egy másik régióban, mint a HDInsight-fürt használata nem támogatott.</span><span class="sxs-lookup"><span data-stu-id="43128-179">Using a storage account in a different region than the HDInsight cluster is not supported.</span></span>

### <a name="additional-charges"></a><span data-ttu-id="43128-180">További díjak</span><span class="sxs-lookup"><span data-stu-id="43128-180">Additional charges</span></span>

<span data-ttu-id="43128-181">Ha a tárfiók egy másik régióban, mint a HDInsight-fürthöz, további kilépő díjak Észreveheti az Azure számlázás a.</span><span class="sxs-lookup"><span data-stu-id="43128-181">If the storage account is in a different region than the HDInsight cluster, you may notice additional egress charges on your Azure billing.</span></span> <span data-ttu-id="43128-182">Egy kimenő kell fizetni akkor érvényes, ha az adatok egy regionális adatközpont hagyja.</span><span class="sxs-lookup"><span data-stu-id="43128-182">An egress charge is applied when data leaves a regional data center.</span></span> <span data-ttu-id="43128-183">Ez a díj lesz alkalmazva, akkor is, ha a forgalom egy másik Azure-adatközpont egy másik régióban szánt.</span><span class="sxs-lookup"><span data-stu-id="43128-183">This charge is applied even if the traffic is destined for another Azure data center in a different region.</span></span>

> [!WARNING]
> <span data-ttu-id="43128-184">A storage-fiók egy másik régióban, mint a HDInsight-fürt használata nem támogatott.</span><span class="sxs-lookup"><span data-stu-id="43128-184">Using a storage account in a different region than the HDInsight cluster is not supported.</span></span>

## <a name="next-steps"></a><span data-ttu-id="43128-185">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="43128-185">Next steps</span></span>

<span data-ttu-id="43128-186">Megtanulhatta, további tárfiókok hozzáadásáról meglévő HDInsight-fürtre.</span><span class="sxs-lookup"><span data-stu-id="43128-186">You have learned how to add additional storage accounts to an existing HDInsight cluster.</span></span> <span data-ttu-id="43128-187">A Parancsfájlműveletek további információkért lásd: [testreszabása Linux-alapú HDInsight-fürtök parancsfájlművelet használatával](hdinsight-hadoop-customize-cluster-linux.md)</span><span class="sxs-lookup"><span data-stu-id="43128-187">For more information on script actions, see [Customize Linux-based HDInsight clusters using script action](hdinsight-hadoop-customize-cluster-linux.md)</span></span>
