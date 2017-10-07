---
title: "a Recovery Services-tárolók aaaOverview |} Microsoft Docs"
description: "Áttekintés és Recovery Services-tárolók és az Azure mentési tárolókban összehasonlítása."
services: backup
documentationcenter: " "
author: markgalioto
manager: carmonm
ms.assetid: 38d4078b-ebc8-41ff-9bc8-47acf256dc80
ms.service: backup
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: storage-backup-recovery
ms.date: 05/15/2017
ms.author: markgal;arunak
ms.openlocfilehash: 77dd9ece7fe86429cc6f9a47a68b5150a1f4af71
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="recovery-services-vaults-overview"></a>Helyreállítási szolgáltatások tárolók áttekintése

Ez a cikk ismerteti a Recovery Services-tároló hello funkcióit. Recovery Services-tároló egy tárolási egység, amely az adatok Azure-ban. hello adata általában másolja az adatokat, vagy a virtuális gépek (VM), a munkaterhelés, a kiszolgálókra vagy munkaállomásokra konfigurációs adatait. Recovery Services-tároló egy biztonsági mentési tárolót hello erőforrás-kezelő verziója telepítve. A Microsoft támogatja toouse Recovery Services-tárolók és tooconvert bármely a mentési tárolókat tooRecovery szolgáltatások tárolók.

## <a name="what-is-a-recovery-services-vault"></a>Mi az a Recovery Services-tároló?

Recovery Services-tároló egy online tárolási entitás az Azure-ban használt toohold adatok, például a biztonsági másolat, a helyreállítási pontokat és a biztonsági mentési házirendek. Recovery Services-tárolók toohold biztonsági mentési adatok például IaaS virtuális gépeket (Linux- vagy Windows-) és az Azure SQL-adatbázisok különböző Azure-szolgáltatásokhoz használhat. Helyreállítási szolgáltatások tárolók támogatása a System Center DPM, a Windows Server, Azure Backup Server, és több. Helyreállítási szolgáltatások tárolók egyszerűen tooorganize révén a biztonsági mentési adatokat, ugyanakkor minimalizálja a kezelési terhelés mellett.

Belül egy Azure-előfizetés tetszés szerinti számú Recovery Services-tárolók is létrehozhat.

## <a name="comparing-recovery-services-vaults-and-backup-vaults"></a>Összehasonlító Recovery Services-tárolók és a mentési tárolókat

Recovery Services-tárolók hello Azure Resource Manager modellt az Azure-ban alapulnak, mivel a biztonsági mentési tárolók hello Azure Service Manager modell alapulnak. Amikor frissít egy biztonsági mentési tároló tooa Recovery Services-tároló, hello biztonsági mentési adatok sértetlenek maradnak alatt és után hello frissítési folyamatát. Helyreállítási szolgáltatások tárolók funkciók nem érhető el a biztonsági mentési tárolóból, például a biztosítására:

- **Képességek toohelp biztonságos biztonsági mentési adatok fokozott**: A Recovery Services-tárolók, Azure biztonsági mentési biztonságot nyújt képességek tooprotect felhőbeli biztonsági másolatok. Biztonsági funkció ellenőrizze, hogy a biztonsági másolatok biztonságos, és biztonságosan tárolt adatok helyreállításához felhőbeli biztonsági mentések akkor is, ha az éles tárhely és a biztonsági mentés kiszolgálók kerülnek veszélybe. [További információ](backup-azure-security-feature.md)

- **A hibrid informatikai környezetben központi figyelését**: A Recovery Services-tárolók, figyelheti nem csak a [Azure IaaS virtuális gépeket](backup-azure-manage-vms.md) , hanem a [a helyszíni eszközök](backup-azure-manage-windows-server.md#manage-backup-items) az egy központi portál. [További információ](http://azure.microsoft.com/blog/alerting-and-monitoring-for-azure-backup)

- **Szerepköralapú hozzáférés-vezérlés (RBAC)**: RBAC biztosít a minden részletre kiterjedő felügyeleti hozzáférés az Azure-ban. [Azure biztosít a különféle beépített szerepkörök](../active-directory/role-based-access-built-in-roles.md), és az Azure biztonsági mentés van három [beépített szerepkörök toomanage helyreállítási pontok](backup-rbac-rs-vault.md). Helyreállítási szolgáltatások tárolók kompatibilisek-e az RBAC, amely korlátozza a biztonsági mentés, és felhasználói szerepkörök hozzáférés meghatározott toohello visszaállítja. [További információ](backup-rbac-rs-vault.md)

- **Az összes konfiguráció Azure virtuális gépek védelme**: Recovery Services-tárolók a Resource Manager-alapú virtuális gépek többek között a Premium lemezek, a felügyelt lemezek és a titkosított virtuális gépek védelme. A biztonsági mentési tároló tooa frissítése Recovery Services tároló hello lehetőség tooupgrade a Service Manager-alapú virtuális gépek tooResource Manager-alapú virtuális gépek által biztosított. Hello tároló a frissítés során a Service Manager-alapú virtuális gép helyreállítási pontok megőrzése, és konfigurálja a védelmet, a hello frissítése (Resource Manager-kompatibilis) virtuális gépeket. [További információ](http://azure.microsoft.com/blog/azure-backup-recovery-services-vault-ga)

- **Infrastruktúra-szolgáltatási virtuális gépek azonnali visszaállítási**: használatával Recovery Services-tárolók, helyreállíthatja a fájlok és mappák helyreállítása nélkül egy infrastruktúra-szolgáltatási virtuális hello teljes virtuális gép, amely lehetővé teszi a gyorsabb visszaállítás. Infrastruktúra-szolgáltatási virtuális gépek azonnali visszaállítási Windows és Linux virtuális gépek érhető el. [További információ](http://azure.microsoft.com/blog/instant-file-recovery-from-azure-linux-vm-backup-using-azure-backup-preview)

## <a name="managing-your-recovery-services-vaults-in-hello-portal"></a>A Recovery Services-tárolók hello portálon kezelése
Létrehozását és kezelését a Recovery Services-tárolók hello Azure-portálon az nem egyszerű, mert a biztonsági mentési szolgáltatás hello hello Azure beállítások panel integrálva van. Ez az integráció azt jelenti, hogy hozzon létre, vagy kezelheti a Recovery Services-tároló *hello célszolgáltatás hello környezetében*. Például tooview hello helyreállítási pontok a virtuális gépek, válassza ki azt, és kattintson a **biztonsági mentés** hello-beállítások panelen. hello biztonsági mentési információt adott toothat a virtuális gép jelenik meg. A példában a következő hello **ContosoVM** hello hello virtuális gép neve. **ContosoVM-demovault** Recovery Services-tároló hello hello neve. Tooremember hello hello helyreállítási pontokat tároló hello Recovery Services-tároló neve nem szükséges, ez az információ elérhető hello virtuális gépet.  

![Helyreállítási szolgáltatások tároló tartalmazó virtuális gép részletei](./media/backup-azure-recovery-services-vault-overview/rs-vault-in-context.png)

Ha több kiszolgáló védi hello azonos Recovery Services-tároló, lehet, hogy a Recovery Services-tároló hello több logikai toolook. Keresse meg az összes Recovery Services-tárolók hello előfizetésben, és válassza ki az egyiket hello listából.

hello következő szakaszok tartalmazzák, amely azt ismertetik, hogyan toouse a Recovery Services tároló minden tevékenység típusú hivatkozások tooarticles.

### <a name="back-up-data"></a>Adatok biztonsági mentése
- [Készítsen biztonsági másolatot az Azure virtuális gép](backup-azure-vms-first-look-arm.md)
- [Biztonsági másolatot készíthet a Windows Server és a Windows munkaállomás](backup-try-azure-backup-in-10-mins.md)
- [Készítsen biztonsági másolatot a DPM-munkaterhelések tooAzure](backup-azure-dpm-introduction.md)
- [Készítse elő a munkaterhelés Azure Backup Server mentése tooback](backup-azure-microsoft-azure-backup.md)

### <a name="manage-recovery-points"></a>Helyreállítási pontok kezelése
- [Azure virtuális gép biztonsági mentések kezelése](backup-azure-manage-vms.md)
- [Fájlok és mappák kezelése](backup-azure-manage-windows-server.md)

### <a name="restore-data-from-hello-vault"></a>Adatok visszaállítása hello tárolóból
- [Egy Azure virtuális gép egyes fájlok helyreállítása](backup-azure-restore-files-from-vm.md)
- [Állítsa vissza az Azure virtuális gép](backup-azure-arm-restore-vms.md)

### <a name="secure-hello-vault"></a>Biztonságos hello tároló
- [A Recovery Services-tárolók a felhő biztonsági mentési adatok védelme](backup-azure-security-feature.md)



## <a name="next-steps"></a>Következő lépések
A következő cikk hello használata:</br>
[Készítsen biztonsági másolatot az infrastruktúra-szolgáltatási virtuális gép](backup-azure-arm-vms-prepare.md)</br>
[Készítsen biztonsági másolatot az Azure Backup Server](backup-azure-microsoft-azure-backup.md)</br>
[Készítsen biztonsági másolatot a Windows Server](backup-configure-vault.md)
