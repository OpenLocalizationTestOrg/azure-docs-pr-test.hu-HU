---
title: "a StorSimple 8000 series eszközön lemezmeghajtó aaaReplace |} Microsoft Docs"
description: "Ismerteti, hogyan tooreplace lemez meghajtója StorSimple elsődleges ház vagy egy EBOD ház."
services: storsimple
documentationcenter: 
author: alkohli
manager: timlt
editor: 
ms.assetid: 
ms.service: storsimple
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: TBD
ms.date: 073/2017
ms.author: alkohli
ms.openlocfilehash: 09c1bf5e97adf54a609a868e921351bc63e00a83
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="replace-a-disk-drive-on-your-storsimple-8000-series-device"></a><span data-ttu-id="0e821-103">Cserélje le a lemezmeghajtó a StorSimple 8000 series eszközön</span><span class="sxs-lookup"><span data-stu-id="0e821-103">Replace a disk drive on your StorSimple 8000 series device</span></span>

## <a name="overview"></a><span data-ttu-id="0e821-104">Áttekintés</span><span class="sxs-lookup"><span data-stu-id="0e821-104">Overview</span></span>
<span data-ttu-id="0e821-105">Ez az oktatóanyag azt ismerteti, hogyan távolítsa el, és cserélje le a hibás vagy nem sikerült merevlemez-meghajtó a Microsoft Azure StorSimple eszközön.</span><span class="sxs-lookup"><span data-stu-id="0e821-105">This tutorial explains how you can remove and replace a malfunctioning or failed hard disk drive on a Microsoft Azure StorSimple device.</span></span> <span data-ttu-id="0e821-106">a meghajtó tooreplace, kell:</span><span class="sxs-lookup"><span data-stu-id="0e821-106">tooreplace a disk drive, you need to:</span></span>

* <span data-ttu-id="0e821-107">Hello antitamper zárolási működésűeknek</span><span class="sxs-lookup"><span data-stu-id="0e821-107">Disengage hello antitamper lock</span></span>
* <span data-ttu-id="0e821-108">Távolítsa el a hello a lemezmeghajtó</span><span class="sxs-lookup"><span data-stu-id="0e821-108">Remove hello disk drive</span></span>
* <span data-ttu-id="0e821-109">Hello helyettesítő meghajtó telepítése</span><span class="sxs-lookup"><span data-stu-id="0e821-109">Install hello replacement disk drive</span></span>

> [!IMPORTANT]
> <span data-ttu-id="0e821-110">Mielőtt eltávolítása és cseréje egy meghajtó, tekintse át a biztonsági információk hello [StorSimple hardver összetevő cseréje](storsimple-8000-hardware-component-replacement.md).</span><span class="sxs-lookup"><span data-stu-id="0e821-110">Before removing and replacing a disk drive, review hello safety information in [StorSimple hardware component replacement](storsimple-8000-hardware-component-replacement.md).</span></span>
 

## <a name="disengage-hello-antitamper-lock"></a><span data-ttu-id="0e821-111">Hello antitamper zárolási működésűeknek</span><span class="sxs-lookup"><span data-stu-id="0e821-111">Disengage hello antitamper lock</span></span>
<span data-ttu-id="0e821-112">Ez az eljárás azt ismerteti, hogyan hello antitamper zárolásokat a StorSimple eszköz is lehet vagy kikapcsolt állapotban hello lemezmeghajtók cseréjekor.</span><span class="sxs-lookup"><span data-stu-id="0e821-112">This procedure explains how hello antitamper locks on your StorSimple device can be engaged or disengaged when you replace hello disk drives.</span></span> <span data-ttu-id="0e821-113">hello antitamper zárolások szerelnek hello meghajtó szolgáltatónként kezelik, és azokat egy kis nyílás hello zárolás szakaszában hello leíró keresztül érik el.</span><span class="sxs-lookup"><span data-stu-id="0e821-113">hello antitamper locks are fitted in hello drive carrier handles, and they are accessed through a small aperture in hello latch section of hello handle.</span></span> <span data-ttu-id="0e821-114">Meghajtók hello zárolások beállított zárolt toohello pozíciót végzik.</span><span class="sxs-lookup"><span data-stu-id="0e821-114">Drives are supplied with hello locks set toohello locked position.</span></span>

#### <a name="toounlock-hello-antitamper-lock"></a><span data-ttu-id="0e821-115">toounlock hello antitamper zárolása</span><span class="sxs-lookup"><span data-stu-id="0e821-115">toounlock hello antitamper lock</span></span>
1. <span data-ttu-id="0e821-116">Gondosan beszúrása hello zárolási kulcs ("tamperproof" T10 csavarhúzót Microsoft biztosított), a hello leíró hello nyílás és a szoftvercsatorna.</span><span class="sxs-lookup"><span data-stu-id="0e821-116">Carefully insert hello lock key (a "tamperproof" T10 screwdriver that Microsoft provided) into hello aperture in hello handle and into its socket.</span></span> 
   
   <span data-ttu-id="0e821-117">Ha hello antitamper zárolási aktív, piros hello mutató hello nyílás látható.</span><span class="sxs-lookup"><span data-stu-id="0e821-117">If hello antitamper lock is activated, hello red indicator is visible in hello aperture.</span></span>
  
    ![Zárolt meghajtó](./media/storsimple-disk-drive-replacement/IC741056.png)
   
    <span data-ttu-id="0e821-119">**1. ábra** részt vevő elleni illetéktelen zárolása</span><span class="sxs-lookup"><span data-stu-id="0e821-119">**Figure 1** Anti-tamper lock engaged</span></span>
   
   | <span data-ttu-id="0e821-120">Címke</span><span class="sxs-lookup"><span data-stu-id="0e821-120">Label</span></span> | <span data-ttu-id="0e821-121">Leírás</span><span class="sxs-lookup"><span data-stu-id="0e821-121">Description</span></span> |
   |:--- |:--- |
   | <span data-ttu-id="0e821-122">1</span><span class="sxs-lookup"><span data-stu-id="0e821-122">1</span></span> |<span data-ttu-id="0e821-123">Kijelző nyílás</span><span class="sxs-lookup"><span data-stu-id="0e821-123">Indicator aperture</span></span> |
   | <span data-ttu-id="0e821-124">2</span><span class="sxs-lookup"><span data-stu-id="0e821-124">2</span></span> |<span data-ttu-id="0e821-125">Antitamper zárolása</span><span class="sxs-lookup"><span data-stu-id="0e821-125">Antitamper lock</span></span> |
2. <span data-ttu-id="0e821-126">Forgassa el hello kulcs anticlockwise irányba, amíg a piros hello mutató nem látható a fenti hello kulcs hello nyílás.</span><span class="sxs-lookup"><span data-stu-id="0e821-126">Rotate hello key in an anticlockwise direction until hello red indicator is not visible in hello aperture above hello key.</span></span>
3. <span data-ttu-id="0e821-127">Távolítsa el a hello kulcsot.</span><span class="sxs-lookup"><span data-stu-id="0e821-127">Remove hello key.</span></span>
   
    ![Nem zárolt meghajtó](./media/storsimple-disk-drive-replacement/IC741057.png)
   
    <span data-ttu-id="0e821-129">**2. ábra** feloldva lemezmeghajtó</span><span class="sxs-lookup"><span data-stu-id="0e821-129">**Figure 2** Unlocked disk drive</span></span>
4. <span data-ttu-id="0e821-130">hello a lemezmeghajtó most eltávolítható.</span><span class="sxs-lookup"><span data-stu-id="0e821-130">hello disk drive can now be removed.</span></span>

<span data-ttu-id="0e821-131">Hello lépésekkel fordított tooengage hello zárolva.</span><span class="sxs-lookup"><span data-stu-id="0e821-131">Follow hello steps in reverse tooengage hello lock.</span></span>

## <a name="remove-hello-disk-drive"></a><span data-ttu-id="0e821-132">Távolítsa el a hello a lemezmeghajtó</span><span class="sxs-lookup"><span data-stu-id="0e821-132">Remove hello disk drive</span></span>
<span data-ttu-id="0e821-133">A StorSimple eszköz támogatja a RAID 10-szerű szóközöket tárkonfiguráció.</span><span class="sxs-lookup"><span data-stu-id="0e821-133">Your StorSimple device supports a RAID 10-like storage spaces configuration.</span></span> <span data-ttu-id="0e821-134">Ez azt jelenti, hogy azt is megfelelően működik egy meghibásodott lemezt, SSD-meghajtót (SSD), vagy a merevlemez-meghajtó (HDD).</span><span class="sxs-lookup"><span data-stu-id="0e821-134">This implies that it can operate normally with one failed disk, solid-state drive (SSD), or hard disk drive (HDD).</span></span>

> [!IMPORTANT]
> * <span data-ttu-id="0e821-135">Ha a rendszer több mint egy meghibásodott lemezt, távolítsa el egynél több SSD és HDD hello rendszer bármikor időben.</span><span class="sxs-lookup"><span data-stu-id="0e821-135">If your system has more than one failed disk, do not remove more than one SSD or HDD from hello system at any point in time.</span></span> <span data-ttu-id="0e821-136">Így adatvesztést eredményezhet.</span><span class="sxs-lookup"><span data-stu-id="0e821-136">Doing so could result in loss of data.</span></span>
> * <span data-ttu-id="0e821-137">Ellenőrizze, hogy egy helyettesítő SSD helyez egy SSD korábban tartalmazó tárhely.</span><span class="sxs-lookup"><span data-stu-id="0e821-137">Make sure that you place a replacement SSD in a slot that previously contained an SSD.</span></span> <span data-ttu-id="0e821-138">Hasonlóképpen helyezze HDD helyettesíti, amely korábban tartalmazott egy HDD tárolóhelyen található.</span><span class="sxs-lookup"><span data-stu-id="0e821-138">Similarly, place a replacement HDD in a slot that previously contained an HDD.</span></span>
> * <span data-ttu-id="0e821-139">Hello Azure-portálon, a bővítőhelyek számozása 0 – 11.</span><span class="sxs-lookup"><span data-stu-id="0e821-139">In hello Azure portal, slots are numbered from 0 – 11.</span></span> <span data-ttu-id="0e821-140">Ezért ha hello portal jeleníti meg, hogy egy lemezt a tárolóhely 2 rendelkezik, az eszközön nem sikerült hello, keresse meg hello harmadik tárolóhely hello meghibásodott lemez a bal felső hello.</span><span class="sxs-lookup"><span data-stu-id="0e821-140">Therefore, if hello portal shows that a disk in slot 2 has failed, on hello device, look for hello failed disk in hello third slot from hello top left.</span></span>
> 
> 

<span data-ttu-id="0e821-141">Meghajtók távolítva, és amíg hello rendszer cserélni.</span><span class="sxs-lookup"><span data-stu-id="0e821-141">Drives can be removed and replaced while hello system is operating.</span></span>

#### <a name="tooremove-a-drive"></a><span data-ttu-id="0e821-142">a meghajtó tooremove</span><span class="sxs-lookup"><span data-stu-id="0e821-142">tooremove a drive</span></span>
1. <span data-ttu-id="0e821-143">tooidentify hello nem sikerült a lemezt, hello Azure-portálon lépjen tooyour eszköz **beállítások > hardver állapotának**.</span><span class="sxs-lookup"><span data-stu-id="0e821-143">tooidentify hello failed disk, in hello Azure portal, go tooyour device **Settings > Hardware health**.</span></span> <span data-ttu-id="0e821-144">Hello lemezei hello állapotának tekintse meg a lemez sikertelen lehet hello elsődleges ház és/vagy egy EBOD ház (Ha egy 8600 modellt használ), mert **összetevők megosztott** és a **EBOD megosztott összetevők** .</span><span class="sxs-lookup"><span data-stu-id="0e821-144">Because a disk can fail in hello primary enclosure and/or in an EBOD enclosure (if you are using a 8600 model), look at hello status of hello disks under **Shared components** and under **EBOD shared components**.</span></span> <span data-ttu-id="0e821-145">A hibás lemez vagy szolgáltatással vörös állapottal jelenik meg.</span><span class="sxs-lookup"><span data-stu-id="0e821-145">A failed disk in either enclosure will be shown with a red status.</span></span>
2. <span data-ttu-id="0e821-146">Keresse meg hello meghajtók hello elsődleges ház vagy hello EBOD ház hello elejéhez.</span><span class="sxs-lookup"><span data-stu-id="0e821-146">Locate hello drives in hello front of hello primary enclosure or hello EBOD enclosure.</span></span> 
3. <span data-ttu-id="0e821-147">Hello lemez fel oldva, folytassa a következő lépésre toohello műveletet.</span><span class="sxs-lookup"><span data-stu-id="0e821-147">If hello disk is unlocked, proceed toohello next step.</span></span> <span data-ttu-id="0e821-148">Hello lemez le van tiltva, ha a zárolást a hello eljárást követve [hello antitamper zárolási működésűeknek](#disengage-the-antitamper-lock).</span><span class="sxs-lookup"><span data-stu-id="0e821-148">If hello disk is locked, unlock it by following hello procedure in [Disengage hello antitamper lock](#disengage-the-antitamper-lock).</span></span>
4. <span data-ttu-id="0e821-149">Nyomja le az hello fekete hello meghajtó szolgáltatónként modul zárolni, és hello meghajtó szolgáltatónként leíró számítógépnél ki és lekéréses hello Váztípus hello elölről.</span><span class="sxs-lookup"><span data-stu-id="0e821-149">Press hello black latch on hello drive carrier module and pull hello drive carrier handle out and away from hello front of hello chassis.</span></span>
   
    ![Lemezmeghajtó leíró feloldása](./media/storsimple-disk-drive-replacement/IC741051.png)
   
    <span data-ttu-id="0e821-151">**3. ábra** felszabadításával hello meghajtó leíró</span><span class="sxs-lookup"><span data-stu-id="0e821-151">**Figure 3** Releasing hello drive handle</span></span>
5. <span data-ttu-id="0e821-152">Ha teljesen ki van bővítve a hello meghajtó szolgáltatónként leíró, csúsztassa be hello meghajtó szolgáltatónként hello váz kívül.</span><span class="sxs-lookup"><span data-stu-id="0e821-152">When hello drive carrier handle is fully extended, slide hello drive carrier out of hello chassis.</span></span> 
   
    ![A késleltetett lemez Lemezmeghajtó kívül](./media/storsimple-disk-drive-replacement/IC741052.png)
   
    <span data-ttu-id="0e821-154">**4. ábra** késleltetett hello meghajtó kívül hello szolgáltatója</span><span class="sxs-lookup"><span data-stu-id="0e821-154">**Figure 4** Sliding hello disk drive out of hello carrier</span></span>

## <a name="install-hello-replacement-disk-drive"></a><span data-ttu-id="0e821-155">Hello helyettesítő meghajtó telepítése</span><span class="sxs-lookup"><span data-stu-id="0e821-155">Install hello replacement disk drive</span></span>
<span data-ttu-id="0e821-156">Miután egy meghajtót a StorSimple eszközt a sikertelen volt, és törölte, hajtsa végre az ezen eljárás tooreplace azt egy új meghajtót.</span><span class="sxs-lookup"><span data-stu-id="0e821-156">After a drive has failed in your StorSimple device and you have removed it, follow this procedure tooreplace it with a new drive.</span></span>

#### <a name="tooinsert-a-drive"></a><span data-ttu-id="0e821-157">a meghajtó tooinsert</span><span class="sxs-lookup"><span data-stu-id="0e821-157">tooinsert a drive</span></span>
1. <span data-ttu-id="0e821-158">Győződjön meg arról, hello meghajtó szolgáltatónként leíró teljesen ki van bővítve, ahogy az a következő kép hello.</span><span class="sxs-lookup"><span data-stu-id="0e821-158">Ensure hello drive carrier handle is fully extended, as shown in hello following image.</span></span>
   
    ![A kiterjesztett leíró meghajtó](./media/storsimple-disk-drive-replacement/IC741044.png)
   
    <span data-ttu-id="0e821-160">**5. ábra** kiterjesztett leíró rendelkező meghajtóra</span><span class="sxs-lookup"><span data-stu-id="0e821-160">**Figure 5** Drive with handle extended</span></span>
2. <span data-ttu-id="0e821-161">Csúsztassa be a hello meghajtó szolgáltatónként összes hello módon történő hello váz.</span><span class="sxs-lookup"><span data-stu-id="0e821-161">Slide hello drive carrier all hello way into hello chassis.</span></span>
   
    ![A meghajtó szolgáltatónként mozgó lemez](./media/storsimple-disk-drive-replacement/IC741045.png)
   
    <span data-ttu-id="0e821-163">**6. ábra** csúszó hello meghajtó szolgáltatónként hello váz be</span><span class="sxs-lookup"><span data-stu-id="0e821-163">**Figure 6**  Sliding hello drive carrier into hello chassis</span></span>
3. <span data-ttu-id="0e821-164">Hello meghajtó vivőjel zárja be, beszúrt hello meghajtó szolgáltatónként leírójú folyamatos toopush hello meghajtó szolgáltatónként hello váz, amíg nem hello meghajtó szolgáltatónként leíró zárolt helyzetbe illeszkedik az közben.</span><span class="sxs-lookup"><span data-stu-id="0e821-164">With hello drive carrier inserted, close hello drive carrier handle while continuing toopush hello drive carrier into hello chassis, until hello drive carrier handle snaps into a locked position.</span></span>
4. <span data-ttu-id="0e821-165">Hello zárolási kulcs használata által biztosított Microsoft (tamperproof Torx csavarhúzóra) toosecure hello szolgáltatónként leíró helyen bekapcsolásával hello zárolási csavart negyedéves kapcsolja jobbra.</span><span class="sxs-lookup"><span data-stu-id="0e821-165">Use hello lock key that was provided by Microsoft (tamperproof Torx screwdriver) toosecure hello carrier handle into place by turning hello lock screw a quarter turn clockwise.</span></span>
5. <span data-ttu-id="0e821-166">Ellenőrizze, hogy sikeres volt-e hello csere és hello meghajtó működési.</span><span class="sxs-lookup"><span data-stu-id="0e821-166">Verify that hello replacement was successful and hello drive is operational.</span></span> <span data-ttu-id="0e821-167">Hello Azure-portál eléréséhez, és keresse meg a túl**beállítások** > **hardver állapotának**.</span><span class="sxs-lookup"><span data-stu-id="0e821-167">Access hello Azure portal and navigate too**Settings** > **Hardware health**.</span></span> <span data-ttu-id="0e821-168">A **összetevők megosztott** vagy **EBOD megosztott összetevők**, hello meghajtó állapota zöld, jelezve, hogy kifogástalan kell lennie.</span><span class="sxs-lookup"><span data-stu-id="0e821-168">Under **Shared components** or **EBOD shared components**, hello drive status should be green, indicating that it is healthy.</span></span>
<!---Loc Comment: It seems it should say "Device settings > Hardware health" instead of "Settings > Hardware health"---->
   
   > [!NOTE]
   > <span data-ttu-id="0e821-169">Ez több óráig is eltarthat a hello lemez állapota tooturn zöld hello csere utáni.</span><span class="sxs-lookup"><span data-stu-id="0e821-169">It may take several hours for hello disk status tooturn green after hello replacement.</span></span>
  
## <a name="next-steps"></a><span data-ttu-id="0e821-170">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="0e821-170">Next steps</span></span>
<span data-ttu-id="0e821-171">További információ [StorSimple hardver összetevő cseréje](storsimple-8000-hardware-component-replacement.md).</span><span class="sxs-lookup"><span data-stu-id="0e821-171">Learn more about [StorSimple hardware component replacement](storsimple-8000-hardware-component-replacement.md).</span></span>

