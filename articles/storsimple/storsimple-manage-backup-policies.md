---
title: "aaaManage a StorSimple biztonsági mentési házirendek |} Microsoft Docs"
description: "Azt ismerteti, hogyan használhatja a hello StorSimple Manager szolgáltatás toocreate, és manuális biztonsági mentések, a biztonsági mentési ütemezés és a biztonsági másolatok megőrzésének kezelése."
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
ms.openlocfilehash: 710cbe54d14031b4de43e9da292ed169085d5af9
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="use-hello-storsimple-manager-service-toomanage-backup-policies"></a><span data-ttu-id="3682b-103">Hello StorSimple Manager szolgáltatás toomanage biztonsági mentési házirendek használata</span><span class="sxs-lookup"><span data-stu-id="3682b-103">Use hello StorSimple Manager service toomanage backup policies</span></span>
[!INCLUDE [storsimple-version-selector-manage-backup-policies](../../includes/storsimple-version-selector-manage-backup-policies.md)]

## <a name="overview"></a><span data-ttu-id="3682b-104">Áttekintés</span><span class="sxs-lookup"><span data-stu-id="3682b-104">Overview</span></span>
<span data-ttu-id="3682b-105">Ez az oktatóanyag azt ismerteti, hogyan toouse hello StorSimple Manager szolgáltatás **biztonsági mentési házirendek** toocontrol biztonsági mentési folyamatokat és a StorSimple-köteteket a biztonsági másolatok megőrzésének lapon.</span><span class="sxs-lookup"><span data-stu-id="3682b-105">This tutorial explains how toouse hello StorSimple Manager service **Backup Policies** page toocontrol backup processes and backup retention for your StorSimple volumes.</span></span> <span data-ttu-id="3682b-106">Azt is ismerteti, hogyan toocomplete manuális biztonsági mentés.</span><span class="sxs-lookup"><span data-stu-id="3682b-106">It also describes how toocomplete a manual backup.</span></span>

<span data-ttu-id="3682b-107">Hello **biztonsági mentési házirendek** lap lehetővé teszi a biztonsági mentési házirendek toomanage és ütemezés helyi és felhőbeli pillanatképeket.</span><span class="sxs-lookup"><span data-stu-id="3682b-107">hello **Backup Policies** page allows you toomanage backup policies and schedule local and cloud snapshots.</span></span> <span data-ttu-id="3682b-108">(A biztonsági mentési házirendek nincsenek használt tooconfigure biztonsági mentési ütemezés és a biztonsági másolatok megőrzésének kötetek gyűjteményét.) Biztonsági mentési házirendek lehetővé teszik több kötet pillanatképe tootake egyidejűleg.</span><span class="sxs-lookup"><span data-stu-id="3682b-108">(Backup policies are used tooconfigure backup schedules and backup retention for a collection of volumes.) Backup policies enable you tootake a snapshot of multiple volumes simultaneously.</span></span> <span data-ttu-id="3682b-109">Ez azt jelenti, hogy hello hozta létre a biztonsági mentési házirend lesznek összeomlás-konzisztens másolja.</span><span class="sxs-lookup"><span data-stu-id="3682b-109">This means that hello backups created by a backup policy will be crash-consistent copies.</span></span> <span data-ttu-id="3682b-110">Ezen a lapon hello biztonsági mentési házirendek, típusok, hello társított kötetek, megőrzött biztonsági mentések hello számát sorolja fel, és hello beállítás tooenable ezeket a házirendeket.</span><span class="sxs-lookup"><span data-stu-id="3682b-110">This page lists hello backup policies, their types, hello associated volumes, hello number of backups retained, and hello option tooenable these policies.</span></span>

<span data-ttu-id="3682b-111">Hello **biztonsági mentési házirendek** lap lehetővé teszi toofilter hello meglévő biztonsági mentési házirendek közül egy vagy több mezőt a következő hello:</span><span class="sxs-lookup"><span data-stu-id="3682b-111">hello **Backup Policies** page also allows you toofilter hello existing backup policies by one or more of hello following fields:</span></span>

* <span data-ttu-id="3682b-112">**Házirend neve** – hello hello házirendhez társított név.</span><span class="sxs-lookup"><span data-stu-id="3682b-112">**Policy name** – hello name associated with hello policy.</span></span> <span data-ttu-id="3682b-113">hello különböző típusú házirendek a következők:</span><span class="sxs-lookup"><span data-stu-id="3682b-113">hello different types of policies include:</span></span>
  
  * <span data-ttu-id="3682b-114">Ütemezett házirendek, amelyeket a rendszer explicit módon hello felhasználó által létrehozott.</span><span class="sxs-lookup"><span data-stu-id="3682b-114">Scheduled policies, which are explicitly created by hello user.</span></span>
  * <span data-ttu-id="3682b-115">Az automatikus házirendek, amelyek jönnek létre, ha a kötet beállítás hello alapértelmezett biztonsági mentés a kötetek létrehozását hello időpontjában volt engedélyezve.</span><span class="sxs-lookup"><span data-stu-id="3682b-115">Automatic policies, which are created when hello default backup for this volume option was enabled at hello time of volume creation.</span></span> <span data-ttu-id="3682b-116">Ezek a házirendek, ahol kötetneve hivatkozik toohello neve a klasszikus Azure portálon hello hello felhasználó által beállított StorSimple-kötet hello VolumeName_Default neve.</span><span class="sxs-lookup"><span data-stu-id="3682b-116">These policies are named as VolumeName_Default where Volume name refers toohello name of hello StorSimple volume configured by hello user in hello Azure classic portal.</span></span> <span data-ttu-id="3682b-117">hello automatikus házirendek 22:30 eszköz időpontban verziótól napi felhőalapú pillanatfelvételek eredményez.</span><span class="sxs-lookup"><span data-stu-id="3682b-117">hello automatic policies result in daily cloud snapshots beginning at 22:30 device time.</span></span>
  * <span data-ttu-id="3682b-118">A StorSimple Snapshot Manager hello eredetileg létrehozott házirendek importálása.</span><span class="sxs-lookup"><span data-stu-id="3682b-118">Imported policies, which were originally created in hello StorSimple Snapshot Manager.</span></span> <span data-ttu-id="3682b-119">Ezek rendelkezik hello StorSimple Snapshot Manager állomás hello házirendek az importált címkéjét.</span><span class="sxs-lookup"><span data-stu-id="3682b-119">These have a tag that describes hello StorSimple Snapshot Manager host that hello policies were imported from.</span></span>
* <span data-ttu-id="3682b-120">**Kötetek** – hello hello házirendhez társított kötetek.</span><span class="sxs-lookup"><span data-stu-id="3682b-120">**Volumes** – hello volumes associated with hello policy.</span></span> <span data-ttu-id="3682b-121">A biztonsági mentési házirend társított összes hello kötet egy csoportba kerülnek, biztonsági mentések létrehozásakor.</span><span class="sxs-lookup"><span data-stu-id="3682b-121">All hello volumes associated with a backup policy are grouped together when backups are created.</span></span>
* <span data-ttu-id="3682b-122">**Utolsó sikeres biztonsági mentés** – hello dátum meghatározott időpontjakor a hello utolsó sikeres készült biztonsági másolat az ehhez a szabályzathoz.</span><span class="sxs-lookup"><span data-stu-id="3682b-122">**Last successful backup** – hello date and time of hello last successful backup that was taken with this policy.</span></span>
* <span data-ttu-id="3682b-123">**Következő biztonsági mentés** – hello dátum meghatározott időpontjakor hello következő ütemezett biztonsági mentés, hogy ez a házirend indul.</span><span class="sxs-lookup"><span data-stu-id="3682b-123">**Next backup** – hello date and time of hello next scheduled backup that will be initiated by this policy.</span></span>
* <span data-ttu-id="3682b-124">**Ütemezések** – hello hello biztonsági mentési házirend kapcsolódó száma.</span><span class="sxs-lookup"><span data-stu-id="3682b-124">**Schedules** – hello number of schedules associated with hello backup policy.</span></span>

<span data-ttu-id="3682b-125">Ezen a lapon elvégezhető a gyakran használt hello műveletek a következők:</span><span class="sxs-lookup"><span data-stu-id="3682b-125">hello frequently used operations that you can perform from this page are:</span></span>

* <span data-ttu-id="3682b-126">Biztonsági mentési szabályzat hozzáadása</span><span class="sxs-lookup"><span data-stu-id="3682b-126">Add a backup policy</span></span> 
* <span data-ttu-id="3682b-127">Adja hozzá vagy ütemezésének módosítása</span><span class="sxs-lookup"><span data-stu-id="3682b-127">Add or modify a schedule</span></span> 
* <span data-ttu-id="3682b-128">A biztonsági mentési házirend törlése</span><span class="sxs-lookup"><span data-stu-id="3682b-128">Delete a backup policy</span></span> 
* <span data-ttu-id="3682b-129">Manuális biztonsági mentés készítése</span><span class="sxs-lookup"><span data-stu-id="3682b-129">Take a manual backup</span></span> 
* <span data-ttu-id="3682b-130">Hozzon létre egy egyéni biztonsági mentési házirend több kötetek és ütemezése</span><span class="sxs-lookup"><span data-stu-id="3682b-130">Create a custom backup policy with multiple volumes and schedules</span></span> 

## <a name="add-a-backup-policy"></a><span data-ttu-id="3682b-131">Biztonsági mentési szabályzat hozzáadása</span><span class="sxs-lookup"><span data-stu-id="3682b-131">Add a backup policy</span></span>
<span data-ttu-id="3682b-132">Adja hozzá a biztonsági mentési házirend tooautomatically ütemezés a biztonsági másolatokat.</span><span class="sxs-lookup"><span data-stu-id="3682b-132">Add a backup policy tooautomatically schedule your backups.</span></span> <span data-ttu-id="3682b-133">Hajtsa végre a következő lépéseket az Azure klasszikus portál tooadd a biztonsági mentési házirend beállítása a StorSimple eszközhöz hello hello.</span><span class="sxs-lookup"><span data-stu-id="3682b-133">Perform hello following steps in hello Azure classic portal tooadd a backup policy for your StorSimple device.</span></span> <span data-ttu-id="3682b-134">Hello házirend hozzáadása után megadhat egy ütemezést (lásd: [hozzáadása vagy ütemezésének módosítása](#add-or-modify-a-schedule)).</span><span class="sxs-lookup"><span data-stu-id="3682b-134">After you add hello policy, you can define a schedule (see [Add or modify a schedule](#add-or-modify-a-schedule)).</span></span>

[!INCLUDE [storsimple-add-backup-policy](../../includes/storsimple-add-backup-policy.md)]

<span data-ttu-id="3682b-135">![Videó elérhető](./media/storsimple-manage-backup-policies/Video_icon.png)**Videó elérhető**</span><span class="sxs-lookup"><span data-stu-id="3682b-135">![Video available](./media/storsimple-manage-backup-policies/Video_icon.png) **Video available**</span></span>

<span data-ttu-id="3682b-136">a videó bemutatja, hogyan toocreate egy helyi vagy felhőalapú biztonsági másolatot készíteni a házirend, toowatch kattintson [Itt](https://azure.microsoft.com/documentation/videos/create-storsimple-backup-policies/).</span><span class="sxs-lookup"><span data-stu-id="3682b-136">toowatch a video that demonstrates how toocreate a local or cloud backup policy, click [here](https://azure.microsoft.com/documentation/videos/create-storsimple-backup-policies/).</span></span>

## <a name="add-or-modify-a-schedule"></a><span data-ttu-id="3682b-137">Adja hozzá vagy ütemezésének módosítása</span><span class="sxs-lookup"><span data-stu-id="3682b-137">Add or modify a schedule</span></span>
<span data-ttu-id="3682b-138">Adja hozzá, vagy a StorSimple eszköz biztonsági mentési házirend meglévő csatolt tooan ütemezésének módosítása.</span><span class="sxs-lookup"><span data-stu-id="3682b-138">You can add or modify a schedule that is attached tooan existing backup policy on your StorSimple device.</span></span> <span data-ttu-id="3682b-139">Hajtsa végre a következő lépéseket az Azure klasszikus portál tooadd hello hello, vagy módosítsa a ütemezését.</span><span class="sxs-lookup"><span data-stu-id="3682b-139">Perform hello following steps in hello Azure classic portal tooadd or modify a schedule.</span></span>

[!INCLUDE [storsimple-add-modify-backup-schedule](../../includes/storsimple-add-modify-backup-schedule.md)]

## <a name="delete-a-backup-policy"></a><span data-ttu-id="3682b-140">A biztonsági mentési házirend törlése</span><span class="sxs-lookup"><span data-stu-id="3682b-140">Delete a backup policy</span></span>
<span data-ttu-id="3682b-141">Hajtsa végre a StorSimple eszköz hello Azure klasszikus portál toodelete a biztonsági mentési házirend lépések hello.</span><span class="sxs-lookup"><span data-stu-id="3682b-141">Perform hello following steps in hello Azure classic portal toodelete a backup policy on your StorSimple device.</span></span>

[!INCLUDE [storsimple-delete-backup-policy](../../includes/storsimple-delete-backup-policy.md)]

## <a name="take-a-manual-backup"></a><span data-ttu-id="3682b-142">Manuális biztonsági mentés készítése</span><span class="sxs-lookup"><span data-stu-id="3682b-142">Take a manual backup</span></span>
<span data-ttu-id="3682b-143">Hajtsa végre a hello lépései (hello Azure klasszikus portál toocreate igény szerinti manuális) biztonsági mentés egyetlen kötetén.</span><span class="sxs-lookup"><span data-stu-id="3682b-143">Perform hello following steps in hello Azure classic portal toocreate an on-demand (manual) backup for a single volume.</span></span>

[!INCLUDE [storsimple-create-manual-backup](../../includes/storsimple-create-manual-backup.md)]

## <a name="create-a-custom-backup-policy-with-multiple-volumes-and-schedules"></a><span data-ttu-id="3682b-144">Hozzon létre egy egyéni biztonsági mentési házirend több kötetek és ütemezése</span><span class="sxs-lookup"><span data-stu-id="3682b-144">Create a custom backup policy with multiple volumes and schedules</span></span>
<span data-ttu-id="3682b-145">Hajtsa végre a következő lépéseket az Azure klasszikus portál toocreate egy egyéni biztonsági mentési házirend, amelyen több kötetek és ütemezések hello hello.</span><span class="sxs-lookup"><span data-stu-id="3682b-145">Perform hello following steps in hello Azure classic portal toocreate a custom backup policy that has multiple volumes and schedules.</span></span>

[!INCLUDE [storsimple-create-custom-backup-policy](../../includes/storsimple-create-custom-backup-policy.md)]

## <a name="next-steps"></a><span data-ttu-id="3682b-146">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="3682b-146">Next steps</span></span>
<span data-ttu-id="3682b-147">További információ [használatával hello StorSimple Manager szolgáltatás tooadminister a StorSimple eszköz](storsimple-manager-service-administration.md).</span><span class="sxs-lookup"><span data-stu-id="3682b-147">Learn more about [using hello StorSimple Manager service tooadminister your StorSimple device](storsimple-manager-service-administration.md).</span></span>

