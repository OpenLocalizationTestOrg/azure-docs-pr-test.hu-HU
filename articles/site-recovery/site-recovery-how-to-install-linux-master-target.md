---
title: "A Linux-fő célkiszolgáló feladatátvevő telepítése az Azure-ból a helyszíni |} Microsoft Docs"
description: "A Linux virtuális gép újbóli védelméhez, előtt kell egy Linux fő célkiszolgáló. Megtudhatja, hogyan telepítsen."
services: site-recovery
documentationcenter: 
author: rajani-janaki-ram
manager: gauravd
editor: 
ms.assetid: 44813a48-c680-4581-a92e-cecc57cc3b1e
ms.service: site-recovery
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: 
ms.date: 11/22/2017
ms.author: rajanaki
ms.openlocfilehash: 7b2416617696e1df30b08f039ab39bfe7b57e093
ms.sourcegitcommit: 310748b6d66dc0445e682c8c904ae4c71352fef2
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 11/28/2017
---
# <a name="install-a-linux-master-target-server"></a>A Linux fő célkiszolgáló telepítése
Után a virtuális gépeket nem sikerül, akkor is feladat-visszavételt a virtuális gépeket a helyszíni hely. A feladat-visszavételt, állítsa a virtuális gépet az Azure-ból a helyszíni helyre kell. Ez a folyamat szüksége van egy a helyszíni fő célkiszolgáló forgalom fogadására. 

Ha a védett virtuális gépet egy Windows rendszerű virtuális gép, majd meg kell olyan Windows fő célkiszolgálót. A Linux virtuális gép van szüksége a Linuxos fő célkiszolgáló. Olvassa el az alábbi lépések végrehajtásával megtudhatja, hogyan hozzon létre és telepítse a Linuxos fő célkiszolgáló.

> [!IMPORTANT]
> A 9.10.0 a kiadástól kezdve fő célkiszolgálót, a legújabb fő célkiszolgáló csak telepíthető egy Ubuntu 16.04 kiszolgálót. Új telepítések CentOS6.6 kiszolgálók nincsenek engedélyezve. Azonban továbbra is a régi fő célkiszolgálók frissítése a 9.10.0 használatával verziója.

## <a name="overview"></a>Áttekintés
Ez a cikk ismerteti a Linuxos fő célkiszolgáló telepítése.

Megjegyzéseit vagy kérdéseit a cikk vagy a végén utáni a [Azure Recovery Services fórumon](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr).

## <a name="prerequisites"></a>Előfeltételek

* Válassza ki a gazdagépet, amelyre a fő célkiszolgáló telepítése, használatával állapítsa meg, ha a feladat-visszavétel lesz egy meglévő helyszíni virtuális gép vagy egy új virtuális gépet. 
    * Egy meglévő virtuális gép a gazdagép a fő célkiszolgáló hozzáféréssel kell rendelkeznie a virtuális gép adattároló.
    * Ha a helyszíni virtuális gép nem létezik, akkor a feladat-visszavétel a virtuális gép jön létre a fő célkiszolgáló ugyanazon a gazdagépen. Kiválaszthatja, hogy bármely telepítéséhez a fő célkiszolgáló ESXi-állomáson.
* A fő célkiszolgálón, amely a folyamatkiszolgáló és a kiszolgáló képes kommunikálni a hálózaton kell lennie.
* A fő célkiszolgáló verziója korábbi, mint a folyamatkiszolgáló és a konfigurációs kiszolgáló verziói vagy azzal egyenlőnek kell lennie. Például ha a konfigurációs kiszolgáló verziószáma 9.4, a fő célkiszolgáló verziója lehet 9.4 vagy 9.3, de nem 9,5.
* A fő célkiszolgáló csak akkor lehet VMware virtuális gépek nem egy fizikai kiszolgálón.

## <a name="create-the-master-target-according-to-the-sizing-guidelines"></a>Hozzon létre a fő célkiszolgálón a méretezési igényeinek megfelelően

Hozza létre a fő célkiszolgálón a következő méretezési irányelvek szerint:
- **RAM**: legalább 6 GB
- **Az operációs rendszer lemezméret**: 100 GB vagy több (CentOS6.6 telepítendő)
- **További lemezmérete adatmegőrzési meghajtó**: 1 TB-os
- **A Processzormagok**: 4 mag, vagy több

A következő támogatott Ubuntu kernelek támogatottak.


|Kernel-sorozat  |Legfeljebb  |
|---------|---------|
|4.4      |4.4.0-81-Generic         |
|4.8      |4.8.0-56-Generic         |
|4.10     |4.10.0-24-Generic        |


## <a name="deploy-the-master-target-server"></a>A fő célkiszolgáló telepítése

### <a name="install-ubuntu-16042-minimal"></a>Ubuntu 16.04.2 telepítése minimális

A következő igénybe az Ubuntu 16.04.2 64 bites operációs rendszer telepítésének lépéseit.

**1. lépés:** Ugrás a [letöltése hivatkozásra](https://www.ubuntu.com/download/server/thank-you?version=16.04.2&architecture=amd64) , és válassza a legközelebbi tükrözésének mely le Ubuntu 16.04.2 minimális 64 bites ISO-Lemezképet.

Ubuntu 16.04.2 minimális 64 bites ISO-Lemezképet tartani a DVD-meghajtóba, és indítsa el a rendszer.

**2. lépés:** válasszon **angol** a kívánt nyelvet, és válassza ki, **Enter**.

![Válasszon nyelvet](./media/site-recovery-how-to-install-linux-master-target/ubuntu/image1.png)

**3. lépés:** válasszon **Ubuntu Server telepítése**, majd válassza ki **Enter**.

![Válassza ki a telepítés Ubuntu Server](./media/site-recovery-how-to-install-linux-master-target/ubuntu/image2.png)

**4. lépés:** válasszon **angol** a kívánt nyelvet, és válassza ki, **Enter**.

![Jelölje ki a kívánt nyelvet angol nyelven](./media/site-recovery-how-to-install-linux-master-target/ubuntu/image3.png)

**5. lépés:** válassza ki a megfelelő beállítást a **időzóna** beállítások listáját, és válassza **Enter**.

![Válassza ki a megfelelő időzónát](./media/site-recovery-how-to-install-linux-master-target/ubuntu/image4.png)

**6. lépés:** válasszon **nem** (az alapértelmezett beállítás), majd válassza ki **Enter**.


![A billentyűzet konfigurálása](./media/site-recovery-how-to-install-linux-master-target/ubuntu/image5.png)

**7. lépés:** válasszon **angol (amerikai)** , billentyűzet, és válassza ki azt a származási **Enter**.

![Jelölje ki a származási USA](./media/site-recovery-how-to-install-linux-master-target/ubuntu/image6.png)

**8. lépés:** kiválasztása **angol (amerikai)** a billentyűzetkiosztás módosítása, és válassza ki, **Enter**.

![A billentyűzetkiosztás angol kiválasztása](./media/site-recovery-how-to-install-linux-master-target/ubuntu/image7.png)

**9. lépés:** az állomásnév megadása a kiszolgáló a **állomásnév** mezőbe, majd válassza ki **Folytatás**.

![Adja meg a kiszolgáló állomásneve](./media/site-recovery-how-to-install-linux-master-target/ubuntu/image8.png)

**10. lépés:** hozzon létre egy felhasználói fiókot, adja meg a felhasználónevet, és válassza **Folytatás**.

![Felhasználói fiók létrehozása](./media/site-recovery-how-to-install-linux-master-target/ubuntu/image9.png)

**11. lépés:** adja meg az új felhasználói fiók jelszavát, majd válassza ki **Folytatás**.

![Adja meg a jelszót](./media/site-recovery-how-to-install-linux-master-target/ubuntu/image10.png)

**12. lépés:** erősítse meg az új felhasználói jelszót, majd válassza ki **Folytatás**.

![A jelszó megerősítése](./media/site-recovery-how-to-install-linux-master-target/ubuntu/image11.png)

**13. lépés:** válasszon **nem** (az alapértelmezett beállítás), majd válassza ki **Enter**.

![Felhasználók és a jelszavak beállítása](./media/site-recovery-how-to-install-linux-master-target/ubuntu/image12.png)

**14. lépés:** Ha helyesek az időzónát, amely akkor jelenik meg, válassza ki a **Igen** (az alapértelmezett beállítás), majd válassza ki **Enter**.

Válassza ki, hogy engedélyezzék az időzónát, **nem**.

![Az óra konfigurálása](./media/site-recovery-how-to-install-linux-master-target/ubuntu/image13.png)

**15. lépés:** a particionálási mód közül válassza ki a **interaktív - használja a teljes lemez**, majd válassza ki **Enter**.

![Válassza a particionálási mód lehetőséget](./media/site-recovery-how-to-install-linux-master-target/ubuntu/image14.png)

**16. lépés:** válassza ki a megfelelő lemezt a **partícióra válassza lemez** beállítások, és válassza **Enter**.


![Válassza ki a lemezt](./media/site-recovery-how-to-install-linux-master-target/ubuntu/image15.png)

**17. lépés:** válasszon **Igen** írása a lemezre, majd válassza a módosítások **Enter**.

![A módosítások írása a lemezre](./media/site-recovery-how-to-install-linux-master-target/ubuntu/image16.png)

**18. lépés:** jelölje be az alapértelmezett beállítást, jelölje ki **Folytatás**, majd válassza ki **Enter**.

![Válassza ki az alapértelmezett beállítás](./media/site-recovery-how-to-install-linux-master-target/ubuntu/image17.png)

**19. lépést:** a megfelelő beállítást a rendszer a frissítések kezelése, majd válassza ki és **Enter**.

![Válassza ki a frissítések kezelése](./media/site-recovery-how-to-install-linux-master-target/ubuntu/image18.png)

> [!WARNING]
> Az Azure Site Recovery fő célkiszolgáló az Ubuntu olyan speciális verziója szükséges, mert annak érdekében, hogy a frissítések le vannak tiltva, a virtuális gép kernel szüksége. Ha engedélyezve vannak, a rendszeres megjelenjenek ezután a fő célkiszolgáló hibás működését. Mindenképpen jelölje ki a **automatikus frissítések** lehetőséget.


**20. lépés:** válassza ki az alapértelmezett beállításokat. Ha openSSH az SSH csatlakozni, jelölje be a **OpenSSH server** lehetőséget, majd válassza ki **Folytatás**.

![Válassza ki a szoftver](./media/site-recovery-how-to-install-linux-master-target/ubuntu/image19.png)

**21. lépés:** válasszon **Igen**, majd válassza ki **Enter**.

![Telepítés céljaként a LÁRVAJÁRAT rendszertöltő](./media/site-recovery-how-to-install-linux-master-target/ubuntu/image20.png)

**22. lépés:** válassza ki a megfelelő eszköz, a rendszerindító betöltő telepítés (lehetőleg **/dev/sda**), majd válassza ki **Enter**.

![Jelöljön ki egy eszközt a rendszerindító betöltő telepítéshez](./media/site-recovery-how-to-install-linux-master-target/ubuntu/image21.png)

**23. lépés:** kiválasztása **Folytatás**, majd válassza ki **Enter** fejezze be a telepítést.

![Fejezze be a telepítést](./media/site-recovery-how-to-install-linux-master-target/ubuntu/image22.png)

Miután a telepítés befejeződött, jelentkezzen be a virtuális Gépet az új felhasználói hitelesítő adatokkal. (Hivatkoznak **lépés 10** további információt.)

Az alábbi képernyőfelvételen a legfelső szintű felhasználói jelszó beállítása ismertetett lépéseket. Legfelső szintű felhasználóként, majd jelentkezzen be.

![A legfelső szintű felhasználói jelszó beállítása](./media/site-recovery-how-to-install-linux-master-target/ubuntu/image23.png)


### <a name="prepare-the-machine-for-configuration-as-a-master-target-server"></a>A gép konfigurációjának előkészítése a fő célkiszolgáló-kiszolgálóként
A következő előkészítése a számítógépet, a konfiguráció fő kiszolgálóként.

Ahhoz, hogy az azonosító az egyes SCSI merevlemezekhez a Linux virtuális gépeken, engedélyezze a **lemez. EnableUUID = TRUE** paraméter.

Ahhoz, hogy ez a paraméter, a következő lépéseket:

1. Állítsa le a virtuális gépet.

2. Kattintson a jobb gombbal a virtuális gép a bal oldali panelen a bejegyzést, és válassza ki **beállításainak szerkesztése**.

3. Válassza ki a **beállítások** fülre.

4. A bal oldali panelen válassza ki a **speciális** > **általános**, majd válassza ki a **konfigurációs paraméterek** gomb a képernyő jobb alsó részén.

    ![Beállítások lap](./media/site-recovery-how-to-install-linux-master-target/media/image20.png)

    A **konfigurációs paraméterek** beállítás nem érhető el, a gép futása közben. Ahhoz, hogy aktív ezen a lapon, a virtuális gép leállítása.

5. Tekintse meg, hogy a sor **lemez. EnableUUID** már létezik.

    - Ha az érték létezik-e, és értéke **hamis**, módosítsa a beállítást **igaz**. (A értékei nem kis-és nagybetűket.)

    - Ha az érték létezik-e, és értéke **igaz**, jelölje be **Mégse**.

    - Ha az érték nem létezik, válassza ki a **Hozzáadás sor**.

    - A Név oszlopban hozzáadása **lemez. EnableUUID**, majd állítsa be az értéket, és **igaz**.

    ![A rendszer ellenőrzi a e lemezen. EnableUUID már létezik.](./media/site-recovery-how-to-install-linux-master-target/media/image21.png)

#### <a name="disable-kernel-upgrades"></a>Tiltsa le a kernel frissítések

Az Azure Site Recovery fő célkiszolgáló az Ubuntu adott verzióját igényli, győződjön meg arról, hogy a rendszermag-frissítéseket a virtuális géphez van-e tiltva.

Kernel frissítések engedélyezve vannak, ha a rendszeres megjelenjenek a fő célkiszolgáló hibás működését okozhatja.

#### <a name="download-and-install-additional-packages"></a>Töltse le és telepítse a további csomagokat

> [!NOTE]
> Győződjön meg arról, hogy rendelkezik-e internetkapcsolat, letöltése és telepítése további csomagokat. Ha nem rendelkezik internetkapcsolattal, szeretné manuálisan a RPM csomagok keresése és telepítése.

```
apt-get install -y multipath-tools lsscsi python-pyasn1 lvm2 kpartx
```

### <a name="get-the-installer-for-setup"></a>A telepítő a telepítés beolvasása

Ha a fő célkiszolgáló rendelkezik internetkapcsolattal, a következő lépések segítségével töltse le a telepítő. Ellenkező esetben a folyamatkiszolgálót a telepítő másolja, majd telepítés.

#### <a name="download-the-master-target-installation-packages"></a>Töltse le a fő célkiszolgáló telepítőcsomagok

[Töltse le a legfrissebb Linux fő célkiszolgáló telepítése bits](https://aka.ms/latestlinuxmobsvc).

Letöltheti a Linux, írja be:

```
wget https://aka.ms/latestlinuxmobsvc -O latestlinuxmobsvc.tar.gz
```

Győződjön meg arról, hogy töltse le és csomagolja ki a telepítő a kezdőkönyvtárban. Ha Ön csomagolja ki, hogy **/usr/helyi**, majd a telepítés sikertelen lesz.


#### <a name="access-the-installer-from-the-process-server"></a>A telepítő adatok a folyamatkiszolgálótól hozzáférés

1. A folyamatkiszolgáló Ugrás **C:\Program Files (x86) \Microsoft Azure Site Recovery\home\svsystems\pushinstallsvc\repository**.

2. Másolja a szükséges telepítőfájl a folyamatkiszolgálót, és mentse a fájt **latestlinuxmobsvc.tar.gz** a kezdőkönyvtárban.


### <a name="apply-custom-configuration-changes"></a>Egyéni konfigurációs módosítások alkalmazása

Egyéni konfigurációs módosítások alkalmazásához, tegye a következőket:


1. A következő parancsot a bináris untar.
    ```
    tar -zxvf latestlinuxmobsvc.tar.gz
    ```
    ![Képernyőfelvétel a parancs futtatása](./media/site-recovery-how-to-install-linux-master-target/image16.png)

2. A következő parancsot engedélyt.
    ```
    chmod 755 ./ApplyCustomChanges.sh
    ```

3. A következő parancsot a parancsfájl futtatásához.
    ```
    ./ApplyCustomChanges.sh
    ```
> [!NOTE]
> Csak egyszer a parancsfájl futtatása a kiszolgálón. Állítsa le a kiszolgálót. Indítsa újra a kiszolgálót lemez hozzáadása után a következő szakaszban leírtak szerint.

### <a name="add-a-retention-disk-to-the-linux-master-target-virtual-machine"></a>A fő célkiszolgáló Linux virtuális gép egy adatmegőrzési lemez hozzáadása

Adatmegőrzési lemez létrehozásához tegye a következőket:

1. A Linux fő célkiszolgáló virtuális géphez csatolni egy új 1 TB méretű lemezt, és indítsa el a gépet.

2. Használja a **többutas -inden** parancs további az adatmegőrzési lemez többutas Azonosítóját.

    ```
    multipath -ll
    ```
    ![Az adatmegőrzési lemez többutas azonosítója](./media/site-recovery-how-to-install-linux-master-target/media/image22.png)

3. Formázza a meghajtót, és majd létrehozni egy fájlrendszert az új meghajtón.

    ```
    mkfs.ext4 /dev/mapper/<Retention disk's multipath id>
    ```
    ![A meghajtón fájlrendszer létrehozása](./media/site-recovery-how-to-install-linux-master-target/media/image23.png)

4. Miután létrehozta a fájlrendszer, csatlakoztassa az adatmegőrzési lemez.
    ```
    mkdir /mnt/retention
    mount /dev/mapper/<Retention disk's multipath id> /mnt/retention
    ```
    ![Az adatmegőrzési lemez csatlakoztatása](./media/site-recovery-how-to-install-linux-master-target/media/image24.png)

5. Hozzon létre a **fstab** bejegyzés az adatmegőrzési meghajtó csatlakoztatása, a rendszer minden indításakor.
    ```
    vi /etc/fstab
    ```
    Válassza ki **beszúrása** a fájl szerkeszthető. Hozzon létre egy új sort, és helyezze be a következő szöveg. A kijelölt többutas az előző parancs azonosítója alapján többutas Lemezazonosítót szerkesztése.

    **/dev/eseményleképező/ <Retention disks multipath id> /mnt/megőrzési ext4 rw 0 0**

    Válassza ki **Esc**, majd írja be **: wq** (írása, és lépjen ki a) a szerkesztő ablak bezárásához.

### <a name="install-the-master-target"></a>A fő célkiszolgáló telepítése

> [!IMPORTANT]
> A fő célkiszolgáló verziója korábbi, mint a folyamatkiszolgáló és a konfigurációs kiszolgáló verziói vagy azzal egyenlőnek kell lennie. Ha ez a feltétel nem teljesül, a védelem-újrabeállítási sikeres, de a replikálás mindaddig sikertelen.


> [!NOTE]
> A fő célkiszolgáló telepítése előtt ellenőrizze, hogy a **/etc/hosts** fájl a virtuális gépen bejegyzéseit, amelyek a társított összes hálózati adapter IP-címek hozzárendelését a helyi állomásnevével tartalmazza.

1. Másolja a jelszót a **C:\ProgramData\Microsoft Azure hely Recovery\private\connection.passphrase** a konfigurációs kiszolgálón. Majd mentse a fájt **passphrase.txt** ugyanabban a helyi könyvtárban a következő parancs futtatásával:

    ```
    echo <passphrase> >passphrase.txt
    ```
    Példa: 
    
    ```
    echo itUx70I47uxDuUVY >passphrase.txt
    ```

2. Megjegyzés: a konfigurációs kiszolgáló IP-címet. A következő lépésben van szükség.

3. A következő parancsot a fő célkiszolgáló telepítése, és regisztrálja a kiszolgálót a konfigurációs kiszolgáló.

    ```
    ./install -q -d /usr/local/ASR -r MT -v VmWare
    /usr/local/ASR/Vx/bin/UnifiedAgentConfigurator.sh -i <ConfigurationServer IP Address> -P passphrase.txt
    ```

    Példa: 
    
    ```
    /usr/local/ASR/Vx/bin/UnifiedAgentConfigurator.sh -i 104.40.75.37 -P passphrase.txt
    ```

    Várjon, amíg a parancsfájl befejeződik. Ha a fő célkiszolgáló regisztrálása sikeres volt, a fő célkiszolgáló megtalálható a **Site Recovery-infrastruktúra** a portál lapján.


#### <a name="install-the-master-target-by-using-interactive-installation"></a>Interaktív telepítés használatával a fő célkiszolgáló telepítése

1. A következő parancsot a fő célkiszolgáló telepítése. Válassza ki az ügynök szerepkör **fő célkiszolgáló**.

    ```
    ./install
    ```

2. Válassza ki az alapértelmezett helyet a telepítéshez, majd válassza ki **Enter** folytatja.

    ![Fő célkiszolgáló telepítése az alapértelmezett hely kiválasztása](./media/site-recovery-how-to-install-linux-master-target/image17.png)

Miután a telepítés befejeződött, a konfigurációs kiszolgáló regisztrálása a parancssor használatával.

1. Megjegyzés: a konfigurációs kiszolgáló IP-címét. A következő lépésben van szükség.

2. A következő parancsot a fő célkiszolgáló telepítése, és regisztrálja a kiszolgálót a konfigurációs kiszolgáló.

    ```
    ./install -q -d /usr/local/ASR -r MT -v VmWare
    /usr/local/ASR/Vx/bin/UnifiedAgentConfigurator.sh -i <ConfigurationServer IP Address> -P passphrase.txt
    ```
    Példa: 

    ```
    /usr/local/ASR/Vx/bin/UnifiedAgentConfigurator.sh -i 104.40.75.37 -P passphrase.txt
    ```

   Várjon, amíg a parancsfájl befejeződik. Ha a fő célkiszolgáló sikeresen regisztrálva van, a fő célkiszolgáló megtalálható a **Site Recovery-infrastruktúra** a portál lapján.


### <a name="upgrade-the-master-target"></a>A fő célkiszolgáló frissítése

Futtassa a telepítőt. Automatikusan észleli, hogy az ügynök telepítve van-e a fő célkiszolgáló. Frissítéséhez válassza **Y**.  A telepítés befejezése után, a fő célkiszolgálón a következő parancs használatával telepített verziójának ellenőrzése:

    ```
    cat /usr/local/.vx_version
    ```

Láthatja, hogy a **verzió** mező biztosít a fő célkiszolgáló verziószámát.

### <a name="install-vmware-tools-on-the-master-target-server"></a>A VMware-eszközök a fő célkiszolgáló telepítése

Szeretne telepíteni a VMware-eszközök a fő célkiszolgáló, így képes felderíteni, akkor az adattároló. Ha az eszközök nincsenek telepítve, a védelem-újrabeállítási képernyőn nem szerepel az adattároló. A VMware-eszközök telepítése, után újra kell indítania.

## <a name="next-steps"></a>Következő lépések
Miután befejezte a telepítési és a fő célkiszolgáló regisztrálását, megjelenik a fő célkiszolgáló jelennek meg a **fő célkiszolgáló** szakasz **Site Recovery-infrastruktúra**, a konfigurációja Server áttekintése.

Most már folytathatja az [ismételt védelem](site-recovery-how-to-reprotect.md), feladat-visszavétel követ.

## <a name="common-issues"></a>Gyakori problémák

* Győződjön meg arról, hogy ne kapcsolja be az összes felügyeleti összetevőkön, például az olyan fő célkiszolgálót, a Storage vmotion szolgáltatások használata. Ha a fő célkiszolgáló sikeres védelem-újrabeállítási után helyezi át, nem választható le a virtuális gép lemezeinek (VMDKs). Ebben az esetben a feladat-visszavétel sikertelen lesz.

* A fő célkiszolgáló nem kell az esetlegesen pillanatképek a virtuális gépen. Ha a pillanatképek vannak, a feladat-visszavétel sikertelen lesz.

* Bizonyos egyéni hálózati konfigurációk, az egyes ügyfelek, mert a hálózati illesztő le van tiltva, indításakor, és a fő célkiszolgáló ügynöke nem tudja inicializálni. Győződjön meg arról, hogy helyesen vannak-e beállítva a következő tulajdonságokkal. Ellenőrizze, hogy ezeket a tulajdonságokat a Ethernet a kártya fájl /etc/sysconfig/network-scripts/ifcfg-eth *.
    * BOOTPROTO = dhcp
    * ONBOOT = Igen
