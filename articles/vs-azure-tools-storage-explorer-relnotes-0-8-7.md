---
title: "A Microsoft Azure Tártallózó (előzetes verzió) 0.8.7 |} Microsoft Docs"
description: "Kibocsátási megjegyzések a Microsoft Azure Tártallózó (előzetes verzió) 0.8.7"
services: storage
documentationcenter: na
author: cawa
manager: paulyuk
editor: 
ms.assetid: 
ms.service: storage
ms.devlang: multiple
ms.topic: release-notes
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 01/18/2017
ms.author: cawa
ms.openlocfilehash: fc3620ca19d4824bc8ffb3bbe89ef47c5127b9d1
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="microsoft-azure-storage-explorer-087-preview"></a>A Microsoft Azure Tártallózó (előzetes verzió) 0.8.7
## <a name="overview"></a>Áttekintés
Ez a cikk az Azure Tártallózó 0.8.7 előzetes kiadás kibocsátási megjegyzései tartalmazza.

[A Microsoft Azure Tártallózó (előzetes verzió)](./vs-azure-tools-storage-manage-with-storage-explorer.md) egy különálló alkalmazás, amelynek segítségével egyszerűen dolgozhat Azure Storage-adatokkal Windows, a macOS és a Linux rendszeren.

## <a name="azure-storage-explorer-087-preview"></a>Az Azure Tártallózó (előzetes verzió) 0.8.7
### <a name="download-azure-storage-explorer-087-preview"></a>Töltse le az Azure Tártallózó (előzetes verzió) 0.8.7
- [A Windows Azure Storage Explorer 0.8.7 előzetes verzió](https://go.microsoft.com/fwlink/?LinkId=708343)
- [Az Azure Storage Explorer 0.8.7 Preview Mac rendszerre](https://go.microsoft.com/fwlink/?LinkId=708342)
- [Az Azure Storage Explorer 0.8.7 Preview Linux](https://go.microsoft.com/fwlink/?LinkId=722418)

### <a name="new-updates"></a>Új frissítések
* Kiválaszthatja, hogyan frissítés, letöltés, vagy a másolási munkamenet elején ütközések feloldásáról a **tevékenységek** ablak.
* Mutasson a lapon, láthatja a tárolási erőforrások elérési útját.
* Kattintson egy fülre, amikor szinkronizálják a bal oldali navigációs ablak helyére.

### <a name="fixes"></a>Javítások
* Rögzített: Tártallózó mostantól egy megbízható macOS alkalmazást.
* Rögzített: Ubuntu 14.04 támogatja.
* Rögzített: Néha a fiók hozzáadása felhasználói felület tokenkódot előfizetések betöltésekor.
* Rögzített: Néha nem minden tárolási erőforrások volt a bal oldali navigációs ablaktábla található.
* Rögzített: A Műveletek ablaktáblában néha megjelenített üres műveletek.
* Rögzített: A legutóbbi lezárt munkamenetet a az ablak mérete most őrződnek meg.
* Rögzített: Nyithatja meg a helyi menü az azonos erőforrás több lapot.

### <a name="known-issues"></a>Ismert problémák
* A gyors hozzáférés csak előfizetés alapú elemek működik. Ebben a kiadásban nem támogatottak a helyi erőforrások vagy a kulcs- vagy SAS-jogkivonat keresztül csatlakozó erőforrásokat.
* A gyors hozzáférés a keresse meg a cél erőforráson, attól függően, hogy hány erőforrásokhoz rendelkezik néhány másodpercet vehet igénybe.
* Mivel a fájlok feltöltése egy időben és blobokat több mint három csoport hibákat okozhat.
* Keresési kezeli a keresés - körülbelül 50 000-csomópont között, a teljesítmény negatív hatással lehet, vagy nem kezelt kivételeket okozhat.
* MacOS Tártallózóval először jelenhet meg a kulcsláncban eléréséhez felhasználói engedélyt kér a több kér. Javasoljuk, hogy bejelöli **mindig** , a kérdés nem jelenjen meg többé

## <a name="previous-releases"></a>Korábbi kiadások
### <a name="features"></a>Szolgáltatások
#### <a name="main-features"></a>Főbb funkciók
* macOS, Linux, és a Windows-verziók
* Jelentkezzen be a Storage-fiókok előfizetés szerint csoportosítva megtekintheti:
    * Használja a szervezeti fiók Microsoft Account, 2FA, stb.
    * Konfigurálhatja és kezelheti a proxybeállítások
    * Távolítsa el a fiókokat kijelentkezéssel egyidejűleg
* Kapcsolódás Storage-fiókok:
    * Fiók neve és kulcsa
    * Egyéni végpontokat (beleértve az Azure Kína)
    * Storage-fiókok SAS URI-azonosítója
* Az Azure Resource Manager és klasszikus tárolási támogatása
* A blobok, blobtárolók, várólisták, táblák vagy fájlmegosztások SAS-kulcsok létrehozása
* Blob tárolók, várólisták, táblák csatlakozni, vagy fájlmegosztások megosztott hozzáférési aláírásokkal (SAS) kulccsal
* A blobtárolók, a várólisták, a táblák vagy a fájlmegosztásokon tárolt hozzáférési házirendjeinek kezelése
* Helyi fejlesztési tároló a Storage Emulator (csak Windows)
* Hozzon létre vagy töröljön a blobtárolók, a várólisták és a táblázatok
* Nézet $logs blob tárolók és $metrics táblák
* Keresse meg az adott BLOB, üzenetsorok, táblák vagy fájlmegosztások
* Közvetlen hivatkozások mutatnak a storage-fiókok vagy a tárolók, a várólisták, a táblák vagy a fájl megosztásához, és az erőforrások egyszerűen elérése megosztások
* Nevezze át a blobtárolók, táblák, fájlmegosztások
* Képes kezelni, és konfigurálja a CORS-szabályokat
* Könnyen másolja az elsődleges és másodlagos kulcsot Storage-fiókok
* Feltöltés és letöltés adatintegritást és az egységességet MD5 ellenőrzése
* Keresse meg a blobtárolók, táblák, üzenetsorok, fájlmegosztások vagy a keresőmezőbe a storage-fiókok
* PIN-kód a leggyakrabban használt szolgáltatások a gyors elérés egyszerű kezelhetőség lehet
* Több szerkesztők egyes lapjainak most már úgy is megnyithatja. Egyetlen kattintással megnyitni egy ideiglenes fülre. Kattintson duplán egy állandó lap megnyitásához. Az ideiglenes lapon abba, hogy egy állandó fülre kattintva
* Észrevehető teljesítményét és stabilitását érintő fejlesztések a feltöltéseket és letöltéseket, különösen olyan nagyméretű fájlok gyors gépeken
* Azt a továbbfejlesztett hatókörös keresésre vannak ismételt bevezetése, és a hatókör fogalmát hozzáadni. Adja meg az elérési út egy csomópont, keresztül az egérmutatót ikonra, jobb kattintás -> "Keresési az itt", vagy manuálisan hatókörének csomóponton. Miután hatókörűek, adhat hozzá a kívánt keresőkifejezést elérési útját a részletes keresést, hogy a csomópont végén
* Jelentek meg különböző témák: ritka (alapértelmezett), sötét, kontrasztos fekete és kontrasztos fehér.
* Válassza a Szerkesztés -> témák használatát igény szerint módosíthatja témák
* Linux, a 64 bites operációs rendszer mostantól szükséges
* Frissítettük az embléma!
#### <a name="blobs"></a>Blobok
* Blobok megtekintése, és keresse meg a könyvtárak
* Töltse fel, töltse le, törlése, és másolja a blobok és mappák
* Megnyithatja és megtekintheti a tartalmak szöveg és a kép blobok
* A blob tulajdonságai és metaadatok megtekintése és szerkesztése
* Keresés a blobokban előtag alapján
* Hozzon létre, és címbérleteket a blobok és blobtárolók megszüntetése
* Húzza az n dobja el a fájlok feltöltése
* Nevezze át a blobok és mappák
* Blob tárolók most hozhatók létre üres "virtuális" mappák
* Módosíthatja a Blob és a fájl tulajdonságait
#### <a name="tables"></a>Táblák
* Tekintheti meg és ODATA rendelkező entitás lekérdezése vagy lekérdezés-szerkesztő összetett lekérdezések létrehozása
* Hozzáadása, szerkesztése, törlése entitások
* Importálás és Exportálás CSV formátumban (beleértve a lekérdezési eredmények exportálása) tábla tartalma
* Oszlopok sorrendjének testreszabása
* Lekérdezések mentése képessége
#### <a name="queues"></a>Üzenetsorok
* A legutóbbi 32 üzenet megtekintése
* Adja hozzá, Created, üzenet megtekintése
* Sor törlése
* Azt teszik lehetővé, hogy meghatározza, hogy szeretné-e a várólista-üzenetek kódolási/dekódolási
#### <a name="file-shares"></a>Fájlmegosztások
* Fájlok megtekintése és haladjon végig a könyvtárak
* Fájlok és könyvtárat feltöltése, letöltése, törlése és másolása
* Fájl tulajdonságainak megtekintése
* Nevezze át a fájlok és könyvtárak

### <a name="bug-fixes"></a>Hibajavítások
* Rögzített: Képernyőn befagyasztási problémák
* Rögzített: Fokozott biztonság

### <a name="known-issues"></a>Ismert problémák
* Kezeli a keresés - körülbelül 50 000-csomópont között, ez lehet a teljesítmény érintett macOS telepítés keresési lehet szükség emelt szintű engedélyekkel
* Fiók beállítások panel jelenhet meg, hogy meg kell adnia a hitelesítő adatok előfizetések szűrése
* Blobok (külön-külön vagy átnevezett blob tárolóhoz belül) átnevezése nem őrzi meg a pillanatképeket. Minden más tulajdonságok és metaadatok blobokat, fájlok és entitások megőriz egy átnevezése
* Az Azure verem jelenleg nem támogatja a fájlokat, ezért próbál bontsa ki a **fájlok** csomópont hibát eredményez.
* Linux 14.04 telepítés ÖET verziójának frissítése, vagy frissíteni kell. A következő lépések bemutatják, hogyan frissítheti:

```
sudo add-apt-repository ppa:ubuntu-toolchain-r/test
sudo apt-get update
sudo apt-get upgrade
sudo apt-get dist-upgrade
```

### <a name="previous-versions"></a>Korábbi verziók
#### <a name="october-2016-release-version-085"></a>2016. októberi kiadása (0.8.5 verzió)
* [Töltse le a Windows](https://go.microsoft.com/fwlink/?LinkId=809306)
* [Töltse le a Mac](https://go.microsoft.com/fwlink/?LinkId=809307)
* [Töltse le a Linux](https://go.microsoft.com/fwlink/?LinkId=809308)
