---
title: "a feladatátvétel aaaStorSimple 8000 sorozat eszközeire vész-helyreállítási |} Microsoft Docs"
description: "Ismerje meg, hogy a StorSimple eszköz toohello keresztül toofail ugyanarra az eszközre."
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
ms.openlocfilehash: b0b4216c7af6745ff68b85ca3d655691b43b4334
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="fail-over-your-storsimple-physical-device-toosame-device"></a><span data-ttu-id="b4ac7-103">Feladatok átadása a StorSimple fizikai eszköz toosame eszköz</span><span class="sxs-lookup"><span data-stu-id="b4ac7-103">Fail over your StorSimple physical device toosame device</span></span>

## <a name="overview"></a><span data-ttu-id="b4ac7-104">Áttekintés</span><span class="sxs-lookup"><span data-stu-id="b4ac7-104">Overview</span></span>

<span data-ttu-id="b4ac7-105">Ez az oktatóanyag leírja a StorSimple 8000 series fizikai eszköz tooitself keresztül hello lépéseket szükséges toofail, ha egy olyan vészhelyzet esetén.</span><span class="sxs-lookup"><span data-stu-id="b4ac7-105">This tutorial describes hello steps required toofail over a StorSimple 8000 series physical device tooitself if there is a disaster.</span></span> <span data-ttu-id="b4ac7-106">StorSimple hello eszköz feladatátvételi szolgáltatás toomigrate adatait használja a forrás fizikai eszköz hello datacenter tooanother fizikai eszköz.</span><span class="sxs-lookup"><span data-stu-id="b4ac7-106">StorSimple uses hello device failover feature toomigrate data from a source physical device in hello datacenter tooanother physical device.</span></span> <span data-ttu-id="b4ac7-107">Ebben az oktatóanyagban hello útmutatást tooStorSimple 8000 sorozat fizikai eszközök 3 és újabb verzióit futtató a szoftver verziója frissítés vonatkozik.</span><span class="sxs-lookup"><span data-stu-id="b4ac7-107">hello guidance in this tutorial applies tooStorSimple 8000 series physical devices running software versions Update 3 and later.</span></span>

<span data-ttu-id="b4ac7-108">További információ az eszköz feladatátvételi, és hogyan egy olyan vészhelyzet esetén, a használt toorecover toolearn lépjen túl[feladatátvétel és a katasztrófa utáni helyreállítás a StorSimple 8000 sorozat eszközeire](storsimple-8000-device-failover-disaster-recovery.md).</span><span class="sxs-lookup"><span data-stu-id="b4ac7-108">toolearn more about device failover and how it is used toorecover from a disaster, go too[Failover and disaster recovery for StorSimple 8000 series devices](storsimple-8000-device-failover-disaster-recovery.md).</span></span>

<span data-ttu-id="b4ac7-109">egy fizikai eszköz tooanother fizikai eszközön keresztül toofail lépjen túl[feladatátvételt toohello azonos StorSimple fizikai eszköz](storsimple-8000-device-failover-physical-device.md).</span><span class="sxs-lookup"><span data-stu-id="b4ac7-109">toofail over a physical device tooanother physical device, go too[Fail over toohello same StorSimple physical device](storsimple-8000-device-failover-physical-device.md).</span></span> <span data-ttu-id="b4ac7-110">túl nyissa meg a StorSimple fizikai eszköz tooa StorSimple felhő készülék keresztül toofail[tooa StorSimple felhő készülék feladatátvételt](storsimple-8000-device-failover-cloud-appliance.md).</span><span class="sxs-lookup"><span data-stu-id="b4ac7-110">toofail over a StorSimple physical device tooa StorSimple Cloud Appliance, go too[Fail over tooa StorSimple Cloud Appliance](storsimple-8000-device-failover-cloud-appliance.md).</span></span>


## <a name="prerequisites"></a><span data-ttu-id="b4ac7-111">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="b4ac7-111">Prerequisites</span></span>

- <span data-ttu-id="b4ac7-112">Győződjön meg arról, hogy átolvasta eszköz feladatátvételi hello szempontjai.</span><span class="sxs-lookup"><span data-stu-id="b4ac7-112">Ensure that you have reviewed hello considerations for device failover.</span></span> <span data-ttu-id="b4ac7-113">További információ: túl[eszköz feladatátvételi a gyakori szempontok](storsimple-8000-device-failover-disaster-recovery.md).</span><span class="sxs-lookup"><span data-stu-id="b4ac7-113">For more information, go too[Common considerations for device failover](storsimple-8000-device-failover-disaster-recovery.md).</span></span>


## <a name="steps-toofail-over-toohello-same-device"></a><span data-ttu-id="b4ac7-114">Lépéseket toofail keresztül toohello ugyanarra az eszközre</span><span class="sxs-lookup"><span data-stu-id="b4ac7-114">Steps toofail over toohello same device</span></span>

<span data-ttu-id="b4ac7-115">Hajtsa végre a következő lépéseket, ha toofail toohello keresztül kell hello ugyanarra az eszközre.</span><span class="sxs-lookup"><span data-stu-id="b4ac7-115">Perform hello following steps if you need toofail over toohello same device.</span></span>

1. <span data-ttu-id="b4ac7-116">Az összes hello kötet felhőalapú pillanatfelvételek az eszközt a hálózatról.</span><span class="sxs-lookup"><span data-stu-id="b4ac7-116">Take cloud snapshots of all hello volumes in your device.</span></span> <span data-ttu-id="b4ac7-117">További információ: túl[StorSimple az Eszközkezelő szolgáltatás toocreate biztonsági mentések](storsimple-8000-manage-backup-policies-u2.md).</span><span class="sxs-lookup"><span data-stu-id="b4ac7-117">For more information, go too[Use StorSimple Device Manager service toocreate backups](storsimple-8000-manage-backup-policies-u2.md).</span></span>
2. <span data-ttu-id="b4ac7-118">Az eszköz toofactory Alapértelmezések visszaállítása.</span><span class="sxs-lookup"><span data-stu-id="b4ac7-118">Reset your device toofactory defaults.</span></span> <span data-ttu-id="b4ac7-119">Hajtsa végre a hello részletes utasításait [hogyan tooreset a StorSimple eszköz toofactory alapértelmezett beállítások](storsimple-8000-manage-device-controller.md#reset-the-device-to-factory-default-settings).</span><span class="sxs-lookup"><span data-stu-id="b4ac7-119">Follow hello detailed instructions in [how tooreset a StorSimple device toofactory default settings](storsimple-8000-manage-device-controller.md#reset-the-device-to-factory-default-settings).</span></span>
3. <span data-ttu-id="b4ac7-120">Nyissa meg toohello StorSimple Device Manager szolgáltatást, és válassza ki **eszközök**.</span><span class="sxs-lookup"><span data-stu-id="b4ac7-120">Go toohello StorSimple Device Manager service and then select **Devices**.</span></span> <span data-ttu-id="b4ac7-121">A hello **eszközök** panelen jelennek meg hello régi eszköz **Offline**.</span><span class="sxs-lookup"><span data-stu-id="b4ac7-121">In hello **Devices** blade, hello old device should show as **Offline**.</span></span>

    ![Forrás-eszköz kapcsolat nélküli módban](./media/storsimple-8000-device-failover-disaster-recovery/failover-single-dev2.png)

4. <span data-ttu-id="b4ac7-123">Állítsa be az eszközt, és regisztrálja újra a StorSimple eszköz Manager szolgáltatásban.</span><span class="sxs-lookup"><span data-stu-id="b4ac7-123">Configure your device and register it again with your StorSimple Device Manager service.</span></span> <span data-ttu-id="b4ac7-124">hello újonnan regisztrált eszközre kell megjeleníteni **tooset kész mentése**.</span><span class="sxs-lookup"><span data-stu-id="b4ac7-124">hello newly registered device should show as **Ready tooset up**.</span></span> <span data-ttu-id="b4ac7-125">hello eszköznevet hello új eszköz hello ugyanaz, mint a régi eszköz hello azonban kiegészül a számok tooindicate hello eszköz alaphelyzetbe állítása toofactory alapértelmezett volt, és újra regisztrálva.</span><span class="sxs-lookup"><span data-stu-id="b4ac7-125">hello device name for hello new device is hello same as hello old device but appended with a numeral tooindicate that hello device was reset toofactory default and registered again.</span></span>

    ![Újonnan regisztrált eszközre készen tooset mentése](./media/storsimple-8000-device-failover-disaster-recovery/failover-single-dev3.png)
5. <span data-ttu-id="b4ac7-127">Hello új eszközhöz hello eszköz beállításának befejezése.</span><span class="sxs-lookup"><span data-stu-id="b4ac7-127">For hello new device, complete hello device setup.</span></span> <span data-ttu-id="b4ac7-128">További információ: túl[4. lépés: minimális eszközbeállítások végrehajtása](storsimple-8000-deployment-walkthrough-u2.md#step-4-complete-minimum-device-setup).</span><span class="sxs-lookup"><span data-stu-id="b4ac7-128">For more information, go too[Step 4: Complete minimum device setup](storsimple-8000-deployment-walkthrough-u2.md#step-4-complete-minimum-device-setup).</span></span> <span data-ttu-id="b4ac7-129">A hello **eszközök** panelen hello hello eszköz állapota túl**Online**.</span><span class="sxs-lookup"><span data-stu-id="b4ac7-129">On hello **Devices** blade, hello status of hello device changes too**Online**.</span></span>

   > [!IMPORTANT]
   > <span data-ttu-id="b4ac7-130">**Hello minimális konfiguráció befejezéséhez először, vagy a vész-Helyreállítási sikertelenek lehetnek.**</span><span class="sxs-lookup"><span data-stu-id="b4ac7-130">**Complete hello minimum configuration first, or your DR may fail.**</span></span>

    ![Online az újonnan regisztrált eszközre](./media/storsimple-8000-device-failover-disaster-recovery/failover-single-dev7.png)

6. <span data-ttu-id="b4ac7-132">Válassza ki a régi eszköz hello (kapcsolat nélküli állapota) és hello parancssávon kattintson **feladatátvételt**.</span><span class="sxs-lookup"><span data-stu-id="b4ac7-132">Select hello old device (status offline) and from hello command bar, click **Fail over**.</span></span> <span data-ttu-id="b4ac7-133">A hello **feladatátvételt** panelen válassza ki azt a régi eszközt hello forrásként, és adja meg a hello céleszközt, hello újonnan regisztrált eszköz.</span><span class="sxs-lookup"><span data-stu-id="b4ac7-133">In hello **Fail over** blade, select old device as hello source and specify hello target device as hello newly registered device.</span></span>

    ![Feladatátvételi összegzése](./media/storsimple-8000-device-failover-disaster-recovery/failover-single-dev11.png)

    <span data-ttu-id="b4ac7-135">Részletes útmutatásért lásd túl[tooanother fizikai eszköz feladatátvételt](#fail-over-to-another-physical-device).</span><span class="sxs-lookup"><span data-stu-id="b4ac7-135">For detailed instructions, refer too[Fail over tooanother physical device](#fail-over-to-another-physical-device).</span></span>

7. <span data-ttu-id="b4ac7-136">Egy eszköz visszaállítási feladat jön létre, hogy hello a figyelheti **feladatok** panelen.</span><span class="sxs-lookup"><span data-stu-id="b4ac7-136">A device restore job is created that you can monitor from hello **Jobs** blade.</span></span>

8. <span data-ttu-id="b4ac7-137">Miután hello feladat sikeresen befejeződött, hello új eszközhöz való hozzáféréshez, és keresse meg a toohello **kötettárolók** panelen.</span><span class="sxs-lookup"><span data-stu-id="b4ac7-137">After hello job has successfully completed, access hello new device and navigate toohello **Volume containers** blade.</span></span> <span data-ttu-id="b4ac7-138">Győződjön meg arról, hogy minden hello kötettárolók hello régi eszközről áttelepített toohello új eszköz.</span><span class="sxs-lookup"><span data-stu-id="b4ac7-138">Verify that all hello volume containers from hello old device have migrated toohello new device.</span></span>

   ![Kötettárolók áttelepítése](./media/storsimple-8000-device-failover-disaster-recovery/failover-single-dev13.png)

9. <span data-ttu-id="b4ac7-140">Hello feladatátvételi befejeződése után inaktiválása, és törli a régi eszköz hello hello portálról.</span><span class="sxs-lookup"><span data-stu-id="b4ac7-140">After hello failover is complete, you can deactivate and delete hello old device from hello portal.</span></span> <span data-ttu-id="b4ac7-141">Hello régi eszköz (offline), kattintson a jobb gombbal, majd válassza ki és **Deactivate**.</span><span class="sxs-lookup"><span data-stu-id="b4ac7-141">Select hello old device (offline), right-click, and then select **Deactivate**.</span></span> <span data-ttu-id="b4ac7-142">Hello eszközt az Inaktiválás után hello eszköz hello állapota frissül.</span><span class="sxs-lookup"><span data-stu-id="b4ac7-142">After hello device is deactivated, hello status of hello device is updated.</span></span>

     ![Forrás eszköz inaktiválása](./media/storsimple-8000-device-failover-disaster-recovery/failover-single-dev14.png)

10. <span data-ttu-id="b4ac7-144">Jelölje be hello inaktiválása eszközt, kattintson a jobb gombbal, és válassza **törlése**.</span><span class="sxs-lookup"><span data-stu-id="b4ac7-144">Select hello deactivated device, right-click, and then select **Delete**.</span></span> <span data-ttu-id="b4ac7-145">Hello eszköz törlése hello eszközök listáját.</span><span class="sxs-lookup"><span data-stu-id="b4ac7-145">This deletes hello device from hello list of devices.</span></span>

    ![Forrás eszköz törlése](./media/storsimple-8000-device-failover-disaster-recovery/failover-single-dev15.png)



## <a name="next-steps"></a><span data-ttu-id="b4ac7-147">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="b4ac7-147">Next steps</span></span>

* <span data-ttu-id="b4ac7-148">Miután elvégezte a feladatátvételt, esetleg túl[inaktiválja vagy törölje a StorSimple eszköz](storsimple-8000-deactivate-and-delete-device.md).</span><span class="sxs-lookup"><span data-stu-id="b4ac7-148">After you have performed a failover, you may need too[deactivate or delete your StorSimple device](storsimple-8000-deactivate-and-delete-device.md).</span></span>
* <span data-ttu-id="b4ac7-149">Hogyan toouse hello StorSimple Device Manager információ szolgáltatás, nyissa meg túl[használata hello StorSimple Device Manager szolgáltatás tooadminister a StorSimple eszköz](storsimple-8000-manager-service-administration.md).</span><span class="sxs-lookup"><span data-stu-id="b4ac7-149">For information about how toouse hello StorSimple Device Manager service, go too[Use hello StorSimple Device Manager service tooadminister your StorSimple device](storsimple-8000-manager-service-administration.md).</span></span>

