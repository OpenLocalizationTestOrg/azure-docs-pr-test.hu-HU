---
title: "Adatok áthelyezése, és az Azure Blob Storage használatával Python |} Microsoft Docs"
description: "Adatok áthelyezése Azure Blob Storage-tárolóba vagy onnan máshová Python használatával"
services: machine-learning,storage
documentationcenter: 
author: bradsev
manager: jhubbard
editor: cgronlun
ms.assetid: 24276252-b3dd-4edf-9e5d-f6803f8ccccc
ms.service: machine-learning
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/24/2017
ms.author: bradsev
ms.openlocfilehash: 0eea1ff8e4f4c1d108445e1a1250b6fa8ff48910
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/29/2017
---
# <a name="move-data-to-and-from-azure-blob-storage-using-python"></a><span data-ttu-id="3a124-103">Adatok áthelyezése, és az Azure Blob Storage pythonos környezetekben</span><span class="sxs-lookup"><span data-stu-id="3a124-103">Move data to and from Azure Blob Storage using Python</span></span>
<span data-ttu-id="3a124-104">Ez a témakör ismerteti a listában, töltse fel, és töltse le a blobok a Python API használatával.</span><span class="sxs-lookup"><span data-stu-id="3a124-104">This topic describes how to list, upload, and download blobs using the Python API.</span></span> <span data-ttu-id="3a124-105">A Python API-hoz megadott Azure SDK-t a következőket teheti:</span><span class="sxs-lookup"><span data-stu-id="3a124-105">With the Python API provided in Azure SDK, you can:</span></span>

* <span data-ttu-id="3a124-106">Tároló létrehozása</span><span class="sxs-lookup"><span data-stu-id="3a124-106">Create a container</span></span>
* <span data-ttu-id="3a124-107">Blobok feltöltése a tárolóba</span><span class="sxs-lookup"><span data-stu-id="3a124-107">Upload a blob into a container</span></span>
* <span data-ttu-id="3a124-108">Blobok letöltése</span><span class="sxs-lookup"><span data-stu-id="3a124-108">Download blobs</span></span>
* <span data-ttu-id="3a124-109">A tárolóban lévő blobok listázása</span><span class="sxs-lookup"><span data-stu-id="3a124-109">List the blobs in a container</span></span>
* <span data-ttu-id="3a124-110">Blob törlése</span><span class="sxs-lookup"><span data-stu-id="3a124-110">Delete a blob</span></span>

<span data-ttu-id="3a124-111">A Python API használatával kapcsolatos további információkért lásd: [használata a Blob Storage szolgáltatást Python](../storage/blobs/storage-python-how-to-use-blob-storage.md).</span><span class="sxs-lookup"><span data-stu-id="3a124-111">For more information about using the Python API, see [How to Use the Blob Storage Service from Python](../storage/blobs/storage-python-how-to-use-blob-storage.md).</span></span>

[!INCLUDE [blob-storage-tool-selector](../../includes/machine-learning-blob-storage-tool-selector.md)]

> [!NOTE]
> <span data-ttu-id="3a124-112">Ha virtuális Gépet, amely a parancsfájlok által biztosított be lett állítva az [adatok tudományos virtuális gépek Azure-ban](machine-learning-data-science-virtual-machines.md), majd az AzCopy már telepítve van a virtuális gép.</span><span class="sxs-lookup"><span data-stu-id="3a124-112">If you are using VM that was set up with the scripts provided by [Data Science Virtual machines in Azure](machine-learning-data-science-virtual-machines.md), then AzCopy is already installed on the VM.</span></span>
> 
> [!NOTE]
> <span data-ttu-id="3a124-113">Az Azure blob storage teljes bevezetéséhez hivatkozik [Azure Blob alapjai](../storage/blobs/storage-dotnet-how-to-use-blobs.md) és [Azure Blob szolgáltatás](https://msdn.microsoft.com/library/azure/dd179376.aspx).</span><span class="sxs-lookup"><span data-stu-id="3a124-113">For a complete introduction to Azure blob storage, refer to [Azure Blob Basics](../storage/blobs/storage-dotnet-how-to-use-blobs.md) and to [Azure Blob Service](https://msdn.microsoft.com/library/azure/dd179376.aspx).</span></span>
> 
> 

## <a name="prerequisites"></a><span data-ttu-id="3a124-114">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="3a124-114">Prerequisites</span></span>
<span data-ttu-id="3a124-115">Jelen dokumentum céljából feltételezzük, hogy az Azure-előfizetéssel, a tárfiók és a megfelelő kulcsot adott fiók rendelkezik.</span><span class="sxs-lookup"><span data-stu-id="3a124-115">This document assumes that you have an Azure subscription, a storage account, and the corresponding storage key for that account.</span></span> <span data-ttu-id="3a124-116">Adatok feltöltése/letöltése, előtt ismernie kell az Azure storage-fiók nevét és a fiók kulcs.</span><span class="sxs-lookup"><span data-stu-id="3a124-116">Before uploading/downloading data, you must know your Azure storage account name and account key.</span></span>

* <span data-ttu-id="3a124-117">Állítsa be Azure-előfizetéssel, lásd: [ingyenes egy hónapos próbaverzió](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="3a124-117">To set up an Azure subscription, see [Free one-month trial](https://azure.microsoft.com/pricing/free-trial/).</span></span>
* <span data-ttu-id="3a124-118">A storage-fiók létrehozásával és az első fiók és a fontos információkat lásd: [tudnivalók az Azure storage-fiókok](../storage/common/storage-create-storage-account.md).</span><span class="sxs-lookup"><span data-stu-id="3a124-118">For instructions on creating a storage account and for getting account and key information, see [About Azure storage accounts](../storage/common/storage-create-storage-account.md).</span></span>

## <a name="upload-data-to-blob"></a><span data-ttu-id="3a124-119">Adatok feltöltése a Blob</span><span class="sxs-lookup"><span data-stu-id="3a124-119">Upload Data to Blob</span></span>
<span data-ttu-id="3a124-120">Adja hozzá a következő kódrészletet a Python kódját, amelyben programon keresztüli eléréséhez az Azure Storage kívánja felső részén:</span><span class="sxs-lookup"><span data-stu-id="3a124-120">Add the following snippet near the top of any Python code in which you wish to programmatically access Azure Storage:</span></span>

    from azure.storage.blob import BlobService

<span data-ttu-id="3a124-121">A **BlobService** objektum lehetővé teszi, hogy a tárolók és blobok.</span><span class="sxs-lookup"><span data-stu-id="3a124-121">The **BlobService** object lets you work with containers and blobs.</span></span> <span data-ttu-id="3a124-122">Az alábbi kód létrehoz egy BlobService objektumot, a tárfiók nevét és a fiók kulcsot használ.</span><span class="sxs-lookup"><span data-stu-id="3a124-122">The following code creates a BlobService object using the storage account name and account key.</span></span> <span data-ttu-id="3a124-123">Fiók neve és a fiókkulcsot cserélje le a valódi fiókot és kulcsot.</span><span class="sxs-lookup"><span data-stu-id="3a124-123">Replace account name and account key with your real account and key.</span></span>

    blob_service = BlobService(account_name="<your_account_name>", account_key="<your_account_key>")

<span data-ttu-id="3a124-124">Az alábbi módszerekkel adatok feltöltése a blob:</span><span class="sxs-lookup"><span data-stu-id="3a124-124">Use the following methods to upload data to a blob:</span></span>

1. <span data-ttu-id="3a124-125">PUT\_blokk\_blob\_a\_(feltölt egy fájlt a megadott elérési) elérési útja</span><span class="sxs-lookup"><span data-stu-id="3a124-125">put\_block\_blob\_from\_path (uploads the contents of a file from the specified path)</span></span>
2. <span data-ttu-id="3a124-126">PUT\_block_blob\_a\_(feltölt egy már megnyitott fájl vagy adatfolyam tartalma) fájlt</span><span class="sxs-lookup"><span data-stu-id="3a124-126">put\_block_blob\_from\_file (uploads the contents from an already opened file/stream)</span></span>
3. <span data-ttu-id="3a124-127">PUT\_blokk\_blob\_a\_bájt (feltöltések egy bájttömb)</span><span class="sxs-lookup"><span data-stu-id="3a124-127">put\_block\_blob\_from\_bytes (uploads an array of bytes)</span></span>
4. <span data-ttu-id="3a124-128">PUT\_blokk\_blob\_a\_szöveg (feltölti az adott szöveges értéket a megadott kódolás használatával)</span><span class="sxs-lookup"><span data-stu-id="3a124-128">put\_block\_blob\_from\_text (uploads the specified text value using the specified encoding)</span></span>

<span data-ttu-id="3a124-129">Az alábbi példakód egy tároló feltölt egy helyi fájlt:</span><span class="sxs-lookup"><span data-stu-id="3a124-129">The following sample code uploads a local file to a container:</span></span>

    blob_service.put_block_blob_from_path("<your_container_name>", "<your_blob_name>", "<your_local_file_name>")

<span data-ttu-id="3a124-130">Az alábbi példakód a blob storage egy helyi könyvtárban (kivéve a könyvtárak) minden fájl feltöltését:</span><span class="sxs-lookup"><span data-stu-id="3a124-130">The following sample code uploads all the files (excluding directories) in a local directory to blob storage:</span></span>

    from azure.storage.blob import BlobService
    from os import listdir
    from os.path import isfile, join

    # Set parameters here
    ACCOUNT_NAME = "<your_account_name>"
    ACCOUNT_KEY = "<your_account_key>"
    CONTAINER_NAME = "<your_container_name>"
    LOCAL_DIRECT = "<your_local_directory>"        

    blob_service = BlobService(account_name=ACCOUNT_NAME, account_key=ACCOUNT_KEY)
    # find all files in the LOCAL_DIRECT (excluding directory)
    local_file_list = [f for f in listdir(LOCAL_DIRECT) if isfile(join(LOCAL_DIRECT, f))]

    file_num = len(local_file_list)
    for i in range(file_num):
        local_file = join(LOCAL_DIRECT, local_file_list[i])
        blob_name = local_file_list[i]
        try:
            blob_service.put_block_blob_from_path(CONTAINER_NAME, blob_name, local_file)
        except:
            print "something wrong happened when uploading the data %s"%blob_name


## <a name="download-data-from-blob"></a><span data-ttu-id="3a124-131">A Blob adatok letöltése</span><span class="sxs-lookup"><span data-stu-id="3a124-131">Download Data from Blob</span></span>
<span data-ttu-id="3a124-132">Az alábbi módszerekkel adatok egy blob töltheti le:</span><span class="sxs-lookup"><span data-stu-id="3a124-132">Use the following methods to download data from a blob:</span></span>

1. <span data-ttu-id="3a124-133">első\_blob\_való\_elérési útja</span><span class="sxs-lookup"><span data-stu-id="3a124-133">get\_blob\_to\_path</span></span>
2. <span data-ttu-id="3a124-134">első\_blob\_való\_fájl</span><span class="sxs-lookup"><span data-stu-id="3a124-134">get\_blob\_to\_file</span></span>
3. <span data-ttu-id="3a124-135">első\_blob\_való\_bájt</span><span class="sxs-lookup"><span data-stu-id="3a124-135">get\_blob\_to\_bytes</span></span>
4. <span data-ttu-id="3a124-136">első\_blob\_való\_szöveg</span><span class="sxs-lookup"><span data-stu-id="3a124-136">get\_blob\_to\_text</span></span>

<span data-ttu-id="3a124-137">Ezek a módszerek, hajtsa végre a szükséges adattömbösítő, ha az adatok mérete meghaladja a 64 MB.</span><span class="sxs-lookup"><span data-stu-id="3a124-137">These methods that perform the necessary chunking when the size of the data exceeds 64 MB.</span></span>

<span data-ttu-id="3a124-138">Az alábbi példakód a tárolóban lévő blob tartalmát egy helyi fájl tölti le:</span><span class="sxs-lookup"><span data-stu-id="3a124-138">The following sample code downloads the contents of a blob in a container to a local file:</span></span>

    blob_service.get_blob_to_path("<your_container_name>", "<your_blob_name>", "<your_local_file_name>")

<span data-ttu-id="3a124-139">Az alábbi példakód összes BLOB tölt le egy tárolót.</span><span class="sxs-lookup"><span data-stu-id="3a124-139">The following sample code downloads all blobs from a container.</span></span> <span data-ttu-id="3a124-140">Lista használ\_elérhető blobok listájának lekérdezése a tárolóban lévő blobok és letölti azokat a helyi könyvtárat.</span><span class="sxs-lookup"><span data-stu-id="3a124-140">It uses list\_blobs to get the list of available blobs in the container and downloads them to a local directory.</span></span>

    from azure.storage.blob import BlobService
    from os.path import join

    # Set parameters here
    ACCOUNT_NAME = "<your_account_name>"
    ACCOUNT_KEY = "<your_account_key>"
    CONTAINER_NAME = "<your_container_name>"
    LOCAL_DIRECT = "<your_local_directory>"        

    blob_service = BlobService(account_name=ACCOUNT_NAME, account_key=ACCOUNT_KEY)

    # List all blobs and download them one by one
    blobs = blob_service.list_blobs(CONTAINER_NAME)
    for blob in blobs:
        local_file = join(LOCAL_DIRECT, blob.name)
        try:
            blob_service.get_blob_to_path(CONTAINER_NAME, blob.name, local_file)
        except:
            print "something wrong happened when downloading the data %s"%blob.name
