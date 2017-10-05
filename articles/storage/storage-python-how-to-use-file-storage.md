---
title: "Az Azure File storage Python kidolgozása |} Microsoft Docs"
description: "Ismerje meg, hogyan fejleszthet Python-alkalmazások és szolgáltatások Azure File storage használatával tárolhatja a fájljait."
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
ms.openlocfilehash: a1a37266908277b54e7b42d85b9b4873af77e622
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/03/2017
---
# <a name="develop-for-azure-file-storage-with-python"></a><span data-ttu-id="42353-103">Az Azure File storage Python fejlesztése</span><span class="sxs-lookup"><span data-stu-id="42353-103">Develop for Azure File storage with Python</span></span>
[!INCLUDE [storage-selector-file-include](../../includes/storage-selector-file-include.md)]

[!INCLUDE [storage-try-azure-tools-files](../../includes/storage-try-azure-tools-files.md)]

## <a name="about-this-tutorial"></a><span data-ttu-id="42353-104">Az oktatóanyag ismertetése</span><span class="sxs-lookup"><span data-stu-id="42353-104">About this tutorial</span></span>
<span data-ttu-id="42353-105">Ez az oktatóanyag mutatni, Python alkalmazásokhoz vagy szolgáltatásokhoz, tárolhatja a fájljait az Azure File storage segítségével fejlesztéséhez használatának alapjaival.</span><span class="sxs-lookup"><span data-stu-id="42353-105">This tutorial will demonstrate the basics of using Python to develop applications or services that use Azure File storage to store file data.</span></span> <span data-ttu-id="42353-106">Ebben az oktatóanyagban a rendszer egyszerű Konzolalkalmazás létrehozása és a Python és az Azure File storage alapvető műveleteket szemléltetik:</span><span class="sxs-lookup"><span data-stu-id="42353-106">In this tutorial, we will create a simple console application and show how to perform basic actions with Python and Azure File storage:</span></span>

* <span data-ttu-id="42353-107">Az Azure fájlmegosztások létrehozása</span><span class="sxs-lookup"><span data-stu-id="42353-107">Create Azure File shares</span></span>
* <span data-ttu-id="42353-108">Könyvtárak létrehozása</span><span class="sxs-lookup"><span data-stu-id="42353-108">Create directories</span></span>
* <span data-ttu-id="42353-109">Fájlok és könyvtárak egy Azure fájlmegosztás számbavétele</span><span class="sxs-lookup"><span data-stu-id="42353-109">Enumerate files and directories in an Azure File share</span></span>
* <span data-ttu-id="42353-110">Töltse fel, töltse le és törölje a fájlt</span><span class="sxs-lookup"><span data-stu-id="42353-110">Upload, download, and delete a file</span></span>

> [!Note]  
> <span data-ttu-id="42353-111">Mivel előfordulhat, hogy Azure fájltároló SMB-n keresztül érhető el, akkor lehet egyszerű alkalmazások írását, amelyek a szabványos Python i/o-osztályok és függvény használata Azure fájlmegosztás eléréséhez.</span><span class="sxs-lookup"><span data-stu-id="42353-111">Because Azure File storage may be accessed over SMB, it is possible to write simple applications that access the Azure File share using the standard Python I/O classes and functions.</span></span> <span data-ttu-id="42353-112">Ez a cikk azt ismerteti, hogyan alkalmazások írását, amelyek az Azure Storage Python SDK-val, használja a [Azure File storage REST API](https://docs.microsoft.com/en-us/rest/api/storageservices/fileservices/file-service-rest-api) felvegye a Azure File storage.</span><span class="sxs-lookup"><span data-stu-id="42353-112">This article will describe how to write applications that use the Azure Storage Python SDK, which uses the [Azure File storage REST API](https://docs.microsoft.com/en-us/rest/api/storageservices/fileservices/file-service-rest-api) to talk to Azure File storage.</span></span>

### <a name="set-up-your-application-to-use-azure-file-storage"></a><span data-ttu-id="42353-113">Állítsa be az alkalmazás Azure File storage használata</span><span class="sxs-lookup"><span data-stu-id="42353-113">Set up your application to use Azure File storage</span></span>
<span data-ttu-id="42353-114">Adja hozzá a következő tetejénél található bármely Python forrásfájl, amelyben programon keresztüli eléréséhez az Azure Storage kívánja.</span><span class="sxs-lookup"><span data-stu-id="42353-114">Add the following near the top of any Python source file in which you wish to programmatically access Azure Storage.</span></span>

```python
from azure.storage.file import FileService
```

### <a name="set-up-a-connection-to-azure-file-storage"></a><span data-ttu-id="42353-115">Egy Azure File Storage-kapcsolat beállítása</span><span class="sxs-lookup"><span data-stu-id="42353-115">Set up a connection to Azure File storage</span></span> 
<span data-ttu-id="42353-116">A `FileService` objektum lehetővé teszi, hogy a megosztások, könyvtárak és fájlok.</span><span class="sxs-lookup"><span data-stu-id="42353-116">The `FileService` object lets you work with shares, directories and files.</span></span> <span data-ttu-id="42353-117">Az alábbi kód létrehoz egy `FileService` objektumba a tárfiók nevét és a fiók kulcsot.</span><span class="sxs-lookup"><span data-stu-id="42353-117">The following code creates a `FileService` object using the storage account name and account key.</span></span> <span data-ttu-id="42353-118">Cserélje le `<myaccount>` és `<mykey>` a fióknevet és kulcsot.</span><span class="sxs-lookup"><span data-stu-id="42353-118">Replace `<myaccount>` and `<mykey>` with your account name and key.</span></span>

```python
file_service = FileService(account_name='myaccount', account_key='mykey')
```

### <a name="create-an-azure-file-share"></a><span data-ttu-id="42353-119">Azure-fájlmegosztás létrehozása</span><span class="sxs-lookup"><span data-stu-id="42353-119">Create an Azure File share</span></span>
<span data-ttu-id="42353-120">Az alábbi példakód használhat egy `FileService` objektumot a megosztás létrehozásához, ha még nem létezik.</span><span class="sxs-lookup"><span data-stu-id="42353-120">In the following code example, you can use a `FileService` object to create the share if it doesn't exist.</span></span>

```python
file_service.create_share('myshare')
```

### <a name="create-a-directory"></a><span data-ttu-id="42353-121">Könyvtár létrehozása</span><span class="sxs-lookup"><span data-stu-id="42353-121">Create a directory</span></span>
<span data-ttu-id="42353-122">Tárolási tegyen alkönyvtárat így ahelyett hogy ezek a gyökérkönyvtárban található fájlok is rendezhetők.</span><span class="sxs-lookup"><span data-stu-id="42353-122">You can also organize storage by putting files inside sub-directories instead of having all of them in the root directory.</span></span> <span data-ttu-id="42353-123">Az Azure File storage hozhat létre a fiókját engedélyezi a könyvtárat.</span><span class="sxs-lookup"><span data-stu-id="42353-123">Azure File storage allows you to create as many directories as your account will allow.</span></span> <span data-ttu-id="42353-124">Az alábbi kódot hoz létre a nevű alkönyvtárát **sampledir** a gyökérkönyvtárban.</span><span class="sxs-lookup"><span data-stu-id="42353-124">The code below will create a sub-directory named **sampledir** under the root directory.</span></span>

```python
file_service.create_directory('myshare', 'sampledir')
```

### <a name="enumerate-files-and-directories-in-an-azure-file-share"></a><span data-ttu-id="42353-125">Fájlok és könyvtárak egy Azure fájlmegosztás számbavétele</span><span class="sxs-lookup"><span data-stu-id="42353-125">Enumerate files and directories in an Azure File share</span></span>
<span data-ttu-id="42353-126">Kilistázhatja a fájlok és könyvtárak olyan megosztáson található, a **lista\_könyvtárak\_és\_fájlok** metódust.</span><span class="sxs-lookup"><span data-stu-id="42353-126">To list the files and directories in a share, use the **list\_directories\_and\_files** method.</span></span> <span data-ttu-id="42353-127">Ez a módszer egy generátor adja vissza.</span><span class="sxs-lookup"><span data-stu-id="42353-127">This method returns a generator.</span></span> <span data-ttu-id="42353-128">A következő kimenetek kódot a **neve** minden fájl és a könyvtár egy megosztáson található, a konzolhoz.</span><span class="sxs-lookup"><span data-stu-id="42353-128">The following code outputs the **name** of each file and directory in a share to the console.</span></span>

```python
generator = file_service.list_directories_and_files('myshare')
for file_or_dir in generator:
    print(file_or_dir.name)
```

### <a name="upload-a-file"></a><span data-ttu-id="42353-129">Fájl feltöltése</span><span class="sxs-lookup"><span data-stu-id="42353-129">Upload a file</span></span> 
<span data-ttu-id="42353-130">A megosztás tartalmaz legalább az Azure File, egy gyökérkönyvtár fájlokat tároló is.</span><span class="sxs-lookup"><span data-stu-id="42353-130">Azure File share contains at the very least, a root directory where files can reside.</span></span> <span data-ttu-id="42353-131">Ebben a szakaszban megtudhatja, hogyan feltölteni a fájlt a helyi tároló megosztás gyökérkönyvtárában alakzatot.</span><span class="sxs-lookup"><span data-stu-id="42353-131">In this section, you'll learn how to upload a file from local storage onto the root directory of a share.</span></span>

<span data-ttu-id="42353-132">Hozzon létre egy fájlt, és feltölteni az adatokat, használja a `create_file_from_path`, `create_file_from_stream`, `create_file_from_bytes` vagy `create_file_from_text` módszerek.</span><span class="sxs-lookup"><span data-stu-id="42353-132">To create a file and upload data, use the `create_file_from_path`, `create_file_from_stream`, `create_file_from_bytes` or `create_file_from_text` methods.</span></span> <span data-ttu-id="42353-133">Hajtsa végre a szükséges adattömbösítő, ha az adatok mérete meghaladja a 64 MB magas szintű módszerek.</span><span class="sxs-lookup"><span data-stu-id="42353-133">They are high-level methods that perform the necessary chunking when the size of the data exceeds 64 MB.</span></span>

<span data-ttu-id="42353-134">`create_file_from_path`feltölt egy fájlt a megadott elérési és `create_file_from_stream` feltölt egy már megnyitott fájl vagy adatfolyam tartalmát.</span><span class="sxs-lookup"><span data-stu-id="42353-134">`create_file_from_path` uploads the contents of a file from the specified path, and `create_file_from_stream` uploads the contents from an already opened file/stream.</span></span> <span data-ttu-id="42353-135">`create_file_from_bytes`Bájttömb, feltölti és `create_file_from_text` feltölti az adott szöveges értéket a megadott kódolás (alapértelmezett értéke UTF-8) használatával.</span><span class="sxs-lookup"><span data-stu-id="42353-135">`create_file_from_bytes` uploads an array of bytes, and `create_file_from_text` uploads the specified text value using the specified encoding (defaults to UTF-8).</span></span>

<span data-ttu-id="42353-136">Az alábbi példa feltölti a tartalmát a **sunset.png** fájlt a **saját_fájl** fájlt.</span><span class="sxs-lookup"><span data-stu-id="42353-136">The following example uploads the contents of the **sunset.png** file into the **myfile** file.</span></span>

```python
from azure.storage.file import ContentSettings
file_service.create_file_from_path(
    'myshare',
    None, # We want to create this blob in the root directory, so we specify None for the directory_name
    'myfile',
    'sunset.png',
    content_settings=ContentSettings(content_type='image/png'))
```

### <a name="download-a-file"></a><span data-ttu-id="42353-137">Fájl letöltése</span><span class="sxs-lookup"><span data-stu-id="42353-137">Download a file</span></span>
<span data-ttu-id="42353-138">Adatok fájlból való letöltéséhez használjon `get_file_to_path`, `get_file_to_stream`, `get_file_to_bytes`, vagy `get_file_to_text`.</span><span class="sxs-lookup"><span data-stu-id="42353-138">To download data from a file, use `get_file_to_path`, `get_file_to_stream`, `get_file_to_bytes`, or `get_file_to_text`.</span></span> <span data-ttu-id="42353-139">Hajtsa végre a szükséges adattömbösítő, ha az adatok mérete meghaladja a 64 MB magas szintű módszerek.</span><span class="sxs-lookup"><span data-stu-id="42353-139">They are high-level methods that perform the necessary chunking when the size of the data exceeds 64 MB.</span></span>

<span data-ttu-id="42353-140">A következő példa bemutatja, hogy használatával `get_file_to_path` tartalmának letöltése a **saját_fájl** fájlt, és tárolja el azt, hogy a **out-sunset.png** fájlt.</span><span class="sxs-lookup"><span data-stu-id="42353-140">The following example demonstrates using `get_file_to_path` to download the contents of the **myfile** file and store it to the **out-sunset.png** file.</span></span>

```python
file_service.get_file_to_path('myshare', None, 'myfile', 'out-sunset.png')
```

### <a name="delete-a-file"></a><span data-ttu-id="42353-141">Fájl törlése</span><span class="sxs-lookup"><span data-stu-id="42353-141">Delete a file</span></span>
<span data-ttu-id="42353-142">Végezetül, a fájl törléséhez hívja meg a `delete_file`.</span><span class="sxs-lookup"><span data-stu-id="42353-142">Finally, to delete a file, call `delete_file`.</span></span>

```python
file_service.delete_file('myshare', None, 'myfile')
```

## <a name="next-steps"></a><span data-ttu-id="42353-143">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="42353-143">Next steps</span></span>
<span data-ttu-id="42353-144">Most, hogy megismerte hogyan szeretné módosítani az Azure File storage Python, ezek hivatkozásokat követve tudhat meg többet.</span><span class="sxs-lookup"><span data-stu-id="42353-144">Now that you've learned how to manipulate Azure File storage with Python, follow these links to learn more.</span></span>

* [<span data-ttu-id="42353-145">Python fejlesztői központ</span><span class="sxs-lookup"><span data-stu-id="42353-145">Python Developer Center</span></span>](/develop/python/)
* [<span data-ttu-id="42353-146">Az Azure Storage-szolgáltatások REST API-ja</span><span class="sxs-lookup"><span data-stu-id="42353-146">Azure Storage Services REST API</span></span>](http://msdn.microsoft.com/library/azure/dd179355)
* [<span data-ttu-id="42353-147">A Microsoft Azure Storage Python SDK</span><span class="sxs-lookup"><span data-stu-id="42353-147">Microsoft Azure Storage SDK for Python</span></span>](https://github.com/Azure/azure-storage-python)