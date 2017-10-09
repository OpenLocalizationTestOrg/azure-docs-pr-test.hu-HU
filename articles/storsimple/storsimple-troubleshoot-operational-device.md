---
title: "a StorSimple eszköz üzembe helyezett aaaTroubleshoot |} Microsoft Docs"
description: "Ismerteti, hogyan toodiagnose és javítsa ki a StorSimple eszközön a jelenleg telepített és működő előforduló hibákat."
services: storsimple
documentationcenter: NA
author: SharS
manager: carmonm
editor: 
ms.assetid: ea5d89ae-e379-423f-b68b-53785941d9d0
ms.service: storsimple
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: TBD
ms.date: 05/16/2016
ms.author: v-sharos
ms.openlocfilehash: b48433055e05e3fb27575b88dca9f6b23c649ca2
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="troubleshoot-an-operational-storsimple-device"></a>Az operatív StorSimple eszköz hibáinak elhárítása
## <a name="overview"></a>Áttekintés
A cikkben kapcsolatos konfigurációs problémák hasznos hibaelhárítási útmutatót, hogy esetleg felmerülő után a StorSimple eszköz üzembe helyezett és működik. Ismerteti a gyakori problémákat, a lehetséges okok és a javasolt lépéseket toohelp Microsoft Azure StorSimple futtatásakor az esetleg előforduló problémák megoldása. Ezek az információk tooboth hello StorSimple helyszíni fizikai eszköz és a StorSimple virtuális eszköz hello vonatkoznak.

Ez a cikk a Microsoft Azure StorSimple művelet során esetleg előforduló hibakódok listáját megtalálja hello végén valamint lépéseket végre tooresolve hello hibák. 

## <a name="setup-wizard-process-for-operational-devices"></a>A telepítő varázsló folyamat működési eszközökhöz
Hello beállítása varázslóval ([Invoke-HcsSetupWizard][1]) toocheck hello eszközök konfigurációját, és hajtsa végre a javítási műveletet, ha szükséges.

A korábban konfigurált és működési eszközön hello beállítása varázsló futtatásakor hello folyamata nem azonos. A következő tételek csak hello módosíthatja:

* IP-cím, alhálózati maszk és átjáró
* Elsődleges DNS-kiszolgáló
* Elsődleges NTP-kiszolgáló
* Nem kötelező webproxy-konfigurációja

hello beállítása varázsló hello műveletek kapcsolódó toopassword adatgyűjtési és -eszközök regisztrációja nem végzi el.

## <a name="errors-that-occur-during-subsequent-runs-of-hello-setup-wizard"></a>Hello beállítása varázsló későbbi frissítési kísérletei során előforduló hibák
hello a következő táblázat ismerteti a működési eszközön hello beállítása varázsló futtatásakor előforduló hello hibák, lehetséges okai hello hibák, és a javasolt művelet tooresolve őket. 

| Nem. | Hiba vagy a feltétel | Lehetséges okok | Javasolt művelet |
|:--- |:--- |:--- |:--- |
| 1 |350032. hiba: Az eszköz már inaktív. |Ez a hiba jelenik meg egy eszközön inaktivált hello beállítása varázsló futtatásakor. |[Forduljon a Microsoft Support](storsimple-contact-microsoft-support.md) további lépéseket. Deaktivált eszköz szolgáltatást nem lehet rendezni. A gyári beállítások visszaállítása lehet szükség, mielőtt hello eszközt újra kell aktiválni. |
| 2 |Invoke-HcsSetupWizard: ERROR_INVALID_FUNCTION (kivétel HRESULT: 0x80070001) |hello DNS-kiszolgáló frissítése sikertelen. DNS-beállítások globálisak, és engedélyezve hello hálózati adaptereken keresztül alkalmazza. |Engedélyezze a hello felületet, és hello DNS-beállítások alkalmazásához újra. Az zavart okozhat más engedélyezett illesztőfelületek hello hálózatot, mert ezek a beállítások globálisak. |
| 3 |hello eszköz toobe online hello StorSimple Manager szolgáltatás portálon jelenik meg, de próbálkozzon toocomplete hello minimális, és hello konfiguráció mentéséhez, hello művelet sikertelen lesz. |Kezdeti beállítás során hello webproxy nem lett konfigurálva, annak ellenére, hogy hiba történt egy tényleges proxykiszolgáló helyen. |Használjon hello [Test-HcsmConnection parancsmag] [ 2] toolocate hello hiba. [Forduljon a Microsoft Support](storsimple-contact-microsoft-support.md) nem toocorrect hello probléma esetén. |
| 4 |Invoke-HcsSetupWizard: Érték nem esik hello várt tartományon belül. |Alhálózati maszk ezt a hibát eredményez. Lehetséges okok a következők: <ul><li> hello alhálózati maszk nem található vagy üres.</li><li>hello Ipv6-előtag formátuma nem megfelelő.</li><li>hello felületet a felhőalapú, de hello átjáró hiányzik vagy nem helyes.</li></ul>Vegye figyelembe, hogy a DATA 0 automatikusan felhő-engedélyezett, ha hello beállítása varázsló segítségével konfigurálható. |toodetermine hello probléma, 0.0.0.0 vagy 256.256.256.256 használni, és tekintse meg a kimeneti hello. Adja meg a megfelelő értékeket hello alhálózati maszk, az átjáró és az Ipv6-előtagot, igény szerint. |

## <a name="error-codes"></a>Hibakódok
Hibák numerikus sorrendben vannak felsorolva.

| Hiba száma | Hibaüzenet-szöveg vagy leírása | Ajánlott teendő |
|:--- |:--- |:--- |
| 10502 |Hiba történt a tárfiók elérése során. |Várjon néhány percet, és próbálkozzon újra. Ha hello hiba továbbra is fennáll, további lépéseket kérjük, forduljon a Microsoft ügyfélszolgálatához. |
| 40017 |hello biztonsági mentési művelet sikertelen volt, mint egy hello biztonsági mentési házirendben beállított kötet nem található hello eszközön. |Próbálja megismételni a biztonsági mentési művelet hello, ha hello hiba továbbra is fennáll, forduljon a Microsoft Support. a következő lépéseket. |
| 40018 |hello biztonsági mentési művelet sikertelen volt, mint hello biztonsági mentési házirendben megadott hello kötetek egyike sem található hello eszközön. |Próbálja megismételni a biztonsági mentési művelet hello, ha hello hiba továbbra is fennáll, forduljon a Microsoft Support. a következő lépéseket. |
| 390061 |hello rendszer foglalt vagy nem érhető el. |Várjon néhány percet, és próbálkozzon újra. Ha hello hiba továbbra is fennáll, további lépéseket kérjük, forduljon a Microsoft ügyfélszolgálatához. |
| 390143 |Hiba történt a következő hibakóddal 390143. (Ismeretlen hiba történt.) |Ha hello hiba továbbra is fennáll, forduljon a Microsoft Support további lépéseket. |

## <a name="next-steps"></a>Következő lépések
Ha Ön nem tooresolve hello probléma [forduljon a Microsoft Support](storsimple-contact-microsoft-support.md) segítségért. 

[1]: https://technet.microsoft.com/en-us/%5Clibrary/Dn688135(v=WPS.630).aspx
[2]: https://technet.microsoft.com/en-us/%5Clibrary/Dn715782(v=WPS.630).aspx
