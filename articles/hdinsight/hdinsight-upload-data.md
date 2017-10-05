---
title: "Hdinsight Hadoop-feladatokat az adatok feltöltése |} Microsoft Docs"
description: "Megtudhatja, hogyan töltse fel, és hozzáférési adatokat az Azure parancssori felület, a Azure Tártallózó, Azure PowerShell, a Hadoop parancssor vagy a Sqoop használatával hdinsight Hadoop-feladatokat."
keywords: "etl hadoop adatok beérkezése hadoop, hadoop adatok betöltése"
services: hdinsight,storage
documentationcenter: 
tags: azure-portal
author: mumian
manager: jhubbard
editor: cgronlun
ms.assetid: 56b913ee-0f9a-4e9f-9eaf-c571f8603dd6
ms.service: hdinsight
ms.custom: hdinsightactive,hdiseo17may2017
ms.workload: big-data
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/12/2017
ms.author: jgao
ms.openlocfilehash: 6867f96c8ea0e31ed0e682cef48e7aa5e3f65f86
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/29/2017
---
# <a name="upload-data-for-hadoop-jobs-in-hdinsight"></a><span data-ttu-id="6a465-104">Hadoop-feladatok adatainak feltöltése a HDInsightba</span><span class="sxs-lookup"><span data-stu-id="6a465-104">Upload data for Hadoop jobs in HDInsight</span></span>
<span data-ttu-id="6a465-105">Az Azure HDInsight egy teljes körű Hadoop elosztott fájlrendszer (HDFS) biztosítanak Azure Blob Storage tárolóban.</span><span class="sxs-lookup"><span data-stu-id="6a465-105">Azure HDInsight provides a full-featured Hadoop distributed file system (HDFS) over Azure Blob storage.</span></span> <span data-ttu-id="6a465-106">HDFS bővítményként való zökkenőmentes élményt nyújtsanak az ügyfeleinek tervezték.</span><span class="sxs-lookup"><span data-stu-id="6a465-106">It is designed as an HDFS extension to provide a seamless experience to customers.</span></span> <span data-ttu-id="6a465-107">Ez lehetővé teszi a Hadoop ökoszisztémájának közvetlenül az általa felügyelt adatok való működésre összetevők teljes készlete.</span><span class="sxs-lookup"><span data-stu-id="6a465-107">It enables the full set of components in the Hadoop ecosystem to operate directly on the data it manages.</span></span> <span data-ttu-id="6a465-108">Az Azure Blob storage és HDFS különböző fájlrendszerek, adatokat és a számítások az adatok tárolására vannak optimalizálva.</span><span class="sxs-lookup"><span data-stu-id="6a465-108">Azure Blob storage and HDFS are distinct file systems that are optimized for storage of data and computations on that data.</span></span> <span data-ttu-id="6a465-109">Az Azure Blob storage használatával előnyeivel kapcsolatos információt, [használata Azure Blob storage a hdinsight eszközzel][hdinsight-storage].</span><span class="sxs-lookup"><span data-stu-id="6a465-109">For information about the benefits of using Azure Blob storage, see [Use Azure Blob storage with HDInsight][hdinsight-storage].</span></span>

<span data-ttu-id="6a465-110">**Előfeltételek**</span><span class="sxs-lookup"><span data-stu-id="6a465-110">**Prerequisites**</span></span>

<span data-ttu-id="6a465-111">Mielőtt elkezdené, vegye figyelembe az alábbi követelményeknek:</span><span class="sxs-lookup"><span data-stu-id="6a465-111">Note the following requirement before you begin:</span></span>

* <span data-ttu-id="6a465-112">Egy Azure HDInsight-fürtre.</span><span class="sxs-lookup"><span data-stu-id="6a465-112">An Azure HDInsight cluster.</span></span> <span data-ttu-id="6a465-113">Útmutatásért lásd: [Ismerkedés az Azure HDInsight] [ hdinsight-get-started] vagy [Provision HDInsight clusters][hdinsight-provision].</span><span class="sxs-lookup"><span data-stu-id="6a465-113">For instructions, see [Get started with Azure HDInsight][hdinsight-get-started] or [Provision HDInsight clusters][hdinsight-provision].</span></span>

## <a name="why-blob-storage"></a><span data-ttu-id="6a465-114">Miért blob storage?</span><span class="sxs-lookup"><span data-stu-id="6a465-114">Why blob storage?</span></span>
<span data-ttu-id="6a465-115">Az Azure HDInsight-fürtök általában telepítve van a MapReduce-feladatok futtatásához, és a fürtök eldobott datagramok, ezek a feladatok befejezése után.</span><span class="sxs-lookup"><span data-stu-id="6a465-115">Azure HDInsight clusters are typically deployed to run MapReduce jobs, and the clusters are dropped after these jobs complete.</span></span> <span data-ttu-id="6a465-116">Gondoskodik az adatok a HDFS-ben fürtök számítások végrehajtását követően lenne egy drága módja adatainak tárolásához.</span><span class="sxs-lookup"><span data-stu-id="6a465-116">Keeping the data in the HDFS clusters after computations are complete would be an expensive way to store this data.</span></span> <span data-ttu-id="6a465-117">Az Azure Blob storage egy magas rendelkezésre állású, nagymértékben méretezhető, nagy kapacitás, a HDInsight eszközzel feldolgozandó adatok alacsony költségű, és megosztható tárolási lehetőség.</span><span class="sxs-lookup"><span data-stu-id="6a465-117">Azure Blob storage is a highly available, highly scalable, high capacity, low cost, and shareable storage option for data that is to be processed using HDInsight.</span></span> <span data-ttu-id="6a465-118">Adattárolás egy blobba lehetővé teszi, hogy biztonságosan kiadandó adatok elvesztése nélkül számításhoz használt HDInsight fürtöket.</span><span class="sxs-lookup"><span data-stu-id="6a465-118">Storing data in a blob enables the HDInsight clusters that are used for computation to be safely released without losing data.</span></span>

### <a name="directories"></a><span data-ttu-id="6a465-119">Könyvtárak</span><span class="sxs-lookup"><span data-stu-id="6a465-119">Directories</span></span>
<span data-ttu-id="6a465-120">Az Azure Blob storage tárolók kulcs/érték párként adatok tárolásához, és nincs könyvtár-hierarchia.</span><span class="sxs-lookup"><span data-stu-id="6a465-120">Azure Blob storage containers store data as key/value pairs, and there is no directory hierarchy.</span></span> <span data-ttu-id="6a465-121">Azonban a "/" karakter használható a kulcsnévben hogy úgy tűnjön, mintha a fájl könyvtárszerkezetben tárolódik.</span><span class="sxs-lookup"><span data-stu-id="6a465-121">However the "/" character can be used within the key name to make it appear as if a file is stored within a directory structure.</span></span> <span data-ttu-id="6a465-122">HDInsight ezek látja, mintha tényleges könyvtárak is.</span><span class="sxs-lookup"><span data-stu-id="6a465-122">HDInsight sees these as if they are actual directories.</span></span>

<span data-ttu-id="6a465-123">Egy blob kulcsa lehet például az *input/log1.txt*.</span><span class="sxs-lookup"><span data-stu-id="6a465-123">For example, a blob's key may be *input/log1.txt*.</span></span> <span data-ttu-id="6a465-124">Nem tényleges "bemeneti" könyvtár létezik, de mivel jelen van a "/" karakter a kulcsnévben, egy fájl elérési útját megjelenésének rendelkezik.</span><span class="sxs-lookup"><span data-stu-id="6a465-124">No actual "input" directory exists, but due to the presence of the "/" character in the key name, it has the appearance of a file path.</span></span>

<span data-ttu-id="6a465-125">Ebből kifolyólag Azure Explorer eszközök használatakor előfordulhat néhány 0 bájt fájlt.</span><span class="sxs-lookup"><span data-stu-id="6a465-125">Because of this, if you use Azure Explorer tools you may notice some 0 byte files.</span></span> <span data-ttu-id="6a465-126">Ezeket a fájlokat két célokra szolgál:</span><span class="sxs-lookup"><span data-stu-id="6a465-126">These files serve two purposes:</span></span>

* <span data-ttu-id="6a465-127">Ha üres mappák, azok jelölje meg a mappa meglétét.</span><span class="sxs-lookup"><span data-stu-id="6a465-127">If there are empty folders, they mark of the existence of the folder.</span></span> <span data-ttu-id="6a465-128">Az Azure Blob storage elég intelligens tudnia, hogy ha PEL/sáv nevű blob már létezik, nincs nevű mappában **PEL**.</span><span class="sxs-lookup"><span data-stu-id="6a465-128">Azure Blob storage is clever enough to know that if a blob called foo/bar exists, there is a folder called **foo**.</span></span> <span data-ttu-id="6a465-129">Az egyetlen lehetőség jelölésére üres mappa neve azonban **PEL** azzal, hogy ez a különleges 0 bájt fájl érvényben van.</span><span class="sxs-lookup"><span data-stu-id="6a465-129">But the only way to signify an empty folder called **foo** is by having this special 0 byte file in place.</span></span>
* <span data-ttu-id="6a465-130">A Hadoop-fájlrendszer, nevezetesen engedélyeit és a mappák tulajdonosait által igényelt különleges metaadatok tartanak fenn.</span><span class="sxs-lookup"><span data-stu-id="6a465-130">They hold special metadata that is needed by the Hadoop file system, notably the permissions and owners for the folders.</span></span>

## <a name="command-line-utilities"></a><span data-ttu-id="6a465-131">Parancssori segédprogramok</span><span class="sxs-lookup"><span data-stu-id="6a465-131">Command-line utilities</span></span>
<span data-ttu-id="6a465-132">A Microsoft az Azure Blob storage szolgáltatással működéséhez az alábbi segédprogramokat biztosít:</span><span class="sxs-lookup"><span data-stu-id="6a465-132">Microsoft provides the following utilities to work with Azure Blob storage:</span></span>

| <span data-ttu-id="6a465-133">Eszköz</span><span class="sxs-lookup"><span data-stu-id="6a465-133">Tool</span></span> | <span data-ttu-id="6a465-134">Linux</span><span class="sxs-lookup"><span data-stu-id="6a465-134">Linux</span></span> | <span data-ttu-id="6a465-135">OS X</span><span class="sxs-lookup"><span data-stu-id="6a465-135">OS X</span></span> | <span data-ttu-id="6a465-136">Windows</span><span class="sxs-lookup"><span data-stu-id="6a465-136">Windows</span></span> |
| --- |:---:|:---:|:---:|
| <span data-ttu-id="6a465-137">[Azure parancssori felület][azurecli]</span><span class="sxs-lookup"><span data-stu-id="6a465-137">[Azure Command-Line Interface][azurecli]</span></span> |<span data-ttu-id="6a465-138">✔</span><span class="sxs-lookup"><span data-stu-id="6a465-138">✔</span></span> |<span data-ttu-id="6a465-139">✔</span><span class="sxs-lookup"><span data-stu-id="6a465-139">✔</span></span> |<span data-ttu-id="6a465-140">✔</span><span class="sxs-lookup"><span data-stu-id="6a465-140">✔</span></span> |
| <span data-ttu-id="6a465-141">[Az Azure PowerShell][azure-powershell]</span><span class="sxs-lookup"><span data-stu-id="6a465-141">[Azure PowerShell][azure-powershell]</span></span> | | |<span data-ttu-id="6a465-142">✔</span><span class="sxs-lookup"><span data-stu-id="6a465-142">✔</span></span> |
| <span data-ttu-id="6a465-143">[AzCopy][azure-azcopy]</span><span class="sxs-lookup"><span data-stu-id="6a465-143">[AzCopy][azure-azcopy]</span></span> | | |<span data-ttu-id="6a465-144">✔</span><span class="sxs-lookup"><span data-stu-id="6a465-144">✔</span></span> |
| [<span data-ttu-id="6a465-145">Hadoop parancs</span><span class="sxs-lookup"><span data-stu-id="6a465-145">Hadoop command</span></span>](#commandline) |<span data-ttu-id="6a465-146">✔</span><span class="sxs-lookup"><span data-stu-id="6a465-146">✔</span></span> |<span data-ttu-id="6a465-147">✔</span><span class="sxs-lookup"><span data-stu-id="6a465-147">✔</span></span> |<span data-ttu-id="6a465-148">✔</span><span class="sxs-lookup"><span data-stu-id="6a465-148">✔</span></span> |

> [!NOTE]
> <span data-ttu-id="6a465-149">Amíg az Azure CLI-t, az Azure PowerShell és az AzCopy segítségével minden külső Azure, a Hadoop parancs csak akkor érhető el, a HDInsight-fürt, és csak a helyi fájlrendszer adatok betöltését Azure Blob storage lehetővé használható.</span><span class="sxs-lookup"><span data-stu-id="6a465-149">While the Azure CLI, Azure PowerShell, and AzCopy can all be used from outside Azure, the Hadoop command is only available on the HDInsight cluster and only allows loading data from the local file system into Azure Blob storage.</span></span>
>
>

### <span data-ttu-id="6a465-150"><a id="xplatcli"></a>Az Azure parancssori felület</span><span class="sxs-lookup"><span data-stu-id="6a465-150"><a id="xplatcli"></a>Azure CLI</span></span>
<span data-ttu-id="6a465-151">Az Azure parancssori felület az Azure-szolgáltatások kezelése platformfüggetlen eszközzel.</span><span class="sxs-lookup"><span data-stu-id="6a465-151">The Azure CLI is a cross-platform tool that allows you to manage Azure services.</span></span> <span data-ttu-id="6a465-152">Az alábbi lépések segítségével feltölteni az adatokat az Azure Blob storage:</span><span class="sxs-lookup"><span data-stu-id="6a465-152">Use the following steps to upload data to Azure Blob storage:</span></span>

[!INCLUDE [use-latest-version](../../includes/hdinsight-use-latest-cli.md)]

1. <span data-ttu-id="6a465-153">[Telepítse és konfigurálja az Azure parancssori felület Mac, Linux és Windows](../cli-install-nodejs.md).</span><span class="sxs-lookup"><span data-stu-id="6a465-153">[Install and configure the Azure CLI for Mac, Linux and Windows](../cli-install-nodejs.md).</span></span>
2. <span data-ttu-id="6a465-154">Nyisson meg egy parancssort, bash vagy más rendszerhéj, és használja a következő hitelesítéséhez az Azure-előfizetéshez.</span><span class="sxs-lookup"><span data-stu-id="6a465-154">Open a command prompt, bash, or other shell, and use the following to authenticate to your Azure subscription.</span></span>

        azure login

    <span data-ttu-id="6a465-155">Amikor a rendszer kéri, adja meg a felhasználónevet és jelszót az előfizetéséhez.</span><span class="sxs-lookup"><span data-stu-id="6a465-155">When prompted, enter the user name and password for your subscription.</span></span>
3. <span data-ttu-id="6a465-156">Adja meg a következő paranccsal listát készíthet a storage-fiókok az előfizetéséhez:</span><span class="sxs-lookup"><span data-stu-id="6a465-156">Enter the following command to list the storage accounts for your subscription:</span></span>

        azure storage account list
4. <span data-ttu-id="6a465-157">Válassza ki a tárfiókot, amely tartalmazza a blob, amellyel dolgozni szeretne, majd beolvasni a fiók a kulcsot a következő paranccsal:</span><span class="sxs-lookup"><span data-stu-id="6a465-157">Select the storage account that contains the blob you want to work with, then use the following command to retrieve the key for this account:</span></span>

        azure storage account keys list <storage-account-name>

    <span data-ttu-id="6a465-158">Ez visszaadja **elsődleges** és **másodlagos** kulcsok.</span><span class="sxs-lookup"><span data-stu-id="6a465-158">This should return **Primary** and **Secondary** keys.</span></span> <span data-ttu-id="6a465-159">Másolás a **elsődleges** kulcs értékét, mert a következő lépésben lesz használható.</span><span class="sxs-lookup"><span data-stu-id="6a465-159">Copy the **Primary** key value because it will be used in the next steps.</span></span>
5. <span data-ttu-id="6a465-160">A következő paranccsal kérje le a tárfiókon belül blob tárolók listáját:</span><span class="sxs-lookup"><span data-stu-id="6a465-160">Use the following command to retrieve a list of blob containers within the storage account:</span></span>

        azure storage container list -a <storage-account-name> -k <primary-key>
6. <span data-ttu-id="6a465-161">Fájlok feltöltését és letöltését a blobra mutató használja a következő parancsokat:</span><span class="sxs-lookup"><span data-stu-id="6a465-161">Use the following commands to upload and download files to the blob:</span></span>

   * <span data-ttu-id="6a465-162">A fájl feltöltéséhez:</span><span class="sxs-lookup"><span data-stu-id="6a465-162">To upload a file:</span></span>

           azure storage blob upload -a <storage-account-name> -k <primary-key> <source-file> <container-name> <blob-name>
   * <span data-ttu-id="6a465-163">A fájl letöltésére:</span><span class="sxs-lookup"><span data-stu-id="6a465-163">To download a file:</span></span>

           azure storage blob download -a <storage-account-name> -k <primary-key> <container-name> <blob-name> <destination-file>

> [!NOTE]
> <span data-ttu-id="6a465-164">Mindig dolgozni fog a tárfiókon, ha a fiók megadása helyett a következő környezeti változók értékét, és billentyűt minden parancs:</span><span class="sxs-lookup"><span data-stu-id="6a465-164">If you will always be working with the same storage account, you can set the following environment variables instead of specifying the account and key for every command:</span></span>
>
> * <span data-ttu-id="6a465-165">**AZURE\_tárolási\_fiók**: A tárfiók neve</span><span class="sxs-lookup"><span data-stu-id="6a465-165">**AZURE\_STORAGE\_ACCOUNT**: The storage account name</span></span>
> * <span data-ttu-id="6a465-166">**AZURE\_tárolási\_hozzáférés\_kulcs**: A tárfiók kulcsa</span><span class="sxs-lookup"><span data-stu-id="6a465-166">**AZURE\_STORAGE\_ACCESS\_KEY**: The storage account key</span></span>
>
>

### <span data-ttu-id="6a465-167"><a id="powershell"></a>Az Azure PowerShell</span><span class="sxs-lookup"><span data-stu-id="6a465-167"><a id="powershell"></a>Azure PowerShell</span></span>
<span data-ttu-id="6a465-168">Az Azure PowerShell egy parancsfájl-kezelési környezet, melyekkel automatizálhatja a központi telepítési és felügyeleti a munkaterhelések az Azure-ban, és szabályozhatja.</span><span class="sxs-lookup"><span data-stu-id="6a465-168">Azure PowerShell is a scripting environment that you can use to control and automate the deployment and management of your workloads in Azure.</span></span> <span data-ttu-id="6a465-169">A munkaállomás Azure PowerShell futtatásához konfigurálásával kapcsolatos további információkért lásd: [telepítse és konfigurálja az Azure Powershellt](/powershell/azure/overview).</span><span class="sxs-lookup"><span data-stu-id="6a465-169">For information about configuring your workstation to run Azure PowerShell, see [Install and configure Azure PowerShell](/powershell/azure/overview).</span></span>

[!INCLUDE [use-latest-version](../../includes/hdinsight-use-latest-powershell.md)]

<span data-ttu-id="6a465-170">**Az Azure Blob storage egy helyi fájl feltöltése**</span><span class="sxs-lookup"><span data-stu-id="6a465-170">**To upload a local file to Azure Blob storage**</span></span>

1. <span data-ttu-id="6a465-171">Nyissa meg az Azure PowerShell-konzolt, útmutatását [telepítse és konfigurálja az Azure Powershellt](/powershell/azure/overview).</span><span class="sxs-lookup"><span data-stu-id="6a465-171">Open the Azure PowerShell console as instructed in [Install and configure Azure PowerShell](/powershell/azure/overview).</span></span>
2. <span data-ttu-id="6a465-172">Az első öt változók értékeinek beállítása a következő parancsfájlban:</span><span class="sxs-lookup"><span data-stu-id="6a465-172">Set the values of the first five variables in the following script:</span></span>

        $resourceGroupName = "<AzureResourceGroupName>"
        $storageAccountName = "<StorageAccountName>"
        $containerName = "<ContainerName>"

        $fileName ="<LocalFileName>"
        $blobName = "<BlobName>"

        # Get the storage account key
        $storageAccountKey = (Get-AzureRmStorageAccountKey -ResourceGroupName $resourceGroupName -Name $storageAccountName)[0].Value
        # Create the storage context object
        $destContext = New-AzureStorageContext -StorageAccountName $storageAccountName -StorageAccountKey $storageaccountkey

        # Copy the file from local workstation to the Blob container
        Set-AzureStorageBlobContent -File $fileName -Container $containerName -Blob $blobName -context $destContext
3. <span data-ttu-id="6a465-173">Illessze be a parancsfájl futtatásához, a fájl másolása az Azure PowerShell-konzolon.</span><span class="sxs-lookup"><span data-stu-id="6a465-173">Paste the script into the Azure PowerShell console to run it to copy the file.</span></span>

<span data-ttu-id="6a465-174">Például a PowerShell-parancsfájlok működéséhez a hdinsight eszközzel, lásd: [a HDInsight tools](https://github.com/blackmist/hdinsight-tools).</span><span class="sxs-lookup"><span data-stu-id="6a465-174">For example PowerShell scripts created to work with HDInsight, see [HDInsight tools](https://github.com/blackmist/hdinsight-tools).</span></span>

### <span data-ttu-id="6a465-175"><a id="azcopy"></a>AzCopy</span><span class="sxs-lookup"><span data-stu-id="6a465-175"><a id="azcopy"></a>AzCopy</span></span>
<span data-ttu-id="6a465-176">AzCopy parancssori eszköz, amely arra tervezték, hogy egyszerűbbé tehető adatátvitel esetében bejövő és kimenő egy Azure Storage-fiók.</span><span class="sxs-lookup"><span data-stu-id="6a465-176">AzCopy is a command-line tool that is designed to simplify the task of transferring data into and out of an Azure Storage account.</span></span> <span data-ttu-id="6a465-177">Önálló eszközként használható, vagy az eszköz már meglévő alkalmazás tartalmaznia.</span><span class="sxs-lookup"><span data-stu-id="6a465-177">You can use it as a standalone tool or incorporate this tool in an existing application.</span></span> <span data-ttu-id="6a465-178">[Töltse le az AzCopy][azure-azcopy-download].</span><span class="sxs-lookup"><span data-stu-id="6a465-178">[Download AzCopy][azure-azcopy-download].</span></span>

<span data-ttu-id="6a465-179">Az AzCopy szintaxisa:</span><span class="sxs-lookup"><span data-stu-id="6a465-179">The AzCopy syntax is:</span></span>

    AzCopy <Source> <Destination> [filePattern [filePattern...]] [Options]

<span data-ttu-id="6a465-180">További információkért lásd: [AzCopy - feltöltése/letöltése fájlok Azure Blobokra vonatkozó][azure-azcopy].</span><span class="sxs-lookup"><span data-stu-id="6a465-180">For more information, see [AzCopy - Uploading/Downloading files for Azure Blobs][azure-azcopy].</span></span>

### <span data-ttu-id="6a465-181"><a id="commandline"></a>Hadoop parancssor</span><span class="sxs-lookup"><span data-stu-id="6a465-181"><a id="commandline"></a>Hadoop command line</span></span>
<span data-ttu-id="6a465-182">A Hadoop parancssora csak hasznos, amikor az adatok már megtalálható az átjárócsomóponthoz adatok tárolásához a blob-tárolóba.</span><span class="sxs-lookup"><span data-stu-id="6a465-182">The Hadoop command line is only useful for storing data into blob storage when the data is already present on the cluster head node.</span></span>

<span data-ttu-id="6a465-183">A Hadoop parancs használatához először csatlakoznia kell a headnode, az alábbi módszerek egyikének használatával:</span><span class="sxs-lookup"><span data-stu-id="6a465-183">In order to use the Hadoop command, you must first connect to the headnode using one of the following methods:</span></span>

* <span data-ttu-id="6a465-184">**Windows-alapú HDInsight**: [csatlakozzon a távoli asztali kapcsolattal](hdinsight-administer-use-management-portal.md#connect-to-clusters-using-rdp)</span><span class="sxs-lookup"><span data-stu-id="6a465-184">**Windows-based HDInsight**: [Connect using Remote Desktop](hdinsight-administer-use-management-portal.md#connect-to-clusters-using-rdp)</span></span>
* <span data-ttu-id="6a465-185">**Linux-alapú HDInsight**: SSH protokoll használatával kapcsolódó levelezőprogramokkal ([az SSH-parancstól](hdinsight-hadoop-linux-use-ssh-unix.md) vagy [PuTTY](hdinsight-hadoop-linux-use-ssh-windows.md))</span><span class="sxs-lookup"><span data-stu-id="6a465-185">**Linux-based HDInsight**: Connect using SSH ([the SSH command](hdinsight-hadoop-linux-use-ssh-unix.md) or [PuTTY](hdinsight-hadoop-linux-use-ssh-windows.md))</span></span>

<span data-ttu-id="6a465-186">A csatlakozás után a következő szintaxissal fájl feltöltése tárolás.</span><span class="sxs-lookup"><span data-stu-id="6a465-186">Once connected, you can use the following syntax to upload a file to storage.</span></span>

    hadoop -copyFromLocal <localFilePath> <storageFilePath>

<span data-ttu-id="6a465-187">Például: `hadoop fs -copyFromLocal data.txt /example/data/data.txt`</span><span class="sxs-lookup"><span data-stu-id="6a465-187">For example, `hadoop fs -copyFromLocal data.txt /example/data/data.txt`</span></span>

<span data-ttu-id="6a465-188">Mivel az alapértelmezett fájlrendszer a HDInsight az Azure Blob Storage tárolóban, /example/data.txt valójában az Azure Blob Storage tárolóban.</span><span class="sxs-lookup"><span data-stu-id="6a465-188">Because the default file system for HDInsight is in Azure Blob storage, /example/data.txt is actually in Azure Blob storage.</span></span> <span data-ttu-id="6a465-189">A fájlt is vonatkozik:</span><span class="sxs-lookup"><span data-stu-id="6a465-189">You can also refer to the file as:</span></span>

    wasb:///example/data/data.txt

<span data-ttu-id="6a465-190">vagy</span><span class="sxs-lookup"><span data-stu-id="6a465-190">or</span></span>

    wasb://<ContainerName>@<StorageAccountName>.blob.core.windows.net/example/data/davinci.txt

<span data-ttu-id="6a465-191">Más Hadoop listáját, hogy a munkahelyi fájlok parancsok, lásd: [http://hadoop.apache.org/docs/r2.7.0/hadoop-project-dist/hadoop-common/FileSystemShell.html](http://hadoop.apache.org/docs/r2.7.0/hadoop-project-dist/hadoop-common/FileSystemShell.html)</span><span class="sxs-lookup"><span data-stu-id="6a465-191">For a list of other Hadoop commands that work with files, see [http://hadoop.apache.org/docs/r2.7.0/hadoop-project-dist/hadoop-common/FileSystemShell.html](http://hadoop.apache.org/docs/r2.7.0/hadoop-project-dist/hadoop-common/FileSystemShell.html)</span></span>

> [!WARNING]
> <span data-ttu-id="6a465-192">A HBase fürtök esetén az alapértelmezett blokkméret használható, ha a adatainak írása 256KB.</span><span class="sxs-lookup"><span data-stu-id="6a465-192">On HBase clusters, the default block size used when writing data is 256KB.</span></span> <span data-ttu-id="6a465-193">Amíg ez a HBase API-k és a REST API-k használata remekül működik, használja a `hadoop` vagy `hdfs dfs` parancsok írása ~ 12 GB-nál nagyobb adatok hibát eredményez.</span><span class="sxs-lookup"><span data-stu-id="6a465-193">While this works fine when using HBase APIs or REST APIs, using the `hadoop` or `hdfs dfs` commands to write data larger than ~12GB results in an error.</span></span> <span data-ttu-id="6a465-194">Tekintse meg a [írás a blob storage-hibát](#storageexception) szakasz alatt további információt.</span><span class="sxs-lookup"><span data-stu-id="6a465-194">See the [storage exception for write on blob](#storageexception) section below for more information.</span></span>
>
>

## <a name="graphical-clients"></a><span data-ttu-id="6a465-195">Grafikus ügyfelek</span><span class="sxs-lookup"><span data-stu-id="6a465-195">Graphical clients</span></span>
<span data-ttu-id="6a465-196">Van még több alkalmazás, amely a grafikus felületet biztosít az Azure Storage.</span><span class="sxs-lookup"><span data-stu-id="6a465-196">There are also several applications that provide a graphical interface for working with Azure Storage.</span></span> <span data-ttu-id="6a465-197">A következő néhány olyan ezek az alkalmazások listája:</span><span class="sxs-lookup"><span data-stu-id="6a465-197">The following is a list of a few of these applications:</span></span>

| <span data-ttu-id="6a465-198">Ügyfél</span><span class="sxs-lookup"><span data-stu-id="6a465-198">Client</span></span> | <span data-ttu-id="6a465-199">Linux</span><span class="sxs-lookup"><span data-stu-id="6a465-199">Linux</span></span> | <span data-ttu-id="6a465-200">OS X</span><span class="sxs-lookup"><span data-stu-id="6a465-200">OS X</span></span> | <span data-ttu-id="6a465-201">Windows</span><span class="sxs-lookup"><span data-stu-id="6a465-201">Windows</span></span> |
| --- |:---:|:---:|:---:|
| [<span data-ttu-id="6a465-202">A Microsoft Visual Studio Tools for HDInsight</span><span class="sxs-lookup"><span data-stu-id="6a465-202">Microsoft Visual Studio Tools for HDInsight</span></span>](hdinsight-hadoop-visual-studio-tools-get-started.md#navigate-the-linked-resources) |<span data-ttu-id="6a465-203">✔</span><span class="sxs-lookup"><span data-stu-id="6a465-203">✔</span></span> |<span data-ttu-id="6a465-204">✔</span><span class="sxs-lookup"><span data-stu-id="6a465-204">✔</span></span> |<span data-ttu-id="6a465-205">✔</span><span class="sxs-lookup"><span data-stu-id="6a465-205">✔</span></span> |
| [<span data-ttu-id="6a465-206">Azure Storage Explorer</span><span class="sxs-lookup"><span data-stu-id="6a465-206">Azure Storage Explorer</span></span>](http://storageexplorer.com/) |<span data-ttu-id="6a465-207">✔</span><span class="sxs-lookup"><span data-stu-id="6a465-207">✔</span></span> |<span data-ttu-id="6a465-208">✔</span><span class="sxs-lookup"><span data-stu-id="6a465-208">✔</span></span> |<span data-ttu-id="6a465-209">✔</span><span class="sxs-lookup"><span data-stu-id="6a465-209">✔</span></span> |
| [<span data-ttu-id="6a465-210">Felhőalapú tárolás Studio 2</span><span class="sxs-lookup"><span data-stu-id="6a465-210">Cloud Storage Studio 2</span></span>](http://www.cerebrata.com/Products/CloudStorageStudio/) | | |<span data-ttu-id="6a465-211">✔</span><span class="sxs-lookup"><span data-stu-id="6a465-211">✔</span></span> |
| [<span data-ttu-id="6a465-212">CloudXplorer</span><span class="sxs-lookup"><span data-stu-id="6a465-212">CloudXplorer</span></span>](http://clumsyleaf.com/products/cloudxplorer) | | |<span data-ttu-id="6a465-213">✔</span><span class="sxs-lookup"><span data-stu-id="6a465-213">✔</span></span> |
| [<span data-ttu-id="6a465-214">Az Azure Explorer</span><span class="sxs-lookup"><span data-stu-id="6a465-214">Azure Explorer</span></span>](http://www.cloudberrylab.com/free-microsoft-azure-explorer.aspx) | | |<span data-ttu-id="6a465-215">✔</span><span class="sxs-lookup"><span data-stu-id="6a465-215">✔</span></span> |
| [<span data-ttu-id="6a465-216">Cyberduck</span><span class="sxs-lookup"><span data-stu-id="6a465-216">Cyberduck</span></span>](https://cyberduck.io/) | |<span data-ttu-id="6a465-217">✔</span><span class="sxs-lookup"><span data-stu-id="6a465-217">✔</span></span> |<span data-ttu-id="6a465-218">✔</span><span class="sxs-lookup"><span data-stu-id="6a465-218">✔</span></span> |

### <a name="visual-studio-tools-for-hdinsight"></a><span data-ttu-id="6a465-219">HDInsight Visual Studio eszközök</span><span class="sxs-lookup"><span data-stu-id="6a465-219">Visual Studio Tools for HDInsight</span></span>
<span data-ttu-id="6a465-220">További információkért lásd: [Navigálás a kapcsolt erőforrásokban](hdinsight-hadoop-visual-studio-tools-get-started.md#navigate-the-linked-resources).</span><span class="sxs-lookup"><span data-stu-id="6a465-220">For more information, see [Navigate the linked resources](hdinsight-hadoop-visual-studio-tools-get-started.md#navigate-the-linked-resources).</span></span>

### <span data-ttu-id="6a465-221"><a id="storageexplorer"></a>Az Azure Storage Explorer</span><span class="sxs-lookup"><span data-stu-id="6a465-221"><a id="storageexplorer"></a>Azure Storage Explorer</span></span>
<span data-ttu-id="6a465-222">*Az Azure Tártallózó* hasznos eszköz a Kérem, és módosítsa az adatokat a BLOB.</span><span class="sxs-lookup"><span data-stu-id="6a465-222">*Azure Storage Explorer* is a useful tool for inspecting and altering the data in blobs.</span></span> <span data-ttu-id="6a465-223">Ingyenes, nyissa meg a forrás eszköz, amely letölthető [http://storageexplorer.com/](http://storageexplorer.com/).</span><span class="sxs-lookup"><span data-stu-id="6a465-223">It is a free, open source tool that can be downloaded from [http://storageexplorer.com/](http://storageexplorer.com/).</span></span> <span data-ttu-id="6a465-224">A forráskódot, valamint a hivatkozás érhető el.</span><span class="sxs-lookup"><span data-stu-id="6a465-224">The source code is available from this link as well.</span></span>

<span data-ttu-id="6a465-225">Az eszköz használata előtt ismernie kell az Azure storage-fiók nevét és a fiók kulcs.</span><span class="sxs-lookup"><span data-stu-id="6a465-225">Before using the tool, you must know your Azure storage account name and account key.</span></span> <span data-ttu-id="6a465-226">Az alábbi információk eljussanak vonatkozó utasításokért lásd: a "hogyan: megtekintése, másolása és újragenerálása tárolási hívóbetűk" szakasza [létrehozása, kezelése vagy törlése a tárfiók][azure-create-storage-account].</span><span class="sxs-lookup"><span data-stu-id="6a465-226">For instructions about getting this information, see the "How to: View, copy and regenerate storage access keys" section of [Create, manage, or delete a storage account][azure-create-storage-account].</span></span>

1. <span data-ttu-id="6a465-227">Az Azure Storage-kezelő futtatása.</span><span class="sxs-lookup"><span data-stu-id="6a465-227">Run Azure Storage Explorer.</span></span> <span data-ttu-id="6a465-228">Ha első alkalommal Tártallózó futtatása, akkor a rendszer bekéri a a **tá_rolási fióknév** és **Tárfiók kulcsa**.</span><span class="sxs-lookup"><span data-stu-id="6a465-228">If this is the first time you have run the Storage Explorer, you will be prompted for the **_Storage account name** and **Storage account key**.</span></span> <span data-ttu-id="6a465-229">Futtatásakor az előtt, használja a **Hozzáadás** gombra kattintva adja hozzá egy új tárfiók neve és kulcsa.</span><span class="sxs-lookup"><span data-stu-id="6a465-229">If you have run it before, use the **Add** button to add a new storage account name and key.</span></span>

    <span data-ttu-id="6a465-230">Adja meg a nevét, és a tárfiók a HDInsight-fürt által használt kulcs, és válassza ki **MENTÉS és MEGNYITÁS**.</span><span class="sxs-lookup"><span data-stu-id="6a465-230">Enter the name and key for the storage account used by your HDInsight cluster and then select **SAVE & OPEN**.</span></span>

    ![HDI. AzureStorageExplorer][image-azure-storage-explorer]
2. <span data-ttu-id="6a465-232">A bal oldali interfész tárolók listájáról kattintson a tárolót a HDInsight-fürthöz társított nevét.</span><span class="sxs-lookup"><span data-stu-id="6a465-232">In the list of containers to the left of the interface, click the name of the container that is associated with your HDInsight cluster.</span></span> <span data-ttu-id="6a465-233">Alapértelmezés szerint ez a HDInsight-fürt neve, de eltérő, ha a fürt létrehozásakor adja meg az adott név lehet.</span><span class="sxs-lookup"><span data-stu-id="6a465-233">By default, this is the name of the HDInsight cluster, but may be different if you entered a specific name when creating the cluster.</span></span>
3. <span data-ttu-id="6a465-234">Az eszköztáron válassza ki a Feltöltés ikonra.</span><span class="sxs-lookup"><span data-stu-id="6a465-234">From the tool bar, select the upload icon.</span></span>

    ![A feltöltés ikonja kiemelve eszköztár](./media/hdinsight-upload-data/toolbar.png)
4. <span data-ttu-id="6a465-236">Adjon meg egy fájlt feltölteni, majd **nyitott**.</span><span class="sxs-lookup"><span data-stu-id="6a465-236">Specify a file to upload, and then click **Open**.</span></span> <span data-ttu-id="6a465-237">Amikor a rendszer kéri, válassza ki a **feltöltése** feltölteni a fájlt a tároló gyökeréhez.</span><span class="sxs-lookup"><span data-stu-id="6a465-237">When prompted, select **Upload** to upload the file to the root of the storage container.</span></span> <span data-ttu-id="6a465-238">Ha szeretne feltölteni a fájlt egy adott helyre, adja meg az elérési utat a **cél** mezőbe, majd válassza ki **feltöltése**.</span><span class="sxs-lookup"><span data-stu-id="6a465-238">If you want to upload the file to a specific path, enter the path in the **Destination** field and then select **Upload**.</span></span>

    ![Fájl feltöltése párbeszédpanel](./media/hdinsight-upload-data/fileupload.png)

    <span data-ttu-id="6a465-240">Ha a fájl befejezte a feltöltést, használhatja a HDInsight-fürt feladatokból.</span><span class="sxs-lookup"><span data-stu-id="6a465-240">Once the file has finished uploading, you can use it from jobs on the HDInsight cluster.</span></span>

## <a name="mount-azure-blob-storage-as-local-drive"></a><span data-ttu-id="6a465-241">Helyi meghajtó csatlakoztatási Azure Blob Storage tárolót</span><span class="sxs-lookup"><span data-stu-id="6a465-241">Mount Azure Blob Storage as Local Drive</span></span>
<span data-ttu-id="6a465-242">Lásd: [csatlakoztatási Azure Blob Storage tárolót helyi meghajtó](http://blogs.msdn.com/b/bigdatasupport/archive/2014/01/09/mount-azure-blob-storage-as-local-drive.aspx).</span><span class="sxs-lookup"><span data-stu-id="6a465-242">See [Mount Azure Blob Storage as Local Drive](http://blogs.msdn.com/b/bigdatasupport/archive/2014/01/09/mount-azure-blob-storage-as-local-drive.aspx).</span></span>

## <a name="services"></a><span data-ttu-id="6a465-243">Szolgáltatások</span><span class="sxs-lookup"><span data-stu-id="6a465-243">Services</span></span>
### <a name="azure-data-factory"></a><span data-ttu-id="6a465-244">Azure Data Factory</span><span class="sxs-lookup"><span data-stu-id="6a465-244">Azure Data Factory</span></span>
<span data-ttu-id="6a465-245">Az Azure Data Factory szolgáltatásnak egy teljes körűen felügyelt szolgáltatás zökkenőmentes, méretezhető és megbízható éles adatcsatornák a tárolás, az adatfeldolgozás és adatok adatátviteli szolgáltatások létrehozására.</span><span class="sxs-lookup"><span data-stu-id="6a465-245">The Azure Data Factory service is a fully managed service for composing data storage, data processing, and data movement services into streamlined, scalable, and reliable data production pipelines.</span></span>

<span data-ttu-id="6a465-246">Az Azure Data Factory áthelyezni az adatokat az Azure Blob Storage tárolóban, vagy hozzon létre az adatok folyamatok, amelyek közvetlenül a HDInsight-szolgáltatások, mint a Hive és a Pig használható.</span><span class="sxs-lookup"><span data-stu-id="6a465-246">Azure Data Factory can be used to move data into Azure Blob storage, or to create data pipelines that directly use HDInsight features such as Hive and Pig.</span></span>

<span data-ttu-id="6a465-247">További információkért lásd: a [Azure Data Factory dokumentáció](https://azure.microsoft.com/documentation/services/data-factory/).</span><span class="sxs-lookup"><span data-stu-id="6a465-247">For more information, see the [Azure Data Factory documentation](https://azure.microsoft.com/documentation/services/data-factory/).</span></span>

### <span data-ttu-id="6a465-248"><a id="sqoop"></a>Apache Sqoop</span><span class="sxs-lookup"><span data-stu-id="6a465-248"><a id="sqoop"></a>Apache Sqoop</span></span>
<span data-ttu-id="6a465-249">Sqoop az eszköz a Hadoop és a relációs adatbázisok közötti adattovábbításra.</span><span class="sxs-lookup"><span data-stu-id="6a465-249">Sqoop is a tool designed to transfer data between Hadoop and relational databases.</span></span> <span data-ttu-id="6a465-250">Adatok importálása egy relációs adatbázis-kezelő rendszerének (RDBMS), például az SQL Server, MySQL, vagy a Hadoop elosztott fájlrendszer (HDFS), az Oracle átalakíthatja az adatokat a Hadoop MapReduce vagy a Hive, majd az adatok exportálása vissza egy RDBMS használhatja.</span><span class="sxs-lookup"><span data-stu-id="6a465-250">You can use it to import data from a relational database management system (RDBMS), such as SQL Server, MySQL, or Oracle into the Hadoop distributed file system (HDFS), transform the data in Hadoop with MapReduce or Hive, and then export the data back into an RDBMS.</span></span>

<span data-ttu-id="6a465-251">További információkért lásd: [és a HDInsight együttes használata Sqoop][hdinsight-use-sqoop].</span><span class="sxs-lookup"><span data-stu-id="6a465-251">For more information, see [Use Sqoop with HDInsight][hdinsight-use-sqoop].</span></span>

## <a name="development-sdks"></a><span data-ttu-id="6a465-252">Fejlesztési SDK-k</span><span class="sxs-lookup"><span data-stu-id="6a465-252">Development SDKs</span></span>
<span data-ttu-id="6a465-253">Az Azure Blob storage használata az Azure SDK-t a következő programozási nyelven is elérhető:</span><span class="sxs-lookup"><span data-stu-id="6a465-253">Azure Blob storage can also be accessed using an Azure SDK from the following programming languages:</span></span>

* <span data-ttu-id="6a465-254">.NET</span><span class="sxs-lookup"><span data-stu-id="6a465-254">.NET</span></span>
* <span data-ttu-id="6a465-255">Java</span><span class="sxs-lookup"><span data-stu-id="6a465-255">Java</span></span>
* <span data-ttu-id="6a465-256">Node.js</span><span class="sxs-lookup"><span data-stu-id="6a465-256">Node.js</span></span>
* <span data-ttu-id="6a465-257">PHP</span><span class="sxs-lookup"><span data-stu-id="6a465-257">PHP</span></span>
* <span data-ttu-id="6a465-258">Python</span><span class="sxs-lookup"><span data-stu-id="6a465-258">Python</span></span>
* <span data-ttu-id="6a465-259">Ruby</span><span class="sxs-lookup"><span data-stu-id="6a465-259">Ruby</span></span>

<span data-ttu-id="6a465-260">Az Azure SDK-k telepítésével kapcsolatos további információkért lásd: [Azure tölti le.](https://azure.microsoft.com/downloads/)</span><span class="sxs-lookup"><span data-stu-id="6a465-260">For more information on installing the Azure SDKs, see [Azure downloads](https://azure.microsoft.com/downloads/)</span></span>

## <a name="troubleshooting"></a><span data-ttu-id="6a465-261">Hibaelhárítás</span><span class="sxs-lookup"><span data-stu-id="6a465-261">Troubleshooting</span></span>
### <span data-ttu-id="6a465-262"><a id="storageexception"></a>Írás a blob Storage-hibát</span><span class="sxs-lookup"><span data-stu-id="6a465-262"><a id="storageexception"></a>Storage exception for write on blob</span></span>
<span data-ttu-id="6a465-263">**A jelenség**: használata esetén a `hadoop` vagy `hdfs dfs` parancsok fájloknak, amelyek ~ 12 GB vagy nagyobb a HBase-fürtöt, felmerülhet a következő hiba:</span><span class="sxs-lookup"><span data-stu-id="6a465-263">**Symptoms**: When using the `hadoop` or `hdfs dfs` commands to write files that are ~12GB or larger on an HBase cluster, you may encounter the following error:</span></span>

    ERROR azure.NativeAzureFileSystem: Encountered Storage Exception for write on Blob : example/test_large_file.bin._COPYING_ Exception details: null Error Code : RequestBodyTooLarge
    copyFromLocal: java.io.IOException
            at com.microsoft.azure.storage.core.Utility.initIOException(Utility.java:661)
            at com.microsoft.azure.storage.blob.BlobOutputStream$1.call(BlobOutputStream.java:366)
            at com.microsoft.azure.storage.blob.BlobOutputStream$1.call(BlobOutputStream.java:350)
            at java.util.concurrent.FutureTask.run(FutureTask.java:262)
            at java.util.concurrent.Executors$RunnableAdapter.call(Executors.java:471)
            at java.util.concurrent.FutureTask.run(FutureTask.java:262)
            at java.util.concurrent.ThreadPoolExecutor.runWorker(ThreadPoolExecutor.java:1145)
            at java.util.concurrent.ThreadPoolExecutor$Worker.run(ThreadPoolExecutor.java:615)
            at java.lang.Thread.run(Thread.java:745)
    Caused by: com.microsoft.azure.storage.StorageException: The request body is too large and exceeds the maximum permissible limit.
            at com.microsoft.azure.storage.StorageException.translateException(StorageException.java:89)
            at com.microsoft.azure.storage.core.StorageRequest.materializeException(StorageRequest.java:307)
            at com.microsoft.azure.storage.core.ExecutionEngine.executeWithRetry(ExecutionEngine.java:182)
            at com.microsoft.azure.storage.blob.CloudBlockBlob.uploadBlockInternal(CloudBlockBlob.java:816)
            at com.microsoft.azure.storage.blob.CloudBlockBlob.uploadBlock(CloudBlockBlob.java:788)
            at com.microsoft.azure.storage.blob.BlobOutputStream$1.call(BlobOutputStream.java:354)
            ... 7 more

<span data-ttu-id="6a465-264">**OK**: a HBase a HDInsight-fürtök alapértelmezett egy 256 KB-os blokkméretet az Azure storage írása közben.</span><span class="sxs-lookup"><span data-stu-id="6a465-264">**Cause**: HBase on HDInsight clusters default to a block size of 256KB when writing to Azure storage.</span></span> <span data-ttu-id="6a465-265">Amíg ez működik, a HBase API-k vagy a REST API-k, okoz hiba használata esetén a `hadoop` vagy `hdfs dfs` parancssori segédprogramok.</span><span class="sxs-lookup"><span data-stu-id="6a465-265">While this works for HBase APIs or REST APIs, it will result in an error when using the `hadoop` or `hdfs dfs` command-line utilities.</span></span>

<span data-ttu-id="6a465-266">**Megoldási**: használata `fs.azure.write.request.size` adhatja meg a nagyobb blokkméret.</span><span class="sxs-lookup"><span data-stu-id="6a465-266">**Resolution**: Use `fs.azure.write.request.size` to specify a larger block size.</span></span> <span data-ttu-id="6a465-267">Ehhez a használati alapon használatával a `-D` paraméter.</span><span class="sxs-lookup"><span data-stu-id="6a465-267">You can do this on a per-use basis by using the `-D` parameter.</span></span> <span data-ttu-id="6a465-268">Az alábbiakban egy példa segítségével ezt a paramétert a `hadoop` parancs:</span><span class="sxs-lookup"><span data-stu-id="6a465-268">The following is an example using this parameter with the `hadoop` command:</span></span>

    hadoop -fs -D fs.azure.write.request.size=4194304 -copyFromLocal test_large_file.bin /example/data

<span data-ttu-id="6a465-269">Növelje a értékének `fs.azure.write.request.size` globálisan Ambari használatával.</span><span class="sxs-lookup"><span data-stu-id="6a465-269">You can also increase the value of `fs.azure.write.request.size` globally by using Ambari.</span></span> <span data-ttu-id="6a465-270">Az alábbi lépések segítségével módosítsa az értéket a Ambari webes felhasználói felületén:</span><span class="sxs-lookup"><span data-stu-id="6a465-270">The following steps can be used to change the value in the Ambari Web UI:</span></span>

1. <span data-ttu-id="6a465-271">A böngészőben nyissa meg az Ambari webes felhasználói felületén, a fürt számára.</span><span class="sxs-lookup"><span data-stu-id="6a465-271">In your browser, go to the Ambari Web UI for your cluster.</span></span> <span data-ttu-id="6a465-272">Ez a https://CLUSTERNAME.azurehdinsight.net, ahol **CLUSTERNAME** a fürt neve.</span><span class="sxs-lookup"><span data-stu-id="6a465-272">This is https://CLUSTERNAME.azurehdinsight.net, where **CLUSTERNAME** is the name of your cluster.</span></span>

    <span data-ttu-id="6a465-273">Amikor a rendszer kéri, adja meg a rendszergazda nevét és jelszavát a fürt.</span><span class="sxs-lookup"><span data-stu-id="6a465-273">When prompted, enter the admin name and password for the cluster.</span></span>
2. <span data-ttu-id="6a465-274">Válassza ki a képernyő bal oldalán, **HDFS**, majd válassza ki a **Configs** fülre.</span><span class="sxs-lookup"><span data-stu-id="6a465-274">From the left side of the screen, select **HDFS**, and then select the **Configs** tab.</span></span>
3. <span data-ttu-id="6a465-275">Az a **szűrő...**  mezőbe írja be `fs.azure.write.request.size`.</span><span class="sxs-lookup"><span data-stu-id="6a465-275">In the **Filter...** field, enter `fs.azure.write.request.size`.</span></span> <span data-ttu-id="6a465-276">Ez a mező és az aktuális érték közepén a lap megjeleníti.</span><span class="sxs-lookup"><span data-stu-id="6a465-276">This will display the field and current value in the middle of the page.</span></span>
4. <span data-ttu-id="6a465-277">Módosítsa az értéket a 262144 (256KB) az új érték.</span><span class="sxs-lookup"><span data-stu-id="6a465-277">Change the value from 262144 (256KB) to the new value.</span></span> <span data-ttu-id="6a465-278">Például 4194304 (4MB).</span><span class="sxs-lookup"><span data-stu-id="6a465-278">For example, 4194304 (4MB).</span></span>

![Az Ambari webes felhasználói felületén keresztül érték módosítása képe](./media/hdinsight-upload-data/hbase-change-block-write-size.png)

<span data-ttu-id="6a465-280">További információ az Ambari használatával, lásd: [kezelése HDInsight-fürtök az Ambari webes felhasználói felület használatával](hdinsight-hadoop-manage-ambari.md).</span><span class="sxs-lookup"><span data-stu-id="6a465-280">For more information on using Ambari, see [Manage HDInsight clusters using the Ambari Web UI](hdinsight-hadoop-manage-ambari.md).</span></span>

## <a name="next-steps"></a><span data-ttu-id="6a465-281">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="6a465-281">Next steps</span></span>
<span data-ttu-id="6a465-282">Most, hogy megismerkedett az adatok lekérése a HDInsight, olvassa el az elemzést további információt a következő cikkeket:</span><span class="sxs-lookup"><span data-stu-id="6a465-282">Now that you understand how to get data into HDInsight, read the following articles to learn how to perform analysis:</span></span>

* <span data-ttu-id="6a465-283">[Azure HDInsight – első lépések][hdinsight-get-started]</span><span class="sxs-lookup"><span data-stu-id="6a465-283">[Get started with Azure HDInsight][hdinsight-get-started]</span></span>
* <span data-ttu-id="6a465-284">[Hadoop-feladatokat programozott módon küldhet][hdinsight-submit-jobs]</span><span class="sxs-lookup"><span data-stu-id="6a465-284">[Submit Hadoop jobs programmatically][hdinsight-submit-jobs]</span></span>
* <span data-ttu-id="6a465-285">[A Hive használata a HDInsightban][hdinsight-use-hive]</span><span class="sxs-lookup"><span data-stu-id="6a465-285">[Use Hive with HDInsight][hdinsight-use-hive]</span></span>
* <span data-ttu-id="6a465-286">[A Pig használata a HDInsightban][hdinsight-use-pig]</span><span class="sxs-lookup"><span data-stu-id="6a465-286">[Use Pig with HDInsight][hdinsight-use-pig]</span></span>

[azure-management-portal]: https://porta.azure.com
[azure-powershell]: http://msdn.microsoft.com/library/windowsazure/jj152841.aspx

[azure-storage-client-library]: /develop/net/how-to-guides/blob-storage/
[azure-create-storage-account]:../storage/common/storage-create-storage-account.md
[azure-azcopy-download]:../storage/common/storage-use-azcopy.md
[azure-azcopy]:../storage/common/storage-use-azcopy.md

[hdinsight-use-sqoop]: hdinsight-use-sqoop.md

[hdinsight-storage]: hdinsight-hadoop-use-blob-storage.md
[hdinsight-submit-jobs]: hdinsight-submit-hadoop-jobs-programmatically.md
[hdinsight-get-started]: hdinsight-hadoop-linux-tutorial-get-started.md

[hdinsight-use-hive]: hdinsight-use-hive.md
[hdinsight-use-pig]: hdinsight-use-pig.md
[hdinsight-provision]: hdinsight-hadoop-provision-linux-clusters.md

[sqldatabase-create-configure]: ../sql-database-create-configure.md

[apache-sqoop-guide]: http://sqoop.apache.org/docs/1.4.4/SqoopUserGuide.html

[Powershell-install-configure]: /powershell/azureps-cmdlets-docs

[azurecli]: ../cli-install-nodejs.md


[image-azure-storage-explorer]: ./media/hdinsight-upload-data/HDI.AzureStorageExplorer.png
[image-ase-addaccount]: ./media/hdinsight-upload-data/HDI.ASEAddAccount.png
[image-ase-blob]: ./media/hdinsight-upload-data/HDI.ASEBlob.png
