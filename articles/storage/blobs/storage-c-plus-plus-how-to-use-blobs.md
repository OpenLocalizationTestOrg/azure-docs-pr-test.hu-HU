---
title: aaaHow toouse a blob storage (object storage) a C++ |} Microsoft Docs
description: "Strukturálatlan adatok tárolása az Azure Blob storage (object storage) hello felhő."
services: storage
documentationcenter: .net
author: michaelhauss
manager: vamshik
editor: tysonn
ms.assetid: 53844120-1c48-4e2f-8f77-5359ed0147a4
ms.service: storage
ms.workload: storage
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/11/2017
ms.author: michaelhauss
ms.openlocfilehash: e63df4683e5c10c9f8fbe4106c655df61be0e1a6
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="how-toouse-blob-storage-from-c"></a><span data-ttu-id="2d77c-103">Hogyan toouse Blob Storage-ának C++</span><span class="sxs-lookup"><span data-stu-id="2d77c-103">How toouse Blob Storage from C++</span></span>
[!INCLUDE [storage-selector-blob-include](../../../includes/storage-selector-blob-include.md)]

[!INCLUDE [storage-try-azure-tools-blobs](../../../includes/storage-try-azure-tools-blobs.md)]

## <a name="overview"></a><span data-ttu-id="2d77c-104">Áttekintés</span><span class="sxs-lookup"><span data-stu-id="2d77c-104">Overview</span></span>
<span data-ttu-id="2d77c-105">Az Azure Blob storage egy olyan szolgáltatás, amely hello felhő strukturálatlan adatokat objektumként/blobként tárolja.</span><span class="sxs-lookup"><span data-stu-id="2d77c-105">Azure Blob storage is a service that stores unstructured data in hello cloud as objects/blobs.</span></span> <span data-ttu-id="2d77c-106">A Blob Storage képes tárolni bármilyen szöveget vagy bináris adatot, például dokumentumot, médiafájlt vagy egy alkalmazástelepítőt.</span><span class="sxs-lookup"><span data-stu-id="2d77c-106">Blob storage can store any type of text or binary data, such as a document, media file, or application installer.</span></span> <span data-ttu-id="2d77c-107">A BLOB storage is az említett tooas objektum tároló.</span><span class="sxs-lookup"><span data-stu-id="2d77c-107">Blob storage is also referred tooas object storage.</span></span>

<span data-ttu-id="2d77c-108">Ez az útmutató mutatni, hogyan tooperform szolgáltatást használó általános forgatókönyvhöz hello Azure Blob storage szolgáltatásban.</span><span class="sxs-lookup"><span data-stu-id="2d77c-108">This guide will demonstrate how tooperform common scenarios using hello Azure Blob storage service.</span></span> <span data-ttu-id="2d77c-109">hello minták C++ nyelven íródtak, és használja a hello [Azure Storage ügyféloldali kódtára a C++](http://github.com/Azure/azure-storage-cpp/blob/master/README.md).</span><span class="sxs-lookup"><span data-stu-id="2d77c-109">hello samples are written in C++ and use hello [Azure Storage Client Library for C++](http://github.com/Azure/azure-storage-cpp/blob/master/README.md).</span></span> <span data-ttu-id="2d77c-110">hello tárgyalt forgatókönyvekben szerepel a **feltöltése**, **felsoroló**, **letöltése**, és **törlése** blobokat.</span><span class="sxs-lookup"><span data-stu-id="2d77c-110">hello scenarios covered include **uploading**, **listing**, **downloading**, and **deleting** blobs.</span></span>  

> [!NOTE]
> <span data-ttu-id="2d77c-111">Ez az útmutató célok hello a Azure Storage ügyféloldali kódtára a C++ 1.0.0 verzió vagy újabb.</span><span class="sxs-lookup"><span data-stu-id="2d77c-111">This guide targets hello Azure Storage Client Library for C++ version 1.0.0 and above.</span></span> <span data-ttu-id="2d77c-112">hello ajánlott verzió a Storage ügyféloldali kódtára 2.2.0, amelyik keresztül elérhető [NuGet](http://www.nuget.org/packages/wastorage) vagy [GitHub](https://github.com/Azure/azure-storage-cpp).</span><span class="sxs-lookup"><span data-stu-id="2d77c-112">hello recommended version is Storage Client Library 2.2.0, which is available via [NuGet](http://www.nuget.org/packages/wastorage) or [GitHub](https://github.com/Azure/azure-storage-cpp).</span></span>
> 
> 

[!INCLUDE [storage-blob-concepts-include](../../../includes/storage-blob-concepts-include.md)]

[!INCLUDE [storage-create-account-include](../../../includes/storage-create-account-include.md)]

## <a name="create-a-c-application"></a><span data-ttu-id="2d77c-113">A C++-alkalmazás létrehozása</span><span class="sxs-lookup"><span data-stu-id="2d77c-113">Create a C++ application</span></span>
<span data-ttu-id="2d77c-114">Ez az útmutató egy C++ alkalmazáson belül futtatható tárolási szolgáltatásokkal fog használni.</span><span class="sxs-lookup"><span data-stu-id="2d77c-114">In this guide, you will use storage features which can be run within a C++ application.</span></span>  

<span data-ttu-id="2d77c-115">toodo tooinstall kell tehát hello Azure Storage ügyféloldali kódtára a C++ és az Azure storage-fiók létrehozása az Azure-előfizetése.</span><span class="sxs-lookup"><span data-stu-id="2d77c-115">toodo so, you will need tooinstall hello Azure Storage Client Library for C++ and create an Azure storage account in your Azure subscription.</span></span>   

<span data-ttu-id="2d77c-116">tooinstall hello Azure Storage ügyféloldali kódtára a C++, a következő módszerek hello használhatják:</span><span class="sxs-lookup"><span data-stu-id="2d77c-116">tooinstall hello Azure Storage Client Library for C++, you can use hello following methods:</span></span>

* <span data-ttu-id="2d77c-117">**Linux:** hello megadott hello utasítások [Azure Storage ügyféloldali kódtára a C++ információs](https://github.com/Azure/azure-storage-cpp/blob/master/README.md) lap.</span><span class="sxs-lookup"><span data-stu-id="2d77c-117">**Linux:** Follow hello instructions given in hello [Azure Storage Client Library for C++ README](https://github.com/Azure/azure-storage-cpp/blob/master/README.md) page.</span></span>  
* <span data-ttu-id="2d77c-118">**Windows:** a Visual Studióban kattintson **eszközök > NuGet-Csomagkezelő > Csomagkezelő konzol**.</span><span class="sxs-lookup"><span data-stu-id="2d77c-118">**Windows:** In Visual Studio, click **Tools > NuGet Package Manager > Package Manager Console**.</span></span> <span data-ttu-id="2d77c-119">Típus hello következő parancsot a hello történő [NuGet Package Manager console](http://docs.nuget.org/docs/start-here/using-the-package-manager-console) nyomja le az ENTER **ENTER**.</span><span class="sxs-lookup"><span data-stu-id="2d77c-119">Type hello following command into hello [NuGet Package Manager console](http://docs.nuget.org/docs/start-here/using-the-package-manager-console) and press **ENTER**.</span></span>  
  
     <span data-ttu-id="2d77c-120">Install-Package wastorage</span><span class="sxs-lookup"><span data-stu-id="2d77c-120">Install-Package wastorage</span></span>

## <a name="configure-your-application-tooaccess-blob-storage"></a><span data-ttu-id="2d77c-121">Az alkalmazás tooaccess Blob-tároló konfigurálása</span><span class="sxs-lookup"><span data-stu-id="2d77c-121">Configure your application tooaccess Blob Storage</span></span>
<span data-ttu-id="2d77c-122">Adja hozzá, hello következő tartalmazó utasítások toohello felső részén hello C++ fájl toouse hello az Azure storage API-k tooaccess blobok:</span><span class="sxs-lookup"><span data-stu-id="2d77c-122">Add hello following include statements toohello top of hello C++ file where you want toouse hello Azure storage APIs tooaccess blobs:</span></span>  

```cpp
#include <was/storage_account.h>
#include <was/blob.h>
```

## <a name="setup-an-azure-storage-connection-string"></a><span data-ttu-id="2d77c-123">Egy Azure storage kapcsolati karakterlánc beállítása</span><span class="sxs-lookup"><span data-stu-id="2d77c-123">Setup an Azure storage connection string</span></span>
<span data-ttu-id="2d77c-124">Egy Azure storage-ügyfél egy tárolási kapcsolati karakterlánc toostore végpontok használ, és adatok felügyeleti szolgáltatások eléréséhez szükséges hitelesítő adatokat.</span><span class="sxs-lookup"><span data-stu-id="2d77c-124">An Azure storage client uses a storage connection string toostore endpoints and credentials for accessing data management services.</span></span> <span data-ttu-id="2d77c-125">Ha egy ügyfél-alkalmazás fut, meg kell adnia hello tárolási kapcsolati karakterlánc formátuma a következő, a tárolási fiók és hello tárelérési kulcs hello nevének használatával hello felsorolt hello tárfiók hello [Azure Portal](https://portal.azure.com)a hello *AccountName* és *AccountKey* értékeket.</span><span class="sxs-lookup"><span data-stu-id="2d77c-125">When running in a client application, you must provide hello storage connection string in hello following format, using hello name of your storage account and hello storage access key for hello storage account listed in hello [Azure Portal](https://portal.azure.com) for hello *AccountName* and *AccountKey* values.</span></span> <span data-ttu-id="2d77c-126">A storage-fiókok és a hívóbetűk információkért lásd: [kapcsolatos Azure Storage-fiókok](../common/storage-create-storage-account.md?toc=%2fazure%2fstorage%2fblobs%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="2d77c-126">For information on storage accounts and access keys, see [About Azure Storage Accounts](../common/storage-create-storage-account.md?toc=%2fazure%2fstorage%2fblobs%2ftoc.json).</span></span> <span data-ttu-id="2d77c-127">Ez a példa bemutatja, hogyan deklarálhatnak statikus mező toohold hello kapcsolati karakterláncot:</span><span class="sxs-lookup"><span data-stu-id="2d77c-127">This example shows how you can declare a static field toohold hello connection string:</span></span>  

```cpp
// Define hello connection-string with your values.
const utility::string_t storage_connection_string(U("DefaultEndpointsProtocol=https;AccountName=your_storage_account;AccountKey=your_storage_account_key"));
```

<span data-ttu-id="2d77c-128">tootest az alkalmazás a helyi számítógép Windows hello Microsoft Azure használhat [storage emulator](../storage-use-emulator.md) hello a telepített [Azure SDK](https://azure.microsoft.com/downloads/).</span><span class="sxs-lookup"><span data-stu-id="2d77c-128">tootest your application in your local Windows computer, you can use hello Microsoft Azure [storage emulator](../storage-use-emulator.md) that is installed with hello [Azure SDK](https://azure.microsoft.com/downloads/).</span></span> <span data-ttu-id="2d77c-129">hello storage emulator egy segédprogram, amely a hello Blob, Queue és Table szolgáltatások az Azure-ban található a helyi fejlesztési számítógépén szimulálja.</span><span class="sxs-lookup"><span data-stu-id="2d77c-129">hello storage emulator is a utility that simulates hello Blob, Queue, and Table services available in Azure on your local development machine.</span></span> <span data-ttu-id="2d77c-130">hello következő példa bemutatja, hogyan deklarálhatnak egy statikus mező toohold hello kapcsolati karakterlánc tooyour helyi storage emulatort:</span><span class="sxs-lookup"><span data-stu-id="2d77c-130">hello following example shows how you can declare a static field toohold hello connection string tooyour local storage emulator:</span></span>

```cpp
// Define hello connection-string with Azure Storage Emulator.
const utility::string_t storage_connection_string(U("UseDevelopmentStorage=true;"));  
```

<span data-ttu-id="2d77c-131">toostart hello Azure storage emulatort, jelölje be hello **Start** gombra vagy nyomja le az hello **Windows** kulcs.</span><span class="sxs-lookup"><span data-stu-id="2d77c-131">toostart hello Azure storage emulator, Select hello **Start** button or press hello **Windows** key.</span></span> <span data-ttu-id="2d77c-132">Írja be a szöveget **Azure Storage Emulator**, és válassza ki **Microsoft Azure Storage Emulator** hello alkalmazások listája.</span><span class="sxs-lookup"><span data-stu-id="2d77c-132">Begin typing **Azure Storage Emulator**, and select **Microsoft Azure Storage Emulator** from hello list of applications.</span></span>  

<span data-ttu-id="2d77c-133">hello következő mintákat feltételezik használt egyik alábbi két módszer tooget hello tárolási kapcsolati karakterlánc.</span><span class="sxs-lookup"><span data-stu-id="2d77c-133">hello following samples assume that you have used one of these two methods tooget hello storage connection string.</span></span>  

## <a name="retrieve-your-connection-string"></a><span data-ttu-id="2d77c-134">A kapcsolat-karakterlánc beolvasása</span><span class="sxs-lookup"><span data-stu-id="2d77c-134">Retrieve your connection string</span></span>
<span data-ttu-id="2d77c-135">Használhatja a hello **cloud_storage_account** osztály toorepresent Tárfiók adatait.</span><span class="sxs-lookup"><span data-stu-id="2d77c-135">You can use hello **cloud_storage_account** class toorepresent your Storage Account information.</span></span> <span data-ttu-id="2d77c-136">tooretrieve a tárolási fiók hello tárolási kapcsolati karakterlánc adatait, használhatja a hello **elemezni** metódust.</span><span class="sxs-lookup"><span data-stu-id="2d77c-136">tooretrieve your storage account information from hello storage connection string, you can use hello **parse** method.</span></span>  

```cpp
// Retrieve storage account from connection string.
azure::storage::cloud_storage_account storage_account = azure::storage::cloud_storage_account::parse(storage_connection_string);
```

<span data-ttu-id="2d77c-137">A következő lekérni egy hivatkozás tooa **cloud_blob_client** teszi lehetővé, amelyek megfelelnek a tárolók és blobok hello Blob Storage szolgáltatásban tárolt objektumok tooretrieve osztályt.</span><span class="sxs-lookup"><span data-stu-id="2d77c-137">Next, get a reference tooa **cloud_blob_client** class as it allows you tooretrieve objects that represent containers and blobs stored within hello Blob Storage Service.</span></span> <span data-ttu-id="2d77c-138">hello alábbi kód létrehoz egy **cloud_blob_client** objektum hello tárolási fiók objektum azt lekérése fent alapján:</span><span class="sxs-lookup"><span data-stu-id="2d77c-138">hello following code creates a **cloud_blob_client** object using hello storage account object we retrieved above:</span></span>  

```cpp
// Create hello blob client.
azure::storage::cloud_blob_client blob_client = storage_account.create_cloud_blob_client();  
```

## <a name="how-to-create-a-container"></a><span data-ttu-id="2d77c-139">Útmutató: a tároló létrehozása</span><span class="sxs-lookup"><span data-stu-id="2d77c-139">How to: Create a container</span></span>
[!INCLUDE [storage-container-naming-rules-include](../../../includes/storage-container-naming-rules-include.md)]

<span data-ttu-id="2d77c-140">A példa bemutatja, hogyan toocreate egy tárolót, ha még nem létezik:</span><span class="sxs-lookup"><span data-stu-id="2d77c-140">This example shows how toocreate a container if it does not already exist:</span></span>  

```cpp
try
{
    // Retrieve storage account from connection string.
    azure::storage::cloud_storage_account storage_account = azure::storage::cloud_storage_account::parse(storage_connection_string);

    // Create hello blob client.
    azure::storage::cloud_blob_client blob_client = storage_account.create_cloud_blob_client();

    // Retrieve a reference tooa container.
    azure::storage::cloud_blob_container container = blob_client.get_container_reference(U("my-sample-container"));

    // Create hello container if it doesn't already exist.
    container.create_if_not_exists();
}
catch (const std::exception& e)
{
    std::wcout << U("Error: ") << e.what() << std::endl;
}  
```

<span data-ttu-id="2d77c-141">Alapértelmezés szerint hello új tároló privát, és meg kell adnia a tárolási kulcs toodownload blobok elérése az ebben a tárolóban.</span><span class="sxs-lookup"><span data-stu-id="2d77c-141">By default, hello new container is private and you must specify your storage access key toodownload blobs from this container.</span></span> <span data-ttu-id="2d77c-142">Ha belül hello tároló elérhető tooeveryone toomake hello fájlok (blobok), beállíthatja hello tároló toobe nyilvános hello kód a következő használatával:</span><span class="sxs-lookup"><span data-stu-id="2d77c-142">If you want toomake hello files (blobs) within hello container available tooeveryone, you can set hello container toobe public using hello following code:</span></span>  

```cpp
// Make hello blob container publicly accessible.
azure::storage::blob_container_permissions permissions;
permissions.set_public_access(azure::storage::blob_container_public_access_type::blob);
container.upload_permissions(permissions);  
```

<span data-ttu-id="2d77c-143">Hello Internet bárki láthatja a nyilvános tárolókban lévő blobokat, de módosítja, vagy törölje azokat, csak ha hello megfelelő hozzáférési kulcsot.</span><span class="sxs-lookup"><span data-stu-id="2d77c-143">Anyone on hello Internet can see blobs in a public container, but you can modify or delete them only if you have hello appropriate access key.</span></span>  

## <a name="how-to-upload-a-blob-into-a-container"></a><span data-ttu-id="2d77c-144">Hogyan: tölthetők fel blobok egy tárolóba</span><span class="sxs-lookup"><span data-stu-id="2d77c-144">How to: Upload a blob into a container</span></span>
<span data-ttu-id="2d77c-145">Az Azure Blob Storage támogatja a blokkblobokat és a lapblobokat.</span><span class="sxs-lookup"><span data-stu-id="2d77c-145">Azure Blob Storage supports block blobs and page blobs.</span></span> <span data-ttu-id="2d77c-146">Hello legtöbb esetben a blokkblob típusú toouse ajánlott hello.</span><span class="sxs-lookup"><span data-stu-id="2d77c-146">In hello majority of cases, block blob is hello recommended type toouse.</span></span>  

<span data-ttu-id="2d77c-147">a fájl tooa blokkblob tooupload beolvasni a tároló hivatkozását, és használja úgy tooget le a blokkblob hivatkozását.</span><span class="sxs-lookup"><span data-stu-id="2d77c-147">tooupload a file tooa block blob, get a container reference and use it tooget a block blob reference.</span></span> <span data-ttu-id="2d77c-148">Miután egy blobhivatkozást, az adatok tooit bármilyen streamet feltölthet által hívó hello **upload_from_stream** metódust.</span><span class="sxs-lookup"><span data-stu-id="2d77c-148">Once you have a blob reference, you can upload any stream of data tooit by calling hello **upload_from_stream** method.</span></span> <span data-ttu-id="2d77c-149">Ez a művelet létrehozza a hello blob, ha azt az előzőleg nem létezik, vagy felülírja, ha már létezik.</span><span class="sxs-lookup"><span data-stu-id="2d77c-149">This operation will create hello blob if it didn't previously exist, or overwrite it if it does exist.</span></span> <span data-ttu-id="2d77c-150">a következő példa azt mutatja meg hogyan hello tooupload blob egy tárolóba, és feltételezi, hogy hello tároló már létre lett hozva.</span><span class="sxs-lookup"><span data-stu-id="2d77c-150">hello following example shows how tooupload a blob into a container and assumes that hello container was already created.</span></span>  

```cpp
// Retrieve storage account from connection string.
azure::storage::cloud_storage_account storage_account = azure::storage::cloud_storage_account::parse(storage_connection_string);

// Create hello blob client.
azure::storage::cloud_blob_client blob_client = storage_account.create_cloud_blob_client();

// Retrieve a reference tooa previously created container.
azure::storage::cloud_blob_container container = blob_client.get_container_reference(U("my-sample-container"));

// Retrieve reference tooa blob named "my-blob-1".
azure::storage::cloud_block_blob blockBlob = container.get_block_blob_reference(U("my-blob-1"));

// Create or overwrite hello "my-blob-1" blob with contents from a local file.
concurrency::streams::istream input_stream = concurrency::streams::file_stream<uint8_t>::open_istream(U("DataFile.txt")).get();
blockBlob.upload_from_stream(input_stream);
input_stream.close().wait();

// Create or overwrite hello "my-blob-2" and "my-blob-3" blobs with contents from text.
// Retrieve a reference tooa blob named "my-blob-2".
azure::storage::cloud_block_blob blob2 = container.get_block_blob_reference(U("my-blob-2"));
blob2.upload_text(U("more text"));

// Retrieve a reference tooa blob named "my-blob-3".
azure::storage::cloud_block_blob blob3 = container.get_block_blob_reference(U("my-directory/my-sub-directory/my-blob-3"));
blob3.upload_text(U("other text"));  
```

<span data-ttu-id="2d77c-151">Másik lehetőségként használhatja a hello **upload_from_file** metódus tooupload egy fájl tooa blokkblob.</span><span class="sxs-lookup"><span data-stu-id="2d77c-151">Alternatively, you can use hello **upload_from_file** method tooupload a file tooa block blob.</span></span>

## <a name="how-to-list-hello-blobs-in-a-container"></a><span data-ttu-id="2d77c-152">Hogyan: hello a tárolóban lévő blobok listázása</span><span class="sxs-lookup"><span data-stu-id="2d77c-152">How to: List hello blobs in a container</span></span>
<span data-ttu-id="2d77c-153">toolist hello BLOB a tárolóban lévő először kapnak a tároló hivatkozását.</span><span class="sxs-lookup"><span data-stu-id="2d77c-153">toolist hello blobs in a container, first get a container reference.</span></span> <span data-ttu-id="2d77c-154">Hello tároló segítségével **list_blobs** metódus tooretrieve hello blobokat és/vagy könyvtárakat belül.</span><span class="sxs-lookup"><span data-stu-id="2d77c-154">You can then use hello container's **list_blobs** method tooretrieve hello blobs and/or directories within it.</span></span> <span data-ttu-id="2d77c-155">tooaccess hello tulajdonságai és metódusai visszaadásakor széles skáláját **list_blob_item**, meg kell hívnia hello **list_blob_item.as_blob** metódus tooget egy **cloud_blob** objektum vagy hello **list_blob.as_directory** metódus tooget egy cloud_blob_directory objektum.</span><span class="sxs-lookup"><span data-stu-id="2d77c-155">tooaccess hello rich set of properties and methods for a returned **list_blob_item**, you must call hello **list_blob_item.as_blob** method tooget a  **cloud_blob** object, or hello **list_blob.as_directory** method tooget a  cloud_blob_directory object.</span></span> <span data-ttu-id="2d77c-156">hello következő kód bemutatja, hogyan tooretrieve és a kimeneti hello hello az egyes elemek URI **a minta-tároló** tároló:</span><span class="sxs-lookup"><span data-stu-id="2d77c-156">hello following code demonstrates how tooretrieve and output hello URI of each item in hello **my-sample-container** container:</span></span>

```cpp
// Retrieve storage account from connection string.
azure::storage::cloud_storage_account storage_account = azure::storage::cloud_storage_account::parse(storage_connection_string);

// Create hello blob client.
azure::storage::cloud_blob_client blob_client = storage_account.create_cloud_blob_client();

// Retrieve a reference tooa previously created container.
azure::storage::cloud_blob_container container = blob_client.get_container_reference(U("my-sample-container"));

// Output URI of each item.
azure::storage::list_blob_item_iterator end_of_results;
for (auto it = container.list_blobs(); it != end_of_results; ++it)
{
    if (it->is_blob())
    {
        std::wcout << U("Blob: ") << it->as_blob().uri().primary_uri().to_string() << std::endl;
    }
    else
    {
        std::wcout << U("Directory: ") << it->as_directory().uri().primary_uri().to_string() << std::endl;
    }
}
```

<span data-ttu-id="2d77c-157">További műveletek listázásával további információkért lásd: [lista Azure Storage-erőforrások c++](../storage-c-plus-plus-enumeration.md).</span><span class="sxs-lookup"><span data-stu-id="2d77c-157">For more details on listing operations, see [List Azure Storage Resources in C++](../storage-c-plus-plus-enumeration.md).</span></span>

## <a name="how-to-download-blobs"></a><span data-ttu-id="2d77c-158">Hogyan: blobok letöltése</span><span class="sxs-lookup"><span data-stu-id="2d77c-158">How to: Download blobs</span></span>
<span data-ttu-id="2d77c-159">toodownload blobokat, először kérjen le egy blobhivatkozást, és majd meghívják a hello **download_to_stream** metódust.</span><span class="sxs-lookup"><span data-stu-id="2d77c-159">toodownload blobs, first retrieve a blob reference and then call hello **download_to_stream** method.</span></span> <span data-ttu-id="2d77c-160">hello alábbi példában hello **download_to_stream** metódus tootransfer hello blob tartalma tooa stream objektumot, hogy Ön majd megőrizni a tooa helyi fájlt.</span><span class="sxs-lookup"><span data-stu-id="2d77c-160">hello following example uses hello **download_to_stream** method tootransfer hello blob contents tooa stream object that you can then persist tooa local file.</span></span>  

```cpp
// Retrieve storage account from connection string.
azure::storage::cloud_storage_account storage_account = azure::storage::cloud_storage_account::parse(storage_connection_string);

// Create hello blob client.
azure::storage::cloud_blob_client blob_client = storage_account.create_cloud_blob_client();

// Retrieve a reference tooa previously created container.
azure::storage::cloud_blob_container container = blob_client.get_container_reference(U("my-sample-container"));

// Retrieve reference tooa blob named "my-blob-1".
azure::storage::cloud_block_blob blockBlob = container.get_block_blob_reference(U("my-blob-1"));

// Save blob contents tooa file.
concurrency::streams::container_buffer<std::vector<uint8_t>> buffer;
concurrency::streams::ostream output_stream(buffer);
blockBlob.download_to_stream(output_stream);

std::ofstream outfile("DownloadBlobFile.txt", std::ofstream::binary);
std::vector<unsigned char>& data = buffer.collection();

outfile.write((char *)&data[0], buffer.size());
outfile.close();  
```

<span data-ttu-id="2d77c-161">Másik lehetőségként használhatja a hello **download_to_file** metódus toodownload hello egy blob tooa fájl tartalmát.</span><span class="sxs-lookup"><span data-stu-id="2d77c-161">Alternatively, you can use hello **download_to_file** method toodownload hello contents of a blob tooa file.</span></span>
<span data-ttu-id="2d77c-162">Ezenkívül használhatja hello **download_text** metódus toodownload hello tartalmát szöveges karakterláncként blob.</span><span class="sxs-lookup"><span data-stu-id="2d77c-162">In addition, you can also use hello **download_text** method toodownload hello contents of a blob as a text string.</span></span>  

```cpp
// Retrieve storage account from connection string.
azure::storage::cloud_storage_account storage_account = azure::storage::cloud_storage_account::parse(storage_connection_string);

// Create hello blob client.
azure::storage::cloud_blob_client blob_client = storage_account.create_cloud_blob_client();

// Retrieve a reference tooa previously created container.
azure::storage::cloud_blob_container container = blob_client.get_container_reference(U("my-sample-container"));

// Retrieve reference tooa blob named "my-blob-2".
azure::storage::cloud_block_blob text_blob = container.get_block_blob_reference(U("my-blob-2"));

// Download hello contents of a blog as a text string.
utility::string_t text = text_blob.download_text();
```

## <a name="how-to-delete-blobs"></a><span data-ttu-id="2d77c-163">Hogyan: blobok törlése</span><span class="sxs-lookup"><span data-stu-id="2d77c-163">How to: Delete blobs</span></span>
<span data-ttu-id="2d77c-164">egy blob toodelete először kérjen le egy blobhivatkozást, és majd meghívják a hello **delete_blob** metódust.</span><span class="sxs-lookup"><span data-stu-id="2d77c-164">toodelete a blob, first get a blob reference and then call hello **delete_blob** method on it.</span></span>  

```cpp
// Retrieve storage account from connection string.
azure::storage::cloud_storage_account storage_account = azure::storage::cloud_storage_account::parse(storage_connection_string);

// Create hello blob client.
azure::storage::cloud_blob_client blob_client = storage_account.create_cloud_blob_client();

// Retrieve a reference tooa previously created container.
azure::storage::cloud_blob_container container = blob_client.get_container_reference(U("my-sample-container"));

// Retrieve reference tooa blob named "my-blob-1".
azure::storage::cloud_block_blob blockBlob = container.get_block_blob_reference(U("my-blob-1"));

// Delete hello blob.
blockBlob.delete_blob();
```

## <a name="next-steps"></a><span data-ttu-id="2d77c-165">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="2d77c-165">Next steps</span></span>
<span data-ttu-id="2d77c-166">Most, hogy megismerte a blob storage alapjait hello, kövesse az alábbi hivatkozások toolearn Azure Storage-ról további.</span><span class="sxs-lookup"><span data-stu-id="2d77c-166">Now that you've learned hello basics of blob storage, follow these links toolearn more about Azure Storage.</span></span>  

* [<span data-ttu-id="2d77c-167">Hogyan toouse C++ a Queue Storage</span><span class="sxs-lookup"><span data-stu-id="2d77c-167">How toouse Queue Storage from C++</span></span>](../storage-c-plus-plus-how-to-use-queues.md)
* [<span data-ttu-id="2d77c-168">Hogyan toouse Table Storage-ának C++</span><span class="sxs-lookup"><span data-stu-id="2d77c-168">How toouse Table Storage from C++</span></span>](../../cosmos-db/table-storage-how-to-use-c-plus.md)
* [<span data-ttu-id="2d77c-169">A c++ Azure Storage-erőforrások listája</span><span class="sxs-lookup"><span data-stu-id="2d77c-169">List Azure Storage Resources in C++</span></span>](../storage-c-plus-plus-enumeration.md)
* [<span data-ttu-id="2d77c-170">A Storage ügyféloldali kódtára a c++ nyelvhez – dokumentáció</span><span class="sxs-lookup"><span data-stu-id="2d77c-170">Storage Client Library for C++ Reference</span></span>](http://azure.github.io/azure-storage-cpp)
* [<span data-ttu-id="2d77c-171">Az Azure Storage dokumentációja</span><span class="sxs-lookup"><span data-stu-id="2d77c-171">Azure Storage Documentation</span></span>](https://azure.microsoft.com/documentation/services/storage/)
* [<span data-ttu-id="2d77c-172">Adatátvitel az AzCopy parancssori segédprogram hello</span><span class="sxs-lookup"><span data-stu-id="2d77c-172">Transfer data with hello AzCopy command-line utility</span></span>](../storage-use-azcopy.md)

