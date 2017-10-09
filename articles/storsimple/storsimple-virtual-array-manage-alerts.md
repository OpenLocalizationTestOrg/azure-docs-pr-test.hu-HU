---
title: "aaaView és a Microsoft Azure StorSimple virtuális tömb riasztások kezelése |} Microsoft Docs"
description: "Ismerteti a StorSimple virtuális tömb riasztási feltételek és a súlyosság, és hogyan toouse hello StorSimple Manager szolgáltatás toomanage riasztások."
services: storsimple
documentationcenter: NA
author: alkohli
manager: timlt
editor: 
ms.assetid: 97ee25a1-0ec3-4883-9a0a-54b722598462
ms.service: storsimple
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 02/27/2017
ms.author: alkohli
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: b0fb5b1b9064f33df1d8fa7ace45f0d72b0a1622
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="use-storsimple-device-manager-toomanage-alerts-for-hello-storsimple-virtual-array"></a>A StorSimple virtuális tömb hello StorSimple az Eszközkezelő toomanage riasztásai

## <a name="overview"></a>Áttekintés

hello riasztások szolgáltatását hello StorSimple Device Manager szolgáltatás lehetőséget biztosít az Ön tooreview, és törölje a riasztások kapcsolódó tooStorSimple virtuális tömbök valós idejű alapon. Hello riasztások hello használhatja **szolgáltatási összegző** panel toocentrally hello ügynökállapottal kapcsolatos hibákkal a StorSimple virtuális tömbök figyelése és hello általános Microsoft Azure StorSimple megoldáshoz.

Ez az oktatóanyag leírja, hogyan tooconfigure riasztási értesítéseket, közös riasztási feltételek, riasztás súlyossági szintek, és hogyan tooview és nyomon követése riasztásokat. Ezenkívül azt is riasztás rövid összefoglaló táblákat, amelyek lehetővé teszik, hogy tooquickly keresse meg az adott riasztásra, és megfelelő választ.

![Riasztások lap](./media/storsimple-virtual-array-manage-alerts/alerts1.png)

## <a name="configure-alert-settings"></a>A riasztási beállítások konfigurálása

Kiválaszthatja, hogy kívánja-e toobe hello riasztási feltételek mindegyikének a StorSimple virtuális tömbök e-mailben értesítést kap. Ezenkívül azonosítsa a más riasztási értesítés címzettjeinek hello e-mail címüket megadásával **további e-mailek címzettjeinek** mezőbe pontosvesszővel elválasztva.

> [!NOTE]
> Legfeljebb 20 e-mail cím adható virtuális tömb adhat meg.


Egy virtuális tömb kapcsolódó e-mail értesítések engedélyezése után tagok hello értesítési lista minden alkalommal, amikor egy kritikus riasztás esetén e-mailt fog kapni. hello üzeneteket küld a  *storsimple-alerts-noreply@mail.windowsazure.com*  és hello riasztási feltétel ismerteti. Címzettek kattintva **Unsubscribe** tooremove maguk hello e-mail értesítési listáról.

#### <a name="tooenable-email-notification-for-alerts"></a>tooenable riasztásokhoz kapcsolódó e-mail értesítések

1. Nyissa meg tooyour StorSimple Device Manager szolgáltatás és a hello **felügyeleti** szakaszt, válassza ki, és kattintson **eszközök**. A megjelenített eszközök hello listáról válassza ki, majd kattintson az eszközre.
   
    ![a riasztási beállítások](./media/storsimple-virtual-array-manage-alerts/alerts2.png)
2. Ezzel megnyílik hello **beállítások** panelen. A hello **eszközbeállítások** szakaszban jelölje be **általános**. Ezzel megnyílik hello **általános beállítások** panelen.
   
    ![riasztások értesítési konfiguráció](./media/storsimple-virtual-array-manage-alerts/alerts4.png)
3. A hello **általános beállítások** panelen, lépjen túl**riasztási beállítások** szakaszt, és állítsa be a következőket hello:
   
   1. A hello **e-mail értesítő engedélyezése** mezőben válassza **Igen**.
   2. A hello **szolgáltatás-rendszergazdák E-mail** mezőben válassza **Igen** Ha toohave hello szolgáltatás-rendszergazda kívánja, és minden társrendszergazdák hello riasztási értesítéseket kaphat.
   3. A hello **további e-mailek címzettjeinek** mezőbe írja be az összes többi címzett hello riasztási értesítéseket kapjanak hello e-mail címét. Írja be a neveket hello formátumban  *someone@somewhere.com* . Pontosvesszővel tooseparate hello e-mail-címek használata. Legfeljebb 20 e-mail címeket a virtuális eszköz konfigurálása
      
       ![riasztások értesítési konfiguráció](./media/storsimple-virtual-array-manage-alerts/alerts6.png)
   4. Kattintson egy teszt e-mail-értesítések toosend **teszt e-mail küldése**. hello tesztértesítés továbbítja hello StorSimple Device Manager szolgáltatás jelennek-állapotüzenetek.
      
       ![Riasztások által küldött értesítő e-mail tesztelése](./media/storsimple-virtual-array-manage-alerts/alerts7.png)
      
      > [!NOTE]
      > Ha hello tesztelése értesítési üzenet nem küldhető, hello StorSimple Device Manager szolgáltatás egy megfelelő üzenetet jelenít meg. Kattintson a **OK**, várjon néhány percet, és próbálkozzon toosend az értesítési tesztüzenet újra.
      > 
      > 
   5. Hello a hello lap alján, kattintson **mentése** toosave a konfigurációt. Ha a rendszer megerősítést kér, kattintson az **Igen** gombra.
      
      ![Riasztások által küldött értesítő e-mail tesztelése](./media/storsimple-virtual-array-manage-alerts/alerts10.png)

## <a name="common-alert-conditions"></a>Gyakori riasztási állapot

A StorSimple virtuális tömb riasztásokat állít elő, a válasz tooa különböző feltételeket. Az alábbiakban hello hello leggyakoribb riasztási feltételek:

* **Kapcsolódási problémák** – ezek a riasztások fordulhat elő, ha az adatok átvitele nehézségekbe ütközik. Hello Azure storage-fiókot vagy megfelelő adatok tooand adatátvitel során fordulhatnak elő a kommunikációs problémák toolack a virtuális eszközök hello és hello StorSimple Device Manager szolgáltatás közötti kapcsolat. Kommunikációs problémák hello nagyon nehéz toofix közé tartoznak, mert nincs olyan sok ponton felmerülő hibákat is. Először mindig ellenőrizze, hogy hálózati kapcsolatot és az Internet-hozzáférés érhetők el, mielőtt továbblép a toomore speciális hibaelhárítás. Portok és a tűzfal beállításaival kapcsolatos információkért nyissa meg túl[StorSimple virtuális tömb rendszerkövetelmények](storsimple-ova-system-requirements.md). A segítséget kaptak, go túl[hello Test-Connection parancsmaggal kapcsolatos problémák elhárítása](storsimple-troubleshoot-deployment.md).
* **Teljesítményproblémák** – ezek a riasztások vannak okozza, ha a rendszer nem optimális, például a nagy terhelés alatt van.

Emellett riasztásokat kapcsolódó toosecurity, vagy csak a sikertelen feladatok vehet észre.

## <a name="alert-severity-levels"></a>Riasztások súlyossági szintjei

Riasztások különböző súlyossági szinttel rendelkezik, hello gyakorolt, amely hello riasztási helyzet rendelkezik lesz, és a válasz toohello riasztások szükségességét hello. hello súlyossági szintek a következők:

* **Kritikus** – Ez a riasztás válasz tooa állapotban, amely érinti a rendszer hello sikeres teljesítményét van. A művelet akkor szükséges tooensure hello StorSimple szolgáltatás nem szakad meg.
* **Figyelmeztetés** – Ez az állapot kritikus, ha nem sikerül válhat. Meg kell hello helyzet vizsgálata hajtanak végre minden szükséges művelet tooclear hello kapcsolatos probléma.
* **Információ** – Ez a riasztás információkat tartalmaznak, amelyek nyomon követése és a rendszer kezelése hasznos lehet.

## <a name="view-and-track-alerts"></a>Megtekintheti, és nyomon követéséhez értesítések

hello StorSimple Device Manager szolgáltatás összefoglaló panel biztosít kiadványok riasztások súlyossági szint szerint, a virtuális eszközön hello számát.

![Riasztások irányítópult](./media/storsimple-virtual-array-manage-alerts/alerts14.png)

Kattintson a hello súlyossági szint tooopen hello **riasztások** panelen. hello eredmények csak azonos adott súlyossági szinttel hello riasztások tartalmazzák.

![Riasztások jelentés hatókörű tooalert típusa](./media/storsimple-virtual-array-manage-alerts/alerts15.png)

Kattintson egy riasztásra hello lista tooget további hello riasztás részletei, beleértve a hello legutóbbi hello riasztás érkezett jelentés, hello hello riasztás hello eszközön, és hello javasolt művelet tooresolve hello riasztás előfordulásainak száma.

![Riasztások listája és részletek](./media/storsimple-virtual-array-manage-alerts/alerts16.png)

Ha toosend hello információk tooMicrosoft támogatási kell hello riasztás részleteinek tooa szövegfájl másolhatja. Hello javaslat követni és hello riasztási feltétel helyszíni feloldva, után törölje hello riasztás hello listából. Jelölje ki a hello riasztást hello listából, majd kattintson a **egyértelmű**. tooclear több riasztást, jelölje be minden riasztást, kattintson a hello kivételével minden oszlop **riasztás** oszlop, és kattintson **egyértelmű** kiválasztása után az összes hello riasztások toobe nincs bejelölve.

Amikor rákattint **egyértelmű**, hello lehetőség tooprovide megjegyzéseket hello riasztást fog, és hello lépéseket, hogy, tooresolve hello probléma. 

![riasztási megjegyzések](./media/storsimple-virtual-array-manage-alerts/alerts17.png)

Egyes események hello rendszer törlődik, ha egy másik esemény akkor váltódik ki, új információkkal. 

## <a name="sort-and-review-alerts"></a>A rendezés és tekintse át a riasztások

Hello **riasztások** panel too250 figyelmeztetéseket tudja megjeleníteni. Túllépte a riasztások számát is, ha nem az összes riasztás hello alapértelmezett nézetben jelenik meg. A következő mezők toocustomize mely riasztások megjelenítésének hello kombinálva:

* **Állapot** – jelenítheti meg vagy **aktív** vagy **nincs bejelölve** riasztásokat. Aktív továbbra is folyamatban figyelmeztetéseket a rendszeren, amíg üres riasztások vagy manuálisan törölték egy rendszergazda, vagy programozott módon törölve, mert hello rendszer hello riasztási feltétel frissülnek az új adatokkal.
* **Súlyossági** – az összes súlyossági szint (kritikus, figyelmeztetés, tájékoztatás), vagy csak egy bizonyos súlyossági, például csak a kritikus riasztások riasztások jelenítheti meg.
* **Forrás** – forrásokból származó riasztások megjelenítése, vagy korlátozza a hello riasztások toothose hello szolgáltatás vagy egy érkező vagy virtuális eszközök hello összes.
* **Időtartománynak** – hello megadásával **a** és **való** dátumok és időbélyegeket, vessen egy pillantást riasztások hello érdekli időtartamnak során.

## <a name="alerts-quick-reference"></a>Riasztások rövid összefoglalása

a következő táblák hello megjelenő néhány hello StorSimple riasztásokat, amelyek akkor fordulhatnak, valamint további információkra és ajánlásokra ahol az rendelkezésre áll. A StorSimple virtuális tömb riasztások egyikébe hello a következő kategóriák:

* [Felhő csatlakozási riasztások](#cloud-connectivity-alerts)
* [Konfigurációs riasztásokat](#configuration-alerts)
* [Figyelmeztetések](#job-failure-alerts)
* [Teljesítményével kapcsolatos riasztások](#performance-alerts)
* [Biztonsági riasztások](#security-alerts)
* [Riasztások frissítése](#update-alerts)

### <a name="cloud-connectivity-alerts"></a>Felhő csatlakozási riasztások

| Figyelmeztető szöveg | Esemény | További információ / javasolt műveletek |
|:--- |:--- |:--- |
| Eszköz  *<device name>*  van toohello felhő nincs csatlakoztatva. |hello eszköz neve nem lehet kapcsolódni a toohello felhő. |Nem sikerült csatlakozni a toohello felhő. Ennek oka lehet esedékes tooone hello alábbi:<ul><li>Előfordulhat, hogy az eszközön található hello hálózati beállítások problémáját.</li><li>Előfordulhat, hogy a hello tárfiók hitelesítő adatainak kapcsolatos probléma.</li></ul>A kapcsolódási problémák elhárításával kapcsolatos további információkért lásd a toohello [helyi webes felhasználói felület](storsimple-ova-web-ui-admin.md) hello eszköz. |

### <a name="configuration-alerts"></a>Konfigurációs riasztásokat

| Figyelmeztető szöveg | Esemény | További információ / javasolt műveletek |
|:--- |:--- |:--- |
| A helyszíni virtuális eszközök konfigurációját nem támogatott. |Lassú teljesítmény. |hello aktuális konfigurációs teljesítménycsökkenést eredményezhet. Győződjön meg arról, hogy a kiszolgáló megfelel-e a hello minimális konfigurációs követelményeinek. További információ: túl[a StorSimple virtuális tömb követelményei](storsimple-ova-system-requirements.md). |
| Futtatja kiosztott lemezterület <*eszköznév*>. |Figyelmeztetés. |Kevés a kiosztott lemezterület. toofree fel lemezterületet, fontolja meg a munkaterhelések tooanother kötetet vagy megosztást áthelyezése vagy adatok törlése. |

### <a name="job-failure-alerts"></a>Figyelmeztetések

| Figyelmeztető szöveg | Esemény | További információ / javasolt műveletek |
|:--- |:--- |:--- |
| A biztonsági mentési <*eszköznév*> nem sikerült végrehajtani. |Biztonsági mentési feladat sikertelen. |Nem sikerült létrehozni a biztonsági másolat. Vegye figyelembe az alábbi hello:<ul><li>Kapcsolódási problémák megakadályozzák, hogy hello biztonsági mentési művelet sikeres befejezését. Győződjön meg arról, hogy nincsenek-e kapcsolódási problémák. A kapcsolódási problémák elhárításával kapcsolatos további információkért lásd a toohello [helyi webes felhasználói felület](storsimple-ova-web-ui-admin.md) a virtuális eszköz.</li><li>Hello rendelkezésre álló tár korlátot elérte. toofree fel lemezterületet, fontolja meg az összes biztonsági másolatot, amely már nem szükséges törlését.</li></ul> Hello problémák megoldására, törölje a hello riasztást, majd próbálja megismételni hello műveletet. |
| A klón <*eszköznév*> nem sikerült végrehajtani. |Klónozza a feladat sikertelen. |Nem sikerült létrehozni a klónozott. Vegye figyelembe az alábbi hello:<ul><li>A biztonságimásolat-lista nem lehet érvényes. Frissítse a hello lista tooverify továbbra is érvényes.</li><li>Kapcsolódási problémák megakadályozzák, hogy hello klónozási művelet sikeres befejezését. Győződjön meg arról, hogy nincsenek-e kapcsolódási problémák.</li><li>Hello rendelkezésre álló tár korlátot elérte. toofree fel lemezterületet, fontolja meg az összes biztonsági másolatot, amely már nem szükséges törlését.</li></ul>Hello problémák megoldására, törölje a hello riasztást, majd próbálja megismételni hello műveletet. |

### <a name="performance-alerts"></a>Teljesítményével kapcsolatos riasztások

| Figyelmeztető szöveg | Esemény | További információ / javasolt műveletek |
|:--- |:--- |:--- |
| Adatátvitel váratlan késést tapasztal. |Lassú az adatátvitelt. |Sávszélesség-szabályozási hiba történik, amikor a túllépi hello méretezhetőségi célok tároló szolgáltatás. hello tárolási szolgáltatásnak nincs a tooensure, amely nem egy ügyfél vagy bérlői hello szolgáltatást használhatják hello költségén mások. Az Azure storage-fiók elhárításával kapcsolatos további információkért lépjen túl[figyelése, diagnosztizálása és elhárítása a Microsoft Azure Storage](../storage/common/storage-monitoring-diagnosing-troubleshooting.md). |
| Kevés a szabad lemezterület helyi lefoglalás <*eszköznév*>. |Lassú válasz időpontja. |10 %-a hello teljes kiosztott mérete a <*eszköznév*> fenn tartva hello a helyi eszközön, és használhatók kevés hello a foglalt területet. hello terhelése <*eszköznév*> generál egy nagyobb mértékű forgalom, vagy előfordulhat, hogy nemrég áttelepített nagy mennyiségű adatot. Emiatt előfordulhat, a csökkentett teljesítményt. Fontolja meg az egyik hello műveletek tooresolve követően ez:<ul><li>Növelje a hello felhő sávszélesség toothis eszköz.</li><li>Csökkentse, vagy helyezze át a munkaterheléseket tooanother kötetet vagy megosztást.</li></ul> |

### <a name="security-alerts"></a>Biztonsági riasztások

| Figyelmeztető szöveg | Esemény | További információ / javasolt műveletek |
|:--- |:--- |:--- |
| A jelszó <*eszköznév*> múlva le fog járni <*szám*> nap. |Jelszó figyelmeztetés. |A jelszó lejár < szám < nap. Fontolja meg a jelszót. További információ: túl[módosítás hello StorSimple virtuális tömb eszköz rendszergazdai jelszava](storsimple-virtual-array-change-device-admin-password.md). |

### <a name="update-alerts"></a>Riasztások frissítése

| Figyelmeztető szöveg | Esemény | További információ / javasolt műveletek |
|:--- |:--- |:--- |
| Új frissítések érhetők el az eszközhöz. |A StorSimple virtuális tömb frissítések toohello érhetők el. |Új frissítések hello telepítheti **karbantartási** lap. |
| Sikerült nem új frissítések keresése a <*eszköznév*>. |A frissítés sikertelen. |Hiba történt az új frissítések telepítése közben. Manuálisan telepítheti hello frissítéseket. Ha hello a probléma továbbra is fennáll, forduljon a [Microsoft Support](storsimple-contact-microsoft-support.md). |

## <a name="next-steps"></a>Következő lépések

* [További tudnivalók a StorSimple virtuális tömb hello](storsimple-ova-overview.md).

