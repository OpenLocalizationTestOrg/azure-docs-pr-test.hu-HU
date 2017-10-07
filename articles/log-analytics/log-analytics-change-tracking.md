---
title: "az Azure Naplóelemzés aaaTrack módosítások |} Microsoft Docs"
description: "hello Naplóelemzési változáskövetési megoldás könnyebb legyen azonosítani a szoftver- és Windows-szolgáltatás módosításait a környezetében bekövetkező."
services: log-analytics
documentationcenter: 
author: bandersmsft
manager: carmonm
editor: 
ms.assetid: f8040d5d-3c89-4f0c-8520-751c00251cb7
ms.service: log-analytics
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/11/2017
ms.author: banders
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 2bb1938caad25101e167927200ac3ef495120fe0
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="track-software-changes-in-your-environment-with-hello-change-tracking-solution"></a>A környezetben, a változáskövetési megoldás hello szoftver változásainak követése

![Nyomkövetési szimbólum módosítása](./media/log-analytics-change-tracking/change-tracking-symbol.png)

Ez a cikk segít használata hello Naplóelemzési tooeasily a változáskövetési megoldás – a változásokat a környezetben. hello megoldás módosítások tooWindows és Linux-szoftver, Windows-fájlok és beállításkulcsok, központi Windows-szolgáltatások és Linux-démonok követi nyomon. Konfigurációs változásokat segíthetnek a rögzítési ponthoz működési problémák.

Hello megoldás tooupdate hello típusa telepített ügynök telepítését. Módosítások tooinstalled szoftver, a központi Windows-szolgáltatások és a Linux-démonok hello figyelt kiszolgálók olvasható. Ezt követően hello adatküldést toohello Naplóelemzés szolgáltatás hello felhő feldolgozásra. Logikai alkalmazott toohello fogadott adatok és hello felhőszolgáltatás hello adatait rögzíti. Hello információk hello változások követése Irányítópult segítségével egyszerűen meg lehet hello a kiszolgálói infrastruktúra végzett módosításokat.

## <a name="installing-and-configuring-hello-solution"></a>Telepítése és konfigurálása hello megoldás
A következő információk tooinstall hello használja, és hello megoldás konfigurálása.

* Rendelkeznie kell egy [Windows](log-analytics-windows-agents.md), [Operations Manager](log-analytics-om-agents.md), vagy [Linux](log-analytics-linux-agents.md) ügynököt minden olyan számítógépen, ahol azt szeretné, hogy toomonitor módosítások.
* Hello változáskövetési megoldás tooyour OMS-munkaterület felvétele hello [Azure piactér](https://azuremarketplace.microsoft.com/marketplace/apps/Microsoft.ChangeTrackingOMS?tab=Overview). Vagy hello megoldás hello témakörben található információk alapján is hozzáadhat [hozzáadni a Naplóelemzési megoldásokat az hello megoldások gyűjtemény](log-analytics-add-solutions.md). További beállításokra nincs is szükség.

### <a name="configure-linux-files-tootrack"></a>Linux-fájlok tootrack konfigurálása
Hello követő lépéseket tooconfigure fájlok tootrack a Linux rendszerű számítógépeken használható.

1. Az OMS-portálon hello kattintson **beállítások** (hello fogaskerék jel).
2. A hello **beállítások** kattintson **adatok**, és kattintson a **Linux fájl követése**.
3. A Linux fájl változások követése, írja be a hello teljes elérési útja, beleértve hello fájl tootrack szeretne, és kattintson a hello hello neve **Hozzáadás** szimbólum. Például: "/etc/*.conf"
4. Kattintson a **Save** (Mentés) gombra.  

> [!NOTE]
> Linux fájl nyomkövetési könyvtár nyomon követése, könyvtárak, és nyomon követése helyettesítő recrusion beleértve további képességekkel rendelkezik.

### <a name="configure-windows-files-tootrack"></a>Windows-fájlok tootrack konfigurálása
A következő lépéseket tooconfigure használata hello tootrack fájlok a Windows rendszerű számítógépeken.

1. Az OMS-portálon hello kattintson **beállítások** (hello fogaskerék jel).
2. A hello **beállítások** kattintson **adatok**, és kattintson a **Windows fájl követése**.
3. A Windows fájl változások követése, írja be a hello teljes elérési útja, beleértve hello fájl tootrack szeretne, és kattintson a hello hello neve **Hozzáadás** szimbólum. Például: C:\Program Files (x86) \Internet Explorer\iexplore.exe vagy C:\Windows\System32\drivers\etc\hosts.
4. Kattintson a **Save** (Mentés) gombra.  
   ![Windows-fájl változáskövetési](./media/log-analytics-change-tracking/windows-file-change-tracking.png)

### <a name="configure-windows-registry-keys-tootrack"></a>A Windows beállításjegyzék kulcsok tootrack konfigurálása
Hello követő lépéseket tooconfigure beállításjegyzék kulcsok tootrack Windows rendszerű számítógépeken használható.

1. Az OMS-portálon hello kattintson **beállítások** (hello fogaskerék jel).
2. A hello **beállítások** kattintson **adatok**, és kattintson a **Windows beállításjegyzék követési**.
3. A Windows beállításjegyzék változások követése, írja be a teljes kulcsot hello tootrack szeretne, és kattintson a hello **Hozzáadás** szimbólum.
4. Kattintson a **Save** (Mentés) gombra.  
   ![A Windows beállításjegyzék változások követése](./media/log-analytics-change-tracking/windows-registry-change-tracking.png)

### <a name="explanation-of-linux-file-collection-properties"></a>Linux-fájl tulajdonságait ismertetése
1. **Típus**
   * **Fájl** (jelentést fájl metaadat - mérete, a módosítás dátuma, a kivonatoló, stb.)
   * **Directory** (jelentés directory metaadat - mérete, a módosítás dátuma, stb.)
2. **Hivatkozások** (kezelése Linux symlink hivatkozik tooother fájlok vagy könyvtárak)
   * **Figyelmen kívül hagyása** (recurions toonot során figyelmen kívül hagyása symlinks között hello fájlok vagy könyvtárak hivatkozott)
   * **Hajtsa végre a** (kövesse hello symlinks során rekurzió tooalso között hello fájlok vagy könyvtárak hivatkozott)
   * **Kezelése** (hello symlinks kövesse, és módosítsa a visszaadott tartalom hello kezelése)

   > [!NOTE]   
   > hello "Kezelése" hivatkozást beállítás használata nem ajánlott. A fájl tartalma nem támogatott.

3. **Recurse** (Recurse keresztül mappa szintek és nyomon követheti a hello elérési utat értekezlet minden fájl)
4. **Sudo** (fájlok elérése vagy a sudo jogosultságot igénylő könyvtárak engedélyezése)

### <a name="limitations"></a>Korlátozások
hello változáskövetési megoldás jelenleg nem támogatja a következő elemek hello:

* Mappák (könyvtárak) Windows fájl nyomon követése
* A rekurzió Windows fájl nyomon követése
* Helyettesítő karakterek Windows fájl nyomon követése
* Elérésiút-változók
* Hálózati fájlrendszer
* A fájl

Egyéb korlátozások is érvényesek:

* Hello **maximális fájlméret** oszlop és értékek nem használt hello aktuális megvalósításában.
* Ha több mint 2500-fájlok gyűjtése a hello 30 perces adatgyűjtési ciklus, megoldás teljesítménye csökkent.
* Ha nagy hálózati forgalmat módosítás rekordok is tarthat tooa legfeljebb hat órán keresztül toodisplay.
* Hello konfigurációs akkor módosítható, amíg a számítógép leállítása, ha a hello számítógép utáni előfordulhat, hogy tartoztak toohello előző konfigurációs fájl módosításokat.

## <a name="change-tracking-data-collection-details"></a>Nyomkövetési adatok gyűjtése adatainak módosítása
Változáskövetés gyűjti a szoftverleltár- és Windows-szolgáltatás metaadatok hello ügynökök, amelyeken engedélyezve.

hello következő táblázatban adatgyűjtési módszerek és egyéb hogyan adatok gyűjtése változáskövetési adatait.

| Platform | Közvetlen ügynök | Operations Manager-ügynök | Linux-ügynök | Azure Storage | Az Operations Manager szükséges? | Az Operations Manager ügynök adatait a felügyeleti csoport keresztül küldött | Gyűjtemény gyakorisága |
| --- | --- | --- | --- | --- | --- | --- | --- |
| A Windows és Linux | &#8226; | &#8226; | &#8226; |  |  | &#8226; | 5 perc too50 perc, attól függően, hogy hello típusának módosítása. Tekintse meg a következő táblázat további információt hello. |


hello következő táblázatban hello adatok gyűjtési gyakoriságát hello típusú változtatásokat.

| **típusának módosítása** | **gyakoriság** | **Does****ügynök****küldése, ha eltérés?**  |
| --- | --- | --- |
| A Windows beállításjegyzékben | 50 perc | Nem |
| Windows-fájl | 30 perc | Igen. Ha nincs változás 24 órában, pillanatképet küld el. |
| Linux-fájl | 15 perc | Igen. Ha nincs változás 24 órában, pillanatképet küld el. |
| Windows-szolgáltatások | 30 perc | Igen, ha módosítások találhatók 30 percenként. 24 óránként pillanatképet küld, függetlenül attól, módosítsa. Igen, hello pillanatkép még küldött amennyiben nem módosult. |
| Linux-démonok | 5 perc | Igen. Ha nincs változás 24 órában, pillanatképet küld el. |
| A Windows szoftverek | 30 perc | Igen, ha módosítások találhatók 30 percenként. 24 óránként pillanatképet küld, függetlenül attól, módosítsa. Igen, hello pillanatkép még küldött amennyiben nem módosult. |
| Linux-szoftver | 5 perc | Igen. Ha nincs változás 24 órában, pillanatképet küld el. |

### <a name="registry-key-change-tracking"></a>Beállításjegyzék-kulcs módosítás nyomon követése

A Naplóelemzési Windows beállításjegyzék figyelését és követését a változáskövetési megoldás hello hajt végre. hello módosítások tooregistry kulcsok figyelése célja toopinpoint bővítési pontokat, ahol külső kód és a kártevők is aktiválhatja. hello következő lista megjelenítése hello alapértelmezett beállításkulcsok hello megoldás követi, és miért egyes követhető nyomon.

- HKEY\_helyi\_MACHINE\Software\Microsoft\Windows\CurrentVersion\Group Policy\Scripts\Startup
    - Figyelők parancsfájlok, amelyek indítási parancsot.
- HKEY\_helyi\_MACHINE\Software\Microsoft\Windows\CurrentVersion\Group Policy\Scripts\Shutdown
    - Figyelők futtatott parancsfájlok, leállításkor.
- HKEY\_helyi\_MACHINE\SOFTWARE\Wow6432Node\Microsoft\Windows\CurrentVersion\Run
    - Mielőtt hello felhasználó bejelentkezik a Windows-fiók tootheir betöltött kulcsokat figyeli. hello kulcs szolgál 64 bites számítógépeken futó 32 bites program.
- HKEY\_helyi\_MACHINE\SOFTWARE\Microsoft\Active Setup\Installed összetevők
    - Figyelők tooapplication beállításokat módosítja.
- HKEY\_helyi\_MACHINE\Software\Classes\Directory\ShellEx\ContextMenuHandlers
    - Figyelők közös autostart bejegyzéseit, amelyek közvetlenül a Windows Intézőt, és általában futtatási-folyamat az Explorer.exe környezet igénybe vételét.
- HKEY\_helyi\_MACHINE\Software\Classes\Directory\Shellex\CopyHookHandlers
    - Figyelők közös autostart bejegyzéseit, amelyek közvetlenül a Windows Intézőt, és általában futtatási-folyamat az Explorer.exe környezet igénybe vételét.
- HKEY\_helyi\_MACHINE\Software\Classes\Directory\Background\ShellEx\ContextMenuHandlers
    - Figyelők közös autostart bejegyzéseit, amelyek közvetlenül a Windows Intézőt, és általában futtatási-folyamat az Explorer.exe környezet igénybe vételét.
- HKEY\_helyi\_MACHINE\Software\Microsoft\Windows\CurrentVersion\Explorer\ShellIconOverlayIdentifiers
    - Figyeli a ikon kezelő regisztrációs átfedő.
- HKEY\_helyi\_MACHINE\Software\Wow6432Node\Microsoft\Windows\CurrentVersion\Explorer\ShellIconOverlayIdentifiers
    - Figyeli a ikon átfedő 64 bites számítógépeken futó 32 bites program kezelő regisztrálása.
- HKEY\_helyi\_MACHINE\Software\Microsoft\Windows\CurrentVersion\Explorer\Browser segédobjektuma
    - Új böngésző segítő objektum beépülők az Internet Explorer figyeli. Használt tooaccess hello Document Object Model (DOM) hello aktuális lap és toocontrol navigációs.
- HKEY\_helyi\_MACHINE\Software\Wow6432Node\Microsoft\Windows\CurrentVersion\Explorer\Browser segédobjektuma
    - Új böngésző segítő objektum beépülők az Internet Explorer figyeli. Használt tooaccess hello Document Object Model (DOM) hello aktuális lap és toocontrol navigációs a 64 bites számítógépeken futó 32 bites program.
- HKEY\_helyi\_MACHINE\Software\Microsoft\Internet Explorer\Extensions
    - Új Internet Explorer-bővítmények, például egyéni eszköz menük és az egyéni gombok figyeli.
- HKEY\_helyi\_MACHINE\Software\Wow6432Node\Microsoft\Internet Explorer\Extensions
    - Új Internet Explorer-bővítmények, például egyéni eszköz menük és 64 bites számítógépeken futó 32 bites program egyéni eszköztár gombjai figyeli.
- HKEY\_helyi\_MACHINE\Software\Microsoft\Windows NT\CurrentVersion\Drivers32
    - Hello 32-bites illesztőprogramok wavemapper, wave1 és wave2, msacm.imaadpcm, .msadpcm, .msgsm610 és vidc társított figyeli. Hello rendszer hasonló toohello [illesztőprogramok] szakasz. INI-fájl.
- HKEY\_helyi\_MACHINE\Software\Wow6432Node\Microsoft\Windows NT\CurrentVersion\Drivers32
    - Figyelők hello 32-bites illesztőprogramok wavemapper, wave1 és wave2, msacm.imaadpcm, .msadpcm, .msgsm610 és a 64 bites számítógépeken futó 32 bites program vidc társított. Hello rendszer hasonló toohello [illesztőprogramok] szakasz. INI-fájl.
- HKEY\_helyi\_MACHINE\System\CurrentControlSet\Control\Session Manager\KnownDlls
    - Figyelők hello ismert vagy a gyakran használt rendszer DLL; listája a rendszer megakadályozza, hogy a személyek gyenge Alkalmazásengedélyek directory kihasználva ejtésével rendszer DLL-ek trójai faló verzióiban.
- HKEY\_helyi\_MACHINE\SOFTWARE\Microsoft\Windows NT\CurrentVersion\Winlogon\Notify
    - Figyelők hello csomagok képes tooreceive rendszeresemény-értesítéseket a Winlogon, hello interaktív bejelentkezési támogatási modell hello Windows operációs rendszer listája.


## <a name="use-change-tracking"></a>A változáskövetés használata
Hello megoldás telepítése után megtekintheti hello foglalja össze a figyelt kiszolgálók hello segítségével **változások követése** hello csempét **áttekintése** OMS lapján.

![Változások követése csempe képe](./media/log-analytics-change-tracking/change-tracking-tile.png)

Módosítások tooyour infrastruktúra és a következő kategóriák hello-feltárás részleteket tekintheti meg:

* Módosítások konfigurációtípus szoftver és a központi Windows-szolgáltatások szerint
* Szoftver tooapplications és az egyes kiszolgálók frissítését módosítása
* Szoftvermódosítások az egyes alkalmazások száma összesen
* Linux-csomagok
* Windows-szolgáltatások módosításai az egyes kiszolgálókon
* Linuxos démonok módosításai

![Változások követése irányítópult képe](./media/log-analytics-change-tracking/change-tracking-dash01.png)

![Változások követése irányítópult képe](./media/log-analytics-change-tracking/change-tracking-dash02.png)

### <a name="tooview-changes-for-any-change-type"></a>az összes tooview módosítások típusának módosítása
1. A hello **áttekintése** hello kattintson **változások követése** csempére.
2. A hello **módosítása követési** irányítópult, tekintse át a hello összefoglaló információkat valamelyik hello módosítás típus paneleken, és kattintson egy tooview részletes információkat, akkor a hello **naplófájl-keresési** lap.
3. Bármely hello napló keresése lapok eredmények megtekintése idő, illetve részletes leírást és a keresési korábbi naplók. Értékkorlátozás toonarrow hello eredmények is végezhet.

## <a name="next-steps"></a>Következő lépések
* Használjon [Log Analytics-e jelentkezni a keresések](log-analytics-log-searches.md) tooview részletes a változáskövetési adatok.
