---
title: "Azure StorSimple Manager virtuális tömb felügyeleti aaaMicrosoft |} Microsoft Docs"
description: "Ismerje meg, hogyan toomanage a StorSimple a helyszíni virtuális tömb hello StorSimple Device Manager szolgáltatással a hello Azure-portálon."
services: storsimple
documentationcenter: 
author: alkohli
manager: carmonm
editor: 
ms.assetid: 958244a5-f9f5-455e-b7ef-71a65558872e
ms.service: storsimple
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 12/1/2016
ms.author: alkohli
ms.openlocfilehash: 1fabf9ca524b461266346a6cabf49aef772032ea
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="use-hello-storsimple-device-manager-service-tooadminister-your-storsimple-virtual-array"></a>Használja a következő hello StorSimple Device Manager szolgáltatás tooadminister a StorSimple virtuális tömböt
![a telepítő folyamatábra](./media/storsimple-virtual-array-manager-service-administration/manage4.png)

## <a name="overview"></a>Áttekintés
Ez a cikk ismerteti a hello StorSimple Device Manager szolgáltatási felületén, beleértve a hogyan tooconnect tooit és a különböző lehetőségekről, és hivatkozások toohello adott munkafolyamatok előírja, hogy hello hajtható végre a felhasználói felületen keresztül.

A cikk elolvasása után fog tudni hogyan:

* Connect toohello StorSimple Device Manager szolgáltatás
* Keresse meg a StorSimple eszköz kezelő felhasználói felületén hello
* A StorSimple virtuális tömb keresztül hello StorSimple Device Manager szolgáltatás felügyelete

> [!NOTE]
> tooview hello rendelkezésre álló felügyeleti lehetőségeket hello StorSimple 8000 series eszköz esetén nyissa meg túl[használata hello StorSimple Manager szolgáltatás tooadminister a StorSimple eszköz](storsimple-manager-service-administration.md).
> 
> 

## <a name="connect-toohello-storsimple-device-manager-service"></a>Connect toohello StorSimple Device Manager szolgáltatás
hello StorSimple Device Manager szolgáltatás fut a Microsoft Azure-ban, és toomultiple StorSimple virtuális tömbök kapcsolódik. Használja ezeket az eszközöket egy böngésző toomanage futó központi Microsoft Azure-portálon. tooconnect toohello StorSimple Device Manager szolgáltatás a következő hello.

#### <a name="tooconnect-toohello-service"></a>tooconnect toohello szolgáltatás
1. Nyissa meg túl[https://ms.portal.azure.com](https://ms.portal.azure.com).
2. A Microsoft-fiók hitelesítő adatait használja, jelentkezzen be toohello Microsoft Azure-portálra (hello jobb felső részén hello ablaktáblában található).
3. Keresse meg tooBrowse--> "Filter" a StorSimple eszköz kezelői tooview megadott előfizetés az eszköz vezetők.

## <a name="use-hello-storsimple-device-manager-service-tooperform-management-tasks"></a>Hello StorSimple Device Manager szolgáltatás tooperform felügyeleti feladatok használata
hello következő tábla összegzi hello általános kezelési feladatok és a bonyolult munkafolyamatok hello StorSimple Device Manager szolgáltatás összefoglaló panel belül végrehajtható. Ezek a feladatok hello panelt, amelyen kezdeményezett alapján vannak rendezve.

Minden munkafolyamat kapcsolatos további információkért kattintson a hello eljárások hello táblában.

#### <a name="storsimple-device-manager-workflows"></a>StorSimple Device Manager munkafolyamatok
| Ha azt szeretné, toodo ez... | Ezzel az eljárással |
| --- | --- | --- |
| Szolgáltatás létrehozása</br>A szolgáltatás törlése</br>Hello Szolgáltatásregisztrációs kulcs lekérése</br>Hello Szolgáltatásregisztrációs kulcs újragenerálása |[Hello StorSimple Device Manager szolgáltatás telepítése](storsimple-virtual-array-manage-service.md) |
| Hello tevékenység naplók megtekintése |[Összegző hello StorSimple szolgáltatás használata](storsimple-virtual-array-service-summary.md) |
| Egy virtuális tömb inaktiválása</br>Egy virtuális tömb törlése |[Inaktiválja vagy törölje a virtuális tömb](storsimple-virtual-array-deactivate-and-delete-device.md) |
| Vész-helyreállítási és eszköz feladatátvevő</br>Feladatátvételi Előfeltételek</br>Üzleti folytonosság vészhelyreállítási (BCDR)</br>A vészhelyreállítás során hibák |[A StorSimple virtuális tömb vész helyreállítási és eszköz feladatátvétele](storsimple-virtual-array-failover-dr.md) |
| Megosztások és a kötetek biztonsági mentése</br>Manuális biztonsági mentés készítése</br>Hello biztonsági mentési ütemezés módosítása</br>Meglévő biztonsági másolatok megtekintése |[Készítsen biztonsági másolatot a StorSimple virtuális tömb](storsimple-virtual-array-backup.md) |
| Klónozás megosztások biztonságimásolat-készletből</br>A biztonságimásolat-készletből kötetek klónozása</br>Elemszintű helyreállítás (csak a fájlkiszolgáló) |[A StorSimple virtuális tömb biztonsági másolatából klónozása](storsimple-virtual-array-clone.md) |
| Tárolási fiókokról</br>Tárfiók hozzáadása</br>A storage-fiók szerkesztése</br>Tárfiók törlése |[Hello StorSimple virtuális tömb storage-fiókok kezelése](storsimple-virtual-array-manage-storage-accounts.md) |
| Hozzáférés-vezérlési rekordokat kapcsolatos</br>Hozzáadása vagy módosítása egy hozzáférés-vezérlési rekordot </br>Egy hozzáférés-vezérlési rekordot törlése |[Hozzáférés-vezérlési rekordokat a StorSimple virtuális tömb hello kezelése](storsimple-virtual-array-manage-acrs.md) |
| Feladatok részleteinek megjelenítése |[A StorSimple virtuális tömb feladatok kezelése](storsimple-virtual-array-manage-jobs.md) |
| A riasztási beállítások konfigurálása</br>Riasztási értesítések fogadása</br>Riasztások kezelése</br>Riasztások áttekintése |[A StorSimple virtuális tömb hello riasztások megtekintése és kezelése](storsimple-virtual-array-manage-alerts.md) |
| Hello eszköz rendszergazdai jelszavának módosítása |[Hello StorSimple virtuális tömb eszköz rendszergazdai jelszavának módosítása](storsimple-virtual-array-change-device-admin-password.md) |
| Telepítse a szoftverfrissítéseket |[A virtuális tömb frissítése](storsimple-virtual-array-install-update.md) |

> [!NOTE]
> Hello kell használnia [helyi webes felhasználói felület](storsimple-ova-web-ui-admin.md) a hello a következő feladatokat:
> 
> * [Hello szolgáltatásadat-titkosítási kulcs beolvasása](storsimple-ova-web-ui-admin.md#get-the-service-data-encryption-key)
> * [Hozzon létre egy támogatási csomag](storsimple-ova-web-ui-admin.md#generate-a-log-package)
> * [Állítsa le és indítsa újra a virtuális tömb](storsimple-ova-web-ui-admin.md#shut-down-and-restart-your-device)
> 
> 

## <a name="next-steps"></a>Következő lépések
Hello információ a webes felhasználói felülete és hogyan toouse, nyissa meg túl[használata hello StorSimple webes felhasználói felület tooadminister a StorSimple virtuális tömb](storsimple-ova-web-ui-admin.md).

