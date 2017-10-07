---
title: "aaaStorSimple 8000 Series Update 5 kibocsátási megjegyzései |} Microsoft Docs"
description: "Hello új funkciókat, problémák és megoldások ismerteti a StorSimple 8000 Series Update 5."
services: storsimple
documentationcenter: NA
author: alkohli
manager: timlt
editor: 
ms.assetid: 
ms.service: storsimple
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: TBD
ms.date: 08/23/2017
ms.author: alkohli
ms.openlocfilehash: 9eb8ffb97b41ce3d4f1ffdf2975f904d0a2958e7
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="storsimple-8000-series-update-5-release-notes"></a>A StorSimple 8000 Series Update 5 kibocsátási megjegyzései

## <a name="overview"></a>Áttekintés

hello alábbi kibocsátási megjegyzések leírják hello új szolgáltatásokat és a StorSimple 8000 Series Update 5 hello kritikus megnyitott problémák azonosításához. Ebben a kiadásban szereplő hello StorSimple szoftverfrissítések listáját is tartalmaznak.

5. frissítésétől alkalmazott tooany StorSimple eszköz 0,1 Update futtatása – Update 4 lehet. 5. frissítésétől társított hello eszköz verziója 6.3.9600.17845.

Tekintse át a hello kibocsátási megjegyzések hello központi telepítése előtt frissítse a StorSimple megoldásban lévő hello információt.

> [!IMPORTANT]
> * 5. frissítésétől eszközhöz, lemez belső vezérlőprogramok, operációs rendszer biztonsági és más operációs rendszer frissítése érdekében van. Megközelítőleg 4 óra tooinstall veszi ezt a frissítést. Vezérlőprogram-frissítés zavaró frissítésről, és az eszköz egy leállásának eredményez. Azt javasoljuk, hogy alkalmaz frissítési 5 tookeep az eszköz naprakész.
> * Új kiadásokban nem láthatók frissítések azonnal, mert a hello frissítések fázisokra bontva történő bevezetéséhez végezzük. Néhány napot várni, majd újra ezeket a frissítéseket a frissítések keresése hamarosan elérhető lesz.

## <a name="whats-new-in-update-5"></a>5. frissítésétől újdonságai

hello következő fontos fejlesztést tartalmaz, és hibajavítások végzett frissítés 5.

* **Használja az Azure Active Directory (AAD) tooauthenticate StorSimple Device Manager szolgáltatással** – az 5. frissítésétől kezdve, az Azure Active Directory használt tooauthenticate hello StorSimple Device Manager szolgáltatással. hello régi hitelesítési mechanizmus December 2017 elavulttá válik. Hello minden felhasználó be a tűzfalszabályok hello új hitelesítési URL-címének tartalmaznia kell. További információ: túl[hitelesítési URL-címek, hálózati követelményei a StorSimple eszköz hello felsorolt](storsimple-8000-system-requirements.md#url-patterns-for-azure-portal).

    Hello hitelesítés URL-címe nem szerepel a hello tűzfalszabályok, ha a hello felhasználók látni fogják a kritikus riasztás, amely a StorSimple eszköz hello szolgáltatás nem tudta hitelesíteni. Hello látnak a felhasználók ezt a figyelmeztetést, ha szükségük tooinclude hello új hitelesítési URL-címet. További információ: túl[riasztások hálózati StorSimple](storsimple-8000-manage-alerts.md#networking-alerts).

* **Új verziót a StorSimple Snapshot Manager** -alkalmazás Update 5 megjelenik a StorSimple Snapshot Manager új verziója. Azt javasoljuk, hogy a toothis verzióra frissíteni. Ezen verziója kompatibilis frissítés 3 futtató összes hello StorSimple eszközökhöz vagy újabb verzió van. További információ: túl[központi telepítése a StorSimple Snapshot Manager](storsimple-snapshot-manager-deployment.md).


## <a name="issues-fixed-in-update-5"></a>A frissítés 5 megoldott problémák

hello következő táblázat az összefoglalást tartalmazza a problémákat, amelyek a frissítés 5 javítva lett.

| Nem | Szolgáltatás | Probléma | Érvényes toophysical eszköz | Érvényes toovirtual eszköz |
| --- | --- | --- | --- | --- |
| 1 |Windows PowerShell-távelérés |Hello a korábbi verzióban egy jelenít meg a felhasználót hiba tooestablish közben egy távoli kapcsolat toohello StorSimple felhő készülék Windows PowerShell segítségével. Ez a probléma volt legfelső szintű okozott, és ebben a kiadásban rögzített. |Nem |Igen |
| 2 |Sávszélesség-sablonok |A korábbi kiadás következne sávszélesség-sablonok, amelyek alacsony sávszélességű, mint a milyen hello eszköz konfigurálása. Ebben a kiadásban a probléma megoldásához. |Igen |Igen |
| 3 |Feladatátvétel |Előző kiadásban Ha egy kötet nagy mennyiségű eszköz keresztül Update 4 futtató tooanother eszköz volt sikertelen hello folyamat sikertelen lesz tooapply hello hozzáférés-vezérlési rekordokat tett kísérlet során. Ez a probléma fennáll ebben a kiadásban. |Igen |Igen |



## <a name="known-issues-in-update-5-from-previous-releases"></a>A korábbi kiadásokból származó 5. frissítésétől kapcsolatos ismert problémák

Nincsenek új ismert problémák a frissítési 5. A korábbi kiadásokból származó 5 tooUpdate átvitt problémák listáját, a go túl[Update 3 kibocsátási megjegyzések](storsimple-update3-release-notes.md#known-issues-in-update-3).

## <a name="serial-attached-scsi-sas-controller-and-firmware-updates-in-update-5"></a>Soros csatlakozású SCSI (SAS) vezérlő és a belső vezérlőprogram frissítések frissítés 5

Ebben a kiadásban a vezérlő és LSI illesztőprogram és a belső vezérlőprogram frissítések rendelkezik. További információ a hogyan tooinstall ezeket a frissítéseket: [frissítés 5 telepítése](storsimple-8000-install-update-5.md) a StorSimple eszköz.

## <a name="storsimple-cloud-appliance-updates-in-update-5"></a>A frissítés 5 StorSimple felhő készülék frissítések

A frissítés alkalmazott toohello StorSimple felhő készülék (más néven hello virtuális eszköz) nem lehet. Új felhőalapú készülékek hello frissítés 5-lemezkép használatával létrehozott toobe kell. Kapcsolatos információk túl nyissa meg a StorSimple felhő készülék toocreate[központi telepítése és kezelése a StorSimple felhő készülék](storsimple-8000-cloud-appliance-u2.md).

## <a name="next-step"></a>Következő lépés

Ismerje meg, hogyan túl[frissítés 5 telepítése](storsimple-8000-install-update-5.md) a StorSimple eszköz.

