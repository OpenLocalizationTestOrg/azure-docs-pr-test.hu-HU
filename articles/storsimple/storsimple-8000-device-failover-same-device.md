---
title: "A feladatátvétel a StorSimple 8000 sorozat eszközeire vész-helyreállítási |} Microsoft Docs"
description: "Útmutató: a StorSimple eszköz ugyanarra az eszközre a feladatátvétel."
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
ms.date: 06/23/2017
ms.author: alkohli
ms.openlocfilehash: acc8929dc3476e9590e8e4d9526b38b7c0719570
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="fail-over-your-storsimple-physical-device-to-same-device"></a><span data-ttu-id="a6bb4-103">A StorSimple fizikai eszköz ugyanarra az eszközre történő feladatátvételt</span><span class="sxs-lookup"><span data-stu-id="a6bb4-103">Fail over your StorSimple physical device to same device</span></span>

## <a name="overview"></a><span data-ttu-id="a6bb4-104">Áttekintés</span><span class="sxs-lookup"><span data-stu-id="a6bb4-104">Overview</span></span>

<span data-ttu-id="a6bb4-105">Ez az oktatóanyag a feladatátvételt a StorSimple 8000 series fizikai az eszköz magát katasztrófa esetén szükséges lépéseket ismerteti.</span><span class="sxs-lookup"><span data-stu-id="a6bb4-105">This tutorial describes the steps required to fail over a StorSimple 8000 series physical device to itself if there is a disaster.</span></span> <span data-ttu-id="a6bb4-106">StorSimple eszköz feladatátvételi szolgáltatását használja egy másik fizikai eszközre át adatokat a forrás fizikai eszközt az adatközpontban.</span><span class="sxs-lookup"><span data-stu-id="a6bb4-106">StorSimple uses the device failover feature to migrate data from a source physical device in the datacenter to another physical device.</span></span> <span data-ttu-id="a6bb4-107">Ez az oktatóanyag az útmutató a StorSimple 8000 Series érvényes 3 és újabb verzióit szoftververziók frissítést futtató fizikai eszközöket.</span><span class="sxs-lookup"><span data-stu-id="a6bb4-107">The guidance in this tutorial applies to StorSimple 8000 series physical devices running software versions Update 3 and later.</span></span>

<span data-ttu-id="a6bb4-108">Eszköz feladatátvételi, és hogyan használja a katasztrófa utáni helyreállítás kapcsolatos további tudnivalókért keresse fel [feladatátvétel és a katasztrófa utáni helyreállítás a StorSimple 8000 sorozat eszközeire](storsimple-8000-device-failover-disaster-recovery.md).</span><span class="sxs-lookup"><span data-stu-id="a6bb4-108">To learn more about device failover and how it is used to recover from a disaster, go to [Failover and disaster recovery for StorSimple 8000 series devices](storsimple-8000-device-failover-disaster-recovery.md).</span></span>

<span data-ttu-id="a6bb4-109">Feladatok átadása egy másik fizikai eszköz fizikai eszköz, keresse fel [átveheti az ugyanazon fizikai StorSimple-eszköz](storsimple-8000-device-failover-physical-device.md).</span><span class="sxs-lookup"><span data-stu-id="a6bb4-109">To fail over a physical device to another physical device, go to [Fail over to the same StorSimple physical device](storsimple-8000-device-failover-physical-device.md).</span></span> <span data-ttu-id="a6bb4-110">Feladatok átadása a StorSimple felhő készülék StorSimple fizikai eszköz, keresse fel [feladatok átadása a StorSimple felhő készülék](storsimple-8000-device-failover-cloud-appliance.md).</span><span class="sxs-lookup"><span data-stu-id="a6bb4-110">To fail over a StorSimple physical device to a StorSimple Cloud Appliance, go to [Fail over to a StorSimple Cloud Appliance](storsimple-8000-device-failover-cloud-appliance.md).</span></span>


## <a name="prerequisites"></a><span data-ttu-id="a6bb4-111">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="a6bb4-111">Prerequisites</span></span>

- <span data-ttu-id="a6bb4-112">Győződjön meg arról, hogy átolvasta eszköz feladatátvételi szempontokat.</span><span class="sxs-lookup"><span data-stu-id="a6bb4-112">Ensure that you have reviewed the considerations for device failover.</span></span> <span data-ttu-id="a6bb4-113">További információkért látogasson el [eszköz feladatátvételi a gyakori szempontok](storsimple-8000-device-failover-disaster-recovery.md).</span><span class="sxs-lookup"><span data-stu-id="a6bb4-113">For more information, go to [Common considerations for device failover](storsimple-8000-device-failover-disaster-recovery.md).</span></span>


## <a name="steps-to-fail-over-to-the-same-device"></a><span data-ttu-id="a6bb4-114">Feladatok átadása a ugyanarra az eszközre lépései</span><span class="sxs-lookup"><span data-stu-id="a6bb4-114">Steps to fail over to the same device</span></span>

<span data-ttu-id="a6bb4-115">Ha feladatátvételt ugyanarra az eszközre van szüksége, hajtsa végre az alábbi lépéseket.</span><span class="sxs-lookup"><span data-stu-id="a6bb4-115">Perform the following steps if you need to fail over to the same device.</span></span>

1. <span data-ttu-id="a6bb4-116">A kötetek felhőalapú pillanatfelvételek az eszközt a hálózatról.</span><span class="sxs-lookup"><span data-stu-id="a6bb4-116">Take cloud snapshots of all the volumes in your device.</span></span> <span data-ttu-id="a6bb4-117">További információkért látogasson el [StorSimple az Eszközkezelő szolgáltatás a biztonsági mentések létrehozását](storsimple-8000-manage-backup-policies-u2.md).</span><span class="sxs-lookup"><span data-stu-id="a6bb4-117">For more information, go to [Use StorSimple Device Manager service to create backups](storsimple-8000-manage-backup-policies-u2.md).</span></span>
2. <span data-ttu-id="a6bb4-118">Az eszköz visszaállítása a gyári beállításokat.</span><span class="sxs-lookup"><span data-stu-id="a6bb4-118">Reset your device to factory defaults.</span></span> <span data-ttu-id="a6bb4-119">Részletes utasításait [a StorSimple eszköz visszaállítása a gyári alapértelmezett beállításokra](storsimple-8000-manage-device-controller.md#reset-the-device-to-factory-default-settings).</span><span class="sxs-lookup"><span data-stu-id="a6bb4-119">Follow the detailed instructions in [how to reset a StorSimple device to factory default settings](storsimple-8000-manage-device-controller.md#reset-the-device-to-factory-default-settings).</span></span>
3. <span data-ttu-id="a6bb4-120">Nyissa meg a StorSimple eszköz Manager szolgáltatáshoz, és válassza ki **eszközök**.</span><span class="sxs-lookup"><span data-stu-id="a6bb4-120">Go to the StorSimple Device Manager service and then select **Devices**.</span></span> <span data-ttu-id="a6bb4-121">Az a **eszközök** panelen jelennek meg a régi eszköz **Offline**.</span><span class="sxs-lookup"><span data-stu-id="a6bb4-121">In the **Devices** blade, the old device should show as **Offline**.</span></span>

    ![Forrás-eszköz kapcsolat nélküli módban](./media/storsimple-8000-device-failover-disaster-recovery/failover-single-dev2.png)

4. <span data-ttu-id="a6bb4-123">Állítsa be az eszközt, és regisztrálja újra a StorSimple eszköz Manager szolgáltatásban.</span><span class="sxs-lookup"><span data-stu-id="a6bb4-123">Configure your device and register it again with your StorSimple Device Manager service.</span></span> <span data-ttu-id="a6bb4-124">Az újonnan regisztrált eszközre kell megjeleníteni **már beállíthat**.</span><span class="sxs-lookup"><span data-stu-id="a6bb4-124">The newly registered device should show as **Ready to set up**.</span></span> <span data-ttu-id="a6bb4-125">Az eszköz nevét az új eszköz ugyanaz, mint a régi eszköz, azonban kiegészül a számot jelzi, hogy az eszköz gyári Alapértelmezések visszaállítása, és újra regisztrálni.</span><span class="sxs-lookup"><span data-stu-id="a6bb4-125">The device name for the new device is the same as the old device but appended with a numeral to indicate that the device was reset to factory default and registered again.</span></span>

    ![Újonnan regisztrált eszköz beállítása kész](./media/storsimple-8000-device-failover-disaster-recovery/failover-single-dev3.png)
5. <span data-ttu-id="a6bb4-127">Az új eszközhöz az eszköz beállításának befejezése.</span><span class="sxs-lookup"><span data-stu-id="a6bb4-127">For the new device, complete the device setup.</span></span> <span data-ttu-id="a6bb4-128">További információkért látogasson el [4. lépés: minimális eszközbeállítások végrehajtása](storsimple-8000-deployment-walkthrough-u2.md#step-4-complete-minimum-device-setup).</span><span class="sxs-lookup"><span data-stu-id="a6bb4-128">For more information, go to [Step 4: Complete minimum device setup](storsimple-8000-deployment-walkthrough-u2.md#step-4-complete-minimum-device-setup).</span></span> <span data-ttu-id="a6bb4-129">Az a **eszközök** panel, az eszköz állapota a **Online**.</span><span class="sxs-lookup"><span data-stu-id="a6bb4-129">On the **Devices** blade, the status of the device changes to **Online**.</span></span>

   > [!IMPORTANT]
   > <span data-ttu-id="a6bb4-130">**Először végezze el a minimális konfiguráció, vagy a vész-Helyreállítási sikertelenek lehetnek.**</span><span class="sxs-lookup"><span data-stu-id="a6bb4-130">**Complete the minimum configuration first, or your DR may fail.**</span></span>

    ![Online az újonnan regisztrált eszközre](./media/storsimple-8000-device-failover-disaster-recovery/failover-single-dev7.png)

6. <span data-ttu-id="a6bb4-132">Válassza ki azt a régi eszközt (kapcsolat nélküli állapota), majd kattintson a parancssávon **feladatátvételt**.</span><span class="sxs-lookup"><span data-stu-id="a6bb4-132">Select the old device (status offline) and from the command bar, click **Fail over**.</span></span> <span data-ttu-id="a6bb4-133">Az a **feladatátvételt** panelen válassza ki azt a régi eszközt forrásként, és adja meg a céleszközt az újonnan regisztrált eszköz.</span><span class="sxs-lookup"><span data-stu-id="a6bb4-133">In the **Fail over** blade, select old device as the source and specify the target device as the newly registered device.</span></span>

    ![Feladatátvételi összegzése](./media/storsimple-8000-device-failover-disaster-recovery/failover-single-dev11.png)

    <span data-ttu-id="a6bb4-135">Részletes útmutatásért tekintse meg [átveheti egy másik fizikai eszköz](#fail-over-to-another-physical-device).</span><span class="sxs-lookup"><span data-stu-id="a6bb4-135">For detailed instructions, refer to [Fail over to another physical device](#fail-over-to-another-physical-device).</span></span>

7. <span data-ttu-id="a6bb4-136">Egy eszköz-visszaállítási feladat jön létre, hogy a figyelheti a **feladatok** panelen.</span><span class="sxs-lookup"><span data-stu-id="a6bb4-136">A device restore job is created that you can monitor from the **Jobs** blade.</span></span>

8. <span data-ttu-id="a6bb4-137">Miután a feladat sikeresen befejeződött, az új eszközhöz való hozzáféréshez, és keresse meg a **kötettárolók** panelen.</span><span class="sxs-lookup"><span data-stu-id="a6bb4-137">After the job has successfully completed, access the new device and navigate to the **Volume containers** blade.</span></span> <span data-ttu-id="a6bb4-138">Győződjön meg arról, hogy a régi eszközről kötettárolók átvitelét az új eszköz.</span><span class="sxs-lookup"><span data-stu-id="a6bb4-138">Verify that all the volume containers from the old device have migrated to the new device.</span></span>

   ![Kötettárolók áttelepítése](./media/storsimple-8000-device-failover-disaster-recovery/failover-single-dev13.png)

9. <span data-ttu-id="a6bb4-140">A feladatátvétel befejezése után inaktiválása, és törli a régi eszközt a portálról.</span><span class="sxs-lookup"><span data-stu-id="a6bb4-140">After the failover is complete, you can deactivate and delete the old device from the portal.</span></span> <span data-ttu-id="a6bb4-141">A régi eszköz (offline), kattintson a jobb gombbal, majd válassza ki és **Deactivate**.</span><span class="sxs-lookup"><span data-stu-id="a6bb4-141">Select the old device (offline), right-click, and then select **Deactivate**.</span></span> <span data-ttu-id="a6bb4-142">Az eszközt az Inaktiválás után az eszköz állapota frissül.</span><span class="sxs-lookup"><span data-stu-id="a6bb4-142">After the device is deactivated, the status of the device is updated.</span></span>

     ![Forrás eszköz inaktiválása](./media/storsimple-8000-device-failover-disaster-recovery/failover-single-dev14.png)

10. <span data-ttu-id="a6bb4-144">Válassza ki a deaktivált eszközt, kattintson a jobb gombbal, és válassza **törlése**.</span><span class="sxs-lookup"><span data-stu-id="a6bb4-144">Select the deactivated device, right-click, and then select **Delete**.</span></span> <span data-ttu-id="a6bb4-145">Ez törli az eszközt az eszközök listája.</span><span class="sxs-lookup"><span data-stu-id="a6bb4-145">This deletes the device from the list of devices.</span></span>

    ![Forrás eszköz törlése](./media/storsimple-8000-device-failover-disaster-recovery/failover-single-dev15.png)



## <a name="next-steps"></a><span data-ttu-id="a6bb4-147">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="a6bb4-147">Next steps</span></span>

* <span data-ttu-id="a6bb4-148">Miután elvégezte a feladatátvételt, esetleg [inaktiválja vagy törölje a StorSimple eszköz](storsimple-8000-deactivate-and-delete-device.md).</span><span class="sxs-lookup"><span data-stu-id="a6bb4-148">After you have performed a failover, you may need to [deactivate or delete your StorSimple device](storsimple-8000-deactivate-and-delete-device.md).</span></span>
* <span data-ttu-id="a6bb4-149">A StorSimple Device Manager szolgáltatás használatával kapcsolatos információkért látogasson el [felügyelete a StorSimple eszközt a StorSimple Device Manager szolgáltatással](storsimple-8000-manager-service-administration.md).</span><span class="sxs-lookup"><span data-stu-id="a6bb4-149">For information about how to use the StorSimple Device Manager service, go to [Use the StorSimple Device Manager service to administer your StorSimple device](storsimple-8000-manager-service-administration.md).</span></span>

