---
title: "aaaUpdate a StorSimple eszköz |} Microsoft Docs"
description: "Ismerteti, hogyan toouse hello StorSimple frissítése szolgáltatás tooinstall rendszeres és karbantartási mód frissítéseket és gyorsjavításokat."
services: storsimple
documentationcenter: NA
author: SharS
manager: carmonm
editor: 
ms.assetid: 786059f5-2a38-4105-941d-0860ce4ac515
ms.service: storsimple
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: TBD
ms.date: 11/18/2016
ms.author: v-sharos
ms.openlocfilehash: 05acf05c8fc89bbb4343f67ad103235bbe3dba0a
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="update-your-storsimple-8000-series-device"></a>A StorSimple 8000 Series eszköz frissítése
## <a name="overview"></a>Áttekintés
hello StorSimple frissítések a szolgáltatások lehetővé teszik tooeasily a StorSimple eszköz naprakész állapotban tartása. Hello frissítés típusától függően a frissítések toohello eszköz hello Windows PowerShell felületén keresztül vagy hello a klasszikus Azure portálon keresztül is alkalmazhat. Ez az oktatóanyag leírja hello frissítési típusait, és hogyan tooinstall minden közülük.

Kétféle eszközfrissítésekhez alkalmazhatja: 

* Rendszeres (vagy normál módú) frissítéseket
* Karbantartási mód frissítések

A klasszikus Azure portálon hello vagy a Windows PowerShell; keresztül rendszeres frissítések telepítése a Windows PowerShell tooinstall karbantartási mód frissítések kell használnia. 

Minden egyes frissítés típus külön-külön alább.

### <a name="regular-updates"></a>Rendszeres frissítések
Rendszeres frissítéseket tudja telepíteni, ha hello eszköz normál üzemmódban nem zavaró frissítések érhetők el. Ezek a frissítések hello Microsoft Update webhely tooeach eszköz vezérlő keresztül lettek alkalmazva. 

> [!IMPORTANT]
> A vezérlő feladatátvétel hello frissítési folyamat alatt fordulhat elő. Azonban ez nem érinti rendszer rendelkezésre állás vagy a műveletet.
> 
> 

* További részletek a tooinstall regular frissítéséről hello segítségével a klasszikus Azure portálon: [hello a klasszikus Azure portálon keresztül rendszeres frissítések telepítése](#install-regular-updates-via-the-azure-classic-portal).
* A StorSimple rendszeres frissítéseket a Windows PowerShell használatával is telepíthet. További információkért lásd: [telepítse a Windows PowerShell segítségével rendszeres frissítéseket StorSimple](#install-regular-updates-via-windows-powershell-for-storsimple).

### <a name="maintenance-mode-updates"></a>Karbantartási mód frissítések
Karbantartási mód frissítések például a belső vezérlőprogram-frissítések lemez zavaró frissítések érhetők el. Ezek a frissítések megkövetelése hello eszköz toobe állítható karbantartási üzemmódba. További információkért lásd: [2. lépés: Adja meg a karbantartási mód](#step2). Hello Azure klasszikus portál tooinstall karbantartási mód frissítések nem használható. Ehelyett a StorSimple Windows PowerShell kell használnia. 

További részletek a karbantartási mód tooinstall frissítéséről: [telepítése karbantartási mód frissítések keresztül a Windows PowerShell-lel](#install-maintenance-mode-updates-via-windows-powershell-for-storsimple).

> [!IMPORTANT]
> Karbantartási módban kell lennie külön-külön alkalmazott tooeach vezérlő. 
> 
> 

## <a name="install-regular-updates-via-hello-azure-classic-portal"></a>Hello a klasszikus Azure portálon keresztül rendszeres frissítések telepítése
Hello Azure klasszikus portál tooapply frissítések tooyour StorSimple eszközt is használhatja.

[!INCLUDE [storsimple-install-updates-manually](../../includes/storsimple-install-updates-manually.md)]

## <a name="install-regular-updates-via-windows-powershell-for-storsimple"></a>Telepítse a Windows PowerShell segítségével rendszeres frissítéseket StorSimple
Alternatív megoldásként használható Windows PowerShell StorSimple tooapply (normál mód) rendszeres frissítések.

> [!IMPORTANT]
> Bár a Windows PowerShell használatával a StorSimple rendszeres frissítések telepítése erősen ajánlott hello a klasszikus Azure portálon keresztül rendszeres frissítések telepítése. Frissítés 1-gyel kezdődően előzetes ellenőrzése fogja elvégezni előzetes tooinstalling frissítések hello portálról. Az előzetes ellenőrzések megelőzik az hibák, és a gördülékenyebb élményt nyújtsanak. 
> 
> 

[!INCLUDE [storsimple-install-regular-updates-powershell](../../includes/storsimple-install-regular-updates-powershell.md)]

## <a name="install-maintenance-mode-updates-via-windows-powershell-for-storsimple"></a>Telepítse a StorSimple karbantartási mód frissítéseket a Windows PowerShell segítségével
A Windows PowerShell használata tooapply karbantartási mód frissítések tooyour StorSimple eszközét. Az összes i/o-kérelmek fel van függesztve, ebben a módban. Például nem felejtő közvetlen elérésű memória (NVRAM) vagy a fürtszolgáltatás hello szolgáltatás is le van állítva. Mindkét tartományvezérlők újraindítása van, adja meg, vagy lépjen ki az ebben a módban. Ebben a módban való kilépéskor minden hello szolgáltatás folytatódik, és megfelelő kell lennie. (Ez eltarthat néhány percig.)

Ha tooapply karbantartási mód frissítések van szüksége, kapni fog egy riasztás hello a klasszikus Azure portálon keresztül, hogy rendelkezik-e telepíteni kell. Ez a riasztás a StorSimple tooinstall hello frissítéseket a Windows PowerShell használatára vonatkozó utasításokat tartalmazza. Miután frissítette az eszközt, használjon hello azonos eljárás toochange hello eszköz tooRegular mód. Részletes útmutatásért lásd: [4. lépés: Kilépés a karbantartási mód](#step4).

> [!IMPORTANT]
> * Mielőtt karbantartási módba, ellenőrizze, hogy mindkét eszközvezérlők kifogástalan hello ellenőrzésével **hardverállapot** a hello **karbantartási** hello klasszikus Azure portálra a lap. Ha hello tartományvezérlő nem működik megfelelően, forduljon a Microsoft Support hello további lépéseket. További információkért nyissa meg a Microsoft Support tooContact. 
> * Ha karbantartási módban van, meg kell-e tooapply hello frissítés először egy tartományvezérlő, és majd a többi tartományvezérlő hello.
> 
> 

### <a name="step-1-connect-toohello-serial-console-a-namestep1"></a>1. lépés: Csatlakozás soros konzolon toohello<a name="step1">
Például a PuTTY tooaccess hello soros konzol először egy alkalmazás használatát. hello következő eljárás azt ismerteti, hogyan toouse PuTTY tooconnect toohello soros konzolon.

[!INCLUDE [storsimple-use-putty](../../includes/storsimple-use-putty.md)]

### <a name="step-2-enter-maintenance-mode-a-namestep2"></a>2. lépés: Adja meg a karbantartási mód<a name="step2">
Toohello konzol csatlakozás után határozza meg, hogy vannak-e frissítések tooinstall, és adja meg a karbantartási mód tooinstall őket.

[!INCLUDE [storsimple-enter-maintenance-mode](../../includes/storsimple-enter-maintenance-mode.md)]

### <a name="step-3-install-your-updates-a-namestep3"></a>3. lépés: A frissítések telepítése<a name="step3">
Következő lépésként telepítse a frissítéseket.

[!INCLUDE [storsimple-install-maintenance-mode-updates](../../includes/storsimple-install-maintenance-mode-updates.md)]

### <a name="step-4-exit-maintenance-mode-a-namestep4"></a>4. lépés: Kilépés a karbantartási mód<a name="step4">
Végezetül kilép karbantartási módból.

[!INCLUDE [storsimple-exit-maintenance-mode](../../includes/storsimple-exit-maintenance-mode.md)]

## <a name="install-hotfixes-via-windows-powershell-for-storsimple"></a>A StorSimple gyorsjavítások Windows PowerShell telepítése
Frissítések a Microsoft Azure StorSimple, ellentétben a gyorsjavítás telepítve van egy megosztott mappából. Csakúgy, mint a frissítések két típusa van gyorsjavítások: 

* Rendszeres gyorsjavítások 
* Karbantartási mód gyorsjavítások  

hello alábbi eljárások azt ismertetik, hogyan toouse Windows PowerShell StorSimple tooinstall rendszeres és karbantartási mód gyorsjavítások.

[!INCLUDE [storsimple-install-regular-hotfixes](../../includes/storsimple-install-regular-hotfixes.md)]

[!INCLUDE [storsimple-install-maintenance-mode-hotfixes](../../includes/storsimple-install-maintenance-mode-hotfixes.md)]

## <a name="what-happens-tooupdates-if-you-perform-a-factory-reset-of-hello-device"></a>Mi történik, ha visszaállítsa a gyári tooupdates hello eszköz alaphelyzetbe állítása?
Ha egy eszköz alaphelyzetbe állítása toofactory beállításokat, majd minden hello frissítések elvesznek. Miután hello gyári alaphelyzetbe történő visszaállításhoz eszköz regisztrálva, és konfigurálva van, szüksége lesz StorSimple toomanually frissítéseinek telepítése a klasszikus Azure portálon hello és/vagy a Windows PowerShell használatával. További információ a gyári beállítások visszaállítása: [hello eszköz toofactory alapértelmezett beállításainak visszaállítása](storsimple-manage-device-controller.md#reset-the-device-to-factory-default-settings).

## <a name="next-steps"></a>Következő lépések
* További információ [Windows PowerShell használatával a StorSimple tooadminister a StorSimple eszköz](storsimple-windows-powershell-administration.md).
* További információ [használatával hello StorSimple Manager szolgáltatás tooadminister a StorSimple eszköz](storsimple-manager-service-administration.md).

