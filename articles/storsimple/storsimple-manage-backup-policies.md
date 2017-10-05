---
title: "A StorSimple biztonsági mentési házirendek kezelése |} Microsoft Docs"
description: "Ismerteti, hogyan használhatja a StorSimple Manager szolgáltatás létrehozásához és kezeléséhez manuális biztonsági mentés, a biztonsági mentési ütemezés és a biztonsági másolatok megőrzésének."
services: storsimple
documentationcenter: NA
author: SharS
manager: carmonm
editor: 
ms.assetid: d1834bc8-d520-4463-82ae-3b32fabd021c
ms.service: storsimple
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: TBD
ms.date: 05/10/2016
ms.author: v-sharos
ms.openlocfilehash: c1e9d5d0450bab5d371aafb40fd7c5920d39dfdb
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="use-the-storsimple-manager-service-to-manage-backup-policies"></a><span data-ttu-id="725de-103">A StorSimple Manager szolgáltatás segítségével biztonsági mentési házirendek kezelése</span><span class="sxs-lookup"><span data-stu-id="725de-103">Use the StorSimple Manager service to manage backup policies</span></span>
[!INCLUDE [storsimple-version-selector-manage-backup-policies](../../includes/storsimple-version-selector-manage-backup-policies.md)]

## <a name="overview"></a><span data-ttu-id="725de-104">Áttekintés</span><span class="sxs-lookup"><span data-stu-id="725de-104">Overview</span></span>
<span data-ttu-id="725de-105">Ez az oktatóanyag ismerteti a StorSimple Manager szolgáltatással **biztonsági mentési házirendek** lap biztonsági mentési folyamatokat és a StorSimple-köteteket a biztonsági másolatok megőrzésének szabályozására.</span><span class="sxs-lookup"><span data-stu-id="725de-105">This tutorial explains how to use the StorSimple Manager service **Backup Policies** page to control backup processes and backup retention for your StorSimple volumes.</span></span> <span data-ttu-id="725de-106">Azt is bemutatja, hogyan hajthatja végre manuális biztonsági mentés.</span><span class="sxs-lookup"><span data-stu-id="725de-106">It also describes how to complete a manual backup.</span></span>

<span data-ttu-id="725de-107">A **biztonsági mentési házirendek** lap lehetővé teszi a biztonsági mentési házirendek kezelése és ütemezése helyi és felhőalapú pillanatfelvételek.</span><span class="sxs-lookup"><span data-stu-id="725de-107">The **Backup Policies** page allows you to manage backup policies and schedule local and cloud snapshots.</span></span> <span data-ttu-id="725de-108">(Biztonsági mentési házirendek segítségével konfigurálhatja a biztonsági mentési ütemezés és a biztonsági másolatok megőrzésének kötetek gyűjteményét.) Biztonsági mentési házirendek lehetővé teszik a pillanatkép készítése több kötet egy időben.</span><span class="sxs-lookup"><span data-stu-id="725de-108">(Backup policies are used to configure backup schedules and backup retention for a collection of volumes.) Backup policies enable you to take a snapshot of multiple volumes simultaneously.</span></span> <span data-ttu-id="725de-109">Ez azt jelenti, hogy a biztonsági mentési házirend által létrehozott biztonsági mentéseket kell-e a összeomlás-konzisztens másolja.</span><span class="sxs-lookup"><span data-stu-id="725de-109">This means that the backups created by a backup policy will be crash-consistent copies.</span></span> <span data-ttu-id="725de-110">Ez a lap felsorolja a biztonsági mentési házirendek, azok típusát, a társított kötetek, a megőrzött biztonsági mentéseinek számát és ezek a házirendek lehetővé teszi.</span><span class="sxs-lookup"><span data-stu-id="725de-110">This page lists the backup policies, their types, the associated volumes, the number of backups retained, and the option to enable these policies.</span></span>

<span data-ttu-id="725de-111">A **biztonsági mentési házirendek** lap is lehetővé teszi egy vagy több a következő mezőket a meglévő biztonsági mentési házirendek szűrése:</span><span class="sxs-lookup"><span data-stu-id="725de-111">The **Backup Policies** page also allows you to filter the existing backup policies by one or more of the following fields:</span></span>

* <span data-ttu-id="725de-112">**Házirend neve** – a házirendhez társított név.</span><span class="sxs-lookup"><span data-stu-id="725de-112">**Policy name** – The name associated with the policy.</span></span> <span data-ttu-id="725de-113">A különböző típusú házirendek a következők:</span><span class="sxs-lookup"><span data-stu-id="725de-113">The different types of policies include:</span></span>
  
  * <span data-ttu-id="725de-114">Ütemezett házirendek, amelyek kifejezetten a felhasználó által létrehozott.</span><span class="sxs-lookup"><span data-stu-id="725de-114">Scheduled policies, which are explicitly created by the user.</span></span>
  * <span data-ttu-id="725de-115">Az automatikus házirendek, amelyek jönnek létre, ha a kötet beállítás az alapértelmezett biztonsági mentési kötet létrehozása idején volt engedélyezve.</span><span class="sxs-lookup"><span data-stu-id="725de-115">Automatic policies, which are created when the default backup for this volume option was enabled at the time of volume creation.</span></span> <span data-ttu-id="725de-116">Ezek a házirendek, ahol kötetneve vonatkozik-e a neve a klasszikus Azure portálon a felhasználó által beállított StorSimple-kötet VolumeName_Default neve.</span><span class="sxs-lookup"><span data-stu-id="725de-116">These policies are named as VolumeName_Default where Volume name refers to the name of the StorSimple volume configured by the user in the Azure classic portal.</span></span> <span data-ttu-id="725de-117">Az automatikus házirendek 22:30 eszköz időpontban verziótól napi felhőalapú pillanatfelvételek eredményez.</span><span class="sxs-lookup"><span data-stu-id="725de-117">The automatic policies result in daily cloud snapshots beginning at 22:30 device time.</span></span>
  * <span data-ttu-id="725de-118">Importált házirendek, amelyeket a StorSimple Snapshot Manager lettek létrehozva.</span><span class="sxs-lookup"><span data-stu-id="725de-118">Imported policies, which were originally created in the StorSimple Snapshot Manager.</span></span> <span data-ttu-id="725de-119">A StorSimple Snapshot Manager gazdagép, a házirendek az importált címkéjét ezek rendelkezik.</span><span class="sxs-lookup"><span data-stu-id="725de-119">These have a tag that describes the StorSimple Snapshot Manager host that the policies were imported from.</span></span>
* <span data-ttu-id="725de-120">**Kötetek** – a házirendhez társított kötetek.</span><span class="sxs-lookup"><span data-stu-id="725de-120">**Volumes** – The volumes associated with the policy.</span></span> <span data-ttu-id="725de-121">A biztonsági mentési házirend társított összes kötet egy csoportba kerülnek, biztonsági mentések létrehozásakor.</span><span class="sxs-lookup"><span data-stu-id="725de-121">All the volumes associated with a backup policy are grouped together when backups are created.</span></span>
* <span data-ttu-id="725de-122">**Utolsó sikeres biztonsági mentés** – a dátum és idő, a legutóbbi sikeres készült biztonsági másolat az ehhez a szabályzathoz.</span><span class="sxs-lookup"><span data-stu-id="725de-122">**Last successful backup** – The date and time of the last successful backup that was taken with this policy.</span></span>
* <span data-ttu-id="725de-123">**Következő biztonsági mentés** – a dátum és idő, a következő ütemezett biztonsági mentés, hogy ez a házirend indul.</span><span class="sxs-lookup"><span data-stu-id="725de-123">**Next backup** – The date and time of the next scheduled backup that will be initiated by this policy.</span></span>
* <span data-ttu-id="725de-124">**Ütemezések** – a biztonsági mentési házirend kapcsolódó száma.</span><span class="sxs-lookup"><span data-stu-id="725de-124">**Schedules** – The number of schedules associated with the backup policy.</span></span>

<span data-ttu-id="725de-125">A gyakran használt, ezen a lapon elvégezhető műveletek a következők:</span><span class="sxs-lookup"><span data-stu-id="725de-125">The frequently used operations that you can perform from this page are:</span></span>

* <span data-ttu-id="725de-126">Biztonsági mentési szabályzat hozzáadása</span><span class="sxs-lookup"><span data-stu-id="725de-126">Add a backup policy</span></span> 
* <span data-ttu-id="725de-127">Adja hozzá vagy ütemezésének módosítása</span><span class="sxs-lookup"><span data-stu-id="725de-127">Add or modify a schedule</span></span> 
* <span data-ttu-id="725de-128">A biztonsági mentési házirend törlése</span><span class="sxs-lookup"><span data-stu-id="725de-128">Delete a backup policy</span></span> 
* <span data-ttu-id="725de-129">Manuális biztonsági mentés készítése</span><span class="sxs-lookup"><span data-stu-id="725de-129">Take a manual backup</span></span> 
* <span data-ttu-id="725de-130">Hozzon létre egy egyéni biztonsági mentési házirend több kötetek és ütemezése</span><span class="sxs-lookup"><span data-stu-id="725de-130">Create a custom backup policy with multiple volumes and schedules</span></span> 

## <a name="add-a-backup-policy"></a><span data-ttu-id="725de-131">Biztonsági mentési szabályzat hozzáadása</span><span class="sxs-lookup"><span data-stu-id="725de-131">Add a backup policy</span></span>
<span data-ttu-id="725de-132">Adja hozzá a biztonsági mentési házirend automatikusan ütemezni a biztonsági másolatokat.</span><span class="sxs-lookup"><span data-stu-id="725de-132">Add a backup policy to automatically schedule your backups.</span></span> <span data-ttu-id="725de-133">Hajtsa végre a következő lépéseket a klasszikus Azure portálon vegye fel a biztonsági mentési házirend beállítása a StorSimple eszközhöz.</span><span class="sxs-lookup"><span data-stu-id="725de-133">Perform the following steps in the Azure classic portal to add a backup policy for your StorSimple device.</span></span> <span data-ttu-id="725de-134">Miután hozzáadta a házirendet, definiálhat egy ütemezés (lásd: [hozzáadása vagy ütemezésének módosítása](#add-or-modify-a-schedule)).</span><span class="sxs-lookup"><span data-stu-id="725de-134">After you add the policy, you can define a schedule (see [Add or modify a schedule](#add-or-modify-a-schedule)).</span></span>

[!INCLUDE [storsimple-add-backup-policy](../../includes/storsimple-add-backup-policy.md)]

<span data-ttu-id="725de-135">![Videó elérhető](./media/storsimple-manage-backup-policies/Video_icon.png) **Videó elérhető**</span><span class="sxs-lookup"><span data-stu-id="725de-135">![Video available](./media/storsimple-manage-backup-policies/Video_icon.png) **Video available**</span></span>

<span data-ttu-id="725de-136">Bemutató videó bemutatja, hogyan hozzon létre egy helyi vagy felhőalapú biztonsági mentési házirend, kattintson a [Itt](https://azure.microsoft.com/documentation/videos/create-storsimple-backup-policies/).</span><span class="sxs-lookup"><span data-stu-id="725de-136">To watch a video that demonstrates how to create a local or cloud backup policy, click [here](https://azure.microsoft.com/documentation/videos/create-storsimple-backup-policies/).</span></span>

## <a name="add-or-modify-a-schedule"></a><span data-ttu-id="725de-137">Adja hozzá vagy ütemezésének módosítása</span><span class="sxs-lookup"><span data-stu-id="725de-137">Add or modify a schedule</span></span>
<span data-ttu-id="725de-138">Adja hozzá, vagy módosítsa egy ütemezést, amely egy meglévő biztonsági mentési házirend, a StorSimple eszköz csatlakozik.</span><span class="sxs-lookup"><span data-stu-id="725de-138">You can add or modify a schedule that is attached to an existing backup policy on your StorSimple device.</span></span> <span data-ttu-id="725de-139">Hajtsa végre a következő lépéseket a klasszikus Azure portálon hozzáadása vagy módosítása egy ütemezés szerint.</span><span class="sxs-lookup"><span data-stu-id="725de-139">Perform the following steps in the Azure classic portal to add or modify a schedule.</span></span>

[!INCLUDE [storsimple-add-modify-backup-schedule](../../includes/storsimple-add-modify-backup-schedule.md)]

## <a name="delete-a-backup-policy"></a><span data-ttu-id="725de-140">A biztonsági mentési házirend törlése</span><span class="sxs-lookup"><span data-stu-id="725de-140">Delete a backup policy</span></span>
<span data-ttu-id="725de-141">Hajtsa végre a következő lépéseket a klasszikus Azure-portálon a StorSimple eszköz biztonsági mentési házirend törlése.</span><span class="sxs-lookup"><span data-stu-id="725de-141">Perform the following steps in the Azure classic portal to delete a backup policy on your StorSimple device.</span></span>

[!INCLUDE [storsimple-delete-backup-policy](../../includes/storsimple-delete-backup-policy.md)]

## <a name="take-a-manual-backup"></a><span data-ttu-id="725de-142">Manuális biztonsági mentés készítése</span><span class="sxs-lookup"><span data-stu-id="725de-142">Take a manual backup</span></span>
<span data-ttu-id="725de-143">Hajtsa végre a következő lépéseket a klasszikus Azure portálon hozzon létre (egy igény szerinti manuális) biztonsági mentés egyetlen kötetén.</span><span class="sxs-lookup"><span data-stu-id="725de-143">Perform the following steps in the Azure classic portal to create an on-demand (manual) backup for a single volume.</span></span>

[!INCLUDE [storsimple-create-manual-backup](../../includes/storsimple-create-manual-backup.md)]

## <a name="create-a-custom-backup-policy-with-multiple-volumes-and-schedules"></a><span data-ttu-id="725de-144">Hozzon létre egy egyéni biztonsági mentési házirend több kötetek és ütemezése</span><span class="sxs-lookup"><span data-stu-id="725de-144">Create a custom backup policy with multiple volumes and schedules</span></span>
<span data-ttu-id="725de-145">Hajtsa végre a következő lépéseket a klasszikus Azure portálon hozzon létre egy egyéni biztonsági mentési házirend, amelyen több kötetek és ütemezések.</span><span class="sxs-lookup"><span data-stu-id="725de-145">Perform the following steps in the Azure classic portal to create a custom backup policy that has multiple volumes and schedules.</span></span>

[!INCLUDE [storsimple-create-custom-backup-policy](../../includes/storsimple-create-custom-backup-policy.md)]

## <a name="next-steps"></a><span data-ttu-id="725de-146">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="725de-146">Next steps</span></span>
<span data-ttu-id="725de-147">További információ [a StorSimple Manager szolgáltatás használata a StorSimple eszköz felügyeletéhez](storsimple-manager-service-administration.md).</span><span class="sxs-lookup"><span data-stu-id="725de-147">Learn more about [using the StorSimple Manager service to administer your StorSimple device](storsimple-manager-service-administration.md).</span></span>

