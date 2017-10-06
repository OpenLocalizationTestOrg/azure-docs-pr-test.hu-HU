---
title: "aaaAzure biztonsági mentés – gyakori kérdések |} Microsoft Docs"
description: "Toocommon kérdésekre vonatkozó: Azure biztonsági mentési funkciót, beleértve a Recovery Services tárolók, mi azt biztonsági mentését is, hogyan működik, titkosítási és korlátokat. "
services: backup
documentationcenter: 
author: markgalioto
manager: carmonm
editor: 
keywords: "biztonsági mentés és vészhelyreállítás; biztonsági mentési szolgáltatás"
ms.assetid: 1011bdd6-7a64-434f-abd7-2783436668d7
ms.service: backup
ms.workload: storage-backup-recovery
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 7/21/2017
ms.author: markgal;arunak;trinadhk;
ms.openlocfilehash: 3338f7620bcc6ebf53c9c161191f2d8bca1a29da
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="questions-about-hello-azure-backup-service"></a>Hello Azure Backup szolgáltatás kapcsolatos kérdések
Ez a cikk válaszok toocommon kérdések toohelp hello Azure biztonsági mentés összetevők gyorsan tisztában van. Hello válaszok némelyike nincsenek hivatkozások toohello cikket, amely átfogó információkat. Kérdéseit teheti fel Azure biztonsági mentéssel kapcsolatos kattintva **megjegyzések** (toohello jobbra). Megjegyzések a cikk hello alján jelennek meg. Egy Livefyre fiók szükséges toocomment. A hello is beküldheti hello Azure Backup szolgáltatás kapcsolatos kérdéseket [vitafóruma](https://social.msdn.microsoft.com/forums/azure/home?forum=windowsazureonlinebackup).

tooquickly vizsgálat hello szakasz ebben a cikkben a használja hello hivatkozások toohello jobb **ebben a cikkben**.


## <a name="recovery-services-vault"></a>Recovery Services-tároló

### <a name="is-there-any-limit-on-hello-number-of-vaults-that-can-be-created-in-each-azure-subscription-br"></a>Nem minden egyes Azure-előfizetések létrehozott tárolók hello számára vonatkozó korlátozást? <br/>
Igen. 2016 szeptemberétől kezdve előfizetésenként 25 Recovery Services-szolgáltatás vagy biztonsági mentési tár hozható létre. Hozhat létre too25 helyreállítási szolgáltatások tárolók régiónként támogatott az Azure Backup szolgáltatásban. Ha több tárolóra van szüksége, hozzon létre egy további előfizetést.

### <a name="are-there-limits-on-hello-number-of-serversmachines-that-can-be-registered-against-each-vault-br"></a>Vannak-e minden egyes tárolóban elleni regisztrálható kiszolgálók/gépek hello számát vonatkozó korlátozások? <br/>
Igen, akkor regisztrálnia too50 gépek / tároló. Az Azure infrastruktúra-szolgáltatási virtuális gépeket a hello határértéke 200 gépek tárolóban. Ha további gépeket kell tooregister, hozzon létre egy másik tárolóban.

### <a name="if-my-organization-has-one-vault-how-can-i-isolate-one-servers-data-from-another-server-when-restoring-databr"></a>Ha a szervezetem egy tárolóval rendelkezik, hogyan tudom az egyik kiszolgáló adatait elszigetelni egy másik kiszolgálóétól az adatok visszaállításakor?<br/>
Összes kiszolgáló, amely regisztrált toohello azonos tároló helyre tudja állítani az adatok biztonsági mentése más kiszolgálók által hello *használó hello ugyanazt a jelszót*. Ha kiszolgálók amelynek biztonsági mentési adatokat szeretne tooisolate a szervezet egyéb kiszolgálóin, használja a kijelölt jelszót az ezeken a kiszolgálókon. Például a humánerőforrás-kiszolgálók használhatnának egy titkosító hozzáférési kódot, a könyvelési kiszolgálók egy másikat és a tároló kiszolgálók egy harmadikat.

### <a name="can-i-migrate-my-backup-data-or-vault-between-subscriptions-br"></a>Át lehet telepíteni a biztonsági mentési adatokat vagy tárolót az előfizetéseim között? <br/>
Nem. hello tároló egy előfizetés szintjén jön létre, és nem lehet máshoz hozzárendelt tooanother előfizetés létrehozása után.

### <a name="recovery-services-vaults-are-resource-manager-based-are-backup-vaults-classic-mode-still-supported-br"></a>A helyreállítási tárak a Resource Manageren alapulnak. A biztonsági mentési tárak (klasszikus módban) továbbra is támogatottak? <br/>
Az összes meglévő mentési tárolókban lévő hello [klasszikus portál](https://manage.windowsazure.com) továbbra is támogatott toobe. Azonban már nem használható hello klasszikus portál toodeploy új biztonsági mentési tárolóból. A Microsoft azt javasolja, Recovery Services-tárolók használatával minden telepítés esetén, mert a jövőbeli fejlesztések tooRecovery szolgáltatások tárolók, csak alkalmazni. A biztonsági másolatok tárolóját a klasszikus portálon hello toocreate kísérli meg, ha fogja átirányított toohello [Azure-portálon](https://portal.azure.com).

### <a name="can-i-migrate-a-backup-vault-tooa-recovery-services-vault-br"></a>Áttelepítheti a biztonsági mentési tároló tooa Recovery Services-tároló? <br/>
Sajnos nem, nem telepíthetők át a biztonsági mentési tároló tooa Recovery Services-tároló hello tartalmát. Jelenleg is dolgozunk ezen funkción, azonban most még nem elérhető.

### <a name="i-backed-up-my-classic-vms-in-a-backup-vault-can-i-migrate-my-vms-from-classic-mode-tooresource-manager-mode-and-protect-them-in-a-recovery-services-vault"></a>Biztonsági másolatot készítettem a klasszikus virtuális gépemről egy biztonsági mentési tárban. A virtuális gépek áttelepítése a klasszikus mód tooResource Manager mód és a Recovery Services-tároló a védelmüket?
Klasszikus virtuális gép helyreállítási pontokat a biztonságimásolat-tárolóban nem automatikusan telepíti át tooa Recovery Services-tároló hello VM áthelyezése klasszikus tooResource kezelő módban. Kövesse ezeket a lépéseket tootransfer a virtuális gép biztonsági mentések:

1. Hello Backup-tárolóban, lépjen a toohello **védett elemek** fülre, és válassza ki a virtuális gép hello. Kattintson a [Védelem kikapcsolása](backup-azure-manage-vms-classic.md#stop-protecting-virtual-machines) parancsra. Hagyja a *Delete associated backup data* (Társított biztonsági mentési adatok törlése) beállítást **bejelöletlenül**.
2. Hello biztonsági másolat vagy pillanatkép-bővítmény törlése a virtuális gép hello.
3. Hello virtuális gép áttelepítése a klasszikus mód tooResource Manager üzemmódból. Ellenőrizze, hogy a megfelelő toohello virtuális gépek egyben hello tárolási és hálózati információk át tooResource kezelő módban.
4. Recovery Services-tároló létrehozása és konfigurálása a hello biztonsági mentési áttelepítése a virtuális gép **biztonsági mentési** művelet tároló irányítópult felett. Részletes információk a virtuális gép tooa Recovery Services biztonsági mentésével tároló, hello cikke [Azure virtuális gépek védelme Recovery Services-tároló](backup-azure-vms-first-look-arm.md).

## <a name="azure-backup-agent"></a>Az Azure Backup ügynöke
A kérdések részletes listája a [Gyakori kérdések az Azure-beli fájlok és mappák biztonsági mentéséről](backup-azure-file-folder-backup-faq.md) című részben található

## <a name="azure-vm-backup"></a>Azure-beli virtuális gép biztonsági mentése
A kérdések részletes listája a [Gyakori kérdések az Azure-beli virtuális gépek biztonsági mentéséről](backup-azure-vm-backup-faq.md) című részben található

## <a name="back-up-vmware-servers"></a>VMware-kiszolgálók biztonsági mentése

### <a name="can-i-back-up-vmware-vcenter-servers-tooazure"></a>Is biztonsági másolatot a VMware vCenter-kiszolgálók tooAzure?

Igen. Használhatja az Azure Backup Server tooback VMware vCenter és az ESXi tooAzure. Hello támogatott VMware-verzión információkért lásd: hello cikk [Azure Backup Server védelmi mátrix](backup-mabs-protection-matrix.md). Részletes útmutatásért lásd: [VMware-kiszolgáló használata Azure Backup Server tooback](backup-azure-backup-server-vmware.md).


## <a name="azure-backup-server-and-system-center-data-protection-manager"></a>Azure Backup Server és System Center Data Protection Manager
### <a name="can-i-use-azure-backup-server-toocreate-a-bare-metal-recovery-bmr-backup-for-a-physical-server-br"></a>Használható Azure Backup Server toocreate egy operációs rendszer nélküli helyreállítást (BMR) biztonsági mentés a fizikai kiszolgáló? <br/>
Igen.

### <a name="can-i-register-my-dpm-server-toomultiple-vaults-br"></a>A DPM-kiszolgáló toomultiple tárolók tud regisztrálni? <br/>
Nem. A DPM vagy MABS kiszolgáló lehet egy regisztrált tooonly tárolóban.

### <a name="which-version-of-system-center-data-protection-manager-is-supported-br"></a>A System Center – Data Protection Manager melyik verziója támogatott? <br/>
Azt javasoljuk, hogy telepítse hello [legújabb](http://aka.ms/azurebackup_agent) hello legújabb kumulatív frissítéssel (UR) a System Center Data Protection Manager (DPM) az Azure Backup szolgáltatás ügynöke. 2016 augusztusától az Update Rollup 11 a hello legújabb frissítés.

### <a name="i-have-installed-azure-backup-agent-tooprotect-my-files-and-folders-can-i-now-install-system-center-dpm-toowork-with-azure-backup-agent-tooprotect-on-premises-applicationvm-workloads-tooazure-br"></a>Azure Backup agent tooprotect telepítése a fájlokat és mappákat. Most már telepíthető System Center DPM toowork az Azure Backup agent tooprotect helyi alkalmazás vagy Virtuálisgép-munkaterhelések tooAzure? <br/>
toouse Azure Backup a System Center Data Protection Manager (DPM), először a DPM telepítéséhez, és telepítse az Azure Backup szolgáltatás ügynöke. Az itt megadott sorrendben hello Azure biztonsági mentés összetevők telepítése hello Azure Backup szolgáltatás ügynökének DPM együttműködve biztosítja. Hello Azure biztonsági mentési ügynök telepítése a DPM telepítése előtt, azt javasoljuk, vagy nem támogatott.


## <a name="how-azure-backup-works"></a>Az Azure Backup működése
### <a name="if-i-cancel-a-backup-job-once-it-has-started-is-hello-transferred-backup-data-deleted-br"></a>Ha egy biztonsági mentési feladata után kezdett I le, hello átvitt biztonsági mentési adatok törlődik? <br/>
Nem. Hello biztonsági mentési feladat megszakadt, mielőtt hello tárolóban történő továbbított összes adat hello tárolóban marad. Az Azure biztonsági mentés használ egy ellenőrzőpont mechanizmus toooccasionally hozzá ellenőrzőpontokat toohello biztonsági mentési adatok során hello biztonsági mentés. Nincsenek ellenőrzőpontok hello biztonsági mentési adatokat, mert hello következő biztonsági mentési folyamat hello fájlok hello sértetlenségének ellenőrzéséhez. hello következő biztonsági mentési feladat lesz növekményes toohello adatokat előzőleg készült biztonsági másolat. Növekményes biztonsági másolatok csak akkor továbbíthatnak új vagy módosított adatok, amely megfelel a sávszélesség toobetter kihasználását.

Ha megszakítja egy Azure virtuális gép valamely biztonsági mentését, a rendszer a már átvitt adatokat figyelmen kívül hagyja. hello következő biztonsági mentési feladat növekményes adatokat visz át a hello utolsó sikeres biztonsági mentési feladat.

### <a name="are-there-limits-on-when-or-how-many-times-a-backup-job-can-be-scheduledbr"></a>Korlátozva van, hogy mikorra vagy hányszor ütemezhető egy biztonsági mentési feladat?<br/>
Igen. A Windows Server vagy Windows munkaállomások mentése toothree időpontokban naponta biztonsági mentési feladatok is futtathatja. System Center DPM biztonsági mentési feladatok mentést napi tootwice futtathatja. Az infrastruktúra-szolgáltatás virtuális gépei esetén a biztonsági mentési feladat naponta legfeljebb egyszer futtatható. Használhatja az ütemezési házirend a Windows Server vagy a Windows munkaállomás toospecify hello napi vagy heti ütemezést. A System Center DPM-mel napi, heti, havi és évi ütemezéseket határozhat meg.

### <a name="why-is-hello-size-of-hello-data-transferred-toohello-recovery-services-vault-smaller-than-hello-data-i-backed-upbr"></a>Miért van hello mérete kisebb, mint a biztonsági mentés I hello adat tároló Recovery Services hello átvitt adatok toohello?<br/>
 Minden hello adatok biztonsági mentése az Azure Backup szolgáltatás ügynökének vagy az SCDPM vagy az Azure Backup Server tömörített és átvitele előtt titkosítja. Miután hello tömörítés és a titkosítás alkalmazásakor hello hello mentési tároló adatai 30-40 %-os kisebb.

## <a name="what-can-i-back-up"></a>Miről tudok biztonsági mentést készíteni?
### <a name="which-operating-systems-do-azure-backup-support-br"></a>Mely operációs rendszereket támogatja az Azure Backup? <br/>
Azure biztonsági mentés támogatja a biztonsági mentési operációs rendszerek listája a következő hello: fájlok és mappák és munkaterhelés-alkalmazások Azure Backup Server és System Center Data Protection Manager (DPM) használatával védett.

| Operációs rendszer | Platform | SKU |
|:--- | --- |:--- |
| Windows 8 és a legújabb szervizcsomagok |64 bit |Enterprise, Pro |
| Windows 7 és a legújabb szervizcsomagok |64 bit |Ultimate, Enterprise, Professional, Home Premium, Home Basic, Starter |
| Windows 8.1 és a legújabb szervizcsomagok |64 bit |Enterprise, Pro |
| Windows 10 |64 bit |Enterprise, Pro, Home |
| Windows Server 2016 |64 bit |Standard, Datacenter, Essentials |
| Windows Server 2012 R2 és a legújabb szervizcsomagok |64 bit |Standard, Datacenter, Foundation |
| Windows Server 2012 és a legújabb szervizcsomagok |64 bit |Datacenter, Foundation, Standard |
| Windows Storage Server 2016 és a legújabb szervizcsomagok |64 bit |Standard, Workgroup | 
| Windows Storage Server 2012 R2 és a legújabb szervizcsomagok |64 bit |Standard, Workgroup |
| Windows Storage Server 2012 és a legújabb szervizcsomagok |64 bit |Standard, Workgroup |
| Windows Server 2012 R2 és a legújabb szervizcsomagok |64 bit |Essential |
| Windows Server 2008 R2 SP1 |64 bit |Standard, Enterprise, Datacenter, Foundation |
| Windows Server 2008 SP2 |64 bit |Standard, Enterprise, Datacenter, Foundation |

**Azure VM Backup esetében:**

* **Linux**: Az Azure Backup az [Azure által támogatott disztribúciókat](../virtual-machines/linux/endorsed-distros.md) támogatja, a Core OS Linux kivételével.  Más kerüljön-a-saját-Linux terjesztéseket is előfordulhat, hogy működni, amíg hello Virtuálisgép-ügynök hello virtuális gépen érhető el, a Python létezik támogatása.
* **Windows Server**: A Windows Server 2008 R2-nél régebbi verziók nem támogatottak.


### <a name="is-there-a-limit-on-hello-size-of-each-data-source-being-backed-up-br"></a>Van korlátja a biztonsági mentés alatt minden adatforrás hello mérete? <br/>
Hello adatmennyiség készíthet biztonsági mentést tooa tároló korlátozva van. Azure biztonsági mentés hello adatforrás hello maximális mérete korlátozza, azonban ezek a korlátozások nagy. 2015. augusztus frissítésétől hello maximális hello támogatott operációs rendszerek adatforrás mérete:

| Sorszám | Operációs rendszer | Adatforrás maximális mérete |
|:---:|:--- |:--- |
| 1 |Windows Server 2012 vagy újabb |54 400 GB |
| 2 |Windows 8 vagy újabb |54 400 GB |
| 3 |Windows Server 2008, Windows Server 2008 R2 |1700 GB |
| 4 |Windows 7 |1700 GB |

hello a következő táblázat ismerteti, hogyan minden adatforrás mérete határozza meg.

| Adatforrás | Részletek |
|:---:|:--- |
| Kötet |hello adatmennyiség egyetlen kötetéről a kiszolgáló vagy az ügyfél gépek biztonsági mentése folyamatban |
| Hyper-V virtuális gép |Összesített összes hello a VHD-k hello virtuális gép biztonsági mentése folyamatban |
| Microsoft SQL Server-adatbázis |A biztonsági mentés alatt álló egyetlen SQL-adatbázis mérete |
| Microsoft SharePoint |Egy SharePoint-farm biztonsági mentés alatt található tartalom és konfigurációs adatbázisok hello összege |
| Microsoft Exchange |Egy biztonsági mentés alatt álló Exchange-kiszolgáló összes Exchange-adatbázisa |
| BMR/Rendszerállapot |BMR vagy rendszerállapot biztonsági mentése folyamatban hello gép minden egyes másolata |

Azure virtuális gép biztonsági mentése, az egyes virtuális gépek lehet too16 adatok lemezeket egyes adatok lemez alatt méretet 1023 GB-os vagy annál kisebb. 

## <a name="retention-policy-and-recovery-points"></a>Adatmegőrzési szabály és helyreállítási pontok
### <a name="is-there-a-difference-between-hello-retention-policy-for-dpm-and-windows-serverclient-that-is-on-windows-server-without-dpmbr"></a>Hello adatmegőrzési a DPM és a Windows Server-ügyfél van (Ez azt jelenti, hogy a Windows Server a DPM nélkül)?<br/>
Nem, a DPM és a Windows Server vagy Windows-ügyfél is rendelkezik napi, heti, havi és évi megtartási házirendekkel.

### <a name="can-i-configure-my-retention-policies-selectively--ie-configure-weekly-and-daily-but-not-yearly-and-monthlybr"></a>Beállíthatom a megtartási házirendeket szelektíven – pl. konfigurálom hetente és naponta, de nem évente és havonta?<br/>
Igen, hello Azure biztonsági mentés megőrzési struktúra lehetővé teszi a toohave teljesen rugalmasan a definiáló hello adatmegőrzési a követelményeknek.

### <a name="can-i-schedule-a-backup-at-6pm-and-specify-retention-policies-at-a-different-timebr"></a>„Ütemezhetek egy biztonsági mentést” este 6 órára, és megadhatok „megtartási házirendeket” egy másik időpontra?<br/>
Nem. Megtartási házirendeket csak biztonsági mentési pontokon lehet alkalmazni. A kép a következő hello hello adatmegőrzési 12 óra és 18: 00 órakor biztonsági mentés van megadva. <br/>

![Biztonsági mentés és megőrzés ütemezése](./media/backup-azure-backup-faq/Schedule.png)
<br/>

### <a name="if-a-backup-is-retained-for-a-long-duration-does-it-take-more-time-toorecover-an-older-data-point-br"></a>Ha a biztonsági másolat egy hosszú ideig őrzi meg, időt vesz igénybe több idő toorecover egy régebbi adatpont? <br/>
Nem – hello idő toorecover hello legrégebbi vagy legújabb pont hello van hello azonos. Minden helyreállítási pont teljes pontként viselkedik.

### <a name="if-each-recovery-point-is-like-a-full-point-does-it-impact-hello-total-billable-backup-storagebr"></a>Ha például egy teljes pont egyes helyreállítási pontok, nem azt befolyásolja a hello számlázható biztonsági mentési tárolókészlet?<br/>
A tipikus hosszú távú megtartási pontok az adatok biztonsági másolatát teljes pontokként tárolják. hello teljes pontok tárolási *nem elég hatékony* , de egyszerűbb és gyorsabb toorestore. Növekményes példányokra tárolási *hatékony* , de használatba toorestore adatait, amely hatással van a helyreállítási idő láncolata. Az Azure biztonsági mentés tárolási architektúra ad meg mindkét világot legjobb hello optimális gyors visszaállítások adattárolás és nélül alacsony tárolási költségek. Ez az adattárolási módszer biztosítja, hogy a bemenő és kimenő sávszélesség is hatékonyan van felhasználva. Mindkét hello időn adatok tárolási és hello szükséges toorecover hello adatok, tooa minimális tárolódik. Megtudhatja, hogy miért hatékonyak a [növekményes biztonsági mentések](https://azure.microsoft.com/blog/microsoft-azure-backup-save-on-long-term-storage/).

### <a name="is-there-a-limit-on-hello-number-of-recovery-points-that-can-be-createdbr"></a>Nem hozható létre helyreállítási pontok számát hello korlátozást?<br/>
Másolatot védett példányonként too9999 helyreállítási pontokat hozhat létre. Egy védett példány egy számítógép, a server (fizikai vagy virtuális) vagy a munkaterhelés konfigurált tooback adatok tooAzure fel. További információkért lásd: hello magyarázatokat [biztonsági mentési és adatmegőrzési](./backup-introduction-to-azure-backup.md#backup-and-retention), és [Mi az, hogy a védett példánya](./backup-introduction-to-azure-backup.md#what-is-a-protected-instance)?

### <a name="how-many-recoveries-can-i-perform-on-hello-data-that-is-backed-up-tooazurebr"></a>Hány helyreállítási végrehajthatok hello adatok biztonsági mentése tooAzure?<br/>
Az Azure Backup a helyreállítások hello száma korlátozva van.

### <a name="when-restoring-data-do-i-pay-for-hello-egress-traffic-from-azure-br"></a>Adatok helyreállításakor kell fizetni hello kimenő forgalom az Azure-ból? <br/>
Nem. A helyreállítások szabadon, és nem kell fizetnie hello kilépő forgalmat.

## <a name="azure-backup-encryption"></a>Azure Backup-titkosítás
### <a name="is-hello-data-sent-tooazure-encrypted-br"></a>Hello adatokat küldött titkosított tooAzure? <br/>
Igen. Adatok titkosítása a gépen hello a helyi kiszolgáló vagy ügyfél/SCDPM használatával AES256 és hello adatküldést biztonságos HTTPS-kapcsolaton keresztül.

### <a name="is-hello-backup-data-on-azure-encrypted-as-wellbr"></a>Hello biztonsági mentési adatok van titkosítva, valamint Azure?<br/>
Igen. hello adatküldés tooAzure (inaktív) titkosított marad. A Microsoft nem fejti vissza a biztonsági mentési adatok hello bármely pontján. Ha biztonsági másolatot az Azure virtuális gép, Azure Backup szolgáltatás titkosítási hello virtuális gép támaszkodik. Például ha a virtuális gép titkosítása a Azure Disk Encryption, vagy olyan titkosítási technológiákat, Azure biztonsági mentés használja a titkosítási toosecure az adatok.

### <a name="what-is-hello-minimum-length-of-encryption-key-used-tooencrypt-backup-data-br"></a>Mi az titkosítási kulcs minimális hosszát hello használt tooencrypt biztonsági mentési adatokat? <br/>
hello titkosítási kulcs legalább 16 karakter lehet, az Azure Backup szolgáltatás ügynöke használata esetén. Azure virtuális gépeken nincs nincs korlát toolength Azure KeyVault által használt kulcsok. 

### <a name="what-happens-if-i-misplace-hello-encryption-key-can-i-recover-hello-data-or-can-microsoft-recover-hello-data-br"></a>Mi történik, ha szeretnék feljegyezte hello titkosítási kulcsot? Helyreállíthatók hello adatok (vagy) helyreállíthatja a Microsoft hello adatokat? <br/>
csak a hello ügyfél helyszíni hello kulcs használt tooencrypt hello biztonsági mentési adatok jelen. A Microsoft nem tart fenn egy másolatot, az Azure-ban, és nincs semmilyen hozzáférési toohello kulcsa. Hello ügyfél misplaces hello kulcsot, ha az Microsoft hello biztonsági mentési adatok nem állíthatók vissza.
