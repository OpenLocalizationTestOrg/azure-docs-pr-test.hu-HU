---
title: "Azure Tártallózó (előzetes verzió) 0.8.7 aaaMicrosoft |} Microsoft Docs"
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
ms.openlocfilehash: 9fdd491a3ea838e20f9d4f82c176cfb02fbe306b
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="microsoft-azure-storage-explorer-087-preview"></a>A Microsoft Azure Tártallózó (előzetes verzió) 0.8.7
## <a name="overview"></a>Áttekintés
Ez a cikk hello Azure Tártallózó 0.8.7 előzetes kiadás kibocsátási megjegyzései tartalmazza.

[A Microsoft Azure Tártallózó (előzetes verzió)](./vs-azure-tools-storage-manage-with-storage-explorer.md) egy különálló alkalmazás, amely lehetővé teszi az Azure Storage-adatokkal Windows, a macOS és a Linux tooeasily használata.

## <a name="azure-storage-explorer-087-preview"></a>Az Azure Tártallózó (előzetes verzió) 0.8.7
### <a name="download-azure-storage-explorer-087-preview"></a>Töltse le az Azure Tártallózó (előzetes verzió) 0.8.7
- [A Windows Azure Storage Explorer 0.8.7 előzetes verzió](https://go.microsoft.com/fwlink/?LinkId=708343)
- [Az Azure Storage Explorer 0.8.7 Preview Mac rendszerre](https://go.microsoft.com/fwlink/?LinkId=708342)
- [Az Azure Storage Explorer 0.8.7 Preview Linux](https://go.microsoft.com/fwlink/?LinkId=722418)

### <a name="new-updates"></a>Új frissítések
* Kiválaszthatja, hogyan tooresolve ütközések hello elején egy adott frissítés letöltése vagy másolása munkamenet hello **tevékenységek** ablak.
* Egy lapon toosee hello teljes elérési útja hello tárolási erőforrás mutasson.
* Kattintson egy fülre, amikor szinkronizálják hello bal oldali navigációs ablak helyére.

### <a name="fixes"></a>Javítások
* Rögzített: Tártallózó mostantól egy megbízható macOS alkalmazást.
* Rögzített: Ubuntu 14.04 támogatja.
* Rögzített: Néha hello vegye fel a fiók felhasználói felület tokenkódot előfizetések betöltésekor.
* Rögzített: Néha nem minden tárolási erőforrások szereplő hello bal oldali navigációs ablaktáblán.
* Rögzített: hello műveletpanelen néha megjelenített üres műveletek.
* Rögzített: hello ablakméret utolsó lezárt hello munkamenetből most őrződnek meg.
* Rögzített: Megnyithatja a hello több lapot azonos erőforrás hello helyi menü segítségével.

### <a name="known-issues"></a>Ismert problémák
* A gyors hozzáférés csak előfizetés alapú elemek működik. Ebben a kiadásban nem támogatottak a helyi erőforrások vagy a kulcs- vagy SAS-jogkivonat keresztül csatlakozó erőforrásokat.
* Gyors elérés érdekében néhány másodperc toonavigate toohello cél erőforráson, attól függően, hogy hány erőforrásokat vehet igénybe.
* Mivel a fájlok feltöltése: hello azonos és blobokat több mint három csoport idő hibákat okozhat.
* Keresési kezeli a keresés - körülbelül 50 000-csomópont között, a teljesítmény negatív hatással lehet, vagy nem kezelt kivételeket okozhat.
* Hello az első alkalommal használja a Tártallózó hello macOS, a jelenhet meg több kér, felhasználó engedély tooaccess hello kulcslánc megadását kéri. Javasoljuk, hogy bejelöli **mindig** , hello kérdés nem jelenjen meg többé

## <a name="previous-releases"></a>Korábbi kiadások
### <a name="features"></a>Szolgáltatások
#### <a name="main-features"></a>Főbb funkciók
* macOS, Linux, és a Windows-verziók
* Bejelentkezés tooview a Storage-fiókok előfizetés szerint csoportosítva:
    * Használja a szervezeti fiók Microsoft Account, 2FA, stb.
    * Konfigurálhatja és kezelheti a proxybeállítások
    * Távolítsa el a fiókokat kijelentkezéssel egyidejűleg
* Csatlakozás tooStorage fiókok használatával:
    * Fiók neve és kulcsa
    * Egyéni végpontokat (beleértve az Azure Kína)
    * Storage-fiókok SAS URI-azonosítója
* Az Azure Resource Manager és klasszikus tárolási támogatása
* A blobok, blobtárolók, várólisták, táblák vagy fájlmegosztások SAS-kulcsok létrehozása
* Csatlakozás tooblob tárolók, várólisták, táblák vagy fájlmegosztások megosztott hozzáférési aláírásokkal (SAS-) kulcs
* A blobtárolók, a várólisták, a táblák vagy a fájlmegosztásokon tárolt hozzáférési házirendjeinek kezelése
* Helyi fejlesztési tároló a Storage Emulator (csak Windows)
* Hozzon létre vagy töröljön a blobtárolók, a várólisták és a táblázatok
* Nézet $logs blob tárolók és $metrics táblák
* Keresse meg az adott BLOB, üzenetsorok, táblák vagy fájlmegosztások
* Közvetlen hivatkozások toostorage fiókok vagy a tárolók, a várólisták, a táblák vagy a fájl közös megosztásához, és az erőforrások egyszerűen elérése
* Nevezze át a blobtárolók, táblák, fájlmegosztások
* Képes toomanage, és konfigurálja a CORS-szabályokat
* Könnyen másolja az elsődleges és másodlagos kulcsot Storage-fiókok
* Feltöltés és letöltés adatintegritást és az egységességet MD5 ellenőrzése
* Keresse meg a blobtárolók, táblák, üzenetsorok, fájlmegosztások vagy hello keresőmezőbe a storage-fiókok
* PIN-kód a leggyakrabban használt szolgáltatások toohello gyorselérési egyszerű kezelhetőség lehet
* Több szerkesztők egyes lapjainak most már úgy is megnyithatja. Egy kattintással tooopen ideiglenes fülre. Kattintson duplán a tooopen állandó fülre. Kattintson a hello ideiglenes lapon toomake azt egy állandó lap
* Észrevehető teljesítményét és stabilitását érintő fejlesztések a feltöltéseket és letöltéseket, különösen olyan nagyméretű fájlok gyors gépeken
* Ismételt a bevezetése hello továbbfejlesztett hatókörös keresésre és hatókör hozzáadott hello fogalma azt. Adja meg a hello elérési tooa csomópont hello rámutatáskor ikon keresztül, kattintson a jobb gombbal -> "Keresési az itt", vagy manuálisan tooscope csomóponton. Miután a hatóköre, felveheti hello elérési toodeep keresést, hogy a csomópont egy keresési kifejezés toohello vége
* Jelentek meg különböző témák: ritka (alapértelmezett), sötét, kontrasztos fekete és kontrasztos fehér.
* Nyissa meg tooEdit -> témák toochange témák használatát igény szerint
* Linux, a 64 bites operációs rendszer mostantól szükséges
* Frissítettük az embléma!
#### <a name="blobs"></a>Blobok
* Blobok megtekintése, és keresse meg a könyvtárak
* Töltse fel, töltse le, törlése, és másolja a blobok és mappák
* Megnyithatja és megtekintheti a hello tartalmát szöveges kép blobok és
* A blob tulajdonságai és metaadatok megtekintése és szerkesztése
* Keresés a blobokban előtag alapján
* Hozzon létre, és címbérleteket a blobok és blobtárolók megszüntetése
* Húzza az n dobja el a fájlok tooupload
* Nevezze át a blobok és mappák
* Blob tárolók most hozhatók létre üres "virtuális" mappák
* Módosíthatja hello Blob és a fájl tulajdonságait
#### <a name="tables"></a>Táblák
* Tekintheti meg és ODATA rendelkező entitás lekérdezése vagy lekérdezés-szerkesztő toocreate összetett lekérdezések
* Hozzáadása, szerkesztése, törlése entitások
* Importálás és Exportálás CSV formátumban (beleértve a lekérdezési eredmények exportálása) tábla tartalma
* Oszlopok sorrendjének testreszabása
* Képes toosave lekérdezések
#### <a name="queues"></a>Üzenetsorok
* A legutóbbi 32 üzenet megtekintése
* Adja hozzá, Created, üzenet megtekintése
* Sor törlése
* Azt tette lehetővé a akkor toodecide kíván-e tooencode/dekódolni a várólista-üzenetek
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
* Fiók beállítások panel jelenhet meg, hogy kell-e tooreenter hitelesítő adatok toofilter előfizetések
* Blobok (külön-külön vagy átnevezett blob tárolóhoz belül) átnevezése nem őrzi meg a pillanatképeket. Minden más tulajdonságok és metaadatok blobokat, fájlok és entitások megőriz egy átnevezése
* Az Azure verem jelenleg nem támogatja a fájlokat, ezért próbált tooexpand hello **fájlok** csomópont hibát eredményez.
* Linux 14.04 telepítés ÖET verziójának frissítése, vagy frissíteni kell. hello következő lépések bemutatják, hogyan tooupgrade:

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
