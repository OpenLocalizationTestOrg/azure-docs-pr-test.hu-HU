---
title: "aaaStorSimple feladatátvétel, vagy vész-helyreállítási tooa StorSimple felhő készülék |} Microsoft Docs"
description: "Ismerje meg, hogyan toofail keresztül a StorSimple 8000 series fizikai eszköz tooa felhőalapú készüléket."
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
ms.openlocfilehash: e8a0bca057024358e3a557fe85a42ddefea36cff
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="fail-over-tooyour-storsimple-cloud-appliance"></a><span data-ttu-id="76680-103">Feladatok átadása tooyour StorSimple felhő készülék</span><span class="sxs-lookup"><span data-stu-id="76680-103">Fail over tooyour StorSimple Cloud Appliance</span></span>

## <a name="overview"></a><span data-ttu-id="76680-104">Áttekintés</span><span class="sxs-lookup"><span data-stu-id="76680-104">Overview</span></span>

<span data-ttu-id="76680-105">Ez az oktatóanyag leírja a StorSimple 8000 series fizikai eszköz tooa StorSimple felhő készülék keresztül hello lépéseket szükséges toofail, ha van egy olyan vészhelyzet esetén.</span><span class="sxs-lookup"><span data-stu-id="76680-105">This tutorial describes hello steps required toofail over a StorSimple 8000 series physical device tooa StorSimple Cloud Appliance if there is a disaster.</span></span> <span data-ttu-id="76680-106">StorSimple hello datacenter tooa felhő készülék Azure-beli hello eszköz feladatátvételi szolgáltatás toomigrate adatait a forrás fizikai eszközről használja.</span><span class="sxs-lookup"><span data-stu-id="76680-106">StorSimple uses hello device failover feature toomigrate data from a source physical device in hello datacenter tooa cloud appliance running in Azure.</span></span> <span data-ttu-id="76680-107">Ebben az oktatóanyagban hello útmutatást tooStorSimple 8000 sorozat fizikai eszközöket és a felhő készülékek 3 és újabb verzióit futtató a szoftver verziója frissítés vonatkozik.</span><span class="sxs-lookup"><span data-stu-id="76680-107">hello guidance in this tutorial applies tooStorSimple 8000 series physical devices and cloud appliances running software versions Update 3 and later.</span></span>

<span data-ttu-id="76680-108">További információ az eszköz feladatátvételi, és hogyan egy olyan vészhelyzet esetén, a használt toorecover toolearn lépjen túl[feladatátvétel és a katasztrófa utáni helyreállítás a StorSimple 8000 sorozat eszközeire](storsimple-8000-device-failover-disaster-recovery.md).</span><span class="sxs-lookup"><span data-stu-id="76680-108">toolearn more about device failover and how it is used toorecover from a disaster, go too[Failover and disaster recovery for StorSimple 8000 series devices](storsimple-8000-device-failover-disaster-recovery.md).</span></span>

<span data-ttu-id="76680-109">túl nyissa meg a StorSimple fizikai eszköz tooanother fizikai eszközön keresztül toofail[tooa StorSimple fizikai eszköz feladatátvételt](storsimple-8000-device-failover-physical-device.md).</span><span class="sxs-lookup"><span data-stu-id="76680-109">toofail over a StorSimple physical device tooanother physical device, go too[Fail over tooa StorSimple physical device](storsimple-8000-device-failover-physical-device.md).</span></span> <span data-ttu-id="76680-110">egy eszköz tooitself keresztül toofail lépjen túl[feladatátvételt toohello azonos StorSimple fizikai eszköz](storsimple-8000-device-failover-same-device.md).</span><span class="sxs-lookup"><span data-stu-id="76680-110">toofail over a device tooitself, go too[Fail over toohello same StorSimple physical device](storsimple-8000-device-failover-same-device.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="76680-111">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="76680-111">Prerequisites</span></span>

- <span data-ttu-id="76680-112">Győződjön meg arról, hogy átolvasta eszköz feladatátvételi hello szempontjai.</span><span class="sxs-lookup"><span data-stu-id="76680-112">Ensure that you have reviewed hello considerations for device failover.</span></span> <span data-ttu-id="76680-113">További információ: túl[eszköz feladatátvételi a gyakori szempontok](storsimple-8000-device-failover-disaster-recovery.md).</span><span class="sxs-lookup"><span data-stu-id="76680-113">For more information, go too[Common considerations for device failover](storsimple-8000-device-failover-disaster-recovery.md).</span></span>

- <span data-ttu-id="76680-114">A StorSimple felhő készülék létrehozása és konfigurálása a művelet futtatása előtt kell rendelkeznie.</span><span class="sxs-lookup"><span data-stu-id="76680-114">You must have a StorSimple Cloud Appliance created and configured before running this procedure.</span></span> <span data-ttu-id="76680-115">Ha a futó 3 ügyfélszoftverének verziójára frissítése, vagy később, érdemes lehet a 8020-as modellt felhő készülék hello vész-Helyreállítási.</span><span class="sxs-lookup"><span data-stu-id="76680-115">If running   Update 3 software version or later, consider using an 8020 cloud appliance for hello DR.</span></span> <span data-ttu-id="76680-116">hello 8020 modell 64 TB-os rendelkezik, és a prémium szintű Storage használja.</span><span class="sxs-lookup"><span data-stu-id="76680-116">hello 8020 model has 64 TB and uses Premium Storage.</span></span> <span data-ttu-id="76680-117">További információ: túl[központi telepítése és kezelése a StorSimple felhő készülék](storsimple-8000-cloud-appliance-u2.md).</span><span class="sxs-lookup"><span data-stu-id="76680-117">For more information, go too[Deploy and manage a StorSimple Cloud Appliance](storsimple-8000-cloud-appliance-u2.md).</span></span>

## <a name="steps-toofail-over-tooa-cloud-appliance"></a><span data-ttu-id="76680-118">Lépéseket toofail tooa felhő készülék keresztül</span><span class="sxs-lookup"><span data-stu-id="76680-118">Steps toofail over tooa cloud appliance</span></span>

<span data-ttu-id="76680-119">Hajtsa végre a következő lépéseket toorestore hello eszköz tooa cél StorSimple felhő készülék hello.</span><span class="sxs-lookup"><span data-stu-id="76680-119">Perform hello following steps toorestore hello device tooa target StorSimple Cloud Appliance.</span></span>

1.  <span data-ttu-id="76680-120">Győződjön meg arról, hogy hello kötettároló toofail kívánt felhőalapú pillanatfelvételek vannak társítva.</span><span class="sxs-lookup"><span data-stu-id="76680-120">Verify that hello volume container you want toofail over has associated cloud snapshots.</span></span> <span data-ttu-id="76680-121">További információ: túl[StorSimple az Eszközkezelő szolgáltatás toocreate biztonsági mentések](storsimple-8000-manage-backup-policies-u2.md).</span><span class="sxs-lookup"><span data-stu-id="76680-121">For more information, go too[Use StorSimple Device Manager service toocreate backups](storsimple-8000-manage-backup-policies-u2.md).</span></span>
2. <span data-ttu-id="76680-122">Nyissa meg tooyour StorSimple Device Manager szolgáltatás és **eszközök**.</span><span class="sxs-lookup"><span data-stu-id="76680-122">Go tooyour StorSimple Device Manager service and click **Devices**.</span></span> <span data-ttu-id="76680-123">A hello **eszközök** panelen, a szolgáltatáshoz kapcsolódó eszközök lépjen toohello listája.</span><span class="sxs-lookup"><span data-stu-id="76680-123">In hello **Devices** blade, go toohello list of devices connected with your service.</span></span>
    <span data-ttu-id="76680-124">![Válassza ki az eszköz](./media/storsimple-8000-device-failover-disaster-recovery/failover-cloud-dev1.png)</span><span class="sxs-lookup"><span data-stu-id="76680-124">![Select device](./media/storsimple-8000-device-failover-disaster-recovery/failover-cloud-dev1.png)</span></span>
3. <span data-ttu-id="76680-125">Válassza ki, és kattintson a forrás-eszközre.</span><span class="sxs-lookup"><span data-stu-id="76680-125">Select and click your source device.</span></span> <span data-ttu-id="76680-126">hello Forráseszköz hello kötettárolók kívánt toofail keresztül.</span><span class="sxs-lookup"><span data-stu-id="76680-126">hello source device has hello volume containers that you want toofail over.</span></span> <span data-ttu-id="76680-127">Nyissa meg túl**beállítások > Kötettárolók**.</span><span class="sxs-lookup"><span data-stu-id="76680-127">Go too**Settings > Volume Containers**.</span></span>

    ![Válassza ki az eszköz](./media/storsimple-8000-device-failover-disaster-recovery/failover-cloud-dev2.png)
    
4. <span data-ttu-id="76680-129">Válassza ki, hogy milyen toofail tooanother eszközön keresztül kötettároló.</span><span class="sxs-lookup"><span data-stu-id="76680-129">Select a volume container that you would like toofail over tooanother device.</span></span> <span data-ttu-id="76680-130">Kattintson a hello kötet tároló toodisplay hello kötetek listáját a tárolóban.</span><span class="sxs-lookup"><span data-stu-id="76680-130">Click hello volume container toodisplay hello list of volumes within this container.</span></span> <span data-ttu-id="76680-131">Válassza ki a kötetet, kattintson a jobb gombbal, majd kattintson **Offline állapotba** tootake hello kötet offline állapotba.</span><span class="sxs-lookup"><span data-stu-id="76680-131">Select a volume, right-click, and click **Take Offline** tootake hello volume offline.</span></span>

    ![Válassza ki az eszköz](./media/storsimple-8000-device-failover-disaster-recovery/failover-cloud-dev5.png)

5. <span data-ttu-id="76680-133">Ismételje meg ezt a folyamatot a hello kötettároló összes hello kötetet.</span><span class="sxs-lookup"><span data-stu-id="76680-133">Repeat this process for all hello volumes in hello volume container.</span></span>

     ![Válassza ki az eszköz](./media/storsimple-8000-device-failover-disaster-recovery/failover-cloud-dev7.png)

6. <span data-ttu-id="76680-135">Ismétlődő hello előző lépést az összes hello kötettárolók milyen toofail tooanother eszközön keresztül.</span><span class="sxs-lookup"><span data-stu-id="76680-135">Repeat hello previous step for all hello volume containers you would like toofail over tooanother device.</span></span>

7. <span data-ttu-id="76680-136">Lépjen vissza toohello **eszközök** panelen.</span><span class="sxs-lookup"><span data-stu-id="76680-136">Go back toohello **Devices** blade.</span></span> <span data-ttu-id="76680-137">Hello parancssávon kattintson **feladatátvételt**.</span><span class="sxs-lookup"><span data-stu-id="76680-137">From hello command bar, click **Fail over**.</span></span>

    ![Kattintson a sikertelen keresztül](./media/storsimple-8000-device-failover-disaster-recovery/failover-cloud-dev8.png)
8. <span data-ttu-id="76680-139">A hello **feladatátvételt** panelen, hajtsa végre az alábbi lépésekkel hello:</span><span class="sxs-lookup"><span data-stu-id="76680-139">In hello **Fail over** blade, perform hello following steps:</span></span>
   
    1. <span data-ttu-id="76680-140">Kattintson a **forrás**.</span><span class="sxs-lookup"><span data-stu-id="76680-140">Click **Source**.</span></span> <span data-ttu-id="76680-141">Válassza ki a hello kötet tárolók toofail keresztül.</span><span class="sxs-lookup"><span data-stu-id="76680-141">Select hello volume containers toofail over.</span></span> <span data-ttu-id="76680-142">**Csak a hozzárendelt felhő pillanatképekkel kötettárolók hello és a kötetek offline állapotban jelennek meg.**</span><span class="sxs-lookup"><span data-stu-id="76680-142">**Only hello volume containers with associated cloud snapshots and offline volumes are displayed.**</span></span>
        <span data-ttu-id="76680-143">![Forrás kiválasztása](./media/storsimple-8000-device-failover-disaster-recovery/failover-cloud-dev11.png)</span><span class="sxs-lookup"><span data-stu-id="76680-143">![Select source](./media/storsimple-8000-device-failover-disaster-recovery/failover-cloud-dev11.png)</span></span>
    2. <span data-ttu-id="76680-144">Kattintson a **cél**.</span><span class="sxs-lookup"><span data-stu-id="76680-144">Click **Target**.</span></span> <span data-ttu-id="76680-145">Válassza ki a cél felhő készülék az elérhető eszközök hello legördülő listából.</span><span class="sxs-lookup"><span data-stu-id="76680-145">Select a target cloud appliance from hello dropdown list of available devices.</span></span> <span data-ttu-id="76680-146">**Csak olyan hello eszközök, amelyeken elegendő kapacitása tooaccommodate forrás kötettárolók hello listában jelennek meg.**</span><span class="sxs-lookup"><span data-stu-id="76680-146">**Only hello devices that have sufficient capacity tooaccommodate source volume containers are displayed in hello list.**</span></span>

        ![Válassza ki a cél](./media/storsimple-8000-device-failover-disaster-recovery/failover-cloud-dev12.png)

    3. <span data-ttu-id="76680-148">Tekintse át a hello feladatátvételi beállításokat **összegzés** válassza ki a hello jelölőnégyzet jelzi, hogy a kijelölt kötettárolók hello kötetek offline állapotban-e.</span><span class="sxs-lookup"><span data-stu-id="76680-148">Review hello failover settings under **Summary** and select hello checkbox indicating that hello volumes in selected volume containers are offline.</span></span> 

        ![Tekintse át a feladatátvételi beállításait](./media/storsimple-8000-device-failover-disaster-recovery/failover-cloud-dev13.png)

9. <span data-ttu-id="76680-150">A feladatátvételi feladatban jön létre.</span><span class="sxs-lookup"><span data-stu-id="76680-150">A failover job is created.</span></span> <span data-ttu-id="76680-151">toomonitor hello feladatátvételi feladat, kattintson a hello feladat értesítés.</span><span class="sxs-lookup"><span data-stu-id="76680-151">toomonitor hello failover job, click hello job notification.</span></span>

    ![A figyelő feladatátvételi feladatban](./media/storsimple-8000-device-failover-disaster-recovery/failover-phy-dev13.png)

10. <span data-ttu-id="76680-153">Hello feladatátvétel elvégzése után térjen vissza toohello **eszközök** panelen.</span><span class="sxs-lookup"><span data-stu-id="76680-153">After hello failover is completed, go back toohello **Devices** blade.</span></span>

    1. <span data-ttu-id="76680-154">Válassza ki a hello feladatátvételi hello célként használt hello eszközt.</span><span class="sxs-lookup"><span data-stu-id="76680-154">Select hello device that was used as hello target for hello failover.</span></span>

       ![Válassza ki az eszköz](./media/storsimple-8000-device-failover-disaster-recovery/failover-phy-dev14.png)

    2. <span data-ttu-id="76680-156">Kattintson a **Kötettárolók**.</span><span class="sxs-lookup"><span data-stu-id="76680-156">Click **Volume Containers**.</span></span> <span data-ttu-id="76680-157">Minden hello kötettárolók, hello kötetek hello régi eszközről, és szerepelnie kell.</span><span class="sxs-lookup"><span data-stu-id="76680-157">All hello volume containers, along with hello volumes from hello old device, should be listed.</span></span>

       <span data-ttu-id="76680-158">Ha rendelkezik a feladatokat átvenni hello kötettároló helyileg rögzített kötetek, a köteteket feladatátvétel történt rétegzett kötetek.</span><span class="sxs-lookup"><span data-stu-id="76680-158">If hello volume container that you failed over has locally pinned volumes, those volumes are failed over as tiered volumes.</span></span> <span data-ttu-id="76680-159">Helyileg rögzített kötetek nem támogatottak a StorSimple-felhő készüléken.</span><span class="sxs-lookup"><span data-stu-id="76680-159">Locally pinned volumes are not supported on a StorSimple Cloud Appliance.</span></span>

       ![Nézet target kötettárolók](./media/storsimple-8000-device-failover-disaster-recovery/failover-phy-dev17.png)


## <a name="next-steps"></a><span data-ttu-id="76680-161">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="76680-161">Next steps</span></span>

* <span data-ttu-id="76680-162">Miután elvégezte a feladatátvételt, esetleg túl[inaktiválja vagy törölje a StorSimple eszköz](storsimple-8000-deactivate-and-delete-device.md).</span><span class="sxs-lookup"><span data-stu-id="76680-162">After you have performed a failover, you may need too[deactivate or delete your StorSimple device](storsimple-8000-deactivate-and-delete-device.md).</span></span>

* <span data-ttu-id="76680-163">Hogyan toouse hello StorSimple Device Manager információ szolgáltatás, nyissa meg túl[használata hello StorSimple Device Manager szolgáltatás tooadminister a StorSimple eszköz](storsimple-8000-manager-service-administration.md).</span><span class="sxs-lookup"><span data-stu-id="76680-163">For information about how toouse hello StorSimple Device Manager service, go too[Use hello StorSimple Device Manager service tooadminister your StorSimple device](storsimple-8000-manager-service-administration.md).</span></span>

