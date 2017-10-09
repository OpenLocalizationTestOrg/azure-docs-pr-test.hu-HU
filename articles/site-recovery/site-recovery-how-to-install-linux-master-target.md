---
title: "a Linux-fő célkiszolgáló feladatátvétel az Azure tooon helyszíni aaaHow tooinstall |} Microsoft Docs"
description: "A Linux virtuális gép újbóli védelméhez, előtt kell egy Linux fő célkiszolgáló. Megtudhatja, hogyan tooinstall egyet."
services: site-recovery
documentationcenter: 
author: ruturaj
manager: gauravd
editor: 
ms.assetid: 44813a48-c680-4581-a92e-cecc57cc3b1e
ms.service: site-recovery
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: 
ms.date: 08/11/2017
ms.author: ruturajd
ms.openlocfilehash: d7c55d115712b9862414979f89efb1f177c5f0dd
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="install-a-linux-master-target-server"></a>A Linux fő célkiszolgáló telepítése
Miután a virtuális gépeket nem sikerül, hátsó hello virtuális gépek toohello helyszíni hely sikertelen lehet. a toofail vissza kell tooreprotect hello virtuális gép Azure toohello a helyszíni helyről. Ez a folyamat egy a helyszíni fő célkiszolgáló tooreceive hello forgalom kell. 

Ha a védett virtuális gépet egy Windows rendszerű virtuális gép, majd meg kell olyan Windows fő célkiszolgálót. A Linux virtuális gép van szüksége a Linuxos fő célkiszolgáló. Olvasási hello alábbi lépéseit toolearn hogyan toocreate és a telepítése egy Linux fő cél.

> [!IMPORTANT]
> A fő célkiszolgáló hello 9.10.0 kiadástól kezdve, hello legújabb fő célkiszolgáló csak telepíthető egy Ubuntu 16.04 kiszolgálón. Új telepítések CentOS6.6 kiszolgálók nincsenek engedélyezve. Azonban tovább tooupgrade a régi fő célkiszolgálók hello 9.10.0 verziójával.

## <a name="overview"></a>Áttekintés
Ez a cikk ismerteti hogyan tooinstall egy Linux fő cél.

Megjegyzéseit vagy kérdéseit a cikk vagy hello hello végén utáni [Azure Recovery Services fórumon](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr).

## <a name="prerequisites"></a>Előfeltételek

* toochoose hello gazdagép mely toodeploy-hello fő célkiszolgáló, határozza meg, ha a feladat-visszavétel hello toobe tooan meglévő helyszíni virtuális gép vagy tooa új virtuális gép lesz. 
    * Egy meglévő virtuális gép hello hello fő célkiszolgáló kell rendelkeznie hozzáférés toohello adattárolókhoz hello virtuális gép.
    * Ha hello a helyszíni virtuális gép nem létezik, azonos gazdagép hello fő célként hello hello feladat-visszavételt a virtuális gép létrejön. Választhat ESXi-állomáson tooinstall hello fő célkiszolgálót.
* hello fő célkiszolgálón, amely hello folyamatkiszolgáló és hello konfigurációs kiszolgáló képes kommunikálni a hálózaton kell lennie.
* hello hello fő célkiszolgáló kell lennie egyenlő tooor rendszernél korábbi hello folyamatkiszolgáló és a konfigurációs kiszolgáló hello hello verzióit. Például ha hello hello konfigurációs kiszolgáló verziószáma 9.4, hello fő célkiszolgáló hello verziója lehet 9.4 vagy 9.3, de nem 9,5.
* hello fő célkiszolgáló csak akkor lehet VMware virtuális gépek nem egy fizikai kiszolgálón.

## <a name="create-hello-master-target-according-toohello-sizing-guidelines"></a>Hello fő célkiszolgáló toohello méretezési irányelvek szerint létrehozása

Hozza létre a fő célkiszolgáló hello méretezési útmutató következő hello megfelelően:
- **RAM**: legalább 6 GB
- **Az operációs rendszer lemezméret**: 100 GB vagy több (tooinstall CentOS6.6)
- **További lemezmérete adatmegőrzési meghajtó**: 1 TB-os
- **A Processzormagok**: 4 mag, vagy több

hello következő támogatott Ubuntu kernelek támogatottak.


|Kernel-sorozat  |Túl támogatja |
|---------|---------|
|4.4      |4.4.0-81-Generic         |
|4.8      |4.8.0-56-Generic         |
|4.10     |4.10.0-24-Generic        |


## <a name="deploy-hello-master-target-server"></a>Hello fő célkiszolgáló telepítése

### <a name="install-ubuntu-16042-minimal"></a>Ubuntu 16.04.2 telepítése minimális

A következő hello lépéseket tooinstall hello Ubuntu 16.04.2 64 bites operációs rendszer hello igénybe vehet.

**1. lépés:** toohello Ugrás [letöltése hivatkozásra](https://www.ubuntu.com/download/server/thank-you?version=16.04.2&architecture=amd64) hello legközelebbi tükör válassza az Ubuntu 16.04.2 minimális 64 bites ISO-Lemezképet töltse le, amely.

Ubuntu 16.04.2 minimális 64 bites ISO-Lemezképet ne hello DVD-meghajtó, és indítsa el a hello rendszer.

**2. lépés:** válasszon **angol** a kívánt nyelvet, és válassza ki, **Enter**.

![Válasszon nyelvet](./media/site-recovery-how-to-install-linux-master-target/ubuntu/image1.png)

**3. lépés:** válasszon **Ubuntu Server telepítése**, majd válassza ki **Enter**.

![Válassza ki a telepítés Ubuntu Server](./media/site-recovery-how-to-install-linux-master-target/ubuntu/image2.png)

**4. lépés:** válasszon **angol** a kívánt nyelvet, és válassza ki, **Enter**.

![Jelölje ki a kívánt nyelvet angol nyelven](./media/site-recovery-how-to-install-linux-master-target/ubuntu/image3.png)

**5. lépés:** válassza ki a megfelelő lehetőséget a hello hello **időzóna** beállítások listáját, és válassza **Enter**.

![Válassza ki a megfelelő időzóna hello](./media/site-recovery-how-to-install-linux-master-target/ubuntu/image4.png)

**6. lépés:** válasszon **nem** (hello az alapértelmezett beállítás), majd válassza ki **Enter**.


![Hello billentyűzet konfigurálása](./media/site-recovery-how-to-install-linux-master-target/ubuntu/image5.png)

**7. lépés:** válasszon **angol (amerikai)** , hello hello billentyűzet származási országot, és adja **Enter**.

![USA hello származási kiválasztása](./media/site-recovery-how-to-install-linux-master-target/ubuntu/image6.png)

**8. lépés:** válasszon **angol (amerikai)** hello billentyűzetkiosztás, és válassza ki, **Enter**.

![Jelölje ki angol hello billentyűzetkiosztás módosítása](./media/site-recovery-how-to-install-linux-master-target/ubuntu/image7.png)

**9. lépés:** hello állomásnév megadása a kiszolgáló hello **állomásnév** mezőbe, majd válassza ki **Folytatás**.

![Adja meg a kiszolgáló hello állomásnév](./media/site-recovery-how-to-install-linux-master-target/ubuntu/image8.png)

**10. lépés:** toocreate egy felhasználói fiókot adjon meg hello felhasználónevet, és válassza **Folytatás**.

![Felhasználói fiók létrehozása](./media/site-recovery-how-to-install-linux-master-target/ubuntu/image9.png)

**11. lépés:** hello jelszó hello új felhasználói fiókhoz, majd válassza ki **Folytatás**.

![Adja meg a hello jelszó](./media/site-recovery-how-to-install-linux-master-target/ubuntu/image10.png)

**12. lépés:** hello új felhasználó hello jelszót, majd válassza ki **Folytatás**.

![Hello jelszó megerősítése](./media/site-recovery-how-to-install-linux-master-target/ubuntu/image11.png)

**13. lépés:** válasszon **nem** (hello az alapértelmezett beállítás), majd válassza ki **Enter**.

![Felhasználók és a jelszavak beállítása](./media/site-recovery-how-to-install-linux-master-target/ubuntu/image12.png)

**14. lépés:** Ha hello időzóna megjelenített helyes, válassza ki a **Igen** (hello az alapértelmezett beállítás), majd válassza ki **Enter**.

tooreconfigure az időzónát, jelölje be **nem**.

![Hello óra konfigurálása](./media/site-recovery-how-to-install-linux-master-target/ubuntu/image13.png)

**15. lépés:** hello metódus beállítások particionálás, válassza ki **interaktív - használja a teljes lemez**, majd válassza ki **Enter**.

![Hello particionálás módszer kiválasztása](./media/site-recovery-how-to-install-linux-master-target/ubuntu/image14.png)

**16. lépés:** válassza ki a megfelelő lemez a hello hello **válassza ki a lemez toopartition** beállítások, és válassza **Enter**.


![Válassza ki a hello lemez](./media/site-recovery-how-to-install-linux-master-target/ubuntu/image15.png)

**17. lépés:** válasszon **Igen** toowrite hello módosítások toodisk, és válassza **Enter**.

![Hello módosítások toodisk írása](./media/site-recovery-how-to-install-linux-master-target/ubuntu/image16.png)

**18. lépés:** hello alapértelmezett beállítást, jelölje be **Folytatás**, majd válassza ki **Enter**.

![Válassza ki a hello alapértelmezett beállítás](./media/site-recovery-how-to-install-linux-master-target/ubuntu/image17.png)

**19. lépést:** hello megfelelő beállítást a rendszer a frissítések kezelése, majd válassza ki és **Enter**.

![Válassza ki, hogyan toomanage frissíti](./media/site-recovery-how-to-install-linux-master-target/ubuntu/image18.png)

> [!WARNING]
> Mivel hello Azure Site Recovery fő célkiszolgáló hello Ubuntu olyan speciális verziója szükséges, meg kell tooensure adott hello kernel frissítések le vannak tiltva hello virtuális géphez. Ha engedélyezve vannak, a rendszeres megjelenjenek hello fő server toomalfunction miatt. Győződjön meg arról, hogy hello **automatikus frissítések** lehetőséget.


**20. lépés:** válassza ki az alapértelmezett beállításokat. Ha openSSH az SSH csatlakozni, jelölje be a hello **OpenSSH server** lehetőséget, majd válassza ki **Folytatás**.

![Válassza ki a szoftver](./media/site-recovery-how-to-install-linux-master-target/ubuntu/image19.png)

**21. lépés:** válasszon **Igen**, majd válassza ki **Enter**.

![Telepítés céljaként hello LÁRVAJÁRAT rendszertöltő](./media/site-recovery-how-to-install-linux-master-target/ubuntu/image20.png)

**22. lépés:** válassza ki a megfelelő eszköz hello rendszerindító betöltő telepítéshez hello (lehetőleg **/dev/sda**), majd válassza ki **Enter**.

![Jelöljön ki egy eszközt a rendszerindító betöltő telepítéshez](./media/site-recovery-how-to-install-linux-master-target/ubuntu/image21.png)

**23. lépés:** kiválasztása **Folytatás**, majd válassza ki **Enter** toofinish hello telepítési.

![Hello telepítés befejezése](./media/site-recovery-how-to-install-linux-master-target/ubuntu/image22.png)

Miután hello telepítés befejeződött, jelentkezzen be toohello VM hello új felhasználói hitelesítő adatokkal. (Lásd túl**lépés 10** további információt.)

Lépéseket hello a következő képernyőkép tooset hello legfelső szintű hello ismertetett felhasználói jelszavát. Legfelső szintű felhasználóként, majd jelentkezzen be.

![Hello legfelső szintű felhasználói jelszó beállítása](./media/site-recovery-how-to-install-linux-master-target/ubuntu/image23.png)


### <a name="prepare-hello-machine-for-configuration-as-a-master-target-server"></a>Konfiguráció hello gép előkészítése a fő célkiszolgáló-kiszolgálóként
A következő előkészíteni a hello gép konfigurációjához, mivel a fő célkiszolgálón.

minden SCSI merevlemez egy Linux virtuális gép tooget hello azonosító engedélyezése hello **lemez. EnableUUID = TRUE** paraméter.

Ez a paraméter, hajtsa végre a megfelelő hello a következő lépések tooenable:

1. Állítsa le a virtuális gépet.

2. Kattintson a jobb gombbal a hello bejegyzés hello virtuális gép hello bal oldali ablaktáblán, majd válassza ki **beállításainak szerkesztése**.

3. Jelölje be hello **beállítások** fülre.

4. Hello bal oldali ablaktáblában jelöljön ki **speciális** > **általános**, majd válassza ki a hello **konfigurációs paraméterek** hello képernyő jobb alsó részén hello gombjára.

    ![Beállítások lap](./media/site-recovery-how-to-install-linux-master-target/media/image20.png)

    Hello **konfigurációs paraméterek** , nincs lehetőség hello gép futása közben. toomake ezen a lapon aktív, hello virtuális gép leállítása.

5. Tekintse meg, hogy a sor **lemez. EnableUUID** már létezik.

    - Ha hello érték létezik-e, és értéke túl**hamis**, módosítsa hello túl**igaz**. (hello értékei nem kis-és nagybetűket.)

    - Ha hello érték létezik-e, és értéke túl**igaz**, jelölje be **Mégse**.

    - Ha hello érték nem létezik, válassza ki a **Hozzáadás sor**.

    - Adja hozzá a hello neve oszlopban **lemez. EnableUUID**, és utána állítsa be hello érték túl**igaz**.

    ![A rendszer ellenőrzi a e lemezen. EnableUUID már létezik.](./media/site-recovery-how-to-install-linux-master-target/media/image21.png)

#### <a name="disable-kernel-upgrades"></a>Tiltsa le a kernel frissítések

Az Azure Site Recovery fő célkiszolgáló hello Ubuntu olyan speciális verziója szükséges, győződjön meg arról, hogy hello kernel frissítések hello virtuális géphez van-e tiltva.

Ha engedélyezve van a rendszermag-frissítések, a rendszeres megjelenjenek hello fő server toomalfunction miatt.

#### <a name="download-and-install-additional-packages"></a>Töltse le és telepítse a további csomagokat

> [!NOTE]
> Győződjön meg arról, hogy az Internet kapcsolat toodownload, és további csomagok telepítése. Ha nem rendelkezik internetkapcsolattal, szükség van-e toomanually keresse meg a RPM csomagok és a telepítést.

```
apt-get install -y multipath-tools lsscsi python-pyasn1 lvm2 kpartx
```

### <a name="get-hello-installer-for-setup"></a>A telepítő hello telepítő beolvasása

Ha a fő célkiszolgáló rendelkezik internetkapcsolattal, a következő lépéseket toodownload hello telepítő hello is használhatja. Ellenkező esetben másolja hello telepítő hello folyamat kiszolgálóról, majd telepítés.

#### <a name="download-hello-master-target-installation-packages"></a>Töltse le a hello fő telepítőcsomagok

[Töltse le a hello legújabb Linux fő célkiszolgáló telepítése bits](https://aka.ms/latestlinuxmobsvc).

toodownload, Linux, típus használatával:

```
wget https://aka.ms/latestlinuxmobsvc -O latestlinuxmobsvc.tar.gz
```

Győződjön meg arról, hogy töltse le és csomagolja ki a hello installer a kezdőkönyvtárban. Ha túl csomagolja ki**/usr/helyi**, majd hello telepítése nem sikerül.


#### <a name="access-hello-installer-from-hello-process-server"></a>Hozzáférés hello telepítő hello folyamat kiszolgálóról

1. Hello folyamatkiszolgáló, nyissa meg túl**C:\Program Files (x86) \Microsoft Azure Site Recovery\home\svsystems\pushinstallsvc\repository**.

2. Hello szükséges installer-fájl másolása hello folyamatkiszolgáló, és mentse a fájt **latestlinuxmobsvc.tar.gz** a kezdőkönyvtárban.


### <a name="apply-custom-configuration-changes"></a>Egyéni konfigurációs módosítások alkalmazása

tooapply egyéni konfigurációs módosításokat, a lépéseket követve hello használata:


1. Futtassa a következő parancs toountar hello bináris hello.
    ```
    tar -zxvf latestlinuxmobsvc.tar.gz
    ```
    ![Képernyőfelvétel a hello parancs toorun](./media/site-recovery-how-to-install-linux-master-target/image16.png)

2. Futtassa a következő parancs toogive engedély hello.
    ```
    chmod 755 ./ApplyCustomChanges.sh
    ```

3. Futtassa a következő parancsfájl toorun hello hello.
    ```
    ./ApplyCustomChanges.sh
    ```
> [!NOTE]
> Csak egyszer hello parancsfájl hello kiszolgálón fut. Hello kiszolgáló leállítása. Indítsa újra hello server lemez hozzáadása után hello a következő szakaszban leírtak szerint.

### <a name="add-a-retention-disk-toohello-linux-master-target-virtual-machine"></a>Adatmegőrzési lemez toohello Linux fő célkiszolgáló virtuális gépek hozzáadása

A következő lépéseket toocreate adatmegőrzési lemez hello használata:

1. Csatlakoztassa az új 1 TB méretű lemez toohello Linux fő célkiszolgáló virtuális gép, és indítsa el a hello gép.

2. Használjon hello **többutas -inden** parancsazonosító toolearn hello többutas hello adatmegőrzési lemez.

    ```
    multipath -ll
    ```
    ![hello adatmegőrzési lemez hello többutas azonosítója](./media/site-recovery-how-to-install-linux-master-target/media/image22.png)

3. Formázza hello meghajtót, és ezután hozzon létre egy fájlrendszer hello új meghajtón.

    ```
    mkfs.ext4 /dev/mapper/<Retention disk's multipath id>
    ```
    ![Fájlrendszer hello meghajtón létrehozása](./media/site-recovery-how-to-install-linux-master-target/media/image23.png)

4. Miután létrehozta a hello fájlrendszer, csatlakoztassa a hello megőrzési lemezt.
    ```
    mkdir /mnt/retention
    mount /dev/mapper/<Retention disk's multipath id> /mnt/retention
    ```
    ![Csatlakoztatását a hello adatmegőrzési lemez](./media/site-recovery-how-to-install-linux-master-target/media/image24.png)

5. Hozzon létre hello **fstab** bejegyzés toomount hello adatmegőrzési meghajtó hello rendszer minden indításakor.
    ```
    vi /etc/fstab
    ```
    Válassza ki **beszúrása** toobegin hello fájl szerkesztése. Hozzon létre egy új sort, és helyezze be a következő szöveg hello. Hello többutas Lemezazonosítót kiemelt hello többutas azonosító hello előző parancs alapján szerkesztheti.

     **/dev/eseményleképező/ <Retention disks multipath id> /mnt/megőrzési ext4 rw 0 0**

    Válassza ki **Esc**, majd írja be **: wq** (írása, és lépjen ki a) tooclose hello szerkesztő ablakot.

### <a name="install-hello-master-target"></a>Hello fő célkiszolgáló telepítése

> [!IMPORTANT]
> hello hello fő célkiszolgáló kell lennie egyenlő tooor rendszernél korábbi hello folyamatkiszolgáló és a konfigurációs kiszolgáló hello hello verzióit. Ha ez a feltétel nem teljesül, a védelem-újrabeállítási sikeres, de a replikálás mindaddig sikertelen.


> [!NOTE]
> Hello fő célkiszolgáló telepítése előtt ellenőrizze, hogy hello **/etc/hosts** hello virtuálisgép-fájlt tartalmaz bejegyzéseit, amelyek leképezése hello helyi állomásnevével toohello IP-címek társított összes hálózati adapter.

1. Másolja a hello jelszót **C:\ProgramData\Microsoft Azure hely Recovery\private\connection.passphrase** hello konfigurációs kiszolgálón. Majd mentse a fájt **passphrase.txt** hello futtatásával helyi könyvtárába hello a következő parancsot:

    ```
    echo <passphrase> >passphrase.txt
    ```
    Példa: 
    
    ```
    echo itUx70I47uxDuUVY >passphrase.txt
    ```

2. Vegye figyelembe a hello konfigurációs kiszolgáló IP-címét. Esetleg szükség lenne rá hello következő lépésben.

3. Futtassa a következő parancs tooinstall hello fő célkiszolgáló hello és hello kiszolgáló regisztrálása hello konfigurációs kiszolgálóval.

    ```
    ./install -q -d /usr/local/ASR -r MT -v VmWare
    /usr/local/ASR/Vx/bin/UnifiedAgentConfigurator.sh -i <ConfigurationServer IP Address> -P passphrase.txt
    ```

    Példa: 
    
    ```
    /usr/local/ASR/Vx/bin/UnifiedAgentConfigurator.sh -i 104.40.75.37 -P passphrase.txt
    ```

    Várjon, amíg hello parancsfájl befejeződik. Ha hello fő célkiszolgáló regisztrálása sikeres volt, a fő célkiszolgáló hello szerepel-e a hello **Site Recovery-infrastruktúra** hello portál lapján.


#### <a name="install-hello-master-target-by-using-interactive-installation"></a>Interaktív telepítés használatával hello fő célkiszolgáló telepítése

1. Futtassa a következő parancs tooinstall hello fő célkiszolgáló hello. Hello ügynök szerepkör, válasszon **fő célkiszolgáló**.

    ```
    ./install
    ```

2. Válasszon telepítési hello alapértelmezett elérési utat, majd válassza ki **Enter** toocontinue.

    ![Fő célkiszolgáló telepítése az alapértelmezett hely kiválasztása](./media/site-recovery-how-to-install-linux-master-target/image17.png)

Hello telepítés befejeződését követően hello konfigurációs kiszolgáló regisztrálása a hello parancssor használatával.

1. Vegye figyelembe a hello hello konfigurációs kiszolgáló IP-címét. Esetleg szükség lenne rá hello következő lépésben.

2. Futtassa a következő parancs tooinstall hello fő célkiszolgáló hello és hello kiszolgáló regisztrálása hello konfigurációs kiszolgálóval.

    ```
    ./install -q -d /usr/local/ASR -r MT -v VmWare
    /usr/local/ASR/Vx/bin/UnifiedAgentConfigurator.sh -i <ConfigurationServer IP Address> -P passphrase.txt
    ```
    Példa: 

    ```
    /usr/local/ASR/Vx/bin/UnifiedAgentConfigurator.sh -i 104.40.75.37 -P passphrase.txt
    ```

   Várjon, amíg hello parancsfájl befejeződik. Ha hello fő célkiszolgáló sikeresen regisztrált, a fő célkiszolgáló hello szerepel-e a hello **Site Recovery-infrastruktúra** hello portál lapján.


### <a name="upgrade-hello-master-target"></a>A fő célkiszolgáló hello frissítése

Hello telepítő futtatásához. Automatikusan észleli, hogy hello ügynök telepítve van a hello fő célkiszolgálón. Jelölje be tooupgrade **Y**.  Hello telepítő befejezése után ellenőrizze a hello hello fő célkiszolgáló hello a következő parancs használatával telepített verzióját.

    ```
    cat /usr/local/.vx_version
    ```

Láthatja, hogy hello **verzió** mező ad hello fő célkiszolgáló hello verziószámát.

### <a name="install-vmware-tools-on-hello-master-target-server"></a>A VMware-eszközök hello fő célkiszolgáló telepítése

Tooinstall VMware-eszközök hello fő célkiszolgáló kell, hogy azt felderíthesse hello adattárolókhoz. Hello eszközök telepítése nem történik meg, ha hello védelem-újrabeállítási képernyőn nem szerepel a listában a hello adattárolókhoz. Hello VMware-eszközök telepítését, miután toorestart kell.

## <a name="next-steps"></a>Következő lépések
Hello telepítése és regisztrálása a hello fő célkiszolgáló rendelkezik finsihed, miután hello fő célkiszolgáló hello megjelenő láthatja **fő célkiszolgáló** szakasz **Site Recovery-infrastruktúra**, hello alatt konfigurációs kiszolgáló áttekintése.

Most már folytathatja az [ismételt védelem](site-recovery-how-to-reprotect.md), feladat-visszavétel követ.

## <a name="common-issues"></a>Gyakori problémák

* Győződjön meg arról, hogy ne kapcsolja be az összes felügyeleti összetevőkön, például az olyan fő célkiszolgálót, a Storage vmotion szolgáltatások használata. Ha hello fő célkiszolgáló sikeres védelem-újrabeállítási után helyezi át, nem lehet leválasztani hello virtuálisgép-lemezeket (VMDKs). Ebben az esetben a feladat-visszavétel sikertelen lesz.

* hello fő célkiszolgáló nem kell meglévő pillanatképeket hello virtuális gépen. Ha a pillanatképek vannak, a feladat-visszavétel sikertelen lesz.

* Miatt toosome egyéni hálózati konfigurációját egyes ügyfelek hello hálózati illesztő a rendszerindítás során le van tiltva, és hello fő célkiszolgáló ügynöke nem lehet inicializálni. Győződjön meg arról, hogy hello következő tulajdonságai megfelelően vannak beállítva. Ellenőrizze, hogy ezeket a tulajdonságokat a hello Ethernet kártya fájl /etc/sysconfig/network-scripts/ifcfg-eth *.
    * BOOTPROTO = dhcp
    * ONBOOT = Igen
