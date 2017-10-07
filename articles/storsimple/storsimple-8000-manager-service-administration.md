---
title: "Eszközkezelő felügyelő aaaStorSimple |} Microsoft Docs"
description: "Ismerje meg, hogyan a StorSimple eszközt a StorSimple eszköz Manager szolgáltatás a hello toomanage hello Azure-portálon."
services: storsimple
documentationcenter: 
author: alkohli
manager: timlt
editor: 
ms.assetid: 
ms.service: storsimple
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 07/12/2017
ms.author: alkohli
ms.openlocfilehash: d73dc32bd39b2c832d5c4a5edda32a2e7f55a503
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="use-hello-storsimple-device-manager-service-tooadminister-your-storsimple-device"></a>Hello StorSimple Device Manager szolgáltatás tooadminister a StorSimple eszköz használata

## <a name="overview"></a>Áttekintés

Ez a cikk ismerteti a hello StorSimple Device Manager szolgáltatási felületén, beleértve a hogyan tooconnect tooit hello különböző lehetőségeit, és hivatkozásokat tartalmaz, toohello adott munkafolyamatok, amelyek a felhasználói felületen keresztül végezhető el. Ez az útmutató az alkalmazandó tooboth; hello StorSimple fizikai eszköz és hello felhő készüléket.

A cikk elolvasása után megtudhatja, hogy:

* Connect tooStorSimple Eszközkezelő szolgáltatás
* A StorSimple eszköz StorSimple Device Manager szolgáltatás hello keresztül felügyelete

## <a name="connect-toostorsimple-device-manager-service"></a>Connect tooStorSimple Eszközkezelő szolgáltatás

hello StorSimple Device Manager szolgáltatás fut a Microsoft Azure-ban, és kapcsolódik toomultiple StorSimple eszközökhöz. Használja ezeket az eszközöket egy böngésző toomanage futó központi Microsoft Azure-portálon. tooconnect toohello StorSimple Device Manager szolgáltatás a következő hello.

#### <a name="tooconnect-toohello-service"></a>tooconnect toohello szolgáltatás
1. Keresse meg a túl[https://portal.azure.com/](https://portal.azure.com/).
2. A Microsoft-fiók hitelesítő adatait használja, jelentkezzen be toohello Microsoft Azure-portálra (hello jobb felső részén hello ablaktáblában található).
3. Görgessen le a bal oldali navigációs ablaktábla tooaccess hello StorSimple Device Manager szolgáltatás hello.


## <a name="administer-storsimple-device-using-storsimple-device-manager-service"></a>StorSimple Device Manager szolgáltatással a StorSimple eszköz felügyelete

hello következő tábla összegzi hello általános kezelési feladatok és a bonyolult munkafolyamatok hello StorSimple Device Manager szolgáltatás felhasználói felületi belül végrehajtható. Ezek a feladatok hello felhasználói felület panelt, amelyen kezdeményezett alapján vannak rendezve.

Minden munkafolyamat kapcsolatos további információkért kattintson a hello eljárások hello táblában.

#### <a name="storsimple-device-manager-workflows"></a>StorSimple Device Manager munkafolyamatok

| Ha azt szeretné, toodo ez... | Ez az eljárás használható. |
| --- | --- |
| Szolgáltatás létrehozása</br>A szolgáltatás törlése</br>Szolgáltatásregisztrációs kulcs lekérése</br>Szolgáltatásregisztrációs kulcs újragenerálása |[A StorSimple Device Manager szolgáltatás telepítése](storsimple-8000-manage-service.md) |
| Hello tevékenység naplók megtekintése |[Hello StorSimple eszköz Manager szolgáltatással összefoglaló](storsimple-8000-service-dashboard.md) |
| Szolgáltatásadat-titkosítási kulcs hello módosítása</br>Hello művelet naplók megtekintése |[Hello StorSimple Device Manager szolgáltatás irányítópult](storsimple-8000-service-dashboard.md) |
| Eszköz inaktiválása</br>Eszköz törlése |[Inaktiválja vagy törölje az eszköz](storsimple-8000-deactivate-and-delete-device.md) |
| Vész-helyreállítási és eszköz-feladatátvétellel kapcsolatos további tudnivalók</br>Feladatátvételi tooa fizikai eszköz</br>Feladatátvételi tooa virtuális eszköz</br>Üzleti folytonosság vészhelyreállítási (BCDR) |[A StorSimple eszköz feladatátvétel és a katasztrófa utáni helyreállítás](storsimple-8000-device-failover-disaster-recovery.md) |
| A kötet biztonsági mentéseit lista</br>Válassza ki a biztonságimásolat-készletből</br>A biztonságimásolat-készlet törlése |[Biztonsági másolatok kezelése](storsimple-8000-manage-backup-catalog.md) |
| Kötet klónozása |[Kötet klónozása](storsimple-8000-clone-volume-u2.md) |
| Állítsa vissza a biztonságimásolat-készletből |[Állítsa vissza a biztonságimásolat-készletből](storsimple-8000-restore-from-backup-set-u2.md) |
| Tárolási fiókokról</br>Tárfiók hozzáadása</br>A storage-fiók szerkesztése</br>Tárfiók törlése</br>Kulcs elforgatási szögét storage-fiókok |[Tárfiókok kezelése](storsimple-8000-manage-storage-accounts.md) |
| Sávszélesség-sablonok</br>Sávszélességsablon hozzáadása</br>A sávszélesség-sablon szerkesztése</br>Sávszélesség sablon törlése</br>Sávszélesség alapértelmezett sablon használata</br>Egy megadott időpontban elinduló napos sávszélesség sablon létrehozása |[Sávszélességsablonok kezelése](storsimple-8000-manage-bandwidth-templates.md) |
| Hozzáférés-vezérlési rekordokat kapcsolatos</br>Hozzon létre egy hozzáférés-vezérlési rekordot</br>Egy hozzáférés-vezérlési rekordot szerkesztése</br>Egy hozzáférés-vezérlési rekordot törlése |[Hozzáférés-vezérlési rekordokat kezelése](storsimple-8000-manage-acrs.md) |
| Feladatok részleteinek megjelenítése</br>Feladatok megszakítása |[Feladatok kezelése](storsimple-8000-manage-jobs-u2.md) |
| Riasztási értesítések fogadása</br>Riasztások kezelése</br>Riasztások áttekintése |[StorSimple-riasztások megtekintése és kezelése](storsimple-8000-manage-alerts.md) |
| Figyelési diagramok létrehozása |[A StorSimple eszköz figyelése](storsimple-monitor-device.md) |
| A kötettároló hozzáadása</br>A kötettároló módosítása</br>A kötettároló törlése |[Kötettárolók kezelése](storsimple-8000-manage-volume-containers.md) |
| Kötet hozzáadása</br>A kötet módosítása</br>A kötet offline állapotba helyezése</br>Kötet törlése</br>A kötet figyelése |[Kötetek kezelése](storsimple-8000-manage-volumes-u2.md) |
| Eszköz beállításainak módosítása</br>Idő beállításainak módosítása</br>DNS.md beállításainak módosítása</br>Hálózati adapterek konfigurálása |[A StorSimple eszköz eszköz konfigurációjának módosítása](storsimple-8000-modify-device-config.md) |
| Webalkalmazás-proxy beállítások megjelenítése |[Az eszköz webalkalmazás-proxy konfigurálása](storsimple-8000-configure-web-proxy.md) |
| Eszköz rendszergazdai jelszavának módosítása</br>StorSimple Snapshot Manager jelszavának módosítása |[StorSimple-jelszavak módosítása](storsimple-8000-change-passwords.md) |
| A távfelügyelet konfigurálása |[Távoli csatlakozás tooyour StorSimple-eszköz](storsimple-8000-remote-connect.md) |
| A riasztási beállítások konfigurálása |[StorSimple-riasztások megtekintése és kezelése](storsimple-8000-manage-alerts.md) |
| A CHAP konfigurálása a StorSimple eszköz |[A CHAP konfigurálása a StorSimple eszköz](storsimple-configure-chap.md) |
| Biztonsági mentési szabályzat hozzáadása</br>Adja hozzá vagy ütemezésének módosítása</br>A biztonsági mentési házirend törlése</br>Manuális biztonsági mentés készítése</br>Hozzon létre egy egyéni biztonsági mentési házirend több kötetek és ütemezése |[Biztonsági mentési szabályzatok kezelése](storsimple-8000-manage-backup-policies-u2.md) |
| Állítsa le a eszközvezérlők</br>Indítsa újra a eszközvezérlők</br>Eszközvezérlők leállítása</br>Az eszköz toofactory visszaállítása</br>(Csak a helyi eszköz újabb rendszer) |[A StorSimple eszköz vezérlő kezelése](storsimple-8000-manage-device-controller.md) |
| További tudnivalók a StorSimple hardverösszetevők</br>Figyelje meg a hardver állapotát</br>(Csak a helyi eszköz újabb rendszer) |[A figyelő hardverösszetevők](storsimple-8000-monitor-hardware-status.md) |
| Hozzon létre egy támogatási csomag |[Létrehozását és kezelését egy támogatási csomag](storsimple-8000-contact-microsoft-support.md#start-a-support-session-in-windows-powershell-for-storsimple) |
| Telepítse a szoftverfrissítéseket |[Az eszköz frissítése](storsimple-update-device.md) |

## <a name="next-steps"></a>Következő lépések

Ha probléma merül fel a StorSimple eszköz hello napi szintű üzemeltetése vagy bármelyik hardver, tekintse meg:

* [Hello diagnosztikai eszköz használata – hibaelhárítás](storsimple-8000-diagnostics.md)
* [Kijelző LED figyelési StorSimple használata](storsimple-monitoring-indicators.md)

Ha mégsem sikerül megoldani a hello problémákat, és meg kell toocreate szolgáltatáskérés, tekintse meg a túl[forduljon a Microsoft ügyfélszolgálatához](storsimple-8000-contact-microsoft-support.md).

