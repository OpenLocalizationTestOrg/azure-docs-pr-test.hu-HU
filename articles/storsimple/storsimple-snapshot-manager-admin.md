---
title: "Pillanatkép-kezelő felügyeleti aaaStorSimple |} Microsoft Docs"
description: "Áttekintés és hivatkozások toomore információ a StorSimple Snapshot Manager megoldás felügyeleti feladatokkal és munkafolyamatok biztosít."
services: storsimple
documentationcenter: NA
author: SharS
manager: timlt
editor: 
ms.assetid: 1cdbb61d-bd16-4be4-ade2-ceab11508acb
ms.service: storsimple
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: TBD
ms.date: 06/05/2016
ms.author: v-sharos
ms.openlocfilehash: d875f2efbdeb844b412cf8d9f1f971f18da7526e
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="use-storsimple-snapshot-manager-tooadminister-your-storsimple-solution"></a>StorSimple Snapshot Manager tooadminister a StorSimple megoldás használja

## <a name="overview"></a>Áttekintés
StorSimple Snapshot Manager egy Microsoft Management Console (MMC) beépülő modulja, amely leegyszerűsíti az adatvédelem és biztonsági mentési kezelése a Microsoft Azure StorSimple környezetben. A StorSimple Snapshot Manager segítségével kezelheti a Microsoft Azure StorSimple adatok hello adatközpont és felhő hello egyetlen integrált tárolási megoldásként, így biztonsági mentési folyamat egyszerűsítése, illetve csökkenti a költségeket.

hello StorSimple Snapshot Manager központi felügyeleti konzol lehetővé teszi toocreate egységes, időpontban a biztonsági másolatok a helyi és felhőbeli adatát. Például hello konzolját is használhatja:

* Konfigurálhatja, készítsen biztonsági másolatot, és törölje köteteket.
* Konfigurálja a kötet, amely az adatok biztonsági másolatát csoportok tooensure alkalmazáskonzisztens.
* Biztonsági mentési házirendek kezelése, így az adatok biztonsági másolatát egy előre meghatározott ütemezés szerint.
* Az adatokat, amelyeket hello felhőben tárolt és a katasztrófa utáni helyreállítás független példányt hoz létre.

Ez a cikk ismerteti, amelyek ismertetik a StorSimple Snapshot Manager hivatkozások tootutorials és hogyan toouse azt toocomplete rendszer felügyeleti feladatokkal és munkafolyamatok.

* További információ a StorSimple Snapshot Manager összetevőit és architektúra: [Mi az StorSimple Snapshot Manager?](storsimple-what-is-snapshot-manager.md) 
* StorSimple Snapshot Manager toodownload lépjen túl[hello StorSimple Snapshot Manager letöltési oldala](https://www.microsoft.com/download/details.aspx?id=44220).
* A StorSimple Snapshot Manager központi telepítési eljárásokat, nyissa meg túl[StorSimple Snapshot Manager telepítése](storsimple-snapshot-manager-deployment.md).

> [!NOTE]
> StorSimple Snapshot Manager toomanage Microsoft Azure StorSimple virtuális tömbök (más néven StorSimple a helyszíni virtuális eszköz) nem használható.


## <a name="storsimple-snapshot-manager-tasks-and-workflows"></a>StorSimple Snapshot Manager feladatok és a munkafolyamatok
StorSimple Snapshot Manager toomonitor hello használja, és aktuális, ütemezett és befejezett biztonsági mentési feladatok kezelése. StorSimple Snapshot Manager emellett mentése befejeződött too64 biztonsági mentés katalógusa. Hello katalógus toofind használja, és a kötetek vagy fájlok visszaállítása. 

| Ha azt SZERETNÉ, tooDO ez... | EZ AZ OKTATÓANYAG HASZNÁLNI... |
|:--- |:--- |
| További információ a StorSimple Snapshot Manager |[Mi az a StorSimple Snapshot Manager?](storsimple-what-is-snapshot-manager.md) |
| StorSimple Snapshot Manager telepítése<br>Telepítse újra a StorSimple Snapshot Manager<br>Távolítsa el a StorSimple Snapshot Manager |[A StorSimple Snapshot Manager központi telepítése](storsimple-snapshot-manager-deployment.md) |
| Használja a StorSimple Snapshot Manager menük és szolgáltatások:<ul><li>Menüsáv</li><li>Eszköztár</li><li>Hatókör ablaktábla</li><li>Eredmények ablaktábla</li><li>Műveletek panel</li><li>Billentyűparancsokkal és parancsikonok</li></ul> |[StorSimple Snapshot Manager felhasználói felülete](storsimple-use-snapshot-manager.md) |
| Hello közös MMC szolgáltatásai a StorSimple Snapshot Manager használata:<ul><li>Nézet</li><li>Itt új ablak</li><li>Frissítés</li><li>Lista exportálása</li><li>Súgó</li></ul> |[A StorSimple Snapshot Manager hello MMC menü műveleteket használni](storsimple-snapshot-manager-mmc-menu.md) |
| Adja hozzá, vagy cserélje le az eszköz<br>Egy eszköz csatlakoztatása<br>Ellenőrizze az importált kötet csoportok<br>Frissítse a csatlakoztatott eszközök<br>Egy eszköz hitelesítéséhez<br>Eszköz részleteinek megtekintése<br>Egy eszköz konfigurálásának törlése<br>A jelszó módosítása<br>Cserélje le a hibás eszköz<br> |[StorSimple Snapshot Manager tooconnect használja, és a StorSimple eszközök kezelése](storsimple-snapshot-manager-manage-devices.md) |
| Kötet csatlakoztatása<br>Kötetek adatainak megtekintése<br>Kötet törlése<br>Kötetek újraellenőrzése<br>Konfigurálja, és készítsen biztonsági másolatot alaplemezek<br>Konfigurálja és a dinamikus tükrözött kötetek biztonsági mentése |[StorSimple Snapshot Manager tooview használja, és a kötetek kezelése](storsimple-snapshot-manager-manage-volumes.md) |
| Kötet csoportok megtekintése<br>Kötet csoport létrehozása<br>Készítsen biztonsági másolatot egy kötet csoport<br>Kötet csoport szerkesztése<br>Kötet csoport törlése |[StorSimple Snapshot Manager toocreate használja, és a kötet csoportok kezelése](storsimple-snapshot-manager-manage-volume-groups.md) |
| A biztonsági mentési házirend létrehozása <br>A biztonsági mentési házirend szerkesztése<br>A biztonsági mentési házirend törlése |[StorSimple Snapshot Manager toocreate használatát és kezelését a biztonsági mentési házirendek](storsimple-snapshot-manager-manage-backup-policies.md) |
| Megtekinthető és kezelhető az ütemezett biztonsági mentési feladatok<br>Megtekintheti és kezelheti a legutóbbi biztonsági mentési feladatok<br>Megtekintheti és kezelheti a jelenleg futó biztonsági mentési feladatok |[StorSimple Snapshot Manager tooview használja, és a biztonsági mentési feladatok kezelése](storsimple-snapshot-manager-manage-backup-jobs.md) |
| A kötet visszaállítása<br>Egy kötet vagy kötet klónozása<br>Biztonsági<br>A fájl helyreállításához<br>Hello StorSimple Snapshot Manager adatbázisának visszaállítása |[Használja a StorSimple Snapshot Manager toomanage hello biztonságimásolat-katalógus](storsimple-snapshot-manager-manage-backup-catalog.md) |

## <a name="next-steps"></a>Következő lépések
[Töltse le a StorSimple Snapshot Manager](https://www.microsoft.com/download/details.aspx?id=44220).

