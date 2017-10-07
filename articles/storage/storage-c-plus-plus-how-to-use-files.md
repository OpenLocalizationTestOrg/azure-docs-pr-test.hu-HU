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
ms.openlocfilehash: 40c3aac94214a91121913e2ded315031aeed1c30
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="develop-for-azure-file-storage-with-c"></a>Az Azure File storage a C++ fejlesztése
[!INCLUDE [storage-selector-file-include](../../includes/storage-selector-file-include.md)]

[!INCLUDE [storage-try-azure-tools-files](../../includes/storage-try-azure-tools-files.md)]

## <a name="about-this-tutorial"></a>Az oktatóanyag ismertetése

Ebben az oktatóanyagban megtudhatja, hogyan tooperform Azure File storage alapvető műveleteket. Keresztül írt C++ minták, megtudhatja, hogyan osztja meg toocreate és könyvtárak, feltöltése, listázása és fájlok törlése. Ha új tooAzure fájltároló, hasznos információkat hello minták hello fogalmak hello szakaszokban szereplő áthaladás lesz.


* Hozzon létre vagy töröljön az Azure fájlmegosztások
* Hozzon létre vagy töröljön a könyvtárak
* Fájlok és könyvtárak egy Azure fájlmegosztás számbavétele
* Töltse fel, töltse le és törölje a fájlt
* Egy Azure fájlmegosztás hello kvótájának (maximális méretének) beállítása
* Hozzon létre egy megosztott elérési házirendet hello megosztáson definiált használó fájl a közös hozzáférésű jogosultságkód (SAS-kulcsot).

> [!Note]  
> Előfordulhat, hogy Azure fájltároló SMB-n keresztül érhető el, mert lehetséges toowrite elérhető hello Azure fájlmegosztás hello szabványos C++ i/o-osztály és függvények használata egyszerű alkalmazásokat is. Ez a cikk ismerteti, hogyan toowrite használó alkalmazások hello használ hello Azure Storage C++ SDK [Azure File storage REST API](https://docs.microsoft.com/rest/api/storageservices/fileservices/file-service-rest-api) tootalk tooAzure fájlok tárolására.

## <a name="create-a-c-application"></a>A C++-alkalmazás létrehozása
toobuild hello minták, szüksége lesz tooinstall hello Azure Storage ügyféloldali kódtár 2.4.0 C++. Meg is létrehozott egy Azure storage-fiókot.

tooinstall hello Azure Storage ügyfél 2.4.0 C++, használhatja hello a következő módszerek egyikét:

* **Linux:** hello megadott hello utasítások [Azure Storage ügyféloldali kódtára a C++ információs](https://github.com/Azure/azure-storage-cpp/blob/master/README.md) lap.
* **Windows:** a Visual Studióban kattintson **eszközök &gt; NuGet-Csomagkezelő &gt; Csomagkezelő konzol**. Típus hello következő parancsot a hello történő [NuGet Package Manager console](http://docs.nuget.org/docs/start-here/using-the-package-manager-console) nyomja le az ENTER **ENTER**.
  
```
Install-Package wastorage
```

## <a name="set-up-your-application-toouse-azure-file-storage"></a>Az alkalmazás toouse Azure File storage beállítása
Adja hozzá, hello következő tartalmazó utasítások toohello felső hello C++ toomanipulate Azure File storage kívánt forrásfájl:

```cpp
#include <was/storage_account.h>
#include <was/file.h>
```

## <a name="set-up-an-azure-storage-connection-string"></a>Egy Azure storage kapcsolati karakterlánc beállítása
a File storage toouse, tooconnect tooyour az Azure storage-fiók szükséges. hello első lépéseként lenne egy kapcsolati karakterláncot, amely fogjuk használni tooconfigure tooconnect tooyour tárfiók. Is határozza meg egy statikus változó toodo, amely.

```cpp
// Define hello connection-string with your values.
const utility::string_t 
storage_connection_string(U("DefaultEndpointsProtocol=https;AccountName=your_storage_account;AccountKey=your_storage_account_key"));
```

## <a name="connecting-tooan-azure-storage-account"></a>Csatlakozás tooan Azure storage-fiók
Használhatja a hello **cloud_storage_account** osztály toorepresent Tárfiók adatait. tooretrieve a tárolási fiók hello tárolási kapcsolati karakterlánc adatait, használhatja a hello **elemezni** metódust.

```cpp
// Retrieve storage account from connection string.    
azure::storage::cloud_storage_account storage_account = 
  azure::storage::cloud_storage_account::parse(storage_connection_string);
```

## <a name="create-an-azure-file-share"></a>Azure-fájlmegosztás létrehozása
A fájlok és könyvtárak az Azure File storage találhatók nevű tároló egy **megosztás**. A tárfiók lehet a kapacitásával lehetővé teszi tetszőleges számú megosztások. tooobtain hozzáférés tooa megosztást és tartalmát, egy Azure File storage ügyfél toouse kell.

```cpp
// Create hello Azure File storage client.
azure::storage::cloud_file_client file_client = 
  storage_account.create_cloud_file_client();
```

Hello Azure File storage-ügyfelet használ, majd szerezhet be egy hivatkozást tooa megosztást.

```cpp
// Get a reference toohello file share
azure::storage::cloud_file_share share = 
  file_client.get_share_reference(_XPLATSTR("my-sample-share"));
```

toocreate hello megosztás használata hello **create_if_not_exists** hello metódusában **cloud_file_share** objektum.

```cpp
if (share.create_if_not_exists()) {    
    std::wcout << U("New share created") << std::endl;    
}
```

Ezen a ponton **megosztása** tartalmazza a referencia-tooa nevű megosztás **a minta-megosztás**.

## <a name="delete-an-azure-file-share"></a>Egy Azure fájlmegosztás törlése
A megosztás törlése hívó hello végezhető el **delete_if_exists** cloud_file_share objektum metódust. Itt látható mintakód, amelyet, amely.

```cpp
// Get a reference toohello share.
azure::storage::cloud_file_share share = 
  file_client.get_share_reference(_XPLATSTR("my-sample-share"));

// delete hello share if exists
share.delete_share_if_exists();
```

## <a name="create-a-directory"></a>Könyvtár létrehozása
Tároló üzembe alkönyvtárak ahelyett, az összes hello gyökérkönyvtárában lévő fájlok rendezhetők. Az Azure File storage lehetővé teszi toocreate számos könyvtárat, a fiók fog lehetővé. hello az alábbi kód létrehoz egy nevű könyvtár **saját minta directory** alatt hello gyökérkönyvtár, valamint a nevű **saját minta alkönyvtár**.

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

## <a name="delete-a-directory"></a>Könyvtár törlése
A könyvtár törlése mely egyszerű feladat, érdemes megjegyezni, hogy nem törölhető a fájlok továbbra is tartalmazó könyvtár vagy más címtárakban.

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

## <a name="enumerate-files-and-directories-in-an-azure-file-share"></a>Fájlok és könyvtárak egy Azure fájlmegosztás számbavétele
Fájlok és könyvtárak egy megosztáson belüli listájának beszerzésével könnyen történik meghívásával **list_files_and_directories** a egy **cloud_file_directory** hivatkozás. tooaccess hello tulajdonságai és metódusai visszaadásakor széles skáláját **list_file_and_directory_item**, meg kell hívnia hello **list_file_and_directory_item.as_file** metódus tooget egy **cloud_ fájl** objektumot, vagy hello **list_file_and_directory_item.as_directory** metódus tooget egy **cloud_file_directory** objektum.

hello a következő kód bemutatja, hogyan tooretrieve és a kimeneti hello hello megosztás hello gyökérkönyvtárában egyes elemek URI Azonosítóját.

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

## <a name="upload-a-file"></a>Fájl feltöltése
Nagyon hello: legalább egy Azure fájlmegosztás tartalmazza a gyökérkönyvtár fájlokat tároló is. Ebben a szakaszban megtudhatja, hogyan tooupload egy fájlt a helyi tároló alakzatot hello gyökérkönyvtár megosztás.

hello első lépése a fájl feltöltése tooobtain egy hivatkozás toohello könyvtárat, ahol kell tárolni. Ehhez hívó hello **get_root_directory_reference** hello megosztás metódusa.

```cpp
//Get a reference toohello root directory for hello share.
azure::storage::cloud_file_directory root_dir = share.get_root_directory_reference();
```

Most, hogy hello megosztás hivatkozás toohello gyökérkönyvtár, feltöltheti azt a fájlt. Ebben a példában feltölt egy fájlt, szöveg és a folyam.

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

## <a name="download-a-file"></a>Fájl letöltése
toodownload fájlok, először kérjen le egy hivatkozást, és majd meghívják a hello **download_to_stream** metódus tootransfer hello fájl tartalma tooa adatfolyam-objektum, amely meg tudja majd megőrizni a tooa helyi fájlt. Másik lehetőségként használhatja a hello **download_to_file** metódus toodownload hello fájl tooa helyi fájl tartalmát. Használhatja a hello **download_text** metódus toodownload hello tartalmát szöveges karakterláncként fájl.

hello alábbi példában hello **download_to_stream** és **download_text** módszerek toodemonstrate hello fájlokat, amelyek a korábbi szakaszokban létrehozott letöltése.

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

## <a name="delete-a-file"></a>Fájl törlése
Közös Azure File storage egy másik művelet a fájl törlése. hello alábbira töröl egy fájlt saját-minta-fájl – 3 nevű hello gyökérkönyvtárban tárolja.

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

## <a name="set-hello-quota-maximum-size-for-an-azure-file-share"></a>Egy Azure fájlmegosztás hello kvótájának (maximális méretének) beállítása
Beállíthat hello kvótát (vagy maximális méretet) egy fájlmegosztáshoz GB-ban. Mennyi adatot hello megosztáson tárolt toosee is ellenőrizheti.

Által beállítás hello kvótát egy megosztáshoz korlátozhatja a hello hello tárolják hello fájlok összesített mérete. Ha hello hello megosztáson található fájlok összesített mérete meghaladja hello kvóta hello megosztáson, akkor az ügyfelek meglévő fájlok mérete nem lehet tooincrease hello vagy fogja új fájlok létrehozása, kivéve azokat, amelyek üresek.

hello az alábbi példa bemutatja, hogyan toocheck hello egy megosztás aktuális kihasználását, és hogyan tooset hello hello fájlmegosztás kvótája.

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

## <a name="generate-a-shared-access-signature-for-a-file-or-file-share"></a>Közös hozzáférésű jogosultságkód létrehozása egy fájlhoz vagy fájlmegosztáshoz
Létrehozhat egy közös hozzáférésű jogosultságkód (SAS) egy fájlmegosztáshoz vagy fájlhoz. Is létrehozhat egy megosztott elérési házirendet egy fájlmegosztási toomanage megosztott hozzáférést aláírások. Megosztott hozzáférési házirend létrehozása az ajánlott, mivel hello SAS visszavonása, ha kell sérülhet módszert biztosít.

a következő példa hello hoz létre egy megosztott elérési házirendet egy megosztáson, és akkor használja, hogy használ-e tooprovide hello Házirendmegkötések tartozó SAS korlátozására található hello fájlhoz.

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
## <a name="next-steps"></a>Következő lépések
További információ az Azure Storage toolearn ismerheti meg ezeket az erőforrásokat:

* [A Storage ügyféloldali kódtára a C++ programnyelvhez](https://github.com/Azure/azure-storage-cpp)
* [Az azure Storage szolgáltatás fájlminták c++] (https://github.com/Azure-Samples/storage-file-cpp-getting-started)
* [Azure Storage Explorer](http://go.microsoft.com/fwlink/?LinkID=822673&clcid=0x409)
* [Az Azure Storage dokumentációja](https://azure.microsoft.com/documentation/services/storage/)