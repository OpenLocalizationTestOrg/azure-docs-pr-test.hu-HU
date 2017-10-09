---
title: "aaaStorSimple Snapshot Manager felhasználói felületén |} Microsoft Docs"
description: "Hello StorSimple Snapshot Manager felhasználói felület ismerteti és bemutatja hogyan toouse azt toomanage biztonsági mentési feladatok és hello a katalógus biztonsági mentését."
services: storsimple
documentationcenter: NA
author: SharS
manager: timlt
editor: 
ms.assetid: c7d91892-2881-41a2-a7a2-908dc3646493
ms.service: storsimple
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: TBD
ms.date: 06/05/2017
ms.author: v-sharos
ms.custom: 
ms.openlocfilehash: 865520fdaf1b559714b52b428ad136b084d65c99
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="use-storsimple-snapshot-manager-user-interface-toomanage-backup-jobs-and-backup-catalog"></a>Használja a StorSimple Snapshot Manager felhasználói felület toomanage biztonsági mentési feladatok és a biztonságimásolat-katalógus

## <a name="overview"></a>Áttekintés
StorSimple Snapshot Manager hello egy egyszerűen elsajátítható felhasználói felületet használja tootake és biztonsági mentéseit is rendelkezik. Ez az oktatóanyag bemutatása toohello felhasználói felületet biztosít, és ezután azt ismerteti, hogyan toouse minden hello összetevőt. StorSimple Snapshot Manager hello részletes ismertetését lásd: [Mi az StorSimple Snapshot Manager?](storsimple-what-is-snapshot-manager.md)

### <a name="console-description"></a>Konzol leírása
tooview hello felhasználói felület, hello StorSimple Snapshot Manager ikonra az asztalon. hello konzolablak jelenik meg, ahogy az ábra a következő hello.

![StorSimple Snapshot Manager ablaktáblák](./media/storsimple-use-snapshot-manager/HCS_SSM_gui_panes.png)

hello konzolablak öt fő elemmel rendelkezik. Minden elem teljes leírását hello megfelelő hivatkozásra kattintva.

* [Menüsáv](#menu-bar) 
* [Eszköztár](#tool-bar) 
* [Hatókör ablaktábla](#scope-pane) 
* [Eredmények ablaktábla](#results-pane) 
* [Műveletek panel](#actions-pane) 

Emellett támogatja a StorSimple Snapshot Manager hello [navigációs és számos parancsikont billentyűzet](#keyboard-navigation-and-shortcuts).

### <a name="console-accessibility"></a>Konzol kisegítő lehetőségek
hello StorSimple Snapshot Manager felhasználói felülete támogatja az hello kisegítő lehetőségek hello Windows operációs rendszer és hello Microsoft Management Console (MMC), valamint az egyes StorSimple Snapshot Manager-specifikus billentyűparancsok által biztosított. 

* Hello Windows kisegítő lehetőségei leírását, nyissa meg túl[azok a billentyűparancsok Windows](https://support.microsoft.com/kb/126449). 
* Hello MMC kisegítő lehetőségek leírása, a go túl[kisegítő lehetőségek az MMC 3.0](https://technet.microsoft.com/library/cc766075.aspx)
* Hello StorSimple Snapshot Manager kisegítő lehetőségei leírását, nyissa meg túl[navigációs és parancsikon billentyűzet](#keyboard-navigation-and-shortcuts).

## <a name="menu-bar"></a>Menüsáv
hello menüsorban hello konzolablak hello tetején található [fájl](#file-menu), [művelet](#action-menu), [nézet](#view-menu), [Kedvencek](#favorites-menu), [ablak ](#window-menu), és [súgó](#help-menu) menün kellene végiglépkednie.

A rendelkezésre álló parancsok a menüt hello menü sáv toosee a listában valamelyik elemre kattintva. hello alábbi példa bemutatja hello **nézet** hello menüsávjában kijelölt menü.

![Kijelölt nézet menü](./media/storsimple-use-snapshot-manager/HCS_SSM_View_menu.png)

### <a name="file-menu"></a>Fájl menü
Hello **fájl** menü szabványos Microsoft Management Console (MMC) parancsokat tartalmaz.

#### <a name="menu-access"></a>Menüpont
tooview hello **fájl** menüben kattintson a **fájl** hello menüsávjában. a következő menü hello jelenik meg.

![StorSimple Snapshot Manager fájl menü](./media/storsimple-use-snapshot-manager/HCS_SSM_FileMenu.png) 

#### <a name="menu-description"></a>Menü leírása
hello következő táblázat ismerteti megjelenő elemek hello **fájl** menü.

| Menüpont | Leírás |
|:--- |:--- |
| új |Kattintson a **új** toocreate hello StorSimple Snapshot Manager alapján egy új konzolt. |
| Nyílt |Kattintson a **nyitott** tooopen egy meglévő konzolt. |
| Mentés |Kattintson a **mentése** toosave hello jelenlegi konzolt. |
| Mentés másként |Kattintson a **Mentés másként** toocreate egy új, átnevezett példánya hello jelenlegi konzolt. Használjon hello **Mentés másként** toocustomize nézet lehetőséget, majd mentse a későbbi beolvasásához. Például létrehozhatja a StorSimple Snapshot Manager beépülő modulok adott pont toospecific kiszolgálók. |
| Hozzáadása beépülő modul |Kattintson a **beépülő modul hozzáadása/eltávolítása** tooadd, vagy távolítsa el a beépülő modulok és tooorganize hello csomópontjának **hatókör** ablaktáblán. További információ: túl[hozzáadása, eltávolítása, és rendezés beépülő modulok és bővítmények az MMC 3.0](https://technet.microsoft.com/library/cc722035.aspx). |
| Beállítások |Kattintson a **beállítások** toochange hello konzol ikonra, adja meg a felhasználói hozzáférési mód és az engedélyek, vagy törölje a konzol fájlok tooincrease rendelkezésre álló szabad lemezterület. |
| Fájl elérési utak listája |Kattintson a hello számozott lista tooreopen nemrég megnyitott fájl elérési útját. |
| Kilépés |Kattintson a **kilépési** tooclose hello **fájl** menü. |

### <a name="action-menu"></a>Művelet menü
Használjon hello **művelet** menü tooselect az elérhető műveletek. hello elemek elérhető tooyou függenek hello beállítástól hello **hatókör** ablaktáblán vagy **eredmények** ablaktáblán.

#### <a name="menu-access"></a>Menüpont
tooview hello **művelet** menü, tegye a következők hello valamelyikét:

* Kattintson a jobb gombbal a hello elemének **hatókör** ablaktáblán vagy **eredmények** ablaktáblán.
* Jelöljön ki egy elemet a hello **hatókör** ablaktáblán vagy **eredmények** ablaktáblán, és kattintson **művelet** hello menüsávjában. 

Például, ha kiválaszt hello felső csomópont hello **hatókör** ablaktáblán, majd kattintson a jobb gombbal, illetve kattintson **művelet** hello menüsávban hello következő menü jelenik meg.

![StorSimple Snapshot Manager művelet menü](./media/storsimple-use-snapshot-manager/HCS_SSM_Action_menu.png)

Hello **műveletek** ablaktáblán (a hello sarkában hello konzol) tartalmaz hello műveletek hello, ugyanazt a listát **művelet** menü. Emellett hello **műveletek** ablaktábla tartalmaz hello **nézet** menü elemeit, amelyek lehetővé teszik a toocreate hello egyéni nézetét **eredmények** ablaktáblán.

![Nyissa meg a Műveletek panel a Nézet menü](./media/storsimple-use-snapshot-manager/HCS_SSM_ActionsPane_Results.png)

#### <a name="menu-description"></a>Menü leírása
a következő táblázat hello StorSimple Snapshot Manager műveletek betűrendbe szedett listáját tartalmazza. 

* Hello **művelet** oszlopban található csomópontokon és eredmények hajthat végre műveleteket. 
* Hello **navigációs** oszlop ismerteti, hogyan toodisplay hello megfelelő **művelet** menü, hogy hello műveletét választja. Egyes műveletek jelennek meg több **művelet** menün kellene végiglépkednie. Ezek a műveletek végrehajtásához, válasszon ki egy **navigációs** hello felsorolás elemet. 
* Hello **leírás** oszlop ismerteti, hogyan toouse minden egyes művelethez hello **művelet** menü vagy a Műveletek ablaktáblában, és elmagyarázza, hogyan kezeli.

> [!NOTE]
> Hello **műveletek** ablaktáblán és **művelet** menük tartalmazhat további beállításokat, például **nézet**, **új ablak innen**,  **Frissítse**, **lista exportálása**, és **súgó**. Ezek a beállítások az hello MMC részeként elérhető, és nem adott tooStorSimple Snapshot Manager. hello táblázat ezek a beállítások leírását tartalmazza.
> 
> 

| Műveletek | Navigációs | Leírás |
|:--- |:--- |:--- |
| Hitelesítés |Hello kattintson **eszközök** csomópontját, és kattintson a jobb gombbal egy eszközt az hello **eredmények** ablaktáblán. |Kattintson a **hitelesítés** tooenter hello jelszó hello eszköz konfigurált. |
| Klónozás |Bontsa ki a **biztonságimásolat-katalógus**, bontsa ki a **felhőalapú pillanatfelvételek**, és egy dátummal biztonsági mentés gombra, majd jelöljön ki egy kötetet hello **eredmények** ablaktáblán. |Kattintson a **Klónozás** toocreate felhő másolatát pillanatkép- és a megadott helyen tárolja. |
| Egy eszköz konfigurálása |Kattintson a jobb gombbal hello **eszközök** csomópont. |Kattintson a **eszköz konfigurálása** tooconfigure egyetlen vagy több eszköz tooconnect toohello Windows-állomás. |
| A biztonsági mentési házirend létrehozása |Hello következő módokon:<ul><li>Kattintson a jobb gombbal **biztonsági mentési házirendek**.</li><li>Kattintson a gombra, vagy bontsa ki a **kötet csoportok**, és kattintson a jobb gombbal egy kötet csoportot.</li><li>Kattintson a gombra, vagy bontsa ki a **biztonságimásolat-katalógus**, majd kattintson a jobb gombbal egy kötet csoportot.</li></ul> |Kattintson a **biztonsági mentési házirend létrehozása** tooconfigure egy kötet csoport ütemezett biztonsági mentés. |
| Kötet csoport létrehozása |Hello következő módokon:<ul><li>Hello kattintson **kötetek** csomópontját, és kattintson rá jobb gombbal a hello kötet **eredmények** ablaktáblán.</li><li>Kattintson a jobb gombbal hello **kötet csoportok** csomópont.</li></ul> |Kattintson a **kötet csoport létrehozása** tooassign kötetek tooa kötet csoport. |
| Törlés |Kattintson a csomópont vagy az eredmény (Ez az elem megjelenő számos **művelet** menük és **műveletek** ablaktáblák.) |Kattintson a **törlése** toodelete hello csomópont vagy a kiválasztott eredménye. Amikor hello megerősítést kérő párbeszédpanel jelenik meg, győződjön meg arról, vagy hello törlésének visszavonásához. |
| Részletek |Hello kattintson **eszközök** csomópontját, és kattintson rá jobb gombbal egy eszközt az hello **eredmények** ablaktáblán. |Kattintson a **részletek** toosee hello konfigurációs adatait egy eszköz számára. |
| Szerkesztés |Kattintson a **biztonsági mentési házirendek**, majd kattintson a jobb gombbal a hello-szabályzat **eredmények** ablaktáblán. |Kattintson a **szerkesztése** toochange hello biztonsági mentés ütemezése egy kötet csoport. |
| Lista exportálása |Kattintson bármelyik csomópontját vagy az eredmény (Ez az elem jelenik meg az összes **művelet** menük és **műveletek** ablaktáblák.) |Kattintson a **lista exportálása** toosave listáját, amely egy vesszővel tagolt (CSV) fájl. Egy táblázat alkalmazásba elemzéshez majd importálja ezt a fájlt. |
| Súgó |Kattintson bármelyik csomópontját vagy eredménye. (Ez az elem jelenik meg az összes **művelet** menük és **műveletek** ablaktáblák.) |Kattintson a **súgó** tooopen egy külön böngészőablakban online súgójában. |
| Itt új ablak |Kattintson bármelyik csomópontját vagy az eredmény (Ez az elem jelenik meg az összes **művelet** menük és **műveletek** ablaktáblák.) |Kattintson a **új ablak innen** tooopen StorSimple Snapshot Manager új ablakban. |
| Frissítés |Kattintson bármelyik csomópontját vagy az eredmény (Ez az elem jelenik meg az összes **művelet** menük és **műveletek** ablaktáblák.) |Kattintson a **frissítése** tooupdate aktuálisan megjelenő hello StorSimple Snapshot Manager ablak. |
| Eszköz frissítése |Hello kattintson **eszközök** csomópontját, és kattintson a jobb gombbal egy eszközt az hello **eredmények** ablaktáblán. |Kattintson a **frissítése eszköz** toosynchronize egy adott csatlakoztatott eszköz StorSimple Snapshot Manager. |
| Eszközök frissítése |Kattintson a jobb gombbal hello **eszközök** csomópont. |Kattintson a **frissítési eszközök** toosynchronize a StorSimple Snapshot Manager csatlakoztatott eszközök listáját. |
| Kötetek újraellenőrzése |Kattintson a jobb gombbal hello **kötetek** csomópont. |Kattintson a **ismételt vizsgálat kötetek** tooupdate hello kötetek listáját hello megjelenő **eredmények** ablaktáblán. |
| Visszaállítás |Bontsa ki a **biztonságimásolat-katalógus**, bontsa ki a kötet csoport, **helyi pillanatképeket** vagy **felhőalapú pillanatfelvételek**, majd kattintson a jobb gombbal a biztonsági mentés. |Kattintson a **visszaállítása** tooreplace hello aktuális kötet csoport adatai hello kijelölt biztonsági hello adataival. |
| Biztonsági mentés igénybe |Hello következő módokon:<ul><li>Bontsa ki a **kötet csoportok**, majd kattintson a jobb gombbal egy kötet csoportot.</li><li>Bontsa ki a **biztonságimásolat-katalógus**, majd kattintson a jobb gombbal egy kötet csoportot.</li></ul> |Kattintson a **érvénybe biztonsági mentési** toostart azonnal a biztonsági mentési feladatot. |
| Váltás importálja megjelenítése |Kattintson a jobb gombbal hello felső csomópont hello **hatókör** ablaktáblán (hello **StorSimple Snapshot Manager** hello példákban csomópont). |Kattintson a **váltása importálja megjelenítési** tooshow vagy elrejtés hello kötet csoportok és a társított biztonsági másolatok, az importált hello StorSimple Device Manager szolgáltatás irányítópultját. |

### <a name="view-menu"></a>Nézet menü
Használjon hello **nézet** menü toocreate hello egyéni nézetét **eredmények** ablaktábla tartalmát. Hello **nézet** menü tartalmaz **oszlopok hozzáadása/eltávolítása** és **Testreszabás** beállítások.

#### <a name="menu-access"></a>Menüpont
Van-e hozzáférési hello **nézet** menü hello menüsáv vagy hello **műveletek** ablaktáblán.

![StorSimple Snapshot Manager Nézet menü](./media/storsimple-use-snapshot-manager/HCS_SSM_View_menu.png) 

#### <a name="menu-description"></a>Menü leírása
hello következő táblázat ismerteti megjelenő elemek hello **nézet** menü.

| Menüpont | Leírás |
|:--- |:--- |
| Oszlopok hozzáadása/eltávolítása |Kattintson a **oszlopok hozzáadása/eltávolítása** hello tooadd, vagy távolítsa el oszlopai **eredmények** ablaktáblán. |
| Testreszabás |Kattintson a **Testreszabás** tooshow vagy elrejtés elemek hello StorSimple Snapshot Manager console ablakban. |

### <a name="favorites-menu"></a>A Kedvencek menü
Hello használata **Kedvencek** menü tooadd, eltávolítása és rendszerezése Lapmegtekintések és feladatokat, amelyek gyakran használják. 

#### <a name="menu-access"></a>Menüpont
Van-e hozzáférési hello **Kedvencek** hello menüsáv menüjéből.

![StorSimple Snapshot Manager Kedvencek menü](./media/storsimple-use-snapshot-manager/HCS_SSM_FavoritesMenu.png)

#### <a name="menu-description"></a>Menü leírása
hello következő táblázat ismerteti megjelenő elemek hello **Kedvencek** menü.

| Menüpont | Leírás |
|:--- |:--- |
| TooFavorites hozzáadása |Kattintson a **tooFavorites hozzáadása** tooadd hello aktuális tooyour listájának megtekintése a Kedvencek. |
| Kedvencek rendezése |Kattintson a **Kedvencek rendezése** tooorganize hello a Kedvencek mappa tartalma. |

### <a name="window-menu"></a>Ablak menü
Használjon hello **ablak** menü tooadd és megváltoztassa a StorSimple Snapshot Manager konzol windows.

#### <a name="menu-access"></a>Menüpont
Van-e hozzáférési hello **ablak** hello menüsáv menüjéből.

![StorSimple Snapshot Manager ablak menü](./media/storsimple-use-snapshot-manager/HCS_SSM_WindowMenu.png)

számozott lista hello hello menü hello alján látható, nyissa meg a jelenleg használt windows hello. Kattintson a bármely ablakban adott lista toobring hello ablak hello előtér be. 

#### <a name="menu-description"></a>Menü leírása
hello következő táblázatban megjelenő hello elemek hello ablak menüben.

| Menüpont | Leírás |
|:--- |:--- |
| Új ablak |Kattintson a **új ablak** tooopen új konzolablakot (a hozzáadása toohello meglévő ablakban). |
| Lépcsőzetes elrendezés |Kattintson a **Cascade** toodisplay hello nyissa meg a konzolt windows kaszkádolt Style. |
| Egymás alatt |Kattintson a **csempe vízszintesen** toodisplay hello nyissa meg a konzolt windows csempe (vagy a rács) formátumban. |
| Ikonok elrendezése |Ha több konzol windows megnyitása és minimalizálása érdekében őket, és kattintson az asztal Elszórva **ikonok elrendezése** tooarrange őket a képernyő aljára hello a vízszintes sorban. |

### <a name="help-menu"></a>Súgó menü
Használjon hello **súgó** menü tooview elérhető online súgó a StorSimple Snapshot Manager és a hello MMC. Hello MMC és a StorSimple Snapshot Manager szoftver verziója a rendszeren jelenleg telepített kapcsolatos információkat is megtekintheti. 

Van-e hozzáférési hello **súgó** hello menüsáv menüjéből. StorSimple Snapshot Manager súgó-témaköröket is elérheti a hello **műveletek** ablaktáblán.

![StorSimple Snapshot Manager Súgó menü](./media/storsimple-use-snapshot-manager/HCS_SSM_HelpMenu.png)

#### <a name="menu-description"></a>Menü leírása
a következő táblázat hello hello Súgó menü megjelenő elemeket ismerteti.

| Menüpont | Leírás |
|:--- |:--- |
| A StorSimple Snapshot Manager súgó |Kattintson a **a StorSimple Snapshot Manager súgó** tooopen StorSimple Snapshot Manager súgó külön ablakban. |
| Súgótémakörök |Kattintson a **Súgó-témaköröket** tooopen MMC online súgó külön ablakban. |
| TechCenter webhelyen |Kattintson a **TechCenter webhely** tooopen hello Microsoft TechNet technológia Center kezdőlap külön ablakban. |
| A Microsoft Management Console kapcsolatos |Kattintson a **kapcsolatban a Microsoft Management Console** melyik verzióját a Microsoft Management Console hello toosee telepítve van a rendszeren. |
| A StorSimple Snapshot Manager névjegye |Kattintson a **kapcsolatos StorSimple Snapshot Manager** toosee hello beépülő modul melyik verziója telepítve van a rendszeren. |

## <a name="tool-bar"></a>Eszköztár
hello eszköztár alatt hello menüsávban található navigációs és a feladat ikonokat tartalmaz. Minden ikon helyi tooa adott feladat.

### <a name="icon-descriptions"></a>Ikon leírása
hello következő táblázatban megjelenő ikonok hello hello eszköztáron. 

| Ikon | Leírás |
|:--- |:--- |
| ![Bal nyílbillentyű](./media/storsimple-use-snapshot-manager/HCS_SSM_LeftArrow.png) |Kattintson a hello balra mutató nyíl ikon tooreturn toohello előző lapon. |
| ![Jobb nyílbillentyű](./media/storsimple-use-snapshot-manager/HCS_SSM_RightArrow.png) |Kattintson a hello jobbra mutató nyílra toogo toohello következő lapon (ha hello nyíl szürke, hello művelet nem érhető el). |
| ![Másolatot ikon](./media/storsimple-use-snapshot-manager/HCS_SSM_Up.png) |Hello konzolfán egy szinttel feljebb ikon toogo mentése hello kattintson (hello **hatókör** ablaktáblán). |
| ![Konzolfa megjelenítése/elrejtése](./media/storsimple-use-snapshot-manager/HCS_SSM_ShowConsoleTree.png) |Kattintson a hello megjelenítése konzol fa ikon tooshow vagy hello elrejtése **hatókör** ablaktáblán. |
| ![Lista exportálása](./media/storsimple-use-snapshot-manager/HCS_SSM_ExportListIcon.png) |Kattintson a hello exportálási lista ikonja tooexport lista tooa CSV-fájl megadott. |
| ![Súgó ikonra](./media/storsimple-use-snapshot-manager/HCS_SSM_HelpIcon.png) |Kattintson a hello súgó ikonra tooopen online MMC súgó-témakörhöz. |
| ![Műveletek panel megjelenítése vagy elrejtése](./media/storsimple-use-snapshot-manager/HCS_SSM_ShowAction.png) |Kattintson a hello megjelenítése **műveletek** ablaktábla ikon tooshow vagy elrejtés hello **műveletek** ablaktáblán. |

## <a name="scope-pane"></a>Hatókör ablaktábla
Hello **hatókör** ablaktábla hello bal oldali ablaktáblájában hello StorSimple Snapshot Manager felhasználói felületén. Hello konzol (vagy a csomópont) fa tartalmaz, és hello elsődleges navigációs mechanizmus a StorSimple Snapshot Manager. 

### <a name="scope-pane-structure"></a>Hatókör ablaktábla struktúra
Hello **hatókör** ablaktáblában található tartalmazza kattintható objektumok (csomópontok) vannak rendszerezve fastruktúrában. 

![Hatókör ablaktábla](./media/storsimple-use-snapshot-manager/HCS_SSM_Scope_pane.png) 

* tooexpand vagy összecsukásához a csomópontot, kattintson a hello nyíl ikon következő toohello csomópont neve.
* tooview hello állapot vagy a tartalmát egy csomópontot, kattintson a hello csomópont neve. hello információk megjelennek a hello **eredmények** ablaktáblán. 

Hello **hatókör** ablaktáblán hello a következő csomópontokat tartalmazza: 

* [Eszköz csomópont alatt](#devices-node) 
* [Kötetek csomópont](#volumes-node) 
* [Kötet csoportok csomópont](#volume-groups-node) 
* [Biztonsági mentési házirendek csomópont](#backup-policies-node) 
* [Biztonsági mentési katalógus csomópont](#backup-catalog-node) 
* [Feladatok csomópont](#jobs-node) 

### <a name="scope-pane-tasks"></a>Hatókör ablaktábla feladatok
Használhatja a hello **hatókör** ablaktábla toocomplete adott csomóponton művelet. egy feladat tooselect hello következő módokon:

* Kattintson a jobb gombbal a hello csomópontot, majd a hello feladatot menüpontot hello.
* Hello csomópontot, és kattintson a **művelet** hello menüsávjában. Válassza ki a hello feladatot menüpontot hello.
* Kattintson a hello csomópont, és jelölje ki hello műveletét hello **műveletek** ablaktáblán.

Válasszon ki egy csomópontot, és használja az ezen módszerek toosee valamelyikét a feladatlistában, csak a ezen a csomóponton végrehajtható műveletek jelennek meg.

### <a name="devices-node"></a>Eszköz csomópont alatt
Hello **eszközök** csomópont hello StorSimple eszközökhöz és a StorSimple virtuális eszköz sem, amelyek csatlakoztatott tooStorSimple Snapshot Manager jelöli. Válassza ki a csomópont tooconnect eszköz konfigurálása és importálja a társított kötetek, a kötetek csoportok és a meglévő biztonsági másolatok. Több eszköz egyetlen csatlakoztatott tooa gazdagép lehet.

* tooexpand hello csomópontot, kattintson a nyíl ikonra hello tovább túl**eszközök**.
* toosee egy elérhető műveletek menüjében kattintson a jobb gombbal a hello **eszközök** csomópont, vagy kattintson a jobb gombbal bármelyik hello csomópontok hello megjelenő bővített nézet.
* toosee konfigurált eszközöket, kattintson a **eszközök** a hello **hatókör** ablaktáblán. eszközök, és minden eszköz, adatai hello listája jelenik meg hello **eredmények** ablaktáblán.

### <a name="volumes-node"></a>Kötetek csomópont
Hello **kötetek** csomópont jelöli, amelyek megfelelnek a toohello kötetek hello állomást, köztük az iSCSI, és egy eszközön keresztül felderített keresztül felderített által csatlakoztatott meghajtók hello. A rendelkezésre álló köteteken csomópont tooview hello listáját használja, és rendelje hozzá az egyéni kötetek toovolume csoportok.

* tooexpand hello csomópontot, kattintson a nyíl ikonra hello tovább túl**kötetek**.
* toosee egy elérhető műveletek menüjében kattintson a jobb gombbal a hello **kötetek** csomópont, vagy kattintson a jobb gombbal bármelyik hello csomópontok hello megjelenő bővített nézet.
* toosee köteteket, kattintson a **kötetek** a hello **hatókör** ablaktáblán. kötetek, és minden olyan kötetre, információ hello listája jelenik meg hello **eredmények** ablaktáblán.

### <a name="volume-groups-node"></a>Kötet csoportok csomópont
Kötet csoportok olyan néven is ismert konzisztencia csoportok. Minden kötet csoport az alkalmazással kapcsolatos kötetek készlete, amelyek segítségével tooensure alkalmazás konzisztencia biztonsági mentési művelet során. Használjon hello **kötet csoportok** csomópont tooconfigure ezeket a csoportokat és tootake interaktív biztonsági mentések, vagy hozzon létre biztonsági mentési ütemterveket. 

* tooexpand hello csomópontot, kattintson a nyíl ikonra hello tovább túl**kötet csoportok**.
* toosee egy elérhető műveletek menüjében kattintson a jobb gombbal a hello **kötet csoportok** csomópont, vagy kattintson a jobb gombbal bármelyik hello csomópontok hello megjelenő bővített nézet.
* a kötet csoportok listáját toosee kattintson **kötet csoportok** a hello **hatókör** ablaktáblán. kötet csoportok minden kötet csoportra vonatkozó információval hello listája jelenik meg hello **eredmények** ablaktáblán.

### <a name="backup-policies-node"></a>Biztonsági mentési házirendek csomópont
Biztonsági mentési házirendek nincsenek feladatok ütemezését a helyi és felhőalapú pillanatfelvételek. Használjon hello **biztonsági mentési házirendek** csomópont toospecify milyen gyakran egy biztonsági másolat készítése során, és mennyi ideig kell a biztonsági mentés őrződnek meg. 

* tooexpand hello csomópontot, kattintson a nyíl ikonra hello tovább túl**biztonsági mentési házirendek**.
* toosee egy elérhető műveletek menüjében kattintson a jobb gombbal a hello **biztonsági mentési házirendek** csomópont, vagy kattintson a jobb gombbal bármelyik hello csomópontok hello megjelenő bővített nézet.
* a biztonsági mentési házirendek listáját toosee kattintson **biztonsági mentési házirendek** hello a **hatókör** ablaktáblán. biztonsági mentési házirendek, és minden házirendekkel kapcsolatos információ hello listája jelenik meg hello **eredmények** ablaktáblán.

> [!NOTE]
> Legfeljebb 64 biztonsági másolatok továbbra is.


### <a name="backup-catalog-node"></a>Biztonsági mentési katalógus csomópont
Hello **biztonságimásolat-katalógus** csomópont helyszíni és a helyszínen biztonsági mentést Azure StorSimple-köteteket tartalmaz. Ez a csomópont kötet csoport szerint van rendezve, és minden egyes csoport kötettároló a pillanatképek helyi külön szerkezeteket tartalmaz (hello **helyi pillanatfelvétel**s csomópont) és felhőalapú pillanatfelvételek (hello **felhőalapú pillanatfelvételek** csomópont ). Ha kibontva, minden egyes csoport kötettároló összes hello sikeres biztonsági mentések interaktív módon, vagy a beállított házirend által végrehajtott sorolja fel.

* tooexpand hello csomópontot, kattintson a nyíl ikonra hello tovább túl**biztonságimásolat-katalógus**.
* toosee egy elérhető műveletek menüjében kattintson a jobb gombbal a hello **biztonságimásolat-katalógus** csomópont, vagy kattintson a jobb gombbal bármelyik hello csomópontok hello megjelenő bővített nézet.
* Kattintson a biztonsági mentési pillanatképek listája toosee **biztonságimásolat-katalógus** hello a **hatókör** ablaktáblán. pillanatképek minden pillanatkép kapcsolatos információval hello listája jelenik meg hello **eredmények** ablaktáblán.

### <a name="local-snapshots-node"></a>A pillanatképek helyi csomópont
Hello **helyi pillanatképeket** csomópont egy adott kötet csoport helyi pillanatképeket sorolja fel. hello csomópont alatt hello található **biztonságimásolat-katalógus** hello csomópontja **hatókör** ablaktáblán. A pillanatképek helyi másolatai időpontban kötet hello Azure StorSimple eszközön tárolt adatokat. Általában ilyen típusú biztonsági mentést hozható létre és gyorsan visszaállítva. Egy helyi pillanatfelvétel a helyi biztonsági másolat ugyanúgy használhatja.

* tooexpand hello csomópontot, kattintson a nyíl ikonra hello tovább túl**helyi pillanatképeket**.
* toosee egy elérhető műveletek menüjében kattintson a jobb gombbal a hello **helyi pillanatképeket** csomópont, vagy kattintson a jobb gombbal bármelyik hello csomópontok hello megjelenő bővített nézet.
* helyi pillanatképeket listája toosee kattintson **helyi pillanatképeket** a hello **hatókör** ablaktáblán. pillanatképek minden pillanatkép kapcsolatos információval hello listája jelenik meg hello **eredmények** ablaktáblán.

### <a name="cloud-snapshots-node"></a>Felhőalapú pillanatfelvételek csomópont
Hello **felhőalapú pillanatfelvételek** csomópont felhőalapú pillanatfelvételek egy adott kötet csoport sorolja fel. hello csomópont alatt hello található **biztonságimásolat-katalógus** hello csomópontja **hatókör** ablaktáblán. Felhőalapú pillanatfelvételek másolatai időpontban kötet hello felhőben tárolt adatokat. Egy felhőalapú pillanatfelvétel értéke megegyezik egy másik, a telephelytől távoli tárolórendszer replikálva tooa pillanatkép. Felhőalapú pillanatfelvételek különösen hasznosak a vész-helyreállítási eljárással.

* tooexpand hello csomópontot, kattintson a nyíl ikonra hello tovább túl**felhőalapú pillanatfelvételek**.
* toosee egy elérhető műveletek menüjében kattintson a jobb gombbal a hello **felhőalapú pillanatfelvételek** csomópont, vagy kattintson a jobb gombbal bármelyik hello csomópontok hello megjelenő bővített nézet.
* felhőalapú pillanatfelvételek listájának toosee kattintson **felhőalapú pillanatfelvételek** a hello **hatókör** ablaktáblán. pillanatképek minden pillanatkép kapcsolatos információval hello listája jelenik meg hello **eredmények** ablaktáblán.

### <a name="jobs-node"></a>Feladatok csomópont
Hello **feladatok** csomópont ütemezett, futó, és nemrég befejeződött a biztonsági mentési feladatok adatait tartalmazza. 

* tooexpand hello csomópontot, kattintson a nyíl ikonra hello tovább túl**feladatok**.
* toosee egy elérhető műveletek menüjében kattintson a jobb gombbal a hello **feladatok** csomópont, vagy kattintson a jobb gombbal bármelyik hello csomópontok hello megjelenő bővített nézet.
* ütemezett feladatok listája toosee bontsa ki a hello **feladatok** csomópontra, majd **ütemezett**. hello korábban konfigurált feladatok és minden feladat megjelennek hello **eredmények** ablaktáblán. 
* toosee nemrég befejeződött feladatokra, bontsa ki a hello **feladatok** csomópontra, majd **utolsó 24 óra**. Hello megjelenik végrehajtott hello az elmúlt 24 órában feladatokra **eredmények** ablaktáblán. Hello **eredmények** ablaktáblán minden befejezett feladat adatait is tartalmazza.
* toosee feladatokra futnak, bontsa ki a hello **feladatok** csomópontra, majd **futtató**. hello az aktuálisan futó feladatok és minden feladat információk megjelennek hello **eredmények** ablaktáblán.

## <a name="results-pane"></a>Eredmények ablaktábla
Hello **eredmények** ablaktábla hello középső ablaktáblán a StorSimple Snapshot Manager felhasználói felületén hello. Listák és a kiválasztott hello hello csomópont részletes állapotinformációkat tartalmaz **hatókör** ablaktáblán.

### <a name="example"></a>Példa
a következő példában toosee hello kattintson hello **kötet csoportok** hello csomópontja **hatókör** ablaktáblán. Hello **eredmények** ablaktáblán jelennek meg minden egyes csoport részleteivel kötet csoportok listája.

![Eredmények ablaktábla](./media/storsimple-use-snapshot-manager/HCS_SSM_Results_pane.png) 

Hello látható hello részleteit is beállíthat **eredmények** ablaktáblában: kattintson a jobb gombbal hello **hatókör** ablaktáblán kattintson a **nézet**, és kattintson a **hozzáadása Oszlopok**.

## <a name="actions-pane"></a>Műveletek panel
Hello **műveletek** ablaktábla hello hello StorSimple Snapshot Manager felhasználói felületén a jobb oldali ablaktáblában. Hello csomópont, nézet vagy a hello adat elvégezhető műveletek menü tartalmaz **hatókör** ablaktáblán vagy **eredmények** ablaktáblán. Hello **műveletek** ablaktábla tartalmaz ugyanaz, mint hello parancsok hello **művelet** hello elemeinek elérhető menük **hatókör** ablaktáblán és **eredmények**ablaktáblán. Minden művelet leírását lásd: hello hello táblájában **művelet** menü szakasz.

### <a name="examples"></a>Példák
a következő példában a hello toosee hello **hatókör** ablaktáblában bontsa ki a hello **feladatok** csomópontot, és kattintson **ütemezett**. Hello **műveletek** ablaktáblán megjelennek azok hello elérhető műveletek a hello **ütemezett** csomópont.

![Példa a műveletek ablaktáblán ütemezett feladatok](./media/storsimple-use-snapshot-manager/HCS_SSM_ActionsPane.png) 

toosee több lehetőségei, a hello **hatókör** ablaktáblán bontsa ki a hello **feladatok** csomópontot, kattintson **ütemezett**, majd egy ütemezett feladatot a hello **eredmények**ablaktáblán. Hello **műveletek** ablaktáblán megjelennek azok hello elérhető műveletek hello ütemezett feladat, ahogy az alábbi példa hello.

![Példa a műveletek ablaktáblán Feladatműveletek](./media/storsimple-use-snapshot-manager/HCS_SSM_ActionsPane_Results.png)

## <a name="keyboard-navigation-and-shortcuts"></a>Billentyűparancsokkal és parancsikonok
StorSimple Snapshot Manager lehetővé teszi, hogy a hello kisegítő lehetőségei hello Windows operációs rendszer és a Microsoft Management Console (MMC) hello. Emellett néhány billentyűzet navigációs szolgáltatások és arra, hogy adott toohello StorSimple Snapshot Manager hello a következő részekben leírtak szerint.

* [Navigációs billentyűk](#keyboard-navigation-keys) 
* [A gyorsbillentyűk menüsoron](#menu-bar-shortcut-keys) 
* [Hatókör ablaktábla billentyűparancsok](#scope-pane-shortcut-keys) 

### <a name="keyboard-navigation-keys"></a>Navigációs billentyűk
hello következő táblázatban hello kulcsok használható toonavigate hello StorSimple Snapshot Manager felhasználói felületén. 

| Navigációs kulcs | Műveletek |
|:--- |:--- |
| Le nyílbillentyű |Használja a lefelé mutató nyíl kulcs toomove hello függőleges toohello a következő elem a menü vagy ablaktáblán. |
| Adja meg |Nyomja le az ENTER hello Enter kulcs toocomplete egy műveletet, és folytathatja a toohello következő lépésre. Például lenyomja az Enter tooselect **következő**, **OK**, vagy **létrehozása**, és folytassa a toohello a következő lépés a varázslóban. |
| ESC |Nyomja le az ENTER hello Esc kulcs tooclose menü vagy toocancel, és zárja be a lapot. |
| F1 |Nyomja le az hello F1 kulcs tooview súgótémakör hello jelenleg aktív ablak. |
| F5 |Nyomja le az hello F5 kulcs toorefresh egy csomópont. |
| F6 |Nyomja le az ENTER hello F6 kulcs toomove a hello **hatókör** ablaktábla toohello **eredmények** ablaktáblán. |
| F10 |Nyomja le az hello F10 kulcs toogo toohello menüsávjában. |
| Bal nyílbillentyű |Balra nyíl kulcs toomove vízszintesen egy sáv beállítás toohello előző menüpont a hello használata. Előző toohello áthelyezésekor hello sáv, hello művelet (vagy a környezetben) menüjében hello előző elem jelenik meg. |
| Jobb nyílbillentyű |Ezután használja hello jobbra mutató nyílra kulcs toomove vízszintesen eszközterület-beállítás toohello egy menüből. Áthelyezésekor toohello tovább elem hello sáv, hello művelet (vagy a környezetben) menü hello új elem jelenik meg. |
| TAB billentyűt |Használjon hello lapon kulcs toomove toohello következő ablaktábla hello konzol vagy toohello következő kiválasztása vagy a beviteli mezőben lapon. |
| Fel nyílbillentyű |Használja a felfelé mutató nyíl kulcs toomove hello függőleges toohello előző elem menün vagy ablaktáblán. |

### <a name="menu-bar-shortcut-keys"></a>A gyorsbillentyűk menüsoron
hello következő táblázatban hello billentyűparancsokat hello menü sáv. Hello billentyűparancsok nyomja le az ENTER és hello menü megnyitása után (hello hello menü aláhúzott kulcsok) menü billentyűparancsok is használhatja. Hello menüsáv kapcsolatos további információkért lépjen túl[menüsáv](#menu-bar).

| Helyi | eredménye | Menü billentyűparancs | eredménye |
|:--- |:--- |:--- |:--- |
| ALT + F |Megnyílik hello **fájl** menü. |N |Megnyílik egy új konzolt példányt. |
|  |O |Megnyílik hello **felügyeleti eszközök** lap. | |
|  |S |Menti a hello StorSimple Snapshot Manager konzolon. | |
|  |A |Megnyílik hello **Mentés másként** lap. | |
|  |M |Megnyílik hello **beépülő modul hozzáadása/eltávolítása** lap. | |
|  |P |Megnyílik hello **beállítások** lap. | |
|  |H |Online súgójának megnyitása. | |
| ALT + A |Megnyílik hello **művelet** menü. |I |Hello importálási megjelenítési beállítás be- és kikapcsolása. |
|  |W |Megnyit egy új StorSimple Snapshot Manager konzolt. | |
|  |F |Frissítések hello StorSimple Snapshot Manager konzolon. | |
|  |L |Megnyílik hello **lista exportálása** lap. | |
|  |H |Online súgójának megnyitása. | |
| ALT + V |Megnyílik hello **nézet** menü. |A |Megnyílik hello **oszlopok hozzáadása/eltávolítása** lap. |
|  |U |Megnyílik hello **nézet testreszabása** lap. | |
| ALT + O |Megnyílik hello **Kedvencek** menü. |A |Megnyílik hello **tooFavorites hozzáadása** lap. |
|  |O |Megnyílik hello **Kedvencek rendezése** lap. | |
| ALT + W |Megnyílik hello **ablak** menü. |N |StorSimple Snapshot Manager egy másik ablak megnyitása. |
|  |C# |Nyissa meg a konzolt az összes windows kaszkádolt stílus jeleníti meg. | |
|  |T |A rács minta minden megnyitott konzol windows jeleníti meg. | |
|  |I |A képernyő alsó részén hello vízszintes egymás ikonok elrendezése. | |
| ALT + H |Megnyílik hello **súgó** menü. |H |Online súgójának megnyitása. |
|  |T |A Microsoft TechNet technológia Center weblap hello megnyitása. | |
|  |A |Megnyílik hello **kapcsolatban a Microsoft Management Console** lap. | |

### <a name="scope-pane-shortcut-keys"></a>Hatókör ablaktábla billentyűparancsok
hello alábbi táblázatokban hello helyi billentyűkombinációinak az egyes csomópontok a hello **hatókör** ablaktáblán. 

* [Eszközök csomópont billentyűparancsok](#devices-node-shortcut-keys)
* [Kötetek csomópont billentyűparancsok](#volumes-node-shortcut-keys)
* [Kötet csoportok csomópont billentyűparancsok](#volume-groups-node-shortcut-keys)
* [Biztonsági mentési házirendek csomópont billentyűparancsok](#backup-policies-node-shortcut-keys)
* [Biztonsági mentési katalógus csomópont billentyűparancsok](#backup-catalog-node-shortcut-keys)
* [Feladatok csomópont billentyűparancsok](#jobs-node-shortcut-keys)

#### <a name="devices-node-shortcut-keys"></a>Eszközök csomópont billentyűparancsok
| Helyi menü | eredménye |
|:--- |:--- |
| C# |Megnyílik hello **eszköz konfigurálása** lap. |
| D |Eszközök és eszközadatok hello listájának frissítése. |
| V |Megnyílik hello **nézet** menü. |
| W |Megnyílik egy új StorSimple Snapshot Manager-konzol összpontosított hello **részletek** csomópont. |
| F |Frissítések hello StorSimple Snapshot Manager konzolon. |
| L |Megnyílik hello **lista exportálása** lap. |
| H |Online súgójának megnyitása. |

#### <a name="volumes-node-shortcut-keys"></a>Kötetek csomópont billentyűparancsok
| Helyi menü | eredménye |
|:--- |:--- |
| V |Kötetek frissítések hello listáját. |
| V (nyomja le az ENTER kétszer) |Megnyílik hello **nézet** menü. |
| W |Megnyílik egy új StorSimple Snapshot Manager-konzol összpontosított hello **kötetek** csomópont. |
| F |Frissítések hello StorSimple Snapshot Manager konzolon. |
| L |Megnyílik hello **lista exportálása** lap. |
| H |Online súgójának megnyitása. |

#### <a name="volume-groups-node-shortcut-keys"></a>Kötet csoportok csomópont billentyűparancsok
| Helyi menü | eredménye |
|:--- |:--- |
| G |Megnyílik hello **hozzon létre egy kötet csoportot** lap. |
| V |Megnyílik hello **nézet** menü. |
| W |Megnyílik egy új StorSimple Snapshot Manager-konzol összpontosított hello **kötet csoportok** csomópont. |
| F |Frissítések hello StorSimple Snapshot Manager konzolon. |
| L |Megnyílik hello **lista exportálása** lap. |
| H |Online súgójának megnyitása. |

#### <a name="backup-policies-node-shortcut-keys"></a>Biztonsági mentési házirendek csomópont billentyűparancsok
| Helyi menü | eredménye |
|:--- |:--- |
| B |Megnyílik hello **házirend létrehozása** lap. |
| V |Megnyílik hello **nézet** menü. |
| W |Megnyílik egy új StorSimple Snapshot Manager-konzol összpontosított hello **kötet csoportok** csomópont. |
| F |Frissítések hello StorSimple Snapshot Manager konzolon. |
| L |Megnyílik hello ** lista exportálása ** lap. |
| H |Online súgójának megnyitása. |

#### <a name="backup-catalog-node-shortcut-keys"></a>Biztonsági mentési katalógus csomópont billentyűparancsok
| Helyi menü | eredménye |
|:--- |:--- |
| W |Megnyílik egy új StorSimple Snapshot Manager-konzol összpontosított hello **kötet csoportok** csomópont. |
| F |Frissítések hello StorSimple Snapshot Manager konzolon. |
| H |Online súgójának megnyitása. |

#### <a name="jobs-node-shortcut-keys"></a>Feladatok csomópont billentyűparancsok
| Helyi menü | eredménye |
|:--- |:--- |
| V |Megnyílik hello **nézet** menü. |
| W |Megnyílik egy új StorSimple Snapshot Manager-konzol összpontosított hello **feladatok** csomópont. |
| F |Frissítések hello StorSimple Snapshot Manager konzolon. |
| L |Megnyílik hello **lista exportálása** lap. |
| H |Online súgójának megnyitása |

## <a name="next-steps"></a>Következő lépések
* Ismerje meg, hogyan túl[használja a StorSimple Snapshot Manager tooadminister a StorSimple megoldásban](storsimple-snapshot-manager-admin.md).
* Ismerje meg, hogyan túl[StorSimple Snapshot Manager tooconnect használja, és az eszközök kezeléséhez](storsimple-snapshot-manager-manage-devices.md).

