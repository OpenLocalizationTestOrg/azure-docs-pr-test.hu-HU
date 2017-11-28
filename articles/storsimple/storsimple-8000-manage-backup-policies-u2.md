---
title: "aaaManage a StorSimple 8000 series biztonsági mentési házirendek |} Microsoft Docs"
description: "Azt ismerteti, hogyan használhatja a hello StorSimple Device Manager szolgáltatás toocreate, és manuális biztonsági mentések, biztonsági mentési ütemezés és a StorSimple 8000 series eszközön biztonsági másolatok megőrzésének kezelése."
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
ms.openlocfilehash: 7c56365abb6ba69d02008829ca6ae703d4632705
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="use-hello-storsimple-device-manager-service-in-azure-portal-toomanage-backup-policies"></a><span data-ttu-id="948d8-103">Hello StorSimple Device Manager szolgáltatás használata az Azure portál toomanage biztonsági mentési házirendek</span><span class="sxs-lookup"><span data-stu-id="948d8-103">Use hello StorSimple Device Manager service in Azure portal toomanage backup policies</span></span>


## <a name="overview"></a><span data-ttu-id="948d8-104">Áttekintés</span><span class="sxs-lookup"><span data-stu-id="948d8-104">Overview</span></span>

<span data-ttu-id="948d8-105">Ez az oktatóanyag azt ismerteti, hogyan toouse hello StorSimple Device Manager szolgáltatás **biztonsági mentési házirend** panel toocontrol biztonsági mentési folyamatokat és a StorSimple-köteteket a biztonsági másolatok megőrzésének.</span><span class="sxs-lookup"><span data-stu-id="948d8-105">This tutorial explains how toouse hello StorSimple Device Manager service **Backup policy** blade toocontrol backup processes and backup retention for your StorSimple volumes.</span></span> <span data-ttu-id="948d8-106">Azt is ismerteti, hogyan toocomplete manuális biztonsági mentés.</span><span class="sxs-lookup"><span data-stu-id="948d8-106">It also describes how toocomplete a manual backup.</span></span>

<span data-ttu-id="948d8-107">Biztonsági mentését egy köteten, dönthet úgy toocreate egy helyi pillanatfelvétel vagy egy felhőalapú pillanatfelvétel.</span><span class="sxs-lookup"><span data-stu-id="948d8-107">When you back up a volume, you can choose toocreate a local snapshot or a cloud snapshot.</span></span> <span data-ttu-id="948d8-108">Ha biztonsági mentést egy helyileg rögzített kötet, javasoljuk, meg kell adnia egy felhőalapú pillanatfelvétel.</span><span class="sxs-lookup"><span data-stu-id="948d8-108">If you are backing up a locally pinned volume, we recommend that you specify a cloud snapshot.</span></span> <span data-ttu-id="948d8-109">Helyi pillanatképek készítése, amely rendelkezik a nagy mennyiségű forgalom adatkészlet alapján kialakulhat egy helyileg rögzített kötet nagy mennyiségű véve azt eredményezi, hogy olyan helyzet, amelyben gyorsan futtathatja helyi elfogyott.</span><span class="sxs-lookup"><span data-stu-id="948d8-109">Taking a large number of local snapshots of a locally pinned volume coupled with a data set that has a lot of churn will result in a situation in which you could rapidly run out of local space.</span></span> <span data-ttu-id="948d8-110">Ha tootake helyi pillanatképeket, azt javasoljuk, hogy meg kevesebb napi pillanatképek tooback eltarthat, mire hello legutóbbi állapota, tartsa meg őket egy napon, és törölje őket.</span><span class="sxs-lookup"><span data-stu-id="948d8-110">If you choose tootake local snapshots, we recommend that you take fewer daily snapshots tooback up hello most recent state, retain them for a day, and then delete them.</span></span>

<span data-ttu-id="948d8-111">Amikor a pillanatképet egy felhőalapú, egy helyileg rögzített kötetet, csak megváltozott hello adatok toohello felhő, amennyiben deduplikált és tömörített másolja át.</span><span class="sxs-lookup"><span data-stu-id="948d8-111">When you take a cloud snapshot of a locally pinned volume, you copy only hello changed data toohello cloud, where it is deduplicated and compressed.</span></span>

## <a name="hello-backup-policy-blade"></a><span data-ttu-id="948d8-112">hello biztonsági mentési házirend panel</span><span class="sxs-lookup"><span data-stu-id="948d8-112">hello Backup policy blade</span></span>

<span data-ttu-id="948d8-113">Hello **biztonsági mentési házirend** a StorSimple eszköz paneljén lehetővé teszi a biztonsági mentési házirendek toomanage és ütemezés helyi és felhőbeli pillanatképeket.</span><span class="sxs-lookup"><span data-stu-id="948d8-113">hello **Backup policy** blade for your StorSimple device allows you toomanage backup policies and schedule local and cloud snapshots.</span></span> <span data-ttu-id="948d8-114">Biztonsági mentési házirendek nincsenek használt tooconfigure biztonsági mentési ütemezés és a biztonsági másolatok megőrzésének kötetek gyűjteményét.</span><span class="sxs-lookup"><span data-stu-id="948d8-114">Backup policies are used tooconfigure backup schedules and backup retention for a collection of volumes.</span></span> <span data-ttu-id="948d8-115">Biztonsági mentési házirendek lehetővé teszik több kötet pillanatképe tootake egyidejűleg.</span><span class="sxs-lookup"><span data-stu-id="948d8-115">Backup policies enable you tootake a snapshot of multiple volumes simultaneously.</span></span> <span data-ttu-id="948d8-116">Ez azt jelenti, hogy hello hozta létre a biztonsági mentési házirend lesznek összeomlás-konzisztens másolja.</span><span class="sxs-lookup"><span data-stu-id="948d8-116">This means that hello backups created by a backup policy will be crash-consistent copies.</span></span>

<span data-ttu-id="948d8-117">hello biztonsági mentési házirendek táblázatos felsorolása azt is lehetővé teszi, hogy az akkor toofilter hello meglévő biztonsági mentési házirendek egy vagy több mezőt a következő hello:</span><span class="sxs-lookup"><span data-stu-id="948d8-117">hello backup policies tabular listing also allows you toofilter hello existing backup policies by one or more of hello following fields:</span></span>

* <span data-ttu-id="948d8-118">**Házirend neve** – hello hello házirendhez társított név.</span><span class="sxs-lookup"><span data-stu-id="948d8-118">**Policy name** – hello name associated with hello policy.</span></span> <span data-ttu-id="948d8-119">hello különböző típusú házirendek a következők:</span><span class="sxs-lookup"><span data-stu-id="948d8-119">hello different types of policies include:</span></span>

  * <span data-ttu-id="948d8-120">Ütemezett házirendek, amelyeket a rendszer explicit módon hello felhasználó által létrehozott.</span><span class="sxs-lookup"><span data-stu-id="948d8-120">Scheduled policies, which are explicitly created by hello user.</span></span>
  * <span data-ttu-id="948d8-121">A StorSimple Snapshot Manager hello eredetileg létrehozott házirendek importálása.</span><span class="sxs-lookup"><span data-stu-id="948d8-121">Imported policies, which were originally created in hello StorSimple Snapshot Manager.</span></span> <span data-ttu-id="948d8-122">Ezek rendelkezik hello StorSimple Snapshot Manager állomás hello házirendek az importált címkéjét.</span><span class="sxs-lookup"><span data-stu-id="948d8-122">These have a tag that describes hello StorSimple Snapshot Manager host that hello policies were imported from.</span></span>

  > [!NOTE]
  > <span data-ttu-id="948d8-123">Automatikus vagy alapértelmezett biztonsági mentési házirendek már nincs engedélyezve a kötetek létrehozását hello időpontjában.</span><span class="sxs-lookup"><span data-stu-id="948d8-123">Automatic or default backup policies are no longer enabled at hello time of volume creation.</span></span>

* <span data-ttu-id="948d8-124">**Utolsó sikeres biztonsági mentés** – hello dátum meghatározott időpontjakor a hello utolsó sikeres készült biztonsági másolat az ehhez a szabályzathoz.</span><span class="sxs-lookup"><span data-stu-id="948d8-124">**Last successful backup** – hello date and time of hello last successful backup that was taken with this policy.</span></span>

* <span data-ttu-id="948d8-125">**Következő biztonsági mentés** – hello dátum meghatározott időpontjakor hello következő ütemezett biztonsági mentés, hogy ez a házirend indul.</span><span class="sxs-lookup"><span data-stu-id="948d8-125">**Next backup** – hello date and time of hello next scheduled backup that will be initiated by this policy.</span></span>

* <span data-ttu-id="948d8-126">**Kötetek** – hello hello házirendhez társított kötetek.</span><span class="sxs-lookup"><span data-stu-id="948d8-126">**Volumes** – hello volumes associated with hello policy.</span></span> <span data-ttu-id="948d8-127">A biztonsági mentési házirend társított összes hello kötet egy csoportba kerülnek, biztonsági mentések létrehozásakor.</span><span class="sxs-lookup"><span data-stu-id="948d8-127">All hello volumes associated with a backup policy are grouped together when backups are created.</span></span>

* <span data-ttu-id="948d8-128">**Ütemezések** – hello hello biztonsági mentési házirend kapcsolódó száma.</span><span class="sxs-lookup"><span data-stu-id="948d8-128">**Schedules** – hello number of schedules associated with hello backup policy.</span></span>

<span data-ttu-id="948d8-129">a biztonsági mentési házirendek hajthat végre a gyakran használt hello műveletek a következők:</span><span class="sxs-lookup"><span data-stu-id="948d8-129">hello frequently used operations that you can perform for backup policies are:</span></span>

* <span data-ttu-id="948d8-130">Biztonsági mentési szabályzat hozzáadása</span><span class="sxs-lookup"><span data-stu-id="948d8-130">Add a backup policy</span></span>
* <span data-ttu-id="948d8-131">Adja hozzá vagy ütemezésének módosítása</span><span class="sxs-lookup"><span data-stu-id="948d8-131">Add or modify a schedule</span></span>
* <span data-ttu-id="948d8-132">Hozzáadni vagy eltávolítani egy kötet</span><span class="sxs-lookup"><span data-stu-id="948d8-132">Add or remove a volume</span></span>
* <span data-ttu-id="948d8-133">A biztonsági mentési házirend törlése</span><span class="sxs-lookup"><span data-stu-id="948d8-133">Delete a backup policy</span></span>
* <span data-ttu-id="948d8-134">Manuális biztonsági mentés készítése</span><span class="sxs-lookup"><span data-stu-id="948d8-134">Take a manual backup</span></span>

## <a name="add-a-backup-policy"></a><span data-ttu-id="948d8-135">Biztonsági mentési szabályzat hozzáadása</span><span class="sxs-lookup"><span data-stu-id="948d8-135">Add a backup policy</span></span>

<span data-ttu-id="948d8-136">Adja hozzá a biztonsági mentési házirend tooautomatically ütemezés a biztonsági másolatokat.</span><span class="sxs-lookup"><span data-stu-id="948d8-136">Add a backup policy tooautomatically schedule your backups.</span></span> <span data-ttu-id="948d8-137">Először hozzon létre egy kötetet, ha nincs a kötethez társított alapértelmezett biztonsági mentési házirend.</span><span class="sxs-lookup"><span data-stu-id="948d8-137">When you first create a volume, there is no default backup policy associated with your volume.</span></span> <span data-ttu-id="948d8-138">Tooadd kell, és rendelje hozzá a biztonsági mentési házirend tooprotect kötet adatait.</span><span class="sxs-lookup"><span data-stu-id="948d8-138">You need tooadd and assign a backup policy tooprotect volume data.</span></span>

<span data-ttu-id="948d8-139">Hajtsa végre a következő lépéseket az Azure portál tooadd a biztonsági mentési házirend beállítása a StorSimple eszközhöz hello hello.</span><span class="sxs-lookup"><span data-stu-id="948d8-139">Perform hello following steps in hello Azure portal tooadd a backup policy for your StorSimple device.</span></span> <span data-ttu-id="948d8-140">Hello házirend hozzáadása után megadhat egy ütemezést (lásd: [hozzáadása vagy ütemezésének módosítása](#add-or-modify-a-schedule)).</span><span class="sxs-lookup"><span data-stu-id="948d8-140">After you add hello policy, you can define a schedule (see [Add or modify a schedule](#add-or-modify-a-schedule)).</span></span>

[!INCLUDE [storsimple-8000-add-backup-policy-u2](../../includes/storsimple-8000-add-backup-policy-u2.md)]

## <a name="add-or-modify-a-schedule"></a><span data-ttu-id="948d8-141">Adja hozzá vagy ütemezésének módosítása</span><span class="sxs-lookup"><span data-stu-id="948d8-141">Add or modify a schedule</span></span>

<span data-ttu-id="948d8-142">Adja hozzá, vagy a StorSimple eszköz biztonsági mentési házirend meglévő csatolt tooan ütemezésének módosítása.</span><span class="sxs-lookup"><span data-stu-id="948d8-142">You can add or modify a schedule that is attached tooan existing backup policy on your StorSimple device.</span></span> <span data-ttu-id="948d8-143">Hajtsa végre a következő lépéseket az Azure portál tooadd hello hello, vagy módosítsa a ütemezését.</span><span class="sxs-lookup"><span data-stu-id="948d8-143">Perform hello following steps in hello Azure portal tooadd or modify a schedule.</span></span>

[!INCLUDE [storsimple-8000-add-modify-backup-schedule](../../includes/storsimple-8000-add-modify-backup-schedule-u2.md)]


## <a name="add-or-remove-a-volume"></a><span data-ttu-id="948d8-144">Hozzáadni vagy eltávolítani egy kötet</span><span class="sxs-lookup"><span data-stu-id="948d8-144">Add or remove a volume</span></span>

<span data-ttu-id="948d8-145">Adja hozzá, vagy távolítsa el a hozzárendelt biztonsági mentési házirend tooa a StorSimple eszköz kötetet.</span><span class="sxs-lookup"><span data-stu-id="948d8-145">You can add or remove a volume assigned tooa backup policy on your StorSimple device.</span></span> <span data-ttu-id="948d8-146">Hajtsa végre a következő lépéseket az Azure portál tooadd hello hello, vagy távolítsa el a kötetet.</span><span class="sxs-lookup"><span data-stu-id="948d8-146">Perform hello following steps in hello Azure portal tooadd or remove a volume.</span></span>

[!INCLUDE [storsimple-8000-add-volume-backup-policy-u2](../../includes/storsimple-8000-add-remove-volume-backup-policy-u2.md)]


## <a name="delete-a-backup-policy"></a><span data-ttu-id="948d8-147">A biztonsági mentési házirend törlése</span><span class="sxs-lookup"><span data-stu-id="948d8-147">Delete a backup policy</span></span>

<span data-ttu-id="948d8-148">Hajtsa végre a StorSimple eszköz hello Azure portál toodelete a biztonsági mentési házirend lépések hello.</span><span class="sxs-lookup"><span data-stu-id="948d8-148">Perform hello following steps in hello Azure portal toodelete a backup policy on your StorSimple device.</span></span>

[!INCLUDE [storsimple-8000-delete-backup-policy](../../includes/storsimple-8000-delete-backup-policy.md)]

## <a name="take-a-manual-backup"></a><span data-ttu-id="948d8-149">Manuális biztonsági mentés készítése</span><span class="sxs-lookup"><span data-stu-id="948d8-149">Take a manual backup</span></span>

<span data-ttu-id="948d8-150">Hajtsa végre a hello lépései (hello Azure portál toocreate igény szerinti manuális) biztonsági mentés egyetlen kötetén.</span><span class="sxs-lookup"><span data-stu-id="948d8-150">Perform hello following steps in hello Azure portal toocreate an on-demand (manual) backup for a single volume.</span></span>

[!INCLUDE [storsimple-8000-create-manual-backup](../../includes/storsimple-8000-create-manual-backup.md)]

## <a name="next-steps"></a><span data-ttu-id="948d8-151">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="948d8-151">Next steps</span></span>

<span data-ttu-id="948d8-152">További információ [használatával hello StorSimple Device Manager szolgáltatás tooadminister a StorSimple eszköz](storsimple-8000-manager-service-administration.md).</span><span class="sxs-lookup"><span data-stu-id="948d8-152">Learn more about [using hello StorSimple Device Manager service tooadminister your StorSimple device](storsimple-8000-manager-service-administration.md).</span></span>

