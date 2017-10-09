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
ms.openlocfilehash: 816694e411a993dae881816fc62173e2b7afe990
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="create-a-file-share-in-azure-file-storage"></a><span data-ttu-id="215bb-103">Fájlmegosztás létrehozása az Azure File Storage-ban</span><span class="sxs-lookup"><span data-stu-id="215bb-103">Create a File Share in Azure File storage</span></span>
<span data-ttu-id="215bb-104">Az Azure fájlmegosztások használatával hozhat létre [Azure-portálon](https://portal.azure.com/), hello Azure Storage PowerShell parancsmagjainak, hello Azure Storage ügyfélkódtáraival vagy hello Azure Storage REST API-t. Ebben az oktatóanyagban témák köre:</span><span class="sxs-lookup"><span data-stu-id="215bb-104">You can create Azure File shares using [Azure portal](https://portal.azure.com/), hello Azure Storage PowerShell cmdlets, hello Azure Storage client libraries, or hello Azure Storage REST API.In this tutorial, you will learn:</span></span>
* [<span data-ttu-id="215bb-105">Hogyan toocreate egy Azure-fájl megosztása hello Azure-portál használatával</span><span class="sxs-lookup"><span data-stu-id="215bb-105">How toocreate an Azure File share using hello Azure portal</span></span>](#Create file share through hello Portal)
* [<span data-ttu-id="215bb-106">Hogyan toocreate egy Azure fájlmegosztás Powershell használatával</span><span class="sxs-lookup"><span data-stu-id="215bb-106">How toocreate an Azure File share using Powershell</span></span>](#Create file share using PowerShell)
* [<span data-ttu-id="215bb-107">Hogyan toocreate egy Azure fájlmegosztás parancssori felület használatával</span><span class="sxs-lookup"><span data-stu-id="215bb-107">How toocreate an Azure File share using CLI</span></span>](#create-file-share-using-command-line-interface-cli)

## <a name="prerequisites"></a><span data-ttu-id="215bb-108">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="215bb-108">Prerequisites</span></span>
<span data-ttu-id="215bb-109">egy Azure fájlmegosztás toocreate, használhatja a storage-fiók, amely már létezik, vagy [hozzon létre egy új Azure-tárfiók](../common/storage-create-storage-account.md?toc=%2fazure%2fstorage%2ffiles%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="215bb-109">toocreate an Azure File share, you can use a storage account that already exists, or [create a new Azure storage account](../common/storage-create-storage-account.md?toc=%2fazure%2fstorage%2ffiles%2ftoc.json).</span></span> <span data-ttu-id="215bb-110">egy Azure fájlmegosztás PowerShell toocreate, szüksége lesz hello fiókkulcs és a tárfiók nevét...</span><span class="sxs-lookup"><span data-stu-id="215bb-110">toocreate an Azure File share with PowerShell, you will need hello account key and name of your storage account..</span></span> <span data-ttu-id="215bb-111">Ha azt tervezi, hogy a Powershell vagy a CLI toouse kell Tárfiók kulcsa.</span><span class="sxs-lookup"><span data-stu-id="215bb-111">You will need Storage account key if you plan toouse Powershell or CLI.</span></span>

## <a name="create-file-share-through-hello-portal"></a><span data-ttu-id="215bb-112">Hello portálon keresztül fájlmegosztás létrehozása</span><span class="sxs-lookup"><span data-stu-id="215bb-112">Create file share through hello Portal</span></span>
1. <span data-ttu-id="215bb-113">**Azure-portál lépjen tooStorage fiók paneljét**:</span><span class="sxs-lookup"><span data-stu-id="215bb-113">**Go tooStorage Account blade on Azure Portal**:</span></span>    
    <span data-ttu-id="215bb-114">![Tárfiók panel](./media/storage-how-to-create-file-share/create-file-share-portal1.png)</span><span class="sxs-lookup"><span data-stu-id="215bb-114">![Storage Account Blade](./media/storage-how-to-create-file-share/create-file-share-portal1.png)</span></span>

2. <span data-ttu-id="215bb-115">**Kattintson a Fájlmegosztás hozzáadása gombra**:</span><span class="sxs-lookup"><span data-stu-id="215bb-115">**Click on add File Share button**:</span></span>    
    <span data-ttu-id="215bb-116">![Kattintson a hello fájl megosztás gomb hozzáadása](./media/storage-how-to-create-file-share/create-file-share-portal2.png)</span><span class="sxs-lookup"><span data-stu-id="215bb-116">![Click hello add file share button](./media/storage-how-to-create-file-share/create-file-share-portal2.png)</span></span>

3. <span data-ttu-id="215bb-117">**Adja meg a nevet és a kvótát. A kvóta jelenleg legfeljebb 5 TB lehet**:</span><span class="sxs-lookup"><span data-stu-id="215bb-117">**Provide Name and Quota. Quota currently can be maximum 5TB**:</span></span>    
    <span data-ttu-id="215bb-118">![Adjon meg egy nevet és a kívánt kvóta hello Új fájlmegosztás](./media/storage-how-to-create-file-share/create-file-share-portal3.png)</span><span class="sxs-lookup"><span data-stu-id="215bb-118">![Provide a name and a desired quota for hello new file share](./media/storage-how-to-create-file-share/create-file-share-portal3.png)</span></span>

4. <span data-ttu-id="215bb-119">**Tekintse meg az új fájlmegosztást**: ![Az új fájlmegosztás megtekintése](./media/storage-how-to-create-file-share/create-file-share-portal4.png)</span><span class="sxs-lookup"><span data-stu-id="215bb-119">**View your new file share**:  ![View your new file share](./media/storage-how-to-create-file-share/create-file-share-portal4.png)</span></span>

5. <span data-ttu-id="215bb-120">**Töltsön fel egy fájlt**: ![Fájl feltöltése](./media/storage-how-to-create-file-share/create-file-share-portal5.png)</span><span class="sxs-lookup"><span data-stu-id="215bb-120">**Upload a file**:  ![Upload a file](./media/storage-how-to-create-file-share/create-file-share-portal5.png)</span></span>

6. <span data-ttu-id="215bb-121">**Tallózzon az új fájlmegosztásban, és kezelje könyvtárait és fájljait**: ![Fájlmegosztás tallózása](./media/storage-how-to-create-file-share/create-file-share-portal6.png)</span><span class="sxs-lookup"><span data-stu-id="215bb-121">**Browse into your file share and manage your directories and files**:  ![Browse file share](./media/storage-how-to-create-file-share/create-file-share-portal6.png)</span></span>


## <a name="create-file-share-through-powershell"></a><span data-ttu-id="215bb-122">Fájlmegosztás létrehozása PowerShell-lel</span><span class="sxs-lookup"><span data-stu-id="215bb-122">Create file share through PowerShell</span></span>
<span data-ttu-id="215bb-123">tooprepare toouse PowerShell, töltse le és telepítse hello Azure PowerShell-parancsmagokat.</span><span class="sxs-lookup"><span data-stu-id="215bb-123">tooprepare toouse PowerShell, download and install hello Azure PowerShell cmdlets.</span></span> <span data-ttu-id="215bb-124">Lásd: [hogyan tooinstall és konfigurálja az Azure Powershellt](https://azure.microsoft.com/documentation/articles/powershell-install-configure/) hello telepítse pont és a telepítési utasításokat.</span><span class="sxs-lookup"><span data-stu-id="215bb-124">See [How tooinstall and configure Azure PowerShell](https://azure.microsoft.com/documentation/articles/powershell-install-configure/) for hello install point and installation instructions.</span></span>

> [!Note]  
> <span data-ttu-id="215bb-125">Javasoljuk, hogy letöltése és telepítése vagy frissítése toohello legújabb Azure PowerShell modul.</span><span class="sxs-lookup"><span data-stu-id="215bb-125">It's recommended that you download and install or upgrade toohello latest Azure PowerShell module.</span></span>

1. <span data-ttu-id="215bb-126">**A tárfiók és a kulcs környezet létrehozása** hello környezet magában foglalja a hello tárfiók neve és a fiók kulcsot.</span><span class="sxs-lookup"><span data-stu-id="215bb-126">**Create a context for your storage account and key** hello context encapsulates hello storage account name and account key.</span></span> <span data-ttu-id="215bb-127">Útmutatás a fiókkulcs átmásolásához az [Azure Portalról](https://portal.azure.com/): [A tárelérési kulcs megtekintése és másolása](../common/storage-create-storage-account.md?toc=%2fazure%2fstorage%2ffiles%2ftoc.json#view-and-copy-storage-access-keys).</span><span class="sxs-lookup"><span data-stu-id="215bb-127">For instructions on copying your account key from the [Azure portal](https://portal.azure.com/), see [View and copy storage access keys](../common/storage-create-storage-account.md?toc=%2fazure%2fstorage%2ffiles%2ftoc.json#view-and-copy-storage-access-keys).</span></span>

    ```powershell
    $storageContext = New-AzureStorageContext <storage-account-name> <storage-account-key>
    ```
    
2. <span data-ttu-id="215bb-128">**Hozzon létre egy új fájlmegosztást**:</span><span class="sxs-lookup"><span data-stu-id="215bb-128">**Create a new file share**:</span></span>    
    
    ```powershell
    $share = New-AzureStorageShare logs -Context $storageContext
    ```

> [!Note]  
> <span data-ttu-id="215bb-129">a fájlmegosztás nevét hello összes kisbetűnek kell lennie.</span><span class="sxs-lookup"><span data-stu-id="215bb-129">hello name of your file share must be all lowercase.</span></span> <span data-ttu-id="215bb-130">A fájlmegosztások és fájlok elnevezésére vonatkozó információkért lásd: [Naming and Referencing Shares, Directories, Files, and Metadata](https://msdn.microsoft.com/library/azure/dn167011.aspx) (Megosztások, könyvtárak, fájlok és metaadatok elnevezése és hivatkozása).</span><span class="sxs-lookup"><span data-stu-id="215bb-130">For complete details about naming file shares and files, see [Naming and Referencing Shares, Directories, Files, and Metadata](https://msdn.microsoft.com/library/azure/dn167011.aspx).</span></span>

## <a name="create-file-share-through-command-line-interface-cli"></a><span data-ttu-id="215bb-131">Fájlmegosztás létrehozása parancssori felület (CLI) használatával</span><span class="sxs-lookup"><span data-stu-id="215bb-131">Create file share through Command Line Interface (CLI)</span></span>
1. <span data-ttu-id="215bb-132">**tooprepare toouse parancssori felület (CLI) letöltése, és hello Azure parancssori felület telepítése.**</span><span class="sxs-lookup"><span data-stu-id="215bb-132">**tooprepare toouse a Command Line Interface (CLI), download and install hello Azure CLI.**</span></span>  
    <span data-ttu-id="215bb-133">Lásd: [Az Azure CLI 2.0-s verziójának telepítése](/cli/azure/install-az-cli2.md) és [Bevezetés az Azure CLI 2.0-s verziójának használatába](/cli/azure/get-started-with-azure-cli.md).</span><span class="sxs-lookup"><span data-stu-id="215bb-133">See [Install Azure CLI 2.0](/cli/azure/install-az-cli2.md) and [Get started with Azure CLI 2.0](/cli/azure/get-started-with-azure-cli.md).</span></span>

2. <span data-ttu-id="215bb-134">**Hozzon létre egy kapcsolati karakterlánc toohello tárfiókot toocreate hello megosztást, ahová.**</span><span class="sxs-lookup"><span data-stu-id="215bb-134">**Create a connection string toohello storage account where you want toocreate hello share.**</span></span>  
    <span data-ttu-id="215bb-135">Cserélje le ```<storage-account>``` és ```<resource_group>``` az a fiók nevét és az erőforrás tárolócsoportot az alábbi példa hello.</span><span class="sxs-lookup"><span data-stu-id="215bb-135">Replace ```<storage-account>``` and ```<resource_group>``` with your storage account name and resource group in hello following example.</span></span>

   ```azurecli
    current_env_conn_string = $(az storage account show-connection-string -n <storage-account> -g <resource-group> --query 'connectionString' -o tsv)

    if [[ $current_env_conn_string == "" ]]; then  
        echo "Couldn't retrieve hello connection string."
    fi
    ```

3. <span data-ttu-id="215bb-136">**Fájlmegosztás létrehozása**</span><span class="sxs-lookup"><span data-stu-id="215bb-136">**Create file share**</span></span>
    ```azurecli
    az storage share create --name files --quota 2048 --connection-string $current_env_conn_string 1 > /dev/null
    ```

## <a name="next-steps"></a><span data-ttu-id="215bb-137">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="215bb-137">Next steps</span></span>
* [<span data-ttu-id="215bb-138">Fájlmegosztás csatlakoztatása – Windows</span><span class="sxs-lookup"><span data-stu-id="215bb-138">Connect and Mount File Share - Windows</span></span>](storage-how-to-use-files-windows.md)
* [<span data-ttu-id="215bb-139">Fájlmegosztás csatlakoztatása – Linux</span><span class="sxs-lookup"><span data-stu-id="215bb-139">Connect and Mount File Share - Linux</span></span>](../storage-how-to-use-files-linux.md)
* [<span data-ttu-id="215bb-140">Fájlmegosztás csatlakoztatása – macOS</span><span class="sxs-lookup"><span data-stu-id="215bb-140">Connect and Mount File Share - macOS</span></span>](storage-how-to-use-files-mac.md)

<span data-ttu-id="215bb-141">Az alábbi hivatkozások további információkat tartalmaznak az Azure File Storage-ról.</span><span class="sxs-lookup"><span data-stu-id="215bb-141">See these links for more information about Azure File storage.</span></span>

* [<span data-ttu-id="215bb-142">Gyakori kérdések</span><span class="sxs-lookup"><span data-stu-id="215bb-142">FAQ</span></span>](../storage-files-faq.md)
* [<span data-ttu-id="215bb-143">Hibaelhárítás a Windows rendszerben</span><span class="sxs-lookup"><span data-stu-id="215bb-143">Troubleshooting on Windows</span></span>](storage-troubleshoot-windows-file-connection-problems.md)      
* [<span data-ttu-id="215bb-144">Hibaelhárítás a Linux rendszerben</span><span class="sxs-lookup"><span data-stu-id="215bb-144">Troubleshooting on Linux</span></span>](storage-troubleshoot-linux-file-connection-problems.md)   