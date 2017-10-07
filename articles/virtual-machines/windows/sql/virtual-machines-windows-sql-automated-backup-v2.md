---
title: "az SQL Server 2016 Azure virtuális gépek biztonsági mentése v2 aaaAutomated |} Microsoft Docs"
description: "Ismerteti a hello automatikus biztonsági mentés szolgáltatás az SQL Server 2016 rendszert futtató virtuális gépek Azure-ban. Ez a cikk az adott tooVMs hello erőforrás-kezelő használatával."
services: virtual-machines-windows
documentationcenter: na
author: rothja
manager: jhubbard
editor: 
tags: azure-resource-manager
ms.assetid: ebd23868-821c-475b-b867-06d4a2e310c7
ms.service: virtual-machines-sql
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-windows-sql-server
ms.workload: iaas-sql-server
ms.date: 04/05/2017
ms.author: jroth
ms.openlocfilehash: a322792fb22c76bfa74fafb711b8b1927a6e2b3a
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="automated-backup-v2-for-sql-server-2016-azure-virtual-machines-resource-manager"></a>Automatikus biztonsági mentési v2 az SQL Server 2016 az Azure virtuális gépek (erőforrás-kezelő)

> [!div class="op_single_selector"]
> * [SQL Server 2014](virtual-machines-windows-sql-automated-backup.md)
> * [SQL Server 2016](virtual-machines-windows-sql-automated-backup-v2.md)

Az automatikus biztonsági mentés v2 automatikusan konfigurálja a [való felügyelt biztonsági mentésének tooMicrosoft Azure](https://msdn.microsoft.com/library/dn449496.aspx) az összes meglévő és új adatbázis egy Azure virtuális gépen futó SQL Server 2016 Standard, Enterprise vagy fejlesztői kiadását. Ez lehetővé teszi tooconfigure rendszeres biztonsági tartós Azure blob-tárolók. Az automatikus biztonsági mentés v2 függ hello [SQL Server infrastruktúra-szolgáltatási ügynök bővítmény](virtual-machines-windows-sql-server-agent-extension.md).

[!INCLUDE [learn-about-deployment-models](../../../../includes/learn-about-deployment-models-rm-include.md)]

## <a name="prerequisites"></a>Előfeltételek
Automatikus biztonsági mentés 2-es toouse tekintse át a következő előfeltételek hello:

**Operációs rendszer**:

- Windows Server 2012 R2
- Windows Server 2016

**SQL Server verziójához/kiadásához**:

- SQL Server 2016 Standard
- SQL Server 2016 Enterprise
- SQL Server 2016 Developer

> [!IMPORTANT]
> Az automatikus biztonsági mentés v2 SQL Server 2016 működik. Ha az SQL Server 2014-et használ, a automatikus biztonsági mentés v1 tooback másolatot az adatbázisokról is használhatja. További információkért lásd: [automatikus biztonsági mentés az SQL Server 2014 Azure virtuális gépek](virtual-machines-windows-sql-automated-backup.md).

**Adatbázis-konfiguráció**:

- Cél adatbázisok hello teljes helyreállítási modell kell megadni. További információ hello hatás hello teljes helyreállítási modell biztonsági mentések, lásd: [biztonsági mentés alatt hello teljes helyreállítási modell](https://technet.microsoft.com/library/ms190217.aspx).
- Rendszer-adatbázisokat nem rendelkeznek toouse teljes helyreállítási modell. Ha a napló biztonsági mentések toobe igénybe vett minta vagy MSDB van szüksége, a teljes helyreállítási modell kell használnia.
- Céladatbázisokhoz hello alapértelmezett SQL Server-példányon kell lennie. hello SQL Server IaaS-bővítmény nem támogatja az elnevezett példányok.

**Az Azure-alapú üzemi modell**:

- Resource Manager

> [!NOTE]
> Automatikus biztonsági mentés támaszkodik hello **SQL Server infrastruktúra-szolgáltatási ügynök bővítmény**. A gyűjtemény lemezképei aktuális SQL virtuális gép alapértelmezés szerint adja hozzá ezt a bővítményt. További információkért lásd: [SQL Server infrastruktúra-szolgáltatási ügynök bővítmény](virtual-machines-windows-sql-server-agent-extension.md).

## <a name="settings"></a>Beállítások
hello következő táblázat ismerteti, amelyek az automatikus biztonsági mentés v2 hello-beállítások. hello tényleges konfigurációs lépései eltérőek attól függően, hogy használ-e hello Azure-portálon vagy az Azure PowerShell-parancsokkal.

### <a name="basic-settings"></a>Alapbeállítások

| Beállítás | Tartomány (alapértelmezett) | Leírás |
| --- | --- | --- |
| **Automatikus biztonsági mentés** | Engedélyezi/letiltja (letiltva) | Engedélyezi vagy letiltja az automatikus biztonsági mentés az SQL Server 2016 Standard vagy Enterprise rendszert futtató Azure virtuális gép esetében. |
| **Megőrzési időtartam** | 1-30 nap (30 nap) | hello az nap tooretain biztonsági mentéseinek számát. |
| **Tárfiók** | Azure Storage-fiók | Egy Azure storage fiók toouse automatikus biztonsági mentés fájlok tárolásához a blob Storage tárolóban. Egy tároló összes biztonságimásolat-fájl jön létre a hely toostore. hello biztonságimásolat-fájl elnevezési konvenciója hello dátum, idő és adatbázis GUID-ja tartalmaz. |
| **Titkosítás** |Engedélyezi/letiltja (letiltva) | Engedélyezi vagy letiltja a titkosítást. Ha engedélyezve van a titkosítás, hello használt tanúsítványok toorestore hello biztonsági mentés megadott hello találhatók-e tárolási fiókként hello azonos **automaticbackup** tárolót használó hello azonos elnevezési konvenciót. Hello jelszó is módosul, ha új tanúsítványt hoz létre, hogy a jelszó, de a régi tanúsítvány hello marad toorestore korábbi biztonsági másolatok. |
| **Jelszó** |Jelszó szöveg | A titkosítási kulcsok jelszava. Erre csak akkor van szükség, ha engedélyezve van-e a titkosítás. A sorrend toorestore egy biztonsági másolat titkosításának engedélyezése, rendelkeznie kell hello helyes jelszót és a kapcsolódó hello a biztonsági mentés készült hello során használt tanúsítvány. |

### <a name="advanced-settings"></a>Speciális beállítások

| Beállítás | Tartomány (alapértelmezett) | Leírás |
| --- | --- | --- |
| **Rendszer-adatbázis biztonsági mentése** | Engedélyezi/letiltja (letiltva) | Ha engedélyezve van, ez a funkció is készít biztonsági másolatot hello rendszeradatbázisokban: Master, MSDB és modell. Hello MSDB és modell olyan adatbázisai esetén ellenőrizze, hogy azok teljes körű helyreállítási módban, ha azt szeretné, hogy a napló biztonsági mentések toobe venni. Napló-biztonságimásolatokat a rendszer soha nem főkiszolgáló hajtja végre. És a TempDB nem készült biztonsági másolat készül. |
| **Biztonsági mentés ütemezése** | Manuális vagy automatikus (automatikus) | Alapértelmezés szerint biztonsági mentés ütemezése hello automatikusan határozza hello napló növekedési alapján. Manuális biztonsági mentés ütemezése hello felhasználói toospecify hello időkerete biztonsági mentést tesz lehetővé. Ebben az esetben a biztonsági másolatok csak érvénybe hello helyén megadott gyakoriságát és hello során megadott időkerete pedig egy adott napon. |
| **Teljes biztonsági mentés gyakorisága** | Napi vagy heti | Teljes biztonsági mentések gyakoriságát. Mindkét esetben a teljes biztonsági mentés hello következő ütemezett időpontban időszak alatt megkezdődik. Heti kiválasztásakor a biztonsági mentések sikerült span több napon belül végre az összes adatbázis sikeresen biztonsági másolatából. |
| **Teljes biztonsági mentés kezdési ideje** | 00:00 – 23:00 (01:00) | Kezdési időpont egy adott nap során, ami teljes biztonsági mentés akkor kerül sor. |
| **Teljes biztonsági mentési időablak** | 1 – 23 óra (1 óra) | Egy adott nap során, ami teljes biztonsági mentés akkor kerül sor hello időkerete időtartama. |
| **Napló biztonsági mentési gyakoriság** | 5 – 60 perc (60 perc) | A napló biztonsági mentések gyakoriságát. |

## <a name="understanding-full-backup-frequency"></a>Teljes biztonsági mentés gyakoriságát ismertetése
Fontos toounderstand hello különbségének napi és heti teljes biztonsági mentés. Ebből a törekvésből végigvezetjük két példaforgatókönyvek keresztül.

### <a name="scenario-1-weekly-backups"></a>1. forgatókönyv: Heti biztonsági mentései
Egy nagyon nagy adatbázisok számát tartalmazó SQL Server virtuális gép van.

Hétfőn automatikus biztonsági mentés v2 a beállítások a következő hello engedélyezése:

- Biztonsági mentés ütemezése: **manuális**
- Teljes biztonsági mentési gyakoriságot: **heti**
- Teljes biztonsági mentés kezdési ideje: **01:00**
- Teljes biztonsági mentési időablak: **1 óra**

Ez azt jelenti, hogy hello tovább elérhető biztonsági mentési időablak kedd 1 óráig 1 órakor. Automatikus biztonsági mentés idő lejárta után megkezdődik egy adatbázis biztonsági másolatának egyszerre. Ebben a forgatókönyvben az adatbázisok kellőképpen hosszúak legyenek, hogy hello első néhány adatbázisok teljes biztonsági mentés befejeződik. Azonban egy óra múlva összes hello adatbázisok biztonsági mentése volt.

Ez akkor fordul elő, amikor automatikus biztonsági mentés megkezdődik a fennmaradó adatbázisok hello másnap, szerda 13: 00 1 óráig: hello biztonsági mentéséről. Ha nem minden adatbázis biztonsági mentése volt, hogy időben, hello újra következő nap hello, azok, azonos megpróbál idő. Továbbiakban ez fog folytatódni mindaddig, amíg az összes adatbázis sikeresen nincs biztonsági másolata.

Ha ismét eléri kedd, automatikus biztonsági mentés megkezdődik a adatbázisainak biztonsági mentése az összes még egyszer.

Ebből a forgatókönyvből megtudhatja, hogy az automatikus biztonsági mentés csak belül hello megadott időkeretnél fog működni, és az egyes adatbázisok készül biztonsági másolat hetente egyszer. Ez is mutatja, hogy lehetséges a biztonsági mentések toospan hello több napokat eset amennyiben nincs lehetséges toocomplete minden biztonsági mentés az egy nap.

### <a name="scenario-2-daily-backups"></a>2. forgatókönyv: Napi biztonsági mentései
Egy nagyon nagy adatbázisok számát tartalmazó SQL Server virtuális gép van.

Hétfőn automatikus biztonsági mentés v2 a beállítások a következő hello engedélyezése:

- Biztonsági mentés ütemezése: manuális
- Teljes biztonsági mentési gyakoriságot: naponta
- Teljes biztonsági mentés kezdési ideje: 22:00
- Teljes biztonsági mentési időablak: 6 óra

Ez azt jelenti, hogy hello tovább elérhető biztonsági mentési időablak hétfő 10 du. hat órán át. Automatikus biztonsági mentés idő lejárta után megkezdődik egy adatbázis biztonsági másolatának egyszerre.

Ezt követően 10 hat órán át keddjén összes adatbázis teljes biztonsági mentés újra elindul.

> [!IMPORTANT]
> Napi biztonsági mentés ütemezése, ajánlott úgy ütemezni a széles idő ablak tooensure összes adatbázis biztonsági másolat készíthető az időben. Ez különösen fontos hello esetben be nagy mennyiségű adatok tooback esetében.

## <a name="configuration-in-hello-portal"></a>A portál hello konfiguráció

Hello Azure portál tooconfigure automatikus biztonsági mentés v2 is használhatja, vagy meglévő SQL Server 2016 virtuális gépek kiépítése során.

### <a name="new-vms"></a>Új virtuális gépek

Hello Azure portál tooconfigure automatikus biztonsági mentés v2 használja, amikor létrehoz egy új SQL Server 2016-virtuális gép hello Resource Manager üzembe helyezési modellben. 

A hello **SQL Server-beállítások** panelen válassza **automatikus biztonsági mentés**. hello Azure portál következő képernyőfelvételen látható hello **SQL automatikus biztonsági mentés** panelen.

![SQL automatikus biztonsági mentés konfigurálása az Azure portálon](./media/virtual-machines-windows-sql-automated-backup-v2/automated-backup-blade.png)

> [!NOTE]
> Az automatikus biztonsági mentés v2 alapértelmezés szerint le van tiltva.

A környezetben, témakörében hello teljes [az Azure SQL Server virtuális gépek kiépítése](virtual-machines-windows-portal-sql-server-provision.md).

### <a name="existing-vms"></a>Meglévő virtuális gépek

Meglévő SQL Server virtuális gépek válassza ki az SQL Server virtuális gépet. Válassza ki hello **SQL Server-konfigurációs** hello szakasza **beállítások** panelen.

![SQL automatikus biztonsági mentés a meglévő virtuális gépek](./media/virtual-machines-windows-sql-automated-backup-v2/sql-server-configuration.png)

A hello **SQL Server-konfigurációs** panelen kattintson hello **szerkesztése** hello gombjára automatikus biztonsági mentési szakasz.

![Meglévő virtuális gépek SQL automatikus biztonsági mentés konfigurálása](./media/virtual-machines-windows-sql-automated-backup-v2/sql-server-configuration-edit.png)

Ha elkészült, kattintson a hello **OK** hello alsó részén hello gombjára **SQL Server-konfigurációs** panel toosave a módosításokat.

Ha engedélyezi az automatikus biztonsági mentés a hello először, Azure SQL Server IaaS-ügynök hello hello háttérben konfigurálja. Ebben az időszakban hello Azure-portálon előfordulhat, hogy jelenjen meg, hogy az automatikus biztonsági mentés konfigurálva van-e. Várjon egy pár percet hello ügynök toobe telepítve, a konfigurált. Miután adott hello Azure portal hello új beállítások fogja tartalmazni.

## <a name="configuration-with-powershell"></a>PowerShell-konfiguráció

PowerShell tooconfigure automatikus biztonsági mentés v2 is használhatja. Mielőtt elkezdené, a következőket kell tennie:

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
Enable                      : True
EnableEncryption            : False
RetentionPeriod             : 30
StorageUrl                  : https://test.blob.core.windows.net/
StorageAccessKey            :  
Password                    : 
BackupSystemDbs             : False
BackupScheduleType          : Manual
FullBackupFrequency         : WEEKLY
FullBackupStartTime         : 2
FullBackupWindowHours       : 2
LogBackupFrequency          : 60
```

Ha a kimeneti azt mutatja, hogy **engedélyezése** értéke túl**hamis**, akkor el kell tooenable automatikus biztonsági mentés. hello jó hírünk engedélyezése és konfigurálása az automatikus biztonsági mentés hello a megszokott módon. Hello ezt az információt a következő szakaszban talál.

> [!NOTE] 
> Hello beállítások módosítása után azonnal ellenőrzését, akkor lehetséges, hogy kap vissza hello régi konfigurációs értékeket. Várjon néhány percet, és ellenőrizze a hello beállításait újra toomake meg arról, hogy a módosítások alkalmazása megtörtént.

### <a name="configure-automated-backup-v2"></a>Az automatikus biztonsági mentési v2 konfigurálása
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

Ezután használja az hello **New-AzureRmVMSqlServerAutoBackupConfig** tooenable parancsot, és a hello Azure storage-fiók automatikus biztonsági mentés v2 hello beállítások toostore biztonsági mentések konfigurálása. Ebben a példában a hello biztonsági mentések toobe 10 napig őrzi meg vannak beállítva. Rendszer-adatbázis biztonsági másolatait engedélyezve vannak. Teljes biztonsági mentések ütemezett heti 20:00 két órán keresztül kezdési idő ablakot. 30 percenként naplóalapú biztonsági mentések ütemezett. a parancs második hello **Set-AzureRmVMSqlServerExtension**, frissítések hello Azure virtuális Géphez megadott ezekkel a beállításokkal.

```powershell
$autobackupconfig = New-AzureRmVMSqlServerAutoBackupConfig -Enable `
    -RetentionPeriodInDays 10 -StorageContext $storage.Context `
    -ResourceGroupName $storage_resourcegroupname -BackupSystemDbs `
    -BackupScheduleType Manual -FullBackupFrequency Weekly `
    -FullBackupStartHour 20 -FullBackupWindowInHours 2 `
    -LogBackupFrequencyInMinutes 30 

Set-AzureRmVMSqlServerExtension -AutoBackupSettings $autobackupconfig `
    -VMName $vmname -ResourceGroupName $resourcegroupname 
```

Nem sikerült eltarthat néhány percig tooinstall és hello SQL Server IaaS-ügynök konfigurálása. 

tooenable titkosítási hello előző parancsfájl toopass hello módosítása **EnableEncryption** paraméter és egy jelszót (biztonságos karakterlánc) hello **CertificatePassword** paraméter. hello következő parancsfájl lehetővé teszi, hogy az előző példában hello hello automatikus biztonsági mentés beállításait, és hozzáadja titkosítás.

```powershell
$password = "P@ssw0rd"
$encryptionpassword = $password | ConvertTo-SecureString -AsPlainText -Force  

$autobackupconfig = New-AzureRmVMSqlServerAutoBackupConfig -Enable `
    -EnableEncryption -CertificatePassword $encryptionpassword `
    -RetentionPeriodInDays 10 -StorageContext $storage.Context `
    -ResourceGroupName $storage_resourcegroupname -BackupSystemDbs `
    -BackupScheduleType Manual -FullBackupFrequency Weekly `
    -FullBackupStartHour 20 -FullBackupWindowInHours 2 `
    -LogBackupFrequencyInMinutes 30 

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
$backupscheduletype = "Manual"
$fullbackupfrequency = "Weekly"
$fullbackupstarthour = "20"
$fullbackupwindow = "2"
$logbackupfrequency = "30"

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
    -ResourceGroupName $storage_resourcegroupname -BackupSystemDbs `
    -BackupScheduleType $backupscheduletype -FullBackupFrequency $fullbackupfrequency `
    -FullBackupStartHour $fullbackupstarthour -FullBackupWindowInHours $fullbackupwindow `
    -LogBackupFrequencyInMinutes $logbackupfrequency

# Apply hello Automated Backup settings toohello VM

Set-AzureRmVMSqlServerExtension -AutoBackupSettings $autobackupconfig `
    -VMName $vmname -ResourceGroupName $resourcegroupname
```

## <a name="next-steps"></a>Következő lépések
Az automatikus biztonsági mentés v2 való felügyelt biztonsági mentésének konfigurálása Azure virtuális gépeken. Ezért fontos túl[tekintse meg a felügyelt biztonsági mentéshez hello dokumentációt](https://msdn.microsoft.com/library/dn449496.aspx) toounderstand hello viselkedést és megvalósítását.

További biztonsági mentés található, és állítsa vissza a következő témakör hello útmutatást az SQL Server Azure virtuális gépeken: [biztonsági mentése és visszaállítása az SQL Server Azure virtuális gépek](virtual-machines-windows-sql-backup-recovery.md).

Más elérhető automation feladatokkal kapcsolatos további információkért lásd: [SQL Server infrastruktúra-szolgáltatási ügynök bővítmény](virtual-machines-windows-sql-server-agent-extension.md).

További információ az Azure virtuális gépeken futó SQL Server: [SQL Server Azure virtuális gépek – áttekintés](virtual-machines-windows-sql-server-iaas-overview.md).

