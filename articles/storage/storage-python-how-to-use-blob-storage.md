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
ms.openlocfilehash: 6efc61aa89e6d2544b7a18c80ce3546640f90462
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="how-toouse-azure-blob-storage-from-python"></a><span data-ttu-id="8fdac-103">Hogyan toouse Azure Blob storage-ának Python</span><span class="sxs-lookup"><span data-stu-id="8fdac-103">How toouse Azure Blob storage from Python</span></span>
[!INCLUDE [storage-selector-blob-include](../../includes/storage-selector-blob-include.md)]

[!INCLUDE [storage-try-azure-tools-blobs](../../includes/storage-try-azure-tools-blobs.md)]

## <a name="overview"></a><span data-ttu-id="8fdac-104">Áttekintés</span><span class="sxs-lookup"><span data-stu-id="8fdac-104">Overview</span></span>
<span data-ttu-id="8fdac-105">Az Azure Blob storage egy olyan szolgáltatás, amely hello felhő strukturálatlan adatokat objektumként/blobként tárolja.</span><span class="sxs-lookup"><span data-stu-id="8fdac-105">Azure Blob storage is a service that stores unstructured data in hello cloud as objects/blobs.</span></span> <span data-ttu-id="8fdac-106">A Blob Storage képes tárolni bármilyen szöveget vagy bináris adatot, például dokumentumot, médiafájlt vagy egy alkalmazástelepítőt.</span><span class="sxs-lookup"><span data-stu-id="8fdac-106">Blob storage can store any type of text or binary data, such as a document, media file, or application installer.</span></span> <span data-ttu-id="8fdac-107">A BLOB storage is az említett tooas objektum tároló.</span><span class="sxs-lookup"><span data-stu-id="8fdac-107">Blob storage is also referred tooas object storage.</span></span>

<span data-ttu-id="8fdac-108">Ez a cikk bemutatja, hogyan tooperform szolgáltatást használó általános forgatókönyvhöz Blob Storage tárolóban.</span><span class="sxs-lookup"><span data-stu-id="8fdac-108">This article will show you how tooperform common scenarios using Blob storage.</span></span> <span data-ttu-id="8fdac-109">hello minták Python nyelven íródtak, és használja a hello [Microsoft Azure Storage SDK for Python].</span><span class="sxs-lookup"><span data-stu-id="8fdac-109">hello samples are written in Python and use hello [Microsoft Azure Storage SDK for Python].</span></span> <span data-ttu-id="8fdac-110">hello jelzett esetek feltöltése, listázása, letöltése és blobok törlése.</span><span class="sxs-lookup"><span data-stu-id="8fdac-110">hello scenarios covered include uploading, listing, downloading, and deleting blobs.</span></span>

[!INCLUDE [storage-blob-concepts-include](../../includes/storage-blob-concepts-include.md)]

[!INCLUDE [storage-create-account-include](../../includes/storage-create-account-include.md)]

## <a name="create-a-container"></a><span data-ttu-id="8fdac-111">Tároló létrehozása</span><span class="sxs-lookup"><span data-stu-id="8fdac-111">Create a container</span></span>
<span data-ttu-id="8fdac-112">A blob típusú hello alapján szeretné toouse, hozzon létre egy **BlockBlobService**, **AppendBlobService**, vagy **PageBlobService** objektum.</span><span class="sxs-lookup"><span data-stu-id="8fdac-112">Based on hello type of blob you would like toouse, create a **BlockBlobService**, **AppendBlobService**, or **PageBlobService** object.</span></span> <span data-ttu-id="8fdac-113">hello következő kódban a **BlockBlobService** objektum.</span><span class="sxs-lookup"><span data-stu-id="8fdac-113">hello following code uses a **BlockBlobService** object.</span></span> <span data-ttu-id="8fdac-114">Adja hozzá a hello következő közelében hello felső bármely Python fájl tooprogrammatically access Azure Blob Blokktárolást kívánja.</span><span class="sxs-lookup"><span data-stu-id="8fdac-114">Add hello following near hello top of any Python file in which you wish tooprogrammatically access Azure Block Blob Storage.</span></span>

```python
from azure.storage.blob import BlockBlobService
```

<span data-ttu-id="8fdac-115">hello alábbi kód létrehoz egy **BlockBlobService** objektum hello tárfiók neve és a fiók kulcsot használ.</span><span class="sxs-lookup"><span data-stu-id="8fdac-115">hello following code creates a **BlockBlobService** object using hello storage account name and account key.</span></span>  <span data-ttu-id="8fdac-116">"Myaccount" és "SajátKulcs" cserélje le a fióknevet és kulcsot.</span><span class="sxs-lookup"><span data-stu-id="8fdac-116">Replace 'myaccount' and 'mykey' with your account name and key.</span></span>

```python
block_blob_service = BlockBlobService(account_name='myaccount', account_key='mykey')
```

[!INCLUDE [storage-container-naming-rules-include](../../includes/storage-container-naming-rules-include.md)]

<span data-ttu-id="8fdac-117">Az alábbi kódpéldát hello, használhatja a **BlockBlobService** toocreate hello objektumtároló Ha még nem létezik.</span><span class="sxs-lookup"><span data-stu-id="8fdac-117">In hello following code example, you can use a **BlockBlobService** object toocreate hello container if it doesn't exist.</span></span>

```python
block_blob_service.create_container('mycontainer')
```

<span data-ttu-id="8fdac-118">Alapértelmezés szerint hello új tároló sajátja, ezért meg kell adnia a tárelérési kulcsát (úgy, ahogy korábban) toodownload blobok ebből a tárolóból.</span><span class="sxs-lookup"><span data-stu-id="8fdac-118">By default, hello new container is private, so you must specify your storage access key (as you did earlier) toodownload blobs from this container.</span></span> <span data-ttu-id="8fdac-119">Ha található hello tároló elérhető tooeveryone toomake hello blobok, hello tároló létrehozása, és adja át a hello nyilvános hozzáférési szint a következő kód hello használata.</span><span class="sxs-lookup"><span data-stu-id="8fdac-119">If you want toomake hello blobs within hello container available tooeveryone, you can create hello container and pass hello public access level using hello following code.</span></span>

```python
from azure.storage.blob import PublicAccess
block_blob_service.create_container('mycontainer', public_access=PublicAccess.Container)
```

<span data-ttu-id="8fdac-120">Módosíthatja azt is megteheti, tárolója, miután létrehozta a következő kód hello segítségével.</span><span class="sxs-lookup"><span data-stu-id="8fdac-120">Alternatively, you can modify a container after you have created it using hello following code.</span></span>

```python
block_blob_service.set_container_acl('mycontainer', public_access=PublicAccess.Container)
```

<span data-ttu-id="8fdac-121">Ez a módosítás után hello Internet bárki láthatja a nyilvános tárolókban lévő blobokat, de csak módosíthatja vagy törölheti őket.</span><span class="sxs-lookup"><span data-stu-id="8fdac-121">After this change, anyone on hello Internet can see blobs in a public container, but only you can modify or delete them.</span></span>

## <a name="upload-a-blob-into-a-container"></a><span data-ttu-id="8fdac-122">Blobok feltöltése a tárolóba</span><span class="sxs-lookup"><span data-stu-id="8fdac-122">Upload a blob into a container</span></span>
<span data-ttu-id="8fdac-123">a blokkblob toocreate és az adatok feltöltése hello használata **létrehozása\_blob\_a\_elérési**, **létrehozása\_blob\_a\_adatfolyam**, **létrehozása\_blob\_a\_bájt** vagy **létrehozása\_blob\_a\_szöveg** módszerek.</span><span class="sxs-lookup"><span data-stu-id="8fdac-123">toocreate a block blob and upload data, use hello **create\_blob\_from\_path**, **create\_blob\_from\_stream**, **create\_blob\_from\_bytes** or **create\_blob\_from\_text** methods.</span></span> <span data-ttu-id="8fdac-124">Magas szintű hello szükséges adattömbösítő Ha hello hello adatok mérete túllépi a 64 MB végző módszerekkel.</span><span class="sxs-lookup"><span data-stu-id="8fdac-124">They are high-level methods that perform hello necessary chunking when hello size of hello data exceeds 64 MB.</span></span>

<span data-ttu-id="8fdac-125">**Hozzon létre\_blob\_a\_elérési** feltöltések hello fájl megadott elérési hello és **létrehozása\_blob\_a\_adatfolyam** feltöltések hello egy már megnyitott fájl vagy adatfolyam tartalmát.</span><span class="sxs-lookup"><span data-stu-id="8fdac-125">**create\_blob\_from\_path** uploads hello contents of a file from hello specified path, and **create\_blob\_from\_stream** uploads hello contents from an already opened file/stream.</span></span> <span data-ttu-id="8fdac-126">**Hozzon létre\_blob\_a\_bájt** bájttömb, feltölti és **létrehozása\_blob\_a\_szöveg** megadott hello feltölt szöveges érték hello segítségével megadott kódolási (alapértelmezett tooUTF-8).</span><span class="sxs-lookup"><span data-stu-id="8fdac-126">**create\_blob\_from\_bytes** uploads an array of bytes, and **create\_blob\_from\_text** uploads hello specified text value using hello specified encoding (defaults tooUTF-8).</span></span>

<span data-ttu-id="8fdac-127">hello alábbi példa feltölt hello hello tartalmát **sunset.png** hello fájlból **myblob** blob.</span><span class="sxs-lookup"><span data-stu-id="8fdac-127">hello following example uploads hello contents of hello **sunset.png** file into hello **myblob** blob.</span></span>

```python
from azure.storage.blob import ContentSettings
block_blob_service.create_blob_from_path(
    'mycontainer',
    'myblockblob',
    'sunset.png',
    content_settings=ContentSettings(content_type='image/png')
            )
```

## <a name="list-hello-blobs-in-a-container"></a><span data-ttu-id="8fdac-128">Lista hello a tárolóban lévő blobok</span><span class="sxs-lookup"><span data-stu-id="8fdac-128">List hello blobs in a container</span></span>
<span data-ttu-id="8fdac-129">toolist hello BLOB a tárolóban lévő hello használata **lista\_blobok** metódust.</span><span class="sxs-lookup"><span data-stu-id="8fdac-129">toolist hello blobs in a container, use hello **list\_blobs** method.</span></span> <span data-ttu-id="8fdac-130">Ez a módszer egy generátor adja vissza.</span><span class="sxs-lookup"><span data-stu-id="8fdac-130">This method returns a generator.</span></span> <span data-ttu-id="8fdac-131">hello alábbira kimenete hello **neve** minden egyes BLOB tároló toohello konzolban.</span><span class="sxs-lookup"><span data-stu-id="8fdac-131">hello following code outputs hello **name** of each blob in a container toohello console.</span></span>

```python
generator = block_blob_service.list_blobs('mycontainer')
for blob in generator:
    print(blob.name)
```

## <a name="download-blobs"></a><span data-ttu-id="8fdac-132">Blobok letöltése</span><span class="sxs-lookup"><span data-stu-id="8fdac-132">Download blobs</span></span>
<span data-ttu-id="8fdac-133">toodownload adatait egy blob használja **beolvasása\_blob\_való\_elérési**, **beolvasása\_blob\_való\_adatfolyam**, **beolvasása\_blob\_való\_bájt**, vagy **beolvasása\_blob\_való\_szöveg**.</span><span class="sxs-lookup"><span data-stu-id="8fdac-133">toodownload data from a blob, use **get\_blob\_to\_path**, **get\_blob\_to\_stream**, **get\_blob\_to\_bytes**, or **get\_blob\_to\_text**.</span></span> <span data-ttu-id="8fdac-134">Magas szintű hello szükséges adattömbösítő Ha hello hello adatok mérete túllépi a 64 MB végző módszerekkel.</span><span class="sxs-lookup"><span data-stu-id="8fdac-134">They are high-level methods that perform hello necessary chunking when hello size of hello data exceeds 64 MB.</span></span>

<span data-ttu-id="8fdac-135">hello következő példa bemutatja, hogyan használatával **beolvasása\_blob\_a\_elérési** toodownload hello tartalmát hello **myblob** blob és toohello tárolja**out-sunset.png** fájlt.</span><span class="sxs-lookup"><span data-stu-id="8fdac-135">hello following example demonstrates using **get\_blob\_to\_path** toodownload hello contents of hello **myblob** blob and store it toohello **out-sunset.png** file.</span></span>

```python
block_blob_service.get_blob_to_path('mycontainer', 'myblockblob', 'out-sunset.png')
```

## <a name="delete-a-blob"></a><span data-ttu-id="8fdac-136">Blob törlése</span><span class="sxs-lookup"><span data-stu-id="8fdac-136">Delete a blob</span></span>
<span data-ttu-id="8fdac-137">Végezetül toodelete blob, hívja **delete_blob**.</span><span class="sxs-lookup"><span data-stu-id="8fdac-137">Finally, toodelete a blob, call **delete_blob**.</span></span>

```python
block_blob_service.delete_blob('mycontainer', 'myblockblob')
```

## <a name="writing-tooan-append-blob"></a><span data-ttu-id="8fdac-138">Írás tooan hozzáfűző blob</span><span class="sxs-lookup"><span data-stu-id="8fdac-138">Writing tooan append blob</span></span>
<span data-ttu-id="8fdac-139">A hozzáfűző blob hozzáfűzési feladatokra, például naplózásra van optimalizálva.</span><span class="sxs-lookup"><span data-stu-id="8fdac-139">An append blob is optimized for append operations, such as logging.</span></span> <span data-ttu-id="8fdac-140">Blokkblob, például a hozzáfűző blob blokkok magában foglalja, de ha hozzáad egy új blokkot tooan hozzáfűző blob, mindig hozzáfűzött toohello hello blob végéhez.</span><span class="sxs-lookup"><span data-stu-id="8fdac-140">Like a block blob, an append blob is comprised of blocks, but when you add a new block tooan append blob, it is always appended toohello end of hello blob.</span></span> <span data-ttu-id="8fdac-141">Hozzáfűző blob meglévő blokkja nem frissíthető és nem törölhető.</span><span class="sxs-lookup"><span data-stu-id="8fdac-141">You cannot update or delete an existing block in an append blob.</span></span> <span data-ttu-id="8fdac-142">a hozzáfűző BLOB azonosítók hello blokk nem érhetők el, mert azok egy blokkblobhoz tartoznak.</span><span class="sxs-lookup"><span data-stu-id="8fdac-142">hello block IDs for an append blob are not exposed as they are for a block blob.</span></span>

<span data-ttu-id="8fdac-143">A hozzáfűző blob minden blokkja különböző méretű mentése tooa legfeljebb 4 MB lehet, és a hozzáfűző blob legfeljebb 50 000 blokkot tartalmazhat.</span><span class="sxs-lookup"><span data-stu-id="8fdac-143">Each block in an append blob can be a different size, up tooa maximum of 4 MB, and an append blob can include a maximum of 50,000 blocks.</span></span> <span data-ttu-id="8fdac-144">hello maximális hozzáfűző blob mérete ezért valamivel több mint 195 GB (4 MB X 50 000 blokk).</span><span class="sxs-lookup"><span data-stu-id="8fdac-144">hello maximum size of an append blob is therefore slightly more than 195 GB (4 MB X 50,000 blocks).</span></span>

<span data-ttu-id="8fdac-145">hello az alábbi példa létrehoz egy új hozzáfűző blob, és hozzáfűzi egyes adatok tooit, egy egyszerű naplózási műveletet szimulálva.</span><span class="sxs-lookup"><span data-stu-id="8fdac-145">hello example below creates a new append blob and appends some data tooit, simulating a simple logging operation.</span></span>

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

## <a name="next-steps"></a><span data-ttu-id="8fdac-146">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="8fdac-146">Next steps</span></span>
<span data-ttu-id="8fdac-147">Most, hogy megismerte a Blob storage alapjait hello, kövesse az alábbi hivatkozások további toolearn.</span><span class="sxs-lookup"><span data-stu-id="8fdac-147">Now that you've learned hello basics of Blob storage, follow these links toolearn more.</span></span>

* [<span data-ttu-id="8fdac-148">Python fejlesztői központ</span><span class="sxs-lookup"><span data-stu-id="8fdac-148">Python Developer Center</span></span>](https://azure.microsoft.com/develop/python/)
* [<span data-ttu-id="8fdac-149">Az Azure Storage-szolgáltatások REST API-ja</span><span class="sxs-lookup"><span data-stu-id="8fdac-149">Azure Storage Services REST API</span></span>](http://msdn.microsoft.com/library/azure/dd179355)
* <span data-ttu-id="8fdac-150">[Az Azure Storage csapat blogja]</span><span class="sxs-lookup"><span data-stu-id="8fdac-150">[Azure Storage Team Blog]</span></span>
* <span data-ttu-id="8fdac-151">[Microsoft Azure Storage SDK for Python]</span><span class="sxs-lookup"><span data-stu-id="8fdac-151">[Microsoft Azure Storage SDK for Python]</span></span>

[Az Azure Storage csapat blogja]: http://blogs.msdn.com/b/windowsazurestorage/
[Microsoft Azure Storage SDK for Python]: https://github.com/Azure/azure-storage-python
