---
title: "A StorSimple-kötet tárolók kezelése |} Microsoft Docs"
description: "Ismerteti, hogyan használhatja a StorSimple Manager szolgáltatás kötet tárolók lap hozzáadása, módosítása vagy törlése egy kötettárolót."
services: storsimple
documentationcenter: NA
author: SharS
manager: carmonm
editor: 
ms.assetid: 1c64ce75-1fd3-4d3b-9304-d4dc0fc2b069
ms.service: storsimple
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: TBD
ms.date: 05/24/2016
ms.author: v-sharos
ms.openlocfilehash: bb55a7a4bff0fd4319de6f6ce958686ad8a4142b
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="use-the-storsimple-manager-service-to-manage-storsimple-volume-containers"></a><span data-ttu-id="af414-103">A StorSimple Manager szolgáltatással a StorSimple-kötet tárolók kezelése</span><span class="sxs-lookup"><span data-stu-id="af414-103">Use the StorSimple Manager service to manage StorSimple volume containers</span></span>
## <a name="overview"></a><span data-ttu-id="af414-104">Áttekintés</span><span class="sxs-lookup"><span data-stu-id="af414-104">Overview</span></span>
<span data-ttu-id="af414-105">Ez az oktatóanyag ismerteti, hogyan hozhatja létre és kezelheti a StorSimple-kötet tárolók a StorSimple Manager szolgáltatás.</span><span class="sxs-lookup"><span data-stu-id="af414-105">This tutorial explains how to use the StorSimple Manager service to create and manage StorSimple volume containers.</span></span>

<span data-ttu-id="af414-106">A Microsoft Azure StorSimple eszköz a kötettároló tárfiók, a titkosítás és a sávszélesség-használat beállításokat használó egy vagy több kötetet tartalmazza.</span><span class="sxs-lookup"><span data-stu-id="af414-106">A volume container in a Microsoft Azure StorSimple device contains one or more volumes that share storage account, encryption, and bandwidth consumption settings.</span></span> <span data-ttu-id="af414-107">Egy eszköz több kötet tároló az összes kötet lehet.</span><span class="sxs-lookup"><span data-stu-id="af414-107">A device can have multiple volume containers for all its volumes.</span></span> 

<span data-ttu-id="af414-108">A kötettároló a következő attribútumokat:</span><span class="sxs-lookup"><span data-stu-id="af414-108">A volume container has the following attributes:</span></span>

* <span data-ttu-id="af414-109">**Kötetek** – a rétegzett, vagy helyileg rögzített kötet tárolóban található StorSimple-köteteket.</span><span class="sxs-lookup"><span data-stu-id="af414-109">**Volumes** – The tiered or locally pinned StorSimple volumes that are contained within the volume container.</span></span> <span data-ttu-id="af414-110">A kötettároló tartalmazhat 256 StorSimple-köteteket.</span><span class="sxs-lookup"><span data-stu-id="af414-110">A volume container may contain up to 256 StorSimple volumes.</span></span>
* <span data-ttu-id="af414-111">**Titkosítási** – titkosítási kulcsot, amelyek az egyes mennyiségi tároló.</span><span class="sxs-lookup"><span data-stu-id="af414-111">**Encryption** – An encryption key that can be defined for each volume container.</span></span> <span data-ttu-id="af414-112">Ezt a kulcsot a saját StorSimple eszközön a felhőbe küldött adatok titkosításához használt.</span><span class="sxs-lookup"><span data-stu-id="af414-112">This key is used for encrypting the data that is sent from your StorSimple device to the cloud.</span></span> <span data-ttu-id="af414-113">Egy katonai szintű AES-256 bites kulcsot használ a felhasználó által megadott kulccsal.</span><span class="sxs-lookup"><span data-stu-id="af414-113">A military-grade AES-256 bit key is used with the user-entered key.</span></span> <span data-ttu-id="af414-114">Az adatok védelme érdekében javasoljuk, hogy mindig engedélyezze felhőalapú tárolás titkosításának.</span><span class="sxs-lookup"><span data-stu-id="af414-114">To secure your data, we recommend that you always enable cloud storage encryption.</span></span>
* <span data-ttu-id="af414-115">**A tárfiók** – a tárfiókot, amely csatolva van a tároló felhőszolgáltatást.</span><span class="sxs-lookup"><span data-stu-id="af414-115">**Storage account** – The storage account that is linked to your cloud storage service provider.</span></span> <span data-ttu-id="af414-116">A kötettároló szereplő összes kötet ossza meg ezt a tárfiókot.</span><span class="sxs-lookup"><span data-stu-id="af414-116">All the volumes residing in a volume container share this storage account.</span></span> <span data-ttu-id="af414-117">A storage-fiók közül választhat egy meglévő listájához, vagy hozzon létre egy új fiókot, a kötettároló létrehozása, és adja meg a fiókhoz tartozó hozzáférési hitelesítő adatokat.</span><span class="sxs-lookup"><span data-stu-id="af414-117">You can choose a storage account from an existing list, or create a new account when you create the volume container and then specify the access credentials for that account.</span></span>
* <span data-ttu-id="af414-118">**A felhő sávszélesség** – amikor az adatok az eszközről a felhőbe küldi el az eszköz által felhasznált sávszélesség.</span><span class="sxs-lookup"><span data-stu-id="af414-118">**Cloud bandwidth** – The bandwidth consumed by the device when the data from the device is being sent to the cloud.</span></span> <span data-ttu-id="af414-119">1 és 1000 MB/s ha ez a tároló közötti érték megadásával kényszerítheti a sávszélesség-vezérlést.</span><span class="sxs-lookup"><span data-stu-id="af414-119">You can enforce a bandwidth control by specifying a value between 1 and 1000 Mbps when you define this container.</span></span> <span data-ttu-id="af414-120">Ha azt szeretné, hogy az eszköz összes rendelkezésre álló sávszélességet, állítsa be ezt a mezőt a korlátlan.</span><span class="sxs-lookup"><span data-stu-id="af414-120">If you want the device to consume all available bandwidth, set this field to Unlimited.</span></span> <span data-ttu-id="af414-121">Hozhat létre, és a ütemezés szerint sávszélesség sávszélesség sablon alkalmazása.</span><span class="sxs-lookup"><span data-stu-id="af414-121">You can also create and apply a bandwidth template to allocate bandwidth based on schedule.</span></span>

![Kötet tárolók lap](./media/storsimple-manage-volume-containers/HCS_VolumeContainersPage.png)

<span data-ttu-id="af414-123">A következő eljárások azt ismertetik, hogyan használható a StorSimple **kötettárolók** lap a következő gyakori műveletek végrehajtásához:</span><span class="sxs-lookup"><span data-stu-id="af414-123">This following procedures explain how to use the StorSimple **Volume containers** page to complete the following common operations:</span></span>

* <span data-ttu-id="af414-124">A kötettároló hozzáadása</span><span class="sxs-lookup"><span data-stu-id="af414-124">Add a volume container</span></span> 
* <span data-ttu-id="af414-125">A kötettároló módosítása</span><span class="sxs-lookup"><span data-stu-id="af414-125">Modify a volume container</span></span> 
* <span data-ttu-id="af414-126">A kötettároló törlése</span><span class="sxs-lookup"><span data-stu-id="af414-126">Delete a volume container</span></span> 

## <a name="add-a-volume-container"></a><span data-ttu-id="af414-127">A kötettároló hozzáadása</span><span class="sxs-lookup"><span data-stu-id="af414-127">Add a volume container</span></span>
<span data-ttu-id="af414-128">A következő lépésekkel kötettároló hozzáadása.</span><span class="sxs-lookup"><span data-stu-id="af414-128">Perform the following steps to add a volume container.</span></span>

[!INCLUDE [storsimple-add-volume-container](../../includes/storsimple-add-volume-container.md)]

## <a name="modify-a-volume-container"></a><span data-ttu-id="af414-129">A kötettároló módosítása</span><span class="sxs-lookup"><span data-stu-id="af414-129">Modify a volume container</span></span>
<span data-ttu-id="af414-130">A következő lépésekkel módosíthatja egy kötettárolót.</span><span class="sxs-lookup"><span data-stu-id="af414-130">Perform the following steps to modify a volume container.</span></span>

[!INCLUDE [storsimple-modify-volume-container](../../includes/storsimple-modify-volume-container.md)]

## <a name="delete-a-volume-container"></a><span data-ttu-id="af414-131">A kötettároló törlése</span><span class="sxs-lookup"><span data-stu-id="af414-131">Delete a volume container</span></span>
<span data-ttu-id="af414-132">A kötettároló kötetek belül.</span><span class="sxs-lookup"><span data-stu-id="af414-132">A volume container has volumes within it.</span></span> <span data-ttu-id="af414-133">Csak akkor, ha minden benne tárolt kötet először el kell hagyni törölhetők.</span><span class="sxs-lookup"><span data-stu-id="af414-133">It can be deleted only if all the volumes contained in it are first deleted.</span></span> <span data-ttu-id="af414-134">A következő lépésekkel kötettároló törlése.</span><span class="sxs-lookup"><span data-stu-id="af414-134">Perform the following steps to delete a volume container.</span></span>

[!INCLUDE [storsimple-delete-volume-container](../../includes/storsimple-delete-volume-container.md)]

## <a name="next-steps"></a><span data-ttu-id="af414-135">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="af414-135">Next steps</span></span>
* <span data-ttu-id="af414-136">További információ [felügyelete a StorSimple-köteteket](storsimple-manage-volumes.md).</span><span class="sxs-lookup"><span data-stu-id="af414-136">Learn more about [managing StorSimple volumes](storsimple-manage-volumes.md).</span></span> 
* <span data-ttu-id="af414-137">További információ [a StorSimple Manager szolgáltatás használata a StorSimple eszköz felügyeletéhez](storsimple-manager-service-administration.md).</span><span class="sxs-lookup"><span data-stu-id="af414-137">Learn more about [using the StorSimple Manager service to administer your StorSimple device](storsimple-manager-service-administration.md).</span></span>

