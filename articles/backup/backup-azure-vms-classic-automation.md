---
title: "aaaDeploy és a biztonsági mentés kezelése az Azure virtuális gépek PowerShell használatával |} Microsoft Docs"
description: "Megtudhatja, hogyan toodeploy és kezelése az Azure Backup szolgáltatáshoz a PowerShell használatával."
services: backup
documentationcenter: 
author: markgalioto
manager: carmonm
editor: 
ms.assetid: 2e24b1d9-4375-4049-a28d-e3bc01152f32
ms.service: backup
ms.workload: storage-backup-recovery
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 8/2/2017
ms.author: markgal;trinadhk;jimpark
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: f3ecd3f94c5e3e8fc8018e8786302edd4847744b
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="use-azurermbackup-cmdlets-tooback-up-virtual-machines"></a>Virtuális gépek AzureRM.Backup parancsmagok tooback használata
> [!div class="op_single_selector"]
> * [Resource Manager](backup-azure-vms-automation.md)
> * [Klasszikus](backup-azure-vms-classic-automation.md)
>
>

Ez a cikk bemutatja, hogyan toouse Azure PowerShell, a biztonsági mentése és helyreállítása Azure virtuális gépeken. Az Azure két különböző üzemi modellel rendelkezik az erőforrások létrehozásához és használatához: Resource Manager-alapú és klasszikus. Ez a cikk ismerteti, használatával hello klasszikus telepítési modell tooback be adatok tooa Backup-tárolóban. Ha nem hozott létre a biztonsági másolatok tárolóját az előfizetéshez, lásd: hello Resource Manager verziója, ez a cikk [használata AzureRM.RecoveryServices.Backup parancsmagok tooback virtuális gépek](backup-azure-vms-automation.md). A Microsoft azt javasolja, hogy az új telepítések esetén hello Resource Manager modellt használja.

> [!IMPORTANT]
> Most már frissítheti a mentési tárolók tooRecovery szolgáltatások tárolókban. További információkért lásd: hello cikk [frissíteni a biztonsági mentési tároló tooa Recovery Services-tároló](backup-azure-upgrade-backup-to-recovery-services.md). A Microsoft tooupgrade támogatja a mentési tárolókat tooRecovery szolgáltatások tárolók.<br/> 2017. október 15. után PowerShell toocreate mentési tárolókban nem használható. **2017. november 1-től**:
>- Az összes többi biztonsági mentési tárolók lesz automatikusan frissített tooRecovery szolgáltatások tárolók.
>- Ön nem fogja tudni tooaccess a biztonsági mentési adatok hello a klasszikus portálon. Ehelyett használjon hello Azure portál tooaccess a biztonsági mentési adatok a Recovery Services-tárolók.
>

## <a name="concepts"></a>Alapelvek
Ez a cikk adott toohello PowerShell-parancsmagok használt virtuális gépek tooback információkat biztosít. Az Azure virtuális gépek védelméről bevezető információkat talál [tervezze meg a virtuális gép biztonsági mentési infrastruktúra az Azure-ban](backup-azure-vms-introduction.md).

> [!NOTE]
> Kezdés előtt olvassa el a hello [Előfeltételek](backup-azure-vms-prepare.md) az Azure biztonsági mentési és hello szükséges toowork [korlátozások](backup-azure-vms-prepare.md#limitations-when-backing-up-and-restoring-a-vm) hello aktuális virtuális gép biztonsági mentési megoldás.
>
>

toouse PowerShell hatékony, eltarthat egy rövid ideig toounderstand hello hierarchiában, az objektumok és honnan toostart.

![Eltér az objektumhierarchia](./media/backup-azure-vms-classic-automation/object-hierarchy.png)

hello két legfontosabb adatfolyamok engedélyezze a virtuális gép védelmét, és adatok helyreállításához a helyreállítási pontból. hello Ez a cikk célja toohelp adept, két forgatókönyvekben hello PowerShell parancsmagok tooenable használata válik.

## <a name="setup-and-registration"></a>Telepítését és regisztrálását
toobegin:

1. [Töltse le a legfrissebb PowerShell](https://github.com/Azure/azure-powershell/releases) (szükséges minimális verziója: 1.0.0)
2. Keresse meg a rendelkezésre álló hello Azure biztonsági mentés PowerShell-parancsmagok hello a következő parancs beírásával:

```
PS C:\> Get-Command *azurermbackup*

CommandType     Name                                               Version    Source
-----------     ----                                               -------    ------
Cmdlet          Backup-AzureRmBackupItem                           1.0.1      AzureRM.Backup
Cmdlet          Disable-AzureRmBackupProtection                    1.0.1      AzureRM.Backup
Cmdlet          Enable-AzureRmBackupContainerReregistration        1.0.1      AzureRM.Backup
Cmdlet          Enable-AzureRmBackupProtection                     1.0.1      AzureRM.Backup
Cmdlet          Get-AzureRmBackupContainer                         1.0.1      AzureRM.Backup
Cmdlet          Get-AzureRmBackupItem                              1.0.1      AzureRM.Backup
Cmdlet          Get-AzureRmBackupJob                               1.0.1      AzureRM.Backup
Cmdlet          Get-AzureRmBackupJobDetails                        1.0.1      AzureRM.Backup
Cmdlet          Get-AzureRmBackupProtectionPolicy                  1.0.1      AzureRM.Backup
Cmdlet          Get-AzureRmBackupRecoveryPoint                     1.0.1      AzureRM.Backup
Cmdlet          Get-AzureRmBackupVault                             1.0.1      AzureRM.Backup
Cmdlet          Get-AzureRmBackupVaultCredentials                  1.0.1      AzureRM.Backup
Cmdlet          New-AzureRmBackupProtectionPolicy                  1.0.1      AzureRM.Backup
Cmdlet          New-AzureRmBackupRetentionPolicyObject             1.0.1      AzureRM.Backup
Cmdlet          New-AzureRmBackupVault                             1.0.1      AzureRM.Backup
Cmdlet          Register-AzureRmBackupContainer                    1.0.1      AzureRM.Backup
Cmdlet          Remove-AzureRmBackupProtectionPolicy               1.0.1      AzureRM.Backup
Cmdlet          Remove-AzureRmBackupVault                          1.0.1      AzureRM.Backup
Cmdlet          Restore-AzureRmBackupItem                          1.0.1      AzureRM.Backup
Cmdlet          Set-AzureRmBackupProtectionPolicy                  1.0.1      AzureRM.Backup
Cmdlet          Set-AzureRmBackupVault                             1.0.1      AzureRM.Backup
Cmdlet          Stop-AzureRmBackupJob                              1.0.1      AzureRM.Backup
Cmdlet          Unregister-AzureRmBackupContainer                  1.0.1      AzureRM.Backup
Cmdlet          Wait-AzureRmBackupJob                              1.0.1      AzureRM.Backup
```

hello a következő és nyilvántartási feladatok automatizálhatók a PowerShell használatával:

* Backup-tároló létrehozása
* Hello virtuális gépek regisztrálása hello Azure Backup szolgáltatás

### <a name="create-a-backup-vault"></a>Backup-tároló létrehozása
> [!WARNING]
> A felhasználó Azure Backup segítségével hello az első alkalommal esetén tooregister hello Azure biztonsági mentés szolgáltató toobe használja az előfizetéskor kell. Ezt megteheti hello a következő parancs futtatásával: Register-AzureRmResourceProvider - ProviderNamespace "Microsoft.Backup"
>
>

Létrehozhat egy új mentési tárolót használó hello **New-AzureRmBackupVault** parancsmag. hello mentési tároló egy ARM-erőforrás, ezért meg kell tooplace az erőforráscsoporton belül. Egy rendszergazda jogú Azure PowerShell-konzolban futtassa a következő parancsok hello:

```
PS C:\> New-AzureRmResourceGroup –Name “test-rg” –Location “West US”
PS C:\> $backupvault = New-AzureRmBackupVault –ResourceGroupName “test-rg” –Name “test-vault” –Region “West US” –Storage GeoRedundant
```

Minden hello mentési tárolók listája egy adott feliratkozás hello olvashatók be **Get-AzureRmBackupVault** parancsmag.

> [!NOTE]
> Tetszés szerinti toostore hello mentési tároló objektum egy változóba. sok Azure Backup-parancsmagokhoz tartozó bemeneti adatokként hello tároló objektum szükséges.
>
>

### <a name="registering-hello-vms"></a>Hello virtuális gépek regisztrálása
hello első lépést konfigurálása az Azure Backup szolgáltatás biztonsági mentési van tooregister a számítógép vagy virtuális gép egy Azure Backup-tárolóban. Hello **Register-AzureRmBackupContainer** parancsmag hello Azure IaaS virtuális gépként bemeneti adatokat fogad, és regisztrálja azt hello megadott tárolóban. hello regisztrációs műveletet hello Azure virtuális gép társítja hello mentési tárolót, és nyomon követi a virtuális gép hello hello biztonsági mentési teljes életciklusukon keresztül.

A virtuális gép regisztrálása hello Azure Backup szolgáltatás hoz létre a legfelső szintű tároló objektumok. A tároló általában több olyan elemeket tartalmaz, biztonsági másolat készíthető, de hello esetben a virtuális gépek nem lesznek hello tároló csak egy biztonsági mentési elemet.

```
PS C:\> $registerjob = Register-AzureRmBackupContainer -Vault $backupvault -Name "testvm" -ServiceName "testvm"
```

## <a name="backup-azure-vms"></a>Azure-beli virtuális gépek biztonsági mentése
### <a name="create-a-protection-policy"></a>Védelmi házirend létrehozása
Kötelező toocreate egy új védelmi házirend toostart biztonsági másolatot a virtuális gépek nincs. hello tároló tartalmaz egy "alapértelmezett házirendet a" védelem engedélyezése a használt tooquickly lehet, és majd hello jobb oldali később szerkeszthetők. Hello tárolóban elérhető hello házirendek listáját kaphat hello segítségével **Get-AzureRmBackupProtectionPolicy** parancsmagot:

```
PS C:\> Get-AzureRmBackupProtectionPolicy -Vault $backupvault

Name                      Type               ScheduleType       BackupTime
----                      ----               ------------       ----------
DefaultPolicy             AzureVM            Daily              26-Aug-15 12:30:00 AM
```

> [!NOTE]
> hello BackupTime mezőt a PowerShellben hello időzónáját UTC. Azonban hello biztonságimásolat-készítési időpont látható hello Azure-portálon, hello időzóna esetén igazított tooyour helyi rendszer hello UTC eltolás mellett.
>
>

A biztonsági mentési házirend legalább egy megőrzési házirend társítva. hello megőrzési házirend határozza meg, hány helyreállítási pont tartják az Azure Backup szolgáltatással. Hello **New-AzureRmBackupRetentionPolicy** parancsmag megőrzési házirend adatokat PowerShell-objektumokat hoz létre. A megőrzési csoportházirend-objektumokat kell használni, mint a bemeneti toohello *New-AzureRmBackupProtectionPolicy* parancsmagot, vagy közvetlenül az hello *Enable-AzureRmBackupProtection* parancsmag.

A biztonsági mentési házirend határozza meg, mikor és milyen gyakran hello elem biztonsági mentését történik. Hello **New-AzureRmBackupProtectionPolicy** parancsmag létrehoz egy PowerShell-objektum, amely tartalmazza a biztonsági mentési házirend. hello biztonsági mentési házirend szolgál egy bemeneti toohello *Enable-AzureRmBackupProtection* parancsmag.

```
PS C:\> $Daily = New-AzureRmBackupRetentionPolicyObject -DailyRetention -Retention 30
PS C:\> $newpolicy = New-AzureRmBackupProtectionPolicy -Name DailyBackup01 -Type AzureVM -Daily -BackupTime ([datetime]"3:30 PM") -RetentionPolicy $Daily -Vault $backupvault

Name                      Type               ScheduleType       BackupTime
----                      ----               ------------       ----------
DailyBackup01             AzureVM            Daily              01-Sep-15 3:30:00 PM
```

### <a name="enable-protection"></a>Védelem engedélyezése
Védelem engedélyezése magában foglalja a két objektum – hello elem és hello házirend, és mindkét kell toobelong toohello azonos tároló. Miután hello házirend társítva hello elem, hello biztonsági mentési munkafolyamat fog indítsa hello meghatározott ütemezés szerint.

```
PS C:\> Get-AzureRmBackupContainer -Type AzureVM -Status Registered -Vault $backupvault | Get-AzureRmBackupItem | Enable-AzureRmBackupProtection -Policy $newpolicy
```

### <a name="initial-backup"></a>Kezdeti biztonsági mentés
hello biztonsági mentés ütemezése hello azt követő biztonsági mentéseket a hello teljes kezdeti hello elem és hello növekményes módon kezeli. Azonban ha azt szeretné, hogy tooforce hello kezdeti biztonsági mentési toohappen bizonyos egyszerre, vagy akár azonnal használja hello **Backup-AzureRmBackupItem** parancsmagot:

```
PS C:\> $container = Get-AzureRmBackupContainer -Vault $backupvault -Type AzureVM -Name "testvm"
PS C:\> $backupjob = Get-AzureRmBackupItem -Container $container | Backup-AzureRmBackupItem
PS C:\> $backupjob

WorkloadName    Operation       Status          StartTime              EndTime
------------    ---------       ------          ---------              -------
testvm          Backup          InProgress      01-Sep-15 12:24:01 PM  01-Jan-01 12:00:00 AM
```

> [!NOTE]
> hello StartTime időzónáját hello és PowerShell EndTime mezőlistájában UTC. Azonban hello hasonló információkat hello Azure-portálon látható, hello időzóna esetén igazított tooyour rendszeróra.
>
>

### <a name="monitoring-a-backup-job"></a>A biztonsági mentési feladatot figyelése
A legtöbb hosszú ideig futó műveletek Azure Backup feladatként van modellezni. Így könnyen tootrack folyamatban mindig tookeep hello Azure portál megnyitása nélkül.

tooget hello legújabb egy folyamatban lévő feladat állapota, használjon hello **Get-AzureRmBackupJob** parancsmag.

```
PS C:\> $joblist = Get-AzureRmBackupJob -Vault $backupvault -Status InProgress
PS C:\> $joblist[0]

WorkloadName    Operation       Status          StartTime              EndTime
------------    ---------       ------          ---------              -------
testvm          Backup          InProgress      01-Sep-15 12:24:01 PM  01-Jan-01 12:00:00 AM
```

Ezek a feladatok befejezésére – ami szükségtelen, további kód - lekérdezési helyett egyszerűbb toouse hello **várakozási-AzureRmBackupJob** parancsmag. Egy parancsfájlban használatakor hello parancsmag hello végrehajtási idejére felfüggeszti hello feladat befejeződik, vagy hello megadott időtúllépési érték elérésekor.

```
PS C:\> Wait-AzureRmBackupJob -Job $joblist[0] -Timeout 43200
```


## <a name="restore-an-azure-vm"></a>Állítsa vissza az Azure virtuális gép
A sorrend toorestore biztonsági mentési adatokat tooidentify hello biztonsági másolat elem van szüksége, és hello helyreállítási pont, amely hello időpontban adatokat tartalmaz. Ez az információ a megadott toohello visszaállítási-AzureRmBackupItem parancsmag tooinitiate adatainak hello tároló toohello felhasználói fiókhoz való visszaállítás.

### <a name="select-hello-vm"></a>Válassza ki a virtuális gép hello
tooget hello PowerShell-objektum, amely azonosítja a helyes biztonságimásolat-cikk hello, a tároló hello hello tárolóban toostart kell, és működnek az objektum hierarchia valamennyi alsóbb szintjén. virtuális gép, használjon hello hello jelölő tooselect hello tároló **Get-AzureRmBackupContainer** parancsmag és az adott toohello pipe **Get-AzureRmBackupItem** parancsmag.

```
PS C:\> $backupitem = Get-AzureRmBackupContainer -Vault $backupvault -Type AzureVM -name "testvm" | Get-AzureRmBackupItem
```

### <a name="choose-a-recovery-point"></a>A helyreállítási pont kiválasztása
Most listázhatja hello hello biztonsági mentési elem hello használata az összes helyreállítási pontról **Get-AzureRmBackupRecoveryPoint** parancsmagot, és válassza ki a hello helyreállítási pont toorestore. Általában válassza ki a felhasználók a legutóbbi hello *AppConsistent* hello listában mutasson.

```
PS C:\> $rp =  Get-AzureRmBackupRecoveryPoint -Item $backupitem
PS C:\> $rp

RecoveryPointId    RecoveryPointType  RecoveryPointTime      ContainerName
---------------    -----------------  -----------------      -------------
15273496567119     AppConsistent      01-Sep-15 12:27:38 PM  iaasvmcontainer;testvm;testv...
```

hello változó ```$rp``` a helyreállítási pontok egy tömb van hello kijelölt elem biztonsági mentése, fordított sorrendben idő – hello legújabb helyreállítási pont 0. indexnél. Használja a következő szabványos PowerShell tömböt indexelő toopick hello helyreállítási pontot. Például: ```$rp[0]``` hello legutóbbi helyreállítási pontot választja.

### <a name="restoring-disks"></a>Lemezek visszaállítása
Hello Azure-portálon keresztül, és Azure PowerShell használatával végzett hello visszaállítási műveletek közötti fő különbség van. A PowerShell-lel hello visszaállítási művelet megáll hello lemezét és konfigurációs adatát visszaállítása hello helyreállítási pontból. Nem hoz létre egy virtuális gépet.

> [!WARNING]
> hello visszaállítási-AzureRmBackupItem nem hoz létre egy virtuális Gépet. Csak visszaállítja a hello lemezek toohello megadott tárfiók. Ez a rendszer nem hello fog tapasztalni hello Azure-portálon a kívánt viselkedést eredményező beállítást.
>
>

```
PS C:\> $restorejob = Restore-AzureRmBackupItem -StorageAccountName "DestAccount" -RecoveryPoint $rp[0]
PS C:\> $restorejob

WorkloadName    Operation       Status          StartTime              EndTime
------------    ---------       ------          ---------              -------
testvm          Restore         InProgress      01-Sep-15 1:14:01 PM   01-Jan-01 12:00:00 AM
```

Hello részletekért hello visszaállítási művelet használatával hello **Get-AzureRmBackupJobDetails** parancsmag hello visszaállítási feladat befejezése után. Hello *hiba részletei* tulajdonságnak lesz hello információ szükséges toorebuild hello virtuális gép.

```
PS C:\> $restorejob = Get-AzureRmBackupJob -Job $restorejob
PS C:\> $details = Get-AzureRmBackupJobDetails -Job $restorejob
```

### <a name="build-hello-vm"></a>Hello virtuális gép létrehozása
Épület hello VM kívül vissza hello lemezek végezhető hello régebbi Azure Service Management PowerShell-parancsmagok használatával, új Azure Resource Manager-sablonok hello, vagy hello Azure-portál használatával. Az első példában bemutatjuk a hogyan tooget ott hello Azure Szolgáltatáskezelés-parancsmagok használatával.

```
$properties  = $details.Properties

$storageAccountName = $properties["Target Storage Account Name"]
$containerName = $properties["Config Blob Container Name"]
$blobName = $properties["Config Blob Name"]

$keys = Get-AzureStorageKey -StorageAccountName $storageAccountName
$storageAccountKey = $keys.Primary
$storageContext = New-AzureStorageContext -StorageAccountName $storageAccountName -StorageAccountKey $storageAccountKey


$destination_path = "C:\Users\admin\Desktop\vmconfig.xml"
Get-AzureStorageBlobContent -Container $containerName -Blob $blobName -Destination $destination_path -Context $storageContext


$obj = [xml](((Get-Content -Path $destination_path -Encoding UniCode)).TrimEnd([char]0x00))
$pvr = $obj.PersistentVMRole
$os = $pvr.OSVirtualHardDisk
$dds = $pvr.DataVirtualHardDisks
$osDisk = Add-AzureDisk -MediaLocation $os.MediaLink -OS $os.OS -DiskName "panbhaosdisk"
$vm = New-AzureVMConfig -Name $pvr.RoleName -InstanceSize $pvr.RoleSize -DiskName $osDisk.DiskName

if (!($dds -eq $null))
{
    foreach($d in $dds.DataVirtualHardDisk)
    {
        $lun = 0
        if(!($d.Lun -eq $null))
        {
            $lun = $d.Lun
        }
        $name = "panbhadataDisk" + $lun
        Add-AzureDisk -DiskName $name -MediaLocation $d.MediaLink
        $vm | Add-AzureDataDisk -Import -DiskName $name -LUN $lun
    }
}

New-AzureVM -ServiceName "panbhasample" -Location "SouthEast Asia" -VM $vm
```

További információt a toobuild egy virtuális merevlemezről vissza hello, olvassa el a következő parancsmagok hello:

* [Adja hozzá AzureDisk](https://msdn.microsoft.com/library/azure/dn495252.aspx)
* [Új AzureVMConfig](https://msdn.microsoft.com/library/azure/dn495159.aspx)
* [Új AzureVM](https://msdn.microsoft.com/library/azure/dn495254.aspx)

## <a name="code-samples"></a>Kódminták
### <a name="1-get-hello-completion-status-of-job-sub-tasks"></a>1. A feladat alfeladatok hello befejezési állapotának beolvasása
tootrack hello befejezési állapotát az egyes alfeladatok hello használható **Get-AzureRmBackupJobDetails** parancsmagot:

```
PS C:\> $details = Get-AzureRmBackupJobDetails -JobId $backupjob.InstanceId -Vault $backupvault
PS C:\> $details.SubTasks

Name                                                        Status
----                                                        ------
Take Snapshot                                               Completed
Transfer data tooBackup vault                               InProgress
```

### <a name="2-create-a-dailyweekly-report-of-backup-jobs"></a>2. A biztonsági mentési feladatok napi vagy heti jelentés létrehozása
A rendszergazdák általában érdemes tooknow hello utolsó 24 órában, milyen biztonsági mentési feladat futtatása adott biztonsági mentési feladatok állapotának hello. Emellett az átvitt adatok mennyisége hello lehetővé teszi a rendszergazdák olyan módon tooestimate havi használati adatok. az alábbi parancsfájl hello hello hello Azure Backup szolgáltatás nyers adatait kéri le, és hello megjeleníti hello PowerShell-konzolban.

```
param(  [Parameter(Mandatory=$True,Position=1)]
        [string]$backupvaultname,

        [Parameter(Mandatory=$False,Position=2)]
        [int]$numberofdays = 7)


#Initialize variables
$DAILYBACKUPSTATS = @()
$backupvault = Get-AzureRmBackupVault -Name $backupvaultname
$enddate = ([datetime]::Today).AddDays(1)
$startdate = ([datetime]::Today)

for( $i = 1; $i -le $numberofdays; $i++ )
{
    # We query one day at a time because pulling 7 days of data might be too much
    $dailyjoblist = Get-AzureRmBackupJob -Vault $backupvault -From $startdate -too$enddate -Type AzureVM -Operation Backup
    Write-Progress -Activity "Getting job information for hello last $numberofdays days" -Status "Day -$i" -PercentComplete ([int]([decimal]$i*100/$numberofdays))

    foreach( $job in $dailyjoblist )
    {
        #Extract hello information for hello reports
        $newstatsobj = New-Object System.Object
        $newstatsobj | Add-Member -Type NoteProperty -Name Date -Value $startdate
        $newstatsobj | Add-Member -Type NoteProperty -Name VMName -Value $job.WorkloadName
        $newstatsobj | Add-Member -Type NoteProperty -Name Duration -Value $job.Duration
        $newstatsobj | Add-Member -Type NoteProperty -Name Status -Value $job.Status

        $details = Get-AzureRmBackupJobDetails -Job $job
        $newstatsobj | Add-Member -Type NoteProperty -Name BackupSize -Value $details.Properties["Backup Size"]
        $DAILYBACKUPSTATS += $newstatsobj
    }

    $enddate = $enddate.AddDays(-1)
    $startdate = $startdate.AddDays(-1)
}

$DAILYBACKUPSTATS | Out-GridView
```

Ha azt szeretné, hogy tooadd diagramkészítési képességek toothis jelentés kimenetében, ismerje meg, a hello TechNet blogbejegyzés [diagramkészítési a PowerShell használatával](http://blogs.technet.com/b/richard_macdonald/archive/2009/04/28/3231887.aspx)

## <a name="next-steps"></a>Következő lépések
Ha jobban szeret PowerShell tooengage használata az Azure-erőforrások, tekintse meg a Windows Server védelmének hello PowerShell cikk [telepítés és a Windows Server biztonsági másolat kezelése](backup-client-automation-classic.md). Is van a DPM biztonsági mentések kezelése a PowerShell cikk [telepítés és a DPM a biztonsági mentés kezelése](backup-dpm-automation-classic.md). Ezek a cikkek mindegyikét a Resource Manager üzembe helyezések és a klasszikus központi telepítések verziója szükséges.
