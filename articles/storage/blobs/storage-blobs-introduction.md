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
# <a name="introduction-tooblob-storage"></a>TooBlob storage bemutatása

Az Azure Blob storage egy olyan szolgáltatás nagy mennyiségű strukturálatlan adatok, például szövegek vagy bináris adatok, hozzáfér a bárhol a HTTP vagy HTTPS PROTOKOLLON keresztül hello world tárolásához. Használhatja a Blob storage tooexpose adatok nyilvánosan toohello globális vagy toostore alkalmazásadatok közvetlenül a Microsoftnak.

A Blob Storage gyakori használati módjai többek között:

* Képek vagy dokumentumok közvetlen tooa böngésző szolgáló
* Fájlok tárolása megosztott hozzáférésre
* Video- és hangtartalom online továbbítása
* Adattárolás biztonsági mentésekhez és helyreállításhoz, vészhelyreállításhoz és archiváláshoz
* Adattárolás egy helyszíni vagy egy Azure által üzemeltetett szolgáltatás elemzéseihez

## <a name="blob-service-concepts"></a>A Blob szolgáltatással kapcsolatos fogalmak

Blob szolgáltatás hello hello a következő összetevőket tartalmazza:

![Blob-architektúra](./media/storage-blobs-introduction/blob1.png)

* **Tárfiók:** összes keresztül férnek hozzá tooAzure tárolási történik egy tárfiókot. Ez a tárfiók lehet egy **általános célú tárfiók** vagy egy **Blob storage-fiók** , amely kifejezetten objektumok/blobok tárolására. További információk: [Tudnivalók az Azure Storage-fiókokról](../common/storage-create-storage-account.md?toc=%2fazure%2fstorage%2fblobs%2ftoc.json).

* **Tároló:** A tárolók blobkészletek csoportosítását biztosítják. Az összes blobnak tárolóban kell lennie. Egy fiók korlátlan számú tárolót tartalmazhat. Egy tároló korlátlan számú blob tárolására használható. Vegye figyelembe, hogy hello a tároló neve kisbetűnek kell lennie.

* **Blob:** Bármilyen típusú és bármekkora méretű fájl. Az Azure Storage háromféle blobot biztosít: blokkblobokat, lapblobokat és hozzáfűző blobokat.
  
    A *blokkblobok* ideálisak szövegek és bináris fájlok, például dokumentumok és médiafájlok tárolására. *Hozzáfűző blobokat* vannak hasonló tooblock blobok abban a épülnek fel blokkok, de vannak optimalizálva műveletek hozzáfűzésére, ezért hasznosak lehetnek a naplózási forgatókönyvekben. Egyetlen blokkblob tartalmazhat be too50, 000 blokkok too100 MB minden, a maximális méretük ezért valamivel több mint 4.75 TB-os (100 MB X 50 000). Egy hozzáfűző blob tartalmazhat be too50, 000 blokkok too4 MB minden, az a maximális méretük ezért valamivel több mint 195 GB (4 MB X 50 000).
  
    *Lapblobok* mentése too1 TB méretű is lehet, és hatékonyabbak a gyakori olvasási/írási műveletek. Az Azure virtuális gépek lapblobokat operációsrendszer- és adatlemezek használja.
  
    A tárolók és blobok elnevezésével kapcsolatos részletekért lásd: [Naming and Referencing Containers, Blobs, and Metadata](/rest/api/storageservices/Naming-and-Referencing-Containers--Blobs--and-Metadata) (Tárolók, blobok és metaadatok elnevezése és hivatkozása).

## <a name="next-steps"></a>Következő lépések

* [Tárfiók létrehozása](../common/storage-create-storage-account.md?toc=%2fazure%2fstorage%2fblobs%2ftoc.json)
* [Ismerkedés a Blob storage .NET használatával](storage-dotnet-how-to-use-blobs.md)