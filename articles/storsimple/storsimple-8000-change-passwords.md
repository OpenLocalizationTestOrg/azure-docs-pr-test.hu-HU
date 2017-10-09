---
title: aaaChange a StorSimple-jelszavak |} Microsoft Docs
description: "Ismerteti, hogyan toouse hello StorSimple Device Manager szolgáltatás toochange a StorSimple Snapshot Manager és az eszköz rendszergazdai jelszót."
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
ms.date: 07/03/2017
ms.author: alkohli
ms.openlocfilehash: cf884be31b4bbf9e372c0aa11b9da2eadcda35dd
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="use-hello-storsimple-device-manager-service-toochange-your-storsimple-passwords"></a><span data-ttu-id="41f48-103">Hello StorSimple Device Manager szolgáltatás toochange a StorSimple-jelszavak használata</span><span class="sxs-lookup"><span data-stu-id="41f48-103">Use hello StorSimple Device Manager service toochange your StorSimple passwords</span></span>

## <a name="overview"></a><span data-ttu-id="41f48-104">Áttekintés</span><span class="sxs-lookup"><span data-stu-id="41f48-104">Overview</span></span>
<span data-ttu-id="41f48-105">Azure-portálon hello **eszközbeállítások** beállítást tartalmaz olyan módon konfigurálhatja újra a StorSimple eszközön a StorSimple eszköz Manager szolgáltatás által kezelt összes hello eszköz paraméter.</span><span class="sxs-lookup"><span data-stu-id="41f48-105">hello Azure portal **Device settings** option contains all hello device parameters that you can reconfigure on a StorSimple device that is managed by a StorSimple Device Manager service.</span></span> <span data-ttu-id="41f48-106">Ez az oktatóanyag azt ismerteti, hogyan használhatja a hello **biztonsági** lehetőség alatt **eszközbeállítások** toochange az eszköz-rendszergazdai vagy a StorSimple Snapshot Manager jelszavát.</span><span class="sxs-lookup"><span data-stu-id="41f48-106">This tutorial explains how you can use hello **Security** option under **Device settings** toochange your device administrator or StorSimple Snapshot Manager password.</span></span>

## <a name="change-hello-device-administrator-password"></a><span data-ttu-id="41f48-107">Változás hello eszköz rendszergazdai jelszava</span><span class="sxs-lookup"><span data-stu-id="41f48-107">Change hello device administrator password</span></span>
<span data-ttu-id="41f48-108">Windows PowerShell felületet tooaccess hello StorSimple-eszköz használata esetén szükség tooenter egy eszköz rendszergazdai jelszava.</span><span class="sxs-lookup"><span data-stu-id="41f48-108">When you use Windows PowerShell interface tooaccess hello StorSimple device, you are required tooenter a device administrator password.</span></span> <span data-ttu-id="41f48-109">Amikor hello első StorSimple eszköz regisztrálva van a szolgáltatásban, hello alapértelmezett Ez az interfész jelszava *jelszó1*.</span><span class="sxs-lookup"><span data-stu-id="41f48-109">When hello first StorSimple device is registered with a service, hello default password for this interface is *Password1*.</span></span> <span data-ttu-id="41f48-110">Hello biztonsági adatait, áll szükséges toochange ezt a jelszót hello regisztrációs folyamat hello végét.</span><span class="sxs-lookup"><span data-stu-id="41f48-110">For hello security of your data, you are required toochange this password at hello end of hello registration process.</span></span> <span data-ttu-id="41f48-111">Ez a jelszó módosítása nélkül kilép nem hello regisztrációs folyamat során.</span><span class="sxs-lookup"><span data-stu-id="41f48-111">You cannot exit from hello registration process without changing this password.</span></span> <span data-ttu-id="41f48-112">További információkért lásd: [3. lépés: eszköz konfigurálása és regisztrálása hello a Windows PowerShell segítségével a StorSimple](storsimple-8000-deployment-walkthrough-u2.md#step-3-configure-and-register-the-device-through-windows-powershell-for-storsimple).</span><span class="sxs-lookup"><span data-stu-id="41f48-112">For more information, see [Step 3: Configure and register hello device through Windows PowerShell for StorSimple](storsimple-8000-deployment-walkthrough-u2.md#step-3-configure-and-register-the-device-through-windows-powershell-for-storsimple).</span></span>

<span data-ttu-id="41f48-113">hello jelszó, amelyet először hello Windows PowerShell felületen regisztrálás során később módosítható hello Azure-portálon keresztül.</span><span class="sxs-lookup"><span data-stu-id="41f48-113">hello password that was first set through hello Windows PowerShell interface during registration can be changed later via hello Azure portal.</span></span> <span data-ttu-id="41f48-114">Hajtsa végre a következő lépéseket toochange hello eszköz rendszergazdai jelszava hello.</span><span class="sxs-lookup"><span data-stu-id="41f48-114">Perform hello following steps toochange hello device administrator password.</span></span>

#### <a name="toochange-hello-device-administrator-password"></a><span data-ttu-id="41f48-115">toochange hello eszköz rendszergazdai jelszava</span><span class="sxs-lookup"><span data-stu-id="41f48-115">toochange hello device administrator password</span></span>
1. <span data-ttu-id="41f48-116">Nyissa meg tooyour StorSimple Device Manager szolgáltatás és **eszközök**.</span><span class="sxs-lookup"><span data-stu-id="41f48-116">Go tooyour StorSimple Device Manager service and click **Devices**.</span></span>

2. <span data-ttu-id="41f48-117">Eszközök listája táblázatos hello, válassza ki, és kattintson hello eszközre, amelynek a jelszavát szeretné toochange.</span><span class="sxs-lookup"><span data-stu-id="41f48-117">From hello tabular listing of devices, select and click hello device whose password you intend toochange.</span></span>

    ![](./media/storsimple-8000-change-passwords/changepwd1.png)

3. <span data-ttu-id="41f48-118">A hello **beállítások** paneljén lépjen túl**eszközbeállítások > biztonsági**.</span><span class="sxs-lookup"><span data-stu-id="41f48-118">In hello **Settings** blade, go too**Device settings > Security**.</span></span>

    ![](./media/storsimple-8000-change-passwords/changepwd2.png)

4. <span data-ttu-id="41f48-119">A hello **biztonsági beállítások** panelen kattintson a **jelszó** toochange hello eszköz rendszergazdai jelszava.</span><span class="sxs-lookup"><span data-stu-id="41f48-119">In hello **Security settings** blade, click **Password** toochange hello device administrator password.</span></span>

    ![](./media/storsimple-8000-change-passwords/changepwd3.png)

5. <span data-ttu-id="41f48-120">A hello **jelszó** panelen adjon meg egy rendszergazdai jelszót, amely a 8 too15 karaktereket tartalmaz.</span><span class="sxs-lookup"><span data-stu-id="41f48-120">In hello **Password** blade, provide an administrator password that contains from 8 too15 characters.</span></span> <span data-ttu-id="41f48-121">hello jelszó 3 vagy több nagybetű, nagybetűk, numerikus és speciális karakterek kombinációjából kell lennie.</span><span class="sxs-lookup"><span data-stu-id="41f48-121">hello password must be a combination of 3 or more of uppercase, lowercase, numeric, and special characters.</span></span>

6. <span data-ttu-id="41f48-122">Hello jelszót.</span><span class="sxs-lookup"><span data-stu-id="41f48-122">Confirm hello password.</span></span>

    ![](./media/storsimple-8000-change-passwords/changepwd4.png)

7. <span data-ttu-id="41f48-123">Kattintson a **mentése** és megerősítést kér, amikor kattintson **Igen**.</span><span class="sxs-lookup"><span data-stu-id="41f48-123">Click **Save** and when prompted for confirmation, click **Yes**.</span></span>

    ![](./media/storsimple-8000-change-passwords/changepwd6.png)

<span data-ttu-id="41f48-124">hello eszköz rendszergazdai jelszava most frissíteni kell.</span><span class="sxs-lookup"><span data-stu-id="41f48-124">hello device administrator password should now be updated.</span></span> <span data-ttu-id="41f48-125">A módosított jelszó tooaccess hello Windows PowerShell-felületet is használhatja.</span><span class="sxs-lookup"><span data-stu-id="41f48-125">You can use this modified password tooaccess hello Windows PowerShell interface.</span></span>

## <a name="set-hello-storsimple-snapshot-manager-password"></a><span data-ttu-id="41f48-126">Hello StorSimple Snapshot Manager jelszavának beállítása</span><span class="sxs-lookup"><span data-stu-id="41f48-126">Set hello StorSimple Snapshot Manager password</span></span>
<span data-ttu-id="41f48-127">StorSimple Snapshot Manager szoftver a Windows-gazdagépen helyezkedik el, és lehetővé teszi, hogy a rendszergazdák a StorSimple eszköz hello formában a helyi és felhőbeli pillanatképeket toomanage biztonsági másolatait.</span><span class="sxs-lookup"><span data-stu-id="41f48-127">StorSimple Snapshot Manager software resides on your Windows host and allows administrators toomanage backups of your StorSimple device in hello form of local and cloud snapshots.</span></span>

<span data-ttu-id="41f48-128">Amikor konfigurál egy eszközt a StorSimple Snapshot Managerben, kérni fogja tooprovide hello eszköz IP cím és jelszó tooauthenticate a tárolóeszköz.</span><span class="sxs-lookup"><span data-stu-id="41f48-128">When configuring a device in StorSimple Snapshot Manager, you will be prompted tooprovide hello device IP address and password tooauthenticate your storage device.</span></span>

<span data-ttu-id="41f48-129">Állítsa be, vagy módosítsa a hello jelszavát a StorSimple Snapshot Manager hello Azure-portálon keresztül.</span><span class="sxs-lookup"><span data-stu-id="41f48-129">You can set or change hello password for StorSimple Snapshot Manager via hello Azure portal.</span></span> <span data-ttu-id="41f48-130">Hajtsa végre a következő lépéseket tooset hello, vagy módosítsa a hello StorSimple Snapshot Manager jelszavát.</span><span class="sxs-lookup"><span data-stu-id="41f48-130">Perform hello following steps tooset or change hello StorSimple Snapshot Manager password.</span></span>

#### <a name="tooset-hello-storsimple-snapshot-manager-password"></a><span data-ttu-id="41f48-131">tooset hello StorSimple Snapshot Manager jelszava</span><span class="sxs-lookup"><span data-stu-id="41f48-131">tooset hello StorSimple Snapshot Manager password</span></span>
1. <span data-ttu-id="41f48-132">Nyissa meg tooyour StorSimple Device Manager szolgáltatás és **eszközök**.</span><span class="sxs-lookup"><span data-stu-id="41f48-132">Go tooyour StorSimple Device Manager service and click **Devices**.</span></span>

2. <span data-ttu-id="41f48-133">Eszközök listája táblázatos hello, válassza ki, majd kattintson a hello eszköz tooset tervezi, vagy módosítsa, amelynek StorSimple Snapshot Manager jelszavát.</span><span class="sxs-lookup"><span data-stu-id="41f48-133">From hello tabular listing of devices, select and click hello device whose StorSimple Snapshot Manager password you intend tooset or change.</span></span>

     ![](./media/storsimple-8000-change-passwords/changepwd1.png)

3. <span data-ttu-id="41f48-134">A hello **beállítások** paneljén lépjen túl**eszközbeállítások > biztonsági**.</span><span class="sxs-lookup"><span data-stu-id="41f48-134">In hello **Settings** blade, go too**Device settings > Security**.</span></span>

     ![](./media/storsimple-8000-change-passwords/changepwd2.png)

4. <span data-ttu-id="41f48-135">A hello **biztonsági beállítások** panelen kattintson a **jelszó** tooset, vagy módosítsa a hello StorSimple Snapshot Manager jelszavát.</span><span class="sxs-lookup"><span data-stu-id="41f48-135">In hello **Security settings** blade, click **Password** tooset or change hello StorSimple Snapshot Manager password.</span></span>

     ![](./media/storsimple-8000-change-passwords/changepwd3.png) 

5. <span data-ttu-id="41f48-136">A hello **jelszó** panelen adjon meg egy 14 vagy 15 karakter hosszú jelszót.</span><span class="sxs-lookup"><span data-stu-id="41f48-136">In hello **Password** blade, enter a password that is 14 or 15 characters.</span></span> <span data-ttu-id="41f48-137">Győződjön meg arról, hogy hello jelszó 3 vagy több nagybetű, nagybetűk, numerikus és speciális karaktert tartalmaz.</span><span class="sxs-lookup"><span data-stu-id="41f48-137">Make sure that hello password contains a combination of 3 or more of uppercase, lowercase, numeric, and special characters.</span></span>

6. <span data-ttu-id="41f48-138">Hello jelszót.</span><span class="sxs-lookup"><span data-stu-id="41f48-138">Confirm hello password.</span></span>

     ![](./media/storsimple-8000-change-passwords/changepwd5.png)

7. <span data-ttu-id="41f48-139">Kattintson a **mentése** és megerősítést kér, amikor kattintson **Igen**.</span><span class="sxs-lookup"><span data-stu-id="41f48-139">Click **Save** and when prompted for confirmation, click **Yes**.</span></span>

     ![](./media/storsimple-8000-change-passwords/changepwd6.png)

<span data-ttu-id="41f48-140">hello StorSimple Snapshot Manager jelszava most frissíteni kell.</span><span class="sxs-lookup"><span data-stu-id="41f48-140">hello StorSimple Snapshot Manager password should now be updated.</span></span>

## <a name="next-steps"></a><span data-ttu-id="41f48-141">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="41f48-141">Next steps</span></span>
* <span data-ttu-id="41f48-142">További információ [StorSimple biztonsági](storsimple-8000-security.md).</span><span class="sxs-lookup"><span data-stu-id="41f48-142">Learn more about [StorSimple security](storsimple-8000-security.md).</span></span>
* <span data-ttu-id="41f48-143">További információ [az eszköz konfigurációjának módosítása](storsimple-8000-modify-device-config.md).</span><span class="sxs-lookup"><span data-stu-id="41f48-143">Learn more about [modifying your device configuration](storsimple-8000-modify-device-config.md).</span></span>
* <span data-ttu-id="41f48-144">További információ [használatával hello StorSimple Device Manager szolgáltatás tooadminister a StorSimple eszköz](storsimple-8000-manager-service-administration.md).</span><span class="sxs-lookup"><span data-stu-id="41f48-144">Learn more about [using hello StorSimple Device Manager service tooadminister your StorSimple device](storsimple-8000-manager-service-administration.md).</span></span>

