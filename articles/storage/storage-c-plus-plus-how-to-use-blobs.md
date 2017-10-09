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
ms.openlocfilehash: 0d7e7436a109ef54fc07cc238c03cfc7cf2caac0
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="how-toouse-blob-storage-from-c"></a>Hogyan toouse Blob Storage-ának C++
[!INCLUDE [storage-selector-blob-include](../../includes/storage-selector-blob-include.md)]

[!INCLUDE [storage-try-azure-tools-blobs](../../includes/storage-try-azure-tools-blobs.md)]

## <a name="overview"></a>Áttekintés
Az Azure Blob storage egy olyan szolgáltatás, amely hello felhő strukturálatlan adatokat objektumként/blobként tárolja. A Blob Storage képes tárolni bármilyen szöveget vagy bináris adatot, például dokumentumot, médiafájlt vagy egy alkalmazástelepítőt. A BLOB storage is az említett tooas objektum tároló.

Ez az útmutató mutatni, hogyan tooperform szolgáltatást használó általános forgatókönyvhöz hello Azure Blob storage szolgáltatásban. hello minták C++ nyelven íródtak, és használja a hello [Azure Storage ügyféloldali kódtára a C++](http://github.com/Azure/azure-storage-cpp/blob/master/README.md). hello tárgyalt forgatókönyvekben szerepel a **feltöltése**, **felsoroló**, **letöltése**, és **törlése** blobokat.  

> [!NOTE]
> Ez az útmutató célok hello a Azure Storage ügyféloldali kódtára a C++ 1.0.0 verzió vagy újabb. hello ajánlott verzió a Storage ügyféloldali kódtára 2.2.0, amelyik keresztül elérhető [NuGet](http://www.nuget.org/packages/wastorage) vagy [GitHub](https://github.com/Azure/azure-storage-cpp).
> 
> 

[!INCLUDE [storage-blob-concepts-include](../../includes/storage-blob-concepts-include.md)]

[!INCLUDE [storage-create-account-include](../../includes/storage-create-account-include.md)]

## <a name="create-a-c-application"></a>A C++-alkalmazás létrehozása
Ez az útmutató egy C++ alkalmazáson belül futtatható tárolási szolgáltatásokkal fog használni.  

toodo tooinstall kell tehát hello Azure Storage ügyféloldali kódtára a C++ és az Azure storage-fiók létrehozása az Azure-előfizetése.   

tooinstall hello Azure Storage ügyféloldali kódtára a C++, a következő módszerek hello használhatják:

* **Linux:** hello megadott hello utasítások [Azure Storage ügyféloldali kódtára a C++ információs](https://github.com/Azure/azure-storage-cpp/blob/master/README.md) lap.  
* **Windows:** a Visual Studióban kattintson **eszközök > NuGet-Csomagkezelő > Csomagkezelő konzol**. Típus hello következő parancsot a hello történő [NuGet Package Manager console](http://docs.nuget.org/docs/start-here/using-the-package-manager-console) nyomja le az ENTER **ENTER**.  
  
     Install-Package wastorage

## <a name="configure-your-application-tooaccess-blob-storage"></a>Az alkalmazás tooaccess Blob-tároló konfigurálása
Adja hozzá, hello következő tartalmazó utasítások toohello felső részén hello C++ fájl toouse hello az Azure storage API-k tooaccess blobok:  

```cpp
#include <was/storage_account.h>
#include <was/blob.h>
```

## <a name="setup-an-azure-storage-connection-string"></a>Egy Azure storage kapcsolati karakterlánc beállítása
Egy Azure storage-ügyfél egy tárolási kapcsolati karakterlánc toostore végpontok használ, és adatok felügyeleti szolgáltatások eléréséhez szükséges hitelesítő adatokat. Ha egy ügyfél-alkalmazás fut, meg kell adnia hello tárolási kapcsolati karakterlánc formátuma a következő, a tárolási fiók és hello tárelérési kulcs hello nevének használatával hello felsorolt hello tárfiók hello [Azure Portal](https://portal.azure.com)a hello *AccountName* és *AccountKey* értékeket. A storage-fiókok és a hívóbetűk információkért lásd: [kapcsolatos Azure Storage-fiókok](storage-create-storage-account.md). Ez a példa bemutatja, hogyan deklarálhatnak statikus mező toohold hello kapcsolati karakterláncot:  

```cpp
// Define hello connection-string with your values.
const utility::string_t storage_connection_string(U("DefaultEndpointsProtocol=https;AccountName=your_storage_account;AccountKey=your_storage_account_key"));
```

tootest az alkalmazás a helyi számítógép Windows hello Microsoft Azure használhat [storage emulator](storage-use-emulator.md) hello a telepített [Azure SDK](https://azure.microsoft.com/downloads/). hello storage emulator egy segédprogram, amely a hello Blob, Queue és Table szolgáltatások az Azure-ban található a helyi fejlesztési számítógépén szimulálja. hello következő példa bemutatja, hogyan deklarálhatnak egy statikus mező toohold hello kapcsolati karakterlánc tooyour helyi storage emulatort:

```cpp
// Define hello connection-string with Azure Storage Emulator.
const utility::string_t storage_connection_string(U("UseDevelopmentStorage=true;"));  
```

toostart hello Azure storage emulatort, jelölje be hello **Start** gombra vagy nyomja le az hello **Windows** kulcs. Írja be a szöveget **Azure Storage Emulator**, és válassza ki **Microsoft Azure Storage Emulator** hello alkalmazások listája.  

hello következő mintákat feltételezik használt egyik alábbi két módszer tooget hello tárolási kapcsolati karakterlánc.  

## <a name="retrieve-your-connection-string"></a>A kapcsolat-karakterlánc beolvasása
Használhatja a hello **cloud_storage_account** osztály toorepresent Tárfiók adatait. tooretrieve a tárolási fiók hello tárolási kapcsolati karakterlánc adatait, használhatja a hello **elemezni** metódust.  

```cpp
// Retrieve storage account from connection string.
azure::storage::cloud_storage_account storage_account = azure::storage::cloud_storage_account::parse(storage_connection_string);
```

A következő lekérni egy hivatkozás tooa **cloud_blob_client** teszi lehetővé, amelyek megfelelnek a tárolók és blobok hello Blob Storage szolgáltatásban tárolt objektumok tooretrieve osztályt. hello alábbi kód létrehoz egy **cloud_blob_client** objektum hello tárolási fiók objektum azt lekérése fent alapján:  

```cpp
// Create hello blob client.
azure::storage::cloud_blob_client blob_client = storage_account.create_cloud_blob_client();  
```

## <a name="how-to-create-a-container"></a>Útmutató: a tároló létrehozása
[!INCLUDE [storage-container-naming-rules-include](../../includes/storage-container-naming-rules-include.md)]

A példa bemutatja, hogyan toocreate egy tárolót, ha még nem létezik:  

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

Alapértelmezés szerint hello új tároló privát, és meg kell adnia a tárolási kulcs toodownload blobok elérése az ebben a tárolóban. Ha belül hello tároló elérhető tooeveryone toomake hello fájlok (blobok), beállíthatja hello tároló toobe nyilvános hello kód a következő használatával:  

```cpp
// Make hello blob container publicly accessible.
azure::storage::blob_container_permissions permissions;
permissions.set_public_access(azure::storage::blob_container_public_access_type::blob);
container.upload_permissions(permissions);  
```

Hello Internet bárki láthatja a nyilvános tárolókban lévő blobokat, de módosítja, vagy törölje azokat, csak ha hello megfelelő hozzáférési kulcsot.  

## <a name="how-to-upload-a-blob-into-a-container"></a>Hogyan: tölthetők fel blobok egy tárolóba
Az Azure Blob Storage támogatja a blokkblobokat és a lapblobokat. Hello legtöbb esetben a blokkblob típusú toouse ajánlott hello.  

a fájl tooa blokkblob tooupload beolvasni a tároló hivatkozását, és használja úgy tooget le a blokkblob hivatkozását. Miután egy blobhivatkozást, az adatok tooit bármilyen streamet feltölthet által hívó hello **upload_from_stream** metódust. Ez a művelet létrehozza a hello blob, ha azt az előzőleg nem létezik, vagy felülírja, ha már létezik. a következő példa azt mutatja meg hogyan hello tooupload blob egy tárolóba, és feltételezi, hogy hello tároló már létre lett hozva.  

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

Másik lehetőségként használhatja a hello **upload_from_file** metódus tooupload egy fájl tooa blokkblob.

## <a name="how-to-list-hello-blobs-in-a-container"></a>Hogyan: hello a tárolóban lévő blobok listázása
toolist hello BLOB a tárolóban lévő először kapnak a tároló hivatkozását. Hello tároló segítségével **list_blobs** metódus tooretrieve hello blobokat és/vagy könyvtárakat belül. tooaccess hello tulajdonságai és metódusai visszaadásakor széles skáláját **list_blob_item**, meg kell hívnia hello **list_blob_item.as_blob** metódus tooget egy **cloud_blob** objektum vagy hello **list_blob.as_directory** metódus tooget egy cloud_blob_directory objektum. hello következő kód bemutatja, hogyan tooretrieve és a kimeneti hello hello az egyes elemek URI **a minta-tároló** tároló:

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

További műveletek listázásával további információkért lásd: [lista Azure Storage-erőforrások c++](storage-c-plus-plus-enumeration.md).

## <a name="how-to-download-blobs"></a>Hogyan: blobok letöltése
toodownload blobokat, először kérjen le egy blobhivatkozást, és majd meghívják a hello **download_to_stream** metódust. hello alábbi példában hello **download_to_stream** metódus tootransfer hello blob tartalma tooa stream objektumot, hogy Ön majd megőrizni a tooa helyi fájlt.  

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

Másik lehetőségként használhatja a hello **download_to_file** metódus toodownload hello egy blob tooa fájl tartalmát.
Ezenkívül használhatja hello **download_text** metódus toodownload hello tartalmát szöveges karakterláncként blob.  

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

## <a name="how-to-delete-blobs"></a>Hogyan: blobok törlése
egy blob toodelete először kérjen le egy blobhivatkozást, és majd meghívják a hello **delete_blob** metódust.  

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

## <a name="next-steps"></a>Következő lépések
Most, hogy megismerte a blob storage alapjait hello, kövesse az alábbi hivatkozások toolearn Azure Storage-ról további.  

* [Hogyan toouse C++ a Queue Storage](storage-c-plus-plus-how-to-use-queues.md)
* [Hogyan toouse Table Storage-ának C++](storage-c-plus-plus-how-to-use-tables.md)
* [A c++ Azure Storage-erőforrások listája](storage-c-plus-plus-enumeration.md)
* [A Storage ügyféloldali kódtára a c++ nyelvhez – dokumentáció](http://azure.github.io/azure-storage-cpp)
* [Az Azure Storage dokumentációja](https://azure.microsoft.com/documentation/services/storage/)
* [Adatátvitel az AzCopy parancssori segédprogram hello](storage-use-azcopy.md)

