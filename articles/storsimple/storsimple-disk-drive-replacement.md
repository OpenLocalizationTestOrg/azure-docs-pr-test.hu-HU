---
title: "a StorSimple eszközön lemezmeghajtó aaaReplace |} Microsoft Docs"
description: "Ismerteti, hogyan tooreplace lemez meghajtója StorSimple elsődleges ház vagy egy EBOD ház."
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
ms.openlocfilehash: d2c78a6d951b0f00ac42e74a34cf1bc83952a3c8
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="replace-a-disk-drive-on-your-storsimple-device"></a><span data-ttu-id="ae377-103">Cserélje le a lemezmeghajtó a StorSimple eszköz</span><span class="sxs-lookup"><span data-stu-id="ae377-103">Replace a disk drive on your StorSimple device</span></span>
## <a name="overview"></a><span data-ttu-id="ae377-104">Áttekintés</span><span class="sxs-lookup"><span data-stu-id="ae377-104">Overview</span></span>
<span data-ttu-id="ae377-105">Ez az oktatóanyag azt ismerteti, hogyan távolítsa el, és cserélje le a hibás vagy nem sikerült merevlemez-meghajtó a Microsoft Azure StorSimple eszközön.</span><span class="sxs-lookup"><span data-stu-id="ae377-105">This tutorial explains how you can remove and replace a malfunctioning or failed hard disk drive on a Microsoft Azure StorSimple device.</span></span> <span data-ttu-id="ae377-106">a meghajtó tooreplace, kell:</span><span class="sxs-lookup"><span data-stu-id="ae377-106">tooreplace a disk drive, you need to:</span></span>

* <span data-ttu-id="ae377-107">Hello antitamper zárolási működésűeknek</span><span class="sxs-lookup"><span data-stu-id="ae377-107">Disengage hello antitamper lock</span></span>
* <span data-ttu-id="ae377-108">Távolítsa el a hello a lemezmeghajtó</span><span class="sxs-lookup"><span data-stu-id="ae377-108">Remove hello disk drive</span></span>
* <span data-ttu-id="ae377-109">Hello helyettesítő meghajtó telepítése</span><span class="sxs-lookup"><span data-stu-id="ae377-109">Install hello replacement disk drive</span></span>

> [!IMPORTANT]
> <span data-ttu-id="ae377-110">Mielőtt eltávolítása és cseréje egy meghajtó, tekintse át a biztonsági információk hello [StorSimple hardver összetevő cseréje](storsimple-hardware-component-replacement.md).</span><span class="sxs-lookup"><span data-stu-id="ae377-110">Before removing and replacing a disk drive, review hello safety information in [StorSimple hardware component replacement](storsimple-hardware-component-replacement.md).</span></span>
> 
> 

## <a name="disengage-hello-antitamper-lock"></a><span data-ttu-id="ae377-111">Hello antitamper zárolási működésűeknek</span><span class="sxs-lookup"><span data-stu-id="ae377-111">Disengage hello antitamper lock</span></span>
<span data-ttu-id="ae377-112">Ez az eljárás azt ismerteti, hogyan hello antitamper zárolásokat a StorSimple eszköz is lehet vagy kikapcsolt állapotban hello lemezmeghajtók cseréjekor.</span><span class="sxs-lookup"><span data-stu-id="ae377-112">This procedure explains how hello antitamper locks on your StorSimple device can be engaged or disengaged when you replace hello disk drives.</span></span> <span data-ttu-id="ae377-113">hello antitamper zárolások szerelnek hello meghajtó szolgáltatónként kezelik, és azokat egy kis nyílás hello zárolás szakaszában hello leíró keresztül érik el.</span><span class="sxs-lookup"><span data-stu-id="ae377-113">hello antitamper locks are fitted in hello drive carrier handles, and they are accessed through a small aperture in hello latch section of hello handle.</span></span> <span data-ttu-id="ae377-114">Meghajtók hello zárolások beállított zárolt toohello pozíciót végzik.</span><span class="sxs-lookup"><span data-stu-id="ae377-114">Drives are supplied with hello locks set toohello locked position.</span></span>

#### <a name="toounlock-hello-antitamper-lock"></a><span data-ttu-id="ae377-115">toounlock hello antitamper zárolása</span><span class="sxs-lookup"><span data-stu-id="ae377-115">toounlock hello antitamper lock</span></span>
1. <span data-ttu-id="ae377-116">Gondosan beszúrása hello zárolási kulcs ("tamperproof" T10 csavarhúzót Microsoft biztosított), a hello leíró hello nyílás és a szoftvercsatorna.</span><span class="sxs-lookup"><span data-stu-id="ae377-116">Carefully insert hello lock key (a "tamperproof" T10 screwdriver that Microsoft provided) into hello aperture in hello handle and into its socket.</span></span> 
   
   > [!NOTE]
   > <span data-ttu-id="ae377-117">Ha hello antitamper zárolási aktív, piros hello mutató hello nyílás látható.</span><span class="sxs-lookup"><span data-stu-id="ae377-117">If hello antitamper lock is activated, hello red indicator is visible in hello aperture.</span></span>
   > 
   > 
   
    ![Zárolt meghajtó](./media/storsimple-disk-drive-replacement/IC741056.png)
   
    <span data-ttu-id="ae377-119">**1. ábra** részt vevő elleni illetéktelen zárolása</span><span class="sxs-lookup"><span data-stu-id="ae377-119">**Figure 1** Anti-tamper lock engaged</span></span>
   
   | <span data-ttu-id="ae377-120">Címke</span><span class="sxs-lookup"><span data-stu-id="ae377-120">Label</span></span> | <span data-ttu-id="ae377-121">Leírás</span><span class="sxs-lookup"><span data-stu-id="ae377-121">Description</span></span> |
   |:--- |:--- |
   | <span data-ttu-id="ae377-122">1</span><span class="sxs-lookup"><span data-stu-id="ae377-122">1</span></span> |<span data-ttu-id="ae377-123">Kijelző nyílás</span><span class="sxs-lookup"><span data-stu-id="ae377-123">Indicator aperture</span></span> |
   | <span data-ttu-id="ae377-124">2</span><span class="sxs-lookup"><span data-stu-id="ae377-124">2</span></span> |<span data-ttu-id="ae377-125">Antitamper zárolása</span><span class="sxs-lookup"><span data-stu-id="ae377-125">Antitamper lock</span></span> |
2. <span data-ttu-id="ae377-126">Forgassa el hello kulcs anticlockwise irányba, amíg a piros hello mutató nem látható a fenti hello kulcs hello nyílás.</span><span class="sxs-lookup"><span data-stu-id="ae377-126">Rotate hello key in an anticlockwise direction until hello red indicator is not visible in hello aperture above hello key.</span></span>
3. <span data-ttu-id="ae377-127">Távolítsa el a hello kulcsot.</span><span class="sxs-lookup"><span data-stu-id="ae377-127">Remove hello key.</span></span>
   
    ![Nem zárolt meghajtó](./media/storsimple-disk-drive-replacement/IC741057.png)
   
    <span data-ttu-id="ae377-129">**2. ábra** feloldva lemezmeghajtó</span><span class="sxs-lookup"><span data-stu-id="ae377-129">**Figure 2** Unlocked disk drive</span></span>
4. <span data-ttu-id="ae377-130">hello a lemezmeghajtó most eltávolítható.</span><span class="sxs-lookup"><span data-stu-id="ae377-130">hello disk drive can now be removed.</span></span>

<span data-ttu-id="ae377-131">Hello lépésekkel fordított tooengage hello zárolva.</span><span class="sxs-lookup"><span data-stu-id="ae377-131">Follow hello steps in reverse tooengage hello lock.</span></span>

## <a name="remove-hello-disk-drive"></a><span data-ttu-id="ae377-132">Távolítsa el a hello a lemezmeghajtó</span><span class="sxs-lookup"><span data-stu-id="ae377-132">Remove hello disk drive</span></span>
<span data-ttu-id="ae377-133">A StorSimple eszköz támogatja a RAID 10-szerű szóközöket tárkonfiguráció.</span><span class="sxs-lookup"><span data-stu-id="ae377-133">Your StorSimple device supports a RAID 10-like storage spaces configuration.</span></span> <span data-ttu-id="ae377-134">Ez azt jelenti, hogy azt is megfelelően működik egy meghibásodott lemezt, SSD-meghajtót (SSD), vagy a merevlemez-meghajtó (HDD).</span><span class="sxs-lookup"><span data-stu-id="ae377-134">This implies that it can operate normally with one failed disk, solid-state drive (SSD), or hard disk drive (HDD).</span></span> 

> [!IMPORTANT]
> * <span data-ttu-id="ae377-135">Ha a rendszer több mint egy meghibásodott lemezt, távolítsa el egynél több SSD és HDD hello rendszer bármikor időben.</span><span class="sxs-lookup"><span data-stu-id="ae377-135">If your system has more than one failed disk, do not remove more than one SSD or HDD from hello system at any point in time.</span></span> <span data-ttu-id="ae377-136">Így adatvesztést eredményezhet.</span><span class="sxs-lookup"><span data-stu-id="ae377-136">Doing so could result in loss of data.</span></span>
> * <span data-ttu-id="ae377-137">Ellenőrizze, hogy egy helyettesítő SSD helyez egy SSD korábban tartalmazó tárhely.</span><span class="sxs-lookup"><span data-stu-id="ae377-137">Make sure that you place a replacement SSD in a slot that previously contained an SSD.</span></span> <span data-ttu-id="ae377-138">Hasonlóképpen helyezze HDD helyettesíti, amely korábban tartalmazott egy HDD tárolóhelyen található.</span><span class="sxs-lookup"><span data-stu-id="ae377-138">Similarly, place a replacement HDD in a slot that previously contained an HDD.</span></span>
> * <span data-ttu-id="ae377-139">Hello a klasszikus Azure portálon, a bővítőhelyek számozása 0 – 11.</span><span class="sxs-lookup"><span data-stu-id="ae377-139">In hello Azure classic portal, slots are numbered from 0 – 11.</span></span> <span data-ttu-id="ae377-140">Ezért ha hello portal jeleníti meg, hogy egy lemezt a tárolóhely 2 rendelkezik, az eszközön nem sikerült hello, keresse meg hello harmadik tárolóhely hello meghibásodott lemez a bal felső hello.</span><span class="sxs-lookup"><span data-stu-id="ae377-140">Therefore, if hello portal shows that a disk in slot 2 has failed, on hello device, look for hello failed disk in hello third slot from hello top left.</span></span>
> 
> 

<span data-ttu-id="ae377-141">Meghajtók távolítva, és amíg hello rendszer cserélni.</span><span class="sxs-lookup"><span data-stu-id="ae377-141">Drives can be removed and replaced while hello system is operating.</span></span>

#### <a name="tooremove-a-drive"></a><span data-ttu-id="ae377-142">a meghajtó tooremove</span><span class="sxs-lookup"><span data-stu-id="ae377-142">tooremove a drive</span></span>
1. <span data-ttu-id="ae377-143">tooidentify hello a meghibásodott lemezt, a klasszikus Azure portálon hello, nyissa meg túl**eszközök** > **karbantartási** > **hardverállapot**.</span><span class="sxs-lookup"><span data-stu-id="ae377-143">tooidentify hello failed disk, in hello Azure classic portal, go too**Devices** > **Maintenance** > **Hardware Status**.</span></span> <span data-ttu-id="ae377-144">Hello lemezei hello állapotának tekintse meg a lemez sikertelen lehet hello elsődleges ház és/vagy egy EBOD ház (Ha egy 8600 modellt használ), mert **megosztott összetevők** és a **EBOD ház megosztott összetevők**.</span><span class="sxs-lookup"><span data-stu-id="ae377-144">Because a disk can fail in hello primary enclosure and/or in an EBOD enclosure (if you are using a 8600 model), look at hello status of hello disks under **Shared Components** and under **EBOD enclosure Shared Components**.</span></span> <span data-ttu-id="ae377-145">A hibás lemez vagy szolgáltatással vörös állapottal jelenik meg.</span><span class="sxs-lookup"><span data-stu-id="ae377-145">A failed disk in either enclosure will be shown with a red status.</span></span>
2. <span data-ttu-id="ae377-146">Keresse meg hello meghajtók hello elsődleges ház vagy hello EBOD ház hello elejéhez.</span><span class="sxs-lookup"><span data-stu-id="ae377-146">Locate hello drives in hello front of hello primary enclosure or hello EBOD enclosure.</span></span> 
3. <span data-ttu-id="ae377-147">Hello lemez fel oldva, folytassa a következő lépésre toohello műveletet.</span><span class="sxs-lookup"><span data-stu-id="ae377-147">If hello disk is unlocked, proceed toohello next step.</span></span> <span data-ttu-id="ae377-148">Hello lemez le van tiltva, ha a zárolást a hello eljárást követve [hello antitamper zárolási működésűeknek](#disengage-the-antitamper-lock).</span><span class="sxs-lookup"><span data-stu-id="ae377-148">If hello disk is locked, unlock it by following hello procedure in [Disengage hello antitamper lock](#disengage-the-antitamper-lock).</span></span>
4. <span data-ttu-id="ae377-149">Nyomja le az hello fekete hello meghajtó szolgáltatónként modul zárolni, és hello meghajtó szolgáltatónként leíró számítógépnél ki és lekéréses hello Váztípus hello elölről.</span><span class="sxs-lookup"><span data-stu-id="ae377-149">Press hello black latch on hello drive carrier module and pull hello drive carrier handle out and away from hello front of hello chassis.</span></span> 
   
    ![Lemezmeghajtó leíró feloldása](./media/storsimple-disk-drive-replacement/IC741051.png)
   
    <span data-ttu-id="ae377-151">**3. ábra** felszabadításával hello meghajtó leíró</span><span class="sxs-lookup"><span data-stu-id="ae377-151">**Figure 3** Releasing hello drive handle</span></span>
5. <span data-ttu-id="ae377-152">Ha teljesen ki van bővítve a hello meghajtó szolgáltatónként leíró, csúsztassa be hello meghajtó szolgáltatónként hello váz kívül.</span><span class="sxs-lookup"><span data-stu-id="ae377-152">When hello drive carrier handle is fully extended, slide hello drive carrier out of hello chassis.</span></span> 
   
    ![A késleltetett lemez Lemezmeghajtó kívül](./media/storsimple-disk-drive-replacement/IC741052.png)
   
    <span data-ttu-id="ae377-154">**4. ábra** késleltetett hello meghajtó kívül hello szolgáltatója</span><span class="sxs-lookup"><span data-stu-id="ae377-154">**Figure 4** Sliding hello disk drive out of hello carrier</span></span>

## <a name="install-hello-replacement-disk-drive"></a><span data-ttu-id="ae377-155">Hello helyettesítő meghajtó telepítése</span><span class="sxs-lookup"><span data-stu-id="ae377-155">Install hello replacement disk drive</span></span>
<span data-ttu-id="ae377-156">Miután egy meghajtót a StorSimple eszközt a sikertelen volt, és törölte, hajtsa végre az ezen eljárás tooreplace azt egy új meghajtót.</span><span class="sxs-lookup"><span data-stu-id="ae377-156">After a drive has failed in your StorSimple device and you have removed it, follow this procedure tooreplace it with a new drive.</span></span>

#### <a name="tooinsert-a-drive"></a><span data-ttu-id="ae377-157">a meghajtó tooinsert</span><span class="sxs-lookup"><span data-stu-id="ae377-157">tooinsert a drive</span></span>
1. <span data-ttu-id="ae377-158">Győződjön meg arról, hello meghajtó szolgáltatónként leíró teljesen ki van bővítve, ahogy az a következő kép hello.</span><span class="sxs-lookup"><span data-stu-id="ae377-158">Ensure hello drive carrier handle is fully extended, as shown in hello following image.</span></span>
   
    ![A kiterjesztett leíró meghajtó](./media/storsimple-disk-drive-replacement/IC741044.png)
   
    <span data-ttu-id="ae377-160">**5. ábra** kiterjesztett leíró rendelkező meghajtóra</span><span class="sxs-lookup"><span data-stu-id="ae377-160">**Figure 5** Drive with handle extended</span></span>
2. <span data-ttu-id="ae377-161">Csúsztassa be a hello meghajtó szolgáltatónként összes hello módon történő hello váz.</span><span class="sxs-lookup"><span data-stu-id="ae377-161">Slide hello drive carrier all hello way into hello chassis.</span></span> 
   
    ![A meghajtó szolgáltatónként mozgó lemez](./media/storsimple-disk-drive-replacement/IC741045.png)
   
    <span data-ttu-id="ae377-163">**6. ábra** csúszó hello meghajtó szolgáltatónként hello váz be</span><span class="sxs-lookup"><span data-stu-id="ae377-163">**Figure 6**  Sliding hello drive carrier into hello chassis</span></span>
3. <span data-ttu-id="ae377-164">Hello meghajtó vivőjel zárja be, beszúrt hello meghajtó szolgáltatónként leírójú folyamatos toopush hello meghajtó szolgáltatónként hello váz, amíg nem hello meghajtó szolgáltatónként leíró zárolt helyzetbe illeszkedik az közben.</span><span class="sxs-lookup"><span data-stu-id="ae377-164">With hello drive carrier inserted, close hello drive carrier handle while continuing toopush hello drive carrier into hello chassis, until hello drive carrier handle snaps into a locked position.</span></span>
4. <span data-ttu-id="ae377-165">Hello zárolási kulcs használata által biztosított Microsoft (tamperproof Torx csavarhúzóra) toosecure hello szolgáltatónként leíró helyen bekapcsolásával hello zárolási csavart negyedéves kapcsolja jobbra.</span><span class="sxs-lookup"><span data-stu-id="ae377-165">Use hello lock key that was provided by Microsoft (tamperproof Torx screwdriver) toosecure hello carrier handle into place by turning hello lock screw a quarter turn clockwise.</span></span>
5. <span data-ttu-id="ae377-166">Ellenőrizze, hogy sikeres volt-e hello csere és hello meghajtó működési hello klasszikus Azure-portál elérése és a navigáció túl**karbantartási** > **hardverállapot**.</span><span class="sxs-lookup"><span data-stu-id="ae377-166">Verify that hello replacement was successful and hello drive is operational by accessing hello Azure classic portal and navigating too**Maintenance** > **Hardware Status**.</span></span> <span data-ttu-id="ae377-167">A **megosztott összetevők** vagy **EBOD ház megosztott összetevők**, hello meghajtó állapota zöld, jelezve, hogy kifogástalan kell lennie.</span><span class="sxs-lookup"><span data-stu-id="ae377-167">Under **Shared Components** or **EBOD enclosure Shared Components**, hello drive status should be green, indicating that it is healthy.</span></span>
   
   > [!NOTE]
   > <span data-ttu-id="ae377-168">Ez több óráig is eltarthat a hello lemez állapota tooturn zöld hello csere utáni.</span><span class="sxs-lookup"><span data-stu-id="ae377-168">It may take several hours for hello disk status tooturn green after hello replacement.</span></span>
   > 
   > 

## <a name="next-steps"></a><span data-ttu-id="ae377-169">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="ae377-169">Next steps</span></span>
<span data-ttu-id="ae377-170">További információ [StorSimple hardver összetevő cseréje](storsimple-hardware-component-replacement.md).</span><span class="sxs-lookup"><span data-stu-id="ae377-170">Learn more about [StorSimple hardware component replacement](storsimple-hardware-component-replacement.md).</span></span>

