---
title: Azure Blob storage (object storage) a Python aaaHow toouse |} Microsoft Docs
description: "Strukturálatlan adatok tárolása az Azure Blob storage (object storage) hello felhő."
services: storage
documentationcenter: python
author: mmacy
manager: timlt
editor: tysonn
ms.assetid: 0348e360-b24d-41fa-bb12-b8f18990d8bc
ms.service: storage
ms.workload: storage
ms.tgt_pltfrm: na
ms.devlang: python
ms.topic: article
ms.date: 2/24/2017
ms.author: marsma
ms.openlocfilehash: 8f9ca93e52b030384e28a739d2f1c6b610be094a
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="how-toouse-azure-blob-storage-from-python"></a>Hogyan toouse Azure Blob storage-ának Python
[!INCLUDE [storage-selector-blob-include](../../../includes/storage-selector-blob-include.md)]

[!INCLUDE [storage-try-azure-tools-blobs](../../../includes/storage-try-azure-tools-blobs.md)]

## <a name="overview"></a>Áttekintés
Az Azure Blob storage egy olyan szolgáltatás, amely hello felhő strukturálatlan adatokat objektumként/blobként tárolja. A Blob Storage képes tárolni bármilyen szöveget vagy bináris adatot, például dokumentumot, médiafájlt vagy egy alkalmazástelepítőt. A BLOB storage is az említett tooas objektum tároló.

Ez a cikk bemutatja, hogyan tooperform szolgáltatást használó általános forgatókönyvhöz Blob Storage tárolóban. hello minták Python nyelven íródtak, és használja a hello [Microsoft Azure Storage SDK for Python]. hello jelzett esetek feltöltése, listázása, letöltése és blobok törlése.

[!INCLUDE [storage-blob-concepts-include](../../../includes/storage-blob-concepts-include.md)]

[!INCLUDE [storage-create-account-include](../../../includes/storage-create-account-include.md)]

## <a name="create-a-container"></a>Tároló létrehozása
A blob típusú hello alapján szeretné toouse, hozzon létre egy **BlockBlobService**, **AppendBlobService**, vagy **PageBlobService** objektum. hello következő kódban a **BlockBlobService** objektum. Adja hozzá a hello következő közelében hello felső bármely Python fájl tooprogrammatically access Azure Blob Blokktárolást kívánja.

```python
from azure.storage.blob import BlockBlobService
```

hello alábbi kód létrehoz egy **BlockBlobService** objektum hello tárfiók neve és a fiók kulcsot használ.  "Myaccount" és "SajátKulcs" cserélje le a fióknevet és kulcsot.

```python
block_blob_service = BlockBlobService(account_name='myaccount', account_key='mykey')
```

[!INCLUDE [storage-container-naming-rules-include](../../../includes/storage-container-naming-rules-include.md)]

Az alábbi kódpéldát hello, használhatja a **BlockBlobService** toocreate hello objektumtároló Ha még nem létezik.

```python
block_blob_service.create_container('mycontainer')
```

Alapértelmezés szerint hello új tároló sajátja, ezért meg kell adnia a tárelérési kulcsát (úgy, ahogy korábban) toodownload blobok ebből a tárolóból. Ha található hello tároló elérhető tooeveryone toomake hello blobok, hello tároló létrehozása, és adja át a hello nyilvános hozzáférési szint a következő kód hello használata.

```python
from azure.storage.blob import PublicAccess
block_blob_service.create_container('mycontainer', public_access=PublicAccess.Container)
```

Módosíthatja azt is megteheti, tárolója, miután létrehozta a következő kód hello segítségével.

```python
block_blob_service.set_container_acl('mycontainer', public_access=PublicAccess.Container)
```

Ez a módosítás után hello Internet bárki láthatja a nyilvános tárolókban lévő blobokat, de csak módosíthatja vagy törölheti őket.

## <a name="upload-a-blob-into-a-container"></a>Blobok feltöltése a tárolóba
a blokkblob toocreate és az adatok feltöltése hello használata **létrehozása\_blob\_a\_elérési**, **létrehozása\_blob\_a\_adatfolyam**, **létrehozása\_blob\_a\_bájt** vagy **létrehozása\_blob\_a\_szöveg** módszerek. Magas szintű hello szükséges adattömbösítő Ha hello hello adatok mérete túllépi a 64 MB végző módszerekkel.

**Hozzon létre\_blob\_a\_elérési** feltöltések hello fájl megadott elérési hello és **létrehozása\_blob\_a\_adatfolyam** feltöltések hello egy már megnyitott fájl vagy adatfolyam tartalmát. **Hozzon létre\_blob\_a\_bájt** bájttömb, feltölti és **létrehozása\_blob\_a\_szöveg** megadott hello feltölt szöveges érték hello segítségével megadott kódolási (alapértelmezett tooUTF-8).

hello alábbi példa feltölt hello hello tartalmát **sunset.png** hello fájlból **myblob** blob.

```python
from azure.storage.blob import ContentSettings
block_blob_service.create_blob_from_path(
    'mycontainer',
    'myblockblob',
    'sunset.png',
    content_settings=ContentSettings(content_type='image/png')
            )
```

## <a name="list-hello-blobs-in-a-container"></a>Lista hello a tárolóban lévő blobok
toolist hello BLOB a tárolóban lévő hello használata **lista\_blobok** metódust. Ez a módszer egy generátor adja vissza. hello alábbira kimenete hello **neve** minden egyes BLOB tároló toohello konzolban.

```python
generator = block_blob_service.list_blobs('mycontainer')
for blob in generator:
    print(blob.name)
```

## <a name="download-blobs"></a>Blobok letöltése
toodownload adatait egy blob használja **beolvasása\_blob\_való\_elérési**, **beolvasása\_blob\_való\_adatfolyam**, **beolvasása\_blob\_való\_bájt**, vagy **beolvasása\_blob\_való\_szöveg**. Magas szintű hello szükséges adattömbösítő Ha hello hello adatok mérete túllépi a 64 MB végző módszerekkel.

hello következő példa bemutatja, hogyan használatával **beolvasása\_blob\_a\_elérési** toodownload hello tartalmát hello **myblob** blob és toohello tárolja**out-sunset.png** fájlt.

```python
block_blob_service.get_blob_to_path('mycontainer', 'myblockblob', 'out-sunset.png')
```

## <a name="delete-a-blob"></a>Blob törlése
Végezetül toodelete blob, hívja **delete_blob**.

```python
block_blob_service.delete_blob('mycontainer', 'myblockblob')
```

## <a name="writing-tooan-append-blob"></a>Írás tooan hozzáfűző blob
A hozzáfűző blob hozzáfűzési feladatokra, például naplózásra van optimalizálva. Blokkblob, például a hozzáfűző blob blokkok magában foglalja, de ha hozzáad egy új blokkot tooan hozzáfűző blob, mindig hozzáfűzött toohello hello blob végéhez. Hozzáfűző blob meglévő blokkja nem frissíthető és nem törölhető. a hozzáfűző BLOB azonosítók hello blokk nem érhetők el, mert azok egy blokkblobhoz tartoznak.

A hozzáfűző blob minden blokkja különböző méretű mentése tooa legfeljebb 4 MB lehet, és a hozzáfűző blob legfeljebb 50 000 blokkot tartalmazhat. hello maximális hozzáfűző blob mérete ezért valamivel több mint 195 GB (4 MB X 50 000 blokk).

hello az alábbi példa létrehoz egy új hozzáfűző blob, és hozzáfűzi egyes adatok tooit, egy egyszerű naplózási műveletet szimulálva.

```python
from azure.storage.blob import AppendBlobService
append_blob_service = AppendBlobService(account_name='myaccount', account_key='mykey')

# hello same containers can hold all types of blobs
append_blob_service.create_container('mycontainer')

# Append blobs must be created before they are appended to
append_blob_service.create_blob('mycontainer', 'myappendblob')
append_blob_service.append_blob_from_text('mycontainer', 'myappendblob', u'Hello, world!')

append_blob = append_blob_service.get_blob_to_text('mycontainer', 'myappendblob')
```

## <a name="next-steps"></a>Következő lépések
Most, hogy megismerte a Blob storage alapjait hello, kövesse az alábbi hivatkozások további toolearn.

* [Python fejlesztői központ](https://azure.microsoft.com/develop/python/)
* [Az Azure Storage-szolgáltatások REST API-ja](http://msdn.microsoft.com/library/azure/dd179355)
* [Az Azure Storage csapat blogja]
* [Microsoft Azure Storage SDK for Python]

[Az Azure Storage csapat blogja]: http://blogs.msdn.com/b/windowsazurestorage/
[Microsoft Azure Storage SDK for Python]: https://github.com/Azure/azure-storage-python
