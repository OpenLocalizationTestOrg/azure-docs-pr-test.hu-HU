---
title: "aaaAzure virtuális gép biztonsági mentése – gyakori kérdések |} Microsoft Docs"
description: "Toocommon kérdésekre vonatkozó: hogyan Azure virtuális gép biztonsági mentési működik, korlátozások és mi történik, ha módosítások toopolicy történik"
services: backup
documentationcenter: 
author: trinadhk
manager: shreeshd
editor: 
keywords: "azure-beli virtuális gép biztonsági mentése, azure-beli virtuális gép visszaállítása, biztonsági mentési szabályzat"
ms.assetid: c4cd7ff6-8206-45a3-adf5-787f64dbd7e1
ms.service: backup
ms.workload: storage-backup-recovery
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 7/18/2017
ms.author: trinadhk;pullabhk;
ms.openlocfilehash: a1ad2cb3a379577a8c4258c8207ce75809e11a4a
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="questions-about-hello-azure-vm-backup-service"></a>Hello Azure virtuális gép biztonsági mentési szolgáltatás kapcsolatos kérdések
Ez a cikk válaszok toocommon kérdések toohelp hello Azure virtuális gép biztonsági mentése összetevők gyorsan tisztában van. Hello válaszok némelyike nincsenek hivatkozások toohello cikket, amely átfogó információkat. A hello is beküldheti hello Azure Backup szolgáltatás kapcsolatos kérdéseket [vitafóruma](https://social.msdn.microsoft.com/forums/azure/home?forum=windowsazureonlinebackup).

## <a name="configure-backup"></a>Biztonsági mentés konfigurálása
### <a name="do-recovery-services-vaults-support-classic-vms-or-resource-manager-based-vms-br"></a>A Recovery Services-tárolók támogatják a klasszikus vagy a Resource Manager alapú virtuális gépeket? <br/>
A Recovery Services-tárolók mindkét modellt támogatják.  A biztonsági másolatot a klasszikus virtuális gépek (hello klasszikus portálon létre), vagy egy erőforrás-kezelő virtuális gép (hello Azure-portálon létrehozott) tooa Recovery Services-tároló készíthet.

### <a name="what-configurations-are-not-supported-by-azure-vm-backup-"></a>Mely konfigurációkat nem támogatja az Azure VM Backup?
Tekintse át a [támogatott operációs rendszereket](backup-azure-arm-vms-prepare.md#supported-operating-system-for-backup) és a [virtuális gépek biztonsági mentésének korlátozásait](backup-azure-arm-vms-prepare.md#limitations-when-backing-up-and-restoring-a-vm)

### <a name="why-cant-i-see-my-vm-in-configure-backup-wizard"></a>Miért nem látom a virtuális gépemet a Biztonsági mentés konfigurálása varázslóban?
A Biztonsági mentés konfigurálása varázslóban az Azure Backup csak a következő virtuális gépeket jeleníti meg:
* Már nem védett - ellenőrizheti a virtuális gépek biztonsági mentési állapotának hello tooVM panelen lesz, és ellenőrzi a biztonsági mentés állapot panelről hello-beállítások menüjében. További tudnivalókért, hogyan túl[a virtuális gépek biztonsági mentési állapotának ellenőrzése](backup-azure-vms-first-look-arm.md#configure-the-backup-job-from-the-vm-management-blade)
* Toosame régió tartozik, mint a virtuális gép

## <a name="backup"></a>Biztonsági mentés
### <a name="will-on-demand-backup-job-follow-same-retention-schedule-as-scheduled-backups"></a>Az igény szerinti biztonsági mentési feladatok ugyanazt a megőrzési ütemezést követik, mint az ütemezett biztonsági mentések?
Nem. Egy igény szerinti biztonsági mentéshez toospecify hello megőrzési időtartamot kell. A megőrzési idő alapértelmezés szerint 30 nap, ha a portálról indítja el a feladatot. 

### <a name="i-recently-enabled-azure-disk-encryption-on-some-vms-will-my-backups-continue-toowork"></a>Nemrég engedélyeztem az Azure Disk Encryption szolgáltatást néhány virtuális gépen. A biztonsági másolatok továbbra is toowork?
Az Azure Backup szolgáltatás tooaccess Key Vault toogive jogosultság szükséges. Ezeket az engedélyeket a PowerShellben adhatja meg, a [PowerShell-dokumentáció](backup-azure-vms-automation.md) *Biztonsági mentés engedélyezése* szakaszában található lépések végrehajtásával.

### <a name="i-migrated-disks-of-a-vm-toomanaged-disks-will-my-backups-continue-toowork"></a>A virtuális gép toomanaged lemezt a lemezek I át. A biztonsági másolatok továbbra is toowork?
Igen, biztonsági mentések zökkenőmentesen működjön, és nincs szükség toore-biztonsági mentés konfigurálása. 

## <a name="restore"></a>Visszaállítás
### <a name="how-do-i-decide-between-restoring-disks-versus-full-vm-restore"></a>A lemezek visszaállítását vagy a teljes virtuális gép visszaállítását válasszam?
Az Azure-beli virtuális gép teljes visszaállítása a visszaállított virtuális gép gyors létrehozásának lehetőségeként is értelmezhető. Állítsa vissza a virtuális gép lehetőséget lemezek hello neve módosul, lemezek, a nyilvános IP-címek, a hálózati adapter által használt tárolók egyedi-e az első virtuális gép létrehozásának részeként létrehozott erőforrások neve. Azt is nem adja hozzá hello a virtuális gép tooavailability csoportjának. 

A lemezek visszaállítását a következőkhöz használhatja:
* Hello időpontjának beállítása például hello méretének módosítása a biztonsági mentési konfigurációhoz pontjáról lekérdezi létrehozott virtuális gép testreszabása
* Adja hozzá a konfigurációkat, amelyek nincsenek jelen hello biztonsági mentése 
* Erőforrások első létrehozott vezérlő hello elnevezési
* A virtuális gép tooavailability csoportjának hozzáadása
* Olyan konfigurációval rendelkezik, amely csak a PowerShell vagy deklaratív sablondefiníció használatával érhető el

## <a name="manage-vm-backups"></a>Virtuális gép biztonsági mentéseinek kezelése
### <a name="what-happens-when-i-change-a-backup-policy-on-vms"></a>Mi történik, ha módosítom a biztonsági mentési szabályzatot a virtuális gépen vagy gépeken?
Egy új házirend alkalmazása esetén a virtuális gép van, ütemezés és a megőrzési hello új házirend követni kell. Ha megőrzési ki van terjesztve, a meglévő helyreállítási pontok lesz megjelölve tookeep őket az új házirend szerint. Megőrzési csökken, ha a hello következő karbantartási feladat a törlésre megjelölve, és törli. 
