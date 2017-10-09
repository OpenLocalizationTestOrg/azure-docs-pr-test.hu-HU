---
title: "DATA 0 aaaModify hello a StorSimple eszközön található beállítások |} Microsoft Docs"
description: "Megtudhatja, hogyan toouse Windows PowerShell a StorSimple tooreconfigure hello a StorSimple eszköz DATA 0 hálózati adapterén."
services: storsimple
documentationcenter: 
author: alkohli
manager: carmonm
editor: 
ms.assetid: 58e3d509-f425-4a7f-b650-d496a7c85193
ms.service: storsimple
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 08/17/2016
ms.author: alkohli
ms.openlocfilehash: caec51c3344d953299253301c2a0d7577d553c6e
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="modify-hello-data-0-network-interface-settings-on-your-storsimple-device"></a>A StorSimple eszköz hello DATA 0 hálózati kapcsolati beállítások módosítása
## <a name="overview"></a>Áttekintés
A Microsoft Azure StorSimple eszköz rendelkezik hat hálózati adapterrel, a DATA 0 tooDATA 5. hello DATA 0 felület mindig hello Windows PowerShell felületén vagy a soros konzol hello konfigurálva, és automatikusan felhő engedélyezve. Vegye figyelembe, hogy nem konfigurálhat DATA 0 hálózati adapterén hello a klasszikus Azure portálon keresztül. 

hello DATA 0 felület először konfigurálva hello beállítása varázsló hello StorSimple eszköz kezdeti telepítése során. Olyan működési mód hello eszköz esetén szükség lehet a DATA 0 tooreconfigure beállításait. Ez az oktatóanyag toomodify DATA 0 hálózati beállítások, mindkettő a Windows PowerShell-lel két módszert biztosít.

Ez az oktatóanyag elolvasása, után fogja tudni:

* Módosítsa a DATA 0 hálózati beállítás keresztül hello beállítása varázsló
* Módosítsa a DATA 0 hálózati beállítások hello `Set-HcsNetInterface` parancsmag

## <a name="modify-data-0-network-settings-through-setup-wizard"></a>A telepítő varázsló lépéseinek DATA 0 hálózati beállításainak módosítása
DATA 0 hálózati beállítások újrakonfigurálhatja toohello Windows PowerShell felületet a StorSimple eszköz csatlakoztatása és a telepítő varázsló munkamenet elindításához. Hajtsa végre a következő lépéseket toomodify DATA 0 hello beállítások:

#### <a name="toomodify-data-0-network-settings-through-setup-wizard"></a>toomodify DATA 0 hálózati beállítások beállítása varázsló
1. Hello soros konzol menüben válassza az 1. lehetőség – **jelentkezzen be a teljes körű hozzáférési**. Amikor a rendszer kéri adja meg a hello **eszköz rendszergazdai jelszava**. hello alapértelmezett jelszó `Password1`.
2. Hello parancsot, írja be a parancssorba:
   
    `Invoke-HcsSetupWizard`
3. A telepítő varázsló jelenik meg toohelp DATA 0 hello konfigurálása az eszköz felületén. Adjon meg új értékeket hello IP-cím, az átjáró és a hálózati maszk.

> [!NOTE]
> Hello, tartományvezérlői IP-címet kell újrakonfigurálni keresztül hello toobe rögzített **konfigurálása** hello StorSimple eszközt a klasszikus Azure portálon hello oldalán. További információ: túl[módosítása a hálózati adapterek](storsimple-modify-device-config.md#modify-network-interfaces).
> 
> 

## <a name="modify-data-0-network-settings-through-set-hcsnetinterface-cmdlet"></a>Set-HcsNetInterface parancsmaggal DATA 0 hálózati beállításainak módosítása
Egy másik módja tooreconfigure DATA 0 hálózati adapter van hello hello használata `Set-HcsNetInterface` parancsmag. a StorSimple eszköz hello Windows PowerShell felhasználói felületen keresztüli hello parancsmag végrehajtása. Ezzel az eljárással hello vezérlő rögzített IP-címei is konfigurálható itt. Hajtsa végre a következő lépéseket toomodify hello DATA 0 hello beállítások: 

#### <a name="toomodify-data-0-network-settings-through-hello-set-hcsnetinterface-cmdlet"></a>hello Set-HcsNetInterface parancsmag toomodify DATA 0 hálózati beállítások
1. Hello soros konzol menüben válassza az 1. lehetőség – **jelentkezzen be a teljes körű hozzáférési**. Amikor a rendszer kéri hello eszköz rendszergazdai jelszava adja meg. hello alapértelmezett jelszó `Password1`.
2. Hello parancsot, írja be a parancssorba:
   
    `Set-HCSNetInterface -InterfaceAlias Data0 -IPv4Address <> -IPv4Netmask <> -IPv4Gateway <> -Controller0IPv4Address <> -Controller1IPv4Address <> -IsiScsiEnabled 1 -IsCloudEnabled 1`
   
    Hello dőlt zárójelek közé írja be a következő értékeket a DATA 0 hello:
   
   * IPv4-cím
   * IPv4-átjáró
   * IPv4 alhálózati maszk
   * 0. vezérlő rögzített IPv4-cím
   * A vezérlő 1 fix IPv4-cím
     
     További információ a hello használata parancsmag: túl[StorSimple parancsmag-referencia a Windows PowerShell](https://technet.microsoft.com/library/dn688161.aspx).

## <a name="next-steps"></a>Következő lépések
* DATA 0 kivételével tooconfigure hálózati – hello használható [konfigurációs lapján, a klasszikus Azure portálon hello](storsimple-modify-device-config.md). 
* Ha probléma merül fel a hálózati adapterek konfigurálásakor, tekintse meg a túl[telepítési problémák elhárításához](storsimple-troubleshoot-deployment.md).

