---
title: "aaaMicrosoft Azure Tártallózó (előzetes verzió) kibocsátási megjegyzései |} Microsoft Docs"
description: "Kibocsátási megjegyzések a Microsoft Azure Tártallózó (előzetes verzió)"
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
ms.date: 07/31/2017
ms.author: cawa
ms.openlocfilehash: 44ff6dc8e2015f4eb71fa13098b832fbbf48ccac
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="microsoft-azure-storage-explorer-preview-release-notes"></a>Kibocsátási megjegyzések a Microsoft Azure Tártallózó (előzetes verzió)

A cikkben hello kibocsátási megjegyzések a 0.8.16. Azure Tártallózó (előzetes verzió) kiadása, valamint a kibocsátási megjegyzések a korábbi verziók.

[A Microsoft Azure Tártallózó (előzetes verzió)](./vs-azure-tools-storage-manage-with-storage-explorer.md) egy különálló alkalmazás, amely lehetővé teszi az Azure Storage-adatokkal Windows, a macOS és a Linux tooeasily használata.

## <a name="version-0816-preview"></a>Verzió 0.8.16 (előzetes verzió)
8/21/2017

### <a name="download-azure-storage-explorer-0816-preview"></a>Töltse le az Azure Tártallózó (előzetes verzió) 0.8.16
- [A Windows Azure Tártallózó (előzetes verzió) 0.8.16](https://go.microsoft.com/fwlink/?LinkId=708343)
- [A Mac Azure Tártallózó (előzetes verzió) 0.8.16](https://go.microsoft.com/fwlink/?LinkId=708342)
- [Linux rendszerhez készült Azure Tártallózó (előzetes verzió) 0.8.16](https://go.microsoft.com/fwlink/?LinkId=722418)

### <a name="new"></a>új
* Amikor megnyit egy blobot, Tártallózó kérni fogja tooupload hello letöltött fájl változás észlelésekor
* Azure verem bejelentkezési élmény
* Fájlok továbbfejlesztett hello teljesítmény sok kis feltöltése/Letöltés: hello azonos idő


### <a name="fixes"></a>Javítások
* Néhány blob típusok kiválasztása túl "cseréje" során egy feltöltési ütközést néha eredményezne hello feltöltés újraindítja. 
* Verzióban 0.8.15 a feltöltések volna néha megrekedésének 99 %.
* Feltöltésekor a rendszer fájlok tooa fájlmegosztáshoz, ha úgy dönt, amely még nem létezett tooupload tooa directory, a feltöltést sikertelen lesz.
* Tártallózó helytelenül lett időbélyegeket közös hozzáférésű jogosultságkód valamint tábla létrehozásához.


Ismert problémák
* Név és kulcs kapcsolati karakterlánc nem működik. A következő verzióra hello rögzítettek. Addig is használhat nevével és a kulcs csatolása.
* Ha tooopen egy Windows-fájl érvénytelen nevű fájl, hello letöltési egy fájl nem található az hibát eredményez.
* Után a "Mégse gombra" kattintva meg olyan feladatra, amíg az adott feladat toocancel is eltarthat. Ez a hello Azure Storage csomópont könyvtár korlátozása.
* Egy blob feltöltése befejezése után hello fülre, amely hello feltöltés kezdeményezett frissül. Módosult az előző viselkedését, és is miatt a toobe visszakerül a hello tároló toohello gyökérmappájában.
* Ha úgy dönt, hogy helytelen PIN-kód/intelligens kártya tanúsítványának hello, akkor szüksége lesz a rendelés toohave Tártallózó toorestart feledje, hogy döntést.
* hello fiók beállítások panel jelenhet meg, hogy kell-e tooreenter hitelesítő adatok toofilter előfizetések.
* Blobok (külön-külön vagy átnevezett blob tárolóhoz belül) átnevezése nem őrzi meg a pillanatképeket. Minden más tulajdonságok és metaadatok BLOB-, fájl-és entitások egy átnevezési megőriz.
* Bár az Azure-verem jelenleg nem támogatja a fájlmegosztásokat, fájlmegosztások csomópont továbbra is egy csatolt verem Azure storage-fiók alatt jelenik meg.
* A felhasználók számára az Ubuntu 14.04, szüksége lesz a ÖET működik-e toodate tooensure – hello futtatásával ehhez a következő parancsokat, és indítsa újra a gépet:

    ```
    sudo add-apt-repository ppa:ubuntu-toolchain-r/test
    sudo apt-get update
    sudo apt-get upgrade
    sudo apt-get dist-upgrade
    ```

* Ubuntu 17.04 felhasználójához szüksége lesz a tooinstall GConf - ez végezhető el a következő parancsokat, és indítsa újra a gépet hello fut:

    ```
    sudo apt-get install libgconf-2-4
    ```

## <a name="version-0814-preview"></a>Verzió 0.8.14 (előzetes verzió)
06/22/2017

### <a name="download-azure-storage-explorer-0814-preview"></a>Töltse le az Azure Tártallózó (előzetes verzió) 0.8.14
* [Töltse le a Windows Azure Tártallózó (előzetes verzió) 0.8.14](https://go.microsoft.com/fwlink/?LinkId=809306)
* [Töltse le a Mac Azure Tártallózó (előzetes verzió) 0.8.14](https://go.microsoft.com/fwlink/?LinkId=809307)
* [Linux rendszerhez készült Azure Tártallózó (előzetes verzió) 0.8.14 letöltése](https://go.microsoft.com/fwlink/?LinkId=809308)

### <a name="new"></a>új

* Számos fontos biztonsági frissítést rendelés tootake előnyeit too1.7.2, frissített elektronsugár verziója
* Most már elérheti, hello online hibaelhárítási útmutató hello Súgó menü
* A Tártallózó hibaelhárítási [útmutató][2]
* [Útmutatás] [ 3] a tooan Azure verem előfizetés csatlakozó

### <a name="known-issues"></a>Ismert problémák

* Hello delete mappa megerősítő párbeszédpanelen levő hello kattintások Linux nincs regisztrálása. Kerülő megoldás lehet toouse hello Enter billentyűt
* Ha úgy dönt, hogy helytelen PIN-kód/intelligens kártya tanúsítványának hello, akkor szüksége lesz a rendelés toohave Tártallózó toorestart elfelejti hello döntési
* Mivel a fájlok feltöltése: hello azonos és blobokat több mint 3 csoport idő hibákat okozhat
* hello fiók beállítások panel jelenhet meg, hogy a rendelés toofilter előfizetések tooreenter-felhasználó hitelesítő adatait kell
* Blobok (külön-külön vagy átnevezett blob tárolóhoz belül) átnevezése nem őrzi meg a pillanatképeket. Minden más tulajdonságok és metaadatok BLOB-, fájl-és entitások egy átnevezési megőriz.
* Bár az Azure-verem jelenleg nem támogatja a fájlmegosztások, fájlmegosztások csomópont továbbra is egy csatolt verem Azure storage-fiók alatt jelenik meg. 
* Ubuntu 14.04 telepítés igényeinek ÖET verzió frissítése, vagy frissített – lépéseket tooupgrade alatt van:

    ```
    sudo add-apt-repository ppa:ubuntu-toolchain-r/test
    sudo apt-get update
    sudo apt-get upgrade
    sudo apt-get dist-upgrade
    ```




## <a name="previous-releases"></a>Korábbi kiadások

* [0.8.13 verzió](#version-0813)
* [Verzió 0.8.12 / 0.8.11 / 0.8.10](#version-0812--0811--0810)
* [Verzió 0.8.9 / 0.8.8](#version-089--088)
* [0.8.7 verzió](#version-087)
* [0.8.6 verzió](#version-086)
* [0.8.5 verzió](#version-085)
* [0.8.4 verzió](#version-084)
* [0.8.3 verzió](#version-083)
* [0.8.2 verzió](#version-082)
* [0.8.0 verzió](#version-080)
* [0.7.20160509.0 verzió](#version-07201605090)
* [0.7.20160325.0 verzió](#version-07201603250)
* [0.7.20160129.1 verzió](#version-07201601291)
* [0.7.20160105.0 verzió](#version-07201601050)
* [0.7.20151116.0 verzió](#version-07201511160)


### <a name="version-0813"></a>0.8.13 verzió
05/12/2017

#### <a name="new"></a>új

* A Tártallózó hibaelhárítási [útmutató][2]
* [Útmutatás] [ 3] a tooan Azure verem előfizetés csatlakozó

#### <a name="fixes"></a>Javítások

* Rögzített: A fájl feltöltése volna, amely egy kevés a memória, nagy valószínűséggel
* Rögzített: Most már bejelentkezhet PIN-kód/intelligens kártya
* Rögzített: Nyissa meg a portál most works Azure Kína, a Németországi Azure, a Azure Amerikai Egyesült államokbeli kormányzati és az Azure verem
* Rögzített: A mappa tooa blobtárolóban feltöltésekor "Szabálytalan műveletet" hiba néha lépne
* Rögzített: Az összes le lett tiltva, a pillanatképek felügyelete során
* Rögzített: hello alap blob metaadatait hello előfordulhat, hogy első felül után a pillanatképek hello tulajdonságainak megtekintése

#### <a name="known-issues"></a>Ismert problémák

* Ha úgy dönt, hogy helytelen PIN-kód/intelligens kártya tanúsítványának hello, akkor szüksége lesz a rendelés toohave Tártallózó toorestart elfelejti hello döntési
* Bejövő vagy kimenő nagyított, hello nagyítási szintjét, rövid ideig alaphelyzetbe toohello alapértelmezett szint
* Mivel a fájlok feltöltése: hello azonos és blobokat több mint 3 csoport idő hibákat okozhat
* hello fiók beállítások panel jelenhet meg, hogy a rendelés toofilter előfizetések tooreenter-felhasználó hitelesítő adatait kell
* Blobok (külön-külön vagy átnevezett blob tárolóhoz belül) átnevezése nem őrzi meg a pillanatképeket. Minden más tulajdonságok és metaadatok BLOB-, fájl-és entitások egy átnevezési megőriz.
* Bár az Azure-verem jelenleg nem támogatja a fájlmegosztásokat, fájlmegosztások csomópont továbbra is egy csatolt verem Azure storage-fiók alatt jelenik meg. 
* Ubuntu 14.04 telepítés igényeinek ÖET verzió frissítése, vagy frissített – lépéseket tooupgrade alatt van:

    ```
    sudo add-apt-repository ppa:ubuntu-toolchain-r/test
    sudo apt-get update
    sudo apt-get upgrade
    sudo apt-get dist-upgrade
    ```


### <a name="version-0812--0811--0810"></a>Verzió 0.8.12 / 0.8.11 / 0.8.10
04/07/2017

#### <a name="new"></a>új

* A Tártallózó automatikusan leáll egy frissítés a hello frissítési értesítés
* Helyben gyors hozzáférést biztosít a fokozott élményt nyújtó a gyakran használt erőforrásokat használata
* Hello Blob tároló szerkesztő, most már megtekintheti a bérelt blob tartozik virtual machine
* Most csukja össze hello bal oldali panelen
* Felderítési most már fut a hello azonos idő letölthető
* Felhasználási statisztikáinak a hello Blob tároló, a fájlmegosztás és a tábla szerkesztők toosee hello az erőforrás vagy méretének kiválasztása
* Most már bejelentkezhet tooAzure Active Directory (AAD) alapú Azure verem fiókok is. 
* Feltöltés archív fájlok tooPremium storage-fiókok több mint 32 MB lehet
* Továbbfejlesztett kisegítő támogatása
* Mostantól hozzáadhatja azok megbízható Base-64 kódolású X.509 SSL-tanúsítványok által is tooEdit -&gt; SSL-tanúsítványok -&gt; tanúsítványok importálása

#### <a name="fixes"></a>Javítások

* Rögzített: után frissíteni egy fiók hitelesítő adatait, hello fanézetben volna néha nem automatikus frissítése
* Rögzített: emulátor üzenetsorok és táblák SAS-kód generálása eredményeznének egy URL-cím érvénytelen
* Rögzített: prémium szintű storage-fiókok most bővíthetők a proxy be van kapcsolva
* Rögzített: hello alkalmazni gomb hello fiókok kezelése lap csatlakoztatás nem működik, ha kijelölt 1 vagy 0-fiókok
* Rögzített: ütközés megoldások igénylő blobok feltöltése meghiúsulhat - 0.8.11 rögzített 
* Rögzített: visszajelzés küldése lett feltörése 0.8.11 - 0.8.12 rögzített 

#### <a name="known-issues"></a>Ismert problémák

* A frissítés too0.8.10 után szüksége lesz a toorefresh összes hitelesítő adatait.
* Bejövő vagy kimenő nagyított, hello nagyítási szint rövid ideig lehetséges, hogy alaphelyzetbe toohello alapértelmezett szintjét.
* Mivel a fájlok feltöltése: hello azonos és blobokat több mint 3 csoport idő hibákat okozhat.
* hello fiók beállítások panel jelenhet meg, hogy a rendelés toofilter előfizetések tooreenter-felhasználó hitelesítő adatait kell.
* Blobok (külön-külön vagy átnevezett blob tárolóhoz belül) átnevezése nem őrzi meg a pillanatképeket. Minden más tulajdonságok és metaadatok BLOB-, fájl-és entitások egy átnevezési megőriz.
* Bár az Azure-verem jelenleg nem támogatja a fájlmegosztásokat, fájlmegosztások csomópont továbbra is egy csatolt verem Azure storage-fiók alatt jelenik meg. 
* Ubuntu 14.04 telepítés igényeinek ÖET verzió frissítése, vagy frissített – lépéseket tooupgrade alatt van:

    ```
    sudo add-apt-repository ppa:ubuntu-toolchain-r/test
    sudo apt-get update
    sudo apt-get upgrade
    sudo apt-get dist-upgrade
    ```


### <a name="version-089--088"></a>Verzió 0.8.9 / 0.8.8
02/23/2017

<iframe width="560" height="315" src="https://www.youtube.com/embed/R6gonK3cYAc?ecver=1" frameborder="0" allowfullscreen></iframe>

<iframe width="560" height="315" src="https://www.youtube.com/embed/SrRPCm94mfE?ecver=1" frameborder="0" allowfullscreen></iframe>


#### <a name="new"></a>új

* A Tártallózó 0.8.9 automatikusan letölti hello legújabb verzióját a frissítéseket.
* Gyorsjavítás: a portál használatával létrehozott SAS URI tooattach hiba történt a tárfiók eredményezne.
* Most létrehozására, kezelésére és blob pillanatképek lépteti elő.
* Most már bejelentkezhet tooAzure Kína, a Németországi Azure és az Azure Amerikai Egyesült államokbeli kormányzati fiókok.
* Mostantól megváltoztatható hello nagyítási szintjét. Hello Nézet menü tooZoom kicsinyítés és Nagyítás alaphelyzetbe állítja a hello beállításokat használják.
* Unicode-karaktereket mostantól támogatja a felhasználói blobok és a fájlok metaadatait.
* Kisegítő lehetőségek fejlesztései.
* hello frissítési értesítés hello következő verziójára kibocsátási megjegyzéseket is megtekinthetők. Hello aktuális kibocsátási megjegyzések hello Súgó menüjében is megtekintheti.

#### <a name="fixes"></a>Javítások

* Rögzített: hello verziószámának megfelelően megjelenik a Windows Vezérlőpult
* Javítani: keresési már nem korlátozott too50, 000 csomópontok
* Rögzített: feltöltés tooa fájlmegosztást hoz végtelen Ha hello célkönyvtáron már nem létezik
* Rögzített: jobb stabilitás hosszú feltöltések és letöltések

#### <a name="known-issues"></a>Ismert problémák

* Bejövő vagy kimenő nagyított, hello nagyítási szint rövid ideig lehetséges, hogy alaphelyzetbe toohello alapértelmezett szintjét.
* A gyors hozzáférés csak olyan alapú előfizetés elemek működik. Ebben a kiadásban nem támogatottak a helyi erőforrások vagy a kulcs- vagy SAS-jogkivonat keresztül csatlakozó erőforrásokat.
* Gyors elérés érdekében néhány másodperc toonavigate toohello cél erőforráson, attól függően, hogy hány erőforrásokat vehet igénybe.
* Mivel a fájlok feltöltése: hello azonos és blobokat több mint 3 csoport idő hibákat okozhat.

12/16/2016
### <a name="version-087"></a>0.8.7 verzió

<iframe width="560" height="315" src="https://www.youtube.com/embed/Me4Y4jxoer8?ecver=1" frameborder="0" allowfullscreen></iframe>

#### <a name="new"></a>új

* Kiválaszthatja, hogyan tooresolve ütközések hello elején egy adott frissítés letöltése vagy hello tevékenységek ablakban másolja a munkamenet
* Rámutat egy lapon toosee hello teljes elérési útja hello tárolási erőforrás
* Ha egy lap gombra kattint, hello bal oldali navigációs ablak helyére szinkronizálják

#### <a name="fixes"></a>Javítások

* Rögzített: Tártallózó mostantól a megbízható alkalmazások Mac gépen
* Rögzített: Ubuntu 14.04 támogatja
* Rögzített: Néha hello fiók felhasználói felület tokenkódot előfizetések betöltésekor
* Rögzített: Néha nem minden tárolási erőforrások szereplő hello bal oldali navigációs panelen
* Rögzített: hello műveletpanelen néha megjelenített üres műveletek
* Rögzített: hello ablakméret utolsó lezárt hello munkamenetből most őrződnek meg
* Rögzített: Megnyithatja a hello több lapot azonos erőforrást hello helyi menü

#### <a name="known-issues"></a>Ismert problémák

* A gyors hozzáférés csak olyan alapú előfizetés elemek működik. Helyi erőforrások vagy a kulcs- vagy SAS-jogkivonat keresztül csatlakozó erőforrásokat nem támogatott ebben a kiadásban
* Gyors elérés érdekében néhány másodperc toonavigate toohello cél erőforráson, attól függően, hogy hány erőforrásokat is eltarthat
* Mivel a fájlok feltöltése: hello azonos és blobokat több mint 3 csoport idő hibákat okozhat
* Keresési kezeli, ezt követően a körülbelül 50 000 csomópontokon - keresés teljesítmény negatív hatással lehet, vagy nem kezelt kivételt okozhat
* Hello az első alkalommal használja a Tártallózó hello macOS, a jelenhet meg több kér, felhasználó engedély tooaccess kulcslánc megadását kéri. Javasoljuk, hogy mindig választja, az így hello kérdés nem jelenik meg újra

11/18/2016
### <a name="version-086"></a>0.8.6 verzió

#### <a name="new"></a>új

* PIN-kód a leggyakrabban használt szolgáltatások toohello gyorselérési egyszerű kezelhetőség lehet
* Több szerkesztők egyes lapjainak most már úgy is megnyithatja. Egy kattintással tooopen ideiglenes fülre. Kattintson duplán a tooopen állandó fülre. Is rákattinthat a hello ideiglenes lapon toomake azt egy állandó lap
* Hajtottunk észlelhető teljesítménybeli és stabilitását érintő fejlesztések feltölti és tölti le, különösen olyan nagyméretű fájlok gyors gépeken
* Blob tárolók most hozhatók létre üres "virtuális" mappák
* Azt újra vezetett be az új kibővített substring keresés hatókörös keresésre, most két választási lehetősége van a keresés: 
    * Globális keresés - csak meg kell adnia a kívánt keresőkifejezést hello keresési szövegmező
    * Hatókörös keresésre - hello Nagyítót ábrázoló ikonra következő tooa csomópontot, kattintson, majd vegye fel a keresési kifejezés toohello vége hello elérési utat, vagy kattintson a jobb gombbal, "keresési az itt"
* Jelentek meg különböző témák: ritka (alapértelmezett), sötét, kontrasztos fekete és kontrasztos fehér. Nyissa meg tooEdit -&gt; témák toochange témák használatát igény szerint
* Módosíthatja a Blob és a fájl tulajdonságait
* Most már támogatott kódolt (base64) és kódolatlan üzenetek
* Linux, a 64 bites operációs rendszer mostantól szükséges. Ebben a kiadásban csak támogatott 64 bites Ubuntu 16.04.1 LTS
* Frissítettük az embléma!

#### <a name="fixes"></a>Javítások

* Rögzített: Képernyőn befagyasztási problémák
* Rögzített: Fokozott biztonság
* Rögzített: Néha duplikált csatolt fiókot fog megjelenni
* Rögzített: Nincs megadva content típusú blob tudta előállítani kivétel
* Rögzített: Nyitó hello lekérdezés Panel a program üres táblát nem sikerült
* Rögzített: Változó a keresési hibák
* Rögzített: Hello olyan erőforrások száma, ha "Több betöltése" gombra kattintva 50 too100 betöltődnek nőtt
* Rögzített: Első alkalommal történő futtatásakor, a fiók alá van írva, ha azt immár előfizetéseket fiók alapértelmezés szerint 

#### <a name="known-issues"></a>Ismert problémák

* Hello Tártallózó jelen kiadása nem futtatható Ubuntu 14.04
* tooopen több lapot a ugyanazt az erőforrást, így folyamatosan nem kattintson a hello hello ugyanazt az erőforrást. Kattintson egy másik erőforrás és majd lépjen vissza, majd a hello eredeti erőforrás tooopen azt újra egy másik lapján 
* A gyors hozzáférés csak olyan alapú előfizetés elemek működik. Helyi erőforrások vagy a kulcs- vagy SAS-jogkivonat keresztül csatlakozó erőforrásokat nem támogatott ebben a kiadásban
* Gyors elérés érdekében néhány másodperc toonavigate toohello cél erőforráson, attól függően, hogy hány erőforrásokat is eltarthat
* Mivel a fájlok feltöltése: hello azonos és blobokat több mint 3 csoport idő hibákat okozhat
* Keresési kezeli, ezt követően a körülbelül 50 000 csomópontokon - keresés teljesítmény negatív hatással lehet, vagy nem kezelt kivételt okozhat

10/03/2016
### <a name="version-085"></a>0.8.5 verzió

#### <a name="new"></a>új

* Ekkor a portál által létrehozott SAS használja a kulcsok tooattach tooStorage fiókjainak és erőforrásainak

#### <a name="fixes"></a>Javítások

* Rögzített: egyes esetekben a keresés közben versenyhelyzet okozott csomópontok toobecome nem bővíthető
* Rögzített: "HTTP használata" nem működik, amikor tooStorage fiókok összekapcsolása fióknevet és kulcsot
* Rögzített: SAS-kulcsok (kifejezetten portál által létrehozott állók közül.) a hibaüzenetet adja vissza "záró perjelet"
* Rögzített: tábla importálása problémák
    * Egyes esetekben partíciókulcs- és sorkulcsa lettek visszaállítva
    * Nem lehet "null" Partíciókulcsok tooread

#### <a name="known-issues"></a>Ismert problémák

* Keresési kezeli a keresés - körülbelül 50 000-csomópont között, előfordulhat, hogy lehet a teljesítményre
* Azure verem jelenleg nem támogatott fájlok, így tooexpand fájlok próbált megjelenítése hiba

09/12/2016
### <a name="version-084"></a>0.8.4 verzió

<iframe width="560" height="315" src="https://www.youtube.com/embed/cr5tOGyGrIQ?ecver=1" frameborder="0" allowfullscreen></iframe>

#### <a name="new"></a>új

* Közvetlen hivatkozások toostorage fiókok, tárolók, várólisták, táblák létrehozása vagy fájlmegosztásokat és könnyen megosztható tooyour erőforrások - Windows és az elérésére támogatja a Mac OS
* Keresse meg a blobtárolók, táblák, üzenetsorok, fájlmegosztások vagy hello keresőmezőbe a storage-fiókok
* Most csoportosíthatja záradékok hello tábla Lekérdezésszerkesztőben
* Nevezze át, és a másolás/beillesztés blob tárolók, fájlmegosztások, táblák, blobok, blob mappákat, fájlokat és könyvtárakat az SAS-csatolású fiókok és a tárolók belül
* Átnevezése és lemásolná a blob-tárolók és a fájlmegosztások a tulajdonságok és metaadatok most megőrzése

#### <a name="fixes"></a>Javítások

* Rögzített: nem szerkeszthető táblaentitásokat tartalmazó logikai vagy bináris tulajdonság

#### <a name="known-issues"></a>Ismert problémák

* Keresési kezeli a keresés - körülbelül 50 000-csomópont között, előfordulhat, hogy lehet a teljesítményre

08/03/2016
### <a name="version-083"></a>0.8.3 verzió

<iframe width="560" height="315" src="https://www.youtube.com/embed/HeGW-jkSd9Y?ecver=1" frameborder="0" allowfullscreen></iframe>

#### <a name="new"></a>új

* Nevezze át a tárolók, táblák, fájlmegosztások
* Lekérdezés-szerkesztő fejlett élményt
* Képes toosave és a lekérdezések
* Hivatkozások toostorage fiókok vagy a tárolókat, várólisták, táblák közvetlen vagy fájlmegosztások megosztásához, és könnyen fér hozzá az erőforrások (csak Windows - macOS támogatása hamarosan elérhető!)
* Képes toomanage, és konfigurálja a CORS-szabályokat

#### <a name="fixes"></a>Javítások

* Rögzített: Microsoft Accounts ismételt hitelesítés szükséges 8 – 12 óránként

#### <a name="known-issues"></a>Ismert problémák

* Egyes esetekben hello felhasználói felület előfordulhat, hogy válaszol - hello ablak maximalizálva segít a probléma megoldásához
* macOS telepítés emelt jogosultsági szintű lehet szükség.
* Fiók beállítások panel jelenhet meg, hogy a rendelés toofilter előfizetések tooreenter-felhasználó hitelesítő adatait kell
* Átnevezési fájlmegosztások, a blob-tárolók és a táblák nem őrzi meg a metaadat- vagy más tulajdonságaiból hello tárolóhoz, például a fájlmegosztási kvóta, a nyilvános hozzáférési szint vagy a hozzáférési házirendek
* Blobok (külön-külön vagy átnevezett blob tárolóhoz belül) átnevezése nem őrzi meg a pillanatképeket. Minden más tulajdonságok és metaadatok BLOB-, fájl-és entitások megőriz egy átnevezése
* Másolással vagy erőforrások átnevezése nem működik SAS-csatolású fiókok belül

07/07/2016
### <a name="version-082"></a>0.8.2 verzió

<iframe width="560" height="315" src="https://www.youtube.com/embed/nYgKbRUNYZA?ecver=1" frameborder="0" allowfullscreen></iframe>

#### <a name="new"></a>új

* Storage-fiókok vannak csoportosítva, előfizetések; fejlesztési tárolás és a kulcs- vagy SAS keresztül csatlakozó erőforrásokat (helyi és kapcsolódó) csomópont alatt látható
* Jelentkezzen ki a "Azure-fiók beállítások" panelen fiókokhoz
* Proxy beállításainak tooenable konfigurálhatja és kezelheti a bejelentkezés
* Hozzon létre és blob címbérleteket megszüntetése
* Nyissa meg a blob-tárolók, várólisták, táblák és az egy kattintással fájlok

#### <a name="fixes"></a>Javítások

* Rögzített: a .NET vagy Java könyvtárak beszúrni üzenetek vannak nem megfelelően dekódolni a Base64 kódolású anyag
* Rögzített: $metrics táblák nem láthatók a Blob Storage-fiókok
* Rögzített: táblák csomópont nem működik a helyi (fejlesztés) tároláshoz

#### <a name="known-issues"></a>Ismert problémák

* macOS telepítés emelt jogosultsági szintű lehet szükség.

06/15/2016
### <a name="version-080"></a>0.8.0 verzió

<iframe width="560" height="315" src="https://www.youtube.com/embed/ycfQhKztSIY?ecver=1" frameborder="0" allowfullscreen></iframe>

<iframe width="560" height="315" src="https://www.youtube.com/embed/k4_kOUCZ0WA?ecver=1" frameborder="0" allowfullscreen></iframe>

<iframe width="560" height="315" src="https://www.youtube.com/embed/3zEXJcGdl_k?ecver=1" frameborder="0" allowfullscreen></iframe>

#### <a name="new"></a>új

* Megosztás-támogatás: megtekintése, feltöltése, letöltés, fájlok és könyvtárak, SAS URI-azonosítók másolására (létrehozása, és csatlakozzon)
* Növelt felhasználói élmény a SAS URI-azonosító vagy kulcsait tooStorage összekapcsolása
* Tábla lekérdezési eredmények exportálása
* Tábla oszlop újra sorrendbe rendezés és a Testreszabás
* Blob tárolók $logs és $metrics táblák megtekintését Storage-fiókok engedélyezett metrikák
* Továbbfejlesztett exportálási és viselkedésének importálni, mostantól tartalmazza a tulajdonságérték-típus

#### <a name="fixes"></a>Javítások

* Rögzített: vagy nagy blobok feltöltése eredményezhet teljes feltöltések/letöltések
* Rögzített: Szerkesztés, hozzáadását és importálása egy numerikus karakterlánc-érték ("1") rendelkező entitás konvertálja toodouble
* Rögzített: Nem tooexpand hello csomópontjának hello helyi fejlesztési környezet

#### <a name="known-issues"></a>Ismert problémák

* $metrics táblák nem láthatók a Blob Storage-fiókok
* Programozott módon hozzáadott üzenetek jelenhetnek meg megfelelően Ha köszönőüzenetei Base64 kódolás használatával kódolt

05/17/2016
### <a name="version-07201605090"></a>0.7.20160509.0 verzió

#### <a name="new"></a>új

* Jobb hiba történt az alkalmazás kezelése összeomlik

#### <a name="fixes"></a>Javítások

* Ahol információs sáv üzenetek néha nem jelennek meg, ha a bejelentkezési hitelesítő adatok volt szükség rögzített hiba

#### <a name="known-issues"></a>Ismert problémák

* Táblázatok: Hozzáadása, szerkesztése, vagy olyan entitás, amely egy kétértelműen numerikus érték, például az "1" vagy "1.0" tulajdonsággal rendelkezik importálása és felhasználói záma toosend hello azt egy `Edm.String`, hello érték vissza határozza meg, egy Edm.Double API hello ügyfélen keresztül

03/31/2016

### <a name="version-07201603250"></a>0.7.20160325.0 verzió

<iframe width="560" height="315" src="https://www.youtube.com/embed/imbgBRHX65A?ecver=1" frameborder="0" allowfullscreen></iframe>

<iframe width="560" height="315" src="https://www.youtube.com/embed/ceX-P8XZ-s8?ecver=1" frameborder="0" allowfullscreen></iframe>


#### <a name="new"></a>új

* Táblázat a támogatási szolgálathoz: megtekintésre lekérdezése, importálása és exportálása CRUD műveletek az entitások
* Feldolgozási sor támogatási: megtekintését, hozzáadását, dequeueing üzenetek
* SAS URI-azonosítók Storage-fiókok létrehozása
* SAS URI-azonosítók tooStorage fiókok összekapcsolása
* A jövőbeli frissítések tooStorage Explorer vonatkozó frissítési értesítések
* Frissített Megjelenés és működés

#### <a name="fixes"></a>Javítások

* Teljesítmény és megbízhatóság fejlesztései

### <a name="known-issues-amp-mitigations"></a>Ismert problémák &amp; megoldást

* A nagy blob fájlok letöltésének nem működik megfelelően – AzCopy használatát, amíg a hiba megoldása érdekében azt javasoljuk 
* Fiók hitelesítő adatai nem fogja beolvasni nem hello kezdőmappa nem található vagy nem lehet írni a gyorsítótárba
* Ha azt vannak hozzáadása, szerkesztése és importálása olyan entitás, amely egy kétértelműen numerikus érték, például az "1" vagy "1.0" tulajdonsággal rendelkezik, és hello felhasználó megpróbál toosend azt egy `Edm.String`, hello érték vissza határozza meg, egy Edm.Double API hello ügyfélen keresztül
* Többsoros rekordot tartalmazó CSV-fájlok importálásakor hello adatok előfordulhat, hogy darabolja vagy titkosított

02/03/2016

### <a name="version-07201601291"></a>0.7.20160129.1 verzió

#### <a name="fixes"></a>Javítások

* Jobb összesített teljesítményt biztosít, feltöltését, letöltése és a BLOB másolása

01/14/2016

### <a name="version-07201601050"></a>0.7.20160105.0 verzió

#### <a name="new"></a>új

* Linux-támogatás (paritásos szolgáltatások tooOSX)
* Blob tárolók hozzáadása a megosztott hozzáférési aláírásokkal (SAS-) kulcs
* Kínai Azure Storage-fiókok hozzáadása
* Egyéni végpontokat a Storage-fiókok hozzáadása
* Megnyithatja és megtekintheti a hello tartalmát szöveges kép blobok és
* A blob tulajdonságai és metaadatok megtekintése és szerkesztése

#### <a name="fixes"></a>Javítások

* Rögzített: feltöltés és letöltés választható nagyszámú BLOB (500 +) néha okozhat hello app toohave fehér képernyő 
* Rögzített: blob tároló nyilvános hozzáférési szint, hello új érték nem frissül addig, amíg újból be hello tároló hello helyezi a hangsúlyt. Emellett hello párbeszédpanel mindig az alapértelmezett túl nincs nyilvános hozzáférés-", és nem hello tényleges jelenlegi értéke.
* Jobb általános billentyűzet/kisegítő és a felhasználói felület támogatja
* Ha a szóköz karakter hosszú fussa útkövetését előzmények szöveg
* SAS párbeszédpanel támogatja a bemenet-ellenőrzést
* Helyi tárolás továbbra is toobe érhető el, akkor is, ha a felhasználó hitelesítő adatainak tanúsítványa lejárt
* Egy megnyitott blob-tároló törlésekor hello blob explorer hello jobb oldalán ki van zárva

#### <a name="known-issues"></a>Ismert problémák

* Linux telepítés igényeinek ÖET verzió frissítése, vagy frissített – lépéseket tooupgrade alatt van: 
    * `sudo add-apt-repository ppa:ubuntu-toolchain-r/test`
    * `sudo apt-get update`
    * `sudo apt-get upgrade`
    * `sudo apt-get dist-upgrade`

11/18/2015
### <a name="version-07201511160"></a>0.7.20151116.0 verzió

#### <a name="new"></a>új

* macOS, és a Windows-verziók
* A Storage-fiókok tooview bejelentkezés, – használja a szervezeti fiók Microsoft Account, 2FA, stb.
* Helyi fejlesztési tárolási (storage emulatorral csak Windows)
* Az Azure Resource Manager és klasszikus erőforrás-támogatás
* Hozzon létre vagy töröljön a blobokat, üzenetsorokat vagy táblák
* Keresse meg az adott blobokat, üzenetsorokat vagy táblák
* Blob tárolók hello tartalmát felfedezés
* Megtekintheti és haladjon végig a könyvtárak
* Töltse fel, töltse le és törölje a blobok és mappák
* A blob tulajdonságai és metaadatok megtekintése és szerkesztése
* SAS-kulcsok létrehozása
* Kezelése és tárolása hozzáférési házirendek (SAP) létrehozása
* Keresés a blobokban előtag alapján
* Húzza az n dobja el a fájlok tooupload vagy letöltése

#### <a name="known-issues"></a>Ismert problémák

* Blob tároló nyilvános hozzáférési szint beállításakor a hello új érték nem frissül, amíg újra vizsgál hello hello tárolóra
* Nyissa meg a hello párbeszédpanel tooset hello nyilvános hozzáférési szint, mindig mutat "Nem nyilvános hozzáférés" hello alapértelmezett, és nem hello tényleges aktuális érték
* A letöltött blobokat nem nevezhető át.
* Rendszer néha "elakadnak" a egy folyamatban lévő tevékenységek naplóbejegyzések állapot, ha hiba lép fel, és hello hiba nem jelenik meg
* Néha összeomlik vagy viszont teljesen fehér tooupload közben, vagy nagyszámú blobok letöltése
* A másolási műveletek néha megszakítása nem működik
* Során hozza létre a tárolót (blob vagy sor vagy tábla), ha érvénytelen bemeneti és a folytatáshoz toocreate az egy másik tárolóhoz típusa egy másik, fókusza nem adható meg hello új típus
* Nem hozható létre új mappát, vagy nem mappa átnevezése




[2]: https://support.microsoft.com/en-us/help/4021389/storage-explorer-troubleshooting-guide
[3]: vs-azure-tools-storage-manage-with-storage-explorer.md