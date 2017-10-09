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
# <a name="replace-hello-chassis-on-your-storsimple-device"></a>Cserélje le a StorSimple eszköz hello készülékház
## <a name="overview"></a>Áttekintés
Ez az oktatóanyag azt ismerteti, hogyan tooremove, és cserélje le a StorSimple 8000 series eszköz eszközök befogadására szolgáló vázat. a StorSimple 8100 hello modell történik egyetlen ház (egy házban csoportosítva), mivel hello 8600 kettős ház eszköz (két váz). Egy 8600 modell nincsenek potenciálisan hello eszköz feladatátadáshoz két váz: hello elsődleges ház a váz vagy a hello EBOD ház hello váz hello.

Mindkét esetben van a Microsoft által szállított hello helyettesítő váz mező üres. Nincs energia- és hűtési modulok (PCMs), a tartományvezérlő modulok, tartós állapotú meghajtók (SSD), a merevlemezes (HDD) meghajtók vagy a EBOD modulokat is fog szerepelni.

> [!IMPORTANT]
> Mielőtt eltávolítása és cseréje hello váz, tekintse át a biztonsági információk hello [StorSimple hardver összetevő cseréje](storsimple-hardware-component-replacement.md).
> 
> 

## <a name="remove-hello-chassis"></a>Távolítsa el a hello készülékház
Hajtsa végre a következő lépések tooremove hello váz a StorSimple eszköz hello.

#### <a name="tooremove-a-chassis"></a>a váz tooremove
1. Győződjön meg arról, hogy hello StorSimple eszköz állítsa le és minden hello áramforrásokból bontotta a kapcsolatot.
2. Távolítsa el a hello hálózati és a SAS-kábel, ha van ilyen.
3. Hello állvány hello egység eltávolítása.
4. Távolítsa el a hello meghajtókhoz, és jegyezze fel a hello üzembe helyezési ponti, amelyről el. További információkért lásd: [hello a lemezmeghajtó eltávolítása](storsimple-disk-drive-replacement.md#remove-the-disk-drive).
5. A hello EBOD ház (Ha ez sikertelen hello váz) távolítsa el a hello EBOD vezérlő modulok. További információkért lásd: [távolítsa el az EBOD vezérlőhöz](storsimple-ebod-controller-replacement.md#remove-an-ebod-controller). 
   
    Hello (Ha ez sikertelen hello váz) elsődleges ház távolítsa hello, tartományvezérlői, és jegyezze fel a hello üzembe helyezési ponti, amelyről el. További információkért lásd: [távolítsa el a tartományvezérlő](storsimple-controller-replacement.md#remove-a-controller).

## <a name="install-hello-chassis"></a>Hello váz telepítése
Hajtsa végre a következő lépések tooinstall hello váz a StorSimple eszköz hello.

#### <a name="tooinstall-a-chassis"></a>a váz tooinstall
1. Hello váz hello szekrényben csatlakoztatni. További információkért lásd: [állvány csatlakoztatást a StorSimple 8100 eszköz](storsimple-8100-hardware-installation.md#rack-mount-your-storsimple-8100-device) vagy [állvány csatlakoztatást a StorSimple 8600 eszköz](storsimple-8600-hardware-installation.md#rack-mount-your-storsimple-8600-device).
2. Után hello váz hello szekrényben csatlakoztatva van, a hello vezérlő modulok telepítése a hello azonos pozíciót, hogy azok volt előzőleg telepítve.
3. Telepítés hello meghajtókat a hello azonos pozíciót és a tárolóhely, hogy azok volt előzőleg telepítve.
   
   > [!NOTE]
   > Azt javasoljuk, hogy először telepítse a hello SSD hello tárolóhelyre, és telepítse a hello HDD.
   > 
   > 
4. Hello eszköz csatlakoztatva hello állvány és hello összetevője telepítve van az eszköz toohello megfelelő áramforrásokból csatlakozzon, és kapcsolja be hello eszközt. További információkért lásd: [StorSimple 8100 Bekábelezésére](storsimple-8100-hardware-installation.md#cable-your-storsimple-8100-device) vagy [StorSimple 8600 Bekábelezésére](storsimple-8600-hardware-installation.md#cable-your-storsimple-8600-device).

## <a name="next-steps"></a>Következő lépések
További információ [StorSimple hardver összetevő cseréje](storsimple-hardware-component-replacement.md).

