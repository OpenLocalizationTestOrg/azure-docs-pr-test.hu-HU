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
# <a name="develop-for-azure-file-storage-with-python"></a>Az Azure File storage Python fejlesztése
[!INCLUDE [storage-selector-file-include](../../includes/storage-selector-file-include.md)]

[!INCLUDE [storage-try-azure-tools-files](../../includes/storage-try-azure-tools-files.md)]

## <a name="about-this-tutorial"></a>Az oktatóanyag ismertetése
Ez az oktatóanyag mutatni, Python alkalmazásokhoz vagy szolgáltatásokhoz, tárolhatja a fájljait az Azure File storage segítségével fejlesztéséhez használatának alapjaival. Ebben az oktatóanyagban a rendszer egyszerű Konzolalkalmazás létrehozása és a Python és az Azure File storage alapvető műveleteket szemléltetik:

* Az Azure fájlmegosztások létrehozása
* Könyvtárak létrehozása
* Fájlok és könyvtárak egy Azure fájlmegosztás számbavétele
* Töltse fel, töltse le és törölje a fájlt

> [!Note]  
> Mivel előfordulhat, hogy Azure fájltároló SMB-n keresztül érhető el, akkor lehet egyszerű alkalmazások írását, amelyek a szabványos Python i/o-osztályok és függvény használata Azure fájlmegosztás eléréséhez. Ez a cikk azt ismerteti, hogyan alkalmazások írását, amelyek az Azure Storage Python SDK-val, használja a [Azure File storage REST API](https://docs.microsoft.com/en-us/rest/api/storageservices/fileservices/file-service-rest-api) felvegye a Azure File storage.

### <a name="set-up-your-application-to-use-azure-file-storage"></a>Állítsa be az alkalmazás Azure File storage használata
Adja hozzá a következő tetejénél található bármely Python forrásfájl, amelyben programon keresztüli eléréséhez az Azure Storage kívánja.

```python
from azure.storage.file import FileService
```

### <a name="set-up-a-connection-to-azure-file-storage"></a>Egy Azure File Storage-kapcsolat beállítása 
A `FileService` objektum lehetővé teszi, hogy a megosztások, könyvtárak és fájlok. Az alábbi kód létrehoz egy `FileService` objektumba a tárfiók nevét és a fiók kulcsot. Cserélje le `<myaccount>` és `<mykey>` a fióknevet és kulcsot.

```python
file_service = FileService(account_name='myaccount', account_key='mykey')
```

### <a name="create-an-azure-file-share"></a>Azure-fájlmegosztás létrehozása
Az alábbi példakód használhat egy `FileService` objektumot a megosztás létrehozásához, ha még nem létezik.

```python
file_service.create_share('myshare')
```

### <a name="create-a-directory"></a>Könyvtár létrehozása
Tárolási tegyen alkönyvtárat így ahelyett hogy ezek a gyökérkönyvtárban található fájlok is rendezhetők. Az Azure File storage hozhat létre a fiókját engedélyezi a könyvtárat. Az alábbi kódot hoz létre a nevű alkönyvtárát **sampledir** a gyökérkönyvtárban.

```python
file_service.create_directory('myshare', 'sampledir')
```

### <a name="enumerate-files-and-directories-in-an-azure-file-share"></a>Fájlok és könyvtárak egy Azure fájlmegosztás számbavétele
Kilistázhatja a fájlok és könyvtárak olyan megosztáson található, a **lista\_könyvtárak\_és\_fájlok** metódust. Ez a módszer egy generátor adja vissza. A következő kimenetek kódot a **neve** minden fájl és a könyvtár egy megosztáson található, a konzolhoz.

```python
generator = file_service.list_directories_and_files('myshare')
for file_or_dir in generator:
    print(file_or_dir.name)
```

### <a name="upload-a-file"></a>Fájl feltöltése 
A megosztás tartalmaz legalább az Azure File, egy gyökérkönyvtár fájlokat tároló is. Ebben a szakaszban megtudhatja, hogyan feltölteni a fájlt a helyi tároló megosztás gyökérkönyvtárában alakzatot.

Hozzon létre egy fájlt, és feltölteni az adatokat, használja a `create_file_from_path`, `create_file_from_stream`, `create_file_from_bytes` vagy `create_file_from_text` módszerek. Hajtsa végre a szükséges adattömbösítő, ha az adatok mérete meghaladja a 64 MB magas szintű módszerek.

`create_file_from_path`feltölt egy fájlt a megadott elérési és `create_file_from_stream` feltölt egy már megnyitott fájl vagy adatfolyam tartalmát. `create_file_from_bytes`Bájttömb, feltölti és `create_file_from_text` feltölti az adott szöveges értéket a megadott kódolás (alapértelmezett értéke UTF-8) használatával.

Az alábbi példa feltölti a tartalmát a **sunset.png** fájlt a **saját_fájl** fájlt.

```python
from azure.storage.file import ContentSettings
file_service.create_file_from_path(
    'myshare',
    None, # We want to create this blob in the root directory, so we specify None for the directory_name
    'myfile',
    'sunset.png',
    content_settings=ContentSettings(content_type='image/png'))
```

### <a name="download-a-file"></a>Fájl letöltése
Adatok fájlból való letöltéséhez használjon `get_file_to_path`, `get_file_to_stream`, `get_file_to_bytes`, vagy `get_file_to_text`. Hajtsa végre a szükséges adattömbösítő, ha az adatok mérete meghaladja a 64 MB magas szintű módszerek.

A következő példa bemutatja, hogy használatával `get_file_to_path` tartalmának letöltése a **saját_fájl** fájlt, és tárolja el azt, hogy a **out-sunset.png** fájlt.

```python
file_service.get_file_to_path('myshare', None, 'myfile', 'out-sunset.png')
```

### <a name="delete-a-file"></a>Fájl törlése
Végezetül, a fájl törléséhez hívja meg a `delete_file`.

```python
file_service.delete_file('myshare', None, 'myfile')
```

## <a name="next-steps"></a>Következő lépések
Most, hogy megismerte hogyan szeretné módosítani az Azure File storage Python, ezek hivatkozásokat követve tudhat meg többet.

* [Python fejlesztői központ](/develop/python/)
* [Az Azure Storage-szolgáltatások REST API-ja](http://msdn.microsoft.com/library/azure/dd179355)
* [A Microsoft Azure Storage Python SDK](https://github.com/Azure/azure-storage-python)