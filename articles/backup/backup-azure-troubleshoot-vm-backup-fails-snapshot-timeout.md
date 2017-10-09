---
title: "Azure biztonsági mentési hiba elhárítása: Vendég ügynök állapota nem érhető el |} Microsoft Docs"
description: "A jelenség, okait és az Azure biztonsági mentési hibák kapcsolódó tooerror megoldások: nem tudott kommunikálni a hello Virtuálisgép-ügynök"
services: backup
documentationcenter: 
author: genlin
manager: cshepard
editor: 
keywords: "Az Azure backup; Virtuálisgép-ügynök; Hálózati kapcsolat;"
ms.assetid: 4b02ffa4-c48e-45f6-8363-73d536be4639
ms.service: backup
ms.workload: storage-backup-recovery
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/17/2017
ms.author: genli;markgal;
ms.openlocfilehash: 724c61ba80d0a9ef91a5f8543ae72bb86968881b
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="troubleshoot-azure-backup-failure-issues-with-agent-andor-extension"></a>Azure biztonsági mentési hiba elhárítása: ügynök és/vagy kiterjesztés problémái

Ez a cikk ismerteti a biztonsági mentési hibák megoldása hibaelhárítási lépéseket toohelp kapcsolódó tooproblems ügynök és Virtuálisgép-bővítmény kommunikál.

[!INCLUDE [support-disclaimer](../../includes/support-disclaimer.md)]

## <a name="vm-agent-unable-toocommunicate-with-azure-backup"></a>Virtuálisgép-ügynök nem toocommunicate az Azure Backup szolgáltatással
Miután regisztrálja, és egy virtuális Gépet a hello Azure Backup szolgáltatás ütemezése, a biztonsági mentés kezdeményezi a hello feladat úgy hello VM ügynök tootake időpontban pillanatkép folytatott kommunikáció. Hello a következő feltételek bármelyike hello pillanatfelvételét elindul, ami viszont tooBackup hiba előfordulhat. Kövesse az alábbi lépéseket a megadott sorrendben hello, majd próbálja megismételni a műveletet.
##### <a name="cause-1-hello-vm-has-no-internet-accessthe-vm-has-no-internet-access"></a>1. ok: [hello virtuális gép nincs Internet-hozzáféréssel rendelkező](#the-vm-has-no-internet-access)
##### <a name="cause-2-hello-agent-is-installed-in-hello-vm-but-is-unresponsive-for-windows-vmsthe-agent-installed-in-the-vm-but-unresponsive-for-windows-vms"></a>2. ok: [hello ügynök telepítve van a virtuális gép hello, de nem válaszol a (virtuális gépekhez Windows)](#the-agent-installed-in-the-vm-but-unresponsive-for-windows-vms)
##### <a name="cause-3-hello-agent-installed-in-hello-vm-is-out-of-date-for-linux-vmsthe-agent-installed-in-the-vm-is-out-of-date-for-linux-vms"></a>3. ok: [hello ügynök van telepítve, a virtuális gép hello elavult (a Linux virtuális gépek)](#the-agent-installed-in-the-vm-is-out-of-date-for-linux-vms)
##### <a name="cause-4-hello-snapshot-status-cannot-be-retrieved-or-a-snapshot-cannot-be-takenthe-snapshot-status-cannot-be-retrieved-or-a-snapshot-cannot-be-taken"></a>4. ok: [hello pillanatkép állapotát nem sikerült beolvasni vagy pillanatkép nem végezhető.](#the-snapshot-status-cannot-be-retrieved-or-a-snapshot-cannot-be-taken)
##### <a name="cause-5-hello-backup-extension-fails-tooupdate-or-loadthe-backup-extension-fails-to-update-or-load"></a>5. ok: [hello tartalék mellék tooupdate vagy betöltése sikertelen](#the-backup-extension-fails-to-update-or-load)

## <a name="snapshot-operation-failed-due-toono-network-connectivity-on-hello-virtual-machine"></a>Pillanatkép művelet hello virtuális gépen toono hálózati kapcsolat miatt nem sikerült
Miután regisztrálja, és egy virtuális Gépet a hello Azure Backup szolgáltatás ütemezése, a biztonsági mentés kezdeményezi a hello feladat úgy hello VM tartalék mellék tootake egy pont időponthoz kötött pillanatképet kommunikál. Hello a következő feltételek bármelyike hello pillanatfelvételét elindul, ami viszont tooBackup hiba előfordulhat. Kövesse az alábbi lépéseket a megadott sorrendben hello, majd próbálja megismételni a műveletet.
##### <a name="cause-1-hello-vm-has-no-internet-accessthe-vm-has-no-internet-access"></a>1. ok: [hello virtuális gép nincs Internet-hozzáféréssel rendelkező](#the-vm-has-no-internet-access)
##### <a name="cause-2-hello-snapshot-status-cannot-be-retrieved-or-a-snapshot-cannot-be-takenthe-snapshot-status-cannot-be-retrieved-or-a-snapshot-cannot-be-taken"></a>2. ok: [hello pillanatkép állapotát nem sikerült beolvasni vagy pillanatkép nem végezhető.](#the-snapshot-status-cannot-be-retrieved-or-a-snapshot-cannot-be-taken)
##### <a name="cause-3-hello-backup-extension-fails-tooupdate-or-loadthe-backup-extension-fails-to-update-or-load"></a>3. ok: [hello tartalék mellék tooupdate vagy betöltése sikertelen](#the-backup-extension-fails-to-update-or-load)

## <a name="vmsnapshot-extension-operation-failed"></a>VMSnapshot bővítmény művelete sikertelen volt

Miután regisztrálja, és egy virtuális Gépet a hello Azure Backup szolgáltatás ütemezése, a biztonsági mentés kezdeményezi a hello feladat úgy hello VM tartalék mellék tootake egy pont időponthoz kötött pillanatképet kommunikál. Hello a következő feltételek bármelyike hello pillanatfelvételét elindul, ami viszont tooBackup hiba előfordulhat. Kövesse az alábbi lépéseket a megadott sorrendben hello, majd próbálja megismételni a műveletet.
##### <a name="cause-1-hello-snapshot-status-cannot-be-retrieved-or-a-snapshot-cannot-be-takenthe-snapshot-status-cannot-be-retrieved-or-a-snapshot-cannot-be-taken"></a>1. ok: [hello pillanatkép állapotát nem sikerült beolvasni vagy pillanatkép nem végezhető.](#the-snapshot-status-cannot-be-retrieved-or-a-snapshot-cannot-be-taken)
##### <a name="cause-2-hello-backup-extension-fails-tooupdate-or-loadthe-backup-extension-fails-to-update-or-load"></a>2. ok: [hello tartalék mellék tooupdate vagy betöltése sikertelen](#the-backup-extension-fails-to-update-or-load)
##### <a name="cause-3-hello-vm-has-no-internet-accessthe-vm-has-no-internet-access"></a>3. ok: [hello virtuális gép nincs Internet-hozzáféréssel rendelkező](#the-vm-has-no-internet-access)
##### <a name="cause-4-hello-agent-is-installed-in-hello-vm-but-is-unresponsive-for-windows-vmsthe-agent-installed-in-the-vm-but-unresponsive-for-windows-vms"></a>4. ok: [hello ügynök telepítve van a virtuális gép hello, de nem válaszol a (virtuális gépekhez Windows)](#the-agent-installed-in-the-vm-but-unresponsive-for-windows-vms)
##### <a name="cause-5-hello-agent-installed-in-hello-vm-is-out-of-date-for-linux-vmsthe-agent-installed-in-the-vm-is-out-of-date-for-linux-vms"></a>5. ok: [hello ügynök van telepítve, a virtuális gép hello elavult (a Linux virtuális gépek)](#the-agent-installed-in-the-vm-is-out-of-date-for-linux-vms)

## <a name="unable-tooperform-hello-operation-as-hello-vm-agent-is-not-responsive"></a>Nem lehet tooperform hello műveletet, mert hello Virtuálisgép-ügynök nincs rugalmas

Miután regisztrálja, és egy virtuális Gépet a hello Azure Backup szolgáltatás ütemezése, a biztonsági mentés kezdeményezi a hello feladat úgy hello VM tartalék mellék tootake egy pont időponthoz kötött pillanatképet kommunikál. Hello a következő feltételek bármelyike hello pillanatfelvételét elindul, ami viszont tooBackup hiba előfordulhat. Kövesse az alábbi lépéseket a megadott sorrendben hello, majd próbálja megismételni a műveletet.
##### <a name="cause-1-hello-agent-is-installed-in-hello-vm-but-is-unresponsive-for-windows-vmsthe-agent-installed-in-the-vm-but-unresponsive-for-windows-vms"></a>1. ok: [hello ügynök telepítve van a virtuális gép hello, de nem válaszol a (virtuális gépekhez Windows)](#the-agent-installed-in-the-vm-but-unresponsive-for-windows-vms)
##### <a name="cause-2-hello-agent-installed-in-hello-vm-is-out-of-date-for-linux-vmsthe-agent-installed-in-the-vm-is-out-of-date-for-linux-vms"></a>2. ok: [hello ügynök van telepítve, a virtuális gép hello elavult (a Linux virtuális gépek)](#the-agent-installed-in-the-vm-is-out-of-date-for-linux-vms)
##### <a name="cause-3-hello-vm-has-no-internet-accessthe-vm-has-no-internet-access"></a>3. ok: [hello virtuális gép nincs Internet-hozzáféréssel rendelkező](#the-vm-has-no-internet-access)

## <a name="backup-failed-with-an-internal-error---please-retry-hello-operation-in-a-few-minutes"></a>Biztonsági mentés egy belső hiba miatt nem sikerült – hello művelet néhány perc múlva próbálkozzon újra

Miután regisztrálja, és egy virtuális Gépet a hello Azure Backup szolgáltatás ütemezése, a biztonsági mentés kezdeményezi a hello feladat úgy hello VM tartalék mellék tootake egy pont időponthoz kötött pillanatképet kommunikál. Hello a következő feltételek bármelyike hello pillanatfelvételét elindul, ami viszont tooBackup hiba előfordulhat. Kövesse az alábbi lépéseket a megadott sorrendben hello, majd próbálja megismételni a műveletet.
##### <a name="cause-1-hello-vm-has-no-internet-accessthe-vm-has-no-internet-access"></a>1. ok: [hello virtuális gép nincs Internet-hozzáféréssel rendelkező](#the-vm-has-no-internet-access)
##### <a name="cause-2-hello-agent-installed-in-hello-vm-but-unresponsive-for-windows-vmsthe-agent-installed-in-the-vm-but-unresponsive-for-windows-vms"></a>2. ok: [hello-ügynök van telepítve a virtuális gép hello azonban nem válaszoló (a Windows-alapú virtuális gépek)](#the-agent-installed-in-the-vm-but-unresponsive-for-windows-vms)
##### <a name="cause-3-hello-agent-installed-in-hello-vm-is-out-of-date-for-linux-vmsthe-agent-installed-in-the-vm-is-out-of-date-for-linux-vms"></a>3. ok: [hello ügynök van telepítve, a virtuális gép hello elavult (a Linux virtuális gépek)](#the-agent-installed-in-the-vm-is-out-of-date-for-linux-vms)
##### <a name="cause-4-hello-snapshot-status-cannot-be-retrieved-or-a-snapshot-cannot-be-takenthe-snapshot-status-cannot-be-retrieved-or-a-snapshot-cannot-be-taken"></a>4. ok: [hello pillanatkép állapotát nem sikerült beolvasni vagy pillanatkép nem végezhető.](#the-snapshot-status-cannot-be-retrieved-or-a-snapshot-cannot-be-taken)
##### <a name="cause-5-hello-backup-extension-fails-tooupdate-or-loadthe-backup-extension-fails-to-update-or-load"></a>5. ok: [hello tartalék mellék tooupdate vagy betöltése sikertelen](#the-backup-extension-fails-to-update-or-load)


## <a name="causes-and-solutions"></a>Okait és megoldásait

### <a name="hello-vm-has-no-internet-access"></a>virtuális gép hello nincs Internet-hozzáféréssel rendelkező
Hello üzembe helyezése: követelményeknek hello virtuális gép nincs Internet-hozzáféréssel rendelkező, vagy helyben korlátozásokat, amelyek megakadályozzák a hozzáférés toohello Azure-infrastruktúra rendelkezik.

helytelenül lett a toofunction hello biztonságimásolat-bővítményhez olyan kapcsolat toohello Azure nyilvános IP-címek. hello bővítmény küld parancsokat tooan Azure Storage végpontot (HTTP URL-cím) toomanage hello pillanatképek a virtuális gép hello. Ha hello bővítményt nélküli hozzáférés toohello nyilvános Internet, végül a biztonsági mentés sikertelen lesz.

####  <a name="solution"></a>Megoldás
tooresolve hello problémát, próbálja az itt felsorolt hello módszerek.
##### <a name="allow-access-toohello-azure-datacenter-ip-ranges"></a>Engedélyezi a hozzáférést toohello Azure datacenter IP-címtartományok

1. Szerezze be a hello [Azure datacenter IP-címek listája](https://www.microsoft.com/download/details.aspx?id=41653) tooallow elérésére.
2. IP-címek hello feloldása hello futtatásával **New-NetRoute** parancsmag hello Azure virtuális gép, egy emelt szintű PowerShell-ablakban. Hello parancsmag futtatása rendszergazdaként.
3. tooallow hozzáférés toohello IP-címek, szabályok toohello hálózati biztonsági csoport hozzáadása, ha rendelkezik ilyennel.

##### <a name="create-a-path-for-http-traffic-tooflow"></a>A HTTP-forgalom tooflow elérési utat hoz létre

1. Ha korlátozásait a hálózati hely (például a hálózati biztonsági csoport) található, telepíthet egy HTTP-proxy kiszolgáló tooroute hello forgalom.
2. tooallow hozzáférés toohello Internet hello HTTP-proxy kiszolgáló, szabályok toohello hálózati biztonsági csoport hozzáadása, ha rendelkezik ilyennel.

Hogyan tooset egy HTTP-proxy a virtuális gép biztonsági mentése: toolearn [készítse elő a környezetet tooback Azure virtuális gépek](backup-azure-vms-prepare.md#using-an-http-proxy-for-vm-backups).

Abban az esetben felügyelt lemezt használ, szükség lehet egy további port (8443) megnyitása a hello tűzfalak.

### <a name="hello-agent-installed-in-hello-vm-but-unresponsive-for-windows-vms"></a>hello-ügynök van telepítve a virtuális gép hello azonban nem válaszoló (a Windows-alapú virtuális gépek)

#### <a name="solution"></a>Megoldás
Virtuálisgép-ügynök hello előfordulhat, hogy sérült, vagy hello szolgáltatás előfordulhat, hogy le lett állítva. Hello VM ügynök újbóli telepítése segítene hello legújabb verzióra, és indítsa újra a hello kommunikációt.

1. Ellenőrizze, hogy a Windows vendég ügynökszolgáltatás (services.msc) szolgáltatást futtató virtuális gép hello. Próbálja meg hello Windows vendég ügynök szolgáltatás újraindításához, majd biztonsági mentés hello kezdeményezni<br>
2. Ha nem látható a szolgáltatások, ellenőrizze a programok és szolgáltatások, hogy Windows vendég ügynökszolgáltatás telepítve van.
4. Ha a programok és szolgáltatások eltávolítása hello Windows Vendégügynök képes tooview.
5. Töltse le és telepítse a hello [ügynök MSI legfrissebb verzióját](http://go.microsoft.com/fwlink/?LinkID=394789&clcid=0x409). Rendszergazdai jogosultságok toocomplete hello telepítési van szüksége.
6. Majd a szolgáltatások képesek tooview Windows Vendégügynök szolgáltatásaira kell lennie
7. Próbálja meg futtatni egy a-igény szerinti/ad hoc biztonsági mentés most elemre kattintva "biztonsági másolat" hello portálon.

Emellett ellenőrizze a virtuális gép rendelkezik  **[hello rendszeren telepítve a .NET 4.5](https://docs.microsoft.com/en-us/dotnet/framework/migration-guide/how-to-determine-which-versions-are-installed)**. Szükséges, hogy az hello VM ügynök toocommunicate hello szolgáltatásban

### <a name="hello-agent-installed-in-hello-vm-is-out-of-date-for-linux-vms"></a>hello ügynök van telepítve, a virtuális gép hello elavult (a Linux virtuális gépek)

#### <a name="solution"></a>Megoldás
Legtöbb ügynök vagy bővítmény kapcsolódó hibák Linux virtuális gépek elavult Virtuálisgép-ügynök befolyásoló problémákat okozzák. tootroubleshoot probléma, kövesse az alábbi általános irányelveket:

1. Kövesse az utasításokat hello [hello Linux Virtuálisgép-ügynök frissítése](../virtual-machines/linux/update-agent.md).

 >[!NOTE]
 >A Microsoft *erősen ajánlott* csak egy terjesztési tárház hello ügynököt frissíteni. Nem ajánlott közvetlenül a Githubból hello kód letöltése és frissítése. Ha hello legújabb ügynök nem érhető el a terjesztéshez, ügyfélszolgálatához terjesztési útmutatást tooinstall azt. hello legutóbbi ügynök, nyissa meg toohello toocheck [Windows Azure Linux ügynök](https://github.com/Azure/WALinuxAgent/releases) hello GitHub-tárházban lapján.

2. Győződjön meg arról, hogy hello Azure-ügynök fut hello VM hello a következő parancs futtatásával:`ps -e`

 Ha hello folyamat nem fut, indítsa újra a következő parancsok hello segítségével:

 * Az Ubuntu:`service walinuxagent start`
 * Az egyéb terjesztéseket:`service waagent start`

3. [Hello automatikus újraindítás ügynök konfigurálása](https://github.com/Azure/WALinuxAgent/wiki/Known-Issues#mitigate_agent_crash).
4. Egy új teszt biztonsági mentés futtatására. Ha hello hiba továbbra is fennáll, gyűjtse össze hello naplók hello az ügyfél virtuális gépről a következő:

   * /var/lib/waagent/*.XML
   * /var/log/waagent.log
   * / var/jelentkezzen/azure / *

Ha a részletes naplózást az waagent kérjük, kövesse az alábbi lépéseket:

1. Hello /etc/waagent.conf fájlban keresse meg a következő sor hello: **részletes naplózás engedélyezése (y |} n)**
2. Változás hello **Logs.Verbose** értéket  *n*  túl*y*.
3. Hello módosítás mentéséhez, és indítsa újra waagent hello a jelen szakaszban szereplő korábbi lépéseket követve.

### <a name="hello-snapshot-status-cannot-be-retrieved-or-a-snapshot-cannot-be-taken"></a>hello pillanatkép állapotát nem sikerült beolvasni vagy pillanatkép nem végezhető.
egy pillanatkép parancs toohello alapul szolgáló tárfiókot kiállító hello virtuális gép biztonsági mentése támaszkodik. Biztonsági mentés sikertelen lehet, mert nincs hozzáférési toohello tárfiók, vagy késleltetve hello hello pillanatkép-feladat végrehajtása.

#### <a name="solution"></a>Megoldás
a következő feltételek hello okozhat a pillanatkép-feladat sikertelensége:

| Ok | Megoldás |
| --- | --- |
| hello VM SQL Server biztonsági mentés konfigurálva van. | Alapértelmezés szerint az hello VM biztonsági mentés VSS teljes biztonsági mentés futó Windows virtuális gépek. A virtuális gépeken futó SQL Server-alapú kiszolgálók és az SQL Servert mentés konfigurálva van, pillanatkép végrehajtási késések fordulhat elő.<br><br>Ha biztonsági mentés hibát tapasztal egy pillanatkép probléma miatt, állítsa be a következő beállításkulcs hello:<br><br>**[HKEY_LOCAL_MACHINE\SOFTWARE\MICROSOFT\BCDRAGENT] "USEVSSCOPYBACKUP"="TRUE"** |
| hello virtuális gép állapotát jelentette megfelelően, mert hello virtuális gép RDP a leállítása. | Ha kikapcsolja a virtuális gép távoli asztal protokoll (RDP) hello, ellenőrizze a portál toodetermine hello hello virtuális gép állapota megfelelő-e. Ha nem megfelelő, használatával állítsa le virtuális gép hello hello portálon hello **leállítási** hello VM irányítópulton lehetőséget. |
| Sok virtuális gép ugyanazon a felhőalapú szolgáltatás vannak hello a konfigurált tooback mentése hello ugyanannyi időt vesz igénybe. | A bevált gyakorlat toospread hello biztonsági mentés ütemezésének virtuális gépek a hello ki ugyanaz a felhőalapú szolgáltatás. |
| hello VM a magas CPU és memória kihasználtsága fut. | Ha magas CPU-használat (több mint 90 %-) vagy a magas memóriahasználatban hello virtuális gép fut, hello pillanatkép-feladat várólistára, és késleltetett, és végül időtúllépéssel leáll. Ilyen esetben próbálkozzon egy igény szerinti biztonsági mentést. |
| hello virtuális gép nem tud hello gazdagép/háló címet a DHCP-Kiszolgálótól. | DHCP engedélyezve kell lennie az infrastruktúra-szolgáltatási virtuális gép biztonsági mentési toowork hello hello Vendég.  Hello virtuális gép nem tud hello gazdagép/háló címet a DHCP-válaszban 245, azt nem töltse le, vagy nem futtatható a kiterjesztést. Ha statikus magánhálózati IP-címe van szüksége, úgy kell beállítania, hello platformon keresztül. hello DHCP-beállítást engedélyezni kell hagyni a virtuális gép hello belül. További információkért lásd: [egy statikus belső privát IP-cím beállítása](../virtual-network/virtual-networks-reserved-private-ip.md). |

### <a name="hello-backup-extension-fails-tooupdate-or-load"></a>tartalék mellék hello tooupdate vagy betöltése sikertelen
Nem tölthetők be bővítmények, ha a biztonsági mentés meghiúsul, mert a pillanatkép nem végezhető.

#### <a name="solution"></a>Megoldás

**A Windows-vendégek:** győződjön meg arról, hogy hello iaasvmprovider szolgáltatás engedélyezve van, és az indítási típust *automatikus*. Ha így hello szolgáltatás nincs konfigurálva, engedélyezze azt toodetermine e hello következő biztonsági mentés sikeres lesz.

**A Linux-vendégek:** ellenőrizze hello legújabb VMSnapshot Linux (biztonsági mentés által használt hello kiterjesztés) verziószáma 1.0.91.0.<br>


Ha hello tartalék mellék továbbra sem sikerül tooupdate vagy terhelés, kényszerítheti hello VMSnapshot bővítmény toobe hello bővítmény távolítsa el a módosítás. hello következő biztonsági mentési kísérlet hello bővítmény újrabetöltést végez.

toouninstall hello kiterjesztés, a következő hello:

1. Nyissa meg toohello [Azure-portálon](https://portal.azure.com/).
2. Keresse meg a virtuális Gépet, amely a biztonsági mentési problémák vannak az hello.
3. Kattintson a **beállítások**.
4. Kattintson a **bővítmények**.
5. Kattintson a **Vmsnapshot bővítmény**.
6. Kattintson a **eltávolítása**.

Ez az eljárás azt eredményezi, hello bővítmény toobe újratelepítése hello következő biztonsági mentés során.

