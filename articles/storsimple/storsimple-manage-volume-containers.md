---
title: "aaaManage a StorSimple kötettárolók |} Microsoft Docs"
description: "Ismerteti, hogyan használhatja a StorSimple Manager szolgáltatás kötettárolók tooadd lapon, módosítana vagy törölne egy kötettárolót hello."
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
ms.openlocfilehash: 9b29536e0072306e53ac92bacca78a13d932c2b1
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="use-hello-storsimple-manager-service-toomanage-storsimple-volume-containers"></a><span data-ttu-id="7ba87-103">Hello StorSimple Manager szolgáltatás toomanage StorSimple kötet tárolók használata</span><span class="sxs-lookup"><span data-stu-id="7ba87-103">Use hello StorSimple Manager service toomanage StorSimple volume containers</span></span>
## <a name="overview"></a><span data-ttu-id="7ba87-104">Áttekintés</span><span class="sxs-lookup"><span data-stu-id="7ba87-104">Overview</span></span>
<span data-ttu-id="7ba87-105">Ez az oktatóanyag azt ismerteti, hogyan toouse StorSimple Manager szolgáltatás toocreate hello és a StorSimple-kötet tárolók kezelése.</span><span class="sxs-lookup"><span data-stu-id="7ba87-105">This tutorial explains how toouse hello StorSimple Manager service toocreate and manage StorSimple volume containers.</span></span>

<span data-ttu-id="7ba87-106">A Microsoft Azure StorSimple eszköz a kötettároló tárfiók, a titkosítás és a sávszélesség-használat beállításokat használó egy vagy több kötetet tartalmazza.</span><span class="sxs-lookup"><span data-stu-id="7ba87-106">A volume container in a Microsoft Azure StorSimple device contains one or more volumes that share storage account, encryption, and bandwidth consumption settings.</span></span> <span data-ttu-id="7ba87-107">Egy eszköz több kötet tároló az összes kötet lehet.</span><span class="sxs-lookup"><span data-stu-id="7ba87-107">A device can have multiple volume containers for all its volumes.</span></span> 

<span data-ttu-id="7ba87-108">A kötettároló hello a következő attribútumokat:</span><span class="sxs-lookup"><span data-stu-id="7ba87-108">A volume container has hello following attributes:</span></span>

* <span data-ttu-id="7ba87-109">**Kötetek** – hello rétegzett, vagy helyileg rögzített belüli hello kötettároló StorSimple-köteteket.</span><span class="sxs-lookup"><span data-stu-id="7ba87-109">**Volumes** – hello tiered or locally pinned StorSimple volumes that are contained within hello volume container.</span></span> <span data-ttu-id="7ba87-110">A kötettároló tartalmazhat be too256 StorSimple-köteteket.</span><span class="sxs-lookup"><span data-stu-id="7ba87-110">A volume container may contain up too256 StorSimple volumes.</span></span>
* <span data-ttu-id="7ba87-111">**Titkosítási** – titkosítási kulcsot, amelyek az egyes mennyiségi tároló.</span><span class="sxs-lookup"><span data-stu-id="7ba87-111">**Encryption** – An encryption key that can be defined for each volume container.</span></span> <span data-ttu-id="7ba87-112">Ezt a kulcsot a StorSimple eszköz toohello felhőből küldött hello adatok titkosításához használt.</span><span class="sxs-lookup"><span data-stu-id="7ba87-112">This key is used for encrypting hello data that is sent from your StorSimple device toohello cloud.</span></span> <span data-ttu-id="7ba87-113">Egy katonai szintű AES-256 bit kulccsal hello a felhasználó által megadott kulccsal.</span><span class="sxs-lookup"><span data-stu-id="7ba87-113">A military-grade AES-256 bit key is used with hello user-entered key.</span></span> <span data-ttu-id="7ba87-114">toosecure adatait, javasoljuk, hogy mindig engedélyezze felhőalapú tárolás titkosításának.</span><span class="sxs-lookup"><span data-stu-id="7ba87-114">toosecure your data, we recommend that you always enable cloud storage encryption.</span></span>
* <span data-ttu-id="7ba87-115">**A tárfiók** – hello tárfiókja, amely csatolt tooyour felhőalapú tárolási szolgáltató.</span><span class="sxs-lookup"><span data-stu-id="7ba87-115">**Storage account** – hello storage account that is linked tooyour cloud storage service provider.</span></span> <span data-ttu-id="7ba87-116">A kötettároló szereplő összes hello kötetek ossza meg ezt a tárfiókot.</span><span class="sxs-lookup"><span data-stu-id="7ba87-116">All hello volumes residing in a volume container share this storage account.</span></span> <span data-ttu-id="7ba87-117">A storage-fiók közül választhat egy meglévő listájához, vagy hozzon létre egy új fiókot, ha hello kötettároló létrehozása, és adja meg a fiókhoz tartozó hello hozzáférési hitelesítő adatok.</span><span class="sxs-lookup"><span data-stu-id="7ba87-117">You can choose a storage account from an existing list, or create a new account when you create hello volume container and then specify hello access credentials for that account.</span></span>
* <span data-ttu-id="7ba87-118">**A felhő sávszélesség** – hello amikor hello adatokat hello eszközről toohello felhő küldi hello eszköz által felhasznált sávszélesség.</span><span class="sxs-lookup"><span data-stu-id="7ba87-118">**Cloud bandwidth** – hello bandwidth consumed by hello device when hello data from hello device is being sent toohello cloud.</span></span> <span data-ttu-id="7ba87-119">1 és 1000 MB/s ha ez a tároló közötti érték megadásával kényszerítheti a sávszélesség-vezérlést.</span><span class="sxs-lookup"><span data-stu-id="7ba87-119">You can enforce a bandwidth control by specifying a value between 1 and 1000 Mbps when you define this container.</span></span> <span data-ttu-id="7ba87-120">Hello eszköz tooconsume a teljes elérhető sávszélesség, állítsa a mező tooUnlimited.</span><span class="sxs-lookup"><span data-stu-id="7ba87-120">If you want hello device tooconsume all available bandwidth, set this field tooUnlimited.</span></span> <span data-ttu-id="7ba87-121">Hozhat létre, és a sávszélesség sablon tooallocate sávszélesség ütemezés alapján alkalmazza.</span><span class="sxs-lookup"><span data-stu-id="7ba87-121">You can also create and apply a bandwidth template tooallocate bandwidth based on schedule.</span></span>

![Kötet tárolók lap](./media/storsimple-manage-volume-containers/HCS_VolumeContainersPage.png)

<span data-ttu-id="7ba87-123">A következő eljárások azt ismertetik, hogyan toouse hello StorSimple **kötettárolók** lap toocomplete hello közös műveletek a következő:</span><span class="sxs-lookup"><span data-stu-id="7ba87-123">This following procedures explain how toouse hello StorSimple **Volume containers** page toocomplete hello following common operations:</span></span>

* <span data-ttu-id="7ba87-124">A kötettároló hozzáadása</span><span class="sxs-lookup"><span data-stu-id="7ba87-124">Add a volume container</span></span> 
* <span data-ttu-id="7ba87-125">A kötettároló módosítása</span><span class="sxs-lookup"><span data-stu-id="7ba87-125">Modify a volume container</span></span> 
* <span data-ttu-id="7ba87-126">A kötettároló törlése</span><span class="sxs-lookup"><span data-stu-id="7ba87-126">Delete a volume container</span></span> 

## <a name="add-a-volume-container"></a><span data-ttu-id="7ba87-127">A kötettároló hozzáadása</span><span class="sxs-lookup"><span data-stu-id="7ba87-127">Add a volume container</span></span>
<span data-ttu-id="7ba87-128">Hajtsa végre a következő lépéseket tooadd kötettároló hello.</span><span class="sxs-lookup"><span data-stu-id="7ba87-128">Perform hello following steps tooadd a volume container.</span></span>

[!INCLUDE [storsimple-add-volume-container](../../includes/storsimple-add-volume-container.md)]

## <a name="modify-a-volume-container"></a><span data-ttu-id="7ba87-129">A kötettároló módosítása</span><span class="sxs-lookup"><span data-stu-id="7ba87-129">Modify a volume container</span></span>
<span data-ttu-id="7ba87-130">Hajtsa végre a következő lépéseket toomodify kötettároló hello.</span><span class="sxs-lookup"><span data-stu-id="7ba87-130">Perform hello following steps toomodify a volume container.</span></span>

[!INCLUDE [storsimple-modify-volume-container](../../includes/storsimple-modify-volume-container.md)]

## <a name="delete-a-volume-container"></a><span data-ttu-id="7ba87-131">A kötettároló törlése</span><span class="sxs-lookup"><span data-stu-id="7ba87-131">Delete a volume container</span></span>
<span data-ttu-id="7ba87-132">A kötettároló kötetek belül.</span><span class="sxs-lookup"><span data-stu-id="7ba87-132">A volume container has volumes within it.</span></span> <span data-ttu-id="7ba87-133">Csak akkor, ha először törlődnek a benne tárolt összes hello kötetek törölhetők.</span><span class="sxs-lookup"><span data-stu-id="7ba87-133">It can be deleted only if all hello volumes contained in it are first deleted.</span></span> <span data-ttu-id="7ba87-134">Hajtsa végre a következő lépéseket toodelete kötettároló hello.</span><span class="sxs-lookup"><span data-stu-id="7ba87-134">Perform hello following steps toodelete a volume container.</span></span>

[!INCLUDE [storsimple-delete-volume-container](../../includes/storsimple-delete-volume-container.md)]

## <a name="next-steps"></a><span data-ttu-id="7ba87-135">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="7ba87-135">Next steps</span></span>
* <span data-ttu-id="7ba87-136">További információ [felügyelete a StorSimple-köteteket](storsimple-manage-volumes.md).</span><span class="sxs-lookup"><span data-stu-id="7ba87-136">Learn more about [managing StorSimple volumes](storsimple-manage-volumes.md).</span></span> 
* <span data-ttu-id="7ba87-137">További információ [használatával hello StorSimple Manager szolgáltatás tooadminister a StorSimple eszköz](storsimple-manager-service-administration.md).</span><span class="sxs-lookup"><span data-stu-id="7ba87-137">Learn more about [using hello StorSimple Manager service tooadminister your StorSimple device](storsimple-manager-service-administration.md).</span></span>

