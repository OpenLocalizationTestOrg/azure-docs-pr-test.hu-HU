---
title: "StorSimple feladatátvétel, a StorSimple felhő berendezésen vész-helyreállítási |} Microsoft Docs"
description: "Útmutató: a StorSimple 8000 sorozat fizikai eszköz egy felhőalapú készülék a feladatátvételt."
services: storsimple
documentationcenter: 
author: alkohli
manager: timlt
editor: 
ms.assetid: 
ms.service: storsimple
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 07/03/2017
ms.author: alkohli
ms.openlocfilehash: ec8bebf2854e84a37e84b45564e80fc20b63d8d8
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="fail-over-to-your-storsimple-cloud-appliance"></a><span data-ttu-id="565dd-103">Feladatok átadása a StorSimple felhő készülék</span><span class="sxs-lookup"><span data-stu-id="565dd-103">Fail over to your StorSimple Cloud Appliance</span></span>

## <a name="overview"></a><span data-ttu-id="565dd-104">Áttekintés</span><span class="sxs-lookup"><span data-stu-id="565dd-104">Overview</span></span>

<span data-ttu-id="565dd-105">Ez az oktatóanyag a StorSimple 8000 series fizikai eszközöket, hogy a StorSimple felhő készülék feladatátvételt, ha egy olyan vészhelyzet esetén szükséges lépéseket ismerteti.</span><span class="sxs-lookup"><span data-stu-id="565dd-105">This tutorial describes the steps required to fail over a StorSimple 8000 series physical device to a StorSimple Cloud Appliance if there is a disaster.</span></span> <span data-ttu-id="565dd-106">A StorSimple eszköz feladatátvételi szolgáltatását használja át a forrás fizikai eszközt az adatközpontban lévő adatok egy Azure-ban futó felhő készülék.</span><span class="sxs-lookup"><span data-stu-id="565dd-106">StorSimple uses the device failover feature to migrate data from a source physical device in the datacenter to a cloud appliance running in Azure.</span></span> <span data-ttu-id="565dd-107">Ez az oktatóanyag az útmutató a StorSimple 8000 series fizikai eszközöket és a felhő készülékek 3 és újabb verzióit futtató a szoftver verziója frissítés vonatkozik.</span><span class="sxs-lookup"><span data-stu-id="565dd-107">The guidance in this tutorial applies to StorSimple 8000 series physical devices and cloud appliances running software versions Update 3 and later.</span></span>

<span data-ttu-id="565dd-108">Eszköz feladatátvételi, és hogyan használja a katasztrófa utáni helyreállítás kapcsolatos további tudnivalókért keresse fel [feladatátvétel és a katasztrófa utáni helyreállítás a StorSimple 8000 sorozat eszközeire](storsimple-8000-device-failover-disaster-recovery.md).</span><span class="sxs-lookup"><span data-stu-id="565dd-108">To learn more about device failover and how it is used to recover from a disaster, go to [Failover and disaster recovery for StorSimple 8000 series devices](storsimple-8000-device-failover-disaster-recovery.md).</span></span>

<span data-ttu-id="565dd-109">Feladatok átadása a StorSimple fizikai eszköz egy másik fizikai eszközön, keresse fel [feladatok átadása a StorSimple fizikai eszköz](storsimple-8000-device-failover-physical-device.md).</span><span class="sxs-lookup"><span data-stu-id="565dd-109">To fail over a StorSimple physical device to another physical device, go to [Fail over to a StorSimple physical device](storsimple-8000-device-failover-physical-device.md).</span></span> <span data-ttu-id="565dd-110">Maga az eszköz feladatátvételt, keresse fel [átveheti az ugyanazon fizikai StorSimple-eszköz](storsimple-8000-device-failover-same-device.md).</span><span class="sxs-lookup"><span data-stu-id="565dd-110">To fail over a device to itself, go to [Fail over to the same StorSimple physical device](storsimple-8000-device-failover-same-device.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="565dd-111">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="565dd-111">Prerequisites</span></span>

- <span data-ttu-id="565dd-112">Győződjön meg arról, hogy átolvasta eszköz feladatátvételi szempontokat.</span><span class="sxs-lookup"><span data-stu-id="565dd-112">Ensure that you have reviewed the considerations for device failover.</span></span> <span data-ttu-id="565dd-113">További információkért látogasson el [eszköz feladatátvételi a gyakori szempontok](storsimple-8000-device-failover-disaster-recovery.md).</span><span class="sxs-lookup"><span data-stu-id="565dd-113">For more information, go to [Common considerations for device failover](storsimple-8000-device-failover-disaster-recovery.md).</span></span>

- <span data-ttu-id="565dd-114">A StorSimple felhő készülék létrehozása és konfigurálása a művelet futtatása előtt kell rendelkeznie.</span><span class="sxs-lookup"><span data-stu-id="565dd-114">You must have a StorSimple Cloud Appliance created and configured before running this procedure.</span></span> <span data-ttu-id="565dd-115">Ha a futó 3 ügyfélszoftverének verziójára frissítése, vagy később, érdemes lehet egy 8020-as modellt felhő készüléknek az a vész-Helyreállítási.</span><span class="sxs-lookup"><span data-stu-id="565dd-115">If running   Update 3 software version or later, consider using an 8020 cloud appliance for the DR.</span></span> <span data-ttu-id="565dd-116">A 8020-as modellt modell 64 TB-os rendelkezik, és a prémium szintű Storage használja.</span><span class="sxs-lookup"><span data-stu-id="565dd-116">The 8020 model has 64 TB and uses Premium Storage.</span></span> <span data-ttu-id="565dd-117">További információkért látogasson el [központi telepítése és kezelése a StorSimple felhő készülék](storsimple-8000-cloud-appliance-u2.md).</span><span class="sxs-lookup"><span data-stu-id="565dd-117">For more information, go to [Deploy and manage a StorSimple Cloud Appliance](storsimple-8000-cloud-appliance-u2.md).</span></span>

## <a name="steps-to-fail-over-to-a-cloud-appliance"></a><span data-ttu-id="565dd-118">Feladatok átadása a felhő készülék lépései</span><span class="sxs-lookup"><span data-stu-id="565dd-118">Steps to fail over to a cloud appliance</span></span>

<span data-ttu-id="565dd-119">A következő lépésekkel visszaállíthatja az eszköz StorSimple felhő készülék célja.</span><span class="sxs-lookup"><span data-stu-id="565dd-119">Perform the following steps to restore the device to a target StorSimple Cloud Appliance.</span></span>

1.  <span data-ttu-id="565dd-120">Győződjön meg arról, hogy a feladatátvétel kívánt kötettároló felhőalapú pillanatfelvételek vannak társítva.</span><span class="sxs-lookup"><span data-stu-id="565dd-120">Verify that the volume container you want to fail over has associated cloud snapshots.</span></span> <span data-ttu-id="565dd-121">További információkért látogasson el [StorSimple az Eszközkezelő szolgáltatás a biztonsági mentések létrehozását](storsimple-8000-manage-backup-policies-u2.md).</span><span class="sxs-lookup"><span data-stu-id="565dd-121">For more information, go to [Use StorSimple Device Manager service to create backups](storsimple-8000-manage-backup-policies-u2.md).</span></span>
2. <span data-ttu-id="565dd-122">A StorSimple-eszközkezelő szolgáltatásban kattintson az **Eszközök** elemre.</span><span class="sxs-lookup"><span data-stu-id="565dd-122">Go to your StorSimple Device Manager service and click **Devices**.</span></span> <span data-ttu-id="565dd-123">Az a **eszközök** panelen, lépjen a szolgáltatáshoz kapcsolódó eszközök listáját.</span><span class="sxs-lookup"><span data-stu-id="565dd-123">In the **Devices** blade, go to the list of devices connected with your service.</span></span>
    <span data-ttu-id="565dd-124">![Válassza ki az eszköz](./media/storsimple-8000-device-failover-disaster-recovery/failover-cloud-dev1.png)</span><span class="sxs-lookup"><span data-stu-id="565dd-124">![Select device](./media/storsimple-8000-device-failover-disaster-recovery/failover-cloud-dev1.png)</span></span>
3. <span data-ttu-id="565dd-125">Válassza ki, és kattintson a forrás-eszközre.</span><span class="sxs-lookup"><span data-stu-id="565dd-125">Select and click your source device.</span></span> <span data-ttu-id="565dd-126">A forráseszköz átadni kívánt kötettárolók.</span><span class="sxs-lookup"><span data-stu-id="565dd-126">The source device has the volume containers that you want to fail over.</span></span> <span data-ttu-id="565dd-127">Ugrás a **beállítások > Kötettárolók**.</span><span class="sxs-lookup"><span data-stu-id="565dd-127">Go to **Settings > Volume Containers**.</span></span>

    ![Válassza ki az eszköz](./media/storsimple-8000-device-failover-disaster-recovery/failover-cloud-dev2.png)
    
4. <span data-ttu-id="565dd-129">Válassza ki a kötettároló szeretné, hogy áthelyezze a feladatokat egy másik eszközre.</span><span class="sxs-lookup"><span data-stu-id="565dd-129">Select a volume container that you would like to fail over to another device.</span></span> <span data-ttu-id="565dd-130">Kattintson a kötettároló található kötetek listájának megjelenítéséhez.</span><span class="sxs-lookup"><span data-stu-id="565dd-130">Click the volume container to display the list of volumes within this container.</span></span> <span data-ttu-id="565dd-131">Válassza ki a kötetet, kattintson a jobb gombbal, majd kattintson **Offline állapotba** a kötet offline állapotba.</span><span class="sxs-lookup"><span data-stu-id="565dd-131">Select a volume, right-click, and click **Take Offline** to take the volume offline.</span></span>

    ![Válassza ki az eszköz](./media/storsimple-8000-device-failover-disaster-recovery/failover-cloud-dev5.png)

5. <span data-ttu-id="565dd-133">Ismételje meg ezt a folyamatot a a kötettároló összes kötetet.</span><span class="sxs-lookup"><span data-stu-id="565dd-133">Repeat this process for all the volumes in the volume container.</span></span>

     ![Válassza ki az eszköz](./media/storsimple-8000-device-failover-disaster-recovery/failover-cloud-dev7.png)

6. <span data-ttu-id="565dd-135">Ismételje meg az előző lépésben szeretné, hogy áthelyezze a feladatokat egy másik eszközre kötettárolók.</span><span class="sxs-lookup"><span data-stu-id="565dd-135">Repeat the previous step for all the volume containers you would like to fail over to another device.</span></span>

7. <span data-ttu-id="565dd-136">Lépjen vissza a **eszközök** panelen.</span><span class="sxs-lookup"><span data-stu-id="565dd-136">Go back to the **Devices** blade.</span></span> <span data-ttu-id="565dd-137">A parancssávon kattintson **feladatátvételt**.</span><span class="sxs-lookup"><span data-stu-id="565dd-137">From the command bar, click **Fail over**.</span></span>

    ![Kattintson a sikertelen keresztül](./media/storsimple-8000-device-failover-disaster-recovery/failover-cloud-dev8.png)
8. <span data-ttu-id="565dd-139">Az a **feladatátvételt** panelen végezze el a következő lépéseket:</span><span class="sxs-lookup"><span data-stu-id="565dd-139">In the **Fail over** blade, perform the following steps:</span></span>
   
    1. <span data-ttu-id="565dd-140">Kattintson a **forrás**.</span><span class="sxs-lookup"><span data-stu-id="565dd-140">Click **Source**.</span></span> <span data-ttu-id="565dd-141">Válassza ki a kötettárolók, hogy áthelyezze a feladatokat.</span><span class="sxs-lookup"><span data-stu-id="565dd-141">Select the volume containers to fail over.</span></span> <span data-ttu-id="565dd-142">**Csak a hozzárendelt felhő pillanatképek és a kapcsolat nélküli kötetekkel kötettárolók jelennek meg.**</span><span class="sxs-lookup"><span data-stu-id="565dd-142">**Only the volume containers with associated cloud snapshots and offline volumes are displayed.**</span></span>
        <span data-ttu-id="565dd-143">![Forrás kiválasztása](./media/storsimple-8000-device-failover-disaster-recovery/failover-cloud-dev11.png)</span><span class="sxs-lookup"><span data-stu-id="565dd-143">![Select source](./media/storsimple-8000-device-failover-disaster-recovery/failover-cloud-dev11.png)</span></span>
    2. <span data-ttu-id="565dd-144">Kattintson a **cél**.</span><span class="sxs-lookup"><span data-stu-id="565dd-144">Click **Target**.</span></span> <span data-ttu-id="565dd-145">Válassza ki a cél felhő készülék az elérhető eszközök a legördülő listából.</span><span class="sxs-lookup"><span data-stu-id="565dd-145">Select a target cloud appliance from the dropdown list of available devices.</span></span> <span data-ttu-id="565dd-146">**Csak azokat az eszközöket, amelyek forrás kötettárolók befogadásához elegendő kapacitással rendelkeznek a listában jelennek meg.**</span><span class="sxs-lookup"><span data-stu-id="565dd-146">**Only the devices that have sufficient capacity to accommodate source volume containers are displayed in the list.**</span></span>

        ![Válassza ki a cél](./media/storsimple-8000-device-failover-disaster-recovery/failover-cloud-dev12.png)

    3. <span data-ttu-id="565dd-148">Tekintse át a feladatátvételi beállítások **összegzés** , és jelölje be a jelölőnégyzetet, amely azt jelzi, hogy a kijelölt kötet tárolókban lévő kötetek offline módban.</span><span class="sxs-lookup"><span data-stu-id="565dd-148">Review the failover settings under **Summary** and select the checkbox indicating that the volumes in selected volume containers are offline.</span></span> 

        ![Tekintse át a feladatátvételi beállításait](./media/storsimple-8000-device-failover-disaster-recovery/failover-cloud-dev13.png)

9. <span data-ttu-id="565dd-150">A feladatátvételi feladatban jön létre.</span><span class="sxs-lookup"><span data-stu-id="565dd-150">A failover job is created.</span></span> <span data-ttu-id="565dd-151">A feladatátvételi feladatban figyeléséhez, kattintson a feladat értesítésre.</span><span class="sxs-lookup"><span data-stu-id="565dd-151">To monitor the failover job, click the job notification.</span></span>

    ![A figyelő feladatátvételi feladatban](./media/storsimple-8000-device-failover-disaster-recovery/failover-phy-dev13.png)

10. <span data-ttu-id="565dd-153">A feladatátvétel elvégzése után térjen vissza a **eszközök** panelen.</span><span class="sxs-lookup"><span data-stu-id="565dd-153">After the failover is completed, go back to the **Devices** blade.</span></span>

    1. <span data-ttu-id="565dd-154">Válassza ki az eszközt, a feladatátvétel céljaként használt.</span><span class="sxs-lookup"><span data-stu-id="565dd-154">Select the device that was used as the target for the failover.</span></span>

       ![Válassza ki az eszköz](./media/storsimple-8000-device-failover-disaster-recovery/failover-phy-dev14.png)

    2. <span data-ttu-id="565dd-156">Kattintson a **Kötettárolók**.</span><span class="sxs-lookup"><span data-stu-id="565dd-156">Click **Volume Containers**.</span></span> <span data-ttu-id="565dd-157">Minden kötettárolók, a köteteket a régi eszközről együtt kell szerepelnie.</span><span class="sxs-lookup"><span data-stu-id="565dd-157">All the volume containers, along with the volumes from the old device, should be listed.</span></span>

       <span data-ttu-id="565dd-158">Ha a kötet tároló, amely akkor feladatátvételt rendelkezik helyileg rögzített kötetek, a köteteket feladatátvétel történt rétegzett kötetek.</span><span class="sxs-lookup"><span data-stu-id="565dd-158">If the volume container that you failed over has locally pinned volumes, those volumes are failed over as tiered volumes.</span></span> <span data-ttu-id="565dd-159">Helyileg rögzített kötetek nem támogatottak a StorSimple-felhő készüléken.</span><span class="sxs-lookup"><span data-stu-id="565dd-159">Locally pinned volumes are not supported on a StorSimple Cloud Appliance.</span></span>

       ![Nézet target kötettárolók](./media/storsimple-8000-device-failover-disaster-recovery/failover-phy-dev17.png)


## <a name="next-steps"></a><span data-ttu-id="565dd-161">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="565dd-161">Next steps</span></span>

* <span data-ttu-id="565dd-162">Miután elvégezte a feladatátvételt, esetleg [inaktiválja vagy törölje a StorSimple eszköz](storsimple-8000-deactivate-and-delete-device.md).</span><span class="sxs-lookup"><span data-stu-id="565dd-162">After you have performed a failover, you may need to [deactivate or delete your StorSimple device](storsimple-8000-deactivate-and-delete-device.md).</span></span>

* <span data-ttu-id="565dd-163">A StorSimple Device Manager szolgáltatás használatával kapcsolatos információkért látogasson el [felügyelete a StorSimple eszközt a StorSimple Device Manager szolgáltatással](storsimple-8000-manager-service-administration.md).</span><span class="sxs-lookup"><span data-stu-id="565dd-163">For information about how to use the StorSimple Device Manager service, go to [Use the StorSimple Device Manager service to administer your StorSimple device](storsimple-8000-manager-service-administration.md).</span></span>

