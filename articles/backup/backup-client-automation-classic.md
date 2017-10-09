---
title: "aaaUse PowerShell toomanage Windows Server biztonsági mentése az Azure-ban |} Microsoft Docs"
description: "Telepítése és kezelése a PowerShell használatával Windows Server biztonsági mentés."
services: backup
documentationcenter: 
author: saurabhsensharma
manager: shivamg
editor: 
ms.assetid: e7e269e2-1f11-41a9-957b-a2155de6a1e0
ms.service: backup
ms.workload: storage-backup-recovery
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/02/2017
ms.author: saurse;markgal;nkolli;trinadhk
ms.openlocfilehash: 72292e510b0f059102440bd49a195be4ef700a6a
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="deploy-and-manage-backup-tooazure-for-windows-serverwindows-client-using-powershell"></a>Központi telepítése és kezelése a biztonsági mentési tooAzure Windows Server és Windows-ügyfél PowerShell használatával
> [!div class="op_single_selector"]
> * [ARM](backup-client-automation.md)
> * [Klasszikus](backup-client-automation-classic.md)
>
>

Ez a cikk ismerteti, hogyan készíthet a Windows Server és a Windows munkaállomás adatok tooa toouse PowerShell tooback biztonsági mentési tároló. A Microsoft azt javasolja, hogy az összes új központi telepítéseknél Recovery Services-tárolók használatával. Ha egy új Azure biztonságimásolat-felhasználó, és nem hozott létre egy mentési tárolót az előfizetéshez, használja a hello cikk [központi telepítése és kezelése a PowerShell használatával a Data Protection Manager-adatok tooAzure](backup-client-automation.md) , az adatok tárolása a Recovery Services-tároló. 

> [!IMPORTANT]
> Most már frissítheti a mentési tárolók tooRecovery szolgáltatások tárolókban. További információkért lásd: hello cikk [frissíteni a biztonsági mentési tároló tooa Recovery Services-tároló](backup-azure-upgrade-backup-to-recovery-services.md). A Microsoft tooupgrade támogatja a mentési tárolókat tooRecovery szolgáltatások tárolók.<br/> 2017. október 15. után PowerShell toocreate mentési tárolókban nem használható. **2017. november 1-től**:
>- Az összes többi biztonsági mentési tárolók lesz automatikusan frissített tooRecovery szolgáltatások tárolók.
>- Ön nem fogja tudni tooaccess a biztonsági mentési adatok hello a klasszikus portálon. Ehelyett használjon hello Azure portál tooaccess a biztonsági mentési adatok a Recovery Services-tárolók.
>

## <a name="install-azure-powershell"></a>Az Azure PowerShell telepítése
[!INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-include.md)]

2015 október, az Azure PowerShell 1.0-s verziójában jelent meg. Ebben a kiadásban hello 0.9.8-as kiadása sikeresen befejeződött, és néhány jelentős módosításokat, különösen a hello elnevezési hello parancsmagok állapotba. 1.0-ás parancsmagok kövesse hello elnevezési {ige}-AzureRm {főnév}; mivel a hello 0.9.8-as nevek viszont nem tartalmazzák **Rm** (például New-AzureRmResourceGroup New-használják helyett). Ha az Azure PowerShell 0.9.8-as, először engedélyeznie kell hello Resource Manager módra hello futtatásával **Switch-AzureMode AzureResourceManager** parancsot. Ez a parancs nincs szükség az 1.0-s vagy újabb.

Ha azt szeretné, toouse hello 0.9.8-as környezet hello 1.0-s vagy újabb környezetben, a parancsfájlokat körültekintően tesztelje hello parancsfájlok éles üzem előtti környezetben tooavoid éles használat előtt váratlan hatása.

[Töltse le a legfrissebb verzióját hello a PowerShell](https://github.com/Azure/azure-powershell/releases) (szükséges minimális verziója: 1.0.0)

[!INCLUDE [arm-getting-setup-powershell](../../includes/arm-getting-setup-powershell.md)]

## <a name="create-a-backup-vault"></a>Backup-tároló létrehozása
> [!WARNING]
> A felhasználó Azure Backup segítségével hello az első alkalommal esetén tooregister hello Azure biztonsági mentés szolgáltató toobe használja az előfizetéskor kell. Ezt megteheti hello a következő parancs futtatásával: Register-AzureProvider - ProviderNamespace "Microsoft.Backup"
>
>

Létrehozhat egy új mentési tárolót használó hello **New-AzureRMBackupVault** parancsmag. hello mentési tároló egy ARM-erőforrás, ezért meg kell tooplace az erőforráscsoporton belül. Egy rendszergazda jogú Azure PowerShell-konzolban futtassa a következő parancsok hello:

```
PS C:\> New-AzureResourceGroup –Name “test-rg” -Region “West US”
PS C:\> $backupvault = New-AzureRMBackupVault –ResourceGroupName “test-rg” –Name “test-vault” –Region “West US” –Storage GeoRedundant
```

Használjon hello **Get-AzureRMBackupVault** parancsmag toolist hello mentési tárolókat egy előfizetésben.

## <a name="installing-hello-azure-backup-agent"></a>Hello Azure Backup szolgáltatás ügynökének telepítése
Hello Azure Backup szolgáltatás ügynökének telepítése előtt kell toohave hello telepítő letöltött és a Windows Server hello megtalálható. Hello installer legújabb verzióját hello letölthető hello [Microsoft Download Center](http://aka.ms/azurebackup_agent) vagy hello mentési tároló irányítópult-oldalon. Mentés hello telepítő tooan könnyen hozzáférhető helyen, például * C:\Downloads\*.

tooinstall hello ügynök, futtassa a következő parancsot egy rendszergazda jogú PowerShell-konzolban hello:

```
PS C:\> MARSAgentInstaller.exe /q
```

Ezzel telepít hello ügynök minden hello alapértelmezett beállításokkal. hello telepítési hello háttérben néhány percet vesz igénybe. Ha nem adja meg a hello */nu* lehetőséget, majd hello **Windows Update** ablak nyílik meg hello telepítési toocheck bármely frissítések hello végén. A telepítést követően hello ügynök hello telepített programok listájában jelennek meg.

a telepített programok toosee hello listája, nyissa meg túl**Vezérlőpult** > **programok** > **programok és szolgáltatások**.

![Az ügynök telepítve](./media/backup-client-automation/installed-agent-listing.png)

### <a name="installation-options"></a>Telepítési beállítások
az összes elérhető beállítások hello toosee hello parancssori, használja a következő parancs hello:

```
PS C:\> MARSAgentInstaller.exe /?
```

hello elérhető lehetőségek a következők:

| Beállítás | Részletek | Alapértelmezett |
| --- | --- | --- |
| /q |Csendes telepítés |- |
| / p: "hely" |Elérési út toohello telepítési mappáját hello Azure Backup szolgáltatás ügynöke. |C:\Program Files\Microsoft Azure Recovery Services Agent ügynök |
| / s: "hely" |Elérési út toohello gyorsítótármappája hello Azure Backup szolgáltatás ügynöke. |C:\Program Files\Microsoft Azure Recovery Services Agent\Scratch |
| /m |Részt tooMicrosoft frissítés |- |
| /Nu |Ne keressen frissítéseket telepítésének befejezése után |- |
| /d |Eltávolítja a Microsoft Azure Recovery Services Agent ügynök |- |
| /pH |Állomás proxycím |- |
| /po |Proxy Host Port száma |- |
| /Pu |Proxy-állomás felhasználónév |- |
| /pW |Proxy jelszava |- |

## <a name="registering-with-hello-azure-backup-service"></a>Hello Azure Backup szolgáltatás regisztrálása
Az Azure Backup szolgáltatás hello regisztrálhatja, jóvá kell tooensure adott hello [Előfeltételek](backup-configure-vault.md) teljesülnek. A következők szükségesek:

* Egy érvényes Azure-előfizetése
* Rendelkezik egy biztonsági mentési tárolóval

toodownload hello tárolói hitelesítő adatokat, futtassa a hello **Get-AzureRMBackupVaultCredentials** parancsmag egy Azure PowerShell-konzolban és a tároló azt egy tetszőleges helyen, például a * C:\Downloads\*.

```
PS C:\> $credspath = "C:\"
PS C:\> $credsfilename = Get-AzureRMBackupVaultCredentials -Vault $backupvault -TargetLocation $credspath
PS C:\> $credsfilename
f5303a0b-fae4-4cdb-b44d-0e4c032dde26_backuprg_backuprn_2015-08-11--06-22-35.VaultCredentials
```

Regisztrálásakor hello gép hello tárolóban történik hello segítségével [Start-OBRegistration](https://technet.microsoft.com/library/hh770398%28v=wps.630%29.aspx) parancsmagot:

```
PS C:\> $cred = $credspath + $credsfilename
PS C:\> Start-OBRegistration -VaultCredentials $cred -Confirm:$false

CertThumbprint      : 7a2ef2caa2e74b6ed1222a5e89288ddad438df2
SubscriptionID      : ef4ab577-c2c0-43e4-af80-af49f485f3d1
ServiceResourceName : test-vault
Region              : West US

Machine registration succeeded.
```

> [!IMPORTANT]
> Ne használjon relatív elérési utak toospecify hello tároló hitelesítési adatait tartalmazó fájlt. Egy bemeneti toohello parancsmagot, abszolút elérési utat kell megadnia.
>
>

## <a name="networking-settings"></a>Hálózati beállítások
Hello hello csatlakoztatásához Windows számítógép-toohello internet proxykiszolgálón keresztül történik, amikor hello proxybeállítások is megadható toohello ügynök. Ebben a példában nincs nincs proxykiszolgáló, explicit módon azt vannak törlésével bármely proxy kapcsolatos információkat.

A sávszélesség-használat is szabályozhatja az hello beállításokkal ```work hour bandwidth``` és ```non-work hour bandwidth``` egy adott objektumcsoporthoz hello hét nap.

Beállítás hello proxy- és sávszélesség részletei történik hello segítségével [Set-OBMachineSetting](https://technet.microsoft.com/library/hh770409%28v=wps.630%29.aspx) parancsmagot:

```
PS C:\> Set-OBMachineSetting -NoProxy
Server properties updated successfully.

PS C:\> Set-OBMachineSetting -NoThrottle
Server properties updated successfully.
```

## <a name="encryption-settings"></a>Titkosítási beállítások
hello küldött biztonsági mentési adatok tooAzure biztonsági mentés titkosított tooprotect hello adatok bizalmas mivoltát hello. hello titkosítási jelszó visszaállítása a hello időpontjában az hello "password" toodecrypt hello adatai.

```
PS C:\> ConvertTo-SecureString -String "Complex!123_STRING" -AsPlainText -Force | Set-OBMachineSetting
Server properties updated successfully
```

> [!IMPORTANT]
> Biztosíthatja hello jelszót adatok biztonságos be van állítva. Csak akkor tudja toorestore adatokat az Azure-ból nélkül ezt a jelszót.
>
>

## <a name="back-up-files-and-folders"></a>Fájlok és mappák biztonsági mentése
Minden a biztonsági mentéseket Windows-kiszolgálók és ügyfelek tooAzure biztonsági mentési házirend vonatkozik. hello házirend három részből áll:

1. A **biztonsági mentés ütemezése** , amely megadja, amikor biztonsági mentést kell toobe venni, és szinkronizálva hello szolgáltatással.
2. A **adatmegőrzési ütemterv** , amely meghatározza, mennyi ideig tooretain hello helyreállítási pontok az Azure-ban.
3. A **belefoglalási/kizárási megadására** , amely határozzák meg, hogy mi készüljön biztonsági másolat.

Ebben a dokumentumban mivel azt még automatizálása a biztonsági mentés, feltételezzük, semmi nem konfiguráltak. Hozzon létre egy új biztonsági mentési házirendet hello megkezdjük [New-OBPolicy](https://technet.microsoft.com/library/hh770416.aspx) parancsmag és használja azt.

```
PS C:\> $newpolicy = New-OBPolicy
```

: Az idő hello házirend üres, és más parancsmagok olyan szükséges toodefine milyen elemek fog kell, illetve tiltani szeretné, ha biztonsági mentések futtatja, és ahol hello biztonsági mentések tárolódik.

### <a name="configuring-hello-backup-schedule"></a>Biztonsági mentés ütemezése hello konfigurálása
először hello hello házirend 3 részből az hello biztonsági mentési ütemezést, amely hello létre [New-OBSchedule](https://technet.microsoft.com/library/hh770401) parancsmag. hello biztonsági mentés ütemezése határozza meg, amikor biztonsági mentést kell venni toobe. Ütemezés létrehozásakor kell toospecify 2 bemeneti paraméterek:

* **Hello hét napjai** biztonsági mentését végző hello kell futtatnia. Hello biztonságimásolat-készítő feladat csak egy napon vagy hello hét minden napján, vagy tetszőleges kombinációját a közötti futtatása.
* **Hello napszakokban** mikor fusson a hello biztonsági mentés. Másolatot too3 különböző napszakokban hello amikor hello biztonsági mentés akkor is kiváltódik adhat meg.

Például konfigurálhatja a biztonsági mentési házirend du. 4: futtat minden szombat és vasárnap.

```
PS C:\> $sched = New-OBSchedule -DaysofWeek Saturday, Sunday -TimesofDay 16:00
```

hello biztonsági mentés ütemezése a házirendhez társított toobe van szüksége, és ez elérhető hello segítségével [Set-OBSchedule](https://technet.microsoft.com/library/hh770407) parancsmag.

```
PS C:> Set-OBSchedule -Policy $newpolicy -Schedule $sched
BackupSchedule : 4:00 PM Saturday, Sunday, Every 1 week(s) DsList : PolicyName : RetentionPolicy : State : New PolicyState : Valid
```
### <a name="configuring-a-retention-policy"></a>Egy megőrzési házirend konfigurálása
hello megőrzési házirend határozza meg, hogy mennyi ideig helyreállítási pont biztonsági mentési feladatok alapján létrehozott megmaradnak. Hello segítségével új megőrzési házirend létrehozásakor [New-OBRetentionPolicy](https://technet.microsoft.com/library/hh770425) parancsmaggal, megadhatja hello, hogy hány napig biztonsági mentések helyreállítási pontjait hello kell toobe megőrzi az Azure Backup szolgáltatással. az alábbi példa hello 7 napos adatmegőrzési állítja be.

```
PS C:\> $retentionpolicy = New-OBRetentionPolicy -RetentionDays 7
```

hello adatmegőrzési hozzá kell rendelni hello parancsmaggal hello fő irányelvnek [Set-OBRetentionPolicy](https://technet.microsoft.com/library/hh770405):

```
PS C:\> Set-OBRetentionPolicy -Policy $newpolicy -RetentionPolicy $retentionpolicy

BackupSchedule  : 4:00 PM
                  Saturday, Sunday,
                  Every 1 week(s)
DsList          :
PolicyName      :
RetentionPolicy : Retention Days : 7

                  WeeklyLTRSchedule :
                  Weekly schedule is not set

                  MonthlyLTRSchedule :
                  Monthly schedule is not set

                  YearlyLTRSchedule :
                  Yearly schedule is not set

State           : New
PolicyState     : Valid
```
### <a name="including-and-excluding-files-toobe-backed-up"></a>Többek között, és a biztonsági másolatba mentett fájlok toobe kivételével
Egy ```OBFileSpec``` objektum hello fájlok toobe és a biztonsági másolat nem határoz meg. Ez olyan szabályok, hogy a kimenő hello hatókör védett fájlok és mappák a gépen. Akkor is, számos befoglalási vagy kizárási szabály szükség szerint fájlt, és rendelje hozzá őket egy házirendet. Új OBFileSpec objektum létrehozásakor meg a következőket teheti:

* Adja meg a fájlok és mappák szereplő toobe hello
* Adja meg a hello kizárt fájlok és mappák toobe
* Adja meg a rekurzív az adatok biztonsági mentése egy mappában (vagy) e csak hello legfelső szintű mappában lévő fájlok hello megadott biztonsági másolatot kell fel.

Ez utóbbi hello hello - nem rekurzív jelzőt a New-OBFileSpec hello parancs használatával érhető el.

Hello az alábbi példában azt bemutatjuk C: és d. kötet mentésére, hello OS bináris hello Windows-mappában, és ideiglenes mappák kizárása. toodo, hozzon létre két fájl specifikációk hello segítségével [New-OBFileSpec](https://technet.microsoft.com/library/hh770408) parancsmag - befoglalási és kizárási egyet. Létrehozása után a hello fájl specifikációk, fontosságúak társított a hello szabályzatokkal hello [Add-OBFileSpec](https://technet.microsoft.com/library/hh770424) parancsmag.

```
PS C:\> $inclusions = New-OBFileSpec -FileSpec @("C:\", "D:\")

PS C:\> $exclusions = New-OBFileSpec -FileSpec @("C:\windows", "C:\temp") -Exclude

PS C:\> Add-OBFileSpec -Policy $newpolicy -FileSpec $inclusions

BackupSchedule  : 4:00 PM
                  Saturday, Sunday,
                  Every 1 week(s)
DsList          : {DataSource
                  DatasourceId:0
                  Name:C:\
                  FileSpec:FileSpec
                  FileSpec:C:\
                  IsExclude:False
                  IsRecursive:True

                  , DataSource
                  DatasourceId:0
                  Name:D:\
                  FileSpec:FileSpec
                  FileSpec:D:\
                  IsExclude:False
                  IsRecursive:True

                  }
PolicyName      :
RetentionPolicy : Retention Days : 7

                  WeeklyLTRSchedule :
                  Weekly schedule is not set

                  MonthlyLTRSchedule :
                  Monthly schedule is not set

                  YearlyLTRSchedule :
                  Yearly schedule is not set

State           : New
PolicyState     : Valid


PS C:\> Add-OBFileSpec -Policy $newpolicy -FileSpec $exclusions

BackupSchedule  : 4:00 PM
                  Saturday, Sunday,
                  Every 1 week(s)
DsList          : {DataSource
                  DatasourceId:0
                  Name:C:\
                  FileSpec:FileSpec
                  FileSpec:C:\
                  IsExclude:False
                  IsRecursive:True
                  ,FileSpec
                  FileSpec:C:\windows
                  IsExclude:True
                  IsRecursive:True
                  ,FileSpec
                  FileSpec:C:\temp
                  IsExclude:True
                  IsRecursive:True

                  , DataSource
                  DatasourceId:0
                  Name:D:\
                  FileSpec:FileSpec
                  FileSpec:D:\
                  IsExclude:False
                  IsRecursive:True

                  }
PolicyName      :
RetentionPolicy : Retention Days : 7

                  WeeklyLTRSchedule :
                  Weekly schedule is not set

                  MonthlyLTRSchedule :
                  Monthly schedule is not set

                  YearlyLTRSchedule :
                  Yearly schedule is not set

State           : New
PolicyState     : Valid
```

### <a name="applying-hello-policy"></a>Hello házirend alkalmazása
Hello csoportházirend-objektum most már befejeződött, és egy társított biztonsági mentés ütemezése, a megőrzési házirend és a fájlok belefoglalási/kizárási lista. Ez a házirend most hajtható végre, az Azure Backup toouse is. Az újonnan létrehozott hello alkalmazása előtt házirend gondoskodjon arról, hogy nincsenek-e hello segítségével hello kiszolgálóhoz tartozó már meglévő biztonsági mentési házirendek [Remove-OBPolicy](https://technet.microsoft.com/library/hh770415) parancsmag. Jóváhagyás eltávolítása hello házirend fogja kérni. tooskip hello megerősítő hello használata ```-Confirm:$false``` jelző hello parancsmaggal.

```
PS C:> Get-OBPolicy | Remove-OBPolicy
Microsoft Azure Backup Are you sure you want tooremove this backup policy? This will delete all hello backed up data. [Y] Yes [A] Yes tooAll [N] No [L] No tooAll [S] Suspend [?] Help (default is "Y"):
```

Végrehajtása hello csoportházirend-objektum történik hello segítségével [Set-OBPolicy](https://technet.microsoft.com/library/hh770421) parancsmag. Ez is megerősítést kér. tooskip hello megerősítő hello használata ```-Confirm:$false``` jelző hello parancsmaggal.

```
PS C:> Set-OBPolicy -Policy $newpolicy
Microsoft Azure Backup Do you want toosave this backup policy ? [Y] Yes [A] Yes tooAll [N] No [L] No tooAll [S] Suspend [?] Help (default is "Y"):
BackupSchedule : 4:00 PM Saturday, Sunday, Every 1 week(s)
DsList : {DataSource
         DatasourceId:4508156004108672185
         Name:C:\
         FileSpec:FileSpec
         FileSpec:C:\
         IsExclude:False
         IsRecursive:True,

         FileSpec
         FileSpec:C:\windows
         IsExclude:True
         IsRecursive:True,

         FileSpec
         FileSpec:C:\temp
         IsExclude:True
         IsRecursive:True,

         DataSource
         DatasourceId:4508156005178868542
         Name:D:\
         FileSpec:FileSpec
         FileSpec:D:\
         IsExclude:False
         IsRecursive:True
    }
PolicyName : c2eb6568-8a06-49f4-a20e-3019ae411bac
RetentionPolicy : Retention Days : 7
              WeeklyLTRSchedule :
              Weekly schedule is not set

              MonthlyLTRSchedule :
              Monthly schedule is not set

              YearlyLTRSchedule :
              Yearly schedule is not set
State : Existing PolicyState : Valid
```

Megtekintheti a meglévő biztonsági mentési házirend hello hello segítségével hello adatait [Get-OBPolicy](https://technet.microsoft.com/library/hh770406) parancsmag. Akkor is leásási hello használatával [Get-OBSchedule](https://technet.microsoft.com/library/hh770423) hello biztonsági mentési ütemezést és hello parancsmag [Get-OBRetentionPolicy](https://technet.microsoft.com/library/hh770427) hello adatmegőrzési parancsmaggal

```
PS C:> Get-OBPolicy | Get-OBSchedule
SchedulePolicyName : 71944081-9950-4f7e-841d-32f0a0a1359a
ScheduleRunDays : {Saturday, Sunday}
ScheduleRunTimes : {16:00:00}
State : Existing

PS C:> Get-OBPolicy | Get-OBRetentionPolicy
RetentionDays : 7
RetentionPolicyName : ca3574ec-8331-46fd-a605-c01743a5265e
State : Existing

PS C:> Get-OBPolicy | Get-OBFileSpec
FileName : *
FilePath : \?\Volume{b835d359-a1dd-11e2-be72-2016d8d89f0f}\
FileSpec : D:\
IsExclude : False
IsRecursive : True

FileName : *
FilePath : \?\Volume{cdd41007-a22f-11e2-be6c-806e6f6e6963}\
FileSpec : C:\
IsExclude : False
IsRecursive : True

FileName : *
FilePath : \?\Volume{cdd41007-a22f-11e2-be6c-806e6f6e6963}\windows
FileSpec : C:\windows
IsExclude : True
IsRecursive : True

FileName : *
FilePath : \?\Volume{cdd41007-a22f-11e2-be6c-806e6f6e6963}\temp
FileSpec : C:\temp
IsExclude : True
IsRecursive : True
```

### <a name="performing-an-ad-hoc-backup"></a>Az ad hoc biztonsági mentés
Miután a biztonsági mentési házirend van beállítva hello biztonsági mentések hello ütemezés / fog létrejönni. Az ad hoc biztonsági mentés indítására esetében is lehetséges hello segítségével [Start-OBBackup](https://technet.microsoft.com/library/hh770426) parancsmagot:

```
PS C:> Get-OBPolicy | Start-OBBackup
Taking snapshot of volumes...
Preparing storage...
Estimating size of backup items...
Estimating size of backup items...
Transferring data...
Verifying backup...
Job completed.
hello backup operation completed successfully.
```

## <a name="restore-data-from-azure-backup"></a>Állítsa vissza az adatokat az Azure Backup
Ez a szakasz végigvezeti Önt hello lépéseket az Azure biztonsági mentés az adatok helyreállítás automatizálása. Ezzel úgy magában foglalja a hello a következő lépéseket:

1. Hello forráskötet kiválasztása
2. Válassza ki a biztonsági mentési pont toorestore
3. Egy elem toorestore kiválasztása
4. Eseményindító hello visszaállítási folyamat

### <a name="picking-hello-source-volume"></a>Kiadási hello forráskötet
A sorrend toorestore egy elemet az Azure Backup először hello elem tooidentify hello forrását. Mivel azt még hello parancsok végrehajtása a Windows Server vagy egy Windows ügyfél hello környezetében, hello gép már azonosítja. hello következő lépése hello forrás azonosítása tooidentify hello kötetet tartalmazó. A kötetek adatforrásokat, illetve biztonsági másolatot készít a számítógépről kérhető hello végrehajtásával listáját [Get-OBRecoverableSource](https://technet.microsoft.com/library/hh770410) parancsmag. Ez a parancs minden hello adatforrások biztonsági mentése a kiszolgáló ügyfél tömbjét adja vissza.

```
PS C:> $source = Get-OBRecoverableSource
PS C:> $source
FriendlyName : C:\
RecoverySourceName : C:\
ServerName : myserver.microsoft.com

FriendlyName : D:\
RecoverySourceName : D:\
ServerName : myserver.microsoft.com
```

### <a name="choosing-a-backup-point-toorestore"></a>A biztonsági mentési pont toorestore kiválasztása
hello biztonsági mentési pontok listájának lekérhetők hello végrehajtása [Get-OBRecoverableItem](https://technet.microsoft.com/library/hh770399.aspx) parancsmag megfelelő paraméterekkel. A jelen példában mutatjuk be a legújabb biztonsági mentési pontok hello hello forráskötet *D:* és toorecover egy adott fájlt használja.

```
PS C:> $rps = Get-OBRecoverableItem -Source $source[1]
IsDir : False
ItemNameFriendly : D:\
ItemNameGuid : \?\Volume{b835d359-a1dd-11e2-be72-2016d8d89f0f}\
LocalMountPoint : D:\
MountPointName : D:\
Name : D:\
PointInTime : 18-Jun-15 6:41:52 AM
ServerName : myserver.microsoft.com
ItemSize :
ItemLastModifiedTime :

IsDir : False
ItemNameFriendly : D:\
ItemNameGuid : \?\Volume{b835d359-a1dd-11e2-be72-2016d8d89f0f}\
LocalMountPoint : D:\
MountPointName : D:\
Name : D:\
PointInTime : 17-Jun-15 6:31:31 AM
ServerName : myserver.microsoft.com
ItemSize :
ItemLastModifiedTime :
```
hello objektum ```$rps``` biztonsági mentési pontok tömbje. hello első eleme hello legutóbbi pontnak és hello n-edik elem hello legrégebbi pont. toochoose hello legújabb pont használjuk ```$rps[0]```.

### <a name="choosing-an-item-toorestore"></a>Egy elem toorestore kiválasztása
tooidentify hello pontosan a fájl vagy mappa toorestore, rekurzív módon használja a hello [Get-OBRecoverableItem](https://technet.microsoft.com/library/hh770399.aspx) parancsmag. Adott módon hello mappahierarchia kizárólag a hello segítségével tallózható ```Get-OBRecoverableItem```.

Ebben a példában, ha azt szeretné, hogy toorestore hello fájl *finances.xls* azt is hivatkozni lehessen, hogy használatával hello objektum ```$filesFolders[1]```.

```
PS C:> $filesFolders = Get-OBRecoverableItem $rps[0]
PS C:> $filesFolders
IsDir : True
ItemNameFriendly : D:\MyData\
ItemNameGuid : \?\Volume{b835d359-a1dd-11e2-be72-2016d8d89f0f}\MyData\
LocalMountPoint : D:\
MountPointName : D:\
Name : MyData
PointInTime : 18-Jun-15 6:41:52 AM
ServerName : myserver.microsoft.com
ItemSize :
ItemLastModifiedTime : 15-Jun-15 8:49:29 AM

PS C:> $filesFolders = Get-OBRecoverableItem $filesFolders[0]
PS C:> $filesFolders
IsDir : False
ItemNameFriendly : D:\MyData\screenshot.oxps
ItemNameGuid : \?\Volume{b835d359-a1dd-11e2-be72-2016d8d89f0f}\MyData\screenshot.oxps
LocalMountPoint : D:\
MountPointName : D:\
Name : screenshot.oxps
PointInTime : 18-Jun-15 6:41:52 AM
ServerName : myserver.microsoft.com
ItemSize : 228313
ItemLastModifiedTime : 21-Jun-14 6:45:09 AM

IsDir : False
ItemNameFriendly : D:\MyData\finances.xls
ItemNameGuid : \?\Volume{b835d359-a1dd-11e2-be72-2016d8d89f0f}\MyData\finances.xls
LocalMountPoint : D:\
MountPointName : D:\
Name : finances.xls
PointInTime : 18-Jun-15 6:41:52 AM
ServerName : myserver.microsoft.com
ItemSize : 96256
ItemLastModifiedTime : 21-Jun-14 6:43:02 AM
```

Elemek toorestore hello használatával is kereshet ```Get-OBRecoverableItem``` parancsmag. A példánkban a toosearch *finances.xls* azt lehetett beolvasni a leíró hello fájl a következő parancs futtatásával:

```
PS C:\> $item = Get-OBRecoverableItem -RecoveryPoint $rps[0] -Location "D:\MyData" -SearchString "finance*"
```

### <a name="triggering-hello-restore-process"></a>Eseményindító hello visszaállítási folyamat
tootrigger hello visszaállítási folyamat, először kell toospecify hello helyreállítási beállítások. Ezt megteheti a hello [New-OBRecoveryOption](https://technet.microsoft.com/library/hh770417.aspx) parancsmag. Ebben a példában tételezzük fel, hogy toorestore hello fájlok túl szeretnénk*C:\temp*. Tegyük is fel, hogy szeretnénk tooskip már megtalálható fájlok hello rendeltetési mappára *C:\temp*. toocreate ilyen egy helyreállítási lehetőség, a következő parancs hello használata:

```
PS C:\> $recovery_option = New-OBRecoveryOption -DestinationPath "C:\temp" -OverwriteType Skip
```

Most hello segítségével a visszaállítás elindítása [Start-OBRecovery](https://technet.microsoft.com/library/hh770402.aspx) kijelölt hello parancs ```$item``` hello hello kimenetét a ```Get-OBRecoverableItem``` parancsmagot:

```
PS C:\> Start-OBRecovery -RecoverableItem $item -RecoveryOption $recover_option
Estimating size of backup items...
Estimating size of backup items...
Estimating size of backup items...
Estimating size of backup items...
Job completed.
hello recovery operation completed successfully.
```


## <a name="uninstalling-hello-azure-backup-agent"></a>Hello Azure Backup szolgáltatás ügynökének eltávolítása
Eltávolítását hello Azure Backup szolgáltatás ügynökének hello a következő parancs használatával teheti meg:

```
PS C:\> .\MARSAgentInstaller.exe /d /q
```

Néhány következmények tooconsider hello ügynök bináris fájlok eltávolítása a hello gépről rendelkezik:

* Eltávolítja a hello fájlszűrő hello gépről, és az változások követését le van állítva.
* Hello gépet eltávolítja az összes házirend az adatokat, de hello házirenddel kapcsolatos információk továbbra is toobe hello szolgáltatásban tárolja.
* A mentési ütemezések törlődnek, és nincs további biztonsági mentés készül.

Azonban hello Azure marad a adataihoz, és megőrzi hello megőrzési házirend beállítása adott meg. Régebbi pontok automatikusan van elavult.

## <a name="remote-management"></a>Távfelügyelet
Minden hello felügyeleti hello Azure Backup szolgáltatás ügynöke, a házirendek és az adatforrások távolról végezhető el a PowerShell. távolról felügyelt hello gép kell toobe megfelelően előkészítve.

Alapértelmezés szerint kézi indítási hello WinRM szolgáltatás van konfigurálva. hello indítási típusa túl be kell állítani*automatikus* és hello szolgáltatást el kell indítani. tooverify, amely hello a WinRM szolgáltatás fut, hello hello állapot tulajdonság értékének meg kell *futtató*.

```
PS C:\> Get-Service WinRM

Status   Name               DisplayName
------   ----               -----------
Running  winrm              Windows Remote Management (WS-Manag...
```

PowerShell távoli eljáráshívás kell konfigurálni.

```
PS C:\> Enable-PSRemoting -force
WinRM is already set up tooreceive requests on this computer.
WinRM has been updated for remote management.
WinRM firewall exception enabled.

PS C:\> Set-ExecutionPolicy unrestricted -force
```

hello gép most távolról felügyelhetők - hello ügynök telepítése hello kezdve. Például a következő parancsfájl hello hello ügynök toohello távoli számítógépre másolja, és telepíti azt.

```
PS C:\> $dloc = "\\REMOTESERVER01\c$\Windows\Temp"
PS C:\> $agent = "\\REMOTESERVER01\c$\Windows\Temp\MARSAgentInstaller.exe"
PS C:\> $args = "/q"
PS C:\> Copy-Item "C:\Downloads\MARSAgentInstaller.exe" -Destination $dloc - force

PS C:\> $s = New-PSSession -ComputerName REMOTESERVER01
PS C:\> Invoke-Command -Session $s -Script { param($d, $a) Start-Process -FilePath $d $a -Wait } -ArgumentList $agent $args
```

## <a name="next-steps"></a>Következő lépések
További információ az Azure biztonsági mentés a Windows Server vagy Windows-ügyfélen lásd:

* [Bevezetés tooAzure biztonsági mentése](backup-introduction-to-azure-backup.md)
* [Windows-kiszolgálók biztonsági mentését](backup-configure-vault.md)
