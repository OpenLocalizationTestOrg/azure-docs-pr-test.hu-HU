---
title: a Blob storage aaaIntroduction tooAzure |} Microsoft Docs
description: "A Blob storage bemutatása tooAzure"
services: storage
documentationcenter: 
author: robinsh
manager: timlt
editor: tysonn
ms.assetid: 
ms.service: storage
ms.workload: storage
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/17/2017
ms.author: robinsh
ms.openlocfilehash: 3431f826ae51d42dbced084ee60f9ff70a8168d5
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="introduction-tooblob-storage"></a><span data-ttu-id="59713-103">TooBlob storage bemutatása</span><span class="sxs-lookup"><span data-stu-id="59713-103">Introduction tooBlob storage</span></span>

<span data-ttu-id="59713-104">Az Azure Blob storage egy olyan szolgáltatás nagy mennyiségű strukturálatlan adatok, például szövegek vagy bináris adatok, hozzáfér a bárhol a HTTP vagy HTTPS PROTOKOLLON keresztül hello world tárolásához.</span><span class="sxs-lookup"><span data-stu-id="59713-104">Azure Blob storage is a service for storing large amounts of unstructured object data, such as text or binary data, that can be accessed from anywhere in hello world via HTTP or HTTPS.</span></span> <span data-ttu-id="59713-105">Használhatja a Blob storage tooexpose adatok nyilvánosan toohello globális vagy toostore alkalmazásadatok közvetlenül a Microsoftnak.</span><span class="sxs-lookup"><span data-stu-id="59713-105">You can use Blob storage tooexpose data publicly toohello world, or toostore application data privately.</span></span>

<span data-ttu-id="59713-106">A Blob Storage gyakori használati módjai többek között:</span><span class="sxs-lookup"><span data-stu-id="59713-106">Common uses of Blob storage include:</span></span>

* <span data-ttu-id="59713-107">Képek vagy dokumentumok közvetlen tooa böngésző szolgáló</span><span class="sxs-lookup"><span data-stu-id="59713-107">Serving images or documents directly tooa browser</span></span>
* <span data-ttu-id="59713-108">Fájlok tárolása megosztott hozzáférésre</span><span class="sxs-lookup"><span data-stu-id="59713-108">Storing files for distributed access</span></span>
* <span data-ttu-id="59713-109">Video- és hangtartalom online továbbítása</span><span class="sxs-lookup"><span data-stu-id="59713-109">Streaming video and audio</span></span>
* <span data-ttu-id="59713-110">Adattárolás biztonsági mentésekhez és helyreállításhoz, vészhelyreállításhoz és archiváláshoz</span><span class="sxs-lookup"><span data-stu-id="59713-110">Storing data for backup and restore, disaster recovery, and archiving</span></span>
* <span data-ttu-id="59713-111">Adattárolás egy helyszíni vagy egy Azure által üzemeltetett szolgáltatás elemzéseihez</span><span class="sxs-lookup"><span data-stu-id="59713-111">Storing data for analysis by an on-premises or Azure-hosted service</span></span>

## <a name="blob-service-concepts"></a><span data-ttu-id="59713-112">A Blob szolgáltatással kapcsolatos fogalmak</span><span class="sxs-lookup"><span data-stu-id="59713-112">Blob service concepts</span></span>

<span data-ttu-id="59713-113">Blob szolgáltatás hello hello a következő összetevőket tartalmazza:</span><span class="sxs-lookup"><span data-stu-id="59713-113">hello Blob service contains hello following components:</span></span>

![Blob-architektúra](./media/storage-blobs-introduction/blob1.png)

* <span data-ttu-id="59713-115">**Tárfiók:** összes keresztül férnek hozzá tooAzure tárolási történik egy tárfiókot.</span><span class="sxs-lookup"><span data-stu-id="59713-115">**Storage Account:** All access tooAzure Storage is done through a storage account.</span></span> <span data-ttu-id="59713-116">Ez a tárfiók lehet egy **általános célú tárfiók** vagy egy **Blob storage-fiók** , amely kifejezetten objektumok/blobok tárolására.</span><span class="sxs-lookup"><span data-stu-id="59713-116">This storage account can be a **General-purpose storage account** or a **Blob storage account** that is specialized for storing objects/blobs.</span></span> <span data-ttu-id="59713-117">További információk: [Tudnivalók az Azure Storage-fiókokról](../common/storage-create-storage-account.md?toc=%2fazure%2fstorage%2fblobs%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="59713-117">See [About Azure storage accounts](../common/storage-create-storage-account.md?toc=%2fazure%2fstorage%2fblobs%2ftoc.json) for more information.</span></span>

* <span data-ttu-id="59713-118">**Tároló:** A tárolók blobkészletek csoportosítását biztosítják.</span><span class="sxs-lookup"><span data-stu-id="59713-118">**Container:** A container provides a grouping of a set of blobs.</span></span> <span data-ttu-id="59713-119">Az összes blobnak tárolóban kell lennie.</span><span class="sxs-lookup"><span data-stu-id="59713-119">All blobs must be in a container.</span></span> <span data-ttu-id="59713-120">Egy fiók korlátlan számú tárolót tartalmazhat.</span><span class="sxs-lookup"><span data-stu-id="59713-120">An account can contain an unlimited number of containers.</span></span> <span data-ttu-id="59713-121">Egy tároló korlátlan számú blob tárolására használható.</span><span class="sxs-lookup"><span data-stu-id="59713-121">A container can store an unlimited number of blobs.</span></span> <span data-ttu-id="59713-122">Vegye figyelembe, hogy hello a tároló neve kisbetűnek kell lennie.</span><span class="sxs-lookup"><span data-stu-id="59713-122">Note that hello container name must be lowercase.</span></span>

* <span data-ttu-id="59713-123">**Blob:** Bármilyen típusú és bármekkora méretű fájl.</span><span class="sxs-lookup"><span data-stu-id="59713-123">**Blob:** A file of any type and size.</span></span> <span data-ttu-id="59713-124">Az Azure Storage háromféle blobot biztosít: blokkblobokat, lapblobokat és hozzáfűző blobokat.</span><span class="sxs-lookup"><span data-stu-id="59713-124">Azure Storage offers three types of blobs: block blobs, page blobs, and append blobs.</span></span>
  
    <span data-ttu-id="59713-125">A *blokkblobok* ideálisak szövegek és bináris fájlok, például dokumentumok és médiafájlok tárolására.</span><span class="sxs-lookup"><span data-stu-id="59713-125">*Block blobs* are ideal for storing text or binary files, such as documents and media files.</span></span> <span data-ttu-id="59713-126">*Hozzáfűző blobokat* vannak hasonló tooblock blobok abban a épülnek fel blokkok, de vannak optimalizálva műveletek hozzáfűzésére, ezért hasznosak lehetnek a naplózási forgatókönyvekben.</span><span class="sxs-lookup"><span data-stu-id="59713-126">*Append blobs* are similar tooblock blobs in that they are made up of blocks, but they are optimized for append operations, so they are useful for logging scenarios.</span></span> <span data-ttu-id="59713-127">Egyetlen blokkblob tartalmazhat be too50, 000 blokkok too100 MB minden, a maximális méretük ezért valamivel több mint 4.75 TB-os (100 MB X 50 000).</span><span class="sxs-lookup"><span data-stu-id="59713-127">A single block blob can contain up too50,000 blocks of up too100 MB each, for a total size of slightly more than 4.75 TB (100 MB X 50,000).</span></span> <span data-ttu-id="59713-128">Egy hozzáfűző blob tartalmazhat be too50, 000 blokkok too4 MB minden, az a maximális méretük ezért valamivel több mint 195 GB (4 MB X 50 000).</span><span class="sxs-lookup"><span data-stu-id="59713-128">A single append blob can contain up too50,000 blocks of up too4 MB each, for a total size of slightly more than 195 GB (4 MB X 50,000).</span></span>
  
    <span data-ttu-id="59713-129">*Lapblobok* mentése too1 TB méretű is lehet, és hatékonyabbak a gyakori olvasási/írási műveletek.</span><span class="sxs-lookup"><span data-stu-id="59713-129">*Page blobs* can be up too1 TB in size, and are more efficient for frequent read/write operations.</span></span> <span data-ttu-id="59713-130">Az Azure virtuális gépek lapblobokat operációsrendszer- és adatlemezek használja.</span><span class="sxs-lookup"><span data-stu-id="59713-130">Azure Virtual Machines uses page blobs as OS and data disks.</span></span>
  
    <span data-ttu-id="59713-131">A tárolók és blobok elnevezésével kapcsolatos részletekért lásd: [Naming and Referencing Containers, Blobs, and Metadata](/rest/api/storageservices/Naming-and-Referencing-Containers--Blobs--and-Metadata) (Tárolók, blobok és metaadatok elnevezése és hivatkozása).</span><span class="sxs-lookup"><span data-stu-id="59713-131">For details about naming containers and blobs, see [Naming and Referencing Containers, Blobs, and Metadata](/rest/api/storageservices/Naming-and-Referencing-Containers--Blobs--and-Metadata).</span></span>

## <a name="next-steps"></a><span data-ttu-id="59713-132">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="59713-132">Next steps</span></span>

* [<span data-ttu-id="59713-133">Tárfiók létrehozása</span><span class="sxs-lookup"><span data-stu-id="59713-133">Create a storage account</span></span>](../common/storage-create-storage-account.md?toc=%2fazure%2fstorage%2fblobs%2ftoc.json)
* [<span data-ttu-id="59713-134">Ismerkedés a Blob storage .NET használatával</span><span class="sxs-lookup"><span data-stu-id="59713-134">Getting started with Blob storage using .NET</span></span>](storage-dotnet-how-to-use-blobs.md)