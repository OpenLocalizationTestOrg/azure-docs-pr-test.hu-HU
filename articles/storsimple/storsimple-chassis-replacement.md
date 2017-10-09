---
title: "a StorSimple 8000 series eszköz aaaReplace váz |} Microsoft Docs"
description: "Ismerteti, hogyan tooremove és csere hello váz a StorSimple elsődleges ház vagy EBOD ház."
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
ms.openlocfilehash: f8576d63520a6f7d3267180d2a68d4fc38fd48fa
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="replace-hello-chassis-on-your-storsimple-device"></a><span data-ttu-id="c3845-103">Cserélje le a StorSimple eszköz hello készülékház</span><span class="sxs-lookup"><span data-stu-id="c3845-103">Replace hello chassis on your StorSimple device</span></span>
## <a name="overview"></a><span data-ttu-id="c3845-104">Áttekintés</span><span class="sxs-lookup"><span data-stu-id="c3845-104">Overview</span></span>
<span data-ttu-id="c3845-105">Ez az oktatóanyag azt ismerteti, hogyan tooremove, és cserélje le a StorSimple 8000 series eszköz eszközök befogadására szolgáló vázat.</span><span class="sxs-lookup"><span data-stu-id="c3845-105">This tutorial explains how tooremove and replace a chassis in a StorSimple 8000 series device.</span></span> <span data-ttu-id="c3845-106">a StorSimple 8100 hello modell történik egyetlen ház (egy házban csoportosítva), mivel hello 8600 kettős ház eszköz (két váz).</span><span class="sxs-lookup"><span data-stu-id="c3845-106">hello StorSimple 8100 model is a single enclosure device (one chassis), whereas hello 8600 is a dual enclosure device (two chassis).</span></span> <span data-ttu-id="c3845-107">Egy 8600 modell nincsenek potenciálisan hello eszköz feladatátadáshoz két váz: hello elsődleges ház a váz vagy a hello EBOD ház hello váz hello.</span><span class="sxs-lookup"><span data-stu-id="c3845-107">For an 8600 model, there are potentially two chassis that could fail in hello device: hello chassis for hello primary enclosure or hello chassis for hello EBOD enclosure.</span></span>

<span data-ttu-id="c3845-108">Mindkét esetben van a Microsoft által szállított hello helyettesítő váz mező üres.</span><span class="sxs-lookup"><span data-stu-id="c3845-108">In either case, hello replacement chassis that is shipped by Microsoft is empty.</span></span> <span data-ttu-id="c3845-109">Nincs energia- és hűtési modulok (PCMs), a tartományvezérlő modulok, tartós állapotú meghajtók (SSD), a merevlemezes (HDD) meghajtók vagy a EBOD modulokat is fog szerepelni.</span><span class="sxs-lookup"><span data-stu-id="c3845-109">No Power and Cooling Modules (PCMs), controller modules, solid state disk drives (SSDs), hard disk drives (HDDs), or EBOD modules will be included.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="c3845-110">Mielőtt eltávolítása és cseréje hello váz, tekintse át a biztonsági információk hello [StorSimple hardver összetevő cseréje](storsimple-hardware-component-replacement.md).</span><span class="sxs-lookup"><span data-stu-id="c3845-110">Before removing and replacing hello chassis, review hello safety information in [StorSimple hardware component replacement](storsimple-hardware-component-replacement.md).</span></span>
> 
> 

## <a name="remove-hello-chassis"></a><span data-ttu-id="c3845-111">Távolítsa el a hello készülékház</span><span class="sxs-lookup"><span data-stu-id="c3845-111">Remove hello chassis</span></span>
<span data-ttu-id="c3845-112">Hajtsa végre a következő lépések tooremove hello váz a StorSimple eszköz hello.</span><span class="sxs-lookup"><span data-stu-id="c3845-112">Perform hello following steps tooremove hello chassis on your StorSimple device.</span></span>

#### <a name="tooremove-a-chassis"></a><span data-ttu-id="c3845-113">a váz tooremove</span><span class="sxs-lookup"><span data-stu-id="c3845-113">tooremove a chassis</span></span>
1. <span data-ttu-id="c3845-114">Győződjön meg arról, hogy hello StorSimple eszköz állítsa le és minden hello áramforrásokból bontotta a kapcsolatot.</span><span class="sxs-lookup"><span data-stu-id="c3845-114">Make sure that hello StorSimple device is shut down and disconnected from all hello power sources.</span></span>
2. <span data-ttu-id="c3845-115">Távolítsa el a hello hálózati és a SAS-kábel, ha van ilyen.</span><span class="sxs-lookup"><span data-stu-id="c3845-115">Remove all hello network and SAS cables, if applicable.</span></span>
3. <span data-ttu-id="c3845-116">Hello állvány hello egység eltávolítása.</span><span class="sxs-lookup"><span data-stu-id="c3845-116">Remove hello unit from hello rack.</span></span>
4. <span data-ttu-id="c3845-117">Távolítsa el a hello meghajtókhoz, és jegyezze fel a hello üzembe helyezési ponti, amelyről el.</span><span class="sxs-lookup"><span data-stu-id="c3845-117">Remove each of hello drives and note hello slots from which they are removed.</span></span> <span data-ttu-id="c3845-118">További információkért lásd: [hello a lemezmeghajtó eltávolítása](storsimple-disk-drive-replacement.md#remove-the-disk-drive).</span><span class="sxs-lookup"><span data-stu-id="c3845-118">For more information, see [Remove hello disk drive](storsimple-disk-drive-replacement.md#remove-the-disk-drive).</span></span>
5. <span data-ttu-id="c3845-119">A hello EBOD ház (Ha ez sikertelen hello váz) távolítsa el a hello EBOD vezérlő modulok.</span><span class="sxs-lookup"><span data-stu-id="c3845-119">On hello EBOD enclosure (if this is hello chassis that failed), remove hello EBOD controller modules.</span></span> <span data-ttu-id="c3845-120">További információkért lásd: [távolítsa el az EBOD vezérlőhöz](storsimple-ebod-controller-replacement.md#remove-an-ebod-controller).</span><span class="sxs-lookup"><span data-stu-id="c3845-120">For more information, see [Remove an EBOD controller](storsimple-ebod-controller-replacement.md#remove-an-ebod-controller).</span></span> 
   
    <span data-ttu-id="c3845-121">Hello (Ha ez sikertelen hello váz) elsődleges ház távolítsa hello, tartományvezérlői, és jegyezze fel a hello üzembe helyezési ponti, amelyről el.</span><span class="sxs-lookup"><span data-stu-id="c3845-121">On hello primary enclosure (if this is hello chassis that failed), remove hello controllers and note hello slots from which they are removed.</span></span> <span data-ttu-id="c3845-122">További információkért lásd: [távolítsa el a tartományvezérlő](storsimple-controller-replacement.md#remove-a-controller).</span><span class="sxs-lookup"><span data-stu-id="c3845-122">For more information, see [Remove a controller](storsimple-controller-replacement.md#remove-a-controller).</span></span>

## <a name="install-hello-chassis"></a><span data-ttu-id="c3845-123">Hello váz telepítése</span><span class="sxs-lookup"><span data-stu-id="c3845-123">Install hello chassis</span></span>
<span data-ttu-id="c3845-124">Hajtsa végre a következő lépések tooinstall hello váz a StorSimple eszköz hello.</span><span class="sxs-lookup"><span data-stu-id="c3845-124">Perform hello following steps tooinstall hello chassis on your StorSimple device.</span></span>

#### <a name="tooinstall-a-chassis"></a><span data-ttu-id="c3845-125">a váz tooinstall</span><span class="sxs-lookup"><span data-stu-id="c3845-125">tooinstall a chassis</span></span>
1. <span data-ttu-id="c3845-126">Hello váz hello szekrényben csatlakoztatni.</span><span class="sxs-lookup"><span data-stu-id="c3845-126">Mount hello chassis in hello rack.</span></span> <span data-ttu-id="c3845-127">További információkért lásd: [állvány csatlakoztatást a StorSimple 8100 eszköz](storsimple-8100-hardware-installation.md#rack-mount-your-storsimple-8100-device) vagy [állvány csatlakoztatást a StorSimple 8600 eszköz](storsimple-8600-hardware-installation.md#rack-mount-your-storsimple-8600-device).</span><span class="sxs-lookup"><span data-stu-id="c3845-127">For more information, see [Rack-mount your StorSimple 8100 device](storsimple-8100-hardware-installation.md#rack-mount-your-storsimple-8100-device) or [Rack-mount your StorSimple 8600 device](storsimple-8600-hardware-installation.md#rack-mount-your-storsimple-8600-device).</span></span>
2. <span data-ttu-id="c3845-128">Után hello váz hello szekrényben csatlakoztatva van, a hello vezérlő modulok telepítése a hello azonos pozíciót, hogy azok volt előzőleg telepítve.</span><span class="sxs-lookup"><span data-stu-id="c3845-128">After hello chassis is mounted in hello rack, install hello controller modules in hello same positions that they were previously installed in.</span></span>
3. <span data-ttu-id="c3845-129">Telepítés hello meghajtókat a hello azonos pozíciót és a tárolóhely, hogy azok volt előzőleg telepítve.</span><span class="sxs-lookup"><span data-stu-id="c3845-129">Install hello drives in hello same positions and slots that they were previously installed in.</span></span>
   
   > [!NOTE]
   > <span data-ttu-id="c3845-130">Azt javasoljuk, hogy először telepítse a hello SSD hello tárolóhelyre, és telepítse a hello HDD.</span><span class="sxs-lookup"><span data-stu-id="c3845-130">We recommend that you install hello SSDs in hello slots first, and then install hello HDDs.</span></span>
   > 
   > 
4. <span data-ttu-id="c3845-131">Hello eszköz csatlakoztatva hello állvány és hello összetevője telepítve van az eszköz toohello megfelelő áramforrásokból csatlakozzon, és kapcsolja be hello eszközt.</span><span class="sxs-lookup"><span data-stu-id="c3845-131">With hello device mounted in hello rack and hello components installed, connect your device toohello appropriate power sources, and turn on hello device.</span></span> <span data-ttu-id="c3845-132">További információkért lásd: [StorSimple 8100 Bekábelezésére](storsimple-8100-hardware-installation.md#cable-your-storsimple-8100-device) vagy [StorSimple 8600 Bekábelezésére](storsimple-8600-hardware-installation.md#cable-your-storsimple-8600-device).</span><span class="sxs-lookup"><span data-stu-id="c3845-132">For details, see [Cable your StorSimple 8100 device](storsimple-8100-hardware-installation.md#cable-your-storsimple-8100-device) or [Cable your StorSimple 8600 device](storsimple-8600-hardware-installation.md#cable-your-storsimple-8600-device).</span></span>

## <a name="next-steps"></a><span data-ttu-id="c3845-133">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="c3845-133">Next steps</span></span>
<span data-ttu-id="c3845-134">További információ [StorSimple hardver összetevő cseréje](storsimple-hardware-component-replacement.md).</span><span class="sxs-lookup"><span data-stu-id="c3845-134">Learn more about [StorSimple hardware component replacement](storsimple-hardware-component-replacement.md).</span></span>

