---
title: "aaaAzure Backup szolgáltatás ügynökének – gyakori kérdések |} Microsoft Docs"
description: "Toocommon kérdésekre vonatkozó: hogyan hello Azure biztonságimásolat-készítő ügynök működik, a biztonsági mentési és adatmegőrzési korlátok."
services: backup
documentationcenter: 
author: trinadhk
manager: shreeshd
editor: 
keywords: "biztonsági mentés és vészhelyreállítás; biztonsági mentési szolgáltatás"
ms.assetid: 778c6ccf-3e57-4103-a022-367cc60c411a
ms.service: backup
ms.workload: storage-backup-recovery
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 7/18/2017
ms.author: trinadhk;pullabhk;
ms.openlocfilehash: bdefb4efb39301f38cdf692bdc93c841a2bbb441
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="questions-about-hello-azure-backup-agent"></a>Hello Azure Backup szolgáltatás ügynökének kapcsolatos kérdések
Ez a cikk válaszok toocommon kérdések toohelp hello Azure Backup agent-összetevők gyorsan tisztában van. Hello válaszok némelyike nincsenek hivatkozások toohello cikket, amely átfogó információkat. A hello is beküldheti hello Azure Backup szolgáltatás kapcsolatos kérdéseket [vitafóruma](https://social.msdn.microsoft.com/forums/azure/home?forum=windowsazureonlinebackup).

## <a name="configure-backup"></a>Biztonsági mentés konfigurálása
### <a name="where-can-i-download-hello-latest-azure-backup-agent-br"></a>Ha letölthető hello legújabb Azure Backup szolgáltatás ügynökének? <br/>
Letöltheti a legújabb ügynök hello szeretne biztonsági másolatot készíteni a Windows Server, a System Center DPM vagy a Windows-ügyfél, a [Itt](http://aka.ms/azurebackup_agent). Ha azt szeretné, hogy a virtuális gépek tooback, használja a hello Virtuálisgép-ügynök (amely automatikusan telepíti a megfelelő mellékre hello). Virtuálisgép-ügynök hello már létezik hello Azure katalógusában alapján létrehozott virtuális gépeken.

### <a name="when-configuring-hello-azure-backup-agent-i-am-prompted-tooenter-hello-vault-credentials-do-vault-credentials-expire"></a>Hello Azure Backup szolgáltatás ügynökének konfigurálásakor vagyok felszólító tooenter hello tárolói hitelesítő adatokat. A tároló hitelesítő adatai lejárnak?
Igen, 48 óra elteltével lejár hello tárolói hitelesítő adatokat. Hello fájl lejár, ha a naplófájlok toohello az Azure portál és a letöltési hello tároló hitelesítő adatait a tárolóból.

### <a name="what-types-of-drives-can-i-back-up-files-and-folders-from-br"></a>Milyen típusú meghajtókon lévő fájlokról és mappákról tudok biztonsági másolatot készíteni? <br/>
Nem készíthet biztonsági másolatot a következő meghajtók vagy kötetek hello:

* Cserélhető adathordozók: a biztonsági mentés minden forráselemének rögzített állapotban kell lennie.
* Csak olvasható kötetek: hello kötet a hello kötet árnyékmásolata másolási szolgáltatás (VSS) toofunction írhatónak kell lennie.
* Kapcsolat nélküli kötetek: hello kötet online VSS toofunction kell lennie.
* Hálózati megosztás: hello kötetnek kell lennie a helyi toohello server toobe biztonsági mentése online biztonsági mentéssel.
* A BitLocker által védett kötetek: hello kötet fel kell oldani hello biztonsági mentés előtt.
* : Fájl System NTFS az azonosító Hello fájlrendszer támogatja.

### <a name="what-file-and-folder-types-can-i-back-up-from-my-serverbr"></a>A kiszolgálón lévő milyen fájlokról és mappákról készíthetek biztonsági másolatot?<br/>
hello a következő típusok támogatottak:

* Titkosított
* Tömörített
* Ritka
* Tömörített + ritka
* Rögzített hivatkozások: Nem támogatott, átugorva
* Újraelemzési pont: Nem támogatott, átugorva
* Titkosított + ritka: Nem támogatott, átugorva
* Tömörített stream: Nem támogatott, átugorva
* Ritka stream: Nem támogatott, átugorva

### <a name="can-i-install-hello-azure-backup-agent-on-an-azure-vm-already-backed-by-hello-azure-backup-service-using-hello-vm-extension-br"></a>Telepíthető egy Azure virtuális gépen hello Azure Backup szolgáltatás használatával hello Virtuálisgép-bővítmény által már támogatott hello Azure Backup szolgáltatás ügynökének? <br/>
Abszolút. Azure biztonsági mentés biztosít a virtuális gép biztonsági mentése Azure virtuális gépek hello Virtuálisgép-bővítmény használatával. tooprotect fájlok és mappák hello vendég Windows operációs rendszer telepítését hello vendég Windows operációs rendszer hello Azure Backup szolgáltatás ügynöke.

### <a name="can-i-install-hello-azure-backup-agent-on-an-azure-vm-tooback-up-files-and-folders-present-on-temporary-storage-provided-by-hello-azure-vm-br"></a>Telepíthető a fájlok és mappák hello Azure virtuális gép által biztosított ideiglenes tárolási megtalálható egy Azure virtuális gép tooback hello Azure Backup szolgáltatás ügynökének? <br/>
Igen. Hello Azure Backup szolgáltatás ügynökének telepíthető hello vendég Windows operációs rendszer, és készítsen biztonsági másolatot a fájlok és mappák tootemporary tároló. A biztonsági mentési feladatok sikertelenek lesznek, ha törli az ideiglenes tároló adatait. Is ha hello ideiglenes tárolási adat törölve lett, csak helyreállíthatja toonon-tárolóba.

### <a name="whats-hello-minimum-size-requirement-for-hello-cache-folder-br"></a>Mi az a minimális méretkövetelményt hello hello gyorsítótármappa? <br/>
hello gyorsítótár mappájának mérete hello hello adatmennyiséget biztonsági másolatot a határozza meg. A gyorsítótár mappája 5 %-a szükséges adatok tárolási hello helyet kell lennie.

### <a name="how-do-i-register-my-server-tooanother-datacenterbr"></a>Hogyan regisztrálhatom a kiszolgáló tooanother datacenter?<br/>
Toohello datacenter hello tároló toowhich regisztrálva van a biztonsági mentési adatokat küldi el. hello legegyszerűbb módja toochange hello datacenter toouninstall hello ügynök, és újra hello ügynököt, és regisztrálja a tooa új tárolóra, toodesired datacenter tartozik.

### <a name="does-hello-azure-backup-agent-work-on-a-server-that-uses-windows-server-2012-deduplication-br"></a>Hello Azure Backup agent működik egy olyan kiszolgálón, használja a Windows Server 2012 adatdeduplikáció használ? <br/>
Igen. hello ügynökszolgáltatás konvertálja az hello deduplikált adatok toonormal adatokat, amikor előkészíti hello biztonsági mentési műveletet. Azt, majd optimalizálja a biztonsági mentéshez hello adatok, hello adatait, és titkosítja ezután elküldi a hello titkosított adatok toohello online biztonsági mentési szolgáltatás.

## <a name="backup"></a>Biztonsági mentés
### <a name="how-do-i-change-hello-cache-location-specified-for-hello-azure-backup-agentbr"></a>Hogyan változtathatom hello gyorsítótár helyére hello Azure Backup szolgáltatás ügynökének?<br/>
A következő toochange hello gyorsítótár helye hello használata.

1. Állítsa le a hello biztonsági mentés motor hajtja végre a következő parancsot egy rendszergazda jogú parancssort a hello:

    ```PS C:\> Net stop obengine``` 
  
2. Hello fájlok ne kerüljenek. Ehelyett másolása hello gyorsítótár terület mappa tooa olyan meghajtót, amelyen elegendő lemezterület. Miután meggyőződött arról a biztonsági mentések dolgozik hello új gyorsítótár-terület hello hello eredeti gyorsítótár-terület távolíthatja el.
3. A következő beállításjegyzék-bejegyzések hello elérési toohello új gyorsítótár terület mappa hello frissítése.<br/>

    | Beállításjegyzékbeli elérési út | Beállításjegyzék kulcsa | Érték |
    | --- | --- | --- |
    | `HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows Azure Backup\Config` |ScratchLocation |*Új gyorsítótár-mappa helye* |
    | `HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows Azure Backup\Config\CloudBackupProvider` |ScratchLocation |*Új gyorsítótár-mappa helye* |

4. Indítsa újra a hello biztonsági mentés motor hajtja végre a következő parancsot egy rendszergazda jogú parancssort a hello:

    ```PS C:\> Net start obengine```

Biztonsági másolat létrehozása hello hello új gyorsítótár helyét sikeres befejezése után eltávolíthatja a hello eredeti gyorsítótár mappája.


### <a name="where-can-i-put-hello-cache-folder-for-hello-azure-backup-agent-toowork-as-expectedbr"></a>Hová helyezhetik a várt módon hello Azure Backup szolgáltatás ügynökének toowork hello gyorsítótármappája?<br/>
hello hello gyorsítótár mappája a következő helyeken nem ajánlott:

* A hálózati megosztásra vagy cserélhető adathordozóra: hello gyorsítótármappa kell lennie, amelyet a biztonsági mentés online biztonsági mentés a helyi toohello kiszolgáló. A hálózati helyek és a cserélhető adathordozók, például az USB-meghajtók nem támogatottak.
* Kapcsolat nélküli kötetek: hello gyorsítótármappa online biztonsági mentés várt használata az Azure Backup szolgáltatás ügynöke kell lennie.

### <a name="are-there-any-attributes-of-hello-cache-folder-that-are-not-supportedbr"></a>Vannak-e bármilyen attribútumok hello gyorsítótár-mappa nem támogatott?<br/>
hello következő attribútumok vagy ezek kombinációi nem támogatottak a hello gyorsítótár mappája:

* Titkosított
* Deduplikált
* Tömörített
* Ritka
* Újraelemzési pont

hello gyorsítótármappa és hello metaadatok VHD nincs hello Azure Backup szolgáltatás ügynökének hello szükséges attribútumait.

### <a name="is-there-a-way-tooadjust-hello-amount-of-bandwidth-used-by-hello-backup-servicebr"></a>A módszer tooadjust hello méretű hello biztonsági mentési szolgáltatás által használt sávszélesség van?<br/>
  Igen, használja a hello **tulajdonságainak módosítása** hello Backup szolgáltatás ügynökének tooadjust sávszélesség beállítást. Beállíthatja a sávszélesség használata esetén a sávszélesség és hello alkalommal hello mennyisége. A részletes útmutatást lásd: **[Hálózatszabályozás engedélyezése](backup-configure-vault.md#enable-network-throttling)**.

## <a name="manage-backups"></a>Biztonsági másolatok kezelése
### <a name="what-happens-if-i-rename-a-windows-server-that-is-backing-up-data-tooazurebr"></a>Mi történik, ha szeretnék, hogy a biztonsági adatok tooAzure másolatot készít a Windows server nevezze át?<br/>
Ha átnevez egy kiszolgálót, minden aktuálisan konfigurált biztonsági mentés leáll.
Hello mentési tárolóhoz regisztrált hello hello kiszolgáló új nevét. Hello tárolóhoz regisztrált hello új nevet, hello első biztonsági mentés esetén a *teljes* biztonsági mentés. Ha biztonsági másolatba mentett toohello tároló hello régi kiszolgáló nevével toorecover adatok van szüksége, használja a hello [ **egy másik kiszolgáló** ](backup-azure-restore-windows-server.md#use-instant-restore-to-restore-data-to-an-alternate-machine) hello beállítása **adatok helyreállítása** varázsló.

### <a name="what-is-hello-maximum-file-path-length-that-can-be-specified-in-backup-policy-using-azure-backup-agent-br"></a>Mi az a hello maximális fájl elérési útra, amely a biztonsági mentési házirend Azure Backup-ügynök használatával lehet megadni? <br/>
Az Azure Backup ügynök az NTFS-re hagyatkozik. Hello [filepath hosszúság megadása korlátozza hello Windows API](https://msdn.microsoft.com/library/aa365247.aspx#fully_qualified_vs._relative_paths). Ha azt szeretné, hogy tooprotect hello fájlok a fájl elérési útja hosszabb, mint hello Windows API által megengedett érték hossza, készítsen biztonsági másolatot hello szülőmappa vagy hello a lemezmeghajtó.  

### <a name="what-characters-are-allowed-in-file-path-of-azure-backup-policy-using-azure-backup-agent-br"></a>Mely karakterek engedélyezettek az Azure Backup ügynököt használó Azure Backup házirend elérési útjában? <br>
 Az Azure Backup ügynök az NTFS-re hagyatkozik. Ez engedélyezi az [NTFS által támogatott karakterek](https://msdn.microsoft.com/library/aa365247.aspx#naming_conventions) használatát a fájl meghatározásának részeként. 
 
### <a name="i-receive-hello-warning-azure-backups-have-not-been-configured-for-this-server-even-though-i-configured-a-backup-policy-br"></a>Hello figyelmeztetést, a "Azure biztonsági mentések nincsenek konfigurálva ehhez a kiszolgálóhoz" jelenik meg akkor is, ha a biztonsági mentési házirend beállítása <br/>
Ez a figyelmeztetés hello biztonsági mentés ütemezése beállítások hello helyi kiszolgálón tárolt csak azonos hello hello biztonságimásolat-tárolóban tárolt beállítások hello következik be. Hello server vagy hello beállítások helyreállított tooa ismert jó állapotban volt, amikor a mentési ütemezések hello szinkronizálási elveszhet. Ha ezt a figyelmeztetést kap [konfigurálja újra a biztonsági mentési házirend hello](backup-azure-manage-windows-server.md) , majd **futtatása biztonsági másolat készítése most** tooresynchronize hello helyi kiszolgáló Azure.
