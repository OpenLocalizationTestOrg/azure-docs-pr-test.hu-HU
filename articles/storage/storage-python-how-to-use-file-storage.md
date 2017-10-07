---
title: az Azure File storage Python aaaDevelop |} Microsoft Docs
description: "Ismerje meg, hogyan toodevelop Python-alkalmazások és szolgáltatások, Azure File storage toostore használó fájladatok."
services: storage
documentationcenter: python
author: robinsh
manager: timlt
editor: tysonn
ms.assetid: 297f3a14-6b3a-48b0-9da4-db5907827fb5
ms.service: storage
ms.workload: storage
ms.tgt_pltfrm: na
ms.devlang: python
ms.topic: article
ms.date: 12/08/2016
ms.author: robinsh
ms.openlocfilehash: 45623e6dbec6f140cedc4e58e56a93fb4af9054e
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="develop-for-azure-file-storage-with-python"></a><span data-ttu-id="5697e-103">Az Azure File storage Python fejlesztése</span><span class="sxs-lookup"><span data-stu-id="5697e-103">Develop for Azure File storage with Python</span></span>
[!INCLUDE [storage-selector-file-include](../../includes/storage-selector-file-include.md)]

[!INCLUDE [storage-try-azure-tools-files](../../includes/storage-try-azure-tools-files.md)]

## <a name="about-this-tutorial"></a><span data-ttu-id="5697e-104">Az oktatóanyag ismertetése</span><span class="sxs-lookup"><span data-stu-id="5697e-104">About this tutorial</span></span>
<span data-ttu-id="5697e-105">Ez az oktatóanyag mutatni, Python toodevelop alkalmazásokhoz vagy szolgáltatásokhoz, használja az Azure File storage toostore fájladatok használatával hello alapjait.</span><span class="sxs-lookup"><span data-stu-id="5697e-105">This tutorial will demonstrate hello basics of using Python toodevelop applications or services that use Azure File storage toostore file data.</span></span> <span data-ttu-id="5697e-106">Ebben az oktatóanyagban a rendszer létrehoz egy egyszerű konzolalkalmazásként és megjelenítése hogyan tooperform alapszintű műveleteket az Python és az Azure File storage szolgáltatással:</span><span class="sxs-lookup"><span data-stu-id="5697e-106">In this tutorial, we will create a simple console application and show how tooperform basic actions with Python and Azure File storage:</span></span>

* <span data-ttu-id="5697e-107">Az Azure fájlmegosztások létrehozása</span><span class="sxs-lookup"><span data-stu-id="5697e-107">Create Azure File shares</span></span>
* <span data-ttu-id="5697e-108">Könyvtárak létrehozása</span><span class="sxs-lookup"><span data-stu-id="5697e-108">Create directories</span></span>
* <span data-ttu-id="5697e-109">Fájlok és könyvtárak egy Azure fájlmegosztás számbavétele</span><span class="sxs-lookup"><span data-stu-id="5697e-109">Enumerate files and directories in an Azure File share</span></span>
* <span data-ttu-id="5697e-110">Töltse fel, töltse le és törölje a fájlt</span><span class="sxs-lookup"><span data-stu-id="5697e-110">Upload, download, and delete a file</span></span>

> [!Note]  
> <span data-ttu-id="5697e-111">Előfordulhat, hogy Azure fájltároló SMB-n keresztül érhető el, mert lehetséges toowrite elérhető hello Azure fájlmegosztás hello szabványos Python i/o-osztály és függvények használata egyszerű alkalmazásokat is.</span><span class="sxs-lookup"><span data-stu-id="5697e-111">Because Azure File storage may be accessed over SMB, it is possible toowrite simple applications that access hello Azure File share using hello standard Python I/O classes and functions.</span></span> <span data-ttu-id="5697e-112">Ez a cikk ismerteti, hogyan toowrite használó alkalmazások hello használ hello Azure Storage Python SDK [Azure File storage REST API](https://docs.microsoft.com/en-us/rest/api/storageservices/fileservices/file-service-rest-api) tootalk tooAzure fájlok tárolására.</span><span class="sxs-lookup"><span data-stu-id="5697e-112">This article will describe how toowrite applications that use hello Azure Storage Python SDK, which uses hello [Azure File storage REST API](https://docs.microsoft.com/en-us/rest/api/storageservices/fileservices/file-service-rest-api) tootalk tooAzure File storage.</span></span>

### <a name="set-up-your-application-toouse-azure-file-storage"></a><span data-ttu-id="5697e-113">Az alkalmazás toouse Azure File storage beállítása</span><span class="sxs-lookup"><span data-stu-id="5697e-113">Set up your application toouse Azure File storage</span></span>
<span data-ttu-id="5697e-114">Adja hozzá a hello következő bármely Python forrásfájlt, ahol tooprogrammatically access Azure Storage kívánja hello tetején.</span><span class="sxs-lookup"><span data-stu-id="5697e-114">Add hello following near hello top of any Python source file in which you wish tooprogrammatically access Azure Storage.</span></span>

```python
from azure.storage.file import FileService
```

### <a name="set-up-a-connection-tooazure-file-storage"></a><span data-ttu-id="5697e-115">Egy kapcsolat tooAzure a File storage beállítása</span><span class="sxs-lookup"><span data-stu-id="5697e-115">Set up a connection tooAzure File storage</span></span> 
<span data-ttu-id="5697e-116">Hello `FileService` objektum lehetővé teszi, hogy a megosztások, könyvtárak és fájlok.</span><span class="sxs-lookup"><span data-stu-id="5697e-116">hello `FileService` object lets you work with shares, directories and files.</span></span> <span data-ttu-id="5697e-117">hello alábbi kód létrehoz egy `FileService` objektum hello tárfiók neve és a fiók kulcsot használ.</span><span class="sxs-lookup"><span data-stu-id="5697e-117">hello following code creates a `FileService` object using hello storage account name and account key.</span></span> <span data-ttu-id="5697e-118">Cserélje le `<myaccount>` és `<mykey>` a fióknevet és kulcsot.</span><span class="sxs-lookup"><span data-stu-id="5697e-118">Replace `<myaccount>` and `<mykey>` with your account name and key.</span></span>

```python
file_service = FileService(account_name='myaccount', account_key='mykey')
```

### <a name="create-an-azure-file-share"></a><span data-ttu-id="5697e-119">Azure-fájlmegosztás létrehozása</span><span class="sxs-lookup"><span data-stu-id="5697e-119">Create an Azure File share</span></span>
<span data-ttu-id="5697e-120">Az alábbi kódpéldát hello, használhatja a `FileService` objektum toocreate hello megosztást, ha még nem létezik.</span><span class="sxs-lookup"><span data-stu-id="5697e-120">In hello following code example, you can use a `FileService` object toocreate hello share if it doesn't exist.</span></span>

```python
file_service.create_share('myshare')
```

### <a name="create-a-directory"></a><span data-ttu-id="5697e-121">Könyvtár létrehozása</span><span class="sxs-lookup"><span data-stu-id="5697e-121">Create a directory</span></span>
<span data-ttu-id="5697e-122">Tegyen alkönyvtárat ahelyett, az összes hello gyökérkönyvtárában lévő fájlok tárolási is rendezhetők.</span><span class="sxs-lookup"><span data-stu-id="5697e-122">You can also organize storage by putting files inside sub-directories instead of having all of them in hello root directory.</span></span> <span data-ttu-id="5697e-123">Az Azure File storage lehetővé teszi toocreate számos könyvtárat, a fiók fog lehetővé.</span><span class="sxs-lookup"><span data-stu-id="5697e-123">Azure File storage allows you toocreate as many directories as your account will allow.</span></span> <span data-ttu-id="5697e-124">hello kódot hoz létre a nevű alkönyvtárát **sampledir** hello gyökérkönyvtárban.</span><span class="sxs-lookup"><span data-stu-id="5697e-124">hello code below will create a sub-directory named **sampledir** under hello root directory.</span></span>

```python
file_service.create_directory('myshare', 'sampledir')
```

### <a name="enumerate-files-and-directories-in-an-azure-file-share"></a><span data-ttu-id="5697e-125">Fájlok és könyvtárak egy Azure fájlmegosztás számbavétele</span><span class="sxs-lookup"><span data-stu-id="5697e-125">Enumerate files and directories in an Azure File share</span></span>
<span data-ttu-id="5697e-126">toolist hello fájlok és könyvtárak olyan megosztáson található, használja a hello **lista\_könyvtárak\_és\_fájlok** metódust.</span><span class="sxs-lookup"><span data-stu-id="5697e-126">toolist hello files and directories in a share, use hello **list\_directories\_and\_files** method.</span></span> <span data-ttu-id="5697e-127">Ez a módszer egy generátor adja vissza.</span><span class="sxs-lookup"><span data-stu-id="5697e-127">This method returns a generator.</span></span> <span data-ttu-id="5697e-128">hello alábbira kimenete hello **neve** minden fájl és a könyvtár egy megosztás toohello konzolon.</span><span class="sxs-lookup"><span data-stu-id="5697e-128">hello following code outputs hello **name** of each file and directory in a share toohello console.</span></span>

```python
generator = file_service.list_directories_and_files('myshare')
for file_or_dir in generator:
    print(file_or_dir.name)
```

### <a name="upload-a-file"></a><span data-ttu-id="5697e-129">Fájl feltöltése</span><span class="sxs-lookup"><span data-stu-id="5697e-129">Upload a file</span></span> 
<span data-ttu-id="5697e-130">A megosztás nagyon legalább tartalmaz: hello Azure fájlt, fájlokat tároló is gyökérkönyvtár.</span><span class="sxs-lookup"><span data-stu-id="5697e-130">Azure File share contains at hello very least, a root directory where files can reside.</span></span> <span data-ttu-id="5697e-131">Ebben a szakaszban megtudhatja, hogyan tooupload egy fájlt a helyi tároló alakzatot hello gyökérkönyvtár megosztás.</span><span class="sxs-lookup"><span data-stu-id="5697e-131">In this section, you'll learn how tooupload a file from local storage onto hello root directory of a share.</span></span>

<span data-ttu-id="5697e-132">toocreate egy fájl és az adatok feltöltése, használja a hello `create_file_from_path`, `create_file_from_stream`, `create_file_from_bytes` vagy `create_file_from_text` módszerek.</span><span class="sxs-lookup"><span data-stu-id="5697e-132">toocreate a file and upload data, use hello `create_file_from_path`, `create_file_from_stream`, `create_file_from_bytes` or `create_file_from_text` methods.</span></span> <span data-ttu-id="5697e-133">Magas szintű hello szükséges adattömbösítő Ha hello hello adatok mérete túllépi a 64 MB végző módszerekkel.</span><span class="sxs-lookup"><span data-stu-id="5697e-133">They are high-level methods that perform hello necessary chunking when hello size of hello data exceeds 64 MB.</span></span>

<span data-ttu-id="5697e-134">`create_file_from_path`feltöltések hello fájl megadott elérési hello és `create_file_from_stream` feltöltések hello egy már megnyitott fájl vagy adatfolyam tartalmát.</span><span class="sxs-lookup"><span data-stu-id="5697e-134">`create_file_from_path` uploads hello contents of a file from hello specified path, and `create_file_from_stream` uploads hello contents from an already opened file/stream.</span></span> <span data-ttu-id="5697e-135">`create_file_from_bytes`Bájttömb, feltölti és `create_file_from_text` feltöltések hello megadott szöveges érték hello segítségével megadott kódolási (alapértelmezett tooUTF-8).</span><span class="sxs-lookup"><span data-stu-id="5697e-135">`create_file_from_bytes` uploads an array of bytes, and `create_file_from_text` uploads hello specified text value using hello specified encoding (defaults tooUTF-8).</span></span>

<span data-ttu-id="5697e-136">hello alábbi példa feltölt hello hello tartalmát **sunset.png** hello fájlból **saját_fájl** fájlt.</span><span class="sxs-lookup"><span data-stu-id="5697e-136">hello following example uploads hello contents of hello **sunset.png** file into hello **myfile** file.</span></span>

```python
from azure.storage.file import ContentSettings
file_service.create_file_from_path(
    'myshare',
    None, # We want toocreate this blob in hello root directory, so we specify None for hello directory_name
    'myfile',
    'sunset.png',
    content_settings=ContentSettings(content_type='image/png'))
```

### <a name="download-a-file"></a><span data-ttu-id="5697e-137">Fájl letöltése</span><span class="sxs-lookup"><span data-stu-id="5697e-137">Download a file</span></span>
<span data-ttu-id="5697e-138">toodownload adatok fájlból történő használata `get_file_to_path`, `get_file_to_stream`, `get_file_to_bytes`, vagy `get_file_to_text`.</span><span class="sxs-lookup"><span data-stu-id="5697e-138">toodownload data from a file, use `get_file_to_path`, `get_file_to_stream`, `get_file_to_bytes`, or `get_file_to_text`.</span></span> <span data-ttu-id="5697e-139">Magas szintű hello szükséges adattömbösítő Ha hello hello adatok mérete túllépi a 64 MB végző módszerekkel.</span><span class="sxs-lookup"><span data-stu-id="5697e-139">They are high-level methods that perform hello necessary chunking when hello size of hello data exceeds 64 MB.</span></span>

<span data-ttu-id="5697e-140">hello következő példa bemutatja, hogyan használatával `get_file_to_path` toodownload hello tartalmát hello **saját_fájl** fájlt, és tárolja toohello **out-sunset.png** fájlt.</span><span class="sxs-lookup"><span data-stu-id="5697e-140">hello following example demonstrates using `get_file_to_path` toodownload hello contents of hello **myfile** file and store it toohello **out-sunset.png** file.</span></span>

```python
file_service.get_file_to_path('myshare', None, 'myfile', 'out-sunset.png')
```

### <a name="delete-a-file"></a><span data-ttu-id="5697e-141">Fájl törlése</span><span class="sxs-lookup"><span data-stu-id="5697e-141">Delete a file</span></span>
<span data-ttu-id="5697e-142">Végezetül toodelete egy fájl hívás `delete_file`.</span><span class="sxs-lookup"><span data-stu-id="5697e-142">Finally, toodelete a file, call `delete_file`.</span></span>

```python
file_service.delete_file('myshare', None, 'myfile')
```

## <a name="next-steps"></a><span data-ttu-id="5697e-143">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="5697e-143">Next steps</span></span>
<span data-ttu-id="5697e-144">Most, hogy megismerte hogyan toomanipulate Azure File storage Python, kövesse a további hivatkozások toolearn.</span><span class="sxs-lookup"><span data-stu-id="5697e-144">Now that you've learned how toomanipulate Azure File storage with Python, follow these links toolearn more.</span></span>

* [<span data-ttu-id="5697e-145">Python fejlesztői központ</span><span class="sxs-lookup"><span data-stu-id="5697e-145">Python Developer Center</span></span>](/develop/python/)
* [<span data-ttu-id="5697e-146">Az Azure Storage-szolgáltatások REST API-ja</span><span class="sxs-lookup"><span data-stu-id="5697e-146">Azure Storage Services REST API</span></span>](http://msdn.microsoft.com/library/azure/dd179355)
* [<span data-ttu-id="5697e-147">A Microsoft Azure Storage Python SDK</span><span class="sxs-lookup"><span data-stu-id="5697e-147">Microsoft Azure Storage SDK for Python</span></span>](https://github.com/Azure/azure-storage-python)