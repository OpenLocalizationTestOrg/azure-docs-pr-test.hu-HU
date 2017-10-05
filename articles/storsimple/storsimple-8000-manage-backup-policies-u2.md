---
title: "A StorSimple 8000 series biztonsági mentési házirendek kezelése |} Microsoft Docs"
description: "Ismerteti, hogyan használhatja a StorSimple Device Manager szolgáltatás létrehozásához és kezeléséhez manuális biztonsági mentések, a biztonsági mentési ütemezés és a biztonsági másolatok megőrzésének a StorSimple 8000 series eszközön."
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
ms.date: 07/05/2017
ms.author: alkohli
ms.openlocfilehash: 569dbfdeb7dcd526cb5a54b487ea1bfb59b13cc6
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="use-the-storsimple-device-manager-service-in-azure-portal-to-manage-backup-policies"></a><span data-ttu-id="4f1ac-103">A StorSimple Device Manager szolgáltatás használata Azure-portálon kezelheti a biztonsági mentési házirendek</span><span class="sxs-lookup"><span data-stu-id="4f1ac-103">Use the StorSimple Device Manager service in Azure portal to manage backup policies</span></span>


## <a name="overview"></a><span data-ttu-id="4f1ac-104">Áttekintés</span><span class="sxs-lookup"><span data-stu-id="4f1ac-104">Overview</span></span>

<span data-ttu-id="4f1ac-105">Ez az oktatóanyag azt ismerteti, hogyan használhatja a StorSimple Device Manager szolgáltatást **biztonsági mentési házirend** panel biztonsági mentési folyamatokat és a StorSimple-köteteket a biztonsági másolatok megőrzésének szabályozására.</span><span class="sxs-lookup"><span data-stu-id="4f1ac-105">This tutorial explains how to use the StorSimple Device Manager service **Backup policy** blade to control backup processes and backup retention for your StorSimple volumes.</span></span> <span data-ttu-id="4f1ac-106">Azt is bemutatja, hogyan hajthatja végre manuális biztonsági mentés.</span><span class="sxs-lookup"><span data-stu-id="4f1ac-106">It also describes how to complete a manual backup.</span></span>

<span data-ttu-id="4f1ac-107">Biztonsági mentését egy kötetet, ha szeretné, hozzon létre egy helyi pillanatfelvétel vagy egy felhőalapú pillanatfelvétel.</span><span class="sxs-lookup"><span data-stu-id="4f1ac-107">When you back up a volume, you can choose to create a local snapshot or a cloud snapshot.</span></span> <span data-ttu-id="4f1ac-108">Ha biztonsági mentést egy helyileg rögzített kötet, javasoljuk, meg kell adnia egy felhőalapú pillanatfelvétel.</span><span class="sxs-lookup"><span data-stu-id="4f1ac-108">If you are backing up a locally pinned volume, we recommend that you specify a cloud snapshot.</span></span> <span data-ttu-id="4f1ac-109">Helyi pillanatképek készítése, amely rendelkezik a nagy mennyiségű forgalom adatkészlet alapján kialakulhat egy helyileg rögzített kötet nagy mennyiségű véve azt eredményezi, hogy olyan helyzet, amelyben gyorsan futtathatja helyi elfogyott.</span><span class="sxs-lookup"><span data-stu-id="4f1ac-109">Taking a large number of local snapshots of a locally pinned volume coupled with a data set that has a lot of churn will result in a situation in which you could rapidly run out of local space.</span></span> <span data-ttu-id="4f1ac-110">Ha helyi pillanatképek választja, azt javasoljuk, hogy a legutóbbi állapota, készítsen biztonsági másolatot a megőrizni kívánt őket egy napon kevesebb napi pillanatfelvételeket, és törölje őket.</span><span class="sxs-lookup"><span data-stu-id="4f1ac-110">If you choose to take local snapshots, we recommend that you take fewer daily snapshots to back up the most recent state, retain them for a day, and then delete them.</span></span>

<span data-ttu-id="4f1ac-111">Amikor a pillanatképet egy felhőalapú, egy helyileg rögzített kötetet, csak a megváltozott adatok másolása a felhőbe, amennyiben deduplikált és tömörített.</span><span class="sxs-lookup"><span data-stu-id="4f1ac-111">When you take a cloud snapshot of a locally pinned volume, you copy only the changed data to the cloud, where it is deduplicated and compressed.</span></span>

## <a name="the-backup-policy-blade"></a><span data-ttu-id="4f1ac-112">A biztonsági mentési házirend panel</span><span class="sxs-lookup"><span data-stu-id="4f1ac-112">The Backup policy blade</span></span>

<span data-ttu-id="4f1ac-113">A **biztonsági mentési házirend** a StorSimple eszköz paneljén lehetővé teszi a biztonsági mentési házirendek kezelése és ütemezése helyi és felhőalapú pillanatfelvételek.</span><span class="sxs-lookup"><span data-stu-id="4f1ac-113">The **Backup policy** blade for your StorSimple device allows you to manage backup policies and schedule local and cloud snapshots.</span></span> <span data-ttu-id="4f1ac-114">Biztonsági mentési házirendek segítségével konfigurálhatja a biztonsági mentési ütemezés és a biztonsági másolatok megőrzésének kötetek gyűjteményét.</span><span class="sxs-lookup"><span data-stu-id="4f1ac-114">Backup policies are used to configure backup schedules and backup retention for a collection of volumes.</span></span> <span data-ttu-id="4f1ac-115">Biztonsági mentési házirendek lehetővé teszik a pillanatkép készítése több kötet egy időben.</span><span class="sxs-lookup"><span data-stu-id="4f1ac-115">Backup policies enable you to take a snapshot of multiple volumes simultaneously.</span></span> <span data-ttu-id="4f1ac-116">Ez azt jelenti, hogy a biztonsági mentési házirend által létrehozott biztonsági mentéseket kell-e a összeomlás-konzisztens másolja.</span><span class="sxs-lookup"><span data-stu-id="4f1ac-116">This means that the backups created by a backup policy will be crash-consistent copies.</span></span>

<span data-ttu-id="4f1ac-117">A biztonsági mentési házirendek táblázatos felsorolása lehetővé teszi egy vagy több a következő mezőket a meglévő biztonsági mentési házirendek szűrése:</span><span class="sxs-lookup"><span data-stu-id="4f1ac-117">The backup policies tabular listing also allows you to filter the existing backup policies by one or more of the following fields:</span></span>

* <span data-ttu-id="4f1ac-118">**Házirend neve** – a házirendhez társított név.</span><span class="sxs-lookup"><span data-stu-id="4f1ac-118">**Policy name** – The name associated with the policy.</span></span> <span data-ttu-id="4f1ac-119">A különböző típusú házirendek a következők:</span><span class="sxs-lookup"><span data-stu-id="4f1ac-119">The different types of policies include:</span></span>

  * <span data-ttu-id="4f1ac-120">Ütemezett házirendek, amelyek kifejezetten a felhasználó által létrehozott.</span><span class="sxs-lookup"><span data-stu-id="4f1ac-120">Scheduled policies, which are explicitly created by the user.</span></span>
  * <span data-ttu-id="4f1ac-121">Importált házirendek, amelyeket a StorSimple Snapshot Manager lettek létrehozva.</span><span class="sxs-lookup"><span data-stu-id="4f1ac-121">Imported policies, which were originally created in the StorSimple Snapshot Manager.</span></span> <span data-ttu-id="4f1ac-122">A StorSimple Snapshot Manager gazdagép, a házirendek az importált címkéjét ezek rendelkezik.</span><span class="sxs-lookup"><span data-stu-id="4f1ac-122">These have a tag that describes the StorSimple Snapshot Manager host that the policies were imported from.</span></span>

  > [!NOTE]
  > <span data-ttu-id="4f1ac-123">Automatikus vagy alapértelmezett biztonsági mentési házirendek már nincs engedélyezve a kötet létrehozása idején.</span><span class="sxs-lookup"><span data-stu-id="4f1ac-123">Automatic or default backup policies are no longer enabled at the time of volume creation.</span></span>

* <span data-ttu-id="4f1ac-124">**Utolsó sikeres biztonsági mentés** – a dátum és idő, a legutóbbi sikeres készült biztonsági másolat az ehhez a szabályzathoz.</span><span class="sxs-lookup"><span data-stu-id="4f1ac-124">**Last successful backup** – The date and time of the last successful backup that was taken with this policy.</span></span>

* <span data-ttu-id="4f1ac-125">**Következő biztonsági mentés** – a dátum és idő, a következő ütemezett biztonsági mentés, hogy ez a házirend indul.</span><span class="sxs-lookup"><span data-stu-id="4f1ac-125">**Next backup** – The date and time of the next scheduled backup that will be initiated by this policy.</span></span>

* <span data-ttu-id="4f1ac-126">**Kötetek** – a házirendhez társított kötetek.</span><span class="sxs-lookup"><span data-stu-id="4f1ac-126">**Volumes** – The volumes associated with the policy.</span></span> <span data-ttu-id="4f1ac-127">A biztonsági mentési házirend társított összes kötet egy csoportba kerülnek, biztonsági mentések létrehozásakor.</span><span class="sxs-lookup"><span data-stu-id="4f1ac-127">All the volumes associated with a backup policy are grouped together when backups are created.</span></span>

* <span data-ttu-id="4f1ac-128">**Ütemezések** – a biztonsági mentési házirend kapcsolódó száma.</span><span class="sxs-lookup"><span data-stu-id="4f1ac-128">**Schedules** – The number of schedules associated with the backup policy.</span></span>

<span data-ttu-id="4f1ac-129">A gyakran használt műveletek a biztonsági mentési házirendek hajthat végre a következők:</span><span class="sxs-lookup"><span data-stu-id="4f1ac-129">The frequently used operations that you can perform for backup policies are:</span></span>

* <span data-ttu-id="4f1ac-130">Biztonsági mentési szabályzat hozzáadása</span><span class="sxs-lookup"><span data-stu-id="4f1ac-130">Add a backup policy</span></span>
* <span data-ttu-id="4f1ac-131">Adja hozzá vagy ütemezésének módosítása</span><span class="sxs-lookup"><span data-stu-id="4f1ac-131">Add or modify a schedule</span></span>
* <span data-ttu-id="4f1ac-132">Hozzáadni vagy eltávolítani egy kötet</span><span class="sxs-lookup"><span data-stu-id="4f1ac-132">Add or remove a volume</span></span>
* <span data-ttu-id="4f1ac-133">A biztonsági mentési házirend törlése</span><span class="sxs-lookup"><span data-stu-id="4f1ac-133">Delete a backup policy</span></span>
* <span data-ttu-id="4f1ac-134">Manuális biztonsági mentés készítése</span><span class="sxs-lookup"><span data-stu-id="4f1ac-134">Take a manual backup</span></span>

## <a name="add-a-backup-policy"></a><span data-ttu-id="4f1ac-135">Biztonsági mentési szabályzat hozzáadása</span><span class="sxs-lookup"><span data-stu-id="4f1ac-135">Add a backup policy</span></span>

<span data-ttu-id="4f1ac-136">Adja hozzá a biztonsági mentési házirend automatikusan ütemezni a biztonsági másolatokat.</span><span class="sxs-lookup"><span data-stu-id="4f1ac-136">Add a backup policy to automatically schedule your backups.</span></span> <span data-ttu-id="4f1ac-137">Először hozzon létre egy kötetet, ha nincs a kötethez társított alapértelmezett biztonsági mentési házirend.</span><span class="sxs-lookup"><span data-stu-id="4f1ac-137">When you first create a volume, there is no default backup policy associated with your volume.</span></span> <span data-ttu-id="4f1ac-138">Szeretné hozzáadni, és rendelje hozzá a kötet adatainak védelme érdekében biztonsági mentési házirend.</span><span class="sxs-lookup"><span data-stu-id="4f1ac-138">You need to add and assign a backup policy to protect volume data.</span></span>

<span data-ttu-id="4f1ac-139">Hajtsa végre a következő lépéseket a StorSimple eszköz biztonsági mentési házirend hozzáadása az Azure portálon.</span><span class="sxs-lookup"><span data-stu-id="4f1ac-139">Perform the following steps in the Azure portal to add a backup policy for your StorSimple device.</span></span> <span data-ttu-id="4f1ac-140">Miután hozzáadta a házirendet, definiálhat egy ütemezés (lásd: [hozzáadása vagy ütemezésének módosítása](#add-or-modify-a-schedule)).</span><span class="sxs-lookup"><span data-stu-id="4f1ac-140">After you add the policy, you can define a schedule (see [Add or modify a schedule](#add-or-modify-a-schedule)).</span></span>

[!INCLUDE [storsimple-8000-add-backup-policy-u2](../../includes/storsimple-8000-add-backup-policy-u2.md)]

## <a name="add-or-modify-a-schedule"></a><span data-ttu-id="4f1ac-141">Adja hozzá vagy ütemezésének módosítása</span><span class="sxs-lookup"><span data-stu-id="4f1ac-141">Add or modify a schedule</span></span>

<span data-ttu-id="4f1ac-142">Adja hozzá, vagy módosítsa egy ütemezést, amely egy meglévő biztonsági mentési házirend, a StorSimple eszköz csatlakozik.</span><span class="sxs-lookup"><span data-stu-id="4f1ac-142">You can add or modify a schedule that is attached to an existing backup policy on your StorSimple device.</span></span> <span data-ttu-id="4f1ac-143">A következő lépésekkel adja hozzá vagy ütemezésének módosítása az Azure portálon.</span><span class="sxs-lookup"><span data-stu-id="4f1ac-143">Perform the following steps in the Azure portal to add or modify a schedule.</span></span>

[!INCLUDE [storsimple-8000-add-modify-backup-schedule](../../includes/storsimple-8000-add-modify-backup-schedule-u2.md)]


## <a name="add-or-remove-a-volume"></a><span data-ttu-id="4f1ac-144">Hozzáadni vagy eltávolítani egy kötet</span><span class="sxs-lookup"><span data-stu-id="4f1ac-144">Add or remove a volume</span></span>

<span data-ttu-id="4f1ac-145">Adja hozzá, vagy távolítsa el a kötetet a biztonsági mentési házirend, a StorSimple eszköz rendelt.</span><span class="sxs-lookup"><span data-stu-id="4f1ac-145">You can add or remove a volume assigned to a backup policy on your StorSimple device.</span></span> <span data-ttu-id="4f1ac-146">A következő lépésekkel lehet hozzáadni vagy eltávolítani egy kötetet az Azure portálon.</span><span class="sxs-lookup"><span data-stu-id="4f1ac-146">Perform the following steps in the Azure portal to add or remove a volume.</span></span>

[!INCLUDE [storsimple-8000-add-volume-backup-policy-u2](../../includes/storsimple-8000-add-remove-volume-backup-policy-u2.md)]


## <a name="delete-a-backup-policy"></a><span data-ttu-id="4f1ac-147">A biztonsági mentési házirend törlése</span><span class="sxs-lookup"><span data-stu-id="4f1ac-147">Delete a backup policy</span></span>

<span data-ttu-id="4f1ac-148">Hajtsa végre az alábbi lépéseket az Azure-portálon a StorSimple eszköz biztonsági mentési házirend törlése.</span><span class="sxs-lookup"><span data-stu-id="4f1ac-148">Perform the following steps in the Azure portal to delete a backup policy on your StorSimple device.</span></span>

[!INCLUDE [storsimple-8000-delete-backup-policy](../../includes/storsimple-8000-delete-backup-policy.md)]

## <a name="take-a-manual-backup"></a><span data-ttu-id="4f1ac-149">Manuális biztonsági mentés készítése</span><span class="sxs-lookup"><span data-stu-id="4f1ac-149">Take a manual backup</span></span>

<span data-ttu-id="4f1ac-150">Hajtsa végre az alábbi lépéseket az Azure-portálon egyetlen kötetén igény szerinti (manuális) biztonsági másolat létrehozásához.</span><span class="sxs-lookup"><span data-stu-id="4f1ac-150">Perform the following steps in the Azure portal to create an on-demand (manual) backup for a single volume.</span></span>

[!INCLUDE [storsimple-8000-create-manual-backup](../../includes/storsimple-8000-create-manual-backup.md)]

## <a name="next-steps"></a><span data-ttu-id="4f1ac-151">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="4f1ac-151">Next steps</span></span>

<span data-ttu-id="4f1ac-152">További információ [felügyelete a StorSimple eszközt a StorSimple Device Manager szolgáltatással](storsimple-8000-manager-service-administration.md).</span><span class="sxs-lookup"><span data-stu-id="4f1ac-152">Learn more about [using the StorSimple Device Manager service to administer your StorSimple device](storsimple-8000-manager-service-administration.md).</span></span>

