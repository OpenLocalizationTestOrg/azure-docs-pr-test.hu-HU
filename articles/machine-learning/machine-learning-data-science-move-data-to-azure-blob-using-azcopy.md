---
title: "az Azure Blob Storage használata az AzCopy aaaMove adatok tooand |} Microsoft Docs"
description: "Adatok tooand áthelyezése az Azure Blob Storage AzCopy használatával"
services: machine-learning,storage
documentationcenter: 
author: bradsev
manager: jhubbard
editor: cgronlun
ms.assetid: c309ceb2-0e83-4a07-b16d-c997dcd62d5c
ms.service: machine-learning
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/24/2017
ms.author: bradsev
ms.openlocfilehash: b381ba004ac16879b6c633950d03d13ad82d5b72
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="move-data-tooand-from-azure-blob-storage-using-azcopy"></a><span data-ttu-id="d93fb-103">Adatok tooand áthelyezése az Azure Blob Storage AzCopy használatával</span><span class="sxs-lookup"><span data-stu-id="d93fb-103">Move data tooand from Azure Blob Storage using AzCopy</span></span>
<span data-ttu-id="d93fb-104">AzCopy parancssori segédprogram feltöltése, letöltése és a table storage, a Microsoft Azure blob és a fájl másolása adatok tooand készült.</span><span class="sxs-lookup"><span data-stu-id="d93fb-104">AzCopy is a command-line utility designed for uploading, downloading, and copying data tooand from Microsoft Azure blob, file, and table storage.</span></span>

<span data-ttu-id="d93fb-105">AzCopy és további információkat a használja azt a hello Azure platformon telepítésével kapcsolatos útmutatásért lásd: [Ismerkedés az AzCopy parancssori segédprogram hello](../storage/common/storage-use-azcopy.md).</span><span class="sxs-lookup"><span data-stu-id="d93fb-105">For instructions on installing AzCopy and additional information on using it with hello Azure platform, see [Getting Started with hello AzCopy Command-Line Utility](../storage/common/storage-use-azcopy.md).</span></span>

[!INCLUDE [blob-storage-tool-selector](../../includes/machine-learning-blob-storage-tool-selector.md)]

> [!NOTE]
> <span data-ttu-id="d93fb-106">Ha virtuális Gépet, amely hello parancsfájlok által biztosított be lett állítva az [adatok tudományos virtuális gépek Azure-ban](machine-learning-data-science-virtual-machines.md), akkor az AzCopy már telepítve van a virtuális gép hello.</span><span class="sxs-lookup"><span data-stu-id="d93fb-106">If you are using VM that was set up with hello scripts provided by [Data Science Virtual machines in Azure](machine-learning-data-science-virtual-machines.md), then AzCopy is already installed on hello VM.</span></span>
> 
> [!NOTE]
> <span data-ttu-id="d93fb-107">A teljes bemutatása tooAzure blob-tároló, tekintse meg túl[Azure Blob alapjai](../storage/blobs/storage-dotnet-how-to-use-blobs.md) és túl[Azure Blob szolgáltatás](https://msdn.microsoft.com/library/azure/dd179376.aspx).</span><span class="sxs-lookup"><span data-stu-id="d93fb-107">For a complete introduction tooAzure blob storage, refer too[Azure Blob Basics](../storage/blobs/storage-dotnet-how-to-use-blobs.md) and too[Azure Blob Service](https://msdn.microsoft.com/library/azure/dd179376.aspx).</span></span>
> 
> 

## <a name="prerequisites"></a><span data-ttu-id="d93fb-108">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="d93fb-108">Prerequisites</span></span>
<span data-ttu-id="d93fb-109">Jelen dokumentum céljából feltételezzük, hogy rendelkezik Azure-előfizetéssel, a tárfiók, és megfelelő kulcs fiók hello.</span><span class="sxs-lookup"><span data-stu-id="d93fb-109">This document assumes that you have an Azure subscription, a storage account and hello corresponding storage key for that account.</span></span> <span data-ttu-id="d93fb-110">Adatok feltöltése/letöltése, előtt ismernie kell az Azure storage-fiók nevét és a fiók kulcs.</span><span class="sxs-lookup"><span data-stu-id="d93fb-110">Before uploading/downloading data, you must know your Azure storage account name and account key.</span></span>

* <span data-ttu-id="d93fb-111">tooset be Azure-előfizetéssel, lásd: [ingyenes egy hónapos próbaverzió](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="d93fb-111">tooset up an Azure subscription, see [Free one-month trial](https://azure.microsoft.com/pricing/free-trial/).</span></span>
* <span data-ttu-id="d93fb-112">A storage-fiók létrehozásával és az első fiók és a fontos információkat lásd: [tudnivalók az Azure storage-fiókok](../storage/common/storage-create-storage-account.md).</span><span class="sxs-lookup"><span data-stu-id="d93fb-112">For instructions on creating a storage account and for getting account and key information, see [About Azure storage accounts](../storage/common/storage-create-storage-account.md).</span></span>

## <a name="run-azcopy-commands"></a><span data-ttu-id="d93fb-113">AzCopy parancsok futtatása</span><span class="sxs-lookup"><span data-stu-id="d93fb-113">Run AzCopy commands</span></span>
<span data-ttu-id="d93fb-114">toorun AzCopy parancsok, nyisson meg egy parancsablakot, és keresse meg a toohello AzCopy telepítési könyvtárára a számítógépen, ahol hello AzCopy.exe végrehajtható fájl is található.</span><span class="sxs-lookup"><span data-stu-id="d93fb-114">toorun AzCopy commands, open a command window and navigate toohello AzCopy installation directory on your computer, where hello AzCopy.exe executable is located.</span></span> 

<span data-ttu-id="d93fb-115">hello alapvető AzCopy parancs szintaxisa a következő:</span><span class="sxs-lookup"><span data-stu-id="d93fb-115">hello basic syntax for AzCopy commands is:</span></span>

    AzCopy /Source:<source> /Dest:<destination> [Options]

> [!NOTE]
> <span data-ttu-id="d93fb-116">Hello AzCopy telepítési tooyour rendszer elérési útjának hozzáadása, és futtassa a hello parancsokat bármelyik könyvtárból.</span><span class="sxs-lookup"><span data-stu-id="d93fb-116">You can add hello AzCopy installation location tooyour system path and then run hello commands from any directory.</span></span> <span data-ttu-id="d93fb-117">Alapértelmezés szerint AzCopy túl van telepítve*% ProgramFiles (x86) %\Microsoft SDKs\Azure\AzCopy* vagy *%ProgramFiles%\Microsoft SDKs\Azure\AzCopy*.</span><span class="sxs-lookup"><span data-stu-id="d93fb-117">By default, AzCopy is installed too*%ProgramFiles(x86)%\Microsoft SDKs\Azure\AzCopy* or *%ProgramFiles%\Microsoft SDKs\Azure\AzCopy*.</span></span>
> 
> 

## <a name="upload-files-tooan-azure-blob"></a><span data-ttu-id="d93fb-118">Fájlok tooan Azure blob feltöltése</span><span class="sxs-lookup"><span data-stu-id="d93fb-118">Upload files tooan Azure blob</span></span>
<span data-ttu-id="d93fb-119">tooupload egy fájl a következő parancs hello használata:</span><span class="sxs-lookup"><span data-stu-id="d93fb-119">tooupload a file, use hello following command:</span></span>

    # Upload from local file system
    AzCopy /Source:<your_local_directory> /Dest: https://<your_account_name>.blob.core.windows.net/<your_container_name> /DestKey:<your_account_key> /S


## <a name="download-files-from-an-azure-blob"></a><span data-ttu-id="d93fb-120">Egy Azure-blobot a fájlok letöltése</span><span class="sxs-lookup"><span data-stu-id="d93fb-120">Download files from an Azure blob</span></span>
<span data-ttu-id="d93fb-121">egy fájl futtatását egy Azure blob, a következő parancs használata hello toodownload:</span><span class="sxs-lookup"><span data-stu-id="d93fb-121">toodownload a file from an Azure blob, use hello following command:</span></span>

    # Downloading blobs toolocal file system
    AzCopy /Source:https://<your_account_name>.blob.core.windows.net/<your_container_name>/<your_sub_directory_at_blob>  /Dest:<your_local_directory> /SourceKey:<your_account_key> /Pattern:<file_pattern> /S


## <a name="transfer-blobs-between-azure-containers"></a><span data-ttu-id="d93fb-122">Azure tárolók közötti átviteléhez a BLOB</span><span class="sxs-lookup"><span data-stu-id="d93fb-122">Transfer blobs between Azure containers</span></span>
<span data-ttu-id="d93fb-123">Azure tárolók közötti tootransfer blobok hello a következő parancsot használja:</span><span class="sxs-lookup"><span data-stu-id="d93fb-123">tootransfer blobs between Azure containers, use hello following command:</span></span>

    # Transferring blobs between Azure containers
    AzCopy /Source:https://<your_account_name1>.blob.core.windows.net/<your_container_name1>/<your_sub_directory_at_blob1> /Dest:https://<your_account_name2>.blob.core.windows.net/<your_container_name2>/<your_sub_directory_at_blob2> /SourceKey:<your_account_key1> /DestKey:<your_account_key2> /Pattern:<file_pattern> /S

    <your_account_name>: your storage account name
    <your_account_key>: your storage account key
    <your_container_name>: your container name
    <your_sub_directory_at_blob>: hello sub directory in hello container
    <your_local_directory>: directory of local file system where files toobe uploaded from or hello directory of local file system files toobe downloaded to
    <file_pattern>: pattern of file names toobe transferred. hello standard wildcards are supported


## <a name="tips-for-using-azcopy"></a><span data-ttu-id="d93fb-124">Tippek az AzCopy segítségével</span><span class="sxs-lookup"><span data-stu-id="d93fb-124">Tips for using AzCopy</span></span>
> [!TIP]
> 1. <span data-ttu-id="d93fb-125">Ha **feltöltése** fájlok, */S* fájlok rekurzív módon feltölt.</span><span class="sxs-lookup"><span data-stu-id="d93fb-125">When **uploading** files, */S* uploads files recursively.</span></span> <span data-ttu-id="d93fb-126">Ez a paraméter nélkül nem feltöltése fájlok alkönyvtáraiban találhatóak.</span><span class="sxs-lookup"><span data-stu-id="d93fb-126">Without this parameter, files in subdirectories are not uploaded.</span></span>  
> 2. <span data-ttu-id="d93fb-127">Ha **letöltése** fájl, */S* keresések tároló rekurzív módon hello amíg hello megadott könyvtárban és annak alkönyvtáraiban található összes fájl, vagy a megfelelő fájlok hello megadott hello a megadott mintának könyvtár, illetve annak alkönyvtáraiba, letöltődnek.</span><span class="sxs-lookup"><span data-stu-id="d93fb-127">When **downloading** file, */S* searches hello container recursively until all files in hello specified directory and its subdirectories, or all files that match hello specified pattern in hello given directory and its subdirectories, are downloaded.</span></span>  
> 3. <span data-ttu-id="d93fb-128">Nem adható meg egy **adott blob fájl** hello segítségével toodownload */Source* paraméter.</span><span class="sxs-lookup"><span data-stu-id="d93fb-128">You cannot specify a **specific blob file** toodownload using hello */Source* parameter.</span></span> <span data-ttu-id="d93fb-129">toodownload egy adott fájlt, adja meg a hello blob fájl neve toodownload hello segítségével */mintát* paraméter.</span><span class="sxs-lookup"><span data-stu-id="d93fb-129">toodownload a specific file, specify hello blob file name toodownload using hello */Pattern* parameter.</span></span> <span data-ttu-id="d93fb-130">**/S** paraméter lehet használt toohave AzCopy keresse meg a fájl neve mintát rekurzív módon.</span><span class="sxs-lookup"><span data-stu-id="d93fb-130">**/S** parameter can be used toohave AzCopy look for a file name pattern recursively.</span></span> <span data-ttu-id="d93fb-131">Hello mintát paraméter nélkül AzCopy letölti az összes fájlt, hogy a címtárban.</span><span class="sxs-lookup"><span data-stu-id="d93fb-131">Without hello pattern parameter, AzCopy downloads all files in that directory.</span></span>
> 
> 

