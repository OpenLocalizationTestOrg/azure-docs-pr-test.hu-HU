---
title: "Cserélje le a StorSimple 8000 series eszközön váz |} Microsoft Docs"
description: "Távolítsa el, és cserélje le a StorSimple elsődleges ház vagy EBOD ház váz ismerteti."
services: storsimple
documentationcenter: 
author: alkohli
manager: carmonm
editor: 
ms.assetid: 537659ed-4c46-49c1-b1e4-186262f2542d
ms.service: storsimple
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: TBD
ms.date: 08/17/2016
ms.author: alkohli
ms.openlocfilehash: 5295c5dd039b1d4746ebaaf90372932e4c3e7c26
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="replace-the-chassis-on-your-storsimple-device"></a><span data-ttu-id="0b7df-103">Cserélje le a StorSimple eszköz készülékház</span><span class="sxs-lookup"><span data-stu-id="0b7df-103">Replace the chassis on your StorSimple device</span></span>
## <a name="overview"></a><span data-ttu-id="0b7df-104">Áttekintés</span><span class="sxs-lookup"><span data-stu-id="0b7df-104">Overview</span></span>
<span data-ttu-id="0b7df-105">Ez az oktatóanyag azt ismerteti, hogyan cserélje ki a StorSimple 8000 series eszköz eszközök befogadására szolgáló vázat.</span><span class="sxs-lookup"><span data-stu-id="0b7df-105">This tutorial explains how to remove and replace a chassis in a StorSimple 8000 series device.</span></span> <span data-ttu-id="0b7df-106">A StorSimple 8100 modell történik egyetlen ház (egy házban csoportosítva), mivel a 8600 kettős ház eszköz (két váz).</span><span class="sxs-lookup"><span data-stu-id="0b7df-106">The StorSimple 8100 model is a single enclosure device (one chassis), whereas the 8600 is a dual enclosure device (two chassis).</span></span> <span data-ttu-id="0b7df-107">Egy 8600 modell nincsenek esetleg sikertelen lehet, az eszköz két váz: az elsődleges ház vagy a EBOD ház a váz váz.</span><span class="sxs-lookup"><span data-stu-id="0b7df-107">For an 8600 model, there are potentially two chassis that could fail in the device: the chassis for the primary enclosure or the chassis for the EBOD enclosure.</span></span>

<span data-ttu-id="0b7df-108">Mindkét esetben a helyettesítő váz van a Microsoft által szállított mező üres.</span><span class="sxs-lookup"><span data-stu-id="0b7df-108">In either case, the replacement chassis that is shipped by Microsoft is empty.</span></span> <span data-ttu-id="0b7df-109">Nincs energia- és hűtési modulok (PCMs), a tartományvezérlő modulok, tartós állapotú meghajtók (SSD), a merevlemezes (HDD) meghajtók vagy a EBOD modulokat is fog szerepelni.</span><span class="sxs-lookup"><span data-stu-id="0b7df-109">No Power and Cooling Modules (PCMs), controller modules, solid state disk drives (SSDs), hard disk drives (HDDs), or EBOD modules will be included.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="0b7df-110">Mielőtt eltávolítása és cseréje a váz, tekintse át a biztonsági információk [StorSimple hardver összetevő cseréje](storsimple-hardware-component-replacement.md).</span><span class="sxs-lookup"><span data-stu-id="0b7df-110">Before removing and replacing the chassis, review the safety information in [StorSimple hardware component replacement](storsimple-hardware-component-replacement.md).</span></span>
> 
> 

## <a name="remove-the-chassis"></a><span data-ttu-id="0b7df-111">A váz eltávolítása</span><span class="sxs-lookup"><span data-stu-id="0b7df-111">Remove the chassis</span></span>
<span data-ttu-id="0b7df-112">A következő lépésekkel távolítsa el a StorSimple eszköz váz.</span><span class="sxs-lookup"><span data-stu-id="0b7df-112">Perform the following steps to remove the chassis on your StorSimple device.</span></span>

#### <a name="to-remove-a-chassis"></a><span data-ttu-id="0b7df-113">A váz eltávolítása</span><span class="sxs-lookup"><span data-stu-id="0b7df-113">To remove a chassis</span></span>
1. <span data-ttu-id="0b7df-114">Győződjön meg arról, hogy a StorSimple eszköz leállítása és bontotta a kapcsolatot a áramforrásokból.</span><span class="sxs-lookup"><span data-stu-id="0b7df-114">Make sure that the StorSimple device is shut down and disconnected from all the power sources.</span></span>
2. <span data-ttu-id="0b7df-115">Távolítsa el a hálózat és a SAS-kábel, ha van ilyen.</span><span class="sxs-lookup"><span data-stu-id="0b7df-115">Remove all the network and SAS cables, if applicable.</span></span>
3. <span data-ttu-id="0b7df-116">Távolítsa el az egység a állvány.</span><span class="sxs-lookup"><span data-stu-id="0b7df-116">Remove the unit from the rack.</span></span>
4. <span data-ttu-id="0b7df-117">Távolítsa el a meghajtókhoz, és jegyezze fel a tárhelyek, amelyről el.</span><span class="sxs-lookup"><span data-stu-id="0b7df-117">Remove each of the drives and note the slots from which they are removed.</span></span> <span data-ttu-id="0b7df-118">További információkért lásd: [távolítsa el a merevlemez](storsimple-disk-drive-replacement.md#remove-the-disk-drive).</span><span class="sxs-lookup"><span data-stu-id="0b7df-118">For more information, see [Remove the disk drive](storsimple-disk-drive-replacement.md#remove-the-disk-drive).</span></span>
5. <span data-ttu-id="0b7df-119">A (Ha ez a váz sikertelen) EBOD tárolóeszközön távolítsa el a EBOD vezérlő modulok.</span><span class="sxs-lookup"><span data-stu-id="0b7df-119">On the EBOD enclosure (if this is the chassis that failed), remove the EBOD controller modules.</span></span> <span data-ttu-id="0b7df-120">További információkért lásd: [távolítsa el az EBOD vezérlőhöz](storsimple-ebod-controller-replacement.md#remove-an-ebod-controller).</span><span class="sxs-lookup"><span data-stu-id="0b7df-120">For more information, see [Remove an EBOD controller](storsimple-ebod-controller-replacement.md#remove-an-ebod-controller).</span></span> 
   
    <span data-ttu-id="0b7df-121">A elsődleges tárolóeszközön (Ha ez a váz sikertelenül) távolítsa el a tartományvezérlők, és jegyezze fel a tárhelyek, amelyről el.</span><span class="sxs-lookup"><span data-stu-id="0b7df-121">On the primary enclosure (if this is the chassis that failed), remove the controllers and note the slots from which they are removed.</span></span> <span data-ttu-id="0b7df-122">További információkért lásd: [távolítsa el a tartományvezérlő](storsimple-controller-replacement.md#remove-a-controller).</span><span class="sxs-lookup"><span data-stu-id="0b7df-122">For more information, see [Remove a controller](storsimple-controller-replacement.md#remove-a-controller).</span></span>

## <a name="install-the-chassis"></a><span data-ttu-id="0b7df-123">A váz telepítése</span><span class="sxs-lookup"><span data-stu-id="0b7df-123">Install the chassis</span></span>
<span data-ttu-id="0b7df-124">Telepítéséhez hajtsa végre a következő lépéseket a váz a StorSimple eszköz.</span><span class="sxs-lookup"><span data-stu-id="0b7df-124">Perform the following steps to install the chassis on your StorSimple device.</span></span>

#### <a name="to-install-a-chassis"></a><span data-ttu-id="0b7df-125">A váz telepítése</span><span class="sxs-lookup"><span data-stu-id="0b7df-125">To install a chassis</span></span>
1. <span data-ttu-id="0b7df-126">Csatlakoztassa az állvány váz.</span><span class="sxs-lookup"><span data-stu-id="0b7df-126">Mount the chassis in the rack.</span></span> <span data-ttu-id="0b7df-127">További információkért lásd: [állvány csatlakoztatást a StorSimple 8100 eszköz](storsimple-8100-hardware-installation.md#rack-mount-your-storsimple-8100-device) vagy [állvány csatlakoztatást a StorSimple 8600 eszköz](storsimple-8600-hardware-installation.md#rack-mount-your-storsimple-8600-device).</span><span class="sxs-lookup"><span data-stu-id="0b7df-127">For more information, see [Rack-mount your StorSimple 8100 device](storsimple-8100-hardware-installation.md#rack-mount-your-storsimple-8100-device) or [Rack-mount your StorSimple 8600 device](storsimple-8600-hardware-installation.md#rack-mount-your-storsimple-8600-device).</span></span>
2. <span data-ttu-id="0b7df-128">A váz csatlakoztatva van, az állvány, miután a tartományvezérlő-modulok telepítése azokat korábban telepített azonos pozícióra.</span><span class="sxs-lookup"><span data-stu-id="0b7df-128">After the chassis is mounted in the rack, install the controller modules in the same positions that they were previously installed in.</span></span>
3. <span data-ttu-id="0b7df-129">Telepítse a meghajtók azonos pozíciók és üzembe helyezési ponti, akkor a korábban telepített.</span><span class="sxs-lookup"><span data-stu-id="0b7df-129">Install the drives in the same positions and slots that they were previously installed in.</span></span>
   
   > [!NOTE]
   > <span data-ttu-id="0b7df-130">Azt javasoljuk, hogy a tárolóhelyek az SSD-k először telepíteni, és telepítse a HDD-k.</span><span class="sxs-lookup"><span data-stu-id="0b7df-130">We recommend that you install the SSDs in the slots first, and then install the HDDs.</span></span>
   > 
   > 
4. <span data-ttu-id="0b7df-131">Az eszköz, az állvány és a telepített összetevők-e csatlakoztatva csatlakoztassa az eszközt a megfelelő áramforrásokból, és kapcsolja be az eszközt.</span><span class="sxs-lookup"><span data-stu-id="0b7df-131">With the device mounted in the rack and the components installed, connect your device to the appropriate power sources, and turn on the device.</span></span> <span data-ttu-id="0b7df-132">További információkért lásd: [StorSimple 8100 Bekábelezésére](storsimple-8100-hardware-installation.md#cable-your-storsimple-8100-device) vagy [StorSimple 8600 Bekábelezésére](storsimple-8600-hardware-installation.md#cable-your-storsimple-8600-device).</span><span class="sxs-lookup"><span data-stu-id="0b7df-132">For details, see [Cable your StorSimple 8100 device](storsimple-8100-hardware-installation.md#cable-your-storsimple-8100-device) or [Cable your StorSimple 8600 device](storsimple-8600-hardware-installation.md#cable-your-storsimple-8600-device).</span></span>

## <a name="next-steps"></a><span data-ttu-id="0b7df-133">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="0b7df-133">Next steps</span></span>
<span data-ttu-id="0b7df-134">További információ [StorSimple hardver összetevő cseréje](storsimple-hardware-component-replacement.md).</span><span class="sxs-lookup"><span data-stu-id="0b7df-134">Learn more about [StorSimple hardware component replacement](storsimple-hardware-component-replacement.md).</span></span>

