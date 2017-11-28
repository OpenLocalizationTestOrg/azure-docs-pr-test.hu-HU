---
title: "Azure-fájlmegosztás létrehozása | Microsoft Docs"
description: "Azure-fájlmegosztás létrehozása az Azure File Storage szolgáltatásban az Azure Portal, PowerShell és az Azure CLI használatával."
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
ms.openlocfilehash: 10da4d903eaab290a6cca2c4f674548a43a70c53
ms.sourcegitcommit: 422efcbac5b6b68295064bd545132fcc98349d01
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/29/2017
---
# <a name="create-a-file-share-in-azure-file-storage"></a><span data-ttu-id="2ce47-103">Fájlmegosztás létrehozása az Azure File Storage-ban</span><span class="sxs-lookup"><span data-stu-id="2ce47-103">Create a File Share in Azure File storage</span></span>
<span data-ttu-id="2ce47-104">Azure fájlmegosztásokat létrehozhat az [Azure Portalon](https://portal.azure.com/), az Azure Storage PowerShell parancsmagjainak segítségével, illetve az Azure Storage ügyfélkódtáraival vagy az Azure Storage REST API-val. Ez a oktatóanyagban az alábbiakat ismerheti meg:</span><span class="sxs-lookup"><span data-stu-id="2ce47-104">You can create Azure File shares using [Azure portal](https://portal.azure.com/), the Azure Storage PowerShell cmdlets, the Azure Storage client libraries, or the Azure Storage REST API.In this tutorial, you will learn:</span></span>
* [<span data-ttu-id="2ce47-105">Azure-fájlmegosztás létrehozása az Azure Portal használatával</span><span class="sxs-lookup"><span data-stu-id="2ce47-105">How to create an Azure File share using the Azure portal</span></span>](#Create file share through the Portal)
* [<span data-ttu-id="2ce47-106">Azure-fájlmegosztás létrehozása PowerShell használatával</span><span class="sxs-lookup"><span data-stu-id="2ce47-106">How to create an Azure File share using Powershell</span></span>](#Create file share using PowerShell)
* [<span data-ttu-id="2ce47-107">Azure-fájlmegosztás létrehozása parancssori felület használatával</span><span class="sxs-lookup"><span data-stu-id="2ce47-107">How to create an Azure File share using CLI</span></span>](#create-file-share-using-command-line-interface-cli)

## <a name="prerequisites"></a><span data-ttu-id="2ce47-108">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="2ce47-108">Prerequisites</span></span>
<span data-ttu-id="2ce47-109">Azure-fájlmegosztás létrehozásához használhat már létező tárfiókot, vagy [létrehozhat egy új Azure-tárfiókot](storage-create-storage-account.md).</span><span class="sxs-lookup"><span data-stu-id="2ce47-109">To create an Azure File share, you can use a storage account that already exists, or [create a new Azure storage account](storage-create-storage-account.md).</span></span> <span data-ttu-id="2ce47-110">Ha PowerShell-lel szeretne Azure-fájlmegosztást létrehozni, szüksége lesz a fiókkulcsra és a tárfiók nevére.</span><span class="sxs-lookup"><span data-stu-id="2ce47-110">To create an Azure File share with PowerShell, you will need the account key and name of your storage account..</span></span> <span data-ttu-id="2ce47-111">PowerShell vagy parancssori felület használata esetén szüksége lesz a tárfiók kulcsára.</span><span class="sxs-lookup"><span data-stu-id="2ce47-111">You will need Storage account key if you plan to use Powershell or CLI.</span></span>

## <a name="create-file-share-through-the-portal"></a><span data-ttu-id="2ce47-112">Fájlmegosztás létrehozása a Portalon keresztül</span><span class="sxs-lookup"><span data-stu-id="2ce47-112">Create file share through the Portal</span></span>
1. <span data-ttu-id="2ce47-113">**Ugorjon az Azure Portal Tárfiók paneljére**:</span><span class="sxs-lookup"><span data-stu-id="2ce47-113">**Go to Storage Account blade on Azure Portal**:</span></span>    
    <span data-ttu-id="2ce47-114">![Tárfiók panel](media/storage-file-how-to-create-file-share/create-file-share-portal1.png)</span><span class="sxs-lookup"><span data-stu-id="2ce47-114">![Storage Account Blade](media/storage-file-how-to-create-file-share/create-file-share-portal1.png)</span></span>

2. <span data-ttu-id="2ce47-115">**Kattintson a Fájlmegosztás hozzáadása gombra**:</span><span class="sxs-lookup"><span data-stu-id="2ce47-115">**Click on add File Share button**:</span></span>    
    <span data-ttu-id="2ce47-116">![Kattintson a fájlmegosztás hozzáadása gombra](media/storage-file-how-to-create-file-share/create-file-share-portal2.png)</span><span class="sxs-lookup"><span data-stu-id="2ce47-116">![Click the add file share button](media/storage-file-how-to-create-file-share/create-file-share-portal2.png)</span></span>

3. <span data-ttu-id="2ce47-117">**Adja meg a nevet és a kvótát. A kvóta jelenleg legfeljebb 5 TB lehet**:</span><span class="sxs-lookup"><span data-stu-id="2ce47-117">**Provide Name and Quota. Quota currently can be maximum 5TB**:</span></span>    
    <span data-ttu-id="2ce47-118">![Adjon meg egy nevet és egy kívánt kvótát az új fájlmegosztás számára](media/storage-file-how-to-create-file-share/create-file-share-portal3.png)</span><span class="sxs-lookup"><span data-stu-id="2ce47-118">![Provide a name and a desired quota for the new file share](media/storage-file-how-to-create-file-share/create-file-share-portal3.png)</span></span>

4. <span data-ttu-id="2ce47-119">**Tekintse meg az új fájlmegosztást**: ![Az új fájlmegosztás megtekintése](media/storage-file-how-to-create-file-share/create-file-share-portal4.png)</span><span class="sxs-lookup"><span data-stu-id="2ce47-119">**View your new file share**:  ![View your new file share](media/storage-file-how-to-create-file-share/create-file-share-portal4.png)</span></span>

5. <span data-ttu-id="2ce47-120">**Töltsön fel egy fájlt**: ![Fájl feltöltése](media/storage-file-how-to-create-file-share/create-file-share-portal5.png)</span><span class="sxs-lookup"><span data-stu-id="2ce47-120">**Upload a file**:  ![Upload a file](media/storage-file-how-to-create-file-share/create-file-share-portal5.png)</span></span>

6. <span data-ttu-id="2ce47-121">**Tallózzon az új fájlmegosztásban, és kezelje könyvtárait és fájljait**: ![Fájlmegosztás tallózása](media/storage-file-how-to-create-file-share/create-file-share-portal6.png)</span><span class="sxs-lookup"><span data-stu-id="2ce47-121">**Browse into your file share and manage your directories and files**:  ![Browse file share](media/storage-file-how-to-create-file-share/create-file-share-portal6.png)</span></span>


## <a name="create-file-share-through-powershell"></a><span data-ttu-id="2ce47-122">Fájlmegosztás létrehozása PowerShell-lel</span><span class="sxs-lookup"><span data-stu-id="2ce47-122">Create file share through PowerShell</span></span>
<span data-ttu-id="2ce47-123">A PowerShell használatának előkészítéseként töltse le és telepítse az Azure PowerShell-parancsmagokat.</span><span class="sxs-lookup"><span data-stu-id="2ce47-123">To prepare to use PowerShell, download and install the Azure PowerShell cmdlets.</span></span> <span data-ttu-id="2ce47-124">A telepítési helyre és a telepítésre vonatkozó utasításokért lásd: [How to install and configure Azure PowerShell](https://azure.microsoft.com/documentation/articles/powershell-install-configure/) (Az Azure PowerShell telepítése és konfigurálása).</span><span class="sxs-lookup"><span data-stu-id="2ce47-124">See [How to install and configure Azure PowerShell](https://azure.microsoft.com/documentation/articles/powershell-install-configure/) for the install point and installation instructions.</span></span>

> [!Note]  
> <span data-ttu-id="2ce47-125">Javasoljuk, hogy frissítsen a legújabb Azure PowerShell modulra, vagy töltse le és telepítse azt.</span><span class="sxs-lookup"><span data-stu-id="2ce47-125">It's recommended that you download and install or upgrade to the latest Azure PowerShell module.</span></span>

1. <span data-ttu-id="2ce47-126">**Hozzon létre egy környezetet a tárfiókhoz és a fiókkulcshoz** A környezet magában foglalja a tárfiók nevét és a fiókkulcsot.</span><span class="sxs-lookup"><span data-stu-id="2ce47-126">**Create a context for your storage account and key** The context encapsulates the storage account name and account key.</span></span> <span data-ttu-id="2ce47-127">Útmutatás a fiókkulcs átmásolásához az [Azure Portalról](https://portal.azure.com/): [A tárelérési kulcs megtekintése és másolása](storage-create-storage-account.md#view-and-copy-storage-access-keys).</span><span class="sxs-lookup"><span data-stu-id="2ce47-127">For instructions on copying your account key from the [Azure portal](https://portal.azure.com/), see [View and copy storage access keys](storage-create-storage-account.md#view-and-copy-storage-access-keys).</span></span>

    ```powershell
    $storageContext = New-AzureStorageContext <storage-account-name> <storage-account-key>
    ```
    
2. <span data-ttu-id="2ce47-128">**Hozzon létre egy új fájlmegosztást**:</span><span class="sxs-lookup"><span data-stu-id="2ce47-128">**Create a new file share**:</span></span>    
    
    ```powershell
    $share = New-AzureStorageShare logs -Context $storageContext
    ```

> [!Note]  
> <span data-ttu-id="2ce47-129">A fájlmegosztás nevében csak kisbetű szerepelhet.</span><span class="sxs-lookup"><span data-stu-id="2ce47-129">The name of your file share must be all lowercase.</span></span> <span data-ttu-id="2ce47-130">A fájlmegosztások és fájlok elnevezésére vonatkozó információkért lásd: [Naming and Referencing Shares, Directories, Files, and Metadata](https://msdn.microsoft.com/library/azure/dn167011.aspx) (Megosztások, könyvtárak, fájlok és metaadatok elnevezése és hivatkozása).</span><span class="sxs-lookup"><span data-stu-id="2ce47-130">For complete details about naming file shares and files, see [Naming and Referencing Shares, Directories, Files, and Metadata](https://msdn.microsoft.com/library/azure/dn167011.aspx).</span></span>

## <a name="create-file-share-through-command-line-interface-cli"></a><span data-ttu-id="2ce47-131">Fájlmegosztás létrehozása parancssori felület (CLI) használatával</span><span class="sxs-lookup"><span data-stu-id="2ce47-131">Create file share through Command Line Interface (CLI)</span></span>
1. <span data-ttu-id="2ce47-132">**A parancssori felület (CLI) használatának előkészítéseként töltse le és telepítse az Azure CLI-t.**</span><span class="sxs-lookup"><span data-stu-id="2ce47-132">**To prepare to use a Command Line Interface (CLI), download and install the Azure CLI.**</span></span>  
    <span data-ttu-id="2ce47-133">Lásd: [Az Azure CLI 2.0-s verziójának telepítése](/cli/azure/install-az-cli2.md) és [Bevezetés az Azure CLI 2.0-s verziójának használatába](/cli/azure/get-started-with-azure-cli.md).</span><span class="sxs-lookup"><span data-stu-id="2ce47-133">See [Install Azure CLI 2.0](/cli/azure/install-az-cli2.md) and [Get started with Azure CLI 2.0](/cli/azure/get-started-with-azure-cli.md).</span></span>

2. <span data-ttu-id="2ce47-134">**Hozzon létre egy kapcsolati karakterláncot ahhoz a tárfiókhoz, amelyen létre szeretné hozni a megosztást.**</span><span class="sxs-lookup"><span data-stu-id="2ce47-134">**Create a connection string to the storage account where you want to create the share.**</span></span>  
    <span data-ttu-id="2ce47-135">Az alábbi példában cserélje ki a ```<storage-account>``` és a ```<resource_group>``` elemet a tárfiók nevére és erőforráscsoportjára.</span><span class="sxs-lookup"><span data-stu-id="2ce47-135">Replace ```<storage-account>``` and ```<resource_group>``` with your storage account name and resource group in the following example.</span></span>

   ```azurecli
    current_env_conn_string = $(az storage account show-connection-string -n <storage-account> -g <resource-group> --query 'connectionString' -o tsv)

    if [[ $current_env_conn_string == "" ]]; then  
        echo "Couldn't retrieve the connection string."
    fi
    ```

3. <span data-ttu-id="2ce47-136">**Fájlmegosztás létrehozása**</span><span class="sxs-lookup"><span data-stu-id="2ce47-136">**Create file share**</span></span>
    ```azurecli
    az storage share create --name files --quota 2048 --connection-string $current_env_conn_string 1 > /dev/null
    ```

## <a name="next-steps"></a><span data-ttu-id="2ce47-137">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="2ce47-137">Next steps</span></span>
* [<span data-ttu-id="2ce47-138">Fájlmegosztás csatlakoztatása – Windows</span><span class="sxs-lookup"><span data-stu-id="2ce47-138">Connect and Mount File Share - Windows</span></span>](storage-file-how-to-use-files-windows.md)
* [<span data-ttu-id="2ce47-139">Fájlmegosztás csatlakoztatása – Linux</span><span class="sxs-lookup"><span data-stu-id="2ce47-139">Connect and Mount File Share - Linux</span></span>](storage-how-to-use-files-linux.md)
* [<span data-ttu-id="2ce47-140">Fájlmegosztás csatlakoztatása – macOS</span><span class="sxs-lookup"><span data-stu-id="2ce47-140">Connect and Mount File Share - macOS</span></span>](storage-file-how-to-use-files-mac.md)

<span data-ttu-id="2ce47-141">Az alábbi hivatkozások további információkat tartalmaznak az Azure File Storage-ról.</span><span class="sxs-lookup"><span data-stu-id="2ce47-141">See these links for more information about Azure File storage.</span></span>

* [<span data-ttu-id="2ce47-142">Gyakori kérdések</span><span class="sxs-lookup"><span data-stu-id="2ce47-142">FAQ</span></span>](storage-files-faq.md)
* [<span data-ttu-id="2ce47-143">hibaelhárítással</span><span class="sxs-lookup"><span data-stu-id="2ce47-143">Troubleshooting</span></span>](storage-troubleshoot-file-connection-problems.md)