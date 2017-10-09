---
title: "aaaBackup és visszaállítási titkosított virtuális gépek Azure Backup segítségével"
description: "Ez a cikk beszél hello biztonsági mentési és visszaállítási élmény, a virtuális gépek az Azure Disk Encryption használatával titkosított."
services: backup
documentationcenter: 
author: JPallavi
manager: vijayts
editor: 
ms.assetid: 8387f186-7d7b-400a-8fc3-88a85403ea63
ms.service: backup
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: storage-backup-recovery
ms.date: 07/27/2017
ms.author: pajosh;markgal;trinadhk
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: c19ef6f58e3259949535dab32a55aaf7a8c658fd
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="how-tooback-up-and-restore-encrypted-virtual-machines-with-azure-backup"></a>Hogyan tooback mentése és visszaállítása titkosított virtuális gépek az Azure Backup szolgáltatással
Ez a cikk beszél lépéseket toobackup és a helyreállítási virtuális gépek Azure Backup segítségével. A hibák esetén támogatott forgatókönyveket, a szükséges előfeltételek és a hibaelhárítási lépésekkel kapcsolatos adatokat is tartalmazza.

## <a name="supported-scenarios"></a>Támogatott helyzetek
> [!NOTE]
> * Biztonsági mentés és visszaállítás titkosított virtuális gépek csak Resource Manager telepített virtuális gépek esetén támogatott. Klasszikus virtuális gépeknél nem támogatott. <br>
> * Az Azure Disk Encryption használatát, amely hello iparági szabványos BitLocker-szolgáltatás a Windows és Linux tooprovide titkosítási lemezek DM-crypt program segítségével a szolgáltatás használata Windows és Linux virtuális gépek esetén támogatott. <br>
> * Ez csak virtuális gépek titkosítja a BitLocker titkosítási kulcsot és a fő titkosítási kulcs is támogatott. A virtuális gépek titkosítása a BitLocker titkosítási kulcs használatával csak nem támogatott. <br>
>
>

## <a name="prerequisites"></a>Előfeltételek
1. Virtuális gép titkosítása a következő használatával [Azure Disk Encryption](../security/azure-security-disk-encryption.md). Azt kell titkosítja a BitLocker titkosítási kulcsot és a fő titkosítási kulcs is.
2. Recovery services-tároló létrehozását, és segítségével tárolóreplikálást lépések hello cikkben említett [előkészítse a környezetét a biztonsági mentés](backup-azure-arm-vms-prepare.md).
3. Azure biztonsági mentés megkapta-e [engedélyek tooaccess kulcstároló](#provide-permissions-to-azure-backup) tartalmazó kulcsokat, a titkos virtuális gépek titkosítva.

## <a name="backup-encrypted-vm"></a>Biztonsági mentés titkosított virtuális gép
Használja a következő lépéseket tooset biztonsági mentési cél hello, adja meg a házirend, elemek és a biztonsági mentés elindítása konfigurálása.

### <a name="configure-backup"></a>Biztonsági mentés konfigurálása
1. Ha már rendelkezik nyissa meg a Recovery Services-tároló, a folytatáshoz toonext lépés. Ha nem rendelkezik a Recovery Services-tároló nyílt, de a hello Azure-portálon, hello központ menüben kattintson a **Tallózás**.

   * Írja be az erőforrások listájához hello, **Recovery Services**.
   * Írja be megkezdése előtt, hello szűrők megjelenítése a megadott feltételeknek. Amikor meglátja a **Recovery Services-tárolót**, kattintson rá.

      ![Recovery Services-tároló létrehozása – 1. lépés](./media/backup-azure-vms-encryption/browse-to-rs-vaults.png) <br/>

     Recovery Services-tárolók hello listája jelenik meg. Recovery Services-tárolók hello listában jelölje ki a tárolóban.

     Megnyitja a kijelölt tároló irányítópult hello.
2. A hello elemekből álló listák tárolóban megjelenik, kattintson az **biztonsági mentés** tooopen hello biztonsági mentés panelen.

      ![Biztonsági mentés panel megnyitása](./media/backup-azure-vms-encryption/select-backup.png)
3. Hello biztonsági mentés paneljén kattintson **biztonsági mentési cél** tooopen hello biztonsági mentési cél panelen.

      ![Forgatókönyv panel megnyitása](./media/backup-azure-vms-encryption/select-backup-goal-one.png)
4. Hello biztonsági mentési cél paneljén állítsa be **a számítási feladatok futtató** tooAzure és **miről szeretne toobackup** tooVirtual gépre, majd kattintson **OK**.

   hello biztonsági mentési cél panel bezárása, és a hello biztonsági szabályzat panel nyílik meg.

   ![Forgatókönyv panel megnyitása](./media/backup-azure-vms-encryption/select-backup-goal-two.png)
5. Hello biztonsági mentési házirend panelen, jelölje ki a biztonsági mentési házirend hello tooapply toohello tároló ki, majd kattintson **OK**.

      ![Biztonsági mentési házirend kiválasztása](./media/backup-azure-vms-encryption/setting-rs-backup-policy-new.png)

    hello alapértelmezett házirend hello adatait hello részletei láthatók. Ha azt szeretné, hogy toocreate egy házirendet, válassza ki a **hozzon létre új** hello legördülő menüből. Miután rákattintott **OK**, biztonsági mentési házirend hello hello tároló társítva.

    Ezután válassza ki a hello virtuális gépek tooassociate hello tárolóban.
6. Válassza ki a hello titkosított tooassociate hello a megadott házirend, és kattintson a virtuális gépek **OK**.

      ![Válassza ki a titkosított virtuális gépek](./media/backup-azure-vms-encryption/selected-encrypted-vms.png)
7. Ezen a lapon látható társított toohello titkosított virtuális gépek kiválasztott kulcstároló üzenete. A biztonsági mentési szolgáltatás csak olvasási hozzáféréssel toohello kulcsok és titkos hello a key vaultban igényel. Ezen engedélyek toobackup kulcsot használ, és a titkos kulcsot, valamint hello kapcsolódó virtuális gépek. **Biztonsági mentések toowork érdekében meg kell adni engedélyek toobackup szolgáltatás tooaccess kulcstároló**. Megadhatja azokat az engedélyeket használ [hello az alábbi részben leírt lépéseket](#provide-permissions-to-azure-backup).

      ![Titkosított virtuális gépek üzenet](./media/backup-azure-vms-encryption/encrypted-vm-warning-message.png)

      Most, hogy a megadott összes beállítás hello tároló hello biztonsági mentés panelen kattintson a biztonsági mentés engedélyezése alján hello hello. Engedélyezése biztonsági mentés telepíti hello házirend toohello tároló és a hello virtuális gépeket.
8. hello előkészítése során a következő fázis telepíti a hello Virtuálisgép-ügynök, vagy így meg arról, hogy hello Virtuálisgép-ügynök telepítve van. toodo hello azonos, hello cikkben említett hello lépésekkel [előkészítse a környezetét a biztonsági mentés](backup-azure-arm-vms-prepare.md).

### <a name="triggering-backup-job"></a>Biztonsági mentési feladat időt.
Hello cikkben említett hello lépésekkel [biztonsági mentési Azure virtuális gépek toorecovery szolgáltatások tároló](backup-azure-arm-vms.md) tootrigger biztonsági mentési feladat.

### <a name="continue-backups-of-already-backed-up-vms-with-encryption-enabled"></a>A biztonsági másolatok továbbra is már biztonsági mentése a virtuális gépek engedélyezhető a titkosítás  
Ha már folyamatban a biztonsági mentést a recovery services-tároló virtuális gépeket, és később titkosítási engedélyezték, biztonsági mentések toocontinue érdekében meg kell adni engedélyek toobackup szolgáltatás tooaccess kulcstároló. Megadhatja azokat az engedélyeket használ [hello szakaszában az alábbi lépések](#provide-permissions-to-azure-backup) vagy a PowerShell használatával lépéseket említett **biztonsági mentés engedélyezése** szakasza [PowerShell dokumentációs](backup-azure-vms-automation.md). 

## <a name="provide-permissions-tooazure-backup"></a>Adja meg a biztonsági engedélyek tooAzure
A következő lépéseket tooprovide megfelelő engedélyeket tooAzure biztonsági mentés tooaccess kulcstároló hello használja, és titkosított virtuális gépek biztonsági másolatot készíteni:
1. Válassza ki **több szolgáltatások** keresse meg a **tárolók kulcs**.

    ![Keresési kulcstároló](./media/backup-azure-vms-encryption/search-key-vault.png)
    
2. A kulcstárolók hello listáról válassza ki a titkosított virtuális gép, amely szükséges a biztonsági mentés toobe társított hello kulcstároló.

     ![Válassza ki a kulcstartót](./media/backup-azure-vms-encryption/select-key-vault.png)
     
3. Kattintson a **hozzáférési házirendek** , majd **új hozzáadása**.

    ![Hozzáférési házirend hozzáadása](./media/backup-azure-vms-encryption/select-key-vault-access-policy.png)
    
4. Kattintson a **válassza egyszerű** és típus **biztonsági másolat szolgáltatás** hello keresősávban. 

    ![Keresési biztonsági mentési szolgáltatás](./media/backup-azure-vms-encryption/search-backup-service.png)
    
5. Válassza ki **biztonsági másolat szolgáltatás** , és kattintson a Kijelölés gomb.

    ![Válassza ki a biztonsági mentési szolgáltatás](./media/backup-azure-vms-encryption/select-backup-service.png)
    
6. Válassza ki **Azure biztonsági mentés** sablonból konfigurálása a legördülő listán. Előre tölti ki a kulcs engedélyei hello szükséges engedélyekkel, és a titkos engedélyek legördülő listán. 

    ![Válassza ki az Azure biztonsági mentés](./media/backup-azure-vms-encryption/select-backup-template.png)
    
7. Kattintson az **OK** gombra. Figyelje meg, hogy a biztonsági másolat szolgáltatás a hozzáférési házirend panel bekerül. 

    ![A biztonsági mentési szolgáltatás-hozzáférési házirend](./media/backup-azure-vms-encryption/backup-service-access-policy.png)
    
8. Kattintson a **Save** (Mentés) gombra. Ez biztosítja a szükséges hello engedélyek tooAzure biztonsági mentés.

    ![A biztonsági mentési szolgáltatás-hozzáférési házirend](./media/backup-azure-vms-encryption/save-access-policy.png)

Miután sikeresen biztosított engedélyeket, folytathassa titkosított virtuális gépek biztonsági mentés engedélyezése.

## <a name="restore-encrypted-vm"></a>Állítsa helyre a titkosított virtuális Gépet
toorestore titkosított virtuális gép, az említett szakasz visszaállítása lemezek használatával lépések **visszaállítási biztonsági másolatba mentett lemezek** a [kiválasztása a virtuális gép visszaállítási konfigurációs](backup-azure-arm-restore-vms.md#choosing-a-vm-restore-configuration). Ezt követően hello alábbi beállítások egyikét használhatja:
* Hello PowerShell lépésekkel említett [hozzon létre egy virtuális Gépet visszaállított lemezekből](backup-azure-vms-automation.md#create-a-vm-from-restored-disks) toocreate VM teljes visszaállított lemezekről.
* VAGY [visszaállítása lemezek részeként létrehozott sablon használata](backup-azure-arm-restore-vms.md#use-templates-to-customize-restore-vm) toocreate virtuális gépek visszaállított lemezekről. Sablonok csak 2017. április 26. után létrehozott helyreállítási pontok használható.

## <a name="troubleshooting-errors"></a>Kapcsolatos hibák elhárítása
| Művelet | Hiba legutolsó részletes adatai | Megoldás: |
| --- | --- | --- |
| Biztonsági mentés |Érvényesítése sikertelen volt, mert a virtuális gépet önmagában BEK van titkosítva. Biztonsági másolatok csak virtuális gépek BEK és KEK is titkosítani engedélyezhető. |Virtuális gép titkosítani kell BEK és KEK használatával. Először hello VM visszafejtése, és azt BEK és KEK is titkosítani. Engedélyezze a biztonsági mentés után a virtuális gép titkosítása BEK és a KEK. További tudnivalókért, hogyan zajlik a [hello virtuális gép titkosítására és visszafejtésére](../security/azure-security-disk-encryption.md)  |
| Visszaállítás |Titkosított virtuális Gépet nem lehet visszaállítani, mert a virtuális gép társított kulcstároló nem létezik. |Hozzon létre a kulcstartót használó [Ismerkedés az Azure Key Vault](../key-vault/key-vault-get-started.md). Tekintse meg a hello cikk [kulcstároló kulcs és a titkos kulcs Azure Backup segítségével visszaállítása](backup-azure-restore-key-secret.md) toorestore kulcsot és titkos kulcsot, ha azok nincsenek megadva. |
| Visszaállítás |Titkosított virtuális Gépet nem lehet visszaállítani, mert a kulcs és a virtuális gép társított titkos kulcs nem létezik. |Tekintse meg a hello cikk [kulcstároló kulcs és a titkos kulcs Azure Backup segítségével visszaállítása](backup-azure-restore-key-secret.md) toorestore kulcsot és titkos kulcsot, ha azok nincsenek megadva. |
| Visszaállítás |A biztonsági mentési szolgáltatás az előfizetéshez nincs engedélyezési tooaccess erőforrása. |Fent említett, állítsa vissza lemezek először részben leírt lépéseket követve **visszaállítási biztonsági másolatba mentett lemezek** a [kiválasztása a virtuális gép visszaállítási konfigurációs](backup-azure-arm-restore-vms.md#choosing-a-vm-restore-configuration). Ez, a felhasználó PowerShell után túl[hozzon létre egy virtuális Gépet visszaállított lemezekből](backup-azure-vms-automation.md#create-a-vm-from-restored-disks). |
|Biztonsági mentés | Az Azure Backup szolgáltatás nem rendelkezik elegendő engedélyekkel tooKey tároló a biztonsági mentés a titkosított virtuális gépek | Virtuális gép titkosítani kell a BitLocker titkosítási kulcs, mind a kiszolgáló fő titkosítási kulcs használatával. Ezt követően engedélyezni kell a biztonsági mentés.  A biztonsági mentési szolgáltatás azokat az engedélyeket használatával kell megadni [hello szakaszban leírt lépéseket](#provide-permissions-to-azure-backup) vagy hello említett PowerShell lépések segítségével **engedélyezni a védelmet** hello PowerShell szakasza címen [használata AzureRM.RecoveryServices.Backup parancsmagok tooback virtuális gépek](backup-azure-vms-automation.md#back-up-azure-vms). |  
