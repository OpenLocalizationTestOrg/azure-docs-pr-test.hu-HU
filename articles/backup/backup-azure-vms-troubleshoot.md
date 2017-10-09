---
title: "Azure virtuális gép biztonsági mentési hibák aaaTroubleshoot |} Microsoft Docs"
description: "Biztonsági mentés és visszaállítás az Azure virtuális gépek hibaelhárítása"
services: backup
documentationcenter: 
author: trinadhk
manager: shreeshd
editor: 
ms.assetid: 73214212-57a4-4b57-a2e2-eaf9d7fde67f
ms.service: backup
ms.workload: storage-backup-recovery
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/17/2017
ms.author: trinadhk;markgal;jpallavi;
ms.openlocfilehash: 9224c47e02b52688adcba5876c674c88502557c6
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

## <a name="backup"></a>Biztonsági mentés
| Hiba legutolsó részletes adatai | Megkerülő megoldás |
| --- | --- |
| Nem tudta végrehajtani az hello művelet, mivel a virtuális gép már nem létezik. – Állítsa le a virtuális gép védelmét a biztonsági mentési adatok törlése nélkül. További részletekért http://go.microsoft.com/fwlink/?LinkId=808124: |Ez akkor fordul elő, amikor hello elsődleges virtuális gép törlődik, de a biztonsági mentési házirend hello továbbra is fennáll, a virtuális gép tooback keres. toofix Ez a hiba: <ol><li> Hozza létre újra a virtuális gép hello hello azonos nevű és ugyanazon erőforráscsoport neve [cloud service name]<br>(VAGY)</li><li> Állítsa le, vagy anélkül hello biztonsági mentési adatok törlése a virtuális gép védelmét. [További részletekért](http://go.microsoft.com/fwlink/?LinkId=808124)</li></ol> |
| Pillanatkép-művelet végrehajtása sikertelen volt, a hello virtuálisgép - toono hálózati kapcsolat miatt győződjön meg arról, hogy a virtuális gép hálózati hozzáféréssel rendelkezik. A pillanatkép toosucceed lehet, hogy engedélyezett Azure datacenter IP-cím címtartományok beállítása hálózati hozzáféréshez proxykiszolgálót. További részletekért lásd a túl http://go.microsoft.com/fwlink/?LinkId=800034. Ha a proxykiszolgáló már használ, győződjön meg arról, hogy helyesen vannak-e konfigurálva a proxykiszolgáló beállításai | Ez a hiba fordul elő, ha megtagadja a kimenő internetkapcsolat hello hello virtuális gépen. Internetkapcsolat szükség a virtuális gép pillanatkép bővítmény tootake alapul szolgáló lemezek hello virtuális gép pillanatképe. [További](backup-azure-troubleshoot-vm-backup-fails-snapshot-timeout.md#snapshot-operation-failed-due-to-no-network-connectivity-on-the-virtual-machine) a hogyan toofix pillanatkép-hibák miatt tooblocked hálózati hozzáférést. |
| Virtuálisgép-ügynök a hello Azure Backup szolgáltatás nem toocommunicate. -Ellenőrizze a hello virtuális gép rendelkezik hálózati kapcsolattal, illetve hello Virtuálisgép-ügynök legújabb és futnak. További információkért tekintse meg túl http://go.microsoft.com/fwlink/?LinkId=800034 |Ezt a hibát vált ki, ha a probléma oka hello Virtuálisgép-ügynök, vagy hálózati hozzáférési toohello Azure-infrastruktúra valamilyen módon le van tiltva. [További](backup-azure-troubleshoot-vm-backup-fails-snapshot-timeout.md#vm-agent-unable-to-communicate-with-azure-backup) kapcsolatos hibakeresés virtuális gépet pillanatkép-problémák.<br> Ha hello Virtuálisgép-ügynök nem okoz probléma merül fel, majd indítsa újra a hello virtuális gép. Esetenként a virtuális gép állapota nem megfelelő problémákat okozhatnak, és a "hibás állapot" hello virtuális gép újraindítása alaphelyzetbe állítja. |
| A virtuális gép van kiépítési állapota sikertelen – indítsa újra hello virtuális gép, és győződjön meg arról, hogy a virtuális gép biztonsági mentés futó vagy leállítási állapotban van hello | Ez akkor fordul elő, amikor egy hello bővítmény hibák vezet be a virtuális gép állapota toobe üzembe helyezési állapota sikertelen. Nyissa meg tooextensions lista és van-e sikertelen bővítmény, távolítsa el, és próbálja meg újraindítani a virtuális gép hello. Ha az összes bővítmény futó állapotban van, ellenőrizze, hogy fut-e a virtuális gép agent szolgáltatást. Ha nem, hello VM ügynök szolgáltatás újraindításához. | 
| VMSnapshot művelet sikertelen volt a felügyelt lemez – kérjük újrapróbálkozási hello biztonsági mentési műveletet. Ha hello probléma megismétlődik, kövesse a hello található utasítások segítségével: "http://go.microsoft.com/fwlink/?LinkId=800034". Ha további sikertelen, forduljon a Microsoft támogatási szolgálatához | A hiba, ha az Azure Backup szolgáltatás nem sikerül tootrigger egy pillanatkép. [További](backup-azure-troubleshoot-vm-backup-fails-snapshot-timeout.md#vmsnapshot-extension-operation-failed) kapcsolatos hibakeresés a virtuális gép pillanatkép-problémák. |
| Sikerült nem másolási hello pillanatkép hello virtuális gép miatt tooinsufficient szabad hely a hello tárfiók - ügyeljen arra, hogy a tárfiók egyenértékű szabad terület hello prémium tárolólemezek toohello adatok csatolt toohello virtuális gép | Prémium szintű virtuális gépeken, ha azt hello pillanatkép toostorage fiók másolása. Ez a toomake meg arról, hogy biztonságimásolat-felügyeleti forgalmat, amely akkor működik a pillanatkép, IOPS elérhető toohello alkalmazás premium lemezekkel hello száma nem korlátozza. A Microsoft azt javasolja, viszont csak 50 %-át hello teljes fiók tárhely, hello Azure Backup szolgáltatás át tudja másolni hello toostorage fiók és átviteli pillanatképadatok erről a helyről másolt tárolási fiók toohello tárolóban. | 
| Nem lehet tooperform hello műveletet, mert hello Virtuálisgép-ügynök nincs rugalmas |Ezt a hibát vált ki, ha a probléma oka hello Virtuálisgép-ügynök, vagy hálózati hozzáférési toohello Azure-infrastruktúra valamilyen módon le van tiltva. Windows virtuális gép esetében ellenőrizze hello VM-ügynökszolgáltatás állapotát szolgáltatások, valamint hogy hello ügynök jelenik meg, a Vezérlőpult Programok telepítése. Próbálja meg eltávolítani hello program ügynöktől vezérlő panel és újratelepítése hello említett [alatt](#vm-agent). Hello ügynök újbóli telepítése után indul el, az ad hoc biztonsági mentés tooverify. |
| Helyreállítási szolgáltatások művelet sikertelen volt. -Ellenőrizze, hogy, hogy a legújabb virtuálisgép-ügynök-e a virtuális gép hello és ügynökszolgáltatás fut. Próbálja megismételni a biztonsági mentési műveletet, és ha a hiba, forduljon a Microsoft támogatási szolgálatához. |Ez a hiba fordul elő, ha Virtuálisgép-ügynök elavultak. Tekintse meg a "Virtuálisgép-ügynök frissítése hello" című szakaszt tooupdate hello Virtuálisgép-ügynök. |
| Virtuális gép nem létezik. -Ellenőrizze, hogy a virtuális gép létezik-e, vagy válasszon másik virtuális gépet. |Ez akkor fordul elő, amikor hello elsődleges virtuális gép törlődik, de a biztonsági mentési házirend hello továbbra is a virtuális gép tooperform biztonsági mentéshez toolook. toofix Ez a hiba: <ol><li> Hozza létre újra a virtuális gép hello hello azonos nevű és ugyanazon erőforráscsoport neve [cloud service name]<br>(VAGY)<br></li><li>Hello biztonsági mentési adatok törlése nélkül hello virtuális gép védelmének kikapcsolását. [További részletekért](http://go.microsoft.com/fwlink/?LinkId=808124)</li></ol> |
| A parancs végrehajtása sikertelen volt. – Egy másik művelet jelenleg folyamatban van erre az elemre. Várjon, amíg hello korábbi művelet befejeződik, majd próbálkozzon újra |A virtuális gép hello meglévő biztonsági mentésből fut, és egy új feladat nem indítható, hello meglévő feladat futása közben. |
| Másolja a VHD-k hello biztonsági mentési tárolóból túllépte az időkorlátot, mert a hello művelet néhány perc múlva próbálkozzon újra. Ha hello a probléma továbbra is fennáll, forduljon a Microsoft Support. | Ez akkor fordul elő, ha átmeneti hiba van a tárolási oldalon, vagy ha biztonsági mentési szolgáltatás nem kap elegendő IOPS üzemeltető virtuális gép hello rendelés tootransfer adatok időtúllépési időszak toovault belül tárfiókból. Győződjön meg arról, hogy követte [ajánlott eljárások](backup-azure-vms-introduction.md#best-practices) beállítása a biztonsági mentés közben. Helyezze a virtuális gép tooa eltérőek a tárolási fiók, amely nem töltődik be, majd próbálja megismételni a biztonsági mentés.|
| Biztonsági mentés egy belső hiba miatt nem sikerült – hello művelet néhány perc múlva próbálkozzon újra. Ha hello a probléma továbbra is fennáll, forduljon a Microsoft Support |Ez a hibaüzenet 2 lehetnek az okai: <ol><li> Van egy átmeneti hiba hello Virtuálisgép-tároló elérése közben. Győződjön meg róla [Azure állapot](https://azure.microsoft.com/en-us/status/) toosee, ha bármely folyamatos probléma van a kapcsolódó toocompute, a tárolás és a hálózat hello régióban. Miután hello probléma megoldódott, ismételje hello biztonsági mentési feladat. <li>hello eredeti virtuális gép törölve lett, és ezért hello helyreállítási pont nem végezhető. tookeep hello a törölt virtuális gépek biztonsági mentési adatokat, de távolítsa el a biztonsági mentési hibákat hello: hello virtuális gép védelmének megszüntetéséhez és hello beállítás tookeep hello adatok kiválasztásához. Ez a művelet nem hello ütemezett biztonsági mentési feladatot, és a hello ismétlődő hibaüzenetek. |
| Nem sikerült tooinstall hello Azure Recovery Services bővítmény a kiválasztott hello elem – hello Virtuálisgép-ügynök feltétele az Azure Recovery Services-bővítmény hello. Hello Azure Virtuálisgép-ügynök telepítése, és indítsa újra a hello regisztrációs művelet |<ol> <li>Ellenőrizze, hogy ha hello Virtuálisgép-ügynök megfelelően van telepítve. <li>Győződjön meg arról, a virtuális gép konfigurációs hello hello jelző megfelelően van-e beállítva.</ol> [További](#validating-vm-agent-installation) hello Virtuálisgép-ügynök, és hogyan toovalidate hello Virtuálisgép-ügynök telepítése. |
| Bővítmény telepítése nem sikerült hello "COM + volt a Microsoft Distributed Transaction Coordinator nem tootalk toohello |Ez általában azt jelenti, hogy nem fut-e hello COM + szolgáltatást. A probléma megoldásán segítségért forduljon a Microsoft támogatási szolgálatához. |
| Pillanatkép sikertelen volt a művelet hello VSS-művelet hibát "ezen a meghajtón a BitLocker meghajtótitkosítás zárolta. A Vezérlőpult hello meghajtót kell zárolásának feloldásához. |Kapcsolja ki a BitLocker a virtuális gép hello összes merevlemezén, és figyelje meg, ha a hello VSS probléma megoldódott-e |
| Virtuális gép nincs olyan állapotban, amely lehetővé teszi, hogy a biztonsági mentéseket. |<ul><li>Ellenőrizze, hogy virtuális gép átmeneti állapotban fut, valamint a leállítási között le. Ha igen, hello VM állapotot toobe ezek egyikét várja meg, indít újra biztonsági mentését. <li> Ha virtuális gép hello egy Linux virtuális gép és a használt [biztonsági fokozott Linux] kernel modul, meg kell-e tooexclude hello Linux-ügynök elérési útja (_/var/lib/waagent_) a biztonsági házirend toomake meg arról, hogy tartalék mellék telepíti.  |
| Az Azure virtuális gép nem található. |Ez akkor fordul elő, amikor hello elsődleges virtuális gép törlődik, de hello biztonsági mentési házirend továbbra is a virtuális gép tooperform a toolook készítsen biztonsági másolatot. toofix Ez a hiba: <ol><li>Hozza létre újra a virtuális gép hello hello azonos nevű és ugyanazon erőforráscsoport neve [cloud service name] <br>(VAGY) <li> Tiltsa le a védelmet a virtuális gép, ezért hello biztonsági mentési feladatok nem lesz létrehozva. </ol> |
| Virtuálisgép-ügynök nincs jelen a hello virtuálisgép - telepítse az összes előfeltétel és hello Virtuálisgép-ügynök, és indítsa újra hello műveletet. |[További](#vm-agent) Virtuálisgép-ügynök telepítése, és hogyan toovalidate hello Virtuálisgép-ügynök telepítése. |
| Pillanatkép művelet tooVSS írók rossz állapotban miatt sikertelen volt |Toorestart (kötet árnyékmásolata szolgáltatást) VSS-író, hibás állapotban lévő van szüksége. tooachieve, egy rendszergazda jogú parancssorból futtassa _vssadmin lista írók_. Kimenet tartalmazza az összes VSS-író és állapotát. Minden VSS-író amelynek állapota nem "[1] stabil" indítsa újra VSS-író a következő parancsokat egy rendszergazda jogú parancssorból történő futtatásával:<br> _net stop szolgáltatásnév_ <br> _a net start szolgáltatásnév_|
| Pillanatkép művelet hello konfigurációs tooa elemzési hibája miatt sikertelen volt |Ez akkor hello MachineKeys directory toochanged engedélyeinek miatt fordul elő: _%systemdrive%\programdata\microsoft\crypto\rsa\machinekeys_ <br>Futtassa az alábbi parancsot, és győződjön meg róla, hogy MachineKeys könyvtár alapértelmezett:<br>_Icacls %systemdrive%\programdata\microsoft\crypto\rsa\machinekeys_ <br><br> Az alapértelmezett engedélyek a következők:<br>Everyone:(R,W) <br>BUILTIN\Administrators:(F)<br><br>Ha az alapértelmezettől eltérő MachineKeys directory engedélyek látja, kövesse az alábbi lépéseket toocorrect engedélyek, törölje hello tanúsítvány és hello biztonsági mentés.<ol><li>Javítsa ki az engedélyek MachineKeys könyvtárban.<br>Hello directory Explorer biztonsági tulajdonságait és a speciális biztonsági beállítások használ, alaphelyzetbe állítása engedélyek biztonsági toohello alapértelmezett értékeket, távolítsa el a felesleges (alapértelmezettnél) felhasználói objektum hello könyvtárból, és győződjön meg arról, hogy hello "Mindenki" engedélyek kellett különleges a hozzáférés:<br>-Mappa listázása / adatok olvasása <br>-Attribútumok olvasása <br>– Olvassa el a kiterjesztett attribútumok <br>-Fájlok létrehozása / adatok írása <br>-Mappák létrehozása / adatok hozzáfűzése<br>-Attribútumok írása<br>-A kiterjesztett attribútumok írása<br>-Engedélyek olvasása<br><br><li>Törölje az összes tanúsítvány "Kiállítva a következőnek" mező = "A Windows Azure felügyeleti a bővítmények" vagy "Windows Azure KSZT tanúsítvány létrehozója".<ul><li>[Nyissa meg a tanúsítványok (helyi számítógép)](https://msdn.microsoft.com/library/ms788967(v=vs.110).aspx)<li>Minden tanúsítvány törlése (személyi -> tanúsítványok) és "Kiállítva a következőnek" mező = "A Windows Azure felügyeleti a bővítmények" vagy "Windows Azure KSZT tanúsítvány létrehozója".</ul><li>Virtuális gép a biztonsági mentés elindítása. </ol>|
| Érvényesítése sikertelen volt, mert a virtuális gépet önmagában BEK van titkosítva. Biztonsági másolatok csak virtuális gépek BEK és KEK is titkosítani engedélyezhető. |Virtuális gép titkosítani kell a BitLocker titkosítási kulcs, mind a kiszolgáló fő titkosítási kulcs használatával. Ezt követően engedélyezni kell a biztonsági mentés. |
| Az Azure Backup szolgáltatás nem rendelkezik elegendő engedélyekkel tooKey tároló a biztonsági mentés a titkosított virtuális gépek. |A biztonsági mentési szolgáltatás meg kell adni ezeket az engedélyeket a PowerShellben említett lépéseket követve **biztonsági mentés engedélyezése** szakasza [PowerShell dokumentációs](backup-azure-vms-automation.md). |
|Telepítésének pillanatkép-bővítményt nem sikerült – a COM + nem tudta tootalk toohello Microsoft elosztott tranzakciók koordinátora | Adjon próbálja toostart windows-szolgáltatás "COM + rendszer Application" (egy emelt szintű parancssorból - _net start COMSysApp_). <br>Indítása közben nem sikerül, ha adjon kövesse az alábbi lépéseket:<ol><li> Ellenőrizze, hogy hello bejelentkezési fiók az "Elosztott tranzakciók koordinátora" szolgáltatás "Hálózati szolgáltatás". Ha nem, változtassa meg a túl "Hálózati szolgáltatás", indítsa újra a szolgáltatást, és ismételje meg toostart szolgáltatás "COM + rendszer alkalmazás". "<li>Ha toostart sikertelen, távolítsa el/telepítése "Distributed Transaction Coordinator" szolgáltatás által a következő lépések a következők:<br> -Hello MSDTC szolgáltatás leállítása<br> -Nyisson meg egy parancssort (cmd) <br> -Parancs futtatása "msdtc-eltávolítása" <br> -Parancs futtatása "msdtc-telepítés" <br> -Hello MSDTC szolgáltatás elindítása<li>"COM + rendszer alkalmazás" windows szolgáltatás elindítása és az elindítása után indul el, biztonsági mentés a portálról.</ol> |
|  Pillanatkép művelet tooCOM + hiba miatt sikertelen volt | hello ajánlott művelet toorestart windows-szolgáltatás "COM + rendszer Application" (egy emelt szintű parancssorból - _net start COMSysApp_). Ha hello a probléma továbbra is fennáll, indítsa újra a virtuális gép hello. Ha a virtuális gép nem oldották hello újraindítása, próbálja [VMSnapshot bővítmény eltávolítása hello](https://docs.microsoft.com/en-us/azure/backup/backup-azure-troubleshoot-vm-backup-fails-snapshot-timeout#cause-3-the-backup-extension-fails-to-update-or-load) és manuálisan kezdeményezi hello biztonsági mentés. |
| Nem sikerült toofreeze egy vagy több csatlakoztatási pontot a virtuális gép tootake fájlrendszer konzisztens pillanatkép hello | A lépéseket követve hello használata: <ol><li>Hello fájlrendszer állapota az összes csatlakoztatott eszközök _"tune2fs"_ parancsot.<br> Példa: tune2fs -l/dev/sdb1 \| GREP "Filesystem állapot" <li>Mely FileSystem állapota nem tiszta használatával hello eszközök leválasztása _"umount"_ parancs <li> Az ilyen eszközök használatával FileSystemConsistency-ellenőrzés futtatása _"fsck"_ parancs <li> Hello eszközök csatlakoztassa újra, és próbálja meg a biztonsági mentés.</ol> |
| Pillanatkép-művelet miatt nem sikerült biztonságos hálózati kommunikációs csatornát létrehozni toofailure | <ol><Li> Nyissa meg a Beállításszerkesztőt a regedit.exe futtassa egy emelt jogosultságszintű módban. <li> Azonosítsa az összes verzióját. A NetFramework rendszerben. Ezek meg adva a beállításkulcs "HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft" hello hierarchia <li> Az egyes. Beállításkulcs megtalálható NetFramework kulcsot következő hozzáadása: <br> "SchUseStrongCrypto" = dword: 00000001 </ol>|
| Pillanatkép-művelet miatt nem sikerült toofailure a Visual Studio 2012-es Visual C++ újraterjeszthető csomag telepítése | Keresse meg a tooC:\Packages\Plugins\Microsoft.Azure.RecoveryServices.VMSnapshot\agentVersion és vcredist2012_x64 telepítése. Győződjön meg arról, hogy a beállításkulcs a használatának engedélyezése a szolgáltatás telepítési beállításai toocorrect érték azaz beállításkulcs értékét _HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Services\Msiserver_ too3 és nem 4 van beállítva. Ha továbbra is telepítési problémák felől elérhető, indítsa újra telepítési szolgáltatásával futtatásával _MSIEXEC /UNREGISTER_ követ _MSIEXEC /REGISTER_ parancsot egy emelt szintű parancssorból.  |


## <a name="jobs"></a>Feladatok
| Hiba legutolsó részletes adatai | Megkerülő megoldás |
| --- | --- |
| Megszakítása nem támogatott ilyen típusú feladatokat - Várjon, amíg a hello feladat befejeződik. |None |
| hello feladat nem törölhető állapotban van,-várjon, amíg a hello feladat befejeződik. <br>VAGY<br> hello kiválasztott feladat nem törölhető állapotban van, – hello feladat toocomplete várja. |Minden valószínűség szerint hello feladat majdnem befejeződött. Várjon, amíg hello feladat befejezését.|
| Hello feladatot nem lehet megszakítani, mert nincs folyamatban – törlését csak a támogatott feladatok, amely jelenleg folyamatban vannak. Adjon kísérlet megszakítása az egy folyamatban lévő feladat. |Ez akkor fordul elő tooa átmeneti állapot miatt. Várjon egy percet, és próbálja megismételni a hello visszavonási művelet. |
| Sikertelen toocancel hello feladat - Várjon, amíg a feladat befejeződik. |None |

## <a name="restore"></a>Visszaállítás
| Hiba legutolsó részletes adatai | Megkerülő megoldás |
| --- | --- |
| Visszaállítás felhő belső hiba miatt sikertelen volt |<ol><li>Cloud service toowhich toorestore kívánt DNS-beállításokkal van konfigurálva. Ellenőrizheti a <br>$deployment get-AzureDeployment - szolgáltatásnév "ServiceName" =-tárolóhely "Éles" Get-AzureDns - DnsSettings $deployment. DnsSettings<br>Ha nincs konfigurált címet, ez azt jelenti, hogy a DNS-beállítások vannak konfigurálva.<br> <li>Cloud service toowhich tooyou próbált toorestore foglalt IP-cím van konfigurálva, és a meglévő virtuális gépek által a felhőalapú szolgáltatás leállított állapotban van.<br>Ellenőrizheti a felhőszolgáltatás van foglalt IP-cím a következő powershell-parancsmagok használatával:<br>$deployment = get-AzureDeployment - szolgáltatásnév "servicename"-"Éles" $tárolóhely DEP ReservedIPName <br><li>A virtuális gép toosame felhőalapú szolgáltatás a következő speciális hálózati konfigurációkkal toorestore próbált. <br>– Virtuális gépek a terheléselosztó-konfigurációja (külső és belső)<br>-A virtuális gépek a több foglalt IP-cím<br>-A virtuális gépek több hálózati adapterrel rendelkező<br>Jelöljön ki egy új felhőalapú szolgáltatás felhasználói felületi hello, vagy tekintse meg a túl[szempontok visszaállítása](backup-azure-arm-restore-vms.md#restoring-vms-with-special-network-configurations) speciális hálózati konfigurációk rendelkező virtuális gépek.</ol> |
| kijelölt hello DNS-név már használatban van – adjon egy másik DNS-nevet adja meg, és próbálkozzon újra. |hello Itt a DNS-név hivatkozik toohello felhőszolgáltatás neve (általában végződő. cloudapp.net). Egyedi kell toobe. Ha ezt a hibát észlel, visszaállítás során kell toochoose egy másik Virtuálisgép-nevet. <br><br> Ez a hiba csak az Azure-portálon hello toousers látható. hello Powershellen keresztül visszaállítási művelet fogja sikertelen, mert csak hello lemezek állítja vissza, és nem hoz létre a virtuális gép hello. hello hiba fog tapasztalt, létrehozásakor hello VM explicit módon meg hello lemez visszaállítási művelet után. |
| Nincs megfelelő hello megadott virtuális hálózati konfiguráció – adjon meg egy másik virtuális hálózati konfiguráció, és próbálkozzon újra. |None |
| a megadott hello felhőalapú szolgáltatást használ egy fenntartott IP-cím, amely nem egyezik a hello konfigurációs hello virtuális gép helyreállítása – adjon meg egy másik felhőalapú szolgáltatás, amely fenntartott IP-címet nem használja, vagy válasszon másik helyreállítási pont toorestore a. |None |
| A felhőalapú szolgáltatás elérte az újrapróbálkozási hello művelet egy másik felhőalapú szolgáltatást, vagy egy meglévő végpontot a bemeneti végpontok - számára vonatkozó korlátozást. |None |
| Két különböző régiókban biztonsági mentési tároló és a cél tárfiók - győződjön meg arról, hogy a visszaállítási művelet a megadott tárfiók hello hello egy Azure-régióban van hello mentési tároló. |None |
| A Tárfiók megadott hello visszaállítási művelet nem támogatott – a storage-fiókok csak Basic vagy Standard helyileg redundáns vagy földrajzi redundancia replikációs beállítások támogatottak. Válasszon egy támogatott tárfiókot |None |
| A visszaállítási művelet a megadott Tárfiók típus nem online – győződjön meg arról, hogy online állapotban-e a visszaállítási művelet a megadott tárfiók hello |Ez azért fordulhat elő, az Azure Storage vagy tooan leállás miatt egy átmeneti hiba miatt. Válasszon másik tárolási fiókot. |
| Erőforráscsoport kvóta elérve, mert egyes erőforráscsoportok törölni az Azure-portálon, vagy kérje az Azure támogatási tooincrease hello korlátok. |None |
| Nem létezik a kijelölt alhálózat – válasszon ki egy alhálózatot, amely létezik |None |
| A biztonsági mentési szolgáltatás az előfizetéshez nincs engedélyezési tooaccess erőforrása. |tooresolve a, részben leírt lépéseket követve első visszaállítása lemezek **visszaállítási biztonsági másolatba mentett lemezek** a [kiválasztása a virtuális gép visszaállítási konfigurációs](backup-azure-arm-restore-vms.md#choosing-a-vm-restore-configuration). Ezt követően lépésekkel PowerShell említett [hozzon létre egy virtuális Gépet visszaállított lemezekből](backup-azure-vms-automation.md#create-a-vm-from-restored-disks) toocreate VM teljes visszaállított lemezekről. |

## <a name="backup-or-restore-taking-time"></a>Biztonsági mentési vagy helyreállítási időt vesz igénybe
Ha megjelenik a backup(>12 hours) vagy visszaállítási véve time(>6 hours):
* Megértéséhez [toobackup idő tényezők](backup-azure-vms-introduction.md#total-vm-backup-time) és [toorestore idő tényezők](backup-azure-vms-introduction.md#total-restore-time).
* Győződjön meg arról, hogy kövesse [biztonsági mentéshez ajánlott eljárások](backup-azure-vms-introduction.md#best-practices). 

## <a name="vm-agent"></a>Virtuálisgép-ügynök
### <a name="setting-up-hello-vm-agent"></a>Virtuálisgép-ügynök hello beállítása
Általában hello Virtuálisgép-ügynök már jelen az Azure katalógusában hello a létrehozott virtuális gépeken. A helyszíni adatközpontját a áttelepített virtuális gépek azonban nem áll hello Virtuálisgép-ügynök van telepítve. Ilyen virtuális gépek hello Virtuálisgép-ügynök telepítve explicit módon toobe kell.

Windows virtuális gépek:

* Töltse le és telepítse a hello [ügynök MSI](http://go.microsoft.com/fwlink/?LinkID=394789&clcid=0x409). Rendszergazdai jogosultságok toocomplete hello telepítési van szüksége.
* Klasszikus virtuális gépekhez [hello VM tulajdonság](http://blogs.msdn.com/b/mast/archive/2014/04/08/install-the-vm-agent-on-an-existing-azure-vm.aspx) tooindicate, amely hello ügynök telepítve van. Ez a lépés nincs szükség a virtuális gépek erőforrás-kezelő.

Linux virtuális gépek:

* A telepítés legújabb terjesztési tárházból. A Microsoft **erősen ajánlott** csak a tárház terjesztési ügynök telepítése. A csomag neve információkért tekintse meg az túl[Linux ügynök-tárház](https://github.com/Azure/WALinuxAgent) 
* Klasszikus virtuális gépekhez [hello VM tulajdonság](http://blogs.msdn.com/b/mast/archive/2014/04/08/install-the-vm-agent-on-an-existing-azure-vm.aspx) tooindicate, amely hello ügynök telepítve van. Ez a lépés nincs szükség a virtuális gépek erőforrás-kezelő.

### <a name="updating-hello-vm-agent"></a>Hello Virtuálisgép-ügynök frissítése
Windows virtuális gépek:

* Frissítési hello Virtuálisgép-ügynök újratelepítését hello egyszerűen [Virtuálisgép-ügynök bináris](http://go.microsoft.com/fwlink/?LinkID=394789&clcid=0x409). Azonban meg kell tooensure futtató nincs biztonsági mentési művelet során hello Virtuálisgép-ügynök frissítése folyamatban van.

Linux virtuális gépek:

* Hello utasításokat kövesse a megjelenő [frissítése Linux Virtuálisgép-ügynök](../virtual-machines/linux/update-agent.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).
A Microsoft **erősen ajánlott** frissítési ügynök csak terjesztési tárház keresztül. Nem ajánlott a közvetlenül a githubból hello kód letöltése és frissítése. Ha a terjesztési ügynök legújabb verziójára nem érhető el, lépjen kapcsolatba toodistribution támogatási útmutatást tooinstall ügynök legújabb verziójára. Ellenőrizheti a legújabb [Windows Azure Linux ügynök](https://github.com/Azure/WALinuxAgent/releases) github-tárházban található információk.

### <a name="validating-vm-agent-installation"></a>Virtuálisgép-ügynök telepítésének ellenőrzése
Hogyan toocheck a hello Virtuálisgép-ügynök verziója a Windows virtuális gépek:

1. Jelentkezzen be Azure virtuális gép toohello, és keresse meg a toohello mappa *C:\WindowsAzure\Packages*. Keresse meg hello WaAppAgent.exe fájl található.
2. Kattintson a jobb gombbal a hello fájlt, nyissa meg túl**tulajdonságok**, majd válassza ki a hello **részletek** külön-külön hello termékverzió mező lehet 2.6.1198.718 vagy újabb

## <a name="troubleshoot-vm-snapshot-issues"></a>Virtuális gép pillanatkép problémák elhárítása
Virtuális gép biztonsági mentése a pillanatkép parancsok kiadása toounderlying tárolási támaszkodik. Nincs hozzáférés toostorage vagy késést egy pillanatkép-feladat végrehajtása okozhat hello biztonsági mentési feladat toofail. hello következő pillanatkép-feladat hibát okozhat.

1. Hálózati hozzáférés tooStorage le van tiltva, NSG-t használ<br>
    További tudnivalókért, hogyan túl[hálózati hozzáférés engedélyezése](backup-azure-vms-prepare.md#network-connectivity) tooStorage használatával vagy az engedélyezett IP-címek vagy proxykiszolgálón keresztül.
2. Sql Server biztonsági másolat konfigurált virtuális gépek okozhat a pillanatkép tevékenység késleltetés <br>
   Virtuális gép alapértelmezés szerint a biztonsági mentés problémák VSS teljes biztonsági mentés a Windows virtuális gépeken. A virtuális gépeken futó Sql Server-kiszolgálók és az Sql Server biztonsági másolat úgy van konfigurálva ez a késleltetés a pillanatkép végrehajtása okozhat. Ha biztonsági mentési hibák pillanatkép problémák miatt merülnek állítsa be a következő beállításkulcsot.

   ```
   [HKEY_LOCAL_MACHINE\SOFTWARE\MICROSOFT\BCDRAGENT]
   "USEVSSCOPYBACKUP"="TRUE"
   ```
3. Virtuális gép állapotának jelentés nem megfelelően, mert a virtuális gép RDP a leállítása.  <br>
   Ha hello virtuális gép RDP leállításakor ellenőrizze vissza hello portal a virtuális gép állapotának megfelelően megjelenik-e. Ha nem, akkor állítsa le a virtuális gép hello portálon "Leállítás" lehetőség használatával a VM-irányítópulton.
4. Ha a virtuális gépek négynél több megosztás hello ugyanaz a felhőalapú szolgáltatás, ezért nem négynél több virtuális gép biztonsági mentések indított: hello biztonsági mentési időpontokat be a több biztonsági mentési házirendek toostage hello ugyanannyi időt vesz igénybe. Próbálja meg toospread hello biztonsági mentési indítási idő egy órával közötti házirendek egymástól.
5. Virtuális gép fut, magas Processzor/memória.<br>
   Ha hello virtuális gép fut, a magas CPU usage(>90%) vagy a memória, pillanatkép-feladat várólistára, késleltetett, és végül fog lekérdezi az időtúllépés miatt megszakadt. Próbálja meg igény szerinti biztonsági mentést az ilyen helyzetekben.

<br>

## <a name="networking"></a>Hálózat
Például az összes bővítmény a biztonsági mentés bővítmény toohello nyilvános internet toowork kell hozzáférni. Hozzáférés toohello nem rendelkezik nyilvános interneten is manifest maga különböző módokon:

* hello bővítmény telepítése sikertelen lehet
* hello biztonsági mentési műveletek (például a lemez pillanatkép) sikertelen lehet
* Hello biztonsági mentési művelet hello állapotának megjelenítése sikertelen lehet

nyilvános internet címek feloldására hello szükség van lett csuklós [Itt](http://blogs.msdn.com/b/mast/archive/2014/06/18/azure-vm-provisioning-stuck-on-quot-installing-extensions-on-virtual-machine-quot.aspx). Toocheck hello DNS-konfigurációk hello virtuális hálózat szükséges, és győződjön meg arról, hogy hello Azure URI oldható meg.

Hello névfeloldás helyesen végezték el, ha a hozzáférési toohello Azure IP-címek is kell megadott toobe. toounblock hozzáférés toohello Azure-infrastruktúra, kövesse az alábbi lépések egyikét:

1. Engedélyezési lista hello Azure datacenter IP-címtartományok.
   * Hello listáját [Azure datacenter IP-címek](https://www.microsoft.com/download/details.aspx?id=41653) toobe szerepel az engedélyezési listán.
   * Feloldása IP-címek hello hello [New-NetRoute](https://technet.microsoft.com/library/hh826148.aspx) parancsmag. Futtassa ezt a parancsmagot belül hello Azure virtuális gép, egy emelt szintű PowerShell-ablakban (Futtatás rendszergazdaként).
   * Adja hozzá a szabályok toohello NSG-t (ha van érvényben) tooallow hozzáférés toohello IP-címek.
2. A HTTP-forgalom tooflow elérési utat hoz létre
   * Ha rendelkezik néhány a hálózati korlátozás érvényben (hálózati biztonsági csoport, például) telepítése egy HTTP-proxy kiszolgáló tooroute hello forgalom. Egy HTTP-Proxy kiszolgáló is található lépéseket toodeploy [Itt](backup-azure-vms-prepare.md#network-connectivity).
   * Adja hozzá a szabályok toohello NSG-t (ha van érvényben) tooallow hozzáférés toohello INTERNET a hello HTTP-Proxy.

> [!NOTE]
> DHCP engedélyezve kell lennie az infrastruktúra-szolgáltatási virtuális gép biztonsági mentése toowork hello Vendég.  Ha statikus magánhálózati IP-címe van szüksége, úgy kell beállítania, hello platformon keresztül. hello DHCP-beállítást engedélyezni kell hagyni a virtuális gép hello belül.
> További információk megjelenítéséhez kapcsolatos [egy statikus belső privát IP-cím beállítása](../virtual-network/virtual-networks-reserved-private-ip.md).
>
>
