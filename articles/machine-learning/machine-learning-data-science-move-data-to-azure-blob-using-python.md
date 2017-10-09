---
title: "az Azure Blob Storage használatával Python aaaMove adatok tooand |} Microsoft Docs"
description: "Adatok tooand áthelyezése az Azure Blob Storage pythonos környezetekben"
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
ms.openlocfilehash: c2be9600e0d6cb05bcf4109a8d554db522704ecb
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="move-data-tooand-from-azure-blob-storage-using-python"></a><span data-ttu-id="dca7e-103">Adatok tooand áthelyezése az Azure Blob Storage pythonos környezetekben</span><span class="sxs-lookup"><span data-stu-id="dca7e-103">Move data tooand from Azure Blob Storage using Python</span></span>
<span data-ttu-id="dca7e-104">Ez a témakör ismerteti, hogyan toolist, töltse fel, és töltse le a blobok hello Python API használatával.</span><span class="sxs-lookup"><span data-stu-id="dca7e-104">This topic describes how toolist, upload, and download blobs using hello Python API.</span></span> <span data-ttu-id="dca7e-105">Az Azure SDK-ban megadott Python API hello a következőket teheti:</span><span class="sxs-lookup"><span data-stu-id="dca7e-105">With hello Python API provided in Azure SDK, you can:</span></span>

* <span data-ttu-id="dca7e-106">Tároló létrehozása</span><span class="sxs-lookup"><span data-stu-id="dca7e-106">Create a container</span></span>
* <span data-ttu-id="dca7e-107">Blobok feltöltése a tárolóba</span><span class="sxs-lookup"><span data-stu-id="dca7e-107">Upload a blob into a container</span></span>
* <span data-ttu-id="dca7e-108">Blobok letöltése</span><span class="sxs-lookup"><span data-stu-id="dca7e-108">Download blobs</span></span>
* <span data-ttu-id="dca7e-109">Lista hello a tárolóban lévő blobok</span><span class="sxs-lookup"><span data-stu-id="dca7e-109">List hello blobs in a container</span></span>
* <span data-ttu-id="dca7e-110">Blob törlése</span><span class="sxs-lookup"><span data-stu-id="dca7e-110">Delete a blob</span></span>

<span data-ttu-id="dca7e-111">Hello Python API használatával kapcsolatos további információkért lásd: [hogyan tooUse hello Blob Storage szolgáltatással való Python](../storage/blobs/storage-python-how-to-use-blob-storage.md).</span><span class="sxs-lookup"><span data-stu-id="dca7e-111">For more information about using hello Python API, see [How tooUse hello Blob Storage Service from Python](../storage/blobs/storage-python-how-to-use-blob-storage.md).</span></span>

[!INCLUDE [blob-storage-tool-selector](../../includes/machine-learning-blob-storage-tool-selector.md)]

> [!NOTE]
> <span data-ttu-id="dca7e-112">Ha virtuális Gépet, amely hello parancsfájlok által biztosított be lett állítva az [adatok tudományos virtuális gépek Azure-ban](machine-learning-data-science-virtual-machines.md), akkor az AzCopy már telepítve van a virtuális gép hello.</span><span class="sxs-lookup"><span data-stu-id="dca7e-112">If you are using VM that was set up with hello scripts provided by [Data Science Virtual machines in Azure](machine-learning-data-science-virtual-machines.md), then AzCopy is already installed on hello VM.</span></span>
> 
> [!NOTE]
> <span data-ttu-id="dca7e-113">A teljes bemutatása tooAzure blob-tároló, tekintse meg túl[Azure Blob alapjai](../storage/blobs/storage-dotnet-how-to-use-blobs.md) és túl[Azure Blob szolgáltatás](https://msdn.microsoft.com/library/azure/dd179376.aspx).</span><span class="sxs-lookup"><span data-stu-id="dca7e-113">For a complete introduction tooAzure blob storage, refer too[Azure Blob Basics](../storage/blobs/storage-dotnet-how-to-use-blobs.md) and too[Azure Blob Service](https://msdn.microsoft.com/library/azure/dd179376.aspx).</span></span>
> 
> 

## <a name="prerequisites"></a><span data-ttu-id="dca7e-114">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="dca7e-114">Prerequisites</span></span>
<span data-ttu-id="dca7e-115">Jelen dokumentum céljából feltételezzük, hogy az Azure-előfizetéssel, a tárfiók és hello megfelelő kulcs fiók rendelkezik.</span><span class="sxs-lookup"><span data-stu-id="dca7e-115">This document assumes that you have an Azure subscription, a storage account, and hello corresponding storage key for that account.</span></span> <span data-ttu-id="dca7e-116">Adatok feltöltése/letöltése, előtt ismernie kell az Azure storage-fiók nevét és a fiók kulcs.</span><span class="sxs-lookup"><span data-stu-id="dca7e-116">Before uploading/downloading data, you must know your Azure storage account name and account key.</span></span>

* <span data-ttu-id="dca7e-117">tooset be Azure-előfizetéssel, lásd: [ingyenes egy hónapos próbaverzió](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="dca7e-117">tooset up an Azure subscription, see [Free one-month trial](https://azure.microsoft.com/pricing/free-trial/).</span></span>
* <span data-ttu-id="dca7e-118">A storage-fiók létrehozásával és az első fiók és a fontos információkat lásd: [tudnivalók az Azure storage-fiókok](../storage/common/storage-create-storage-account.md).</span><span class="sxs-lookup"><span data-stu-id="dca7e-118">For instructions on creating a storage account and for getting account and key information, see [About Azure storage accounts](../storage/common/storage-create-storage-account.md).</span></span>

## <a name="upload-data-tooblob"></a><span data-ttu-id="dca7e-119">Adatok tooBlob feltöltése</span><span class="sxs-lookup"><span data-stu-id="dca7e-119">Upload Data tooBlob</span></span>
<span data-ttu-id="dca7e-120">Adja hozzá a következő kódrészletet, amelyben tooprogrammatically access Azure Storage kívánja Python kódok hello tetején hello:</span><span class="sxs-lookup"><span data-stu-id="dca7e-120">Add hello following snippet near hello top of any Python code in which you wish tooprogrammatically access Azure Storage:</span></span>

    from azure.storage.blob import BlobService

<span data-ttu-id="dca7e-121">Hello **BlobService** objektum lehetővé teszi, hogy a tárolók és blobok.</span><span class="sxs-lookup"><span data-stu-id="dca7e-121">hello **BlobService** object lets you work with containers and blobs.</span></span> <span data-ttu-id="dca7e-122">a következő kód hello objektumot hoz létre BlobService hello tárfiók neve és a fiók kulcsot használ.</span><span class="sxs-lookup"><span data-stu-id="dca7e-122">hello following code creates a BlobService object using hello storage account name and account key.</span></span> <span data-ttu-id="dca7e-123">Fiók neve és a fiókkulcsot cserélje le a valódi fiókot és kulcsot.</span><span class="sxs-lookup"><span data-stu-id="dca7e-123">Replace account name and account key with your real account and key.</span></span>

    blob_service = BlobService(account_name="<your_account_name>", account_key="<your_account_key>")

<span data-ttu-id="dca7e-124">A következő módszerek tooupload adatblobja tooa hello használata:</span><span class="sxs-lookup"><span data-stu-id="dca7e-124">Use hello following methods tooupload data tooa blob:</span></span>

1. <span data-ttu-id="dca7e-125">PUT\_blokk\_blob\_a\_(feltölt egy fájl megadott elérési hello hello tartalmát) elérési útja</span><span class="sxs-lookup"><span data-stu-id="dca7e-125">put\_block\_blob\_from\_path (uploads hello contents of a file from hello specified path)</span></span>
2. <span data-ttu-id="dca7e-126">PUT\_block_blob\_a\_(feltölt egy már megnyitott fájl vagy adatfolyam tartalmát hello) fájlt</span><span class="sxs-lookup"><span data-stu-id="dca7e-126">put\_block_blob\_from\_file (uploads hello contents from an already opened file/stream)</span></span>
3. <span data-ttu-id="dca7e-127">PUT\_blokk\_blob\_a\_bájt (feltöltések egy bájttömb)</span><span class="sxs-lookup"><span data-stu-id="dca7e-127">put\_block\_blob\_from\_bytes (uploads an array of bytes)</span></span>
4. <span data-ttu-id="dca7e-128">PUT\_blokk\_blob\_a\_szöveg (megadott hello feltölt hello használatával szöveges értéket a megadott kódolás)</span><span class="sxs-lookup"><span data-stu-id="dca7e-128">put\_block\_blob\_from\_text (uploads hello specified text value using hello specified encoding)</span></span>

<span data-ttu-id="dca7e-129">a következő példakód hello feltölt egy helyi fájl tooa tárolót:</span><span class="sxs-lookup"><span data-stu-id="dca7e-129">hello following sample code uploads a local file tooa container:</span></span>

    blob_service.put_block_blob_from_path("<your_container_name>", "<your_blob_name>", "<your_local_file_name>")

<span data-ttu-id="dca7e-130">hello alábbi mintakód feltölt (kivéve a könyvtárak) minden hello fájlok helyi könyvtár tooblob tárolási:</span><span class="sxs-lookup"><span data-stu-id="dca7e-130">hello following sample code uploads all hello files (excluding directories) in a local directory tooblob storage:</span></span>

    from azure.storage.blob import BlobService
    from os import listdir
    from os.path import isfile, join

    # Set parameters here
    ACCOUNT_NAME = "<your_account_name>"
    ACCOUNT_KEY = "<your_account_key>"
    CONTAINER_NAME = "<your_container_name>"
    LOCAL_DIRECT = "<your_local_directory>"        

    blob_service = BlobService(account_name=ACCOUNT_NAME, account_key=ACCOUNT_KEY)
    # find all files in hello LOCAL_DIRECT (excluding directory)
    local_file_list = [f for f in listdir(LOCAL_DIRECT) if isfile(join(LOCAL_DIRECT, f))]

    file_num = len(local_file_list)
    for i in range(file_num):
        local_file = join(LOCAL_DIRECT, local_file_list[i])
        blob_name = local_file_list[i]
        try:
            blob_service.put_block_blob_from_path(CONTAINER_NAME, blob_name, local_file)
        except:
            print "something wrong happened when uploading hello data %s"%blob_name


## <a name="download-data-from-blob"></a><span data-ttu-id="dca7e-131">A Blob adatok letöltése</span><span class="sxs-lookup"><span data-stu-id="dca7e-131">Download Data from Blob</span></span>
<span data-ttu-id="dca7e-132">Módszerek toodownload adatokat a blob a következő hello használata:</span><span class="sxs-lookup"><span data-stu-id="dca7e-132">Use hello following methods toodownload data from a blob:</span></span>

1. <span data-ttu-id="dca7e-133">első\_blob\_való\_elérési útja</span><span class="sxs-lookup"><span data-stu-id="dca7e-133">get\_blob\_to\_path</span></span>
2. <span data-ttu-id="dca7e-134">első\_blob\_való\_fájl</span><span class="sxs-lookup"><span data-stu-id="dca7e-134">get\_blob\_to\_file</span></span>
3. <span data-ttu-id="dca7e-135">első\_blob\_való\_bájt</span><span class="sxs-lookup"><span data-stu-id="dca7e-135">get\_blob\_to\_bytes</span></span>
4. <span data-ttu-id="dca7e-136">első\_blob\_való\_szöveg</span><span class="sxs-lookup"><span data-stu-id="dca7e-136">get\_blob\_to\_text</span></span>

<span data-ttu-id="dca7e-137">Ezek a módszerek végző hello szükséges adattömbösítő Ha hello hello adatok mérete túllépi a 64 MB.</span><span class="sxs-lookup"><span data-stu-id="dca7e-137">These methods that perform hello necessary chunking when hello size of hello data exceeds 64 MB.</span></span>

<span data-ttu-id="dca7e-138">hello alábbi mintakód letölti a blob tároló tooa helyi fájlba hello tartalmát:</span><span class="sxs-lookup"><span data-stu-id="dca7e-138">hello following sample code downloads hello contents of a blob in a container tooa local file:</span></span>

    blob_service.get_blob_to_path("<your_container_name>", "<your_blob_name>", "<your_local_file_name>")

<span data-ttu-id="dca7e-139">hello alábbi mintakód letölti összes BLOB egy tárolót.</span><span class="sxs-lookup"><span data-stu-id="dca7e-139">hello following sample code downloads all blobs from a container.</span></span> <span data-ttu-id="dca7e-140">Lista használ\_blobok érhető el, hello tárolóban lévő blobok tooget hello listáját, és letölti azokat tooa helyi könyvtárba.</span><span class="sxs-lookup"><span data-stu-id="dca7e-140">It uses list\_blobs tooget hello list of available blobs in hello container and downloads them tooa local directory.</span></span>

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
            print "something wrong happened when downloading hello data %s"%blob.name
