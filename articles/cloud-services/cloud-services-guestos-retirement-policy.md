---
title: "Azure vendég operációs rendszer aaaSupportability és a használatból való kivonást házirend útmutató |} Microsoft Docs"
description: "Mi a Microsoft támogatást toohello felhőalapú szolgáltatás által használt Azure vendég operációs rendszer tekintetében információkat biztosít."
services: cloud-services
documentationcenter: na
author: raiye
manager: timlt
editor: 
ms.assetid: 919dd781-4dc6-4e50-bda8-9632966c5458
ms.service: cloud-services
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: tbd
ms.date: 5/26/2017
ms.author: raiye
ms.openlocfilehash: 6a86c42ac95d12bbf116d900b7afb26fc3fe34e6
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="azure-guest-os-supportability-and-retirement-policy"></a>Az Azure vendég operációs rendszer támogatásának és a használatból való kivonást házirend
Ezen az oldalon hello információ vonatkozik toohello Azure vendég operációs rendszer ([vendég operációs rendszer](cloud-services-guestos-update-matrix.md)) Felhőszolgáltatások munkavégző és a webes szerepkörök (PaaS). Nem alkalmazható tooVirtual gépek (IaaS).

A Microsoftnál egy közzétett [házirend támogatása vendég operációs rendszer hello](http://support.microsoft.com/gp/azure-cloud-lifecycle-faq). hello lap olvas most ismerteti, hogyan van megvalósítva hello házirend.

hello házirend

1. A Microsoft támogatást **legalább hello hello vendég operációs rendszer legújabb két termékcsaládok**. Család kivonták a rendszerből, ha a felhasználóknak kell hello hivatalos használatból való kivonást dátum tooupdate tooa újabb támogatott vendég operációsrendszer-család számított 12 hónapig.
2. A Microsoft támogatást **hello támogatott vendég operációsrendszer-családok legújabb két verziója legalább hello**.
3. A Microsoft támogatást **hello Azure SDK legújabb két verziója legalább hello**. Amikor a hello SDK elavult verzióját, az ügyfelek hello hivatalos használatból való kivonást dátum tooupdate tooa újabb verziójából származó 12 hónapig fog rendelkezni.

Esetenként, több mint két családok vagy kiadásokban lehet, hogy támogatja. Hivatalos vendég operációs rendszer támogatási információkat megjelenik hello [Azure vendég operációs rendszereinek kiadásait és SDK-kompatibilitási mátrixát](cloud-services-guestos-update-matrix.md).

## <a name="when-a-guest-os-family-or-version-is-retired"></a>Ha a Vendég operációsrendszer-család vagy a verziójával kivonták a rendszerből
Egy új vendég operációs rendszer **termékcsalád** némi várakozás után hello Windows Server operációs rendszer a hivatalos verziójának hello kiadásában megjelent. Egy új Vendég operációsrendszer-család jelenik meg, amikor Microsoft hello legrégebbi Vendég operációsrendszer-család fogja vonni.

Új vendég operációs rendszer **verziók** minden hónap tooincorporate hello legújabb MSRC frissítésekkel kapcsolatos új. Hello rendszeres havi frissítéseket, mert a vendég operációs rendszer verziója általában letiltott 60 nap után a kiadása. Ez a tevékenység tartja az egyes használható legalább két Vendég operációsrendszer-verziók.

### <a name="process-during-a-guest-os-family-retirement"></a>A folyamat során a vendég operációs rendszer termékcsalád kivonása
Hello használatból való kivonást elavulásának bejelentéséig, az ügyfelek után 12 hónap "átmenet" időszak előtt hello régebbi termékcsalád hivatalosan eltávolítja a szolgáltatásból. A váltás ideje, a Microsoft hello belátása is kiterjeszthető. Frissítések lesznek közzétéve a hello [Azure vendég operációs rendszereinek kiadásait és SDK-kompatibilitási mátrixát](cloud-services-guestos-update-matrix.md).

A fokozatos használatból való kivonást folyamat megkezdődik hat (6) hónap hello áttűnés időszak be. Ebben az időszakban:

1. A Microsoft ügyfelei hello használatból való kivonást értesíti.
2. hello hello Azure SDK újabb verziója már nincs hello Vendég operációsrendszer-család nem támogatja.
3. Új központi telepítéséhez és a Felhőszolgáltatások redeployments nem engedélyezett a kivont hello termékcsalád

A Microsoft továbbra is toointroduce hello MSRC frissítéseket tartalmazó hello hello áttűnés időszak alatt hello "lejárati dátum" néven ismert utolsó napján csak új vendég operációs rendszer verziója. A lejárati dátum hello még mindig fut, a Cloud Services rendszer nem minden hello Azure garantált szolgáltatási szintje alatt. A Microsoft hello megítélése tooforce frissítése, törlése vagy szolgáltatások leállítását, hogy a dátum után van.

### <a name="process-during-a-guest-os-version-retirement"></a>A folyamat során a vendég operációs rendszer verziója kivonása
Ha az ügyfelek a vendég operációs rendszer tooautomatically frissítés, Vendég operációsrendszer-verziók kezelésével kapcsolatos tooworry soha nem rendelkeznek. Majd mindig Vendég operációs rendszer legújabb verzióját hello használ.

Vendég operációsrendszer-verziók havonta kiadott. Rendszeres kiadásokban hello arányát, mert egyes verzióihoz rögzített élettartamot rendelkezik.

60 nap hello élettartamot be, a verzió: "*le van tiltva*". "Letiltott" azt jelenti, hogy hello verziót a rendszer eltávolítja a hello portálon. hello verziója már nem állítható be hello szolgáltatáskonfigurációs SÉMA konfigurációs fájlból. Meglévő telepítések meghagyott futtatása. Azonban nem engedélyezett új központi telepítéséhez és a kód és konfigurációs tooexisting központi telepítését.

Némi várakozás után válik "Letiltva", a vendég operációs rendszer verziója hello "*lejár*", és minden még fut az adott telepítéshez kényszerített frissítése vagy tooautomatically frissítés hello vendég operációs rendszer jövőbeli hello beállítására. Lejárati kötegekben történik, így hello időtartam megfelelő tooexpiration eltérőek lehetnek.

Ezeket az időszakokat Microsoft megítélése tooease ügyfél átmenetek hosszabb lehet elvégezni. A módosításokat kommunikálja a hello [Azure vendég operációs rendszereinek kiadásait és SDK-kompatibilitási mátrixát](cloud-services-guestos-update-matrix.md).

### <a name="notifications-during-retirement"></a>Értesítések során kivonása
* **Családbiztonsági kivonása** <br>Microsoft blogbejegyzések és a portál értesítései fogja használni. Egy kivont Vendég operációsrendszer-család továbbra is használó ügyfelek közvetlen kommunikációt (e-mail, portál üzenetek, telefonhívás) tooassigned szolgáltatás-rendszergazdák keresztül értesítést fog kapni. Minden változást toothis lap és hello RSS-hírcsatorna ezen a lapon hello elején felsorolt lesznek közzétéve.
* **Verzió kivonása** <br>Összes módosítás és az előfordulásuk időpontjában naplózhatja őket hello dátumok lesznek közzétéve toothis lap és hello RSS-hírcsatorna ezen a lapon, beleértve a kiadás, le van tiltva és lejárati hello elején szerepel. Szolgáltatás-rendszergazdák e-maileket kap, ha egy letiltott vendég operációs rendszer verziója vagy a családot futó központi telepítések. az e-maileket hello ütemezése változhatnak. Ezek általában legalább előtt megfelelő, egy hónap, ha a időzítési nem hivatalos szolgáltatásiszint-szerződésben garantált.

## <a name="frequently-asked-questions"></a>Gyakori kérdések
**Hogyan csökkentheti az áttelepítés hatásai hello?**

Azt javasoljuk, hogy használja-e legújabb Vendég operációsrendszer-család a Felhőszolgáltatások tervezéséhez.

1. Indítsa el az áttelepítési tooa újabb termékcsalád korai tervezési.
2. Beállítása ideiglenes teszt központi telepítések tootest a hello új termékcsalád futó felhőalapú szolgáltatás.
3. Állítsa be a vendég operációs rendszer verziója túl**automatikus** (osVersion = * a hello [.cscfg](cloud-services-model-and-package.md#cscfg) fájl), hello áttelepítési toonew Vendég operációsrendszer-verziók automatikusan megtörténik.

**Mi történik, ha a webalkalmazás számára szükséges az operációs rendszer hello szorosabb integrációt?**

Ha a webes alkalmazás felépítésére hello operációs rendszer alapjául szolgáló funkciók függ, használjon támogatott platform képességei például [indítási feladatok](cloud-services-startup-tasks.md) vagy más bővítési mechanizmusokat. Másik megoldásként használhatja [Azure virtuális gépek](https://azure.microsoft.com/documentation/scenarios/virtual-machines/) (IaaS – infrastruktúra-szolgáltatás), hogy hol áll operációs rendszer alapjául szolgáló hello felelős.

## <a name="next-steps"></a>Következő lépések
Felülvizsgálati hello legújabb [feloldja a vendég operációs rendszer](cloud-services-guestos-update-matrix.md).
