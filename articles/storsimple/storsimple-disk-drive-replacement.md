---
title: "Cserélje le a lemezmeghajtó a StorSimple eszközön |} Microsoft Docs"
description: "Cserélje le egy meghajtót a StorSimple elsődleges ház vagy egy EBOD ház ismerteti."
services: storsimple
documentationcenter: 
author: alkohli
manager: carmonm
editor: 
ms.assetid: 98890d36-b613-40fd-994e-330dd907a8a1
ms.service: storsimple
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: TBD
ms.date: 08/17/2016
ms.author: alkohli
ms.openlocfilehash: 0659ab9d304dbfcce72e8c3c79edad68e70b9630
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="replace-a-disk-drive-on-your-storsimple-device"></a><span data-ttu-id="0b78f-103">Cserélje le a lemezmeghajtó a StorSimple eszköz</span><span class="sxs-lookup"><span data-stu-id="0b78f-103">Replace a disk drive on your StorSimple device</span></span>
## <a name="overview"></a><span data-ttu-id="0b78f-104">Áttekintés</span><span class="sxs-lookup"><span data-stu-id="0b78f-104">Overview</span></span>
<span data-ttu-id="0b78f-105">Ez az oktatóanyag azt ismerteti, hogyan távolítsa el, és cserélje le a hibás vagy nem sikerült merevlemez-meghajtó a Microsoft Azure StorSimple eszközön.</span><span class="sxs-lookup"><span data-stu-id="0b78f-105">This tutorial explains how you can remove and replace a malfunctioning or failed hard disk drive on a Microsoft Azure StorSimple device.</span></span> <span data-ttu-id="0b78f-106">A meghajtó cseréjéhez kell:</span><span class="sxs-lookup"><span data-stu-id="0b78f-106">To replace a disk drive, you need to:</span></span>

* <span data-ttu-id="0b78f-107">A antitamper zárolási működésűeknek</span><span class="sxs-lookup"><span data-stu-id="0b78f-107">Disengage the antitamper lock</span></span>
* <span data-ttu-id="0b78f-108">Távolítsa el a lemezmeghajtó</span><span class="sxs-lookup"><span data-stu-id="0b78f-108">Remove the disk drive</span></span>
* <span data-ttu-id="0b78f-109">Telepítse a helyettesítő lemezmeghajtó</span><span class="sxs-lookup"><span data-stu-id="0b78f-109">Install the replacement disk drive</span></span>

> [!IMPORTANT]
> <span data-ttu-id="0b78f-110">Mielőtt eltávolítása és cseréje egy meghajtó, tekintse át a biztonsági információk [StorSimple hardver összetevő cseréje](storsimple-hardware-component-replacement.md).</span><span class="sxs-lookup"><span data-stu-id="0b78f-110">Before removing and replacing a disk drive, review the safety information in [StorSimple hardware component replacement](storsimple-hardware-component-replacement.md).</span></span>
> 
> 

## <a name="disengage-the-antitamper-lock"></a><span data-ttu-id="0b78f-111">A antitamper zárolási működésűeknek</span><span class="sxs-lookup"><span data-stu-id="0b78f-111">Disengage the antitamper lock</span></span>
<span data-ttu-id="0b78f-112">Ez az eljárás azt ismerteti, hogyan a StorSimple eszköz antitamper zárolásokat is lehet vagy kikapcsolt állapotban a merevlemez-meghajtók cseréjekor.</span><span class="sxs-lookup"><span data-stu-id="0b78f-112">This procedure explains how the antitamper locks on your StorSimple device can be engaged or disengaged when you replace the disk drives.</span></span> <span data-ttu-id="0b78f-113">A meghajtó szolgáltatónként Handles antitamper zárolásokat szerelnek, és azokat a leíró zárolás szakaszában egy kis nyílás keresztül érik el.</span><span class="sxs-lookup"><span data-stu-id="0b78f-113">The antitamper locks are fitted in the drive carrier handles, and they are accessed through a small aperture in the latch section of the handle.</span></span> <span data-ttu-id="0b78f-114">A zárolt pozícióra állítsa be a zárolásokat meghajtók végzik.</span><span class="sxs-lookup"><span data-stu-id="0b78f-114">Drives are supplied with the locks set to the locked position.</span></span>

#### <a name="to-unlock-the-antitamper-lock"></a><span data-ttu-id="0b78f-115">A antitamper zárolás feloldásához</span><span class="sxs-lookup"><span data-stu-id="0b78f-115">To unlock the antitamper lock</span></span>
1. <span data-ttu-id="0b78f-116">A zárolás kulcs ("tamperproof" T10 csavarhúzót Microsoft biztosított) gondosan beszúrása a leíró a nyílás és a szoftvercsatorna.</span><span class="sxs-lookup"><span data-stu-id="0b78f-116">Carefully insert the lock key (a "tamperproof" T10 screwdriver that Microsoft provided) into the aperture in the handle and into its socket.</span></span> 
   
   > [!NOTE]
   > <span data-ttu-id="0b78f-117">A antitamper zárolási aktiválódik, ha a piros jelzőikon nyílás látható.</span><span class="sxs-lookup"><span data-stu-id="0b78f-117">If the antitamper lock is activated, the red indicator is visible in the aperture.</span></span>
   > 
   > 
   
    ![Zárolt meghajtó](./media/storsimple-disk-drive-replacement/IC741056.png)
   
    <span data-ttu-id="0b78f-119">**1. ábra** részt vevő elleni illetéktelen zárolása</span><span class="sxs-lookup"><span data-stu-id="0b78f-119">**Figure 1** Anti-tamper lock engaged</span></span>
   
   | <span data-ttu-id="0b78f-120">Címke</span><span class="sxs-lookup"><span data-stu-id="0b78f-120">Label</span></span> | <span data-ttu-id="0b78f-121">Leírás</span><span class="sxs-lookup"><span data-stu-id="0b78f-121">Description</span></span> |
   |:--- |:--- |
   | <span data-ttu-id="0b78f-122">1</span><span class="sxs-lookup"><span data-stu-id="0b78f-122">1</span></span> |<span data-ttu-id="0b78f-123">Kijelző nyílás</span><span class="sxs-lookup"><span data-stu-id="0b78f-123">Indicator aperture</span></span> |
   | <span data-ttu-id="0b78f-124">2</span><span class="sxs-lookup"><span data-stu-id="0b78f-124">2</span></span> |<span data-ttu-id="0b78f-125">Antitamper zárolása</span><span class="sxs-lookup"><span data-stu-id="0b78f-125">Antitamper lock</span></span> |
2. <span data-ttu-id="0b78f-126">Forgassa el anticlockwise irányba a kulcsot, amíg a piros jelző nem látható a fenti a kulcs nyílás.</span><span class="sxs-lookup"><span data-stu-id="0b78f-126">Rotate the key in an anticlockwise direction until the red indicator is not visible in the aperture above the key.</span></span>
3. <span data-ttu-id="0b78f-127">Távolítsa el a kulcsot.</span><span class="sxs-lookup"><span data-stu-id="0b78f-127">Remove the key.</span></span>
   
    ![Nem zárolt meghajtó](./media/storsimple-disk-drive-replacement/IC741057.png)
   
    <span data-ttu-id="0b78f-129">**2. ábra** feloldva lemezmeghajtó</span><span class="sxs-lookup"><span data-stu-id="0b78f-129">**Figure 2** Unlocked disk drive</span></span>
4. <span data-ttu-id="0b78f-130">A meghajtó most eltávolítható.</span><span class="sxs-lookup"><span data-stu-id="0b78f-130">The disk drive can now be removed.</span></span>

<span data-ttu-id="0b78f-131">Kövesse a lépéseket visszafelé végezhetnek a zárolást.</span><span class="sxs-lookup"><span data-stu-id="0b78f-131">Follow the steps in reverse to engage the lock.</span></span>

## <a name="remove-the-disk-drive"></a><span data-ttu-id="0b78f-132">Távolítsa el a lemezmeghajtó</span><span class="sxs-lookup"><span data-stu-id="0b78f-132">Remove the disk drive</span></span>
<span data-ttu-id="0b78f-133">A StorSimple eszköz támogatja a RAID 10-szerű szóközöket tárkonfiguráció.</span><span class="sxs-lookup"><span data-stu-id="0b78f-133">Your StorSimple device supports a RAID 10-like storage spaces configuration.</span></span> <span data-ttu-id="0b78f-134">Ez azt jelenti, hogy azt is megfelelően működik egy meghibásodott lemezt, SSD-meghajtót (SSD), vagy a merevlemez-meghajtó (HDD).</span><span class="sxs-lookup"><span data-stu-id="0b78f-134">This implies that it can operate normally with one failed disk, solid-state drive (SSD), or hard disk drive (HDD).</span></span> 

> [!IMPORTANT]
> * <span data-ttu-id="0b78f-135">Ha a rendszer több mint egy meghibásodott lemezt, távolítsa el egynél több SSD és HDD a rendszer minden ponton időben.</span><span class="sxs-lookup"><span data-stu-id="0b78f-135">If your system has more than one failed disk, do not remove more than one SSD or HDD from the system at any point in time.</span></span> <span data-ttu-id="0b78f-136">Így adatvesztést eredményezhet.</span><span class="sxs-lookup"><span data-stu-id="0b78f-136">Doing so could result in loss of data.</span></span>
> * <span data-ttu-id="0b78f-137">Ellenőrizze, hogy egy helyettesítő SSD helyez egy SSD korábban tartalmazó tárhely.</span><span class="sxs-lookup"><span data-stu-id="0b78f-137">Make sure that you place a replacement SSD in a slot that previously contained an SSD.</span></span> <span data-ttu-id="0b78f-138">Hasonlóképpen helyezze HDD helyettesíti, amely korábban tartalmazott egy HDD tárolóhelyen található.</span><span class="sxs-lookup"><span data-stu-id="0b78f-138">Similarly, place a replacement HDD in a slot that previously contained an HDD.</span></span>
> * <span data-ttu-id="0b78f-139">A klasszikus Azure portálra, a bővítőhelyek számozása 0 – 11.</span><span class="sxs-lookup"><span data-stu-id="0b78f-139">In the Azure classic portal, slots are numbered from 0 – 11.</span></span> <span data-ttu-id="0b78f-140">Ezért ha a portál megjeleníti, hogy a lemez tárolóhelye 2 sikertelen, az eszközön, keresse meg a meghibásodott lemezt, a harmadik tárolóhelye a bal felső.</span><span class="sxs-lookup"><span data-stu-id="0b78f-140">Therefore, if the portal shows that a disk in slot 2 has failed, on the device, look for the failed disk in the third slot from the top left.</span></span>
> 
> 

<span data-ttu-id="0b78f-141">Meghajtók távolítva, és amíg a rendszer lecseréli.</span><span class="sxs-lookup"><span data-stu-id="0b78f-141">Drives can be removed and replaced while the system is operating.</span></span>

#### <a name="to-remove-a-drive"></a><span data-ttu-id="0b78f-142">A meghajtó eltávolítása</span><span class="sxs-lookup"><span data-stu-id="0b78f-142">To remove a drive</span></span>
1. <span data-ttu-id="0b78f-143">Azonosítsa a meghibásodott lemezt, a klasszikus Azure portálon lépjen **eszközök** > **karbantartási** > **hardverállapot**.</span><span class="sxs-lookup"><span data-stu-id="0b78f-143">To identify the failed disk, in the Azure classic portal, go to **Devices** > **Maintenance** > **Hardware Status**.</span></span> <span data-ttu-id="0b78f-144">Lemez sikertelen lehet az elsődleges ház és/vagy egy EBOD ház (Ha egy 8600 modellt használ), mert a lemez állapotát megtekinthetik **megosztott összetevők** és a **EBOD ház megosztott összetevők**.</span><span class="sxs-lookup"><span data-stu-id="0b78f-144">Because a disk can fail in the primary enclosure and/or in an EBOD enclosure (if you are using a 8600 model), look at the status of the disks under **Shared Components** and under **EBOD enclosure Shared Components**.</span></span> <span data-ttu-id="0b78f-145">A hibás lemez vagy szolgáltatással vörös állapottal jelenik meg.</span><span class="sxs-lookup"><span data-stu-id="0b78f-145">A failed disk in either enclosure will be shown with a red status.</span></span>
2. <span data-ttu-id="0b78f-146">Keresse meg a meghajtók elején található az elsődleges vagy a a EBOD ház.</span><span class="sxs-lookup"><span data-stu-id="0b78f-146">Locate the drives in the front of the primary enclosure or the EBOD enclosure.</span></span> 
3. <span data-ttu-id="0b78f-147">Ha a lemez oldva, folytassa a következő lépéssel.</span><span class="sxs-lookup"><span data-stu-id="0b78f-147">If the disk is unlocked, proceed to the next step.</span></span> <span data-ttu-id="0b78f-148">Ha a lemez le van tiltva, oldhatja fel azt az eljárást követve [antitamper zárolás működésűeknek](#disengage-the-antitamper-lock).</span><span class="sxs-lookup"><span data-stu-id="0b78f-148">If the disk is locked, unlock it by following the procedure in [Disengage the antitamper lock](#disengage-the-antitamper-lock).</span></span>
4. <span data-ttu-id="0b78f-149">A fekete zárolás nyomja meg a meghajtó szolgáltatónként modul, és a meghajtó szolgáltatónként leíró számítógépnél ki és lekéréses elölről kell elhelyezni.</span><span class="sxs-lookup"><span data-stu-id="0b78f-149">Press the black latch on the drive carrier module and pull the drive carrier handle out and away from the front of the chassis.</span></span> 
   
    ![Lemezmeghajtó leíró feloldása](./media/storsimple-disk-drive-replacement/IC741051.png)
   
    <span data-ttu-id="0b78f-151">**3. ábra** a meghajtó leíró feloldása</span><span class="sxs-lookup"><span data-stu-id="0b78f-151">**Figure 3** Releasing the drive handle</span></span>
5. <span data-ttu-id="0b78f-152">Ha a meghajtó szolgáltatónként leíró teljesen ki van bővítve, csúsztassa be a meghajtó szolgáltatói a váz kívül.</span><span class="sxs-lookup"><span data-stu-id="0b78f-152">When the drive carrier handle is fully extended, slide the drive carrier out of the chassis.</span></span> 
   
    ![A késleltetett lemez Lemezmeghajtó kívül](./media/storsimple-disk-drive-replacement/IC741052.png)
   
    <span data-ttu-id="0b78f-154">**4. ábra** késleltetett a lemezmeghajtó a szolgáltatói kívül</span><span class="sxs-lookup"><span data-stu-id="0b78f-154">**Figure 4** Sliding the disk drive out of the carrier</span></span>

## <a name="install-the-replacement-disk-drive"></a><span data-ttu-id="0b78f-155">Telepítse a helyettesítő lemezmeghajtó</span><span class="sxs-lookup"><span data-stu-id="0b78f-155">Install the replacement disk drive</span></span>
<span data-ttu-id="0b78f-156">Miután egy meghajtót a StorSimple eszközt a sikertelen volt, és törölte, az alábbi eljárás segítségével cserélje le egy új meghajtót.</span><span class="sxs-lookup"><span data-stu-id="0b78f-156">After a drive has failed in your StorSimple device and you have removed it, follow this procedure to replace it with a new drive.</span></span>

#### <a name="to-insert-a-drive"></a><span data-ttu-id="0b78f-157">A meghajtó beszúrása</span><span class="sxs-lookup"><span data-stu-id="0b78f-157">To insert a drive</span></span>
1. <span data-ttu-id="0b78f-158">Győződjön meg arról, a meghajtó szolgáltatónként leíró teljesen ki van bővítve, a következő ábrán látható módon.</span><span class="sxs-lookup"><span data-stu-id="0b78f-158">Ensure the drive carrier handle is fully extended, as shown in the following image.</span></span>
   
    ![A kiterjesztett leíró meghajtó](./media/storsimple-disk-drive-replacement/IC741044.png)
   
    <span data-ttu-id="0b78f-160">**5. ábra** kiterjesztett leíró rendelkező meghajtóra</span><span class="sxs-lookup"><span data-stu-id="0b78f-160">**Figure 5** Drive with handle extended</span></span>
2. <span data-ttu-id="0b78f-161">Csúsztassa be a meghajtó szolgáltatói egészen a váz be.</span><span class="sxs-lookup"><span data-stu-id="0b78f-161">Slide the drive carrier all the way into the chassis.</span></span> 
   
    ![A meghajtó szolgáltatónként mozgó lemez](./media/storsimple-disk-drive-replacement/IC741045.png)
   
    <span data-ttu-id="0b78f-163">**6. ábra** a meghajtó szolgáltatói késleltetett azokat a váz</span><span class="sxs-lookup"><span data-stu-id="0b78f-163">**Figure 6**  Sliding the drive carrier into the chassis</span></span>
3. <span data-ttu-id="0b78f-164">Az a meghajtó szolgáltatói beszúrni zárja be a meghajtó szolgáltatónként leíró miközben továbbra is a meghajtó szolgáltatói leküldéses azokat a váz, mindaddig, amíg a meghajtó szolgáltatónként leíró zárolt helyzetbe illesztése.</span><span class="sxs-lookup"><span data-stu-id="0b78f-164">With the drive carrier inserted, close the drive carrier handle while continuing to push the drive carrier into the chassis, until the drive carrier handle snaps into a locked position.</span></span>
4. <span data-ttu-id="0b78f-165">Microsoft (tamperproof Torx csavarhúzóra) biztonságossá tétele a vivőjel-leíró által biztosított zárolási kulcs használata helyen által, hogy a zárolás csavart negyedéves kapcsolja jobbra.</span><span class="sxs-lookup"><span data-stu-id="0b78f-165">Use the lock key that was provided by Microsoft (tamperproof Torx screwdriver) to secure the carrier handle into place by turning the lock screw a quarter turn clockwise.</span></span>
5. <span data-ttu-id="0b78f-166">Ellenőrizze, hogy sikeres volt-e a csere és a meghajtó működési fér hozzá a klasszikus Azure portálra, és navigáljon **karbantartási** > **hardverállapot**.</span><span class="sxs-lookup"><span data-stu-id="0b78f-166">Verify that the replacement was successful and the drive is operational by accessing the Azure classic portal and navigating to **Maintenance** > **Hardware Status**.</span></span> <span data-ttu-id="0b78f-167">A **megosztott összetevők** vagy **EBOD ház megosztott összetevők**, a meghajtó állapota zöld, jelezve, hogy kifogástalan kell lennie.</span><span class="sxs-lookup"><span data-stu-id="0b78f-167">Under **Shared Components** or **EBOD enclosure Shared Components**, the drive status should be green, indicating that it is healthy.</span></span>
   
   > [!NOTE]
   > <span data-ttu-id="0b78f-168">A lemez állapota zöld színűre a csere utáni több órát is igénybe vehet.</span><span class="sxs-lookup"><span data-stu-id="0b78f-168">It may take several hours for the disk status to turn green after the replacement.</span></span>
   > 
   > 

## <a name="next-steps"></a><span data-ttu-id="0b78f-169">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="0b78f-169">Next steps</span></span>
<span data-ttu-id="0b78f-170">További információ [StorSimple hardver összetevő cseréje](storsimple-hardware-component-replacement.md).</span><span class="sxs-lookup"><span data-stu-id="0b78f-170">Learn more about [StorSimple hardware component replacement](storsimple-hardware-component-replacement.md).</span></span>

