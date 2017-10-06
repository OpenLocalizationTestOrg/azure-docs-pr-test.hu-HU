---
title: "aaaTroubleshoot Azure biztonsági mentési hibák a klasszikus portálon |} Microsoft Docs"
description: "Végezzen hibaelhárítást az Azure biztonsági mentéséhez és visszaállításához a klasszikus portálon hello Azure virtuális gépeken."
services: backup
documentationcenter: 
author: trinadhk
manager: shreeshd
editor: 
ms.assetid: 117201fb-c0cd-4be4-b47f-abd88fe914cf
ms.service: backup
ms.workload: storage-backup-recovery
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 1/23/2017
ms.author: trinadhk;markgal;
ms.openlocfilehash: b9907f6fa57c631f5446c4d00f946a57c4efd72b
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="troubleshoot-azure-virtual-machine-backup"></a>Azure-beli virtuális gépek biztonsági mentésének hibaelhárítása
> [!div class="op_single_selector"]
> * [Recovery services-tároló](backup-azure-vms-troubleshoot.md)
> * [Mentési tároló](backup-azure-vms-troubleshoot-classic.md)
>
>

Észlelt, miközben az Azure Backup segítségével információkat az alábbi hello táblázatban felsorolt hibák is elháríthatók.

## <a name="discovery"></a>Felderítés
| Biztonsági mentési művelet | Hiba legutolsó részletes adatai | Megkerülő megoldás |
| --- | --- | --- |
| Felderítés |Nem sikerült toodiscover új elemek - észlelt a Microsoft Azure biztonsági mentés és a belső hiba. Várjon néhány percet, és próbálkozzon újra a művelettel hello. |15 perc múlva próbálkozzon újra a hello felderítési folyamat. |
| Felderítés |Nem sikerült toodiscover új cikkek – egy másik felderítési művelet már folyamatban van. Várjon, amíg hello aktuális felderítési művelet befejeződik. |None |

## <a name="register"></a>Regisztráljon
| Biztonsági mentési művelet | Hiba legutolsó részletes adatai | Megkerülő megoldás |
| --- | --- | --- |
| Regisztráljon |Csatolt toohello túllépte a virtuális gép hello támogatott korlátot adatlemezek száma – kérjük leválasztani az egyes adatok lemezek, a virtuális gépet, majd próbálja meg újra hello művelethez. Az Azure biztonsági mentési támogatja too16 adatok lemezeket csatolt tooan Azure virtuális gép biztonsági mentése |None |
| Regisztráljon |A Microsoft Azure Backup szolgáltatás belső hibába ütközött - Várjon néhány percig, és próbálkozzon újra a művelettel hello. Ha hello a probléma továbbra is fennáll, forduljon a Microsoft Support. |Ez a hiba miatt nem támogatott a következő hello tooone konfigurációs VM kaphat a prémium szintű LRS. <br> Prémium szintű storage virtuális gépek biztonsági másolat készíthető a recovery services-tároló segítségével. [További információ](backup-introduction-to-azure-backup.md#using-premium-storage-vms-with-azure-backup) |
| Regisztráljon |Nem sikerült regisztrálni az ügynök telepítése a művelet időkorlátja |Ellenőrizze, hogy ha hello virtuális gép operációs rendszerének verziója hello támogatott. |
| Regisztráljon |Parancs végrehajtása nem sikerült – a elem folyamatban van egy másik művelet. Várjon, amíg hello korábbi művelet befejezése |None |
| Regisztráljon |Prémium szintű storage a tárolt virtuális merevlemezek rendelkező virtuális gépek nem támogatottak a biztonsági mentéshez |None |
| Regisztráljon |Virtuálisgép-ügynök nincs jelen a hello virtuálisgép - telepítse hello szükséges előfeltételt, Virtuálisgép-ügynök, és indítsa újra a hello műveletet. |[További](#vm-agent) Virtuálisgép-ügynök telepítése, és hogyan toovalidate hello Virtuálisgép-ügynök telepítése. |

## <a name="backup"></a>Biztonsági mentés
| Biztonsági mentési művelet | Hiba legutolsó részletes adatai | Megkerülő megoldás |
| --- | --- | --- |
| Biztonsági mentés |Nem sikerült kommunikálni a pillanatkép állapot hello Virtuálisgép-ügynök. Pillanatkép virtuális gép sub feladat túllépte az időkorlátot. -Ellenőrizze a hello hibaelhárítási útmutató hogyan tooresolve ez. |Ezt a hibát vált ki, ha a probléma oka hello Virtuálisgép-ügynök, vagy hálózati hozzáférési toohello Azure-infrastruktúra valamilyen módon le van tiltva. További információ [virtuális gépet hibakeresés pillanatkép-problémák](backup-azure-troubleshoot-vm-backup-fails-snapshot-timeout.md). <br> Ha hello Virtuálisgép-ügynök nem okoz probléma merül fel, majd indítsa újra a hello virtuális gép. Esetenként a virtuális gép állapota nem megfelelő problémákat okozhatnak, és a "hibás állapot" hello virtuális gép újraindítása alaphelyzetbe állítása. |
| Biztonsági mentés |Biztonsági mentés egy belső hiba miatt nem sikerült – hello művelet néhány perc múlva próbálkozzon újra. Ha hello a probléma továbbra is fennáll, forduljon a Microsoft Support |Győződjön meg arról, ha van egy átmeneti hiba Virtuálisgép-tároló elérése közben. Győződjön meg róla [Azure állapot](https://azure.microsoft.com/en-us/status/) toosee bármely folyamatos hello régióban kapcsolódó toocompute/tárolási vagy hálózati probléma esetén. Adja újra hello biztonsági mentés utáni probléma elhárítására. |
| Biztonsági mentés |Nem tudta végrehajtani az hello művelet, mivel a virtuális gép már nem létezik. |Biztonsági mentés nem végezhető el, mert hello a biztonsági mentéshez konfigurált virtuális gép törölve lett. Állítsa le a további biztonsági mentések által is tooProtected elemek nézetében, válassza ki a védett elem, és kattintson a védelem kikapcsolása. Adatok megőrzése biztonsági mentési adatok lehetőség kiválasztásával őrizheti meg. Később folytathatja a védelmet a virtuális gép kattintva a konfigurálás regisztrált elemek nézetében elleni védelem |
| Biztonsági mentés |Sikertelen tooinstall hello Azure Recovery Services bővítmény a kiválasztott hello elem - Virtuálisgép-ügynök az Azure Recovery Services-bővítmény olyan előfeltételt. Hello Azure Virtuálisgép-ügynök telepítése, és indítsa újra a hello regisztrációs művelet |<ol> <li>Ellenőrizze, hogy ha hello Virtuálisgép-ügynök megfelelően van telepítve. <li>Győződjön meg arról, hogy hello hello virtuális gép konfigurációs jelzőt megfelelően van-e beállítva.</ol> [További](#validating-vm-agent-installation) Virtuálisgép-ügynök telepítése, és hogyan toovalidate hello Virtuálisgép-ügynök telepítése. |
| Biztonsági mentés |Parancs végrehajtása nem sikerült – egy másik művelet jelenleg folyamatban van erre az elemre. Várjon, amíg hello korábbi művelet befejeződik, majd próbálkozzon újra |Egy meglévő biztonsági mentési vagy visszaállítási hello VM feladat fut, és egy új feladat nem indítható, hello meglévő feladat futása közben. |
| Biztonsági mentés |Bővítmény telepítése nem sikerült hello "COM + volt a Microsoft Distributed Transaction Coordinator nem tootalk toohello |Ez általában azt jelenti, hogy nem fut-e hello COM + szolgáltatást. A probléma megoldásán segítségért forduljon a Microsoft támogatási szolgálatához. |
| Biztonsági mentés |Pillanatkép sikertelen volt a művelet hello VSS-művelet hibát "ezen a meghajtón a BitLocker meghajtótitkosítás zárolta. Ezen a meghajtón, a Vezérlőpult kell zárolásának feloldásához. |Kapcsolja ki a BitLocker a virtuális gép hello összes merevlemezén, és figyelje meg, ha a hello VSS probléma megoldódott-e |
| Biztonsági mentés |Prémium szintű storage a tárolt virtuális merevlemezek rendelkező virtuális gépek nem támogatottak a biztonsági mentéshez |None |
| Biztonsági mentés |Az Azure virtuális gép nem található. |Ez akkor fordul elő, amikor hello elsődleges virtuális gép törlődik, de a biztonsági mentési házirend hello továbbra is a virtuális gép tooperform biztonsági mentéshez toolook. toofix Ez a hiba: <ol><li>Hozza létre újra a virtuális gép hello hello azonos nevű és ugyanazon erőforráscsoport neve [cloud service name] <br>(VAGY) <li> Tiltsa le a virtuális gép védelmét, így további biztonsági mentések nem indul beolvasása. </ol> |
| Biztonsági mentés |Virtuálisgép-ügynök nincs jelen a hello virtuálisgép - telepítse hello szükséges előfeltételt, Virtuálisgép-ügynök, és indítsa újra a hello műveletet. |[További](#vm-agent) Virtuálisgép-ügynök telepítése, és hogyan toovalidate hello Virtuálisgép-ügynök telepítése. |

## <a name="jobs"></a>Feladatok
| Művelet | Hiba legutolsó részletes adatai | Megkerülő megoldás |
| --- | --- | --- |
| Megszakítása |Megszakítása nem támogatott ilyen típusú feladatokat - Várjon, amíg a hello feladat befejeződik. |None |
| Megszakítása |hello feladat nem törölhető állapotban van,-várjon, amíg a hello feladat befejeződik. <br>VAGY<br> hello kiválasztott feladat nem törölhető állapotban van, – hello feladat toocomplete várja. |Minden valószínűség szerint hello feladat majdnem befejeződött; Várjon, amíg a hello feladat befejeződik |
| Megszakítása |Hello feladatot nem lehet megszakítani, mert nincs folyamatban – törlését csak a támogatott feladatok, amely jelenleg folyamatban vannak. Adjon kísérlet megszakítása az egy folyamatban lévő feladat. |Ez akkor fordul elő tooa átmeneti állapot miatt. Várjon egy percet, majd próbálja megismételni a hello a művelet megszakítása |
| Megszakítása |Nem sikerült toocancel hello feladat - Várjon, amíg a feladat befejeződik. |None |

## <a name="restore"></a>Visszaállítás
| Művelet | Hiba legutolsó részletes adatai | Megkerülő megoldás |
| --- | --- | --- |
| Visszaállítás |Visszaállítás felhő belső hiba miatt sikertelen volt |<ol><li>Cloud service toowhich toorestore kívánt DNS-beállításokkal van konfigurálva. Ellenőrizheti a <br>$deployment get-AzureDeployment - szolgáltatásnév "ServiceName" =-tárolóhely "Éles" Get-AzureDns - DnsSettings $deployment. DnsSettings<br>Ha nincs konfigurált címet, ez azt jelenti, hogy a DNS-beállítások vannak konfigurálva.<br> <li>Cloud service toowhich tooyou próbált toorestore foglalt IP-cím van konfigurálva, és a meglévő virtuális gépek által a felhőalapú szolgáltatás leállított állapotban van.<br>Ellenőrizheti a felhőszolgáltatás van foglalt IP-cím a következő powershell-parancsmagok használatával:<br>$deployment = get-AzureDeployment - szolgáltatásnév "servicename"-"Éles" $tárolóhely DEP ReservedIPName <br><li>A virtuális gép toosame felhőalapú szolgáltatás a következő speciális hálózati konfigurációkkal toorestore próbált. <br>– Virtuális gépek a terheléselosztó-konfigurációja (külső és belső)<br>-A virtuális gépek a több foglalt IP-cím<br>-A virtuális gépek több hálózati adapterrel rendelkező<br>Jelöljön ki egy új felhőalapú szolgáltatás felhasználói felületi hello, vagy tekintse meg a túl[szempontok visszaállítása](backup-azure-restore-vms.md#restoring-vms-with-special-network-configurations) speciális hálózati konfigurációk rendelkező virtuális gépek</ol> |
| Visszaállítás |kijelölt hello DNS-név már használatban van – adjon egy másik DNS-nevet adja meg, és próbálkozzon újra. |hello Itt a DNS-név hivatkozik toohello felhőszolgáltatás neve (általában végződő. cloudapp.net). Egyedi kell toobe. Ha ezt a hibát észlel, visszaállítás során kell toochoose egy másik Virtuálisgép-nevet. <br><br> Ez a hiba csak az Azure-portálon hello toousers látható. hello Powershellen keresztül visszaállítási művelet sikeres lesz, mert csak hello lemezek állítja vissza, és nem hoz létre a virtuális gép hello. hello hiba fog tapasztalt, létrehozásakor hello VM explicit módon meg hello lemez visszaállítási művelet után. |
| Visszaállítás |Nincs megfelelő hello megadott virtuális hálózati konfiguráció – adjon meg egy másik virtuális hálózati konfiguráció, és próbálkozzon újra. |None |
| Visszaállítás |a megadott hello felhőalapú szolgáltatást használ egy fenntartott IP-cím, amely nem egyezik a hello konfigurációs hello virtuális gép helyreállítása – adja meg egy másik felhőalapú szolgáltatást, amely nem használja a foglalt IP-cím, vagy válasszon másik helyreállítási pont toorestore a. |None |
| Visszaállítás |A felhőalapú szolgáltatás elérte az újrapróbálkozási hello művelet egy másik felhőalapú szolgáltatást, vagy egy meglévő végpontot a bemeneti végpontok - számára vonatkozó korlátozást. |None |
| Visszaállítás |Két különböző régiókban biztonsági mentési tároló és a cél tárfiók - győződjön meg arról, hogy a visszaállítási művelet a megadott tárfiók hello hello egy Azure-régióban van hello mentési tároló. |None |
| Visszaállítás |A Tárfiók megadott hello visszaállítási művelet nem támogatott – a storage-fiókok csak Basic vagy Standard helyileg redundáns vagy földrajzi redundancia replikációs beállítások támogatottak. Válasszon egy támogatott tárfiókot |None |
| Visszaállítás |A visszaállítási művelet a megadott Tárfiók típus nem online – győződjön meg arról, hogy online állapotban-e a visszaállítási művelet a megadott tárfiók hello |Ez azért fordulhat elő, az Azure Storage vagy tooan leállás miatt egy átmeneti hiba miatt. Válasszon másik tárolási fiókot. |
| Visszaállítás |Erőforráscsoport kvóta elérve, mert egyes erőforráscsoportok törölni az Azure-portálon, vagy kérje az Azure támogatási tooincrease hello korlátok. |None |
| Visszaállítás |Nem létezik a kijelölt alhálózat – válasszon ki egy alhálózatot, amely létezik |None |

## <a name="policy"></a>Szabályzat
| Művelet | Hiba legutolsó részletes adatai | Megkerülő megoldás |
| --- | --- | --- |
| Házirend létrehozása |Nem sikerült toocreate hello házirend - csökkentse a hello megőrzési lehetőségek toocontinue házirend konfigurációjával kapcsolatban. |None |

## <a name="vm-agent"></a>Virtuálisgép-ügynök
### <a name="setting-up-hello-vm-agent"></a>Virtuálisgép-ügynök hello beállítása
Általában hello Virtuálisgép-ügynök már jelen az Azure katalógusában hello a létrehozott virtuális gépeken. A helyszíni adatközpontját a áttelepített virtuális gépek azonban nem áll hello Virtuálisgép-ügynök van telepítve. Ilyen virtuális gépek hello Virtuálisgép-ügynök telepítve explicit módon toobe kell. Tudjon meg többet az [hello VM ügynök telepítése egy meglévő virtuális gép](http://blogs.msdn.com/b/mast/archive/2014/04/08/install-the-vm-agent-on-an-existing-azure-vm.aspx).

Windows virtuális gépek:

* Töltse le és telepítse a hello [ügynök MSI](http://go.microsoft.com/fwlink/?LinkID=394789&clcid=0x409). Rendszergazdai jogosultságok toocomplete hello telepítési kell.
* [Hello VM tulajdonság](http://blogs.msdn.com/b/mast/archive/2014/04/08/install-the-vm-agent-on-an-existing-azure-vm.aspx) tooindicate, amely hello ügynök telepítve van.

Linux virtuális gépek:

* Telepítse a legújabb [Linux-ügynök](https://github.com/Azure/WALinuxAgent) a githubról.
* [Hello VM tulajdonság](http://blogs.msdn.com/b/mast/archive/2014/04/08/install-the-vm-agent-on-an-existing-azure-vm.aspx) tooindicate, amely hello ügynök telepítve van.

### <a name="updating-hello-vm-agent"></a>Hello Virtuálisgép-ügynök frissítése
Windows virtuális gépek:

* Frissítési hello Virtuálisgép-ügynök újratelepítését hello egyszerűen [Virtuálisgép-ügynök bináris](http://go.microsoft.com/fwlink/?LinkID=394789&clcid=0x409). Azonban meg kell tooensure futtató nincs biztonsági mentési művelet során hello Virtuálisgép-ügynök frissítése folyamatban van.

Linux virtuális gépek:

* Hello utasításokat kövesse a megjelenő [frissítése Linux Virtuálisgép-ügynök](../virtual-machines/linux/update-agent.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).

### <a name="validating-vm-agent-installation"></a>Virtuálisgép-ügynök telepítésének ellenőrzése
Hogyan toocheck a hello Virtuálisgép-ügynök verziója a Windows virtuális gépek:

1. Jelentkezzen be Azure virtuális gép toohello, és keresse meg a toohello mappa *C:\WindowsAzure\Packages*. Keresse meg hello WaAppAgent.exe fájl található.
2. Kattintson a jobb gombbal a hello fájlt, nyissa meg túl**tulajdonságok**, majd válassza ki a hello **részletek** külön-külön hello termékverzió mező lehet 2.6.1198.718 vagy újabb
