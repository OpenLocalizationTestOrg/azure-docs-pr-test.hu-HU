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
# <a name="develop-for-azure-file-storage-with-python"></a>Az Azure File storage Python fejlesztése
[!INCLUDE [storage-selector-file-include](../../includes/storage-selector-file-include.md)]

[!INCLUDE [storage-try-azure-tools-files](../../includes/storage-try-azure-tools-files.md)]

## <a name="about-this-tutorial"></a>Az oktatóanyag ismertetése
Ez az oktatóanyag mutatni, Python toodevelop alkalmazásokhoz vagy szolgáltatásokhoz, használja az Azure File storage toostore fájladatok használatával hello alapjait. Ebben az oktatóanyagban a rendszer létrehoz egy egyszerű konzolalkalmazásként és megjelenítése hogyan tooperform alapszintű műveleteket az Python és az Azure File storage szolgáltatással:

* Az Azure fájlmegosztások létrehozása
* Könyvtárak létrehozása
* Fájlok és könyvtárak egy Azure fájlmegosztás számbavétele
* Töltse fel, töltse le és törölje a fájlt

> [!Note]  
> Előfordulhat, hogy Azure fájltároló SMB-n keresztül érhető el, mert lehetséges toowrite elérhető hello Azure fájlmegosztás hello szabványos Python i/o-osztály és függvények használata egyszerű alkalmazásokat is. Ez a cikk ismerteti, hogyan toowrite használó alkalmazások hello használ hello Azure Storage Python SDK [Azure File storage REST API](https://docs.microsoft.com/en-us/rest/api/storageservices/fileservices/file-service-rest-api) tootalk tooAzure fájlok tárolására.

### <a name="set-up-your-application-toouse-azure-file-storage"></a>Az alkalmazás toouse Azure File storage beállítása
Adja hozzá a hello következő bármely Python forrásfájlt, ahol tooprogrammatically access Azure Storage kívánja hello tetején.

```python
from azure.storage.file import FileService
```

### <a name="set-up-a-connection-tooazure-file-storage"></a>Egy kapcsolat tooAzure a File storage beállítása 
Hello `FileService` objektum lehetővé teszi, hogy a megosztások, könyvtárak és fájlok. hello alábbi kód létrehoz egy `FileService` objektum hello tárfiók neve és a fiók kulcsot használ. Cserélje le `<myaccount>` és `<mykey>` a fióknevet és kulcsot.

```python
file_service = FileService(account_name='myaccount', account_key='mykey')
```

### <a name="create-an-azure-file-share"></a>Azure-fájlmegosztás létrehozása
Az alábbi kódpéldát hello, használhatja a `FileService` objektum toocreate hello megosztást, ha még nem létezik.

```python
file_service.create_share('myshare')
```

### <a name="create-a-directory"></a>Könyvtár létrehozása
Tegyen alkönyvtárat ahelyett, az összes hello gyökérkönyvtárában lévő fájlok tárolási is rendezhetők. Az Azure File storage lehetővé teszi toocreate számos könyvtárat, a fiók fog lehetővé. hello kódot hoz létre a nevű alkönyvtárát **sampledir** hello gyökérkönyvtárban.

```python
file_service.create_directory('myshare', 'sampledir')
```

### <a name="enumerate-files-and-directories-in-an-azure-file-share"></a>Fájlok és könyvtárak egy Azure fájlmegosztás számbavétele
toolist hello fájlok és könyvtárak olyan megosztáson található, használja a hello **lista\_könyvtárak\_és\_fájlok** metódust. Ez a módszer egy generátor adja vissza. hello alábbira kimenete hello **neve** minden fájl és a könyvtár egy megosztás toohello konzolon.

```python
generator = file_service.list_directories_and_files('myshare')
for file_or_dir in generator:
    print(file_or_dir.name)
```

### <a name="upload-a-file"></a>Fájl feltöltése 
A megosztás nagyon legalább tartalmaz: hello Azure fájlt, fájlokat tároló is gyökérkönyvtár. Ebben a szakaszban megtudhatja, hogyan tooupload egy fájlt a helyi tároló alakzatot hello gyökérkönyvtár megosztás.

toocreate egy fájl és az adatok feltöltése, használja a hello `create_file_from_path`, `create_file_from_stream`, `create_file_from_bytes` vagy `create_file_from_text` módszerek. Magas szintű hello szükséges adattömbösítő Ha hello hello adatok mérete túllépi a 64 MB végző módszerekkel.

`create_file_from_path`feltöltések hello fájl megadott elérési hello és `create_file_from_stream` feltöltések hello egy már megnyitott fájl vagy adatfolyam tartalmát. `create_file_from_bytes`Bájttömb, feltölti és `create_file_from_text` feltöltések hello megadott szöveges érték hello segítségével megadott kódolási (alapértelmezett tooUTF-8).

hello alábbi példa feltölt hello hello tartalmát **sunset.png** hello fájlból **saját_fájl** fájlt.

```python
from azure.storage.file import ContentSettings
file_service.create_file_from_path(
    'myshare',
    None, # We want toocreate this blob in hello root directory, so we specify None for hello directory_name
    'myfile',
    'sunset.png',
    content_settings=ContentSettings(content_type='image/png'))
```

### <a name="download-a-file"></a>Fájl letöltése
toodownload adatok fájlból történő használata `get_file_to_path`, `get_file_to_stream`, `get_file_to_bytes`, vagy `get_file_to_text`. Magas szintű hello szükséges adattömbösítő Ha hello hello adatok mérete túllépi a 64 MB végző módszerekkel.

hello következő példa bemutatja, hogyan használatával `get_file_to_path` toodownload hello tartalmát hello **saját_fájl** fájlt, és tárolja toohello **out-sunset.png** fájlt.

```python
file_service.get_file_to_path('myshare', None, 'myfile', 'out-sunset.png')
```

### <a name="delete-a-file"></a>Fájl törlése
Végezetül toodelete egy fájl hívás `delete_file`.

```python
file_service.delete_file('myshare', None, 'myfile')
```

## <a name="next-steps"></a>Következő lépések
Most, hogy megismerte hogyan toomanipulate Azure File storage Python, kövesse a további hivatkozások toolearn.

* [Python fejlesztői központ](/develop/python/)
* [Az Azure Storage-szolgáltatások REST API-ja](http://msdn.microsoft.com/library/azure/dd179355)
* [A Microsoft Azure Storage Python SDK](https://github.com/Azure/azure-storage-python)