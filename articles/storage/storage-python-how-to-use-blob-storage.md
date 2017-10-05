---
title: "Hogyan használható az Azure Blob storage (object storage) a Python |} Microsoft Docs"
description: "Store unstructured data in the cloud with Azure Blob storage (object storage) (Strukturálatlan adatok tárolása a felhőben Azure Blob Storage-fiókkal (objektumtároló))."
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
ms.openlocfilehash: 968814db9496fd410162d482191592c8a56101f0
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="how-to-use-azure-blob-storage-from-python"></a><span data-ttu-id="2af89-103">Azure Blob storage-ának Python használata</span><span class="sxs-lookup"><span data-stu-id="2af89-103">How to use Azure Blob storage from Python</span></span>
[!INCLUDE [storage-selector-blob-include](../../includes/storage-selector-blob-include.md)]

[!INCLUDE [storage-try-azure-tools-blobs](../../includes/storage-try-azure-tools-blobs.md)]

## <a name="overview"></a><span data-ttu-id="2af89-104">Áttekintés</span><span class="sxs-lookup"><span data-stu-id="2af89-104">Overview</span></span>
<span data-ttu-id="2af89-105">Az Azure Blob Storage egy olyan szolgáltatás, amely a strukturálatlan adatokat objektumként/blobként tárolja a felhőben.</span><span class="sxs-lookup"><span data-stu-id="2af89-105">Azure Blob storage is a service that stores unstructured data in the cloud as objects/blobs.</span></span> <span data-ttu-id="2af89-106">A Blob Storage képes tárolni bármilyen szöveget vagy bináris adatot, például dokumentumot, médiafájlt vagy egy alkalmazástelepítőt.</span><span class="sxs-lookup"><span data-stu-id="2af89-106">Blob storage can store any type of text or binary data, such as a document, media file, or application installer.</span></span> <span data-ttu-id="2af89-107">A Blob Storage más néven objektumtárnak is hívható.</span><span class="sxs-lookup"><span data-stu-id="2af89-107">Blob storage is also referred to as object storage.</span></span>

<span data-ttu-id="2af89-108">Ez a cikk bemutatja, hogyan hajthat végre a Blob storage használatával gyakori forgatókönyvek.</span><span class="sxs-lookup"><span data-stu-id="2af89-108">This article will show you how to perform common scenarios using Blob storage.</span></span> <span data-ttu-id="2af89-109">A mintákat a Python és -felhasználási nyelven íródtak a [Microsoft Azure Storage SDK for Python].</span><span class="sxs-lookup"><span data-stu-id="2af89-109">The samples are written in Python and use the [Microsoft Azure Storage SDK for Python].</span></span> <span data-ttu-id="2af89-110">Az ismertetett forgatókönyvek közé tartozik a feltöltése, listázása, letöltése és blobok törlése.</span><span class="sxs-lookup"><span data-stu-id="2af89-110">The scenarios covered include uploading, listing, downloading, and deleting blobs.</span></span>

[!INCLUDE [storage-blob-concepts-include](../../includes/storage-blob-concepts-include.md)]

[!INCLUDE [storage-create-account-include](../../includes/storage-create-account-include.md)]

## <a name="create-a-container"></a><span data-ttu-id="2af89-111">Tároló létrehozása</span><span class="sxs-lookup"><span data-stu-id="2af89-111">Create a container</span></span>
<span data-ttu-id="2af89-112">Alapján a blob szeretne használni, hozzon létre egy **BlockBlobService**, **AppendBlobService**, vagy **PageBlobService** objektum.</span><span class="sxs-lookup"><span data-stu-id="2af89-112">Based on the type of blob you would like to use, create a **BlockBlobService**, **AppendBlobService**, or **PageBlobService** object.</span></span> <span data-ttu-id="2af89-113">A következő kódban a **BlockBlobService** objektum.</span><span class="sxs-lookup"><span data-stu-id="2af89-113">The following code uses a **BlockBlobService** object.</span></span> <span data-ttu-id="2af89-114">Adja hozzá a következő tetejénél található minden olyan Python fájlt, amelyben programon keresztüli eléréséhez az Azure Blob Blokktárolást kívánja.</span><span class="sxs-lookup"><span data-stu-id="2af89-114">Add the following near the top of any Python file in which you wish to programmatically access Azure Block Blob Storage.</span></span>

```python
from azure.storage.blob import BlockBlobService
```

<span data-ttu-id="2af89-115">Az alábbi kód létrehoz egy **BlockBlobService** objektumba a tárfiók nevét és a fiók kulcsot.</span><span class="sxs-lookup"><span data-stu-id="2af89-115">The following code creates a **BlockBlobService** object using the storage account name and account key.</span></span>  <span data-ttu-id="2af89-116">"Myaccount" és "SajátKulcs" cserélje le a fióknevet és kulcsot.</span><span class="sxs-lookup"><span data-stu-id="2af89-116">Replace 'myaccount' and 'mykey' with your account name and key.</span></span>

```python
block_blob_service = BlockBlobService(account_name='myaccount', account_key='mykey')
```

[!INCLUDE [storage-container-naming-rules-include](../../includes/storage-container-naming-rules-include.md)]

<span data-ttu-id="2af89-117">Az alábbi példakód használhat egy **BlockBlobService** objektumot létrehozni a tárolót, ha még nem létezik.</span><span class="sxs-lookup"><span data-stu-id="2af89-117">In the following code example, you can use a **BlockBlobService** object to create the container if it doesn't exist.</span></span>

```python
block_blob_service.create_container('mycontainer')
```

<span data-ttu-id="2af89-118">Alapértelmezés szerint az új tároló sajátja, ezért meg kell adnia a tárelérési kulcsát (úgy, ahogy korábban) a blobok letöltését az ebben a tárolóban.</span><span class="sxs-lookup"><span data-stu-id="2af89-118">By default, the new container is private, so you must specify your storage access key (as you did earlier) to download blobs from this container.</span></span> <span data-ttu-id="2af89-119">Ha azt szeretné, hogy a tárolóban található blobok mindenki számára elérhetővé, hozza létre a tárolót, és adja át a nyilvános hozzáférési szint, az alábbi kód használatával.</span><span class="sxs-lookup"><span data-stu-id="2af89-119">If you want to make the blobs within the container available to everyone, you can create the container and pass the public access level using the following code.</span></span>

```python
from azure.storage.blob import PublicAccess
block_blob_service.create_container('mycontainer', public_access=PublicAccess.Container)
```

<span data-ttu-id="2af89-120">Azt is megteheti módosíthatja a tároló a létrehozást követően az alábbi kód használatával.</span><span class="sxs-lookup"><span data-stu-id="2af89-120">Alternatively, you can modify a container after you have created it using the following code.</span></span>

```python
block_blob_service.set_container_acl('mycontainer', public_access=PublicAccess.Container)
```

<span data-ttu-id="2af89-121">Ez a módosítás után bárki láthatja a nyilvános tárolókban lévő blobokat, de csak módosíthatja vagy törölheti őket.</span><span class="sxs-lookup"><span data-stu-id="2af89-121">After this change, anyone on the Internet can see blobs in a public container, but only you can modify or delete them.</span></span>

## <a name="upload-a-blob-into-a-container"></a><span data-ttu-id="2af89-122">Blobok feltöltése a tárolóba</span><span class="sxs-lookup"><span data-stu-id="2af89-122">Upload a blob into a container</span></span>
<span data-ttu-id="2af89-123">A blokkblob létrehozásához, és töltse fel az adatokat, használja a **létrehozása\_blob\_a\_elérési**, **létrehozása\_blob\_a\_adatfolyam**, **létrehozása\_blob\_a\_bájt** vagy **létrehozása\_blob\_a\_szöveg** módszerek.</span><span class="sxs-lookup"><span data-stu-id="2af89-123">To create a block blob and upload data, use the **create\_blob\_from\_path**, **create\_blob\_from\_stream**, **create\_blob\_from\_bytes** or **create\_blob\_from\_text** methods.</span></span> <span data-ttu-id="2af89-124">Hajtsa végre a szükséges adattömbösítő, ha az adatok mérete meghaladja a 64 MB magas szintű módszerek.</span><span class="sxs-lookup"><span data-stu-id="2af89-124">They are high-level methods that perform the necessary chunking when the size of the data exceeds 64 MB.</span></span>

<span data-ttu-id="2af89-125">**Hozzon létre\_blob\_a\_elérési** feltölt egy fájlt a megadott elérési és **létrehozása\_blob\_a\_adatfolyam** feltölt egy már megnyitott fájl vagy adatfolyam tartalmát.</span><span class="sxs-lookup"><span data-stu-id="2af89-125">**create\_blob\_from\_path** uploads the contents of a file from the specified path, and **create\_blob\_from\_stream** uploads the contents from an already opened file/stream.</span></span> <span data-ttu-id="2af89-126">**Hozzon létre\_blob\_a\_bájt** bájttömb, feltölti és **létrehozása\_blob\_a\_szöveg** feltölti az adott szöveges értéket a megadott kódolás (alapértelmezett értéke UTF-8) használatával.</span><span class="sxs-lookup"><span data-stu-id="2af89-126">**create\_blob\_from\_bytes** uploads an array of bytes, and **create\_blob\_from\_text** uploads the specified text value using the specified encoding (defaults to UTF-8).</span></span>

<span data-ttu-id="2af89-127">Az alábbi példa feltölti a tartalmát a **sunset.png** fájlt a **myblob** blob.</span><span class="sxs-lookup"><span data-stu-id="2af89-127">The following example uploads the contents of the **sunset.png** file into the **myblob** blob.</span></span>

```python
from azure.storage.blob import ContentSettings
block_blob_service.create_blob_from_path(
    'mycontainer',
    'myblockblob',
    'sunset.png',
    content_settings=ContentSettings(content_type='image/png')
            )
```

## <a name="list-the-blobs-in-a-container"></a><span data-ttu-id="2af89-128">A tárolóban lévő blobok listázása</span><span class="sxs-lookup"><span data-stu-id="2af89-128">List the blobs in a container</span></span>
<span data-ttu-id="2af89-129">A tárolóban lévő blobok listázásához, használja a **lista\_blobok** metódust.</span><span class="sxs-lookup"><span data-stu-id="2af89-129">To list the blobs in a container, use the **list\_blobs** method.</span></span> <span data-ttu-id="2af89-130">Ez a módszer egy generátor adja vissza.</span><span class="sxs-lookup"><span data-stu-id="2af89-130">This method returns a generator.</span></span> <span data-ttu-id="2af89-131">A következő kimenetek kódot a **neve** minden egyes BLOB tároló a konzolhoz.</span><span class="sxs-lookup"><span data-stu-id="2af89-131">The following code outputs the **name** of each blob in a container to the console.</span></span>

```python
generator = block_blob_service.list_blobs('mycontainer')
for blob in generator:
    print(blob.name)
```

## <a name="download-blobs"></a><span data-ttu-id="2af89-132">Blobok letöltése</span><span class="sxs-lookup"><span data-stu-id="2af89-132">Download blobs</span></span>
<span data-ttu-id="2af89-133">Blob adatok letöltéséhez használjon **beolvasása\_blob\_való\_elérési**, **beolvasása\_blob\_való\_adatfolyam**, **beolvasása\_blob\_való\_bájt**, vagy **beolvasása\_blob\_való\_szöveg**.</span><span class="sxs-lookup"><span data-stu-id="2af89-133">To download data from a blob, use **get\_blob\_to\_path**, **get\_blob\_to\_stream**, **get\_blob\_to\_bytes**, or **get\_blob\_to\_text**.</span></span> <span data-ttu-id="2af89-134">Hajtsa végre a szükséges adattömbösítő, ha az adatok mérete meghaladja a 64 MB magas szintű módszerek.</span><span class="sxs-lookup"><span data-stu-id="2af89-134">They are high-level methods that perform the necessary chunking when the size of the data exceeds 64 MB.</span></span>

<span data-ttu-id="2af89-135">A következő példa bemutatja, hogy használatával **beolvasása\_blob\_való\_elérési** tartalmának letöltése a **myblob** blob-, és tárolja a a **out-sunset.png** fájlt.</span><span class="sxs-lookup"><span data-stu-id="2af89-135">The following example demonstrates using **get\_blob\_to\_path** to download the contents of the **myblob** blob and store it to the **out-sunset.png** file.</span></span>

```python
block_blob_service.get_blob_to_path('mycontainer', 'myblockblob', 'out-sunset.png')
```

## <a name="delete-a-blob"></a><span data-ttu-id="2af89-136">Blob törlése</span><span class="sxs-lookup"><span data-stu-id="2af89-136">Delete a blob</span></span>
<span data-ttu-id="2af89-137">Végezetül blob törléséhez hívja meg a **delete_blob**.</span><span class="sxs-lookup"><span data-stu-id="2af89-137">Finally, to delete a blob, call **delete_blob**.</span></span>

```python
block_blob_service.delete_blob('mycontainer', 'myblockblob')
```

## <a name="writing-to-an-append-blob"></a><span data-ttu-id="2af89-138">Írás hozzáfűző blobba</span><span class="sxs-lookup"><span data-stu-id="2af89-138">Writing to an append blob</span></span>
<span data-ttu-id="2af89-139">A hozzáfűző blob hozzáfűzési feladatokra, például naplózásra van optimalizálva.</span><span class="sxs-lookup"><span data-stu-id="2af89-139">An append blob is optimized for append operations, such as logging.</span></span> <span data-ttu-id="2af89-140">A blokkblobhoz hasonlóan a hozzáfűző blobot is blokkok alkotják, de ha egy hozzáfűző blobhoz új blokkot ad hozzá, az mindig a blob végéhez lesz hozzáfűzve.</span><span class="sxs-lookup"><span data-stu-id="2af89-140">Like a block blob, an append blob is comprised of blocks, but when you add a new block to an append blob, it is always appended to the end of the blob.</span></span> <span data-ttu-id="2af89-141">Hozzáfűző blob meglévő blokkja nem frissíthető és nem törölhető.</span><span class="sxs-lookup"><span data-stu-id="2af89-141">You cannot update or delete an existing block in an append blob.</span></span> <span data-ttu-id="2af89-142">A hozzáfűző blob blokkazonosítói nincsenek közzétéve, mert azok egy blokkblobhoz tartoznak.</span><span class="sxs-lookup"><span data-stu-id="2af89-142">The block IDs for an append blob are not exposed as they are for a block blob.</span></span>

<span data-ttu-id="2af89-143">Egy hozzáfűző blob minden blokkja különböző méretű lehet – 4 MB maximális értékig –, egy hozzáfűző blob pedig legfeljebb 50 000 blokkot tartalmazhat.</span><span class="sxs-lookup"><span data-stu-id="2af89-143">Each block in an append blob can be a different size, up to a maximum of 4 MB, and an append blob can include a maximum of 50,000 blocks.</span></span> <span data-ttu-id="2af89-144">A hozzáfűző blob maximális mérete ezért valamivel több mint 195 GB (4 MB X 50 000 blokk).</span><span class="sxs-lookup"><span data-stu-id="2af89-144">The maximum size of an append blob is therefore slightly more than 195 GB (4 MB X 50,000 blocks).</span></span>

<span data-ttu-id="2af89-145">Az alábbi példa új hozzáfűző blob létrehozását és adatok hozzáfűzését mutatja be, egy egyszerű naplózási műveletet szimulálva.</span><span class="sxs-lookup"><span data-stu-id="2af89-145">The example below creates a new append blob and appends some data to it, simulating a simple logging operation.</span></span>

```python
from azure.storage.blob import AppendBlobService
append_blob_service = AppendBlobService(account_name='myaccount', account_key='mykey')

# The same containers can hold all types of blobs
append_blob_service.create_container('mycontainer')

# Append blobs must be created before they are appended to
append_blob_service.create_blob('mycontainer', 'myappendblob')
append_blob_service.append_blob_from_text('mycontainer', 'myappendblob', u'Hello, world!')

append_blob = append_blob_service.get_blob_to_text('mycontainer', 'myappendblob')
```

## <a name="next-steps"></a><span data-ttu-id="2af89-146">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="2af89-146">Next steps</span></span>
<span data-ttu-id="2af89-147">Most, hogy megismerte a Blob Storage alapjait, az alábbi hivatkozásokat követve tudhat meg többet.</span><span class="sxs-lookup"><span data-stu-id="2af89-147">Now that you've learned the basics of Blob storage, follow these links to learn more.</span></span>

* [<span data-ttu-id="2af89-148">Python fejlesztői központ</span><span class="sxs-lookup"><span data-stu-id="2af89-148">Python Developer Center</span></span>](https://azure.microsoft.com/develop/python/)
* [<span data-ttu-id="2af89-149">Az Azure Storage-szolgáltatások REST API-ja</span><span class="sxs-lookup"><span data-stu-id="2af89-149">Azure Storage Services REST API</span></span>](http://msdn.microsoft.com/library/azure/dd179355)
* <span data-ttu-id="2af89-150">[Az Azure Storage csapat blogja]</span><span class="sxs-lookup"><span data-stu-id="2af89-150">[Azure Storage Team Blog]</span></span>
* <span data-ttu-id="2af89-151">[Microsoft Azure Storage SDK for Python]</span><span class="sxs-lookup"><span data-stu-id="2af89-151">[Microsoft Azure Storage SDK for Python]</span></span>

<span data-ttu-id="2af89-152">[Az Azure Storage csapat blogja]: http://blogs.msdn.com/b/windowsazurestorage/</span><span class="sxs-lookup"><span data-stu-id="2af89-152">[Azure Storage Team Blog]: http://blogs.msdn.com/b/windowsazurestorage/</span></span>
<span data-ttu-id="2af89-153">[Microsoft Azure Storage SDK for Python]: https://github.com/Azure/azure-storage-python</span><span class="sxs-lookup"><span data-stu-id="2af89-153">[Microsoft Azure Storage SDK for Python]: https://github.com/Azure/azure-storage-python</span></span>
