---
title: "aaaManage a StorSimple kötettárolók hello StorSimple 8000 series eszközön |} Microsoft Docs"
description: "Ismerteti, hogyan használhatja a hello StorSimple Device Manager szolgáltatás kötettárolók tooadd lapon, módosítana vagy törölne egy kötettárolót."
services: storsimple
documentationcenter: NA
author: alkohli
manager: timlt
editor: 
ms.assetid: 
ms.service: storsimple
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: TBD
ms.date: 07/19/2017
ms.author: alkohli
ms.openlocfilehash: 7374d4ab9aecd6280ae1d93a29f17d12d28c9362
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="use-hello-storsimple-device-manager-service-toomanage-storsimple-volume-containers"></a><span data-ttu-id="69b2c-103">Hello StorSimple Device Manager szolgáltatás toomanage StorSimple kötet tárolók használata</span><span class="sxs-lookup"><span data-stu-id="69b2c-103">Use hello StorSimple Device Manager service toomanage StorSimple volume containers</span></span>

## <a name="overview"></a><span data-ttu-id="69b2c-104">Áttekintés</span><span class="sxs-lookup"><span data-stu-id="69b2c-104">Overview</span></span>
<span data-ttu-id="69b2c-105">Ez az oktatóanyag azt ismerteti, hogyan toouse StorSimple Device Manager szolgáltatás toocreate hello és a StorSimple-kötet tárolók kezelése.</span><span class="sxs-lookup"><span data-stu-id="69b2c-105">This tutorial explains how toouse hello StorSimple Device Manager service toocreate and manage StorSimple volume containers.</span></span>

<span data-ttu-id="69b2c-106">A Microsoft Azure StorSimple eszköz a kötettároló tárfiók, a titkosítás és a sávszélesség-használat beállításokat használó egy vagy több kötetet tartalmazza.</span><span class="sxs-lookup"><span data-stu-id="69b2c-106">A volume container in a Microsoft Azure StorSimple device contains one or more volumes that share storage account, encryption, and bandwidth consumption settings.</span></span> <span data-ttu-id="69b2c-107">Egy eszköz több kötet tároló az összes kötet lehet.</span><span class="sxs-lookup"><span data-stu-id="69b2c-107">A device can have multiple volume containers for all its volumes.</span></span> 

<span data-ttu-id="69b2c-108">A kötettároló hello a következő attribútumokat:</span><span class="sxs-lookup"><span data-stu-id="69b2c-108">A volume container has hello following attributes:</span></span>

* <span data-ttu-id="69b2c-109">**Kötetek** – hello rétegzett, vagy helyileg rögzített belüli hello kötettároló StorSimple-köteteket.</span><span class="sxs-lookup"><span data-stu-id="69b2c-109">**Volumes** – hello tiered or locally pinned StorSimple volumes that are contained within hello volume container.</span></span> 
* <span data-ttu-id="69b2c-110">**Titkosítási** – titkosítási kulcsot, amelyek az egyes mennyiségi tároló.</span><span class="sxs-lookup"><span data-stu-id="69b2c-110">**Encryption** – An encryption key that can be defined for each volume container.</span></span> <span data-ttu-id="69b2c-111">Ezt a kulcsot a StorSimple eszköz toohello felhőből küldött hello adatok titkosításához használt.</span><span class="sxs-lookup"><span data-stu-id="69b2c-111">This key is used for encrypting hello data that is sent from your StorSimple device toohello cloud.</span></span> <span data-ttu-id="69b2c-112">Egy katonai szintű AES-256 bit kulccsal hello a felhasználó által megadott kulccsal.</span><span class="sxs-lookup"><span data-stu-id="69b2c-112">A military-grade AES-256 bit key is used with hello user-entered key.</span></span> <span data-ttu-id="69b2c-113">toosecure adatait, javasoljuk, hogy mindig engedélyezze felhőalapú tárolás titkosításának.</span><span class="sxs-lookup"><span data-stu-id="69b2c-113">toosecure your data, we recommend that you always enable cloud storage encryption.</span></span>
* <span data-ttu-id="69b2c-114">**A tárfiók** – hello használt toostore hello adatok Azure storage-fiók.</span><span class="sxs-lookup"><span data-stu-id="69b2c-114">**Storage account** – hello Azure storage account that is used toostore hello data.</span></span> <span data-ttu-id="69b2c-115">A kötettároló szereplő összes hello kötetek ossza meg ezt a tárfiókot.</span><span class="sxs-lookup"><span data-stu-id="69b2c-115">All hello volumes residing in a volume container share this storage account.</span></span> <span data-ttu-id="69b2c-116">A storage-fiók közül választhat egy meglévő listájához, vagy hozzon létre egy új fiókot, ha hello kötettároló létrehozása, és adja meg a fiókhoz tartozó hello hozzáférési hitelesítő adatok.</span><span class="sxs-lookup"><span data-stu-id="69b2c-116">You can choose a storage account from an existing list, or create a new account when you create hello volume container and then specify hello access credentials for that account.</span></span>
* <span data-ttu-id="69b2c-117">**A felhő sávszélesség** – hello amikor hello adatokat hello eszközről toohello felhő küldi hello eszköz által felhasznált sávszélesség.</span><span class="sxs-lookup"><span data-stu-id="69b2c-117">**Cloud bandwidth** – hello bandwidth consumed by hello device when hello data from hello device is being sent toohello cloud.</span></span> <span data-ttu-id="69b2c-118">Ez a tároló létrehozásakor 1 és 1000 közötti érték megadásával kényszerítheti a sávszélesség-vezérlést.</span><span class="sxs-lookup"><span data-stu-id="69b2c-118">You can enforce a bandwidth control by specifying a value between 1 Mbps and 1,000 Mbps when you create this container.</span></span> <span data-ttu-id="69b2c-119">Hello eszköz tooconsume a teljes elérhető sávszélesség, állítsa ezt a mezőt túl**korlátlan**.</span><span class="sxs-lookup"><span data-stu-id="69b2c-119">If you want hello device tooconsume all available bandwidth, set this field too**Unlimited**.</span></span> <span data-ttu-id="69b2c-120">Hozhat létre, és a sávszélesség sablon tooallocate sávszélesség ütemezés alapján alkalmazza.</span><span class="sxs-lookup"><span data-stu-id="69b2c-120">You can also create and apply a bandwidth template tooallocate bandwidth based on schedule.</span></span>

<span data-ttu-id="69b2c-121">hello alábbi eljárások azt ismertetik, hogyan toouse hello StorSimple **kötettárolók** panel toocomplete hello közös műveletek a következő:</span><span class="sxs-lookup"><span data-stu-id="69b2c-121">hello following procedures explain how toouse hello StorSimple **Volume containers** blade toocomplete hello following common operations:</span></span>

* <span data-ttu-id="69b2c-122">A kötettároló hozzáadása</span><span class="sxs-lookup"><span data-stu-id="69b2c-122">Add a volume container</span></span>
* <span data-ttu-id="69b2c-123">A kötettároló módosítása</span><span class="sxs-lookup"><span data-stu-id="69b2c-123">Modify a volume container</span></span>
* <span data-ttu-id="69b2c-124">A kötettároló törlése</span><span class="sxs-lookup"><span data-stu-id="69b2c-124">Delete a volume container</span></span>

## <a name="add-a-volume-container"></a><span data-ttu-id="69b2c-125">A kötettároló hozzáadása</span><span class="sxs-lookup"><span data-stu-id="69b2c-125">Add a volume container</span></span>
<span data-ttu-id="69b2c-126">Hajtsa végre a következő lépéseket tooadd kötettároló hello.</span><span class="sxs-lookup"><span data-stu-id="69b2c-126">Perform hello following steps tooadd a volume container.</span></span>

[!INCLUDE [storsimple-8000-add-volume-container](../../includes/storsimple-8000-create-volume-container.md)]

## <a name="modify-a-volume-container"></a><span data-ttu-id="69b2c-127">A kötettároló módosítása</span><span class="sxs-lookup"><span data-stu-id="69b2c-127">Modify a volume container</span></span>
<span data-ttu-id="69b2c-128">Hajtsa végre a következő lépéseket toomodify kötettároló hello.</span><span class="sxs-lookup"><span data-stu-id="69b2c-128">Perform hello following steps toomodify a volume container.</span></span>

[!INCLUDE [storsimple-8000-modify-volume-container](../../includes/storsimple-8000-modify-volume-container.md)]

## <a name="delete-a-volume-container"></a><span data-ttu-id="69b2c-129">A kötettároló törlése</span><span class="sxs-lookup"><span data-stu-id="69b2c-129">Delete a volume container</span></span>
<span data-ttu-id="69b2c-130">A kötettároló kötetek belül.</span><span class="sxs-lookup"><span data-stu-id="69b2c-130">A volume container has volumes within it.</span></span> <span data-ttu-id="69b2c-131">Csak akkor, ha először törlődnek a benne tárolt összes hello kötetek törölhetők.</span><span class="sxs-lookup"><span data-stu-id="69b2c-131">It can be deleted only if all hello volumes contained in it are first deleted.</span></span> <span data-ttu-id="69b2c-132">Hajtsa végre a következő lépéseket toodelete kötettároló hello.</span><span class="sxs-lookup"><span data-stu-id="69b2c-132">Perform hello following steps toodelete a volume container.</span></span>

[!INCLUDE [storsimple-8000-delete-volume-container](../../includes/storsimple-8000-delete-volume-container.md)]

## <a name="next-steps"></a><span data-ttu-id="69b2c-133">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="69b2c-133">Next steps</span></span>
* <span data-ttu-id="69b2c-134">További információ [felügyelete a StorSimple-köteteket](storsimple-8000-manage-volumes-u2.md).</span><span class="sxs-lookup"><span data-stu-id="69b2c-134">Learn more about [managing StorSimple volumes](storsimple-8000-manage-volumes-u2.md).</span></span> 
* <span data-ttu-id="69b2c-135">További információ [használatával hello StorSimple Device Manager szolgáltatás tooadminister a StorSimple eszköz](storsimple-8000-manager-service-administration.md).</span><span class="sxs-lookup"><span data-stu-id="69b2c-135">Learn more about [using hello StorSimple Device Manager service tooadminister your StorSimple device](storsimple-8000-manager-service-administration.md).</span></span>

