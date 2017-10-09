---
title: "Az Azure Backup: Használja a Powershellt tooback mentése a DPM-munkaterhelések |} Microsoft Docs"
description: "Megtudhatja, hogyan toodeploy és kezelése az Azure Backup a Data Protection Manager (DPM) PowerShell használatával"
services: backup
documentationcenter: 
author: Nkolli1
manager: shreeshd
editor: 
ms.assetid: bcbcef79-9d33-4e84-a558-9866614f2cae
ms.service: backup
ms.workload: storage-backup-recovery
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/02/2017
ms.author: nkolli;trinadhk;anuragm;markgal
ms.openlocfilehash: 48ebe6b520857836e89749ffb6fe83d1f14c5597
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="deploy-and-manage-backup-tooazure-for-data-protection-manager-dpm-servers-using-powershell"></a>Központi telepítése és kezelése a PowerShell használatával a Data Protection Manager (DPM) kiszolgálók biztonsági mentési tooAzure
> [!div class="op_single_selector"]
> * [ARM](backup-dpm-automation.md)
> * [Klasszikus](backup-dpm-automation-classic.md)
>
>

Ez a cikk azt ismerteti, hogyan toouse PowerShell tooback össze, és a DPM-adatok helyreállítása a biztonsági mentési tárolóból. A Microsoft azt javasolja, hogy az összes új központi telepítéseknél Recovery Services-tárolók használatával. Ha egy új Azure Backup-felhasználó, használja a hello a cikkben [központi telepítése és kezelése a PowerShell használatával a Data Protection Manager-adatok tooAzure](backup-dpm-automation.md), így az adatok tárolása a Recovery Services-tároló.

> [!IMPORTANT]
> Most már frissítheti a mentési tárolók tooRecovery szolgáltatások tárolókban. További információkért lásd: hello cikk [frissíteni a biztonsági mentési tároló tooa Recovery Services-tároló](backup-azure-upgrade-backup-to-recovery-services.md). A Microsoft tooupgrade támogatja a mentési tárolókat tooRecovery szolgáltatások tárolók. 2017. október 15. után PowerShell toocreate mentési tárolókban nem használható. **2017. november 1-től**:
>- Az összes többi biztonsági mentési tárolók lesz automatikusan frissített tooRecovery szolgáltatások tárolók.
>- Ön nem fogja tudni tooaccess a biztonsági mentési adatok hello a klasszikus portálon. Ehelyett használjon hello Azure portál tooaccess a biztonsági mentési adatok a Recovery Services-tárolók.
>

## <a name="setting-up-hello-powershell-environment"></a>Hello PowerShell környezet létrehozása
[!INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-include.md)]

PowerShell toomanage készített biztonsági másolat a Data Protection Manager tooAzure használata előtt kell toohave hello megfelelő környezet a PowerShellben. Elején hello hello PowerShell-munkamenetet győződjön meg arról, hogy futtassa a következő parancs tooimport hello jobb modulok hello, és lehetővé teszik toocorrectly hivatkozás hello DPM-parancsmagokat:

```
PS C:> & "C:\Program Files\Microsoft System Center 2012 R2\DPM\DPM\bin\DpmCliInitScript.ps1"

Welcome toohello DPM Management Shell!

Full list of cmdlets: Get-Command
Only DPM cmdlets: Get-DPMCommand
Get general help: help
Get help for a cmdlet: help <cmdlet-name> or <cmdlet-name> -?
Get definition of a cmdlet: Get-Command <cmdlet-name> -Syntax
Sample DPM scripts: Get-DPMSampleScript
```

## <a name="setup-and-registration"></a>Telepítését és regisztrálását
toobegin:

1. [Töltse le a legfrissebb PowerShell](https://github.com/Azure/azure-powershell/releases) (szükséges minimális verziója: 1.0.0)
2. Engedélyezze a hello Azure Backup szolgáltatás parancsmagjaival váltás túl*AzureResourceManager* módból hello **Switch-AzureMode** parancsmagot:

```
PS C:\> Switch-AzureMode AzureResourceManager
```

hello a következő és nyilvántartási feladatok automatizálhatók a PowerShell használatával:

* Backup-tároló létrehozása
* Hello Azure Backup szolgáltatás ügynökének telepítése
* Hello Azure Backup szolgáltatás regisztrálása
* Hálózati beállítások
* Titkosítási beállítások

### <a name="create-a-backup-vault"></a>Backup-tároló létrehozása
> [!WARNING]
> A felhasználó Azure Backup segítségével hello az első alkalommal esetén tooregister hello Azure biztonsági mentés szolgáltató toobe használja az előfizetéskor kell. Ezt megteheti hello a következő parancs futtatásával: Register-AzureProvider - ProviderNamespace "Microsoft.Backup"
>
>

Létrehozhat egy új mentési tárolót használó hello **New-AzureRMBackupVault** parancsmagot. hello mentési tároló egy ARM-erőforrás, ezért meg kell tooplace az erőforráscsoporton belül. Egy rendszergazda jogú Azure PowerShell-konzolban futtassa a következő parancsok hello:

```
PS C:\> New-AzureResourceGroup –Name “test-rg” -Region “West US”
PS C:\> $backupvault = New-AzureRMBackupVault –ResourceGroupName “test-rg” –Name “test-vault” –Region “West US” –Storage GRS
```

Minden hello mentési tárolók listája egy adott feliratkozás hello olvashatók be **Get-AzureRMBackupVault** parancsmagot.

### <a name="installing-hello-azure-backup-agent-on-a-dpm-server"></a>Hello Azure Backup szolgáltatás ügynökének telepítése a DPM-kiszolgálón
Hello Azure Backup szolgáltatás ügynökének telepítése előtt kell toohave hello telepítő letöltött és a Windows Server hello megtalálható. Hello installer legújabb verzióját hello letölthető hello [Microsoft Download Center](http://aka.ms/azurebackup_agent) vagy hello mentési tároló irányítópult-oldalon. Mentés hello telepítő tooan könnyen hozzáférhető helyen, például * C:\Downloads\*.

tooinstall hello ügynök, futtassa a következő parancsot egy rendszergazda jogú PowerShell-konzolban hello **hello DPM-kiszolgálón**:

```
PS C:\> MARSAgentInstaller.exe /q
```

Ezzel telepít hello ügynök minden hello alapértelmezett beállításokkal. hello telepítési hello háttérben néhány percet vesz igénybe. Ha nem adja meg a hello */nu* beállítás hello **Windows Update** ablak nyílik meg hello telepítési toocheck bármely frissítések hello végén.

hello ügynök telepített programok listájában hello fogja megjeleníteni. a telepített programok toosee hello listája, nyissa meg túl**Vezérlőpult** > **programok** > **programok és szolgáltatások**.

![Az ügynök telepítve](./media/backup-dpm-automation/installed-agent-listing.png)

#### <a name="installation-options"></a>Telepítési beállítások
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

### <a name="registering-with-hello-azure-backup-service"></a>Hello Azure Backup szolgáltatás regisztrálása
Az Azure Backup szolgáltatás hello regisztrálhatja, jóvá kell tooensure adott hello [Előfeltételek](backup-azure-dpm-introduction.md) teljesülnek. A következők szükségesek:

* Egy érvényes Azure-előfizetése
* Rendelkezik egy biztonsági mentési tárolóval

toodownload hello tárolói hitelesítő adatokat, futtassa a hello **Get-AzureBackupVaultCredentials** parancsmag egy Azure PowerShell-konzolban és a tároló azt egy tetszőleges helyen, például a * C:\Downloads\*.

```
PS C:\> $credspath = "C:\"
PS C:\> $credsfilename = Get-AzureRMBackupVaultCredentials -Vault $backupvault -TargetLocation $credspath
PS C:\> $credsfilename
f5303a0b-fae4-4cdb-b44d-0e4c032dde26_backuprg_backuprn_2015-08-11--06-22-35.VaultCredentials
```

Regisztrálásakor hello gép hello tárolóban történik hello segítségével [Start-DPMCloudRegistration](https://technet.microsoft.com/library/jj612787) parancsmagot:

```
PS C:\> $cred = $credspath + $credsfilename
PS C:\> Start-DPMCloudRegistration -DPMServerName "TestingServer" -VaultCredentialsFilePath $cred
```

Regisztrálja a hello "Ezután" nevű DPM-kiszolgáló Microsoft Azure-tárolóban megadott hello használata az tároló hitelesítő adatait.

> [!IMPORTANT]
> Ne használjon relatív elérési utak toospecify hello tároló hitelesítési adatait tartalmazó fájlt. Egy bemeneti toohello parancsmagot, abszolút elérési utat kell megadnia.
>
>

### <a name="initial-configuration-settings"></a>Kezdeti konfigurációs beállításai
Miután hello DPM-kiszolgáló regisztrálva hello Azure mentési tárolóval, alapértelmezett előfizetés beállításokkal fog elindulni. Ezek előfizetési beállítások közé tartozik a hálózatkezelés, a titkosítás és a hello az átmeneti területről. toobegin hello előfizetési beállítások módosításának toofirst kell leírót szerezni hello meglévő (alapértelmezett) a beállításokat a hello [Get-DPMCloudSubscriptionSetting](https://technet.microsoft.com/library/jj612793) parancsmagot:

```
$setting = Get-DPMCloudSubscriptionSetting -DPMServerName "TestingServer"
```

Minden módosítások készült toothis helyi PowerShell objektum ```$setting``` , és ezután hello teljes objektum véglegesített tooDPM és az Azure Backup toosave őket hello segítségével [Set-DPMCloudSubscriptionSetting](https://technet.microsoft.com/library/jj612791) parancsmag. Toouse hello kell ```–Commit``` jelző tooensure, amely hello módosítások megmaradnak. hello-beállítások nem is alkalmazott és Azure Backup szolgáltatás csak a véglegesített használni.

```
PS C:\> Set-DPMCloudSubscriptionSetting -DPMServerName "TestingServer" -SubscriptionSetting $setting -Commit
```

### <a name="networking"></a>Hálózat
Ha hello DPM gép toohello hello Azure biztonsági mentési szolgáltatás hello kapcsolatát internet proxykiszolgálón keresztül, majd hello proxykiszolgálót kell megadni a biztonsági mentések toosucceed. Hello használata ehhez ```-ProxyServer```, ```-ProxyPort```, ```-ProxyUsername``` és hello ```ProxyPassword``` hello paramétereket [Set-DPMCloudSubscriptionSetting](https://technet.microsoft.com/library/jj612791) parancsmag. Ebben a példában nincs proxy kiszolgáló, explicit módon azt vannak törlésével bármely proxy kapcsolatos információkat.

```
PS C:\> Set-DPMCloudSubscriptionSetting -DPMServerName "TestingServer" -SubscriptionSetting $setting -NoProxy
```

Sávszélesség is szabályozható a beállítások a ```-WorkHourBandwidth``` és ```-NonWorkHourBandwidth``` egy adott objektumcsoporthoz hello hét nap. Ebben a példában a Microsoft nem állítja a sávszélesség-szabályozás.

```
PS C:\> Set-DPMCloudSubscriptionSetting -DPMServerName "TestingServer" -SubscriptionSetting $setting -NoThrottle
```

### <a name="configuring-hello-staging-area"></a>Az átmeneti területről hello konfigurálása
hello DPM-kiszolgálón futó hello Azure Backup ügynöknek szüksége van ideiglenes tárhelyre, (helyi átmeneti terület) hello felhőből helyreállított adatok számára. Hello átmeneti terület használatával hello konfigurálása [Set-DPMCloudSubscriptionSetting](https://technet.microsoft.com/library/jj612791) parancsmag és hello ```-StagingAreaPath``` paraméter.

```
PS C:\> Set-DPMCloudSubscriptionSetting -DPMServerName "TestingServer" -SubscriptionSetting $setting -StagingAreaPath "C:\StagingArea"
```

Hello a fenti példában a hello átmeneti terület lesz beállítva, túl*C:\StagingArea* hello PowerShell objektumban ```$setting```. Győződjön meg arról, hogy hello megadott mappa már létezik, vagy pedig hello előfizetési beállítások hello végső véglegesítése sikertelen lesz.

### <a name="encryption-settings"></a>Titkosítási beállítások
hello küldött biztonsági mentési adatok tooAzure biztonsági mentés titkosított tooprotect hello adatok bizalmas mivoltát hello. hello titkosítási jelszó visszaállítása a hello időpontjában az hello "password" toodecrypt hello adatai. Fontos tookeep ezen információk biztonságos és biztonságos be van állítva.

Hello az alábbi példában az első parancs hello alakít át hello ```passphrase123456789``` tooa biztonságos karakterlánc és rendel hello biztonságos karakterlánc toohello nevű változó ```$Passphrase```. hello második parancs beállítja hello biztonságos karakterlánc ```$Passphrase``` hello jelszóként biztonsági mentések titkosításához.

```
PS C:\> $Passphrase = ConvertTo-SecureString -string "passphrase123456789" -AsPlainText -Force

PS C:\> Set-DPMCloudSubscriptionSetting -DPMServerName "TestingServer" -SubscriptionSetting $setting -EncryptionPassphrase $Passphrase
```

> [!IMPORTANT]
> Biztosíthatja hello jelszót adatok biztonságos be van állítva. Csak akkor tudja toorestore adatokat az Azure-ból nélkül ezt a jelszót.
>
>

Ezen a ponton az összes szükséges hello módosítások toohello el kell ```$setting``` objektum. Ne felejtse el toocommit hello módosításokat.

```
PS C:\> Set-DPMCloudSubscriptionSetting -DPMServerName "TestingServer" -SubscriptionSetting $setting -Commit
```

## <a name="protect-data-tooazure-backup"></a>Biztonsági mentési adatok tooAzure védelme
Ebben a szakaszban egy üzemi kiszolgáló tooDPM hozzáadása, és majd védelme a DPM toolocal hello adattárolás és tooAzure biztonsági mentés. Hello példákban fogjuk mutatni, hogyan tooback fájlokat és mappákat. hello logikát is egyszerű kiterjesztett toobackup minden DPM által támogatott adatforrás használni. A DPM biztonsági mentések által a védelmi csoport (PG) négy alkotórészek vonatkoznak:

1. **Csoport tagjai** összes hello védhető objektum listája (más néven *adatforrások* a DPM-ben), amelyet a hello tooprotect ugyanahhoz a védelmi csoporthoz. Például érdemes lehet tooprotect üzemi virtuális gépek egy védelmi csoport és egy másik védelmi csoportban található SQL Server-adatbázisok különböző biztonsági követelményeket is rendelkeznek. Biztonsági másolatot készíthet a datasource egy üzemi kiszolgálón meg kell, hogy hello toomake a DPM-ügynök hello kiszolgálóra van telepítve, és a DPM által kezelt. Hello utasításai [a DPM-ügynök telepítése hello](https://technet.microsoft.com/library/bb870935.aspx) és toohello kapcsolásával végezze el a megfelelő DPM-kiszolgálón.
2. **Adatvédelmi módszer** hello biztonsági mentési célhelyek - szalag, a lemez és a felhő határozza meg. Ebben a példában a Microsoft által védendő adatok toohello helyi lemez- és toohello-alapú.
3. A **biztonsági mentés ütemezése** , amely megadja, hogy ha a biztonsági mentések kell venni toobe, és milyen gyakran hello szinkronizálja az adatokat a DPM-kiszolgáló hello és hello az üzemi kiszolgáló között.
4. A **adatmegőrzési ütemterv** , amely meghatározza, mennyi ideig tooretain hello helyreállítási pontok az Azure-ban.

### <a name="creating-a-protection-group"></a>Védelmi csoport létrehozásakor
Először hozzon létre egy új védelmi csoportot a hello [New-DPMProtectionGroup](https://technet.microsoft.com/library/hh881722) parancsmag.

```
PS C:\> $PG = New-DPMProtectionGroup -DPMServerName " TestingServer " -Name "ProtectGroup01"
```

hello fent parancsmag létrehoz egy védelmi csoport neve *ProtectGroup01*. Egy meglévő védelmi csoport is módosíthatja újabb tooadd biztonsági mentési toohello Azure felhőben. Azonban toomake módosításokat toohello új védelmi csoport - vagy a meglévő - igazolnia kell a tooget leírót egy *módosíthatóvá* hello használó [Get-DPMModifiableProtectionGroup](https://technet.microsoft.com/library/hh881713) parancsmag.

```
PS C:\> $MPG = Get-ModifiableProtectionGroup $PG
```

### <a name="adding-group-members-toohello-protection-group"></a>Csoport tagjainak toohello védelmi csoport hozzáadása
Minden DPM-ügynök tudja hello listája adatforrások hello kiszolgálón, amelyre telepítve van. tooadd egy adatforrás toohello védelmi csoport, a DPM-ügynök igények toofirst hello hello adatforrások hátsó toohello DPM-kiszolgáló listájának küldéséhez. Egy vagy több adatforrás nem, akkor a kiválasztott és toohello védelmi csoporthoz hozzáadott. hello PowerShell szükséges lépéseket tooget elérése ennek a következők:

1. Keresztül hello DPM-ügynököt a DPM által kezelt összes kiszolgálók listájának beolvasása.
2. Válasszon egy adott kiszolgálóra.
3. Hello kiszolgálón az összes adatforrás listájának beolvasása.
4. Válasszon egy vagy több adatforrást, és vegye fel őket a védelmi csoport toohello

mely hello a DPM-ügynök telepítve van, és hello DPM-kiszolgáló által kezelt kiszolgálók listájában hello hello van megszerezve [Get-DPMProductionServer](https://technet.microsoft.com/library/hh881600) parancsmag. Ebben a példában azt szűrésére és konfigurálását elvégzi, csak PS nevű *productionserver01* a biztonsági mentéshez.

```
PS C:\> $server = Get-ProductionServer -DPMServerName "TestingServer" | where {($_.servername) –contains “productionserver01”
```

Most beolvasni adatforrások listája hello ```$server``` hello segítségével [Get-DPMDatasource](https://technet.microsoft.com/library/hh881605) parancsmag. Ebben a példában a Microsoft jelenleg korlátozza a hello kötet * D:\* azt szeretnénk, amely a biztonsági mentéshez tooconfigure. Ez az adatforrás kerül toohello védelmi csoport használatával hello [Add-DPMChildDatasource](https://technet.microsoft.com/library/hh881732) parancsmag. Ne feledje toouse hello *modifable* védelmi csoport objektum ```$MPG``` toomake hello kiegészítéseit.

```
PS C:\> $DS = Get-Datasource -ProductionServer $server -Inquire | where { $_.Name -contains “D:\” }

PS C:\> Add-DPMChildDatasource -ProtectionGroup $MPG -ChildDatasource $DS
```

Ismételje meg ezt a lépést, ha szükséges, ahányszor, amíg a kiválasztott adatforrások toohello védelmi csoport összes hello hozzáadását. Is csak egy adatforrást, és teljes hello munkafolyamat hello védelmi csoport létrehozásához kezdődnie, és később adja hozzá a további adatforrások toohello védelmi csoportot.

### <a name="selecting-hello-data-protection-method"></a>Hello adatvédelmi módszer kiválasztása
Hello adatforrások toohello védelmi csoporthoz lettek hozzáadva, miután hello következő lépésre-e toospecify hello védelmi módszer használatával hello [Set-DPMProtectionType](https://technet.microsoft.com/library/hh881725) parancsmag. Ebben a példában hello védelmi csoport lesz a telepítőt a helyi lemezek és felhőbeli biztonsági mentését. Emellett szükség van, amelyet az hello segítségével tooprotect toocloud toospecify hello datasource [Add-DPMChildDatasource](https://technet.microsoft.com/library/hh881732.aspx) jelölővel - Online parancsmag.

```
PS C:\> Set-DPMProtectionType -ProtectionGroup $MPG -ShortTerm Disk –LongTerm Online
PS C:\> Add-DPMChildDatasource -ProtectionGroup $MPG -ChildDatasource $DS –Online
```

### <a name="setting-hello-retention-range"></a>Hello megőrzési tartomány beállítása
Hello megőrzési hello biztonsági mentési pontok használatával hello beállítása [Set-DPMPolicyObjective](https://technet.microsoft.com/library/hh881762) parancsmag. Amíg páratlan tooset hello megőrzési tűnhet, mielőtt hello biztonsági mentési ütemezés definiálása hello segítségével ```Set-DPMPolicyObjective``` parancsmag automatikusan úgy állítja be a alapértelmezett biztonsági mentés ütemezését, majd módosítható. Mindig lehetséges tooset hello biztonsági mentés ütemezése először és hello adatmegőrzési után.

Hello az alábbi példában a hello parancsmag hello megőrzési paramétereinek beállítása lemezes biztonsági mentések. Ez megőrzi biztonsági mentések 10 nap, és szinkronizálja az adatokat az üzemi kiszolgáló hello és hello DPM-kiszolgáló közötti 6 óránként. Hello ```SynchronizationFrequencyMinutes``` nem adja meg, milyen gyakran egy biztonsági mentési pont jön létre, de milyen gyakran adatok DPM-kiszolgálóra másolt toohello; ezzel megakadályozza, hogy a biztonsági mentések túl nagy.

```
PS C:\> Set-DPMPolicyObjective –ProtectionGroup $MPG -RetentionRangeInDays 10 -SynchronizationFrequencyMinutes 360
```

A biztonsági mentésekhez tooAzure (DPM hivatkozik toothese az Online biztonsági mentés másként) fog hello megőrzési időtartamokat konfigurálhatja a [hosszú távon szerzett-Édesapja-fia megjelenítve (GFS) megőrzési](backup-azure-backup-cloud-as-tape.md). Ez azt jelenti, hogy napi, heti, havi és éves adatmegőrzési szabályoknál érintő kombinált adatmegőrzési adhat meg. Ebben a példában azt szeretnénk hello összetett megőrzési séma képviselő tömböt létrehozása, és adja meg hello megőrzési tartományt hello [Set-DPMPolicyObjective](https://technet.microsoft.com/library/hh881762) parancsmag.

```
PS C:\> $RRlist = @()
PS C:\> $RRList += (New-Object -TypeName Microsoft.Internal.EnterpriseStorage.Dls.UI.ObjectModel.OMCommon.RetentionRange -ArgumentList 180, Days)
PS C:\> $RRList += (New-Object -TypeName Microsoft.Internal.EnterpriseStorage.Dls.UI.ObjectModel.OMCommon.RetentionRange -ArgumentList 104, Weeks)
PS C:\> $RRList += (New-Object -TypeName Microsoft.Internal.EnterpriseStorage.Dls.UI.ObjectModel.OMCommon.RetentionRange -ArgumentList 60, Month)
PS C:\> $RRList += (New-Object -TypeName Microsoft.Internal.EnterpriseStorage.Dls.UI.ObjectModel.OMCommon.RetentionRange -ArgumentList 10, Years)
PS C:\> Set-DPMPolicyObjective –ProtectionGroup $MPG -OnlineRetentionRangeList $RRlist
```

### <a name="set-hello-backup-schedule"></a>Hello biztonsági mentési ütemezés szerint
A DPM automatikusan beállítja, egy alapértelmezett biztonsági mentés ütemezése hello védelmi célok hello segítségével noconnection ```Set-DPMPolicyObjective``` parancsmag. toochange hello alapértelmezett ütemtervét, használja a hello [Get-DPMPolicySchedule](https://technet.microsoft.com/library/hh881749) parancsmag követ hello [Set-DPMPolicySchedule](https://technet.microsoft.com/library/hh881723) parancsmag.

```
PS C:\> $onlineSch = Get-DPMPolicySchedule -ProtectionGroup $mpg -LongTerm Online
PS C:\> Set-DPMPolicySchedule -ProtectionGroup $MPG -Schedule $onlineSch[0] -TimesOfDay 02:00
PS C:\> Set-DPMPolicySchedule -ProtectionGroup $MPG -Schedule $onlineSch[1] -TimesOfDay 02:00 -DaysOfWeek Sa,Su –Interval 1
PS C:\> Set-DPMPolicySchedule -ProtectionGroup $MPG -Schedule $onlineSch[2] -TimesOfDay 02:00 -RelativeIntervals First,Third –DaysOfWeek Sa
PS C:\> Set-DPMPolicySchedule -ProtectionGroup $MPG -Schedule $onlineSch[3] -TimesOfDay 02:00 -DaysOfMonth 2,5,8,9 -Months Jan,Jul
PS C:\> Set-DPMProtectionGroup -ProtectionGroup $MPG
```

A fenti, hello példa ```$onlineSch``` tömb négy elemekkel, amely tartalmazza a meglévő online védelmi ütemterv hello hello védelmi csoport hello GFS rendszerben:

1. ```$onlineSch[0]```napi ütemezés hello fogja tartalmazni.
2. ```$onlineSch[1]```hello heti ütemezés fogja tartalmazni.
3. ```$onlineSch[2]```hello havi ütemezés fogja tartalmazni.
4. ```$onlineSch[3]```hello éves ütemezés fogja tartalmazni.

Így ha toomodify hello heti ütemezés van szüksége, toorefer toohello ```$onlineSch[1]```.

### <a name="initial-backup"></a>Kezdeti biztonsági mentés
Amikor első alkalommal biztonsági mentését a hello datasource, DPM-nek egy kezdeti replika, amelyet létre fog hozni hello datasource toobe védett másolatát a DPM-replikakötet toocreate. Ez a tevékenység vagy egy adott időpont ütemezhetők, vagy manuálisan is elindítható, hello segítségével [Set-DPMReplicaCreationMethod](https://technet.microsoft.com/library/hh881715) hello paraméterrel parancsmag ```-NOW```.

```
PS C:\> Set-DPMReplicaCreationMethod -ProtectionGroup $MPG -NOW
```
### <a name="changing-hello-size-of-dpm-replica--recovery-point-volume"></a>DPM-replika & a helyreállításipont-kötet hello méretének módosítása
Hello méretét a DPM-replikakötet, valamint árnyékmásolat-kötet használatával is módosíthatja [Set-DPMDatasourceDiskAllocation](https://technet.microsoft.com/library/hh881618.aspx) parancsmag, ahogy az alábbi példában hello: Get-DatasourceDiskAllocation - Datasource $DS Set-datasourcediskallocation segédprogram - Datasource $DS - ProtectionGroup $MPG-manuális - ReplicaArea (2 gb) – ShadowCopyArea (2 gb)

### <a name="committing-hello-changes-toohello-protection-group"></a>Hello véglegesítése toohello védelmi csoport módosítása
Végezetül hello módosításokat kell a DPM hello biztonsági mentés / hello új védelmi csoport konfigurációs megkezdése előtt véglegesítése toobe. Ebben az esetben hello segítségével [Set-DPMProtectionGroup](https://technet.microsoft.com/library/hh881758) parancsmag.

```
PS C:\> Set-DPMProtectionGroup -ProtectionGroup $MPG
```
## <a name="view-hello-backup-points"></a>Nézet hello biztonsági mentési pontok
Használhatja a hello [Get-DPMRecoveryPoint](https://technet.microsoft.com/library/hh881746) parancsmag tooget egy adatforrás összes helyreállítási pont listáját. Ebben a példában a következő történik:

* DPM-kiszolgálón hello tömbben tárolni összes hello PGs beolvasása```$PG```
* hello adatforrások megfelelő toohello beolvasása```$PG[0]```
* egy adatforrás összes hello helyreállítási pont beolvasása.

```
PS C:\> $PG = Get-DPMProtectionGroup –DPMServerName "TestingServer"
PS C:\> $DS = Get-DPMDatasource -ProtectionGroup $PG[0]
PS C:\> $RecoveryPoints = Get-DPMRecoverypoint -Datasource $DS[0] -Online
```

## <a name="restore-data-protected-on-azure"></a>Az Azure-on védett adatok visszaállítása
Adat-visszaállítást a rendszer kombinációja egy ```RecoverableItem``` objektum és a ```RecoveryOption``` objektum. Hello előző szakaszban egy adatforrás azt kapott hello biztonsági mentési pontok listáját.

A hello az alábbi példában bemutatjuk, hogyan toorestore egy Hyper-V virtuális gép az Azure Backup szolgáltatás biztonsági mentési pontok egyesítő hello helyreállítási célként. Ehhez a következőket:

* A helyreállítási lehetőséget a hello segítségével létrehozása [New-DPMRecoveryOption](https://technet.microsoft.com/library/hh881592) parancsmag.
* Hello segítségével biztonsági mentési pontok tömbjének lekérdezésekor hello ```Get-DPMRecoveryPoint``` parancsmag.
* A biztonsági mentési pont toorestore a kiválasztásához.

```
PS C:\> $RecoveryOption = New-DPMRecoveryOption -HyperVDatasource -TargetServer "HVDCenter02" -RecoveryLocation AlternateHyperVServer -RecoveryType Recover -TargetLocation “C:\VMRecovery”

PS C:\> $PG = Get-DPMProtectionGroup –DPMServerName "TestingServer"
PS C:\> $DS = Get-DPMDatasource -ProtectionGroup $PG[0]
PS C:\> $RecoveryPoints = Get-DPMRecoverypoint -Datasource $DS[0] -Online

PS C:\> Restore-DPMRecoverableItem -RecoverableItem $RecoveryPoints[0] -RecoveryOption $RecoveryOption
```

hello parancsok könnyen bővíthető bármely adatforrástípusnál.

## <a name="next-steps"></a>Következő lépések
* További információ a DPM Azure biztonsági mentés: [bemutatása tooDPM biztonsági mentése](backup-azure-dpm-introduction.md)
