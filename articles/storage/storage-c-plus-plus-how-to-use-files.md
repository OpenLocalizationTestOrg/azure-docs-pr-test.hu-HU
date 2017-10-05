---
title: "Az Azure File storage a C++ fejlesztése |} Microsoft Docs"
description: "Ismerje meg, hogyan fejleszthet C++ alkalmazások és szolgáltatások Azure File storage használatával tárolhatja a fájljait."
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
ms.openlocfilehash: fc0d8451442f1337db4a36718c3fc746f8eb5125
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/03/2017
---
# <a name="develop-for-azure-file-storage-with-c"></a><span data-ttu-id="aae1e-103">Az Azure File storage a C++ fejlesztése</span><span class="sxs-lookup"><span data-stu-id="aae1e-103">Develop for Azure File storage with C++</span></span>
[!INCLUDE [storage-selector-file-include](../../includes/storage-selector-file-include.md)]

[!INCLUDE [storage-try-azure-tools-files](../../includes/storage-try-azure-tools-files.md)]

## <a name="about-this-tutorial"></a><span data-ttu-id="aae1e-104">Az oktatóanyag ismertetése</span><span class="sxs-lookup"><span data-stu-id="aae1e-104">About this tutorial</span></span>

<span data-ttu-id="aae1e-105">Ebben az oktatóanyagban, megtudhatja, hogyan az Azure File storage alapvető műveletek végrehajtása.</span><span class="sxs-lookup"><span data-stu-id="aae1e-105">In this tutorial, you'll learn how to perform basic operations on Azure File storage.</span></span> <span data-ttu-id="aae1e-106">Írt C++ minták segítségével megtudhatja, hogyan megosztások létrehozása és könyvtárak, feltöltése, listázása és fájlok törlése.</span><span class="sxs-lookup"><span data-stu-id="aae1e-106">Through samples written in C++, you'll learn how to create shares and directories, upload, list, and delete files.</span></span> <span data-ttu-id="aae1e-107">Ha most ismerkedik az Azure File storage, a minták hasznos információkat a következő szakaszok a jellemzői áthaladás lesz.</span><span class="sxs-lookup"><span data-stu-id="aae1e-107">If you are new to Azure File storage , going through the concepts in the sections that follow will be helpful in understanding the samples.</span></span>


* <span data-ttu-id="aae1e-108">Hozzon létre vagy töröljön az Azure fájlmegosztások</span><span class="sxs-lookup"><span data-stu-id="aae1e-108">Create and delete Azure File shares</span></span>
* <span data-ttu-id="aae1e-109">Hozzon létre vagy töröljön a könyvtárak</span><span class="sxs-lookup"><span data-stu-id="aae1e-109">Create and delete directories</span></span>
* <span data-ttu-id="aae1e-110">Fájlok és könyvtárak egy Azure fájlmegosztás számbavétele</span><span class="sxs-lookup"><span data-stu-id="aae1e-110">Enumerate files and directories in an Azure File share</span></span>
* <span data-ttu-id="aae1e-111">Töltse fel, töltse le és törölje a fájlt</span><span class="sxs-lookup"><span data-stu-id="aae1e-111">Upload, download, and delete a file</span></span>
* <span data-ttu-id="aae1e-112">Egy Azure fájlmegosztás kvótájának (maximális méretének) beállítása</span><span class="sxs-lookup"><span data-stu-id="aae1e-112">Set the quota (maximum size) for an Azure File share</span></span>
* <span data-ttu-id="aae1e-113">Közös hozzáférésű jogosultságkód (SAS-kulcs) létrehozása egy, a megosztásban meghatározott megosztott elérési házirendet használó fájlhoz.</span><span class="sxs-lookup"><span data-stu-id="aae1e-113">Create a shared access signature (SAS key) for a file that uses a shared access policy defined on the share.</span></span>

> [!Note]  
> <span data-ttu-id="aae1e-114">Mivel előfordulhat, hogy Azure fájltároló SMB-n keresztül érhető el, akkor lehet egyszerű alkalmazások írását, amelyek a szabványos C++ i/o-osztályok és függvény használata Azure fájlmegosztás eléréséhez.</span><span class="sxs-lookup"><span data-stu-id="aae1e-114">Because Azure File storage may be accessed over SMB, it is possible to write simple applications that access the Azure File share using the standard C++ I/O classes and functions.</span></span> <span data-ttu-id="aae1e-115">Ez a cikk azt ismerteti, hogyan alkalmazások írását, amelyek az Azure Storage C++ SDK-val, használja a [Azure File storage REST API](https://docs.microsoft.com/rest/api/storageservices/fileservices/file-service-rest-api) felvegye a Azure File storage.</span><span class="sxs-lookup"><span data-stu-id="aae1e-115">This article will describe how to write applications that use the Azure Storage C++ SDK, which uses the [Azure File storage REST API](https://docs.microsoft.com/rest/api/storageservices/fileservices/file-service-rest-api) to talk to Azure File storage.</span></span>

## <a name="create-a-c-application"></a><span data-ttu-id="aae1e-116">A C++-alkalmazás létrehozása</span><span class="sxs-lookup"><span data-stu-id="aae1e-116">Create a C++ application</span></span>
<span data-ttu-id="aae1e-117">A minták létrehozásához szüksége lesz az Azure Storage ügyféloldali kódtár 2.4.0 for C++ telepítése.</span><span class="sxs-lookup"><span data-stu-id="aae1e-117">To build the samples, you will need to install the Azure Storage Client Library 2.4.0 for C++.</span></span> <span data-ttu-id="aae1e-118">Meg is létrehozott egy Azure storage-fiókot.</span><span class="sxs-lookup"><span data-stu-id="aae1e-118">You should also have created an Azure storage account.</span></span>

<span data-ttu-id="aae1e-119">Az Azure Storage ügyfél 2.4.0 telepítésében C++, a következő módszerek egyikét használhatja:</span><span class="sxs-lookup"><span data-stu-id="aae1e-119">To install the Azure Storage Client 2.4.0 for C++, you can use one of the following methods:</span></span>

* <span data-ttu-id="aae1e-120">**Linux:** megadott kövesse a [Azure Storage ügyféloldali kódtára a C++ információs](https://github.com/Azure/azure-storage-cpp/blob/master/README.md) lap.</span><span class="sxs-lookup"><span data-stu-id="aae1e-120">**Linux:** Follow the instructions given in the [Azure Storage Client Library for C++ README](https://github.com/Azure/azure-storage-cpp/blob/master/README.md) page.</span></span>
* <span data-ttu-id="aae1e-121">**Windows:** a Visual Studióban kattintson **eszközök &gt; NuGet-Csomagkezelő &gt; Csomagkezelő konzol**.</span><span class="sxs-lookup"><span data-stu-id="aae1e-121">**Windows:** In Visual Studio, click **Tools &gt; NuGet Package Manager &gt; Package Manager Console**.</span></span> <span data-ttu-id="aae1e-122">Írja be a következő parancsot a [NuGet Package Manager console](http://docs.nuget.org/docs/start-here/using-the-package-manager-console) nyomja le az ENTER **ENTER**.</span><span class="sxs-lookup"><span data-stu-id="aae1e-122">Type the following command into the [NuGet Package Manager console](http://docs.nuget.org/docs/start-here/using-the-package-manager-console) and press **ENTER**.</span></span>
  
```
Install-Package wastorage
```

## <a name="set-up-your-application-to-use-azure-file-storage"></a><span data-ttu-id="aae1e-123">Állítsa be az alkalmazás Azure File storage használata</span><span class="sxs-lookup"><span data-stu-id="aae1e-123">Set up your application to use Azure File storage</span></span>
<span data-ttu-id="aae1e-124">Adja hozzá a következő utasításokat a felső részén a C++ forrásfájl kezelheti az Azure File storage helyét tartalmazza:</span><span class="sxs-lookup"><span data-stu-id="aae1e-124">Add the following include statements to the top of the C++ source file where you want to manipulate Azure File storage:</span></span>

```cpp
#include <was/storage_account.h>
#include <was/file.h>
```

## <a name="set-up-an-azure-storage-connection-string"></a><span data-ttu-id="aae1e-125">Egy Azure storage kapcsolati karakterlánc beállítása</span><span class="sxs-lookup"><span data-stu-id="aae1e-125">Set up an Azure storage connection string</span></span>
<span data-ttu-id="aae1e-126">A File storage használatához szüksége az Azure storage-fiókjához.</span><span class="sxs-lookup"><span data-stu-id="aae1e-126">To use File storage, you need to connect to your Azure storage account.</span></span> <span data-ttu-id="aae1e-127">Az első lépés is konfigurálhat egy kapcsolati karakterláncot, amely csatlakozni a tárfiókhoz fogjuk használni.</span><span class="sxs-lookup"><span data-stu-id="aae1e-127">The first step would be to configure a connection string, which we'll use to connect to your storage account.</span></span> <span data-ttu-id="aae1e-128">Nézzük meg egy statikus változót ehhez.</span><span class="sxs-lookup"><span data-stu-id="aae1e-128">Let's define a static variable to do that.</span></span>

```cpp
// Define the connection-string with your values.
const utility::string_t 
storage_connection_string(U("DefaultEndpointsProtocol=https;AccountName=your_storage_account;AccountKey=your_storage_account_key"));
```

## <a name="connecting-to-an-azure-storage-account"></a><span data-ttu-id="aae1e-129">Az Azure storage-fiókhoz való csatlakozást</span><span class="sxs-lookup"><span data-stu-id="aae1e-129">Connecting to an Azure storage account</span></span>
<span data-ttu-id="aae1e-130">Használhatja a **cloud_storage_account** osztályt határoz meg a Tárfiók adatait.</span><span class="sxs-lookup"><span data-stu-id="aae1e-130">You can use the **cloud_storage_account** class to represent your Storage Account information.</span></span> <span data-ttu-id="aae1e-131">A tárfiók adatait a tárolási kapcsolati karakterlánc lekéréséhez használja a **elemezni** metódust.</span><span class="sxs-lookup"><span data-stu-id="aae1e-131">To retrieve your storage account information from the storage connection string, you can use the **parse** method.</span></span>

```cpp
// Retrieve storage account from connection string.    
azure::storage::cloud_storage_account storage_account = 
  azure::storage::cloud_storage_account::parse(storage_connection_string);
```

## <a name="create-an-azure-file-share"></a><span data-ttu-id="aae1e-132">Azure-fájlmegosztás létrehozása</span><span class="sxs-lookup"><span data-stu-id="aae1e-132">Create an Azure File share</span></span>
<span data-ttu-id="aae1e-133">A fájlok és könyvtárak az Azure File storage találhatók nevű tároló egy **megosztás**.</span><span class="sxs-lookup"><span data-stu-id="aae1e-133">All files and directories in Azure File storage reside in a container called a **Share**.</span></span> <span data-ttu-id="aae1e-134">A tárfiók lehet a kapacitásával lehetővé teszi tetszőleges számú megosztások.</span><span class="sxs-lookup"><span data-stu-id="aae1e-134">Your storage account can have as many shares as your account capacity allows.</span></span> <span data-ttu-id="aae1e-135">Történő hozzáférés a megosztást és tartalmát, Azure File storage-ügyfél használata szükséges.</span><span class="sxs-lookup"><span data-stu-id="aae1e-135">To obtain access to a share and its contents, you need to use a Azure File storage client.</span></span>

```cpp
// Create the Azure File storage client.
azure::storage::cloud_file_client file_client = 
  storage_account.create_cloud_file_client();
```

<span data-ttu-id="aae1e-136">Az Azure File storage ügyfélprogrammal, majd szerezhet be egy hivatkozást egy megosztásra.</span><span class="sxs-lookup"><span data-stu-id="aae1e-136">Using the Azure File storage client, you can then obtain a reference to a share.</span></span>

```cpp
// Get a reference to the file share
azure::storage::cloud_file_share share = 
  file_client.get_share_reference(_XPLATSTR("my-sample-share"));
```

<span data-ttu-id="aae1e-137">A megosztás létrehozásához használja a **create_if_not_exists** metódusában a **cloud_file_share** objektum.</span><span class="sxs-lookup"><span data-stu-id="aae1e-137">To create the share, use the **create_if_not_exists** method of the **cloud_file_share** object.</span></span>

```cpp
if (share.create_if_not_exists()) {    
    std::wcout << U("New share created") << std::endl;    
}
```

<span data-ttu-id="aae1e-138">Ezen a ponton **megosztása** nevű megosztáshoz egy hivatkozást tartalmaz **a minta-megosztás**.</span><span class="sxs-lookup"><span data-stu-id="aae1e-138">At this point, **share** holds a reference to a share named **my-sample-share**.</span></span>

## <a name="delete-an-azure-file-share"></a><span data-ttu-id="aae1e-139">Egy Azure fájlmegosztás törlése</span><span class="sxs-lookup"><span data-stu-id="aae1e-139">Delete an Azure File share</span></span>
<span data-ttu-id="aae1e-140">Egy megosztás törlése meghívásával történik a **delete_if_exists** cloud_file_share objektum metódus.</span><span class="sxs-lookup"><span data-stu-id="aae1e-140">Deleting a share is done by calling the **delete_if_exists** method on a cloud_file_share object.</span></span> <span data-ttu-id="aae1e-141">Itt látható mintakód, amelyet, amely.</span><span class="sxs-lookup"><span data-stu-id="aae1e-141">Here's sample code that does that.</span></span>

```cpp
// Get a reference to the share.
azure::storage::cloud_file_share share = 
  file_client.get_share_reference(_XPLATSTR("my-sample-share"));

// delete the share if exists
share.delete_share_if_exists();
```

## <a name="create-a-directory"></a><span data-ttu-id="aae1e-142">Könyvtár létrehozása</span><span class="sxs-lookup"><span data-stu-id="aae1e-142">Create a directory</span></span>
<span data-ttu-id="aae1e-143">Tároló üzembe alkönyvtárak így ahelyett hogy ezek a gyökérkönyvtárban található fájlok rendezhetők.</span><span class="sxs-lookup"><span data-stu-id="aae1e-143">You can organize storage by putting files inside subdirectories instead of having all of them in the root directory.</span></span> <span data-ttu-id="aae1e-144">Az Azure File storage hozhat létre a fiókját engedélyezi a könyvtárat.</span><span class="sxs-lookup"><span data-stu-id="aae1e-144">Azure File storage allows you to create as many directories as your account will allow.</span></span> <span data-ttu-id="aae1e-145">Az alábbi kód létrehoz egy nevű könyvtár **saját minta directory** alatt a gyökérkönyvtár, valamint a nevű **saját minta alkönyvtár**.</span><span class="sxs-lookup"><span data-stu-id="aae1e-145">The code below will create a directory named **my-sample-directory** under the root directory as well as a subdirectory named **my-sample-subdirectory**.</span></span>

```cpp
// Retrieve a reference to a directory
azure::storage::cloud_file_directory directory = share.get_directory_reference(_XPLATSTR("my-sample-directory"));

// Return value is true if the share did not exist and was successfully created.
directory.create_if_not_exists();

// Create a subdirectory.
azure::storage::cloud_file_directory subdirectory = 
  directory.get_subdirectory_reference(_XPLATSTR("my-sample-subdirectory"));
subdirectory.create_if_not_exists();
```

## <a name="delete-a-directory"></a><span data-ttu-id="aae1e-146">Könyvtár törlése</span><span class="sxs-lookup"><span data-stu-id="aae1e-146">Delete a directory</span></span>
<span data-ttu-id="aae1e-147">A könyvtár törlése mely egyszerű feladat, érdemes megjegyezni, hogy nem törölhető a fájlok továbbra is tartalmazó könyvtár vagy más címtárakban.</span><span class="sxs-lookup"><span data-stu-id="aae1e-147">Deleting a directory is a simple task, although it should be noted that you cannot delete a directory that still contains files or other directories.</span></span>

```cpp
// Get a reference to the share.
azure::storage::cloud_file_share share = 
  file_client.get_share_reference(_XPLATSTR("my-sample-share"));

// Get a reference to the directory.
azure::storage::cloud_file_directory directory = 
  share.get_directory_reference(_XPLATSTR("my-sample-directory"));

// Get a reference to the subdirectory you want to delete.
azure::storage::cloud_file_directory sub_directory =
  directory.get_subdirectory_reference(_XPLATSTR("my-sample-subdirectory"));

// Delete the subdirectory and the sample directory.
sub_directory.delete_directory_if_exists();

directory.delete_directory_if_exists();
```

## <a name="enumerate-files-and-directories-in-an-azure-file-share"></a><span data-ttu-id="aae1e-148">Fájlok és könyvtárak egy Azure fájlmegosztás számbavétele</span><span class="sxs-lookup"><span data-stu-id="aae1e-148">Enumerate files and directories in an Azure File share</span></span>
<span data-ttu-id="aae1e-149">Fájlok és könyvtárak egy megosztáson belüli listájának beszerzésével könnyen történik meghívásával **list_files_and_directories** a egy **cloud_file_directory** hivatkozás.</span><span class="sxs-lookup"><span data-stu-id="aae1e-149">Obtaining a list of files and directories within a share is easily done by calling **list_files_and_directories** on a **cloud_file_directory** reference.</span></span> <span data-ttu-id="aae1e-150">Számos tulajdonságai és metódusai visszaadásakor eléréséhez **list_file_and_directory_item**, meg kell hívnia a **list_file_and_directory_item.as_file** metódus használatával kérje le a **cloud_file**  objektum, vagy a **list_file_and_directory_item.as_directory** metódus használatával kérje le a **cloud_file_directory** objektum.</span><span class="sxs-lookup"><span data-stu-id="aae1e-150">To access the rich set of properties and methods for a returned **list_file_and_directory_item**, you must call the **list_file_and_directory_item.as_file** method to get a **cloud_file** object, or the **list_file_and_directory_item.as_directory** method to get a **cloud_file_directory** object.</span></span>

<span data-ttu-id="aae1e-151">A következő kód bemutatja, hogyan kérhető le és küldhető a megosztás a gyökérmappában lévő egyes elemek URI.</span><span class="sxs-lookup"><span data-stu-id="aae1e-151">The following code demonstrates how to retrieve and output the URI of each item in the root directory of the share.</span></span>

```cpp
//Get a reference to the root directory for the share.
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

## <a name="upload-a-file"></a><span data-ttu-id="aae1e-152">Fájl feltöltése</span><span class="sxs-lookup"><span data-stu-id="aae1e-152">Upload a file</span></span>
<span data-ttu-id="aae1e-153">Legalább egy Azure fájlmegosztás gyökérkönyvtár fájlokat tároló is tartalmazza.</span><span class="sxs-lookup"><span data-stu-id="aae1e-153">At the very least, an Azure File share contains a root directory where files can reside.</span></span> <span data-ttu-id="aae1e-154">Ebben a szakaszban megtudhatja, hogyan feltölteni a fájlt a helyi tároló megosztás gyökérkönyvtárában alakzatot.</span><span class="sxs-lookup"><span data-stu-id="aae1e-154">In this section, you'll learn how to upload a file from local storage onto the root directory of a share.</span></span>

<span data-ttu-id="aae1e-155">Az első lépés egy feltöltés az beszerzése hivatkozni kell a könyvtárban, ahol kell tárolni.</span><span class="sxs-lookup"><span data-stu-id="aae1e-155">The first step in uploading a file is to obtain a reference to the directory where it should reside.</span></span> <span data-ttu-id="aae1e-156">Ehhez hívja a **get_root_directory_reference** a megosztás metódusa.</span><span class="sxs-lookup"><span data-stu-id="aae1e-156">You do this by calling the **get_root_directory_reference** method of the share object.</span></span>

```cpp
//Get a reference to the root directory for the share.
azure::storage::cloud_file_directory root_dir = share.get_root_directory_reference();
```

<span data-ttu-id="aae1e-157">Most, hogy a megosztás gyökérkönyvtárában hivatkozik, akkor fájlt tölthet fel rá.</span><span class="sxs-lookup"><span data-stu-id="aae1e-157">Now that you have a reference to the root directory of the share, you can upload a file onto it.</span></span> <span data-ttu-id="aae1e-158">Ebben a példában feltölt egy fájlt, szöveg és a folyam.</span><span class="sxs-lookup"><span data-stu-id="aae1e-158">This example uploads from a file, from text, and from a stream.</span></span>

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

## <a name="download-a-file"></a><span data-ttu-id="aae1e-159">Fájl letöltése</span><span class="sxs-lookup"><span data-stu-id="aae1e-159">Download a file</span></span>
<span data-ttu-id="aae1e-160">Fájlok letöltéséhez először kérjen le egy hivatkozást, és, majd hívja a **download_to_stream** módszerrel kell továbbítania a tartalmát egy stream objektumra, amely meg tudja majd továbbra is fennáll egy helyi fájlba.</span><span class="sxs-lookup"><span data-stu-id="aae1e-160">To download files, first retrieve a file reference and then call the **download_to_stream** method to transfer the file contents to a stream object, which you can then persist to a local file.</span></span> <span data-ttu-id="aae1e-161">Másik lehetőségként használhatja a **download_to_file** metódus egy fájl tartalmát egy helyi fájl letöltéséhez.</span><span class="sxs-lookup"><span data-stu-id="aae1e-161">Alternatively, you can use the **download_to_file** method to download the contents of a file to a local file.</span></span> <span data-ttu-id="aae1e-162">Használhatja a **download_text** metódus egy szöveges karakterlánc egy fájlhoz, a tartalom letöltéséhez.</span><span class="sxs-lookup"><span data-stu-id="aae1e-162">You can use the **download_text** method to download the contents of a file as a text string.</span></span>

<span data-ttu-id="aae1e-163">Az alábbi példában a **download_to_stream** és **download_text** módszerek letölti a fájlokat, amelyek a korábbi szakaszokban létrehozott bemutatásához.</span><span class="sxs-lookup"><span data-stu-id="aae1e-163">The following example uses the **download_to_stream** and **download_text** methods to demonstrate downloading the files, which were created in previous sections.</span></span>

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

## <a name="delete-a-file"></a><span data-ttu-id="aae1e-164">Fájl törlése</span><span class="sxs-lookup"><span data-stu-id="aae1e-164">Delete a file</span></span>
<span data-ttu-id="aae1e-165">Közös Azure File storage egy másik művelet a fájl törlése.</span><span class="sxs-lookup"><span data-stu-id="aae1e-165">Another common Azure File storage operation is file deletion.</span></span> <span data-ttu-id="aae1e-166">Az alábbi kód töröl egy fájlt, a-minta-fájl – 3 nevű tárolja a gyökérkönyvtárban.</span><span class="sxs-lookup"><span data-stu-id="aae1e-166">The following code deletes a file named my-sample-file-3 stored under the root directory.</span></span>

```cpp
// Get a reference to the root directory for the share.    
azure::storage::cloud_file_share share = 
  file_client.get_share_reference(_XPLATSTR("my-sample-share"));

azure::storage::cloud_file_directory root_dir = 
  share.get_root_directory_reference();

azure::storage::cloud_file file = 
  root_dir.get_file_reference(_XPLATSTR("my-sample-file-3"));

file.delete_file_if_exists();
```

## <a name="set-the-quota-maximum-size-for-an-azure-file-share"></a><span data-ttu-id="aae1e-167">Egy Azure fájlmegosztás kvótájának (maximális méretének) beállítása</span><span class="sxs-lookup"><span data-stu-id="aae1e-167">Set the quota (maximum size) for an Azure File share</span></span>
<span data-ttu-id="aae1e-168">Beállíthatja a kvótát (vagy maximális méretet) egy fájlmegosztáshoz GB-ban.</span><span class="sxs-lookup"><span data-stu-id="aae1e-168">You can set the quota (or maximum size) for a file share, in gigabytes.</span></span> <span data-ttu-id="aae1e-169">Azt is ellenőrizheti, hogy aktuálisan mennyi adatot tárol a fájlmegosztás.</span><span class="sxs-lookup"><span data-stu-id="aae1e-169">You can also check to see how much data is currently stored on the share.</span></span>

<span data-ttu-id="aae1e-170">Ha beállít egy kvótát egy megosztáshoz, korlátozhatja a megosztáson tárolt fájlok összesített méretét.</span><span class="sxs-lookup"><span data-stu-id="aae1e-170">By setting the quota for a share, you can limit the total size of the files stored on the share.</span></span> <span data-ttu-id="aae1e-171">Ha a megosztásban található fájlok teljes mérete meghaladja a megosztáshoz beállított kvótát, az ügyfelek nem növelhetik tovább a meglévő fájlok méretét, és csak olyan új fájlokat hozhatnak létre, amelyek üresek.</span><span class="sxs-lookup"><span data-stu-id="aae1e-171">If the total size of files on the share exceeds the quota set on the share, then clients will be unable to increase the size of existing files or create new files, unless those files are empty.</span></span>

<span data-ttu-id="aae1e-172">Az alábbi példa bemutatja, hogyan ellenőrizheti egy megosztás aktuális kihasználását, és hogyan adhat meg hozzá kvótát.</span><span class="sxs-lookup"><span data-stu-id="aae1e-172">The example below shows how to check the current usage for a share and how to set the quota for the share.</span></span>

```cpp
// Parse the connection string for the storage account.
azure::storage::cloud_storage_account storage_account = 
  azure::storage::cloud_storage_account::parse(storage_connection_string);

// Create the file client.
azure::storage::cloud_file_client file_client = 
  storage_account.create_cloud_file_client();

// Get a reference to the share.
azure::storage::cloud_file_share share = 
  file_client.get_share_reference(_XPLATSTR("my-sample-share"));
if (share.exists())
{
    std::cout << "Current share usage: " << share.download_share_usage() << "/" << share.properties().quota();

    //This line sets the quota to 2560GB
    share.resize(2560);

    std::cout << "Quota increased: " << share.download_share_usage() << "/" << share.properties().quota();

}
```

## <a name="generate-a-shared-access-signature-for-a-file-or-file-share"></a><span data-ttu-id="aae1e-173">Közös hozzáférésű jogosultságkód létrehozása egy fájlhoz vagy fájlmegosztáshoz</span><span class="sxs-lookup"><span data-stu-id="aae1e-173">Generate a shared access signature for a file or file share</span></span>
<span data-ttu-id="aae1e-174">Létrehozhat egy közös hozzáférésű jogosultságkód (SAS) egy fájlmegosztáshoz vagy fájlhoz.</span><span class="sxs-lookup"><span data-stu-id="aae1e-174">You can generate a shared access signature (SAS) for a file share or for an individual file.</span></span> <span data-ttu-id="aae1e-175">Létrehozhat egy megosztott elérési házirendet is egy fájlmegosztáshoz, hogy kezelni tudja a közös hozzáférésű jogosultságkódokat.</span><span class="sxs-lookup"><span data-stu-id="aae1e-175">You can also create a shared access policy on a file share to manage shared access signatures.</span></span> <span data-ttu-id="aae1e-176">Azért érdemes létrehozni megosztott elérési házirendet, mert annak az eszközeivel vissza lehet hívni az SAS-t, amennyiben sérülne a biztonsága.</span><span class="sxs-lookup"><span data-stu-id="aae1e-176">Creating a shared access policy is recommended, as it provides a means of revoking the SAS if it should be compromised.</span></span>

<span data-ttu-id="aae1e-177">Az alábbi példa létrehoz egy megosztott elérési házirendet egy megosztáson, majd felhasználja a házirendet egy, a megosztásban található fájlhoz tartozó SAS korlátozására.</span><span class="sxs-lookup"><span data-stu-id="aae1e-177">The following example creates a shared access policy on a share, and then uses that policy to provide the constraints for a SAS on a file in the share.</span></span>

```cpp
// Parse the connection string for the storage account.
azure::storage::cloud_storage_account storage_account = 
  azure::storage::cloud_storage_account::parse(storage_connection_string);

// Create the file client and get a reference to the share
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

    //set permissions to expire in 90 minutes
    sharedPolicy.set_expiry(utility::datetime::utc_now() + 
       utility::datetime::from_minutes(90));

    //give read and write permissions
    sharedPolicy.set_permissions(azure::storage::file_shared_access_policy::permissions::write | azure::storage::file_shared_access_policy::permissions::read);

    //set permissions for the share
    azure::storage::file_share_permissions permissions;    

    //retrieve the current list of shared access policies
    azure::storage::shared_access_policies<azure::storage::file_shared_access_policy> policies;

    //add the new shared policy
    policies.insert(std::make_pair(policy_name, sharedPolicy));

    //save the updated policy list
    permissions.set_policies(policies);
    share.upload_permissions(permissions);

    //Retrieve the root directory and file references
    azure::storage::cloud_file_directory root_dir = 
        share.get_root_directory_reference();
    azure::storage::cloud_file file = 
      root_dir.get_file_reference(_XPLATSTR("my-sample-file-1"));

    // Generate a SAS for a file in the share 
    //  and associate this access policy with it.        
    utility::string_t sas_token = file.get_shared_access_signature(sharedPolicy);

    // Create a new CloudFile object from the SAS, and write some text to the file.        
    azure::storage::cloud_file file_with_sas(azure::storage::storage_credentials(sas_token).transform_uri(file.uri().primary_uri()));
    utility::string_t text = _XPLATSTR("My sample content");        
    file_with_sas.upload_text(text);        

    //Download and print URL with SAS.
    utility::string_t downloaded_text = file_with_sas.download_text();        
    ucout << downloaded_text << std::endl;        
    ucout << azure::storage::storage_credentials(sas_token).transform_uri(file.uri().primary_uri()).to_string() << std::endl;

}
```
## <a name="next-steps"></a><span data-ttu-id="aae1e-178">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="aae1e-178">Next steps</span></span>
<span data-ttu-id="aae1e-179">Az alábbi forrásokból többet is megtudhat az Azure Storage-ról:</span><span class="sxs-lookup"><span data-stu-id="aae1e-179">To learn more about Azure Storage, explore these resources:</span></span>

* [<span data-ttu-id="aae1e-180">A Storage ügyféloldali kódtára a C++ programnyelvhez</span><span class="sxs-lookup"><span data-stu-id="aae1e-180">Storage Client Library for C++</span></span>](https://github.com/Azure/azure-storage-cpp)
* <span data-ttu-id="aae1e-181">[Az azure Storage szolgáltatás fájlminták c++] (https://github.com/Azure-Samples/storage-file-cpp-getting-started)</span><span class="sxs-lookup"><span data-stu-id="aae1e-181">[Azure Storage File Service Samples in C++] (https://github.com/Azure-Samples/storage-file-cpp-getting-started)</span></span>
* [<span data-ttu-id="aae1e-182">Azure Storage Explorer</span><span class="sxs-lookup"><span data-stu-id="aae1e-182">Azure Storage Explorer</span></span>](http://go.microsoft.com/fwlink/?LinkID=822673&clcid=0x409)
* [<span data-ttu-id="aae1e-183">Az Azure Storage dokumentációja</span><span class="sxs-lookup"><span data-stu-id="aae1e-183">Azure Storage Documentation</span></span>](https://azure.microsoft.com/documentation/services/storage/)