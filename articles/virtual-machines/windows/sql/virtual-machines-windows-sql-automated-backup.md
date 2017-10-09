---
title: "az SQL Server 2014 Azure virtuális gépek biztonsági mentése aaaAutomated |} Microsoft Docs"
description: "Ismerteti a hello automatikus biztonsági mentés szolgáltatás az SQL Server 2014 rendszert futtató virtuális gépek Azure-ban. Ez a cikk az adott tooVMs hello erőforrás-kezelő használatával."
services: virtual-machines-windows
documentationcenter: na
author: rothja
manager: jhubbard
editor: 
tags: azure-resource-manager
ms.assetid: bdc63fd1-db49-4e76-87d5-b5c6a890e53c
ms.service: virtual-machines-sql
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-windows-sql-server
ms.workload: iaas-sql-server
ms.date: 07/05/2017
ms.author: jroth
ms.openlocfilehash: c6803d8ef9f80e44a2f87918d87e099f1b562483
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="automated-backup-for-sql-server-2014-virtual-machines-resource-manager"></a>Automatikus biztonsági mentés SQL Server 2014 virtuális gépek (erőforrás-kezelő)

> [!div class="op_single_selector"]
> * [SQL Server 2014](virtual-machines-windows-sql-automated-backup.md)
> * [SQL Server 2016](virtual-machines-windows-sql-automated-backup-v2.md)

Automatikus biztonsági mentés automatikusan konfigurálja a [való felügyelt biztonsági mentésének tooMicrosoft Azure](https://msdn.microsoft.com/library/dn449496.aspx) az összes meglévő és új adatbázis egy SQL Server 2014 Standard vagy Enterprise rendszert futtató Azure virtuális gépen. Ez lehetővé teszi tooconfigure rendszeres biztonsági tartós Azure blob-tárolók. Automatikus biztonsági mentés függ hello [SQL Server infrastruktúra-szolgáltatási ügynök bővítmény](virtual-machines-windows-sql-server-agent-extension.md).

[!INCLUDE [learn-about-deployment-models](../../../../includes/learn-about-deployment-models-rm-include.md)]

## <a name="prerequisites"></a>Előfeltételek
toouse automatikus biztonsági mentés, vegye figyelembe a következő előfeltételek hello:

**Operációs rendszer**:

- Windows Server 2012
- Windows Server 2012 R2
- Windows Server 2016

**SQL Server verziójához/kiadásához**:

- Az SQL Server 2014 Standard
- SQL Server 2014 Enterprise

> [!IMPORTANT]
> Automatikus biztonsági mentés együttműködik az SQL Server 2014. Ha az SQL Server 2016 használ, a v2 tooback automatikus biztonsági mentés másolatot az adatbázisokról is használhatja. További információkért lásd: [az SQL Server 2016 Azure virtuális gépek automatikus biztonsági mentés v2](virtual-machines-windows-sql-automated-backup-v2.md).

**Adatbázis-konfiguráció**:

- Cél adatbázisok hello teljes helyreállítási modell kell megadni. További információ hello hatás hello teljes helyreállítási modell biztonsági mentések, lásd: [biztonsági mentés alatt hello teljes helyreállítási modell](https://technet.microsoft.com/library/ms190217.aspx).
- Céladatbázisokhoz hello alapértelmezett SQL Server-példányon kell lennie. hello SQL Server IaaS-bővítmény nem támogatja az elnevezett példányok.

**Az Azure-alapú üzemi modell**:

- Resource Manager

**Az Azure PowerShell**:

- [Telepítse a legújabb Azure PowerShell-parancsok hello](/powershell/azure/overview) Ha azt tervezi, hogy tooconfigure automatikus biztonsági mentés a PowerShell használatával.

> [!NOTE]
> Automatikus biztonsági mentés SQL Server infrastruktúra-szolgáltatási ügynök bővítmény hello támaszkodik. A gyűjtemény lemezképei aktuális SQL virtuális gép alapértelmezés szerint adja hozzá ezt a bővítményt. További információkért lásd: [SQL Server infrastruktúra-szolgáltatási ügynök bővítmény](virtual-machines-windows-sql-server-agent-extension.md).

## <a name="settings"></a>Beállítások

hello következő táblázat ismerteti, amelyek az automatikus biztonsági mentés hello-beállítások. hello tényleges konfigurációs lépései eltérőek attól függően, hogy használ-e hello Azure-portálon vagy az Azure PowerShell-parancsokkal.

| Beállítás | Tartomány (alapértelmezett) | Leírás |
| --- | --- | --- |
| **Automatikus biztonsági mentés** | Engedélyezi/letiltja (letiltva) | Engedélyezi vagy letiltja az automatikus biztonsági mentés az SQL Server 2014 Standard vagy Enterprise rendszert futtató Azure virtuális gép esetében. |
| **Megőrzési időtartam** | 1-30 nap (30 nap) | nap tooretain biztonsági hello száma. |
| **Tárfiók** | Azure Storage-fiók | Egy Azure storage fiók toouse automatikus biztonsági mentés fájlok tárolásához a blob Storage tárolóban. Egy tároló összes biztonságimásolat-fájl jön létre a hely toostore. hello biztonságimásolat-fájl elnevezési konvenciója hello dátum, idő és a gép neve tartalmazza. |
| **Titkosítás** | Engedélyezi/letiltja (letiltva) | Engedélyezi vagy letiltja a titkosítást. Ha engedélyezve van a titkosítás, hello használt tanúsítványok toorestore hello biztonsági mentés megadott hello találhatók-e tárolási fiókként hello azonos `automaticbackup` tárolót használó hello azonos elnevezési konvenciót. Hello jelszó is módosul, ha új tanúsítványt hoz létre, hogy a jelszó, de a régi tanúsítvány hello marad toorestore korábbi biztonsági másolatok. |
| **Jelszó** | Jelszó szöveg | A titkosítási kulcsok jelszava. Erre csak akkor van szükség, ha engedélyezve van-e a titkosítás. A sorrend toorestore egy biztonsági másolat titkosításának engedélyezése, rendelkeznie kell hello helyes jelszót és a kapcsolódó hello a biztonsági mentés készült hello során használt tanúsítvány. |

## <a name="configuration-in-hello-portal"></a>A portál hello konfiguráció

Hello Azure portál tooconfigure automatikus biztonsági mentés is használhatja, vagy meglévő SQL Server 2014 virtuális gépek kiépítése során.

### <a name="new-vms"></a>Új virtuális gépek

Amikor létrehoz egy új SQL Server 2014-virtuális gép hello Resource Manager üzembe helyezési modellel, használja a hello Azure portál tooconfigure automatikus biztonsági mentés.

A hello **SQL Server-beállítások** panelen válassza **automatikus biztonsági mentés**. hello Azure portál következő képernyőfelvételen látható hello **SQL automatikus biztonsági mentés** panelen.

![SQL automatikus biztonsági mentés konfigurálása az Azure portálon](./media/virtual-machines-windows-sql-automated-backup/azure-sql-arm-autobackup.png)

A környezetben, témakörében hello teljes [az Azure SQL Server virtuális gépek kiépítése](virtual-machines-windows-portal-sql-server-provision.md).

### <a name="existing-vms"></a>Meglévő virtuális gépek

Meglévő SQL Server virtuális gépek válassza ki az SQL Server virtuális gépet. Válassza ki hello **SQL Server-konfigurációs** hello szakasza **beállítások** panelen.

![SQL automatikus biztonsági mentés a meglévő virtuális gépek](./media/virtual-machines-windows-sql-automated-backup/azure-sql-rm-autobackup-existing-vms.png)

A hello **SQL Server-konfigurációs** panelen kattintson hello **szerkesztése** hello gombjára automatikus biztonsági mentési szakasz.

![Meglévő virtuális gépek SQL automatikus biztonsági mentés konfigurálása](./media/virtual-machines-windows-sql-automated-backup/azure-sql-rm-autobackup-configuration.png)

Ha elkészült, kattintson a hello **OK** hello alsó részén hello gombjára **SQL Server-konfigurációs** panel toosave a módosításokat.

Ha engedélyezi az automatikus biztonsági mentés a hello először, Azure SQL Server IaaS-ügynök hello hello háttérben konfigurálja. Ebben az időszakban hello Azure-portálon előfordulhat, hogy jelenjen meg, hogy az automatikus biztonsági mentés konfigurálva van-e. Várjon egy pár percet hello ügynök toobe telepítve, a konfigurált. Miután adott hello Azure portal hello új beállítások fogja tartalmazni.

> [!NOTE]
> Automatikus biztonsági mentés sablon használatával is konfigurálhatja. További információkért lásd: [Azure gyors üzembe helyezés sablon automatikus biztonsági mentés](https://github.com/Azure/azure-quickstart-templates/tree/master/101-vm-sql-existing-autobackup-update).

## <a name="configuration-with-powershell"></a>PowerShell-konfiguráció

Használhatja a PowerShell tooconfigure automatikus biztonsági mentés. Mielőtt elkezdené, a következőket kell tennie:

- [Töltse le és telepítse a legújabb Azure PowerShell hello](http://aka.ms/webpi-azps).
- Nyissa meg a Windows Powershellt, és rendelje hozzá azt a fiókot. Ehhez a hello hello lépéseket követve [az előfizetés konfigurálása](https://docs.microsoft.com/azure/virtual-machines/windows/sql/virtual-machines-windows-ps-sql-create#configure-your-subscription) témakör kiépítés hello szakasza.

### <a name="install-hello-sql-iaas-extension"></a>Hello SQL IaaS-bővítmény telepítése
Ha kiépített SQL Server virtuális gépek Azure-portálon hello létrehozását, hello SQL Server IaaS-bővítmény már telepítve kell lennie. Segítségével meghatározhatja, hogy ha az telepítve van a virtuális gép meghívásával **Get-AzureRmVM** parancs és megvizsgálta az hello **bővítmények** tulajdonság.

```powershell
$vmname = "vmname"
$resourcegroupname = "resourcegroupname"

(Get-AzureRmVM -Name $vmname -ResourceGroupName $resourcegroupname).Extensions
```

Ha hello bővítmény SQL Server IaaS-ügynök telepítve van, megjelenik az "SqlIaaSAgent" vagy "SQLIaaSExtension" jelenik. **ProvisioningState** a hello bővítmény is jelenítsen meg a "Sikeres".

Ha nincs telepítve, vagy toobe kiépítése sikertelen volt, a következő parancs hello telepítheti. Továbbá toohello virtuális gép nevét és az erőforrás csoportjában is meg kell hello régió (**$region**), amely a virtuális gép található.

```powershell
$region = “EASTUS2”
Set-AzureRmVMSqlServerExtension -VMName $vmname `
    -ResourceGroupName $resourcegroupname -Name "SQLIaasExtension" `
    -Version "1.2" -Location $region
```

### <a id="verifysettings"></a>Aktuális beállításainak ellenőrzése

Ha engedélyezte az automatikus biztonsági mentés kiépítése során, használhatja a jelenlegi konfiguráció PowerShell toocheck. Futtassa a hello **Get-AzureRmVMSqlServerExtension** parancsot, és vizsgálja meg a hello **AutoBackupSettings** tulajdonság:

```powershell
(Get-AzureRmVMSqlServerExtension -VMName $vmname -ResourceGroupName $resourcegroupname).AutoBackupSettings
```

Kimeneti hasonló toohello következő szerezheti be:

```
Enable                      : False
EnableEncryption            : False
RetentionPeriod             : -1
StorageUrl                  : NOTSET
StorageAccessKey            : 
Password                    : 
BackupSystemDbs             : False
BackupScheduleType          : 
FullBackupFrequency         : 
FullBackupStartTime         : 
FullBackupWindowHours       : 
LogBackupFrequency          : 
```

Ha a kimeneti azt mutatja, hogy **engedélyezése** értéke túl**hamis**, akkor el kell tooenable automatikus biztonsági mentés. hello jó hírünk engedélyezése és konfigurálása az automatikus biztonsági mentés hello a megszokott módon. Hello ezt az információt a következő szakaszban talál.

> [!NOTE] 
> Hello beállítások módosítása után azonnal ellenőrzését, akkor lehetséges, hogy kap vissza hello régi konfigurációs értékeket. Várjon néhány percet, és ellenőrizze a hello beállításait újra toomake meg arról, hogy a módosítások alkalmazása megtörtént.

### <a name="configure-automated-backup"></a>Automatikus biztonsági mentés konfigurálása
Használható PowerShell tooenable automatikus biztonsági mentés, valamint a toomodify a konfiguráció és működés bármikor.

Először válasszon, vagy hozzon létre egy tárfiókot a hello biztonságimásolat-fájlokat. hello alábbi parancsfájl a storage-fiók kiválasztása vagy hoz létre, ha nem létezik.

```powershell
$storage_accountname = “yourstorageaccount”
$storage_resourcegroupname = $resourcegroupname

$storage = Get-AzureRmStorageAccount -ResourceGroupName $resourcegroupname `
    -Name $storage_accountname -ErrorAction SilentlyContinue
If (-Not $storage)
    { $storage = New-AzureRmStorageAccount -ResourceGroupName $storage_resourcegroupname `
    -Name $storage_accountname -SkuName Standard_GRS -Location $region }
```

> [!NOTE]
> Automatikus biztonsági mentés nem támogatja a prémium szintű storage tárolni a biztonsági mentések, de a prémium szintű Storage használó virtuális gépek lemezei készített biztonsági másolat is igénybe vehet.

Ezután használja az hello **New-AzureRmVMSqlServerAutoBackupConfig** tooenable parancsot, és hello automatikus biztonsági mentés beállításait toostore biztonsági mentések konfigurálása a hello Azure storage-fiók. Ebben a példában a hello biztonsági mentések toobe 10 napig őrzi meg vannak beállítva. a parancs második hello **Set-AzureRmVMSqlServerExtension**, frissítések hello Azure virtuális Géphez megadott ezekkel a beállításokkal.

```powershell
$autobackupconfig = New-AzureRmVMSqlServerAutoBackupConfig -Enable `
    -RetentionPeriodInDays 10 -StorageContext $storage.Context `
    -ResourceGroupName $storage_resourcegroupname

Set-AzureRmVMSqlServerExtension -AutoBackupSettings $autobackupconfig `
    -VMName $vmname -ResourceGroupName $resourcegroupname
```

Nem sikerült eltarthat néhány percig tooinstall és hello SQL Server IaaS-ügynök konfigurálása.

> [!NOTE]
> Nincsenek más beállítások **New-AzureRmVMSqlServerAutoBackupConfig** , amelyek csak a tooSQL Server 2016-os és az automatikus biztonsági mentés v2 érvényesek. Az SQL Server 2014 nem támogatja a következő beállítások hello: **BackupSystemDbs**, **BackupScheduleType**, **FullBackupFrequency**,  **FullBackupStartHour**, **FullBackupWindowInHours**, és **LogBackupFrequencyInMinutes**. Ha tooconfigure ezeket a beállításokat az SQL Server 2014 virtuális gépen, nincs hiba, de hello-beállítások nem érvényben. Ha toouse ezeket a beállításokat SQL Server 2016-os virtuális gépen, lásd: [az SQL Server 2016 Azure virtuális gépek automatikus biztonsági mentés v2](virtual-machines-windows-sql-automated-backup-v2.md).

tooenable titkosítási hello előző parancsfájl toopass hello módosítása **EnableEncryption** paraméter és egy jelszót (biztonságos karakterlánc) hello **CertificatePassword** paraméter. hello következő parancsfájl lehetővé teszi, hogy az előző példában hello hello automatikus biztonsági mentés beállításait, és hozzáadja titkosítás.

```powershell
$password = "P@ssw0rd"
$encryptionpassword = $password | ConvertTo-SecureString -AsPlainText -Force

$autobackupconfig = New-AzureRmVMSqlServerAutoBackupConfig -Enable `
    -EnableEncryption -CertificatePassword $encryptionpassword `
    -RetentionPeriodInDays 10 -StorageContext $storage.Context `
    -ResourceGroupName $storage_resourcegroupname

Set-AzureRmVMSqlServerExtension -AutoBackupSettings $autobackupconfig `
    -VMName $vmname -ResourceGroupName $resourcegroupname
```

a beállítások érvényesek, tooconfirm [hello automatikus biztonsági mentés konfigurációjának ellenőrzése](#verifysettings).

### <a name="disable-automated-backup"></a>Automatikus biztonsági mentés tiltása

Automatikus biztonsági mentés, azonos hello nélkül parancsfájl futtatási hello toodisable **-engedélyezése** paraméter toohello **New-AzureRmVMSqlServerAutoBackupConfig** parancsot. hello hiányában hello **-engedélyezése** paraméter jelek hello parancs toodisable hello szolgáltatást. Csakúgy, mint a telepítés több percet toodisable automatikus biztonsági mentés is igénybe vehet.

```powershell
$autobackupconfig = New-AzureRmVMSqlServerAutoBackupConfig -ResourceGroupName $storage_resourcegroupname

Set-AzureRmVMSqlServerExtension -AutoBackupSettings $autobackupconfig `
    -VMName $vmname -ResourceGroupName $resourcegroupname
```

### <a name="example-script"></a>A példaként megadott parancsfájlt

hello következő parancsfájl biztosít a változók, hogy testre szabhatja a tooenable, és konfigurálja az automatikus biztonsági mentés a virtuális gép számára. Az Ön esetében szükség lehet a követelmények alapján toocustomize hello parancsfájl. Például akkor toomake változásai Ha toodisable hello biztonsági mentés, a rendszer-adatbázisokat, vagy engedélyezheti a titkosítást.

```powershell
$vmname = "yourvmname"
$resourcegroupname = "vmresourcegroupname"
$region = “Azure region name such as EASTUS2”
$storage_accountname = “storageaccountname”
$storage_resourcegroupname = $resourcegroupname
$retentionperiod = 10

# ResourceGroupName is hello resource group which is hosting hello VM where you are deploying hello SQL IaaS Extension

Set-AzureRmVMSqlServerExtension -VMName $vmname `
    -ResourceGroupName $resourcegroupname -Name "SQLIaasExtension" `
    -Version "1.2" -Location $region

# Creates/use a storage account toostore hello backups

$storage = Get-AzureRmStorageAccount -ResourceGroupName $resourcegroupname `
    -Name $storage_accountname -ErrorAction SilentlyContinue
If (-Not $storage)
    { $storage = New-AzureRmStorageAccount -ResourceGroupName $storage_resourcegroupname `
    -Name $storage_accountname -SkuName Standard_GRS -Location $region }

# Configure Automated Backup settings

$autobackupconfig = New-AzureRmVMSqlServerAutoBackupConfig -Enable `
    -RetentionPeriodInDays $retentionperiod -StorageContext $storage.Context `
    -ResourceGroupName $storage_resourcegroupname

# Apply hello Automated Backup settings toohello VM

Set-AzureRmVMSqlServerExtension -AutoBackupSettings $autobackupconfig `
    -VMName $vmname -ResourceGroupName $resourcegroupname
```

## <a name="next-steps"></a>Következő lépések

Automatikus biztonsági mentés való felügyelt biztonsági mentésének konfigurálása Azure virtuális gépeken. Ezért fontos túl[tekintse meg a felügyelt biztonsági mentéshez hello dokumentációt](https://msdn.microsoft.com/library/dn449496.aspx) toounderstand hello viselkedést és megvalósítását.

További biztonsági mentés található, és állítsa vissza a következő témakör hello útmutatást az SQL Server Azure virtuális gépeken: [biztonsági mentése és visszaállítása az SQL Server Azure virtuális gépek](virtual-machines-windows-sql-backup-recovery.md).

Más elérhető automation feladatokkal kapcsolatos további információkért lásd: [SQL Server infrastruktúra-szolgáltatási ügynök bővítmény](virtual-machines-windows-sql-server-agent-extension.md).

További információ az Azure virtuális gépeken futó SQL Server: [SQL Server Azure virtuális gépek – áttekintés](virtual-machines-windows-sql-server-iaas-overview.md).

