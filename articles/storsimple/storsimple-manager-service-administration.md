---
title: "Kezelő szolgáltatás felügyeleti aaaStorSimple |} Microsoft Docs"
description: "Ismerje meg, hogyan a StorSimple eszközt a StorSimple Manager szolgáltatás a hello toomanage hello a klasszikus Azure portálon."
services: storsimple
documentationcenter: 
author: alkohli
manager: carmonm
editor: 
ms.assetid: 2586582e-d85c-42e1-afb3-be734c1c0461
ms.service: storsimple
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 09/21/2016
ms.author: alkohli
ms.openlocfilehash: 695ebbb785590a0e3d6de4c73125f65b16dcb776
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="use-hello-storsimple-manager-service-tooadminister-your-storsimple-device"></a>Hello StorSimple Manager szolgáltatás tooadminister a StorSimple eszköz használata
## <a name="overview"></a>Áttekintés
Ez a cikk ismerteti a hello StorSimple Manager szolgáltatási felületén, beleértve a hogyan tooconnect tooit hello különböző lehetőségeit, és hivatkozásokat tartalmaz, toohello adott munkafolyamatok, amelyek a felhasználói felületen keresztül végezhető el. Ez az útmutató az alkalmazandó tooboth; hello StorSimple fizikai és virtuális eszköz hello.

A cikk elolvasása után megtudhatja, hogy:

* Connect tooStorSimple kezelő szolgáltatás
* Keresse meg a StorSimple Manager felhasználói felületén hello
* A StorSimple eszközt a StorSimple Manager szolgáltatás hello keresztül felügyelete

## <a name="connect-toostorsimple-manager-service"></a>Connect tooStorSimple kezelő szolgáltatás
hello StorSimple Manager szolgáltatás fut a Microsoft Azure-ban, és kapcsolódik toomultiple StorSimple eszközökhöz. Ezek az eszközök egy böngésző toomanage futó központi Microsoft Azure klasszikus portál használhatja. tooconnect toohello StorSimple Manager szolgáltatás a következő hello.

#### <a name="tooconnect-toohello-service"></a>tooconnect toohello szolgáltatás
1. Keresse meg a túl[https://manage.windowsazure.com/](https://manage.windowsazure.com/).
2. A Microsoft-fiók hitelesítő adatait használja, jelentkezzen be toohello Microsoft klasszikus Azure portálon (hello jobb felső részén hello ablaktáblában található).
3. Görgessen le a bal oldali navigációs ablaktábla tooaccess hello StorSimple Manager szolgáltatás hello.

## <a name="navigate-storsimple-manager-service-ui"></a>Keresse meg a StorSimple Manager szolgáltatás felhasználói felülete
hello hello StorSimple Manager szolgáltatás felhasználói felületi megjelenik-e a következő táblázat hello navigációs hierarchia.

* **A StorSimple Manager** kezdőlapja viszi toohello felhasználói felület szolgáltatásiszint-lapok megfelelő tooall eszközök a szolgáltatáson belül.
* **Eszközök** lap viszi toohello eszköz – szint felhasználói felület pages alkalmazható tooa adott eszközt.
* **Kötettárolók** lap toohello kötet lap, amely az eszközök összes hello kötetek viszi.

#### <a name="storsimple-manager-service-navigational-hierarchy"></a>A StorSimple Manager szolgáltatás navigációs hierarchia
| Kezdőlapja | Szolgáltatásiszint-lapok | Eszközszintű lap | Eszközszintű lap |
| --- | --- | --- | --- |
| StorSimple Manager szolgáltatás |Szolgáltatási irányítópult |Eszköz irányítópult | |
| Eszközök → |Figyelés | | |
| Biztonságimásolat-katalógus |Kötet containers→ |Kötetek | |
| (Szolgáltatás) konfigurálása |Biztonsági mentési házirendek | | |
| Feladatok |(Eszköz) konfigurálása | | |
| Riasztások |Karbantartás | | |

![Videó elérhető](./media/storsimple-manager-service-administration/Video_icon.png)**Videó elérhető**

toowatch videó bemutatja, hogyan hello StorSimple Manager szolgáltatás felhasználói felületén kattintson [Itt](https://azure.microsoft.com/documentation/videos/storsimple-manager-service-overview/).

## <a name="administer-storsimple-device-using-storsimple-manager-service"></a>A StorSimple Manager szolgáltatással a StorSimple eszköz felügyelete
hello következő tábla összegzi hello általános kezelési feladatok és a bonyolult munkafolyamatok hello StorSimple Manager szolgáltatás felhasználói felületi belül végrehajtható. Ezek a feladatok hello felhasználói felület lapok, amelyen kezdeményezett alapján vannak rendezve.

Minden munkafolyamat kapcsolatos további információkért kattintson a hello eljárások hello táblában.

#### <a name="storsimple-manager-workflows"></a>A StorSimple Manager munkafolyamatok
| Ha azt szeretné, toodo ez... | Lépjen a toothis felhasználói felület lapra... | Ez az eljárás használható. |
| --- | --- | --- |
| Szolgáltatás létrehozása</br>A szolgáltatás törlése</br>Szolgáltatásregisztrációs kulcs lekérése</br>Szolgáltatásregisztrációs kulcs újragenerálása |StorSimple Manager szolgáltatás |[A StorSimple Manager szolgáltatás telepítése](storsimple-manage-service.md) |
| Szolgáltatásadat-titkosítási kulcs hello módosítása</br>Hello művelet naplók megtekintése |A StorSimple Manager szolgáltatás → irányítópult |[Hello StorSimple Manager szolgáltatás irányítópult](storsimple-service-dashboard.md) |
| Eszköz inaktiválása</br>Eszköz törlése |A StorSimple Manager szolgáltatás → eszközök |[Inaktiválja vagy törölje az eszköz](storsimple-deactivate-and-delete-device.md) |
| Vész-helyreállítási és eszköz-feladatátvétellel kapcsolatos további tudnivalók</br>Feladatátvételi tooa fizikai eszköz</br>Feladatátvételi tooa virtuális eszköz</br>Üzleti folytonosság vészhelyreállítási (BCDR) |A StorSimple Manager szolgáltatás → eszközök |[A StorSimple eszköz feladatátvétel és a katasztrófa utáni helyreállítás](storsimple-device-failover-disaster-recovery.md) |
| A kötet biztonsági mentéseit lista</br>Válassza ki a biztonságimásolat-készletből</br>A biztonságimásolat-készlet törlése |A StorSimple Manager szolgáltatás → biztonságimásolat-katalógus |[Biztonsági másolatok kezelése](storsimple-manage-backup-catalog.md) |
| Kötet klónozása |A StorSimple Manager szolgáltatás → biztonságimásolat-katalógus |[Kötet klónozása](storsimple-clone-volume.md) |
| Állítsa vissza a biztonságimásolat-készletből |A StorSimple Manager szolgáltatás → biztonságimásolat-katalógus |[Állítsa vissza a biztonságimásolat-készletből](storsimple-restore-from-backup-set.md) |
| Tárolási fiókokról</br>Tárfiók hozzáadása</br>A storage-fiók szerkesztése</br>Tárfiók törlése</br>Kulcs elforgatási szögét storage-fiókok |Konfigurálja a StorSimple Manager szolgáltatás → |[Tárfiókok kezelése](storsimple-manage-storage-accounts.md) |
| Sávszélesség-sablonok</br>Sávszélességsablon hozzáadása</br>A sávszélesség-sablon szerkesztése</br>Sávszélesség sablon törlése</br>Sávszélesség alapértelmezett sablon használata</br>Egy megadott időpontban elinduló napos sávszélesség sablon létrehozása |Konfigurálja a StorSimple Manager szolgáltatás → |[Sávszélességsablonok kezelése](storsimple-manage-bandwidth-templates.md) |
| Hozzáférés-vezérlési rekordokat kapcsolatos</br>Hozzon létre egy hozzáférés-vezérlési rekordot</br>Egy hozzáférés-vezérlési rekordot szerkesztése</br>Egy hozzáférés-vezérlési rekordot törlése |Konfigurálja a StorSimple Manager szolgáltatás → |[Hozzáférés-vezérlési rekordokat kezelése](storsimple-manage-acrs.md) |
| Feladatok részleteinek megjelenítése</br>Feladatok megszakítása |A StorSimple Manager szolgáltatás → feladatok |[Feladatok kezelése](storsimple-manage-jobs.md) |
| Riasztási értesítések fogadása</br>Riasztások kezelése</br>Riasztások áttekintése |A StorSimple Manager Szolgáltatásriasztások → |[StorSimple-riasztások megtekintése és kezelése](storsimple-manage-alerts.md) |
| Csatlakoztatott kezdeményezők megtekintése</br>Hello eszközsorozatszámot keresése</br>Található IQN hello célja |A StorSimple Manager szolgáltatás → eszközök → irányítópult |[Hello StorSimple eszköz irányítópult](storsimple-device-dashboard.md) |
| Figyelési diagramok létrehozása |A StorSimple Manager szolgáltatás → eszközök figyelése → |[A StorSimple eszköz figyelése](storsimple-monitor-device.md) |
| A kötettároló hozzáadása</br>A kötettároló módosítása</br>A kötettároló törlése |Eszközök → Kötettárolók StorSimple Manager szolgáltatás → |[Kötettárolók kezelése](storsimple-manage-volume-containers.md) |
| Kötet hozzáadása</br>A kötet módosítása</br>A kötet offline állapotba helyezése</br>Kötet törlése</br>A kötet figyelése |A StorSimple Manager szolgáltatás → eszközök → Kötettárolók → kötetek |[Kötetek kezelése](storsimple-manage-volumes.md) |
| Eszköz beállításainak módosítása</br>Idő beállításainak módosítása</br>DNS.md beállításainak módosítása</br>Hálózati adapterek konfigurálása |A StorSimple Manager szolgáltatás → eszközök → konfigurálása |[A StorSimple eszköz eszköz konfigurációjának módosítása](storsimple-modify-device-config.md) |
| Webalkalmazás-proxy beállítások megjelenítése |A StorSimple Manager szolgáltatás → eszközök → konfigurálása |[Az eszköz webalkalmazás-proxy konfigurálása](storsimple-configure-web-proxy.md) |
| Eszköz rendszergazdai jelszavának módosítása</br>StorSimple Snapshot Manager jelszavának módosítása |A StorSimple Manager szolgáltatás → eszközök → konfigurálása |[StorSimple-jelszavak módosítása](storsimple-change-passwords.md) |
| A távfelügyelet konfigurálása |A StorSimple Manager szolgáltatás → eszközök → konfigurálása |[Távoli csatlakozás tooyour StorSimple-eszköz](storsimple-remote-connect.md) |
| A riasztási beállítások konfigurálása |A StorSimple Manager szolgáltatás → eszközök → konfigurálása |[StorSimple-riasztások megtekintése és kezelése](storsimple-manage-alerts.md) |
| A CHAP konfigurálása a StorSimple eszköz |A StorSimple Manager szolgáltatás → eszközök → konfigurálása |[A CHAP konfigurálása a StorSimple eszköz](storsimple-configure-chap.md) |
| Biztonsági mentési szabályzat hozzáadása</br>Adja hozzá vagy ütemezésének módosítása</br>A biztonsági mentési házirend törlése</br>Manuális biztonsági mentés készítése</br>Hozzon létre egy egyéni biztonsági mentési házirend több kötetek és ütemezése |A StorSimple Manager szolgáltatás → eszközök → biztonsági mentési házirendek |[Biztonsági mentési szabályzatok kezelése](storsimple-manage-backup-policies.md) |
| Állítsa le a eszközvezérlők</br>Indítsa újra a eszközvezérlők</br>Eszközvezérlők leállítása</br>Az eszköz toofactory visszaállítása</br>(Csak a helyi eszköz újabb rendszer) |A StorSimple Manager szolgáltatás → eszközök → karbantartás |[A StorSimple eszköz vezérlő kezelése](storsimple-manage-device-controller.md) |
| További tudnivalók a StorSimple hardverösszetevők</br>Figyelje meg a hardver állapotát</br>(Csak a helyi eszköz újabb rendszer) |A StorSimple Manager szolgáltatás → eszközök → karbantartás |[A figyelő hardverösszetevők](storsimple-monitor-hardware-status.md) |
| Hozzon létre egy támogatási csomag |A StorSimple Manager szolgáltatás → eszközök → karbantartás |[Létrehozását és kezelését egy támogatási csomag](storsimple-create-manage-support-package.md) |
| Telepítse a szoftverfrissítéseket |A StorSimple Manager szolgáltatás → eszközök → karbantartás |[Az eszköz frissítése](storsimple-update-device.md) |

## <a name="next-steps"></a>Következő lépések
Ha probléma merül fel a StorSimple eszköz hello napi szintű üzemeltetése vagy bármelyik hardver, tekintse meg:

* [Az operatív eszköz hibáinak elhárítása](storsimple-troubleshoot-operational-device.md)
* [Kijelző LED figyelési StorSimple használata](storsimple-monitoring-indicators.md)

Ha mégsem sikerül megoldani a hello problémákat, és meg kell toocreate szolgáltatáskérés, tekintse meg a túl[forduljon a Microsoft ügyfélszolgálatához](storsimple-contact-microsoft-support.md).

