---
title: "aaaHow toocreate egy egyéni sablon rendszerképet az Azure Remoteappban |} Microsoft Docs"
description: "Ismerje meg, hogyan toocreate egy egyéni sablon rendszerképet az Azure RemoteApp. Ez a sablon felhőalapú vagy hibrid gyűjtemény használható."
services: remoteapp
documentationcenter: 
author: msmbaldwin
manager: mbaldwin
editor: 
ms.assetid: b9ec5b51-f7cd-470b-8545-d0fd714c5982
ms.service: remoteapp
ms.workload: compute
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/26/2017
ms.author: mbaldwin
ms.openlocfilehash: 9e3a0b2d3cc56177ea51406e6cecfe19ff235340
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="how-toocreate-a-custom-template-image-for-azure-remoteapp"></a>Hogyan toocreate egy egyéni sablon rendszerképet az Azure RemoteApp
> [!IMPORTANT]
> Az Azure RemoteApp 2017. augusztus 31-ét követően megszűnik. Olvasási hello [közlemény](https://go.microsoft.com/fwlink/?linkid=821148) részleteiről.
> 
> 

Az Azure RemoteApp használja a Windows Server 2012 R2 sablon kép toohost, amelyet meg szeretne tooshare a felhasználók minden hello program. egyéni RemoteApp-sablonlemezkép toocreate, indítsa el a meglévő lemezképet, de hozzon létre egy újat. 

> [!TIP]
> Tudta, hogy egy lemezképet hozhat létre egy Azure virtuális Gépen? Szövegegység igaz értékű, és csökkenti a hello időn azt veszi tooimport hello kép. Tekintse meg a hello lépéseket [Itt](remoteapp-image-on-azurevm.md).
> 
> 

az Azure RemoteApp használata esetén feltölthető hello kép hello követelményei a következők:

* hello képméret MB többszörösének kell lennie. Ha egy olyanra, amely nincs egy pontos több tooupload, hello feltöltése sikertelen lesz.
* hello a kép mérete 127 GB méretűnek kell lennie, vagy kisebb.
* A VHD-fájlt kell lennie (VHDX-fájlok [Hyper-V virtuális merevlemezek] jelenleg nem támogatottak).
* hello virtuális merevlemez nem lehet a 2. generációs virtuális gépek.
* rögzített méretű vagy dinamikusan bővülő hello VHD lehet. Dinamikusan bővülő virtuális merevlemez használata ajánlott, mivel kevesebb idő tooupload tooAzure, mint a rögzített méretű VHD-fájl szükséges.
* hello lemez inicializálni kell hello fő rendszertöltő rekord (MBR) particionálási stílus használatával. GUID partíciós tábla (GPT) partícióval hello nem támogatott.
* hello VHD-t tartalmaznia kell a Windows Server 2012 R2 egy telepítés. Több kötet, de csak egy Windows példányának tartalmazó tartalmazhat.
* hello távoli asztali munkamenetgazda (RDSH) szerepkör és hello asztali élmény szolgáltatást telepíteni kell.
* Távoli asztali Kapcsolatszervező szerepkör hello kell *nem* telepíthető.
* hello titkosított fájlrendszer (EFS) le kell tiltani.
* hello képnek kell lennie a Sysprep segédprogrammal előkészített hello paraméterek használatával **/oobe / generalize shutdown** (ne használja az hello **/mode:vm** paraméter).
* A pillanatkép-lánc virtuális merevlemez feltöltése nem támogatott.

**Előkészületek**

Hello szolgáltatás létrehozása előtt toodo hello következőkre lesz szüksége:

* [Regisztráció](https://azure.microsoft.com/services/remoteapp/) a RemoteApp.
* A felhasználói fiók létrehozása az Active Directory toouse hello RemoteApp szolgáltatás fiókja. Korlátozza a hello engedélyeket ehhez a fiókhoz, így akkor is csak tartományhoz gép toohello. Lásd: [Azure Active Directory konfigurálása a RemoteApp](remoteapp-ad.md) további információt.
* A helyszíni hálózattal kapcsolatos adatok gyűjtése: IP-cím információkat és a VPN-eszköz részletei.
* Telepítse a hello [Azure PowerShell](/powershell/azure/overview) modul.
* Hello felhasználók toogrant eléréséhez használni kívánt információt gyűjteni. Ez lehet, vagy a Microsoft-fiók adatainak, vagy az Active Directory munkahelyi fiók adatait, a felhasználók számára.

## <a name="create-a-template-image"></a>Sablon lemezkép létrehozása
Hello magas szintű lépéseket toocreate egy teljesen új sablon rendszerképet az alábbiak:

1. Keresse meg a Windows Server 2012 R2 frissítés DVD vagy ISO-lemezkép.
2. Hozzon létre egy VHD-fájlt.
3. Windows Server 2012 R2 telepítése.
4. Hello távoli asztali munkamenetgazda (RDSH) szerepkör és hello asztali élmény összetevő telepítése.
5. Az alkalmazások által megkövetelt további szolgáltatások telepítéséhez.
6. Telepítse és konfigurálja az alkalmazásokat. toomake megosztási alkalmazások egyszerűbb, adja hozzá az alkalmazásokhoz és a programok, amelyet az tooshare toohello **Start** hello kép, különösen a **%systemdrive%\ProgramData\Microsoft\Windows\Start Menu\Programs menü.
7. Hajtsa végre a további Windows-konfigurációkat, az alkalmazások által megkövetelt.
8. Tiltsa le a hello titkosított fájlrendszer (EFS).
9. **SZÜKSÉGES:** tooWindows frissítés lépjen, és telepítse az összes fontos frissítést.
10. A SYSPREP hello kép.

hello új lemezkép létrehozásának részletes lépései a következők:

1. Keresse meg a Windows Server 2012 R2 frissítés DVD vagy ISO-lemezkép.
2. Hozzon létre egy VHD-fájlt a Lemezkezelés eszköz használatával.
   
   1. Nyissa meg a Lemezkezelés (Diskmgmt.msc beépülő modult).
   2. Legalább 40 GB-nál dinamikusan bővülő virtuális merevlemez létrehozása. (A lemezterületre van szükség a Windows, az alkalmazások és a testreszabások becsült hello mennyisége. Windows Server hello RDSH szerepkörrel és az asztali élmény összetevő telepítése szükséges körülbelül 10 GB lemezterületet).
      
      1. Kattintson a **művelet > virtuális merevlemez létrehozásának**.
      2. Adja meg a hello helyét, a méret és a VHD-formátumban. Válassza ki **dinamikusan bővülő**, és kattintson a **OK**.
      
      Ez néhány másodpercig fog futni. Virtuális merevlemez létrehozása hello befejeződése után meg kell jelennie egy új lemezt, anélkül, hogy a meghajtó betűjelével és hello Lemezkezelés konzolt a "Nincs inicializálva" állapotban.
      
      * Kattintson a jobb gombbal a hello lemezterület (nem szabad hello), és kattintson **lemez inicializálása**. Válassza ki **MBR** (fő rendszertöltő rekord) lehetőséget hello partíció stílusát, majd kattintson **OK**.
      * Hozzon létre egy új kötetet: kattintson a jobb gombbal a nem lefoglalt lemezterülettel hello, és kattintson **új egyszerű kötet**. Fogadja el hello alapértelmezett hello varázslóban, de mindenképpen hello sablonlemezkép feltöltésekor rendelje hozzá egy meghajtó betűjele tooavoid potenciális problémákat.
      * Kattintson a jobb gombbal a hello lemez, és kattintson **leválasztani a virtuális merevlemez**.
3. Windows Server 2012 R2 telepítése:
   
   1. Hozzon létre egy új virtuális gépet. Új virtuális gép varázsló Hyper-V kezelője vagy az ügyféloldali Hyper-V hello használata.
      1. Hello generáció a lapon választhatja ki **1. generáció**.
      2. Hello virtuális merevlemez csatlakoztatása oldalon válassza ki a **meglévő virtuális merevlemez használata**, és keresse meg a toohello hello előző lépésben létrehozott virtuális Merevlemezt.
      3. Hello telepítési beállítások lapon válassza az **operációs rendszer telepítése rendszerindító CD/DVD_ROM-ről**, majd válassza ki a Windows Server 2012 R2 telepítési adathordozó hello helyét.
      4. Válassza ki a hello varázsló szükséges tooinstall Windows egyéb beállítások és az alkalmazások. Hello varázsló befejezéséhez.
   2. Hello varázsló befejezése után hello hello virtuális gép beállításainak szerkesztése és más módosítást szükséges tooinstall Windows és a programok, például hello számú virtuális processzort, majd **OK**.
   3. Csatlakoztassa toohello virtuális gépet, és telepítse a Windows Server 2012 R2.
4. Hello távoli asztali munkamenetgazda (RDSH) szerepkör és hello asztali élmény összetevő telepítése:
   1. Indítsa el a Kiszolgálókezelőt.
   2. Kattintson a **szerepkörök és szolgáltatások hozzáadása** hello üdvözlőképernyőn vagy hello **kezelése** menü.
   3. Kattintson a **következő** hello alapismeretek lapon.
   4. Válassza ki **szerepköralapú vagy szolgáltatásalapú telepítés**, és kattintson a **következő**.
   5. Válassza ki a helyi számítógép hello hello listából, és kattintson **következő**.
   6. Válassza ki **távoli asztali szolgáltatások**, és kattintson a **következő**.
   7. Bontsa ki a **felhasználói felületek és infrastruktúra** válassza **asztali élmény**.
   8. Kattintson a **szolgáltatások hozzáadása**, és kattintson a **következő**.
   9. Kattintson a távoli asztali szolgáltatások lap hello **következő**.
   10. Kattintson a **távoli asztali munkamenetgazda**.
   11. Kattintson a **szolgáltatások hozzáadása**, és kattintson a **következő**.
   12. Hello megerősítése telepítendő összetevők megerősítése oldalon válassza **újraindítás hello célkiszolgáló szükség esetén automatikusan**, és kattintson a **Igen** hello indítsa újra figyelmeztetés.
   13. Kattintson az **Install** (Telepítés) gombra. hello számítógép újraindul.
5. Az alkalmazások, például a .NET-keretrendszer 3.5 hello szükséges további szolgáltatások telepítéséhez. tooinstall hello szolgáltatások, hello hozzáadása szerepkörök és szolgáltatások varázsló futtatása.
6. Telepítse és konfigurálja a hello programokat, valamint remoteappot toopublish kívánt alkalmazásokat.

> [!IMPORTANT]
> Hello RDSH szerepkör telepítése feltöltött tooRemoteApp problémák merülnek fel a kompatibilitási előtt hello kép felderített alkalmazások tooensure telepítése előtt.
> 
> Győződjön meg arról, hogy egy helyi tooyour alkalmazás (**.lnk** fájl) hello megjelenik **Start** az összes felhasználó számára (%systemdrive%\ProgramData\Microsoft\Windows\Start Menu\Programs) menü. Gondoskodjon arról is, hogy a hello látni hello ikon **Start** menü, amit meg akar felhasználók toosee. Ha nem, a módosításhoz. (Nem *rendelkezik* tooadd hello alkalmazás toohello Start menüben, de teszi sokkal könnyebb toopublish hello alkalmazás RemoteApp. Ellenkező esetben van tooprovide hello telepítési útvonal hello alkalmazás hello app közzétételekor.)
> 
> 

1. Hajtsa végre a további Windows-konfigurációkat, az alkalmazások által megkövetelt.
2. Tiltsa le a hello titkosított fájlrendszer (EFS). Futtassa a következő parancsot egy rendszergazda jogú parancssori ablakban hello:
   
     Fsutil működése disableencryption 1 beállítása
   
   Azt is megteheti állítsa be, vagy adja hozzá a következő DWORD-értéket hello beállításjegyzék hello:
   
     HKLM\System\CurrentControlSet\Control\FileSystem\NtfsDisableEncryption = 1
3. Ha egy Azure virtuális gépen belül a lemezkép, nevezze át a hello  **\%windir%\Panther\Unattend.xml** fájlt a Ez meggátolja hello feltöltés parancsfájl később használt működését. Módosítsa a fájl tooUnattend.old hello nevét, hogy a rendszer még akkor sem hello fájl abban az esetben, ha a központi telepítés toorevert szüksége.
4. Nyissa meg a frissítés tooWindows, és minden fontos frissítések telepítéséhez. Szükség lehet a Windows Update többször tooget toorun összes frissítést. (Néha olyan frissítést telepít, és maga frissítést egy frissítés szükséges.)
5. A SYSPREP hello kép. Egy rendszergazda jogú parancssorba futtassa a következő parancs hello:
   
   **C:\Windows\System32\sysprep\sysprep.exe / generalize/oobe/shutdown**
   
   **Megjegyzés:** ne használjon hello **/mode:vm** a SYSPREP parancsot annak ellenére, hogy ez a virtuális gépek hello kapcsoló.

## <a name="next-steps"></a>Következő lépések
Most, hogy az egyéni sablon rendszerképet, kell tooupload adott kép tooyour RemoteApp-gyűjteményt. A következő cikkek toocreate hello hello adatokat a gyűjtemény használható:

* [Hogyan toocreate RemoteApp hibrid gyűjteménye](remoteapp-create-hybrid-deployment.md)
* [Hogyan toocreate RemoteApp felhőalapú gyűjtemény](remoteapp-create-cloud-deployment.md)

