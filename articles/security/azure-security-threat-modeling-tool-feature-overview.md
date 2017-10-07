---
title: "Azure fenyegetések modellezése eszköz - aaaMicrosoft |} Microsoft Docs"
description: "A fenyegetések modellezése eszköz hello elérhető hello szolgáltatásainak megismerése"
services: security
documentationcenter: na
author: RodSan
manager: RodSan
editor: RodSan
ms.assetid: na
ms.service: security
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/17/2017
ms.author: rodsan
ms.openlocfilehash: f9ad5e623e7758063084cb7fc723c5735161a846
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="threat-modeling-tool-feature-overview"></a>Fenyegetések modellezése eszköz funkcióinak áttekintése

Örülünk a modellezési igényeinek fenyegetés toouse hello fenyegetések modellezése eszköz választott! Ha még nem tette meg, keresse fel a  **[Ismerkedés a fenyegetések modellezése eszköz hello](./azure-security-threat-modeling-tool-getting-started.md)**  toolearn hello alapjait.

> Az eszköz gyakran frissül, ezért ellenőrizze ezt gyakran toosee útmutató a legújabb szolgáltatásait és fejlesztéseit.

Hello "Hozzon létre egy új modell" gombra kattintva megnyílik egy üres kezdőlapot, hasonló toohello kép alatt:

![Üres Start lap](./media/azure-security-threat-modeling-tool/tmtstart.png)

Hello fenyegetések modellezése használatával hozta létre a csapat a hello  **[bevezetés](./azure-security-threat-modeling-tool-getting-started.md)**  példában most kivétele ma összes hello szolgáltatásához hello eszköz.

![Alapszintű fenyegetések modellezése](./media/azure-security-threat-modeling-tool/basictmt.png)

## <a name="navigation"></a>Navigációs

Hello beépített szolgáltatásai ról, mielőtt ugorjunk keresztül hello fő összetevőkben hello eszközben található

### <a name="menu-items"></a>Menüelemek

hello élmény hasonló tooother Microsoft-termékek kell lennie. Hozzuk, az hello legfelső szintű menüelemek keresztül:

![Menüelemek](./media/azure-security-threat-modeling-tool/menuitems.png)

| Címke                               | Részletek      |
| --------------------------------------- | ------------ |
| **Fájl** | <ul><li>Nyissa meg, mentse és zárja be a fájlok</li><li>OneDrive In/Out bejelentkezési fiókok</li><li>Hivatkozásaival (nézet + Szerkesztés)</li><li>Fájl adatainak megtekintése</li><li>Új sablon tooExisting modellek alkalmazása</li></ul> |
| **Szerkesztése** | Visszavonási vagy visszaállítási műveletek, valamint egy másolatát, Beillesztés és törlése |
| **Nézet** | <ul><li>Váltás a **elemzés** és **tervezési** nézetek</li><li>Nyissa meg a lezárt windows (e.g.stencils, tulajdonságokhoz és üzenetek)</li><li>Elrendezés toodefault beállításainak alaphelyzetbe állítása</li></ul> |
| **Diagram** | Diagramok hozzáadása vagy törlése és a "lap" diagramjainak navigálnak |
| **Jelentések** | HTML-jelentésekben tooshare létrehozása |
| **segítség** | Útmutatók toohelp hello eszközzel |

hello ikonok hello legfelső szintű menük billentyűparancsainak is:

| Ikon                               | Részletek      |
| --------------------------------------- | ------------ |
| **Nyissa meg** | Megnyit egy új fájlt |
| **Mentése** | Menti az aktuális fájl |
| **Tervezési** | Tervező nézetben állapotba kerül, ahol modellek létrehozása |
| **Elemzés** | Azt mutatja be generált fenyegetések és azok tulajdonságait |
| **Diagram hozzáadása** | Új diagram (hasonló toonew lapok az Excelben) hozzáadása |
| **Diagram törlése** | Törli a jelenlegi diagramhoz |
| **Másolás, Kivágás és beillesztési** | Másolatot/darabok/illeszti elemei |
| **Visszavonási vagy visszaállítási** | Műveletek visszavonja/megismétlése |
| **Nagyítás / kicsinyítés** | Mindkét hello diagram jobb nagyít |
| **Visszajelzés** | Megnyitja a hello MSDN fórum |

### <a name="canvas"></a>Vászonra

hello adhatja meg, ahol Ön áthúzása az elemeket. Fogd és vidd módszer hello leggyorsabb és leghatékonyabb módon toobuild modellek. Előfordulhat, hogy kattintson a jobb gombbal, és válassza hello menüpontot, amely hello elemek használata esetén általános verziói hozzáadja az alább látható módon.

#### <a name="dropping-hello-stencil-on-hello-canvas"></a>Hello rajzsablon eldobása hello vászonra

![Vászonra eldobási](./media/azure-security-threat-modeling-tool/canvasdrop1.png)

#### <a name="clicking-on-hello-stencil"></a>Kattintson a hello rajzsablon

![Elem tulajdonságai](./media/azure-security-threat-modeling-tool/canvasdrop2.png)

### <a name="stencils"></a>Rajzsablonok

Ha megtalálja az összes rajzsablonok elérhető toouse hello kiválasztott sablon alapján. Ha hello megfelelő elemek nem talál meg, próbálkozzon egy másik sablonnal, vagy módosítsa egy toofit igényeinek. Általában kell tudni toofind például hello néhányat a meglévők közül alábbi kategóriák kombinációja:

| Rajzsablon neve                               | Részletek      |
| --------------------------------------- | ------------ |
| **Folyamat** | Alkalmazások, a böngésző beépülő modulok, szálak, virtuális gépek |
| **Külső interaktor** | Hitelesítésszolgáltatók és a böngészők felhasználói, a webes alkalmazások |
| **Adattár** | Gyorsítótár, tárolási, konfigurációs fájlok, adatbázisok, beállításjegyzék |
| **Adatfolyam** | Bináris, ALPC, HTTP, HTTPS vagy a TLS/SSL, IOCTL, IPSec, Named Pipe, RPC/DCOM, SMB, UDP |
| **Szegély/határ megbízhatóság** | Vállalati hálózatokhoz, a Internet, a gép, a védőfal, a felhasználó és a Kernel mód |

### <a name="notesmessages"></a>Megjegyzések/üzenetek

| Összetevő                               | Részletek      |
| --------------------------------------- | ------------ |
| **Üzenetek** | Belső eszköz logika, amely értesíti a felhasználókat, ha nem sikerül, például az adatok elemei között zajló kommunikációról |
| **Megjegyzések** | Manuális megjegyzések mérnöki csapat által hozzáadott toohello fájl globálisan hello tervezési és a folyamat áttekintése |

### <a name="element-properties"></a>Elem tulajdonságai

Ezek által kiválasztott hello elemek eltérőek lehetnek. Megbízhatósági határait kívül minden más elemet tartalmazhat 3 általános beállításokat:

| Elem tulajdonság                               | Részletek      |
| --------------------------------------- | ------------ |
| **Name (Név)** | Hasznos, ha a folyamatok, tárolók, interactors és adatfolyamok toobe könnyen felismerhető elnevezése |
| **Hatókörén kívül** | Választásakor hello elem használatban van-e kívül hello fenyegetés generációs mátrix (nem ajánlott) |
| **Ezért a hatókörén kívül** | Indoklás mező toolet felhasználókkal, hogy része volt jelölve |

Tulajdonságok elem kategóriákban módosult. Kattintson az egyes elem tooinspect hello rendelkezésre álló lehetőségeket, vagy nyissa meg a további hello sablon toolearn. Folytassuk hello funkciók be.

## <a name="welcome-screen"></a>Üdvözlőképernyő

hello üdvözlőképernyő hello elsőként hello alkalmazás megnyitásakor látni.

### <a name="open-a-model"></a>Nyissa meg A modell

"Nyissa meg a modell" gomb fölött látható 2 rejtett beállítások: "Nyissa meg a számítógépen" és "Nyissa meg a onedrive-ról." hello először nyitja meg a fájl megnyitása üdvözlő képernyőt, amíg a hello második végigvezeti hello bejelentkezési folyamat során a onedrive vállalati verzióhoz, így toopick mappákról és fájlokról, a sikeres hitelesítés után.

![Modell megnyitása](./media/azure-security-threat-modeling-tool/openmodel.png)

![Nyissa meg a számítógép vagy a onedrive vállalati verzió](./media/azure-security-threat-modeling-tool/openmodel2.png)

### <a name="feedback-suggestions-and-issues"></a>Visszajelzés, a javaslatok és a problémák

Ezzel a beállítással léphet vissza toohello MSDN fórumain SDL eszközök. A kiváló módja toocheck kimenő mi mások véleményét az hello eszközt, beleértve a lehetséges megoldások és új ötleteket is.

![Visszajelzés](./media/azure-security-threat-modeling-tool/feedback.png)

## <a name="design-view"></a>Tervező nézetben

Amikor megnyitásához, vagy hozzon létre egy új modell, akkor megnyílik toohello tervezési nézetben.

### <a name="adding-elements"></a>Elemek hozzáadása

Módon 2 tooadd elemek hello rácson:

- **Adatértékmezők áthúzása** – hello kívánt elem toohello rács húzza hello elem tulajdonságai tooprovide további információt.
- **Kattintson a jobb gombbal** – bármely részén kattintson a jobb gombbal hello rács és hello legördülő menüből válassza ki. Egy adott elem általános ábrázolása üdvözlő képernyőt fog megjelenni.

### <a name="connecting-elements"></a>Kapcsolódó elemek

Módon 2 tooconnect elemek hello eszközben:

- **Adatértékmezők áthúzása** – hello kívánt adatfolyamblokk toohello rács húzza, és csatlakoztassa mindkét ends toohello megfelelő elemeket.
- **Kattintson a + Shift** – hello első eleme (adatok küldése) parancsra, tartsa lenyomva a hello Shift billentyűt, majd jelölje be hello második elemének (az adatok fogadása). Kattintson a jobb gombbal, és kattintson a "Csatlakozás". Ha egy kétirányú adatfolyam használata esetén hello sorrendje nem olyan fontos.

### <a name="properties"></a>Tulajdonságok

Összes hello tulajdonság módosítható hello rajzsablonok helyezett hello ábra mutatja be. toosee hello tulajdonságait, kattintson hello rajzsablonon, és hello információ ennek megfelelően tölti fel. hello az alábbi példában előtt és után egy "adatbázis" rajzsablon van húzott hello diagramja:

#### <a name="before"></a>Előtt

![Előtt](./media/azure-security-threat-modeling-tool/properties1.png)

#### <a name="after"></a>Után

![Után](./media/azure-security-threat-modeling-tool/properties2.png)

### <a name="messages"></a>Üzenetek

Ha a fenyegetések modellezése elfelejti tooconnect adatáramlás tooelements, hello üzenetablakban értesítést küld, tooact. Kiválaszthatja a tooignore, vagy hajtsa végre hello utasításokat toofix hello probléma. 

![Üzenetek](./media/azure-security-threat-modeling-tool/messages.png)

### <a name="notes"></a>Megjegyzések

Váltás a lapok üzenetek tooNotes lehetővé teszi, hogy Ön tooadd megjegyzések tooyour diagram toocapture a gondolatait

## <a name="analysis-view"></a>Elemző nézet

Miután befejezte a diagram felépítése, kapcsoló tooanalysis nézet keresztül fog toohello felső menüjében beállításokat, majd válassza a hello nagyítóüveg következő toohello paint paletta.

![Elemző nézet](./media/azure-security-threat-modeling-tool/analysisview.png)

### <a name="generated-threat-selection"></a>Generált fenyegetés kiválasztása

Amikor fenyegetést kattint, kihasználhatják a három egyedi funkciókat:

| Szolgáltatás                               | Információ      |
| --------------------------------------- | ------------ |
| **Olvasási kijelző** | <p>Fenyegetés most van megjelölve, olvassa el, amely segítségével könnyen nyomon követjük, hello elemek már a végrehajtás során</p><p>![Olvasás/olvasatlan kijelző](./media/azure-security-threat-modeling-tool/readmode.png)</p> |
| **Interakció fókusz** | <p>Hello diagram toothat fenyegetés tartozó kapcsolati ki van jelölve.</p><p>![Interakció fókusz](./media/azure-security-threat-modeling-tool/interactionfocus.png)</p> |
| **Fenyegetés-tulajdonságok** | <p>Hello fenyegetés további információt a telepítéskor hello fenyegetés tulajdonságai ablakban</p><p>![Fenyegetés-tulajdonságok](./media/azure-security-threat-modeling-tool/threatproperties.png)</p> |

### <a name="priority-change"></a>Prioritás módosítása

Minden létrehozott fenyegetettség hello prioritási szintet is módosítása a színek toomake azt könnyen tooidentify magas, közepes és alacsony prioritás fenyegetéseket.

![Prioritás módosítása](./media/azure-security-threat-modeling-tool/prioritychange.png)

### <a name="threat-properties-editable-fields"></a>Fenyegetés-tulajdonságok szerkeszthető mezők

Hello a fenti kép látható, a felhasználók módosíthatják-e a hello eszköz által generált hello adatokat egy toocertain adatmezőket, indoklás is hozzáadhat. Ezek a mezők hello sablon által előállított, ha további információt szeretne az egyes fenyegetésekre, tehát javasolt toomake módosítások.

![Fenyegetés-tulajdonságok](./media/azure-security-threat-modeling-tool/threatproperties.png)

## <a name="reports"></a>Jelentések

Miután elkészült változó prioritások és frissítési hello állapotban vannak az egyes létrehozott fenyegetés, hello fájl mentéséhez, illetve nyomtassa ki a jelentés túl "jelentés" állapotra és majd "teljes jelentés létrehozása." Meg kell adnia tooname hello jelentést, és ha így tesz, megjelenítheti toohello valami hasonló, az alábbi képen:

![Jelentés](./media/azure-security-threat-modeling-tool/report.png)

## <a name="next-steps"></a>Következő lépések

hello közösségi sablonját toocontribute lépjen tooour  **[GitHub](https://github.com/Microsoft/threat-modeling-templates)**  lap. **[Töltse le](https://aka.ms/tmtpreview)**  hello eszköz tooget neki még ma.
