---
title: hdinsight Hadoop-feladatokat aaaUpload adatainak |} Microsoft Docs
description: "Ismerje meg, hogyan tooupload és a hozzáférési adatok a Hadoop-feladatokat a Hdinsightban az Azure CLI-t, Azure Tártallózó, Azure PowerShell, hello Hadoop parancssori vagy Sqoop hello."
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
ms.openlocfilehash: 15da602085d41c19789e34800f3d9e238d7d1de8
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="upload-data-for-hadoop-jobs-in-hdinsight"></a><span data-ttu-id="43355-104">Hadoop-feladatok adatainak feltöltése a HDInsightba</span><span class="sxs-lookup"><span data-stu-id="43355-104">Upload data for Hadoop jobs in HDInsight</span></span>
<span data-ttu-id="43355-105">Az Azure HDInsight egy teljes körű Hadoop elosztott fájlrendszer (HDFS) biztosítanak Azure Blob Storage tárolóban.</span><span class="sxs-lookup"><span data-stu-id="43355-105">Azure HDInsight provides a full-featured Hadoop distributed file system (HDFS) over Azure Blob storage.</span></span> <span data-ttu-id="43355-106">Arra tervezték, mint HDFS bővítmény tooprovide zökkenőmentes toocustomers tapasztalhat.</span><span class="sxs-lookup"><span data-stu-id="43355-106">It is designed as an HDFS extension tooprovide a seamless experience toocustomers.</span></span> <span data-ttu-id="43355-107">Ez lehetővé teszi a hello hello Hadoop ökoszisztémájának toooperate közvetlenül adatokon hello kezeli az összetevők teljes készlete.</span><span class="sxs-lookup"><span data-stu-id="43355-107">It enables hello full set of components in hello Hadoop ecosystem toooperate directly on hello data it manages.</span></span> <span data-ttu-id="43355-108">Az Azure Blob storage és HDFS különböző fájlrendszerek, adatokat és a számítások az adatok tárolására vannak optimalizálva.</span><span class="sxs-lookup"><span data-stu-id="43355-108">Azure Blob storage and HDFS are distinct file systems that are optimized for storage of data and computations on that data.</span></span> <span data-ttu-id="43355-109">Hello előnyeit az Azure Blob storage használatával információ: [használata Azure Blob storage a hdinsight eszközzel][hdinsight-storage].</span><span class="sxs-lookup"><span data-stu-id="43355-109">For information about hello benefits of using Azure Blob storage, see [Use Azure Blob storage with HDInsight][hdinsight-storage].</span></span>

<span data-ttu-id="43355-110">**Előfeltételek**</span><span class="sxs-lookup"><span data-stu-id="43355-110">**Prerequisites**</span></span>

<span data-ttu-id="43355-111">Vegye figyelembe a következő követelmény, mielőtt elkezdené hello:</span><span class="sxs-lookup"><span data-stu-id="43355-111">Note hello following requirement before you begin:</span></span>

* <span data-ttu-id="43355-112">Egy Azure HDInsight-fürtre.</span><span class="sxs-lookup"><span data-stu-id="43355-112">An Azure HDInsight cluster.</span></span> <span data-ttu-id="43355-113">Útmutatásért lásd: [Ismerkedés az Azure HDInsight] [ hdinsight-get-started] vagy [Provision HDInsight clusters][hdinsight-provision].</span><span class="sxs-lookup"><span data-stu-id="43355-113">For instructions, see [Get started with Azure HDInsight][hdinsight-get-started] or [Provision HDInsight clusters][hdinsight-provision].</span></span>

## <a name="why-blob-storage"></a><span data-ttu-id="43355-114">Miért blob storage?</span><span class="sxs-lookup"><span data-stu-id="43355-114">Why blob storage?</span></span>
<span data-ttu-id="43355-115">Az Azure HDInsight fürtök jellemzően toorun MapReduce-feladatok telepítve, és ezek a feladatok befejezése után hello fürtök eldobott.</span><span class="sxs-lookup"><span data-stu-id="43355-115">Azure HDInsight clusters are typically deployed toorun MapReduce jobs, and hello clusters are dropped after these jobs complete.</span></span> <span data-ttu-id="43355-116">Gondoskodik a hello adatok hello HDFS fürtök számítások befejezését követően ezek az adatok lenne egy drága módon toostore.</span><span class="sxs-lookup"><span data-stu-id="43355-116">Keeping hello data in hello HDFS clusters after computations are complete would be an expensive way toostore this data.</span></span> <span data-ttu-id="43355-117">Az Azure Blob storage egy magas rendelkezésre állású, nagymértékben méretezhető, nagy kapacitás, a HDInsight eszközzel feldolgozott toobe adatok alacsony költségű, és megosztható tárolási lehetőség.</span><span class="sxs-lookup"><span data-stu-id="43355-117">Azure Blob storage is a highly available, highly scalable, high capacity, low cost, and shareable storage option for data that is toobe processed using HDInsight.</span></span> <span data-ttu-id="43355-118">Adattárolás egy blobba lehetővé teszi, hogy a hello HDInsight fürtöket a számítási toobe biztonságosan kiadott adatok elvesztése nélkül használt.</span><span class="sxs-lookup"><span data-stu-id="43355-118">Storing data in a blob enables hello HDInsight clusters that are used for computation toobe safely released without losing data.</span></span>

### <a name="directories"></a><span data-ttu-id="43355-119">Könyvtárak</span><span class="sxs-lookup"><span data-stu-id="43355-119">Directories</span></span>
<span data-ttu-id="43355-120">Az Azure Blob storage tárolók kulcs/érték párként adatok tárolásához, és nincs könyvtár-hierarchia.</span><span class="sxs-lookup"><span data-stu-id="43355-120">Azure Blob storage containers store data as key/value pairs, and there is no directory hierarchy.</span></span> <span data-ttu-id="43355-121">Azonban hello "/" karaktert használhatja hello kulcsnév toomake akkor jelenik meg, mintha a fájl könyvtárszerkezetben tárolódik.</span><span class="sxs-lookup"><span data-stu-id="43355-121">However hello "/" character can be used within hello key name toomake it appear as if a file is stored within a directory structure.</span></span> <span data-ttu-id="43355-122">HDInsight ezek látja, mintha tényleges könyvtárak is.</span><span class="sxs-lookup"><span data-stu-id="43355-122">HDInsight sees these as if they are actual directories.</span></span>

<span data-ttu-id="43355-123">Egy blob kulcsa lehet például az *input/log1.txt*.</span><span class="sxs-lookup"><span data-stu-id="43355-123">For example, a blob's key may be *input/log1.txt*.</span></span> <span data-ttu-id="43355-124">Nem tényleges "bemeneti" könyvtár létezik, de hello toohello jelenléte miatt "/" karakter hello kulcsnév van hello megjelenésének egy fájl elérési útját.</span><span class="sxs-lookup"><span data-stu-id="43355-124">No actual "input" directory exists, but due toohello presence of hello "/" character in hello key name, it has hello appearance of a file path.</span></span>

<span data-ttu-id="43355-125">Ebből kifolyólag Azure Explorer eszközök használatakor előfordulhat néhány 0 bájt fájlt.</span><span class="sxs-lookup"><span data-stu-id="43355-125">Because of this, if you use Azure Explorer tools you may notice some 0 byte files.</span></span> <span data-ttu-id="43355-126">Ezeket a fájlokat két célokra szolgál:</span><span class="sxs-lookup"><span data-stu-id="43355-126">These files serve two purposes:</span></span>

* <span data-ttu-id="43355-127">Ha üres mappák, azokat megjelölni hello létezés hello mappa.</span><span class="sxs-lookup"><span data-stu-id="43355-127">If there are empty folders, they mark of hello existence of hello folder.</span></span> <span data-ttu-id="43355-128">Az Azure Blob storage, amely egy blob PEL/sáv nevű létezik, nincs-e egy nevű mappát elég intelligens tooknow **PEL**.</span><span class="sxs-lookup"><span data-stu-id="43355-128">Azure Blob storage is clever enough tooknow that if a blob called foo/bar exists, there is a folder called **foo**.</span></span> <span data-ttu-id="43355-129">Azonban csak úgy toosignify egy üres mappa neve a hello **PEL** azzal, hogy ez a különleges 0 bájt fájl érvényben van.</span><span class="sxs-lookup"><span data-stu-id="43355-129">But hello only way toosignify an empty folder called **foo** is by having this special 0 byte file in place.</span></span>
* <span data-ttu-id="43355-130">Tartanak fenn speciális metaadatok szükséges hello Hadoop által fájlrendszer, nevezetesen hello engedélyek és hello mappák tulajdonosait.</span><span class="sxs-lookup"><span data-stu-id="43355-130">They hold special metadata that is needed by hello Hadoop file system, notably hello permissions and owners for hello folders.</span></span>

## <a name="command-line-utilities"></a><span data-ttu-id="43355-131">Parancssori segédprogramok</span><span class="sxs-lookup"><span data-stu-id="43355-131">Command-line utilities</span></span>
<span data-ttu-id="43355-132">A Microsoft hello segédprogramok toowork az Azure Blob storage a következő biztosít:</span><span class="sxs-lookup"><span data-stu-id="43355-132">Microsoft provides hello following utilities toowork with Azure Blob storage:</span></span>

| <span data-ttu-id="43355-133">Eszköz</span><span class="sxs-lookup"><span data-stu-id="43355-133">Tool</span></span> | <span data-ttu-id="43355-134">Linux</span><span class="sxs-lookup"><span data-stu-id="43355-134">Linux</span></span> | <span data-ttu-id="43355-135">OS X</span><span class="sxs-lookup"><span data-stu-id="43355-135">OS X</span></span> | <span data-ttu-id="43355-136">Windows</span><span class="sxs-lookup"><span data-stu-id="43355-136">Windows</span></span> |
| --- |:---:|:---:|:---:|
| <span data-ttu-id="43355-137">[Azure parancssori felület][azurecli]</span><span class="sxs-lookup"><span data-stu-id="43355-137">[Azure Command-Line Interface][azurecli]</span></span> |<span data-ttu-id="43355-138">✔</span><span class="sxs-lookup"><span data-stu-id="43355-138">✔</span></span> |<span data-ttu-id="43355-139">✔</span><span class="sxs-lookup"><span data-stu-id="43355-139">✔</span></span> |<span data-ttu-id="43355-140">✔</span><span class="sxs-lookup"><span data-stu-id="43355-140">✔</span></span> |
| <span data-ttu-id="43355-141">[Az Azure PowerShell][azure-powershell]</span><span class="sxs-lookup"><span data-stu-id="43355-141">[Azure PowerShell][azure-powershell]</span></span> | | |<span data-ttu-id="43355-142">✔</span><span class="sxs-lookup"><span data-stu-id="43355-142">✔</span></span> |
| <span data-ttu-id="43355-143">[AzCopy][azure-azcopy]</span><span class="sxs-lookup"><span data-stu-id="43355-143">[AzCopy][azure-azcopy]</span></span> | | |<span data-ttu-id="43355-144">✔</span><span class="sxs-lookup"><span data-stu-id="43355-144">✔</span></span> |
| [<span data-ttu-id="43355-145">Hadoop parancs</span><span class="sxs-lookup"><span data-stu-id="43355-145">Hadoop command</span></span>](#commandline) |<span data-ttu-id="43355-146">✔</span><span class="sxs-lookup"><span data-stu-id="43355-146">✔</span></span> |<span data-ttu-id="43355-147">✔</span><span class="sxs-lookup"><span data-stu-id="43355-147">✔</span></span> |<span data-ttu-id="43355-148">✔</span><span class="sxs-lookup"><span data-stu-id="43355-148">✔</span></span> |

> [!NOTE]
> <span data-ttu-id="43355-149">Külső Azure-ban Hadoop parancs csak érhető el hello HDInsight-fürtre, és csak az adatok betöltése a helyi fájlrendszer hello Azure Blob Storage tárolóban történő lehetővé hello közben hello Azure CLI-t, az Azure PowerShell és az AzCopy összes is használható.</span><span class="sxs-lookup"><span data-stu-id="43355-149">While hello Azure CLI, Azure PowerShell, and AzCopy can all be used from outside Azure, hello Hadoop command is only available on hello HDInsight cluster and only allows loading data from hello local file system into Azure Blob storage.</span></span>
>
>

### <span data-ttu-id="43355-150"><a id="xplatcli"></a>Az Azure parancssori felület</span><span class="sxs-lookup"><span data-stu-id="43355-150"><a id="xplatcli"></a>Azure CLI</span></span>
<span data-ttu-id="43355-151">hello Azure CLI az lehetővé teszi a toomanage Azure platformfüggetlen eszközzel szolgáltatások.</span><span class="sxs-lookup"><span data-stu-id="43355-151">hello Azure CLI is a cross-platform tool that allows you toomanage Azure services.</span></span> <span data-ttu-id="43355-152">A következő lépéseket tooupload adatok tooAzure Blob-tároló hello használata:</span><span class="sxs-lookup"><span data-stu-id="43355-152">Use hello following steps tooupload data tooAzure Blob storage:</span></span>

[!INCLUDE [use-latest-version](../../includes/hdinsight-use-latest-cli.md)]

1. <span data-ttu-id="43355-153">[Telepítse és konfigurálja a hello Azure parancssori felület Mac, Linux és Windows](../cli-install-nodejs.md).</span><span class="sxs-lookup"><span data-stu-id="43355-153">[Install and configure hello Azure CLI for Mac, Linux and Windows](../cli-install-nodejs.md).</span></span>
2. <span data-ttu-id="43355-154">Nyisson meg egy parancssort, bash vagy más rendszerhéj, és használja a következő tooauthenticate tooyour Azure-előfizetés hello.</span><span class="sxs-lookup"><span data-stu-id="43355-154">Open a command prompt, bash, or other shell, and use hello following tooauthenticate tooyour Azure subscription.</span></span>

        azure login

    <span data-ttu-id="43355-155">Amikor a rendszer kéri, adja meg hello felhasználónevet és jelszót az előfizetéséhez.</span><span class="sxs-lookup"><span data-stu-id="43355-155">When prompted, enter hello user name and password for your subscription.</span></span>
3. <span data-ttu-id="43355-156">Adja meg a következő parancs toolist hello tárfiókok az előfizetéshez tartozó hello:</span><span class="sxs-lookup"><span data-stu-id="43355-156">Enter hello following command toolist hello storage accounts for your subscription:</span></span>

        azure storage account list
4. <span data-ttu-id="43355-157">Válassza ki, amely tartalmazza azt szeretné, hogy a toowork hello blob hello tárfiókot, majd a következő parancs tooretrieve hello kulcs ehhez a fiókhoz hello használata:</span><span class="sxs-lookup"><span data-stu-id="43355-157">Select hello storage account that contains hello blob you want toowork with, then use hello following command tooretrieve hello key for this account:</span></span>

        azure storage account keys list <storage-account-name>

    <span data-ttu-id="43355-158">Ez visszaadja **elsődleges** és **másodlagos** kulcsok.</span><span class="sxs-lookup"><span data-stu-id="43355-158">This should return **Primary** and **Secondary** keys.</span></span> <span data-ttu-id="43355-159">Másolás hello **elsődleges** kulcs értékét, mert fogja használni a következő lépésekben hello.</span><span class="sxs-lookup"><span data-stu-id="43355-159">Copy hello **Primary** key value because it will be used in hello next steps.</span></span>
5. <span data-ttu-id="43355-160">A következő parancs tooretrieve hello tárfiókon belül blob tárolók listája hello használata:</span><span class="sxs-lookup"><span data-stu-id="43355-160">Use hello following command tooretrieve a list of blob containers within hello storage account:</span></span>

        azure storage container list -a <storage-account-name> -k <primary-key>
6. <span data-ttu-id="43355-161">A következő parancsok tooupload hello használja, és töltse le a fájlokat toohello blob:</span><span class="sxs-lookup"><span data-stu-id="43355-161">Use hello following commands tooupload and download files toohello blob:</span></span>

   * <span data-ttu-id="43355-162">a fájl tooupload:</span><span class="sxs-lookup"><span data-stu-id="43355-162">tooupload a file:</span></span>

           azure storage blob upload -a <storage-account-name> -k <primary-key> <source-file> <container-name> <blob-name>
   * <span data-ttu-id="43355-163">a fájl toodownload:</span><span class="sxs-lookup"><span data-stu-id="43355-163">toodownload a file:</span></span>

           azure storage blob download -a <storage-account-name> -k <primary-key> <container-name> <blob-name> <destination-file>

> [!NOTE]
> <span data-ttu-id="43355-164">Ha lesz mindig kell dolgozik hello azonos tárfiókot, beállíthatja a következő környezeti változók hello fiók megadása helyett hello és billentyűt minden parancs:</span><span class="sxs-lookup"><span data-stu-id="43355-164">If you will always be working with hello same storage account, you can set hello following environment variables instead of specifying hello account and key for every command:</span></span>
>
> * <span data-ttu-id="43355-165">**AZURE\_tárolási\_fiók**: hello tárfiók neve</span><span class="sxs-lookup"><span data-stu-id="43355-165">**AZURE\_STORAGE\_ACCOUNT**: hello storage account name</span></span>
> * <span data-ttu-id="43355-166">**AZURE\_tárolási\_hozzáférés\_kulcs**: hello tárfiók kulcsa</span><span class="sxs-lookup"><span data-stu-id="43355-166">**AZURE\_STORAGE\_ACCESS\_KEY**: hello storage account key</span></span>
>
>

### <span data-ttu-id="43355-167"><a id="powershell"></a>Az Azure PowerShell</span><span class="sxs-lookup"><span data-stu-id="43355-167"><a id="powershell"></a>Azure PowerShell</span></span>
<span data-ttu-id="43355-168">Az Azure PowerShell egy parancsfájl-kezelési környezet, hogy toocontrol használ, és hello telepítése és kezelése az Azure-ban a feladatok automatizálásához.</span><span class="sxs-lookup"><span data-stu-id="43355-168">Azure PowerShell is a scripting environment that you can use toocontrol and automate hello deployment and management of your workloads in Azure.</span></span> <span data-ttu-id="43355-169">A munkaállomás toorun Azure PowerShell konfigurálásával kapcsolatos további információkért lásd: [telepítse és konfigurálja az Azure Powershellt](/powershell/azure/overview).</span><span class="sxs-lookup"><span data-stu-id="43355-169">For information about configuring your workstation toorun Azure PowerShell, see [Install and configure Azure PowerShell](/powershell/azure/overview).</span></span>

[!INCLUDE [use-latest-version](../../includes/hdinsight-use-latest-powershell.md)]

<span data-ttu-id="43355-170">**egy helyi fájl tooAzure Blob-tároló tooupload**</span><span class="sxs-lookup"><span data-stu-id="43355-170">**tooupload a local file tooAzure Blob storage**</span></span>

1. <span data-ttu-id="43355-171">Megnyitás hello Azure PowerShell-konzolon útmutatását [telepítése és konfigurálása az Azure PowerShell](/powershell/azure/overview).</span><span class="sxs-lookup"><span data-stu-id="43355-171">Open hello Azure PowerShell console as instructed in [Install and configure Azure PowerShell](/powershell/azure/overview).</span></span>
2. <span data-ttu-id="43355-172">Hello hello értékének első öt változók beállítása a következő parancsfájl hello:</span><span class="sxs-lookup"><span data-stu-id="43355-172">Set hello values of hello first five variables in hello following script:</span></span>

        $resourceGroupName = "<AzureResourceGroupName>"
        $storageAccountName = "<StorageAccountName>"
        $containerName = "<ContainerName>"

        $fileName ="<LocalFileName>"
        $blobName = "<BlobName>"

        # Get hello storage account key
        $storageAccountKey = (Get-AzureRmStorageAccountKey -ResourceGroupName $resourceGroupName -Name $storageAccountName)[0].Value
        # Create hello storage context object
        $destContext = New-AzureStorageContext -StorageAccountName $storageAccountName -StorageAccountKey $storageaccountkey

        # Copy hello file from local workstation toohello Blob container
        Set-AzureStorageBlobContent -File $fileName -Container $containerName -Blob $blobName -context $destContext
3. <span data-ttu-id="43355-173">Beillesztés hello parancsfájlt a hello Azure PowerShell konzol toorun azt toocopy hello.</span><span class="sxs-lookup"><span data-stu-id="43355-173">Paste hello script into hello Azure PowerShell console toorun it toocopy hello file.</span></span>

<span data-ttu-id="43355-174">Például a PowerShell parancsfájlok toowork a hdinsight eszközzel, lásd: [a HDInsight tools](https://github.com/blackmist/hdinsight-tools).</span><span class="sxs-lookup"><span data-stu-id="43355-174">For example PowerShell scripts created toowork with HDInsight, see [HDInsight tools](https://github.com/blackmist/hdinsight-tools).</span></span>

### <span data-ttu-id="43355-175"><a id="azcopy"></a>AzCopy</span><span class="sxs-lookup"><span data-stu-id="43355-175"><a id="azcopy"></a>AzCopy</span></span>
<span data-ttu-id="43355-176">AzCopy egy parancssori eszköz, amely tervezett toosimplify hello feladatát adatátvitel esetében bejövő és kimenő egy Azure Storage-fiókot.</span><span class="sxs-lookup"><span data-stu-id="43355-176">AzCopy is a command-line tool that is designed toosimplify hello task of transferring data into and out of an Azure Storage account.</span></span> <span data-ttu-id="43355-177">Önálló eszközként használható, vagy az eszköz már meglévő alkalmazás tartalmaznia.</span><span class="sxs-lookup"><span data-stu-id="43355-177">You can use it as a standalone tool or incorporate this tool in an existing application.</span></span> <span data-ttu-id="43355-178">[Töltse le az AzCopy][azure-azcopy-download].</span><span class="sxs-lookup"><span data-stu-id="43355-178">[Download AzCopy][azure-azcopy-download].</span></span>

<span data-ttu-id="43355-179">hello AzCopy szintaxisa a következő:</span><span class="sxs-lookup"><span data-stu-id="43355-179">hello AzCopy syntax is:</span></span>

    AzCopy <Source> <Destination> [filePattern [filePattern...]] [Options]

<span data-ttu-id="43355-180">További információkért lásd: [AzCopy - feltöltése/letöltése fájlok Azure Blobokra vonatkozó][azure-azcopy].</span><span class="sxs-lookup"><span data-stu-id="43355-180">For more information, see [AzCopy - Uploading/Downloading files for Azure Blobs][azure-azcopy].</span></span>

### <span data-ttu-id="43355-181"><a id="commandline"></a>Hadoop parancssor</span><span class="sxs-lookup"><span data-stu-id="43355-181"><a id="commandline"></a>Hadoop command line</span></span>
<span data-ttu-id="43355-182">hello Hadoop parancssori csak akkor hasznos, ha hello adat már szerepel a hello átjárócsomóponthoz adatok tárolásához a blob-tárolóba.</span><span class="sxs-lookup"><span data-stu-id="43355-182">hello Hadoop command line is only useful for storing data into blob storage when hello data is already present on hello cluster head node.</span></span>

<span data-ttu-id="43355-183">A sorrend toouse hello Hadoop parancs először kapcsolódnia toohello headnode hello a következő módszerek egyikével:</span><span class="sxs-lookup"><span data-stu-id="43355-183">In order toouse hello Hadoop command, you must first connect toohello headnode using one of hello following methods:</span></span>

* <span data-ttu-id="43355-184">**Windows-alapú HDInsight**: [csatlakozzon a távoli asztali kapcsolattal](hdinsight-administer-use-management-portal.md#connect-to-clusters-using-rdp)</span><span class="sxs-lookup"><span data-stu-id="43355-184">**Windows-based HDInsight**: [Connect using Remote Desktop](hdinsight-administer-use-management-portal.md#connect-to-clusters-using-rdp)</span></span>
* <span data-ttu-id="43355-185">**Linux-alapú HDInsight**: SSH protokoll használatával kapcsolódó levelezőprogramokkal ([SSH-parancstól hello](hdinsight-hadoop-linux-use-ssh-unix.md) vagy [PuTTY](hdinsight-hadoop-linux-use-ssh-windows.md))</span><span class="sxs-lookup"><span data-stu-id="43355-185">**Linux-based HDInsight**: Connect using SSH ([hello SSH command](hdinsight-hadoop-linux-use-ssh-unix.md) or [PuTTY](hdinsight-hadoop-linux-use-ssh-windows.md))</span></span>

<span data-ttu-id="43355-186">A csatlakozás után a következő szintaxist tooupload egy fájl toostorage hello is használhatja.</span><span class="sxs-lookup"><span data-stu-id="43355-186">Once connected, you can use hello following syntax tooupload a file toostorage.</span></span>

    hadoop -copyFromLocal <localFilePath> <storageFilePath>

<span data-ttu-id="43355-187">Például: `hadoop fs -copyFromLocal data.txt /example/data/data.txt`</span><span class="sxs-lookup"><span data-stu-id="43355-187">For example, `hadoop fs -copyFromLocal data.txt /example/data/data.txt`</span></span>

<span data-ttu-id="43355-188">Mivel hello alapértelmezett fájlrendszer a HDInsight az Azure Blob Storage tárolóban, /example/data.txt valójában az Azure Blob Storage tárolóban.</span><span class="sxs-lookup"><span data-stu-id="43355-188">Because hello default file system for HDInsight is in Azure Blob storage, /example/data.txt is actually in Azure Blob storage.</span></span> <span data-ttu-id="43355-189">Tekintse meg a toohello fájlt is:</span><span class="sxs-lookup"><span data-stu-id="43355-189">You can also refer toohello file as:</span></span>

    wasb:///example/data/data.txt

<span data-ttu-id="43355-190">vagy</span><span class="sxs-lookup"><span data-stu-id="43355-190">or</span></span>

    wasb://<ContainerName>@<StorageAccountName>.blob.core.windows.net/example/data/davinci.txt

<span data-ttu-id="43355-191">Más Hadoop listáját, hogy a munkahelyi fájlok parancsok, lásd: [http://hadoop.apache.org/docs/r2.7.0/hadoop-project-dist/hadoop-common/FileSystemShell.html](http://hadoop.apache.org/docs/r2.7.0/hadoop-project-dist/hadoop-common/FileSystemShell.html)</span><span class="sxs-lookup"><span data-stu-id="43355-191">For a list of other Hadoop commands that work with files, see [http://hadoop.apache.org/docs/r2.7.0/hadoop-project-dist/hadoop-common/FileSystemShell.html](http://hadoop.apache.org/docs/r2.7.0/hadoop-project-dist/hadoop-common/FileSystemShell.html)</span></span>

> [!WARNING]
> <span data-ttu-id="43355-192">A HBase fürtök esetén hello alapértelmezett blokkméret használható, ha a adatainak írása 256KB.</span><span class="sxs-lookup"><span data-stu-id="43355-192">On HBase clusters, hello default block size used when writing data is 256KB.</span></span> <span data-ttu-id="43355-193">Ez a HBase API-k és a REST API-k használata remekül működik, amíg használatával hello `hadoop` vagy `hdfs dfs` parancsok toowrite adatok ~ 12 GB-nál nagyobb hibát eredményez.</span><span class="sxs-lookup"><span data-stu-id="43355-193">While this works fine when using HBase APIs or REST APIs, using hello `hadoop` or `hdfs dfs` commands toowrite data larger than ~12GB results in an error.</span></span> <span data-ttu-id="43355-194">Lásd: hello [írás a blob storage-hibát](#storageexception) szakasz alatt további információt.</span><span class="sxs-lookup"><span data-stu-id="43355-194">See hello [storage exception for write on blob](#storageexception) section below for more information.</span></span>
>
>

## <a name="graphical-clients"></a><span data-ttu-id="43355-195">Grafikus ügyfelek</span><span class="sxs-lookup"><span data-stu-id="43355-195">Graphical clients</span></span>
<span data-ttu-id="43355-196">Van még több alkalmazás, amely a grafikus felületet biztosít az Azure Storage.</span><span class="sxs-lookup"><span data-stu-id="43355-196">There are also several applications that provide a graphical interface for working with Azure Storage.</span></span> <span data-ttu-id="43355-197">hello az alábbiakban néhány ezeket az alkalmazásokat a listája látható:</span><span class="sxs-lookup"><span data-stu-id="43355-197">hello following is a list of a few of these applications:</span></span>

| <span data-ttu-id="43355-198">Ügyfél</span><span class="sxs-lookup"><span data-stu-id="43355-198">Client</span></span> | <span data-ttu-id="43355-199">Linux</span><span class="sxs-lookup"><span data-stu-id="43355-199">Linux</span></span> | <span data-ttu-id="43355-200">OS X</span><span class="sxs-lookup"><span data-stu-id="43355-200">OS X</span></span> | <span data-ttu-id="43355-201">Windows</span><span class="sxs-lookup"><span data-stu-id="43355-201">Windows</span></span> |
| --- |:---:|:---:|:---:|
| [<span data-ttu-id="43355-202">A Microsoft Visual Studio Tools for HDInsight</span><span class="sxs-lookup"><span data-stu-id="43355-202">Microsoft Visual Studio Tools for HDInsight</span></span>](hdinsight-hadoop-visual-studio-tools-get-started.md#navigate-the-linked-resources) |<span data-ttu-id="43355-203">✔</span><span class="sxs-lookup"><span data-stu-id="43355-203">✔</span></span> |<span data-ttu-id="43355-204">✔</span><span class="sxs-lookup"><span data-stu-id="43355-204">✔</span></span> |<span data-ttu-id="43355-205">✔</span><span class="sxs-lookup"><span data-stu-id="43355-205">✔</span></span> |
| [<span data-ttu-id="43355-206">Azure Storage Explorer</span><span class="sxs-lookup"><span data-stu-id="43355-206">Azure Storage Explorer</span></span>](http://storageexplorer.com/) |<span data-ttu-id="43355-207">✔</span><span class="sxs-lookup"><span data-stu-id="43355-207">✔</span></span> |<span data-ttu-id="43355-208">✔</span><span class="sxs-lookup"><span data-stu-id="43355-208">✔</span></span> |<span data-ttu-id="43355-209">✔</span><span class="sxs-lookup"><span data-stu-id="43355-209">✔</span></span> |
| [<span data-ttu-id="43355-210">Felhőalapú tárolás Studio 2</span><span class="sxs-lookup"><span data-stu-id="43355-210">Cloud Storage Studio 2</span></span>](http://www.cerebrata.com/Products/CloudStorageStudio/) | | |<span data-ttu-id="43355-211">✔</span><span class="sxs-lookup"><span data-stu-id="43355-211">✔</span></span> |
| [<span data-ttu-id="43355-212">CloudXplorer</span><span class="sxs-lookup"><span data-stu-id="43355-212">CloudXplorer</span></span>](http://clumsyleaf.com/products/cloudxplorer) | | |<span data-ttu-id="43355-213">✔</span><span class="sxs-lookup"><span data-stu-id="43355-213">✔</span></span> |
| [<span data-ttu-id="43355-214">Az Azure Explorer</span><span class="sxs-lookup"><span data-stu-id="43355-214">Azure Explorer</span></span>](http://www.cloudberrylab.com/free-microsoft-azure-explorer.aspx) | | |<span data-ttu-id="43355-215">✔</span><span class="sxs-lookup"><span data-stu-id="43355-215">✔</span></span> |
| [<span data-ttu-id="43355-216">Cyberduck</span><span class="sxs-lookup"><span data-stu-id="43355-216">Cyberduck</span></span>](https://cyberduck.io/) | |<span data-ttu-id="43355-217">✔</span><span class="sxs-lookup"><span data-stu-id="43355-217">✔</span></span> |<span data-ttu-id="43355-218">✔</span><span class="sxs-lookup"><span data-stu-id="43355-218">✔</span></span> |

### <a name="visual-studio-tools-for-hdinsight"></a><span data-ttu-id="43355-219">HDInsight Visual Studio eszközök</span><span class="sxs-lookup"><span data-stu-id="43355-219">Visual Studio Tools for HDInsight</span></span>
<span data-ttu-id="43355-220">További információkért lásd: [Navigálás hello kapcsolódó erőforrások](hdinsight-hadoop-visual-studio-tools-get-started.md#navigate-the-linked-resources).</span><span class="sxs-lookup"><span data-stu-id="43355-220">For more information, see [Navigate hello linked resources](hdinsight-hadoop-visual-studio-tools-get-started.md#navigate-the-linked-resources).</span></span>

### <span data-ttu-id="43355-221"><a id="storageexplorer"></a>Az Azure Storage Explorer</span><span class="sxs-lookup"><span data-stu-id="43355-221"><a id="storageexplorer"></a>Azure Storage Explorer</span></span>
<span data-ttu-id="43355-222">*Az Azure Tártallózó* hasznos eszköz a Kérem, és módosítsa a blobok hello adatokat.</span><span class="sxs-lookup"><span data-stu-id="43355-222">*Azure Storage Explorer* is a useful tool for inspecting and altering hello data in blobs.</span></span> <span data-ttu-id="43355-223">Ingyenes, nyissa meg a forrás eszköz, amely letölthető [http://storageexplorer.com/](http://storageexplorer.com/).</span><span class="sxs-lookup"><span data-stu-id="43355-223">It is a free, open source tool that can be downloaded from [http://storageexplorer.com/](http://storageexplorer.com/).</span></span> <span data-ttu-id="43355-224">hello a forráskód nem érhető el ez a hivatkozás.</span><span class="sxs-lookup"><span data-stu-id="43355-224">hello source code is available from this link as well.</span></span>

<span data-ttu-id="43355-225">Hello eszköz használata előtt ismernie kell az Azure storage-fiók nevét és a fiók kulcs.</span><span class="sxs-lookup"><span data-stu-id="43355-225">Before using hello tool, you must know your Azure storage account name and account key.</span></span> <span data-ttu-id="43355-226">Az alábbi információk eljussanak vonatkozó utasításokért lásd: hello "hogyan: megtekintése, másolása és újragenerálása tárolási hívóbetűk" szakasza [létrehozása, kezelése vagy törlése a tárfiók][azure-create-storage-account].</span><span class="sxs-lookup"><span data-stu-id="43355-226">For instructions about getting this information, see hello "How to: View, copy and regenerate storage access keys" section of [Create, manage, or delete a storage account][azure-create-storage-account].</span></span>

1. <span data-ttu-id="43355-227">Az Azure Storage-kezelő futtatása.</span><span class="sxs-lookup"><span data-stu-id="43355-227">Run Azure Storage Explorer.</span></span> <span data-ttu-id="43355-228">Ha hello első alkalommal hello Tártallózó futtatása, akkor a rendszer bekéri a hello **tá_rolási fióknév** és **Tárfiók kulcsa**.</span><span class="sxs-lookup"><span data-stu-id="43355-228">If this is hello first time you have run hello Storage Explorer, you will be prompted for hello **_Storage account name** and **Storage account key**.</span></span> <span data-ttu-id="43355-229">Futtatásakor az előtt, használja a hello **Hozzáadás** tooadd egy új tárolási fióknevet és kulcsot gombra.</span><span class="sxs-lookup"><span data-stu-id="43355-229">If you have run it before, use hello **Add** button tooadd a new storage account name and key.</span></span>

    <span data-ttu-id="43355-230">Adja meg a név és kulcs a HDInsight-fürt által használt hello tárfiók, és adja hello **MENTÉS és MEGNYITÁS**.</span><span class="sxs-lookup"><span data-stu-id="43355-230">Enter hello name and key for hello storage account used by your HDInsight cluster and then select **SAVE & OPEN**.</span></span>

    ![HDI. AzureStorageExplorer][image-azure-storage-explorer]
2. <span data-ttu-id="43355-232">Tárolók toohello balra hello felület hello listájában kattintson a HDInsight-fürthöz társított hello tároló hello neve.</span><span class="sxs-lookup"><span data-stu-id="43355-232">In hello list of containers toohello left of hello interface, click hello name of hello container that is associated with your HDInsight cluster.</span></span> <span data-ttu-id="43355-233">Alapértelmezés szerint ez hello hello HDInsight-fürt nevét, de eltérő, ha hello fürt létrehozásakor adja meg az adott név lehet.</span><span class="sxs-lookup"><span data-stu-id="43355-233">By default, this is hello name of hello HDInsight cluster, but may be different if you entered a specific name when creating hello cluster.</span></span>
3. <span data-ttu-id="43355-234">Hello eszköztáron jelölje ki hello Feltöltés ikonra.</span><span class="sxs-lookup"><span data-stu-id="43355-234">From hello tool bar, select hello upload icon.</span></span>

    ![A feltöltés ikonja kiemelve eszköztár](./media/hdinsight-upload-data/toolbar.png)
4. <span data-ttu-id="43355-236">Adjon meg egy fájl tooupload, és kattintson a **nyitott**.</span><span class="sxs-lookup"><span data-stu-id="43355-236">Specify a file tooupload, and then click **Open**.</span></span> <span data-ttu-id="43355-237">Amikor a rendszer kéri, válassza ki a **feltöltése** tooupload hello fájl toohello gyökere hello tároló.</span><span class="sxs-lookup"><span data-stu-id="43355-237">When prompted, select **Upload** tooupload hello file toohello root of hello storage container.</span></span> <span data-ttu-id="43355-238">Ha azt szeretné, hogy tooupload hello tooa megadott elérési útja, adja meg hello elérési hello **cél** mezőbe, majd válassza ki **feltöltése**.</span><span class="sxs-lookup"><span data-stu-id="43355-238">If you want tooupload hello file tooa specific path, enter hello path in hello **Destination** field and then select **Upload**.</span></span>

    ![Fájl feltöltése párbeszédpanel](./media/hdinsight-upload-data/fileupload.png)

    <span data-ttu-id="43355-240">Hello fájl befejezte a feltöltést, amint a HDInsight-fürt hello feladatokból használhatja.</span><span class="sxs-lookup"><span data-stu-id="43355-240">Once hello file has finished uploading, you can use it from jobs on hello HDInsight cluster.</span></span>

## <a name="mount-azure-blob-storage-as-local-drive"></a><span data-ttu-id="43355-241">Helyi meghajtó csatlakoztatási Azure Blob Storage tárolót</span><span class="sxs-lookup"><span data-stu-id="43355-241">Mount Azure Blob Storage as Local Drive</span></span>
<span data-ttu-id="43355-242">Lásd: [csatlakoztatási Azure Blob Storage tárolót helyi meghajtó](http://blogs.msdn.com/b/bigdatasupport/archive/2014/01/09/mount-azure-blob-storage-as-local-drive.aspx).</span><span class="sxs-lookup"><span data-stu-id="43355-242">See [Mount Azure Blob Storage as Local Drive](http://blogs.msdn.com/b/bigdatasupport/archive/2014/01/09/mount-azure-blob-storage-as-local-drive.aspx).</span></span>

## <a name="services"></a><span data-ttu-id="43355-243">Szolgáltatások</span><span class="sxs-lookup"><span data-stu-id="43355-243">Services</span></span>
### <a name="azure-data-factory"></a><span data-ttu-id="43355-244">Azure Data Factory</span><span class="sxs-lookup"><span data-stu-id="43355-244">Azure Data Factory</span></span>
<span data-ttu-id="43355-245">hello Azure Data Factory szolgáltatásnak egy teljes körűen felügyelt szolgáltatás zökkenőmentes, méretezhető és megbízható éles adatcsatornák a tárolás, az adatfeldolgozás és adatok adatátviteli szolgáltatások létrehozására.</span><span class="sxs-lookup"><span data-stu-id="43355-245">hello Azure Data Factory service is a fully managed service for composing data storage, data processing, and data movement services into streamlined, scalable, and reliable data production pipelines.</span></span>

<span data-ttu-id="43355-246">Az Azure Data Factory lehet használt toomove adatok Azure Blob Storage tárolóban, vagy toocreate adatok folyamatok közvetlenül használni, mint a szolgáltatások HDInsight Hive és a sertésfelmérés.</span><span class="sxs-lookup"><span data-stu-id="43355-246">Azure Data Factory can be used toomove data into Azure Blob storage, or toocreate data pipelines that directly use HDInsight features such as Hive and Pig.</span></span>

<span data-ttu-id="43355-247">További információkért lásd: hello [Azure Data Factory dokumentáció](https://azure.microsoft.com/documentation/services/data-factory/).</span><span class="sxs-lookup"><span data-stu-id="43355-247">For more information, see hello [Azure Data Factory documentation](https://azure.microsoft.com/documentation/services/data-factory/).</span></span>

### <span data-ttu-id="43355-248"><a id="sqoop"></a>Apache Sqoop</span><span class="sxs-lookup"><span data-stu-id="43355-248"><a id="sqoop"></a>Apache Sqoop</span></span>
<span data-ttu-id="43355-249">Sqoop egy eszköz tootransfer adatokat Hadoop és a relációs adatbázisok között.</span><span class="sxs-lookup"><span data-stu-id="43355-249">Sqoop is a tool designed tootransfer data between Hadoop and relational databases.</span></span> <span data-ttu-id="43355-250">Használat tooimport adatait egy relációs adatbázis-kezelő rendszerének (RDBMS), például az SQL Server, MySQL, vagy hello Hadoop elosztott fájlrendszer (HDFS), az Oracle hello adatok a Hadoop MapReduce vagy a Hive, majd exportálja újra hello adatok egy RDBMS.</span><span class="sxs-lookup"><span data-stu-id="43355-250">You can use it tooimport data from a relational database management system (RDBMS), such as SQL Server, MySQL, or Oracle into hello Hadoop distributed file system (HDFS), transform hello data in Hadoop with MapReduce or Hive, and then export hello data back into an RDBMS.</span></span>

<span data-ttu-id="43355-251">További információkért lásd: [és a HDInsight együttes használata Sqoop][hdinsight-use-sqoop].</span><span class="sxs-lookup"><span data-stu-id="43355-251">For more information, see [Use Sqoop with HDInsight][hdinsight-use-sqoop].</span></span>

## <a name="development-sdks"></a><span data-ttu-id="43355-252">Fejlesztési SDK-k</span><span class="sxs-lookup"><span data-stu-id="43355-252">Development SDKs</span></span>
<span data-ttu-id="43355-253">Az Azure Blob storage használata az Azure SDK-t a következő programozási nyelvek hello is elérhető:</span><span class="sxs-lookup"><span data-stu-id="43355-253">Azure Blob storage can also be accessed using an Azure SDK from hello following programming languages:</span></span>

* <span data-ttu-id="43355-254">.NET</span><span class="sxs-lookup"><span data-stu-id="43355-254">.NET</span></span>
* <span data-ttu-id="43355-255">Java</span><span class="sxs-lookup"><span data-stu-id="43355-255">Java</span></span>
* <span data-ttu-id="43355-256">Node.js</span><span class="sxs-lookup"><span data-stu-id="43355-256">Node.js</span></span>
* <span data-ttu-id="43355-257">PHP</span><span class="sxs-lookup"><span data-stu-id="43355-257">PHP</span></span>
* <span data-ttu-id="43355-258">Python</span><span class="sxs-lookup"><span data-stu-id="43355-258">Python</span></span>
* <span data-ttu-id="43355-259">Ruby</span><span class="sxs-lookup"><span data-stu-id="43355-259">Ruby</span></span>

<span data-ttu-id="43355-260">Hello Azure SDK-k telepítésével kapcsolatos további információkért lásd: [Azure tölti le.](https://azure.microsoft.com/downloads/)</span><span class="sxs-lookup"><span data-stu-id="43355-260">For more information on installing hello Azure SDKs, see [Azure downloads](https://azure.microsoft.com/downloads/)</span></span>

## <a name="troubleshooting"></a><span data-ttu-id="43355-261">Hibaelhárítás</span><span class="sxs-lookup"><span data-stu-id="43355-261">Troubleshooting</span></span>
### <span data-ttu-id="43355-262"><a id="storageexception"></a>Írás a blob Storage-hibát</span><span class="sxs-lookup"><span data-stu-id="43355-262"><a id="storageexception"></a>Storage exception for write on blob</span></span>
<span data-ttu-id="43355-263">**A jelenség**: hello használatakor `hadoop` vagy `hdfs dfs` toowrite fájlok parancsok is ~ 12 GB vagy nagyobb a HBase-fürtöt, felmerülhet a következő hiba hello:</span><span class="sxs-lookup"><span data-stu-id="43355-263">**Symptoms**: When using hello `hadoop` or `hdfs dfs` commands toowrite files that are ~12GB or larger on an HBase cluster, you may encounter hello following error:</span></span>

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
    Caused by: com.microsoft.azure.storage.StorageException: hello request body is too large and exceeds hello maximum permissible limit.
            at com.microsoft.azure.storage.StorageException.translateException(StorageException.java:89)
            at com.microsoft.azure.storage.core.StorageRequest.materializeException(StorageRequest.java:307)
            at com.microsoft.azure.storage.core.ExecutionEngine.executeWithRetry(ExecutionEngine.java:182)
            at com.microsoft.azure.storage.blob.CloudBlockBlob.uploadBlockInternal(CloudBlockBlob.java:816)
            at com.microsoft.azure.storage.blob.CloudBlockBlob.uploadBlock(CloudBlockBlob.java:788)
            at com.microsoft.azure.storage.blob.BlobOutputStream$1.call(BlobOutputStream.java:354)
            ... 7 more

<span data-ttu-id="43355-264">**OK**: a HBase a HDInsight-fürtök alapértelmezett tooa 256 KB-os blokkméret tooAzure tárolási írása közben.</span><span class="sxs-lookup"><span data-stu-id="43355-264">**Cause**: HBase on HDInsight clusters default tooa block size of 256KB when writing tooAzure storage.</span></span> <span data-ttu-id="43355-265">Amíg ez működik, a HBase API-k vagy a REST API-k, okoz hiba hello használatakor `hadoop` vagy `hdfs dfs` parancssori segédprogramok.</span><span class="sxs-lookup"><span data-stu-id="43355-265">While this works for HBase APIs or REST APIs, it will result in an error when using hello `hadoop` or `hdfs dfs` command-line utilities.</span></span>

<span data-ttu-id="43355-266">**Megoldási**: használata `fs.azure.write.request.size` toospecify a nagyobb blokkméret.</span><span class="sxs-lookup"><span data-stu-id="43355-266">**Resolution**: Use `fs.azure.write.request.size` toospecify a larger block size.</span></span> <span data-ttu-id="43355-267">Ehhez a használati alapon hello segítségével `-D` paraméter.</span><span class="sxs-lookup"><span data-stu-id="43355-267">You can do this on a per-use basis by using hello `-D` parameter.</span></span> <span data-ttu-id="43355-268">hello az alábbiakban látható egy példa, ez a paraméter használata hello `hadoop` parancs:</span><span class="sxs-lookup"><span data-stu-id="43355-268">hello following is an example using this parameter with hello `hadoop` command:</span></span>

    hadoop -fs -D fs.azure.write.request.size=4194304 -copyFromLocal test_large_file.bin /example/data

<span data-ttu-id="43355-269">Növelje a hello értékének `fs.azure.write.request.size` globálisan Ambari használatával.</span><span class="sxs-lookup"><span data-stu-id="43355-269">You can also increase hello value of `fs.azure.write.request.size` globally by using Ambari.</span></span> <span data-ttu-id="43355-270">hello lépések lehet toochange hello érték szerepel hello Ambari webes felhasználói felületén:</span><span class="sxs-lookup"><span data-stu-id="43355-270">hello following steps can be used toochange hello value in hello Ambari Web UI:</span></span>

1. <span data-ttu-id="43355-271">A böngészőben nyissa meg a fürt Ambari webes felhasználói felületén toohello.</span><span class="sxs-lookup"><span data-stu-id="43355-271">In your browser, go toohello Ambari Web UI for your cluster.</span></span> <span data-ttu-id="43355-272">Ez a https://CLUSTERNAME.azurehdinsight.net, ahol **CLUSTERNAME** hello a fürt neve van.</span><span class="sxs-lookup"><span data-stu-id="43355-272">This is https://CLUSTERNAME.azurehdinsight.net, where **CLUSTERNAME** is hello name of your cluster.</span></span>

    <span data-ttu-id="43355-273">Amikor a rendszer kéri, adja meg hello admin nevét és jelszavát hello fürt.</span><span class="sxs-lookup"><span data-stu-id="43355-273">When prompted, enter hello admin name and password for hello cluster.</span></span>
2. <span data-ttu-id="43355-274">Hello bal oldalán található üdvözlő képernyőt, válassza ki **HDFS**, majd válassza ki a hello **Configs** fülre.</span><span class="sxs-lookup"><span data-stu-id="43355-274">From hello left side of hello screen, select **HDFS**, and then select hello **Configs** tab.</span></span>
3. <span data-ttu-id="43355-275">A hello **szűrő...**  mezőbe írja be `fs.azure.write.request.size`.</span><span class="sxs-lookup"><span data-stu-id="43355-275">In hello **Filter...** field, enter `fs.azure.write.request.size`.</span></span> <span data-ttu-id="43355-276">Ez megjeleníti hello mező és az aktuális érték hello lap hello közepén.</span><span class="sxs-lookup"><span data-stu-id="43355-276">This will display hello field and current value in hello middle of hello page.</span></span>
4. <span data-ttu-id="43355-277">Új érték 262144 (256KB) toohello hello érték módosítása</span><span class="sxs-lookup"><span data-stu-id="43355-277">Change hello value from 262144 (256KB) toohello new value.</span></span> <span data-ttu-id="43355-278">Például 4194304 (4MB).</span><span class="sxs-lookup"><span data-stu-id="43355-278">For example, 4194304 (4MB).</span></span>

![Ambari webes felhasználói felületén keresztül hello érték módosítása képe](./media/hdinsight-upload-data/hbase-change-block-write-size.png)

<span data-ttu-id="43355-280">További információ az Ambari használatával, lásd: [hello Ambari webes felhasználói felület használatával kezelheti a HDInsight-fürtök](hdinsight-hadoop-manage-ambari.md).</span><span class="sxs-lookup"><span data-stu-id="43355-280">For more information on using Ambari, see [Manage HDInsight clusters using hello Ambari Web UI](hdinsight-hadoop-manage-ambari.md).</span></span>

## <a name="next-steps"></a><span data-ttu-id="43355-281">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="43355-281">Next steps</span></span>
<span data-ttu-id="43355-282">Most, hogy megismerkedett hogyan tooget adatok HDInsight, olvassa el a következő cikkek toolearn hogyan hello tooperform analysis:</span><span class="sxs-lookup"><span data-stu-id="43355-282">Now that you understand how tooget data into HDInsight, read hello following articles toolearn how tooperform analysis:</span></span>

* <span data-ttu-id="43355-283">[Azure HDInsight – első lépések][hdinsight-get-started]</span><span class="sxs-lookup"><span data-stu-id="43355-283">[Get started with Azure HDInsight][hdinsight-get-started]</span></span>
* <span data-ttu-id="43355-284">[Hadoop-feladatokat programozott módon küldhet][hdinsight-submit-jobs]</span><span class="sxs-lookup"><span data-stu-id="43355-284">[Submit Hadoop jobs programmatically][hdinsight-submit-jobs]</span></span>
* <span data-ttu-id="43355-285">[A Hive használata a HDInsightban][hdinsight-use-hive]</span><span class="sxs-lookup"><span data-stu-id="43355-285">[Use Hive with HDInsight][hdinsight-use-hive]</span></span>
* <span data-ttu-id="43355-286">[A Pig használata a HDInsightban][hdinsight-use-pig]</span><span class="sxs-lookup"><span data-stu-id="43355-286">[Use Pig with HDInsight][hdinsight-use-pig]</span></span>

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
