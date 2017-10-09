---
title: "egy Azure fájlmegosztás aaaHow toocreate |} Microsoft Docs"
description: "Hogyan toocreate egy Azure fájlmegosztás az Azure File storage hello Azure-portálon, a PowerShell és a hello Azure parancssori felület használatával."
services: storage
documentationcenter: 
author: RenaShahMSFT
manager: aungoo
editor: tysonn
ms.assetid: 
ms.service: storage
ms.workload: storage
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 05/27/2017
ms.author: renash
ms.openlocfilehash: c4c59966b82d743fb4ecf79f48c0c8e61295d03d
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="create-a-file-share-in-azure-file-storage"></a><span data-ttu-id="e2ac4-103">Fájlmegosztás létrehozása az Azure File Storage-ban</span><span class="sxs-lookup"><span data-stu-id="e2ac4-103">Create a File Share in Azure File storage</span></span>
<span data-ttu-id="e2ac4-104">Az Azure fájlmegosztások használatával hozhat létre [Azure-portálon](https://portal.azure.com/), hello Azure Storage PowerShell parancsmagjainak, hello Azure Storage ügyfélkódtáraival vagy hello Azure Storage REST API-t. Ebben az oktatóanyagban témák köre:</span><span class="sxs-lookup"><span data-stu-id="e2ac4-104">You can create Azure File shares using [Azure portal](https://portal.azure.com/), hello Azure Storage PowerShell cmdlets, hello Azure Storage client libraries, or hello Azure Storage REST API.In this tutorial, you will learn:</span></span>
* [<span data-ttu-id="e2ac4-105">Hogyan toocreate egy Azure-fájl megosztása hello Azure-portál használatával</span><span class="sxs-lookup"><span data-stu-id="e2ac4-105">How toocreate an Azure File share using hello Azure portal</span></span>](#Create file share through hello Portal)
* [<span data-ttu-id="e2ac4-106">Hogyan toocreate egy Azure fájlmegosztás Powershell használatával</span><span class="sxs-lookup"><span data-stu-id="e2ac4-106">How toocreate an Azure File share using Powershell</span></span>](#Create file share using PowerShell)
* [<span data-ttu-id="e2ac4-107">Hogyan toocreate egy Azure fájlmegosztás parancssori felület használatával</span><span class="sxs-lookup"><span data-stu-id="e2ac4-107">How toocreate an Azure File share using CLI</span></span>](#create-file-share-using-command-line-interface-cli)

## <a name="prerequisites"></a><span data-ttu-id="e2ac4-108">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="e2ac4-108">Prerequisites</span></span>
<span data-ttu-id="e2ac4-109">egy Azure fájlmegosztás toocreate, használhatja a storage-fiók, amely már létezik, vagy [hozzon létre egy új Azure-tárfiók](storage-create-storage-account.md).</span><span class="sxs-lookup"><span data-stu-id="e2ac4-109">toocreate an Azure File share, you can use a storage account that already exists, or [create a new Azure storage account](storage-create-storage-account.md).</span></span> <span data-ttu-id="e2ac4-110">egy Azure fájlmegosztás PowerShell toocreate, szüksége lesz hello fiókkulcs és a tárfiók nevét...</span><span class="sxs-lookup"><span data-stu-id="e2ac4-110">toocreate an Azure File share with PowerShell, you will need hello account key and name of your storage account..</span></span> <span data-ttu-id="e2ac4-111">Ha azt tervezi, hogy a Powershell vagy a CLI toouse kell Tárfiók kulcsa.</span><span class="sxs-lookup"><span data-stu-id="e2ac4-111">You will need Storage account key if you plan toouse Powershell or CLI.</span></span>

## <a name="create-file-share-through-hello-portal"></a><span data-ttu-id="e2ac4-112">Hello portálon keresztül fájlmegosztás létrehozása</span><span class="sxs-lookup"><span data-stu-id="e2ac4-112">Create file share through hello Portal</span></span>
1. <span data-ttu-id="e2ac4-113">**Azure-portál lépjen tooStorage fiók paneljét**:</span><span class="sxs-lookup"><span data-stu-id="e2ac4-113">**Go tooStorage Account blade on Azure Portal**:</span></span>    
    <span data-ttu-id="e2ac4-114">![Tárfiók panel](media/storage-file-how-to-create-file-share/create-file-share-portal1.png)</span><span class="sxs-lookup"><span data-stu-id="e2ac4-114">![Storage Account Blade](media/storage-file-how-to-create-file-share/create-file-share-portal1.png)</span></span>

2. <span data-ttu-id="e2ac4-115">**Kattintson a Fájlmegosztás hozzáadása gombra**:</span><span class="sxs-lookup"><span data-stu-id="e2ac4-115">**Click on add File Share button**:</span></span>    
    <span data-ttu-id="e2ac4-116">![Kattintson a hello fájl megosztás gomb hozzáadása](media/storage-file-how-to-create-file-share/create-file-share-portal2.png)</span><span class="sxs-lookup"><span data-stu-id="e2ac4-116">![Click hello add file share button](media/storage-file-how-to-create-file-share/create-file-share-portal2.png)</span></span>

3. <span data-ttu-id="e2ac4-117">**Adja meg a nevet és a kvótát. A kvóta jelenleg legfeljebb 5 TB lehet**:</span><span class="sxs-lookup"><span data-stu-id="e2ac4-117">**Provide Name and Quota. Quota currently can be maximum 5TB**:</span></span>    
    <span data-ttu-id="e2ac4-118">![Adjon meg egy nevet és a kívánt kvóta hello Új fájlmegosztás](media/storage-file-how-to-create-file-share/create-file-share-portal3.png)</span><span class="sxs-lookup"><span data-stu-id="e2ac4-118">![Provide a name and a desired quota for hello new file share](media/storage-file-how-to-create-file-share/create-file-share-portal3.png)</span></span>

4. <span data-ttu-id="e2ac4-119">**Tekintse meg az új fájlmegosztást**: ![Az új fájlmegosztás megtekintése](media/storage-file-how-to-create-file-share/create-file-share-portal4.png)</span><span class="sxs-lookup"><span data-stu-id="e2ac4-119">**View your new file share**:  ![View your new file share](media/storage-file-how-to-create-file-share/create-file-share-portal4.png)</span></span>

5. <span data-ttu-id="e2ac4-120">**Töltsön fel egy fájlt**: ![Fájl feltöltése](media/storage-file-how-to-create-file-share/create-file-share-portal5.png)</span><span class="sxs-lookup"><span data-stu-id="e2ac4-120">**Upload a file**:  ![Upload a file](media/storage-file-how-to-create-file-share/create-file-share-portal5.png)</span></span>

6. <span data-ttu-id="e2ac4-121">**Tallózzon az új fájlmegosztásban, és kezelje könyvtárait és fájljait**: ![Fájlmegosztás tallózása](media/storage-file-how-to-create-file-share/create-file-share-portal6.png)</span><span class="sxs-lookup"><span data-stu-id="e2ac4-121">**Browse into your file share and manage your directories and files**:  ![Browse file share](media/storage-file-how-to-create-file-share/create-file-share-portal6.png)</span></span>


## <a name="create-file-share-through-powershell"></a><span data-ttu-id="e2ac4-122">Fájlmegosztás létrehozása PowerShell-lel</span><span class="sxs-lookup"><span data-stu-id="e2ac4-122">Create file share through PowerShell</span></span>
<span data-ttu-id="e2ac4-123">tooprepare toouse PowerShell, töltse le és telepítse hello Azure PowerShell-parancsmagokat.</span><span class="sxs-lookup"><span data-stu-id="e2ac4-123">tooprepare toouse PowerShell, download and install hello Azure PowerShell cmdlets.</span></span> <span data-ttu-id="e2ac4-124">Lásd: [hogyan tooinstall és konfigurálja az Azure Powershellt](https://azure.microsoft.com/documentation/articles/powershell-install-configure/) hello telepítse pont és a telepítési utasításokat.</span><span class="sxs-lookup"><span data-stu-id="e2ac4-124">See [How tooinstall and configure Azure PowerShell](https://azure.microsoft.com/documentation/articles/powershell-install-configure/) for hello install point and installation instructions.</span></span>

> [!Note]  
> <span data-ttu-id="e2ac4-125">Javasoljuk, hogy letöltése és telepítése vagy frissítése toohello legújabb Azure PowerShell modul.</span><span class="sxs-lookup"><span data-stu-id="e2ac4-125">It's recommended that you download and install or upgrade toohello latest Azure PowerShell module.</span></span>

1. <span data-ttu-id="e2ac4-126">**A tárfiók és a kulcs környezet létrehozása** hello környezet magában foglalja a hello tárfiók neve és a fiók kulcsot.</span><span class="sxs-lookup"><span data-stu-id="e2ac4-126">**Create a context for your storage account and key** hello context encapsulates hello storage account name and account key.</span></span> <span data-ttu-id="e2ac4-127">Útmutatás a fiókkulcs átmásolásához az [Azure Portalról](https://portal.azure.com/): [A tárelérési kulcs megtekintése és másolása](storage-create-storage-account.md#view-and-copy-storage-access-keys).</span><span class="sxs-lookup"><span data-stu-id="e2ac4-127">For instructions on copying your account key from the [Azure portal](https://portal.azure.com/), see [View and copy storage access keys](storage-create-storage-account.md#view-and-copy-storage-access-keys).</span></span>

    ```powershell
    $storageContext = New-AzureStorageContext <storage-account-name> <storage-account-key>
    ```
    
2. <span data-ttu-id="e2ac4-128">**Hozzon létre egy új fájlmegosztást**:</span><span class="sxs-lookup"><span data-stu-id="e2ac4-128">**Create a new file share**:</span></span>    
    
    ```powershell
    $share = New-AzureStorageShare logs -Context $storageContext
    ```

> [!Note]  
> <span data-ttu-id="e2ac4-129">a fájlmegosztás nevét hello összes kisbetűnek kell lennie.</span><span class="sxs-lookup"><span data-stu-id="e2ac4-129">hello name of your file share must be all lowercase.</span></span> <span data-ttu-id="e2ac4-130">A fájlmegosztások és fájlok elnevezésére vonatkozó információkért lásd: [Naming and Referencing Shares, Directories, Files, and Metadata](https://msdn.microsoft.com/library/azure/dn167011.aspx) (Megosztások, könyvtárak, fájlok és metaadatok elnevezése és hivatkozása).</span><span class="sxs-lookup"><span data-stu-id="e2ac4-130">For complete details about naming file shares and files, see [Naming and Referencing Shares, Directories, Files, and Metadata](https://msdn.microsoft.com/library/azure/dn167011.aspx).</span></span>

## <a name="create-file-share-through-command-line-interface-cli"></a><span data-ttu-id="e2ac4-131">Fájlmegosztás létrehozása parancssori felület (CLI) használatával</span><span class="sxs-lookup"><span data-stu-id="e2ac4-131">Create file share through Command Line Interface (CLI)</span></span>
1. <span data-ttu-id="e2ac4-132">**tooprepare toouse parancssori felület (CLI) letöltése, és hello Azure parancssori felület telepítése.**</span><span class="sxs-lookup"><span data-stu-id="e2ac4-132">**tooprepare toouse a Command Line Interface (CLI), download and install hello Azure CLI.**</span></span>  
    <span data-ttu-id="e2ac4-133">Lásd: [Az Azure CLI 2.0-s verziójának telepítése](/cli/azure/install-az-cli2.md) és [Bevezetés az Azure CLI 2.0-s verziójának használatába](/cli/azure/get-started-with-azure-cli.md).</span><span class="sxs-lookup"><span data-stu-id="e2ac4-133">See [Install Azure CLI 2.0](/cli/azure/install-az-cli2.md) and [Get started with Azure CLI 2.0](/cli/azure/get-started-with-azure-cli.md).</span></span>

2. <span data-ttu-id="e2ac4-134">**Hozzon létre egy kapcsolati karakterlánc toohello tárfiókot toocreate hello megosztást, ahová.**</span><span class="sxs-lookup"><span data-stu-id="e2ac4-134">**Create a connection string toohello storage account where you want toocreate hello share.**</span></span>  
    <span data-ttu-id="e2ac4-135">Cserélje le ```<storage-account>``` és ```<resource_group>``` az a fiók nevét és az erőforrás tárolócsoportot az alábbi példa hello.</span><span class="sxs-lookup"><span data-stu-id="e2ac4-135">Replace ```<storage-account>``` and ```<resource_group>``` with your storage account name and resource group in hello following example.</span></span>

   ```azurecli
    current_env_conn_string = $(az storage account show-connection-string -n <storage-account> -g <resource-group> --query 'connectionString' -o tsv)

    if [[ $current_env_conn_string == "" ]]; then  
        echo "Couldn't retrieve hello connection string."
    fi
    ```

3. <span data-ttu-id="e2ac4-136">**Fájlmegosztás létrehozása**</span><span class="sxs-lookup"><span data-stu-id="e2ac4-136">**Create file share**</span></span>
    ```azurecli
    az storage share create --name files --quota 2048 --connection-string $current_env_conn_string 1 > /dev/null
    ```

## <a name="next-steps"></a><span data-ttu-id="e2ac4-137">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="e2ac4-137">Next steps</span></span>
* [<span data-ttu-id="e2ac4-138">Fájlmegosztás csatlakoztatása – Windows</span><span class="sxs-lookup"><span data-stu-id="e2ac4-138">Connect and Mount File Share - Windows</span></span>](storage-file-how-to-use-files-windows.md)
* [<span data-ttu-id="e2ac4-139">Fájlmegosztás csatlakoztatása – Linux</span><span class="sxs-lookup"><span data-stu-id="e2ac4-139">Connect and Mount File Share - Linux</span></span>](storage-how-to-use-files-linux.md)
* [<span data-ttu-id="e2ac4-140">Fájlmegosztás csatlakoztatása – macOS</span><span class="sxs-lookup"><span data-stu-id="e2ac4-140">Connect and Mount File Share - macOS</span></span>](storage-file-how-to-use-files-mac.md)

<span data-ttu-id="e2ac4-141">Az alábbi hivatkozások további információkat tartalmaznak az Azure File Storage-ról.</span><span class="sxs-lookup"><span data-stu-id="e2ac4-141">See these links for more information about Azure File storage.</span></span>

* [<span data-ttu-id="e2ac4-142">Gyakori kérdések</span><span class="sxs-lookup"><span data-stu-id="e2ac4-142">FAQ</span></span>](storage-files-faq.md)
* [<span data-ttu-id="e2ac4-143">hibaelhárítással</span><span class="sxs-lookup"><span data-stu-id="e2ac4-143">Troubleshooting</span></span>](storage-troubleshoot-file-connection-problems.md)