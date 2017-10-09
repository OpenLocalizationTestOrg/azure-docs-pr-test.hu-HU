---
title: az Azure File storage a C++ aaaDevelop |} Microsoft Docs
description: "Ismerje meg, hogyan toodevelop C++ alkalmazások és szolgáltatások, Azure File storage toostore használó fájladatok."
services: storage
documentationcenter: .net
author: renashahmsft
manager: aungoo
editor: tysonn
ms.assetid: a1e8c99e-47a6-43a9-9541-c9262eb00b38
ms.service: storage
ms.workload: storage
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/27/2017
ms.author: renashahmsft
ms.openlocfilehash: 48f668cf9359f88baeaaa08ee0d44e70bd0f5f1a
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="develop-for-azure-file-storage-with-c"></a><span data-ttu-id="a6bd3-103">Az Azure File storage a C++ fejlesztése</span><span class="sxs-lookup"><span data-stu-id="a6bd3-103">Develop for Azure File storage with C++</span></span>
[!INCLUDE [storage-selector-file-include](../../../includes/storage-selector-file-include.md)]

[!INCLUDE [storage-try-azure-tools-files](../../../includes/storage-try-azure-tools-files.md)]

## <a name="about-this-tutorial"></a><span data-ttu-id="a6bd3-104">Az oktatóanyag ismertetése</span><span class="sxs-lookup"><span data-stu-id="a6bd3-104">About this tutorial</span></span>

<span data-ttu-id="a6bd3-105">Ebben az oktatóanyagban megtudhatja, hogyan tooperform Azure File storage alapvető műveleteket.</span><span class="sxs-lookup"><span data-stu-id="a6bd3-105">In this tutorial, you'll learn how tooperform basic operations on Azure File storage.</span></span> <span data-ttu-id="a6bd3-106">Keresztül írt C++ minták, megtudhatja, hogyan osztja meg toocreate és könyvtárak, feltöltése, listázása és fájlok törlése.</span><span class="sxs-lookup"><span data-stu-id="a6bd3-106">Through samples written in C++, you'll learn how toocreate shares and directories, upload, list, and delete files.</span></span> <span data-ttu-id="a6bd3-107">Ha új tooAzure fájltároló, hasznos információkat hello minták hello fogalmak hello szakaszokban szereplő áthaladás lesz.</span><span class="sxs-lookup"><span data-stu-id="a6bd3-107">If you are new tooAzure File storage , going through hello concepts in hello sections that follow will be helpful in understanding hello samples.</span></span>


* <span data-ttu-id="a6bd3-108">Hozzon létre vagy töröljön az Azure fájlmegosztások</span><span class="sxs-lookup"><span data-stu-id="a6bd3-108">Create and delete Azure File shares</span></span>
* <span data-ttu-id="a6bd3-109">Hozzon létre vagy töröljön a könyvtárak</span><span class="sxs-lookup"><span data-stu-id="a6bd3-109">Create and delete directories</span></span>
* <span data-ttu-id="a6bd3-110">Fájlok és könyvtárak egy Azure fájlmegosztás számbavétele</span><span class="sxs-lookup"><span data-stu-id="a6bd3-110">Enumerate files and directories in an Azure File share</span></span>
* <span data-ttu-id="a6bd3-111">Töltse fel, töltse le és törölje a fájlt</span><span class="sxs-lookup"><span data-stu-id="a6bd3-111">Upload, download, and delete a file</span></span>
* <span data-ttu-id="a6bd3-112">Egy Azure fájlmegosztás hello kvótájának (maximális méretének) beállítása</span><span class="sxs-lookup"><span data-stu-id="a6bd3-112">Set hello quota (maximum size) for an Azure File share</span></span>
* <span data-ttu-id="a6bd3-113">Hozzon létre egy megosztott elérési házirendet hello megosztáson definiált használó fájl a közös hozzáférésű jogosultságkód (SAS-kulcsot).</span><span class="sxs-lookup"><span data-stu-id="a6bd3-113">Create a shared access signature (SAS key) for a file that uses a shared access policy defined on hello share.</span></span>

> [!Note]  
> <span data-ttu-id="a6bd3-114">Előfordulhat, hogy Azure fájltároló SMB-n keresztül érhető el, mert lehetséges toowrite elérhető hello Azure fájlmegosztás hello szabványos C++ i/o-osztály és függvények használata egyszerű alkalmazásokat is.</span><span class="sxs-lookup"><span data-stu-id="a6bd3-114">Because Azure File storage may be accessed over SMB, it is possible toowrite simple applications that access hello Azure File share using hello standard C++ I/O classes and functions.</span></span> <span data-ttu-id="a6bd3-115">Ez a cikk ismerteti, hogyan toowrite használó alkalmazások hello használ hello Azure Storage C++ SDK [Azure File storage REST API](https://docs.microsoft.com/rest/api/storageservices/fileservices/file-service-rest-api) tootalk tooAzure fájlok tárolására.</span><span class="sxs-lookup"><span data-stu-id="a6bd3-115">This article will describe how toowrite applications that use hello Azure Storage C++ SDK, which uses hello [Azure File storage REST API](https://docs.microsoft.com/rest/api/storageservices/fileservices/file-service-rest-api) tootalk tooAzure File storage.</span></span>

## <a name="create-a-c-application"></a><span data-ttu-id="a6bd3-116">A C++-alkalmazás létrehozása</span><span class="sxs-lookup"><span data-stu-id="a6bd3-116">Create a C++ application</span></span>
<span data-ttu-id="a6bd3-117">toobuild hello minták, szüksége lesz tooinstall hello Azure Storage ügyféloldali kódtár 2.4.0 C++.</span><span class="sxs-lookup"><span data-stu-id="a6bd3-117">toobuild hello samples, you will need tooinstall hello Azure Storage Client Library 2.4.0 for C++.</span></span> <span data-ttu-id="a6bd3-118">Meg is létrehozott egy Azure storage-fiókot.</span><span class="sxs-lookup"><span data-stu-id="a6bd3-118">You should also have created an Azure storage account.</span></span>

<span data-ttu-id="a6bd3-119">tooinstall hello Azure Storage ügyfél 2.4.0 C++, használhatja hello a következő módszerek egyikét:</span><span class="sxs-lookup"><span data-stu-id="a6bd3-119">tooinstall hello Azure Storage Client 2.4.0 for C++, you can use one of hello following methods:</span></span>

* <span data-ttu-id="a6bd3-120">**Linux:** hello megadott hello utasítások [Azure Storage ügyféloldali kódtára a C++ információs](https://github.com/Azure/azure-storage-cpp/blob/master/README.md) lap.</span><span class="sxs-lookup"><span data-stu-id="a6bd3-120">**Linux:** Follow hello instructions given in hello [Azure Storage Client Library for C++ README](https://github.com/Azure/azure-storage-cpp/blob/master/README.md) page.</span></span>
* <span data-ttu-id="a6bd3-121">**Windows:** a Visual Studióban kattintson **eszközök &gt; NuGet-Csomagkezelő &gt; Csomagkezelő konzol**.</span><span class="sxs-lookup"><span data-stu-id="a6bd3-121">**Windows:** In Visual Studio, click **Tools &gt; NuGet Package Manager &gt; Package Manager Console**.</span></span> <span data-ttu-id="a6bd3-122">Típus hello következő parancsot a hello történő [NuGet Package Manager console](http://docs.nuget.org/docs/start-here/using-the-package-manager-console) nyomja le az ENTER **ENTER**.</span><span class="sxs-lookup"><span data-stu-id="a6bd3-122">Type hello following command into hello [NuGet Package Manager console](http://docs.nuget.org/docs/start-here/using-the-package-manager-console) and press **ENTER**.</span></span>
  
```
Install-Package wastorage
```

## <a name="set-up-your-application-toouse-azure-file-storage"></a><span data-ttu-id="a6bd3-123">Az alkalmazás toouse Azure File storage beállítása</span><span class="sxs-lookup"><span data-stu-id="a6bd3-123">Set up your application toouse Azure File storage</span></span>
<span data-ttu-id="a6bd3-124">Adja hozzá, hello következő tartalmazó utasítások toohello felső hello C++ toomanipulate Azure File storage kívánt forrásfájl:</span><span class="sxs-lookup"><span data-stu-id="a6bd3-124">Add hello following include statements toohello top of hello C++ source file where you want toomanipulate Azure File storage:</span></span>

```cpp
#include <was/storage_account.h>
#include <was/file.h>
```

## <a name="set-up-an-azure-storage-connection-string"></a><span data-ttu-id="a6bd3-125">Egy Azure storage kapcsolati karakterlánc beállítása</span><span class="sxs-lookup"><span data-stu-id="a6bd3-125">Set up an Azure storage connection string</span></span>
<span data-ttu-id="a6bd3-126">a File storage toouse, tooconnect tooyour az Azure storage-fiók szükséges.</span><span class="sxs-lookup"><span data-stu-id="a6bd3-126">toouse File storage, you need tooconnect tooyour Azure storage account.</span></span> <span data-ttu-id="a6bd3-127">hello első lépéseként lenne egy kapcsolati karakterláncot, amely fogjuk használni tooconfigure tooconnect tooyour tárfiók.</span><span class="sxs-lookup"><span data-stu-id="a6bd3-127">hello first step would be tooconfigure a connection string, which we'll use tooconnect tooyour storage account.</span></span> <span data-ttu-id="a6bd3-128">Is határozza meg egy statikus változó toodo, amely.</span><span class="sxs-lookup"><span data-stu-id="a6bd3-128">Let's define a static variable toodo that.</span></span>

```cpp
// Define hello connection-string with your values.
const utility::string_t 
storage_connection_string(U("DefaultEndpointsProtocol=https;AccountName=your_storage_account;AccountKey=your_storage_account_key"));
```

## <a name="connecting-tooan-azure-storage-account"></a><span data-ttu-id="a6bd3-129">Csatlakozás tooan Azure storage-fiók</span><span class="sxs-lookup"><span data-stu-id="a6bd3-129">Connecting tooan Azure storage account</span></span>
<span data-ttu-id="a6bd3-130">Használhatja a hello **cloud_storage_account** osztály toorepresent Tárfiók adatait.</span><span class="sxs-lookup"><span data-stu-id="a6bd3-130">You can use hello **cloud_storage_account** class toorepresent your Storage Account information.</span></span> <span data-ttu-id="a6bd3-131">tooretrieve a tárolási fiók hello tárolási kapcsolati karakterlánc adatait, használhatja a hello **elemezni** metódust.</span><span class="sxs-lookup"><span data-stu-id="a6bd3-131">tooretrieve your storage account information from hello storage connection string, you can use hello **parse** method.</span></span>

```cpp
// Retrieve storage account from connection string.    
azure::storage::cloud_storage_account storage_account = 
  azure::storage::cloud_storage_account::parse(storage_connection_string);
```

## <a name="create-an-azure-file-share"></a><span data-ttu-id="a6bd3-132">Azure-fájlmegosztás létrehozása</span><span class="sxs-lookup"><span data-stu-id="a6bd3-132">Create an Azure File share</span></span>
<span data-ttu-id="a6bd3-133">A fájlok és könyvtárak az Azure File storage találhatók nevű tároló egy **megosztás**.</span><span class="sxs-lookup"><span data-stu-id="a6bd3-133">All files and directories in Azure File storage reside in a container called a **Share**.</span></span> <span data-ttu-id="a6bd3-134">A tárfiók lehet a kapacitásával lehetővé teszi tetszőleges számú megosztások.</span><span class="sxs-lookup"><span data-stu-id="a6bd3-134">Your storage account can have as many shares as your account capacity allows.</span></span> <span data-ttu-id="a6bd3-135">tooobtain hozzáférés tooa megosztást és tartalmát, egy Azure File storage ügyfél toouse kell.</span><span class="sxs-lookup"><span data-stu-id="a6bd3-135">tooobtain access tooa share and its contents, you need toouse a Azure File storage client.</span></span>

```cpp
// Create hello Azure File storage client.
azure::storage::cloud_file_client file_client = 
  storage_account.create_cloud_file_client();
```

<span data-ttu-id="a6bd3-136">Hello Azure File storage-ügyfelet használ, majd szerezhet be egy hivatkozást tooa megosztást.</span><span class="sxs-lookup"><span data-stu-id="a6bd3-136">Using hello Azure File storage client, you can then obtain a reference tooa share.</span></span>

```cpp
// Get a reference toohello file share
azure::storage::cloud_file_share share = 
  file_client.get_share_reference(_XPLATSTR("my-sample-share"));
```

<span data-ttu-id="a6bd3-137">toocreate hello megosztás használata hello **create_if_not_exists** hello metódusában **cloud_file_share** objektum.</span><span class="sxs-lookup"><span data-stu-id="a6bd3-137">toocreate hello share, use hello **create_if_not_exists** method of hello **cloud_file_share** object.</span></span>

```cpp
if (share.create_if_not_exists()) {    
    std::wcout << U("New share created") << std::endl;    
}
```

<span data-ttu-id="a6bd3-138">Ezen a ponton **megosztása** tartalmazza a referencia-tooa nevű megosztás **a minta-megosztás**.</span><span class="sxs-lookup"><span data-stu-id="a6bd3-138">At this point, **share** holds a reference tooa share named **my-sample-share**.</span></span>

## <a name="delete-an-azure-file-share"></a><span data-ttu-id="a6bd3-139">Egy Azure fájlmegosztás törlése</span><span class="sxs-lookup"><span data-stu-id="a6bd3-139">Delete an Azure File share</span></span>
<span data-ttu-id="a6bd3-140">A megosztás törlése hívó hello végezhető el **delete_if_exists** cloud_file_share objektum metódust.</span><span class="sxs-lookup"><span data-stu-id="a6bd3-140">Deleting a share is done by calling hello **delete_if_exists** method on a cloud_file_share object.</span></span> <span data-ttu-id="a6bd3-141">Itt látható mintakód, amelyet, amely.</span><span class="sxs-lookup"><span data-stu-id="a6bd3-141">Here's sample code that does that.</span></span>

```cpp
// Get a reference toohello share.
azure::storage::cloud_file_share share = 
  file_client.get_share_reference(_XPLATSTR("my-sample-share"));

// delete hello share if exists
share.delete_share_if_exists();
```

## <a name="create-a-directory"></a><span data-ttu-id="a6bd3-142">Könyvtár létrehozása</span><span class="sxs-lookup"><span data-stu-id="a6bd3-142">Create a directory</span></span>
<span data-ttu-id="a6bd3-143">Tároló üzembe alkönyvtárak ahelyett, az összes hello gyökérkönyvtárában lévő fájlok rendezhetők.</span><span class="sxs-lookup"><span data-stu-id="a6bd3-143">You can organize storage by putting files inside subdirectories instead of having all of them in hello root directory.</span></span> <span data-ttu-id="a6bd3-144">Az Azure File storage lehetővé teszi toocreate számos könyvtárat, a fiók fog lehetővé.</span><span class="sxs-lookup"><span data-stu-id="a6bd3-144">Azure File storage allows you toocreate as many directories as your account will allow.</span></span> <span data-ttu-id="a6bd3-145">hello az alábbi kód létrehoz egy nevű könyvtár **saját minta directory** alatt hello gyökérkönyvtár, valamint a nevű **saját minta alkönyvtár**.</span><span class="sxs-lookup"><span data-stu-id="a6bd3-145">hello code below will create a directory named **my-sample-directory** under hello root directory as well as a subdirectory named **my-sample-subdirectory**.</span></span>

```cpp
// Retrieve a reference tooa directory
azure::storage::cloud_file_directory directory = share.get_directory_reference(_XPLATSTR("my-sample-directory"));

// Return value is true if hello share did not exist and was successfully created.
directory.create_if_not_exists();

// Create a subdirectory.
azure::storage::cloud_file_directory subdirectory = 
  directory.get_subdirectory_reference(_XPLATSTR("my-sample-subdirectory"));
subdirectory.create_if_not_exists();
```

## <a name="delete-a-directory"></a><span data-ttu-id="a6bd3-146">Könyvtár törlése</span><span class="sxs-lookup"><span data-stu-id="a6bd3-146">Delete a directory</span></span>
<span data-ttu-id="a6bd3-147">A könyvtár törlése mely egyszerű feladat, érdemes megjegyezni, hogy nem törölhető a fájlok továbbra is tartalmazó könyvtár vagy más címtárakban.</span><span class="sxs-lookup"><span data-stu-id="a6bd3-147">Deleting a directory is a simple task, although it should be noted that you cannot delete a directory that still contains files or other directories.</span></span>

```cpp
// Get a reference toohello share.
azure::storage::cloud_file_share share = 
  file_client.get_share_reference(_XPLATSTR("my-sample-share"));

// Get a reference toohello directory.
azure::storage::cloud_file_directory directory = 
  share.get_directory_reference(_XPLATSTR("my-sample-directory"));

// Get a reference toohello subdirectory you want toodelete.
azure::storage::cloud_file_directory sub_directory =
  directory.get_subdirectory_reference(_XPLATSTR("my-sample-subdirectory"));

// Delete hello subdirectory and hello sample directory.
sub_directory.delete_directory_if_exists();

directory.delete_directory_if_exists();
```

## <a name="enumerate-files-and-directories-in-an-azure-file-share"></a><span data-ttu-id="a6bd3-148">Fájlok és könyvtárak egy Azure fájlmegosztás számbavétele</span><span class="sxs-lookup"><span data-stu-id="a6bd3-148">Enumerate files and directories in an Azure File share</span></span>
<span data-ttu-id="a6bd3-149">Fájlok és könyvtárak egy megosztáson belüli listájának beszerzésével könnyen történik meghívásával **list_files_and_directories** a egy **cloud_file_directory** hivatkozás.</span><span class="sxs-lookup"><span data-stu-id="a6bd3-149">Obtaining a list of files and directories within a share is easily done by calling **list_files_and_directories** on a **cloud_file_directory** reference.</span></span> <span data-ttu-id="a6bd3-150">tooaccess hello tulajdonságai és metódusai visszaadásakor széles skáláját **list_file_and_directory_item**, meg kell hívnia hello **list_file_and_directory_item.as_file** metódus tooget egy **cloud_ fájl** objektumot, vagy hello **list_file_and_directory_item.as_directory** metódus tooget egy **cloud_file_directory** objektum.</span><span class="sxs-lookup"><span data-stu-id="a6bd3-150">tooaccess hello rich set of properties and methods for a returned **list_file_and_directory_item**, you must call hello **list_file_and_directory_item.as_file** method tooget a **cloud_file** object, or hello **list_file_and_directory_item.as_directory** method tooget a **cloud_file_directory** object.</span></span>

<span data-ttu-id="a6bd3-151">hello a következő kód bemutatja, hogyan tooretrieve és a kimeneti hello hello megosztás hello gyökérkönyvtárában egyes elemek URI Azonosítóját.</span><span class="sxs-lookup"><span data-stu-id="a6bd3-151">hello following code demonstrates how tooretrieve and output hello URI of each item in hello root directory of hello share.</span></span>

```cpp
//Get a reference toohello root directory for hello share.
azure::storage::cloud_file_directory root_dir = 
  share.get_root_directory_reference();

// Output URI of each item.
azure::storage::list_file_and_diretory_result_iterator end_of_results;

for (auto it = directory.list_files_and_directories(); it != end_of_results; ++it)
{
    if(it->is_directory())
    {
        ucout << "Directory: " << it->as_directory().uri().primary_uri().to_string() << std::endl;
    }
    else if (it->is_file())
    {
        ucout << "File: " << it->as_file().uri().primary_uri().to_string() << std::endl;
    }        
}
```

## <a name="upload-a-file"></a><span data-ttu-id="a6bd3-152">Fájl feltöltése</span><span class="sxs-lookup"><span data-stu-id="a6bd3-152">Upload a file</span></span>
<span data-ttu-id="a6bd3-153">Nagyon hello: legalább egy Azure fájlmegosztás tartalmazza a gyökérkönyvtár fájlokat tároló is.</span><span class="sxs-lookup"><span data-stu-id="a6bd3-153">At hello very least, an Azure File share contains a root directory where files can reside.</span></span> <span data-ttu-id="a6bd3-154">Ebben a szakaszban megtudhatja, hogyan tooupload egy fájlt a helyi tároló alakzatot hello gyökérkönyvtár megosztás.</span><span class="sxs-lookup"><span data-stu-id="a6bd3-154">In this section, you'll learn how tooupload a file from local storage onto hello root directory of a share.</span></span>

<span data-ttu-id="a6bd3-155">hello első lépése a fájl feltöltése tooobtain egy hivatkozás toohello könyvtárat, ahol kell tárolni.</span><span class="sxs-lookup"><span data-stu-id="a6bd3-155">hello first step in uploading a file is tooobtain a reference toohello directory where it should reside.</span></span> <span data-ttu-id="a6bd3-156">Ehhez hívó hello **get_root_directory_reference** hello megosztás metódusa.</span><span class="sxs-lookup"><span data-stu-id="a6bd3-156">You do this by calling hello **get_root_directory_reference** method of hello share object.</span></span>

```cpp
//Get a reference toohello root directory for hello share.
azure::storage::cloud_file_directory root_dir = share.get_root_directory_reference();
```

<span data-ttu-id="a6bd3-157">Most, hogy hello megosztás hivatkozás toohello gyökérkönyvtár, feltöltheti azt a fájlt.</span><span class="sxs-lookup"><span data-stu-id="a6bd3-157">Now that you have a reference toohello root directory of hello share, you can upload a file onto it.</span></span> <span data-ttu-id="a6bd3-158">Ebben a példában feltölt egy fájlt, szöveg és a folyam.</span><span class="sxs-lookup"><span data-stu-id="a6bd3-158">This example uploads from a file, from text, and from a stream.</span></span>

```cpp
// Upload a file from a stream.
concurrency::streams::istream input_stream = 
  concurrency::streams::file_stream<uint8_t>::open_istream(_XPLATSTR("DataFile.txt")).get();

azure::storage::cloud_file file1 = 
  root_dir.get_file_reference(_XPLATSTR("my-sample-file-1"));
file1.upload_from_stream(input_stream);

// Upload some files from text.
azure::storage::cloud_file file2 = 
  root_dir.get_file_reference(_XPLATSTR("my-sample-file-2"));
file2.upload_text(_XPLATSTR("more text"));

// Upload a file from a file.
azure::storage::cloud_file file4 = 
  root_dir.get_file_reference(_XPLATSTR("my-sample-file-3"));
file4.upload_from_file(_XPLATSTR("DataFile.txt"));    
```

## <a name="download-a-file"></a><span data-ttu-id="a6bd3-159">Fájl letöltése</span><span class="sxs-lookup"><span data-stu-id="a6bd3-159">Download a file</span></span>
<span data-ttu-id="a6bd3-160">toodownload fájlok, először kérjen le egy hivatkozást, és majd meghívják a hello **download_to_stream** metódus tootransfer hello fájl tartalma tooa adatfolyam-objektum, amely meg tudja majd megőrizni a tooa helyi fájlt.</span><span class="sxs-lookup"><span data-stu-id="a6bd3-160">toodownload files, first retrieve a file reference and then call hello **download_to_stream** method tootransfer hello file contents tooa stream object, which you can then persist tooa local file.</span></span> <span data-ttu-id="a6bd3-161">Másik lehetőségként használhatja a hello **download_to_file** metódus toodownload hello fájl tooa helyi fájl tartalmát.</span><span class="sxs-lookup"><span data-stu-id="a6bd3-161">Alternatively, you can use hello **download_to_file** method toodownload hello contents of a file tooa local file.</span></span> <span data-ttu-id="a6bd3-162">Használhatja a hello **download_text** metódus toodownload hello tartalmát szöveges karakterláncként fájl.</span><span class="sxs-lookup"><span data-stu-id="a6bd3-162">You can use hello **download_text** method toodownload hello contents of a file as a text string.</span></span>

<span data-ttu-id="a6bd3-163">hello alábbi példában hello **download_to_stream** és **download_text** módszerek toodemonstrate hello fájlokat, amelyek a korábbi szakaszokban létrehozott letöltése.</span><span class="sxs-lookup"><span data-stu-id="a6bd3-163">hello following example uses hello **download_to_stream** and **download_text** methods toodemonstrate downloading hello files, which were created in previous sections.</span></span>

```cpp
// Download as text
azure::storage::cloud_file text_file = 
  root_dir.get_file_reference(_XPLATSTR("my-sample-file-2"));
utility::string_t text = text_file.download_text();
ucout << "File Text: " << text << std::endl;

// Download as a stream.
azure::storage::cloud_file stream_file = 
  root_dir.get_file_reference(_XPLATSTR("my-sample-file-3"));

concurrency::streams::container_buffer<std::vector<uint8_t>> buffer;
concurrency::streams::ostream output_stream(buffer);
stream_file.download_to_stream(output_stream);
std::ofstream outfile("DownloadFile.txt", std::ofstream::binary);
std::vector<unsigned char>& data = buffer.collection();
outfile.write((char *)&data[0], buffer.size());
outfile.close();
```

## <a name="delete-a-file"></a><span data-ttu-id="a6bd3-164">Fájl törlése</span><span class="sxs-lookup"><span data-stu-id="a6bd3-164">Delete a file</span></span>
<span data-ttu-id="a6bd3-165">Közös Azure File storage egy másik művelet a fájl törlése.</span><span class="sxs-lookup"><span data-stu-id="a6bd3-165">Another common Azure File storage operation is file deletion.</span></span> <span data-ttu-id="a6bd3-166">hello alábbira töröl egy fájlt saját-minta-fájl – 3 nevű hello gyökérkönyvtárban tárolja.</span><span class="sxs-lookup"><span data-stu-id="a6bd3-166">hello following code deletes a file named my-sample-file-3 stored under hello root directory.</span></span>

```cpp
// Get a reference toohello root directory for hello share.    
azure::storage::cloud_file_share share = 
  file_client.get_share_reference(_XPLATSTR("my-sample-share"));

azure::storage::cloud_file_directory root_dir = 
  share.get_root_directory_reference();

azure::storage::cloud_file file = 
  root_dir.get_file_reference(_XPLATSTR("my-sample-file-3"));

file.delete_file_if_exists();
```

## <a name="set-hello-quota-maximum-size-for-an-azure-file-share"></a><span data-ttu-id="a6bd3-167">Egy Azure fájlmegosztás hello kvótájának (maximális méretének) beállítása</span><span class="sxs-lookup"><span data-stu-id="a6bd3-167">Set hello quota (maximum size) for an Azure File share</span></span>
<span data-ttu-id="a6bd3-168">Beállíthat hello kvótát (vagy maximális méretet) egy fájlmegosztáshoz GB-ban.</span><span class="sxs-lookup"><span data-stu-id="a6bd3-168">You can set hello quota (or maximum size) for a file share, in gigabytes.</span></span> <span data-ttu-id="a6bd3-169">Mennyi adatot hello megosztáson tárolt toosee is ellenőrizheti.</span><span class="sxs-lookup"><span data-stu-id="a6bd3-169">You can also check toosee how much data is currently stored on hello share.</span></span>

<span data-ttu-id="a6bd3-170">Által beállítás hello kvótát egy megosztáshoz korlátozhatja a hello hello tárolják hello fájlok összesített mérete.</span><span class="sxs-lookup"><span data-stu-id="a6bd3-170">By setting hello quota for a share, you can limit hello total size of hello files stored on hello share.</span></span> <span data-ttu-id="a6bd3-171">Ha hello hello megosztáson található fájlok összesített mérete meghaladja hello kvóta hello megosztáson, akkor az ügyfelek meglévő fájlok mérete nem lehet tooincrease hello vagy fogja új fájlok létrehozása, kivéve azokat, amelyek üresek.</span><span class="sxs-lookup"><span data-stu-id="a6bd3-171">If hello total size of files on hello share exceeds hello quota set on hello share, then clients will be unable tooincrease hello size of existing files or create new files, unless those files are empty.</span></span>

<span data-ttu-id="a6bd3-172">hello az alábbi példa bemutatja, hogyan toocheck hello egy megosztás aktuális kihasználását, és hogyan tooset hello hello fájlmegosztás kvótája.</span><span class="sxs-lookup"><span data-stu-id="a6bd3-172">hello example below shows how toocheck hello current usage for a share and how tooset hello quota for hello share.</span></span>

```cpp
// Parse hello connection string for hello storage account.
azure::storage::cloud_storage_account storage_account = 
  azure::storage::cloud_storage_account::parse(storage_connection_string);

// Create hello file client.
azure::storage::cloud_file_client file_client = 
  storage_account.create_cloud_file_client();

// Get a reference toohello share.
azure::storage::cloud_file_share share = 
  file_client.get_share_reference(_XPLATSTR("my-sample-share"));
if (share.exists())
{
    std::cout << "Current share usage: " << share.download_share_usage() << "/" << share.properties().quota();

    //This line sets hello quota too2560GB
    share.resize(2560);

    std::cout << "Quota increased: " << share.download_share_usage() << "/" << share.properties().quota();

}
```

## <a name="generate-a-shared-access-signature-for-a-file-or-file-share"></a><span data-ttu-id="a6bd3-173">Közös hozzáférésű jogosultságkód létrehozása egy fájlhoz vagy fájlmegosztáshoz</span><span class="sxs-lookup"><span data-stu-id="a6bd3-173">Generate a shared access signature for a file or file share</span></span>
<span data-ttu-id="a6bd3-174">Létrehozhat egy közös hozzáférésű jogosultságkód (SAS) egy fájlmegosztáshoz vagy fájlhoz.</span><span class="sxs-lookup"><span data-stu-id="a6bd3-174">You can generate a shared access signature (SAS) for a file share or for an individual file.</span></span> <span data-ttu-id="a6bd3-175">Is létrehozhat egy megosztott elérési házirendet egy fájlmegosztási toomanage megosztott hozzáférést aláírások.</span><span class="sxs-lookup"><span data-stu-id="a6bd3-175">You can also create a shared access policy on a file share toomanage shared access signatures.</span></span> <span data-ttu-id="a6bd3-176">Megosztott hozzáférési házirend létrehozása az ajánlott, mivel hello SAS visszavonása, ha kell sérülhet módszert biztosít.</span><span class="sxs-lookup"><span data-stu-id="a6bd3-176">Creating a shared access policy is recommended, as it provides a means of revoking hello SAS if it should be compromised.</span></span>

<span data-ttu-id="a6bd3-177">a következő példa hello hoz létre egy megosztott elérési házirendet egy megosztáson, és akkor használja, hogy használ-e tooprovide hello Házirendmegkötések tartozó SAS korlátozására található hello fájlhoz.</span><span class="sxs-lookup"><span data-stu-id="a6bd3-177">hello following example creates a shared access policy on a share, and then uses that policy tooprovide hello constraints for a SAS on a file in hello share.</span></span>

```cpp
// Parse hello connection string for hello storage account.
azure::storage::cloud_storage_account storage_account = 
  azure::storage::cloud_storage_account::parse(storage_connection_string);

// Create hello file client and get a reference toohello share
azure::storage::cloud_file_client file_client = 
  storage_account.create_cloud_file_client();

azure::storage::cloud_file_share share = 
  file_client.get_share_reference(_XPLATSTR("my-sample-share"));

if (share.exists())
{
    // Create and assign a policy
    utility::string_t policy_name = _XPLATSTR("sampleSharePolicy");

    azure::storage::file_shared_access_policy sharedPolicy = 
      azure::storage::file_shared_access_policy();

    //set permissions tooexpire in 90 minutes
    sharedPolicy.set_expiry(utility::datetime::utc_now() + 
       utility::datetime::from_minutes(90));

    //give read and write permissions
    sharedPolicy.set_permissions(azure::storage::file_shared_access_policy::permissions::write | azure::storage::file_shared_access_policy::permissions::read);

    //set permissions for hello share
    azure::storage::file_share_permissions permissions;    

    //retrieve hello current list of shared access policies
    azure::storage::shared_access_policies<azure::storage::file_shared_access_policy> policies;

    //add hello new shared policy
    policies.insert(std::make_pair(policy_name, sharedPolicy));

    //save hello updated policy list
    permissions.set_policies(policies);
    share.upload_permissions(permissions);

    //Retrieve hello root directory and file references
    azure::storage::cloud_file_directory root_dir = 
        share.get_root_directory_reference();
    azure::storage::cloud_file file = 
      root_dir.get_file_reference(_XPLATSTR("my-sample-file-1"));

    // Generate a SAS for a file in hello share 
    //  and associate this access policy with it.        
    utility::string_t sas_token = file.get_shared_access_signature(sharedPolicy);

    // Create a new CloudFile object from hello SAS, and write some text toohello file.        
    azure::storage::cloud_file file_with_sas(azure::storage::storage_credentials(sas_token).transform_uri(file.uri().primary_uri()));
    utility::string_t text = _XPLATSTR("My sample content");        
    file_with_sas.upload_text(text);        

    //Download and print URL with SAS.
    utility::string_t downloaded_text = file_with_sas.download_text();        
    ucout << downloaded_text << std::endl;        
    ucout << azure::storage::storage_credentials(sas_token).transform_uri(file.uri().primary_uri()).to_string() << std::endl;

}
```
## <a name="next-steps"></a><span data-ttu-id="a6bd3-178">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="a6bd3-178">Next steps</span></span>
<span data-ttu-id="a6bd3-179">További információ az Azure Storage toolearn ismerheti meg ezeket az erőforrásokat:</span><span class="sxs-lookup"><span data-stu-id="a6bd3-179">toolearn more about Azure Storage, explore these resources:</span></span>

* [<span data-ttu-id="a6bd3-180">A Storage ügyféloldali kódtára a C++ programnyelvhez</span><span class="sxs-lookup"><span data-stu-id="a6bd3-180">Storage Client Library for C++</span></span>](https://github.com/Azure/azure-storage-cpp)
* <span data-ttu-id="a6bd3-181">[Az azure Storage szolgáltatás fájlminták c++] (https://github.com/Azure-Samples/storage-file-cpp-getting-started)</span><span class="sxs-lookup"><span data-stu-id="a6bd3-181">[Azure Storage File Service Samples in C++] (https://github.com/Azure-Samples/storage-file-cpp-getting-started)</span></span>
* [<span data-ttu-id="a6bd3-182">Azure Storage Explorer</span><span class="sxs-lookup"><span data-stu-id="a6bd3-182">Azure Storage Explorer</span></span>](http://go.microsoft.com/fwlink/?LinkID=822673&clcid=0x409)
* [<span data-ttu-id="a6bd3-183">Az Azure Storage dokumentációja</span><span class="sxs-lookup"><span data-stu-id="a6bd3-183">Azure Storage Documentation</span></span>](https://azure.microsoft.com/documentation/services/storage/)