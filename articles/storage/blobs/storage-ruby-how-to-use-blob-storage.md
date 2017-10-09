---
title: Blob storage (object storage) Ruby a aaaHow toouse |} Microsoft Docs
description: "Strukturálatlan adatok tárolása az Azure Blob storage (object storage) hello felhő."
services: storage
documentationcenter: ruby
author: mmacy
manager: timlt
editor: tysonn
ms.assetid: e2fe4c45-27b0-4d15-b3fb-e7eb574db717
ms.service: storage
ms.workload: storage
ms.tgt_pltfrm: na
ms.devlang: ruby
ms.topic: article
ms.date: 12/08/2016
ms.author: marsma
ms.openlocfilehash: 776e7d788e69d4960f8dde0b783513f6b39b7a47
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="how-toouse-blob-storage-from-ruby"></a>Hogyan toouse Blob storage-ának Ruby
[!INCLUDE [storage-selector-blob-include](../../../includes/storage-selector-blob-include.md)]

[!INCLUDE [storage-try-azure-tools-queues](../../../includes/storage-try-azure-tools-blobs.md)]

## <a name="overview"></a>Áttekintés
Az Azure Blob storage egy olyan szolgáltatás, amely hello felhő strukturálatlan adatokat objektumként/blobként tárolja. A Blob Storage képes tárolni bármilyen szöveget vagy bináris adatot, például dokumentumot, médiafájlt vagy egy alkalmazástelepítőt. A BLOB storage is az említett tooas objektum tároló.

Ez az útmutató bemutatja, hogyan tooperform szolgáltatást használó általános forgatókönyvhöz Blob Storage tárolóban. hello minták hello Ruby API segítségével készül. hello tárgyalt forgatókönyvekben szerepel a **feltöltése, listázása, letöltése,** és **törlése** blobokat.

[!INCLUDE [storage-blob-concepts-include](../../../includes/storage-blob-concepts-include.md)]

[!INCLUDE [storage-create-account-include](../../../includes/storage-create-account-include.md)]

## <a name="create-a-ruby-application"></a>Ruby-alkalmazás létrehozása
Ruby-alkalmazás létrehozása. Útmutatásért lásd: [Ruby sínek webalkalmazás egy Azure virtuális gépen](../../virtual-machines/linux/classic/virtual-machines-linux-classic-ruby-rails-web-app.md)

## <a name="configure-your-application-tooaccess-storage"></a>Az alkalmazás tooaccess tároló konfigurálása
Azure Storage toouse, és van szükség toodownload használata hello Ruby azure csomagot, amely kényelmi szalagtárak hello tárolószolgáltatások REST kommunikációt készletét tartalmazza.

### <a name="use-rubygems-tooobtain-hello-package"></a>RubyGems tooobtain hello csomag használata
1. Használjon például egy parancssori felületet **PowerShell** (Windows), **Terminálszolgáltatások** (Mac), vagy **Bash** (Unix).
2. Írja be a "gem telepítése azure" hello parancs ablak tooinstall hello gem és függőségeit.

### <a name="import-hello-package"></a>Hello csomag importálása
Kedvenc szövegszerkesztőjével használ, adja hozzá a következő hello toouse tárolási célhelyeként Ruby fájl elejéhez toohello hello:

```ruby
require "azure"
```

## <a name="set-up-an-azure-storage-connection"></a>Egy Azure Storage-kapcsolat beállítása
hello azure modul hello környezeti változókat olvassák **AZURE\_tárolási\_fiók** és **AZURE\_tárolási\_ACCESS_KEY** a információ tooconnect tooyour Azure storage-fiók szükséges. Ha ezek a környezeti változók nem, meg kell adnia hello fiók használata előtt **Azure::Blob::BlobService** a hello a következő kódot:

```ruby
Azure.config.storage_account_name = "<your azure storage account>"
Azure.config.storage_access_key = "<your azure storage access key>"
```

tooobtain ezeket az értékeket a klasszikus vagy erőforrás-kezelő tárolási fiók hello Azure-portálon:

1. Jelentkezzen be toohello [Azure-portálon](https://portal.azure.com).
2. Keresse meg a kívánt toouse toohello tárfiók.
3. Kattintson a jobb oldali hello hello beállítási paneljén **hívóbetűk**.
4. Hello elérési kulcsok paneljén megjelenő láthatja, hello hozzáférési kulcs 1 és 2 elérési kulcsot. Ezek bármelyikét használhatja.
5. Kattintson a hello másolás ikon toocopy hello kulcs toohello vágólapra.

## <a name="create-a-container"></a>Tároló létrehozása
[!INCLUDE [storage-container-naming-rules-include](../../../includes/storage-container-naming-rules-include.md)]

Hello **Azure::Blob::BlobService** objektum lehetővé teszi, hogy a tárolók és blobok. egy tároló toocreate hello használata **létrehozása\_container()** metódust.

hello alábbi példakód létrehoz egy tárolót vagy nyomtatása hello hiba, ha van ilyen.

```ruby
azure_blob_service = Azure::Blob::BlobService.new
begin
    container = azure_blob_service.create_container("test-container")
rescue
    puts $!
end
```

Ha azt szeretné toomake hello fájlok hello tárolóban nyilvános, beállíthatja a hello tároló engedélyeit.

Csak módosíthatja hello <strong>létrehozása\_container()</strong> hívás toopass hello **: nyilvános\_hozzáférés\_szint** lehetőséget:

```ruby
container = azure_blob_service.create_container("test-container",
    :public_access_level => "<public access level>")
```

Hello értékek **: nyilvános\_hozzáférés\_szint** lehetőség van:

* **BLOB:** nyilvános olvasási hozzáférés a blobok határozza meg. Blobadatok található olvasható névtelen kérelem keresztül, de adatai nem érhető el. Ügyfelek névtelen kérelem keresztül hello tárolóban található blobok nem tudja felsorolni.
* **tároló:** határozza meg a tároló és a blob adatait a teljes nyilvános olvasási hozzáférés. Ügyfelek névtelen kérelem keresztül hello tárolóban található blobok enumerálása, de nem tudja felsorolni a tárolók hello tárfiókon belül.

Azt is megteheti, módosíthatja az hello nyilvános hozzáférési szint a tároló használatával **beállítása\_tároló\_acl()** toospecify hello nyilvános hozzáférés szintet.

hello az alábbi kódpéldát módosítások hello nyilvános hozzáférési szint túl**tároló**:

```ruby
azure_blob_service.set_container_acl('test-container', "container")
```

## <a name="upload-a-blob-into-a-container"></a>Blobok feltöltése a tárolóba
tooupload tartalom tooa blob, használjon hello **létrehozása\_blokk\_blob()** metódus toocreate hello blob, egy fájl vagy karakterlánc-hello BLOB hello tartalmat.

hello alábbira feltölt hello fájlt **test.png** "lemezkép-blob" hello tárolóban nevű új blob-ként.

```ruby
content = File.open("test.png", "rb") { |file| file.read }
blob = azure_blob_service.create_block_blob(container.name,
    "image-blob", content)
puts blob.name
```

## <a name="list-hello-blobs-in-a-container"></a>Lista hello a tárolóban lévő blobok
toolist hello tárolók, használjon **list_containers()** metódust.
toolist hello BLOB a tárolóban lévő használja **lista\_blobs()** metódust.

Ennek kimenete hello hello fiók összes hello tárolókban lévő összes hello BLOB URL-címei.

```ruby
containers = azure_blob_service.list_containers()
containers.each do |container|
    blobs = azure_blob_service.list_blobs(container.name)
    blobs.each do |blob|
    puts blob.name
    end
end
```

## <a name="download-blobs"></a>Blobok letöltése
toodownload blobs használata hello **beolvasása\_blob()** metódus tooretrieve hello tartalmát.

hello alábbi példakód bemutatja használatával **beolvasása\_blob()** toodownload hello "lemezkép-blob" tartalmát és azokra írják tooa helyi fájlt.

```ruby
blob, content = azure_blob_service.get_blob(container.name,"image-blob")
File.open("download.png","wb") {|f| f.write(content)}
```

## <a name="delete-a-blob"></a>Blobok törléséhez
Végezetül toodelete blob, használja a hello **törlése\_blob()** metódust. hello következő kódrészlet példa bemutatja, hogyan toodelete blob.

```ruby
azure_blob_service.delete_blob(container.name, "image-blob")
```

## <a name="next-steps"></a>Következő lépések
toolearn kapcsolatos összetettebb tárolási feladatok elvégzéséről az alábbi hivatkozásokat követve:

* [Az Azure Storage csapat blogja](http://blogs.msdn.com/b/windowsazurestorage/)
* [Rubyhoz készült Azure SDK](https://github.com/WindowsAzure/azure-sdk-for-ruby) GitHub tárházából
* [Adatátvitel az AzCopy parancssori segédprogram hello](../common/storage-use-azcopy.md?toc=%2fazure%2fstorage%2fblobs%2ftoc.json)

