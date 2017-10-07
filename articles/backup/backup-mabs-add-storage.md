---
title: "az Azure Backup Server 2 Modern Backup-tárhelyre aaaUse |} Microsoft Docs"
description: "További tudnivalók az Azure Backup Server v2 hello új funkciói. Ez a cikk ismerteti, hogyan tooupgrade a biztonsági mentés Server telepítését."
services: backup
documentationcenter: 
author: markgalioto
manager: carmonm
editor: 
ms.assetid: 
ms.service: backup
ms.workload: storage-backup-recovery
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/15/2017
ms.author: masaran;markgal
ms.openlocfilehash: b2a1ed27a6a682bd611fea1d2df9ef93314404e0
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="add-storage-tooazure-backup-server-v2"></a>Adja hozzá a tárolási tooAzure Backup Server v2

Az Azure Backup Server v2 tartalmaz a System Center 2016 adatok Protection Manager Modern Backup-tárhelyre. Modern Backup-tárhelyre 50 %-, a biztonsági mentések, amelyek háromszor gyorsabb és hatékonyabb tárolási a tárhely-megtakarítást nyújt. Munkaterhelés-kompatibilis tárterületet is biztosít. 

> [!NOTE]
> toouse Modern Backup-tárhelyre, futtatnia kell biztonsági mentést kiszolgáló v2 Windows Server 2016. Ha a Windows Server korábbi verzióján futtatja a biztonsági mentés kiszolgáló v2, Azure Backup Server tudják kihasználni a Modern Backup-tárhelyre. Ehelyett azt védi a munkaterheléseket mint a biztonsági mentés kiszolgáló v1. További információkért lásd: hello biztonsági mentés kiszolgálóverzió [védelmi mátrix](backup-mabs-protection-matrix.md).

## <a name="volumes-in-backup-server-v2"></a>A helykiszolgáló biztonsági mentése v2 kötetek

Tartalék kiszolgáló v2 tárolókötetek fogad el. Ha hozzáad egy kötetet, biztonsági mentés Server formázza hello kötet tooResilient File System (ReFS), amelyek Modern Backup-tárhelyre van szükség. egy köteten, tooadd és tooexpand később Ha kell, javasoljuk, hogy a munkafolyamat használja:

1.  A virtuális gép biztonsági mentése kiszolgáló v2 beállítása.
2.  Kötet létrehozása a virtuális lemez a tárolókészlethez:
    1.  Vegye fel a lemez tooa tárolókészletet, és hozzon létre egy virtuális lemezt egyszerű elrendezés.
    2.  Vegyen fel minden további lemezeket, és hello virtuális lemez.
    3.  Hozzon létre köteteket hello virtuális lemezen.
3.  Adja hozzá a hello kötetek tooBackup kiszolgáló.
4.  Munkaterhelés-t támogató tároló konfigurálásához.

## <a name="create-a-volume-for-modern-backup-storage"></a>Kötet létrehozása a Modern Backup-tárhelyre

Biztonsági mentés kiszolgáló v2 használatával kötetekkel, mint a lemezes tárolás segíthetnek ellenőrzése alatt tartja a tároló karbantartása. A kötet lehet egyetlen lemezre. Ha hello tooextend tárolás jövőbeli, azonban a tárolóhelyek használatával létrehozott lemezterülete kötet létrehozása. Ez segít, ha azt szeretné, tooexpand hello kötet biztonságimásolat-tároláshoz. Ez a rész nyújt gyakorlati tanácsok a kötet létrehozása ezzel a beállítással.

1. A Kiszolgálókezelő ablakában válassza **fájl- és tárolási szolgáltatások** > **kötetek** > **Tárolókészletek**. A **fizikai lemezek**, jelölje be **új tárolókészlet**. 

    ![Új tárolókészlet létrehozása](./media/backup-mabs-add-storage/mabs-add-storage-1.png)

2. A hello **feladatok** legördülő mezőben válassza **új virtuális lemez**.

    ![Virtuális lemez hozzáadása](./media/backup-mabs-add-storage/mabs-add-storage-2.png)

3. Hello tárolókészlet, majd válassza ki és **fizikai lemez hozzáadása**.

    ![Fizikai lemez hozzáadása](./media/backup-mabs-add-storage/mabs-add-storage-3.png)

4. Hello fizikai lemezt, majd válassza ki és **virtuális lemez bővítése**.

    ![Hello virtuális lemez kiterjesztése](./media/backup-mabs-add-storage/mabs-add-storage-4.png)

5. Hello virtuális lemezt, majd válassza ki és **új kötet**.

    ![Hozzon létre egy új kötetet](./media/backup-mabs-add-storage/mabs-add-storage-5.png)

6. A hello **hello kiszolgáló és lemez kiválasztása** párbeszédpanelen, a select hello kiszolgáló és a hello új lemez. Ezt követően válassza **következő**.

    ![Válassza ki a hello kiszolgáló és lemez](./media/backup-mabs-add-storage/mabs-add-storage-6.png)

## <a name="add-volumes-toobackup-server-disk-storage"></a>Adja hozzá a kötetek tooBackup Server lemezes tárolás

egy kötet tooBackup Server, a hello tooadd **felügyeleti** ablaktáblán ismételt vizsgálat hello tárolási, és válassza ki **Hozzáadás**. Megjelenik az összes hello kötetek elérhető toobe Server Backup-tárhelyre hozzá listája. Rendelkezésre álló köteteken ad hozzá a kijelölt kötetek toohello listáját, miután adhat a azokat egy rövid nevet toohelp mobileszközökként kezeli azokat. Ezek a kötetek tooReFS, biztonsági mentés Server használhassa hello előnyei Modern Backup-tárhelyre, válassza ki tooformat **OK**.

![Adja hozzá a rendelkezésre álló köteteken](./media/backup-mabs-add-storage/mabs-add-storage-7.png)

## <a name="set-up-workload-aware-storage"></a>Munkaterhelés-kompatibilis tárolás beállítása

Munkaterhelés-kompatibilis tárolóval kiválaszthatja a lehetőleg tároló bizonyos típusú munkaterhelések hello kötetekről. Például beállíthat költséges kötetek, amely támogatja a bemeneti/kimeneti műveletek második (IOPS) toostore csak hello igénylő munkaterhelések gyakori, nagy mennyiségű biztonsági másolatok száma túl magas száma. Példa: SQL Server, a tranzakciós naplók. Egyéb munkaterhelések, amelyekről a ritkábban, például a virtuális gépek, toolow költségű kötetek készíthető.

### <a name="update-dpmdiskstorage"></a>Frissítés-DPMDiskStorage

Hello PowerShell-parancsmaggal frissíti hello tárolókészletben a Data Protection Manager-kiszolgálón található kötet hello tulajdonságainak frissítése – DPMDiskStorage munkaterhelés-t támogató tárolási állíthat be.

Szintaxis:

`Parameter Set: Volume`

```
Update-DPMDiskStorage [-Volume] <Volume> [[-FriendlyName] <String> ] [[-DatasourceType] <VolumeTag[]> ] [-Confirm] [-WhatIf] [ <CommonParameters>]
```
hello alábbi képernyőfelvételen látható hello frissítés-DPMDiskStorage parancsmag hello PowerShell-ablakban.

![hello frissítés-DPMDiskStorage parancs hello PowerShell-ablakban](./media/backup-mabs-add-storage/mabs-add-storage-8.png)

hello PowerShell használatával is tükröződnek hello Backup Server felügyeleti konzol.

![A lemezek és kötetek a hello felügyeleti konzol](./media/backup-mabs-add-storage/mabs-add-storage-9.png)

## <a name="next-steps"></a>Következő lépések
Biztonsági kiszolgáló telepítése után megtudhatja, hogyan tooprepare kiszolgálóját, vagy indítsa el a védelmet a munkaterhelés.

- [A kiszolgálói biztonsági mentési feladatok előkészítése](backup-azure-microsoft-azure-backup.md)
- [Használja a VMware Server biztonsági másolat Server tooback](backup-azure-backup-server-vmware.md)
- [SQL Server biztonsági másolat Server tooback használata](backup-azure-sql-mabs.md)

