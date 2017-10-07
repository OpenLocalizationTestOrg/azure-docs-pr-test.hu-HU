---
title: "aaaDeploy hello Site Recovery mobilitási szolgáltatás az Azure Automation DSC Szolgáltatásban |} Microsoft Docs"
description: "Ismerteti, hogyan toouse Azure Automation DSC tooautomatically üzembe hello Azure Site Recovery mobilitási szolgáltatás és az Azure-ügynököt a VMware virtuális gép és a fizikai kiszolgáló replikációs tooAzure"
services: site-recovery
documentationcenter: 
author: krnese
manager: lorenr
editor: 
ms.assetid: 1f8cd3ac-0522-48eb-a5f0-679ee9192ddb
ms.service: site-recovery
ms.workload: storage-backup-recovery
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/01/2017
ms.author: krnese
ms.openlocfilehash: 52cdd13ceb61718a21137180c55db86919af5929
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="deploy-hello-mobility-service-with-azure-automation-dsc-for-replication-of-vm"></a>Hello mobilitási szolgáltatás az Azure Automation DSC Szolgáltatásban a replikáció a virtuális gép üzembe helyezése
Az Operations Management Suite azt biztosít egy átfogó biztonsági mentési és vész-helyreállítási megoldást, amely az üzletmenet folytonosságát biztosító terve részeként is használhatja.

A Hyper-V együtt út indította Hyper-V-replika használatával. De tudunk bővített toosupport heterogén telepítő mert azok felhők ügyfelek több hipervizorok és platformok.

Ha futtat VMware munkaterhelések és/vagy fizikai kiszolgálók ma, egy felügyeleti kiszolgálót futtatja összes hello Azure Site Recovery-összetevők a környezet toohandle hello kommunikáció és replikálás az Azure-a cél Azure esetén.

## <a name="deploy-hello-site-recovery-mobility-service-by-using-automation-dsc"></a>Hello Site Recovery mobilitási szolgáltatás telepítését az Automation DSC
Kezdjük gyors részletes információkat a felügyeleti kiszolgáló funkciója végrehajtásával.

hello felügyeleti kiszolgáló számos kiszolgálói szerepkört futtatja. Ezek a szerepkörök egyik *konfigurációs*, amely kommunikáció koordinálását, és kezeli az adatokat replikálás és helyreállítási folyamatok.

Ezenkívül hello *folyamat* szerepkör replikációs átjáróként. Ezt a szerepkört kap replikációs adatokat védett forrásgépek, gyorsítótárazás, tömörítés és titkosítás segítségével optimalizálja őket, és visszaküldi az tooan Azure storage-fiók. Hello funkciók hello folyamat szerepkör egyik is hello mobilitási szolgáltatás tooprotected gépek toopush telepítését, és a VMware virtuális gépek automatikus észlelését.

Ha a feladat-visszavétel az Azure-ból, hello *fő* szerepkör hello replikációs adatok kezelésére, ez a művelet részeként.

Hello védett gépek, a Microsoft hello támaszkodjon *mobilitásiszolgáltatás*. Ez az összetevő telepített tooevery machine (VMware virtuális gép vagy fizikai kiszolgálón), amelyet az tooreplicate tooAzure áll. Hello gépen végbemenő adatírásokat, és továbbítja őket toohello felügyeleti kiszolgáló (folyamat szerepkör).

Üzleti folytonosság kezelése, esetén fontos toounderstand a munkaterhelések, az infrastruktúra és a hello összetevők szerepet játszanak. A helyreállítási idő célkitűzése (RTO) és a helyreállítási időkorlát (RPO) hello követelményei majd megfelelhetnek. Ebben a környezetben hello mobilitásiszolgáltatás a kulcs tooensuring a munkaterhelések védett módon teheti meg.

Igen, hogyan, egy optimalizált módon biztosíthatja, hogy rendelkezik-e súgó néhány Operations Management Suite összetevői egy megbízható védett telepítése?

Ez a cikk bemutatja, hogyan használhatók az Azure Automation szükséges konfiguráló (DSC), és a hely helyreállítását követően tooensure együttes, amelyek:

* hello mobilitási szolgáltatás és Azure Virtuálisgép-ügynök a telepített toohello windowsos gépekre, amelyet az tooprotect.
* hello mobilitási szolgáltatás és az Azure Virtuálisgép-ügynök mindig futnak, amikor Azure hello replikációs cél.

## <a name="prerequisites"></a>Előfeltételek
* A tárház toostore szükséges hello beállítása
* A tárház toostore hello szükséges jelszót tooregister hello felügyeleti kiszolgálóval

  > [!NOTE]
  > Minden felügyeleti kiszolgáló létrejön egy egyedi jelszót. Ha több felügyeleti kiszolgálót fog toodeploy, hogy tooensure, hogy helyes-e jelszót hello passphrase.txt fájl tárolja hello.
  >
  >
* A Windows Management Framework (WMF) telepítve, amelyet az tooenable (előfeltétele annak, hogy Automation DSC) védelemre hello gépen 5.0

  > [!NOTE]
  > Ha azt szeretné, hogy toouse DSC for Windows gépeken, amelyek WMF 4.0-s verziójával, című hello [DSC használata a kapcsolat nélküli környezetben](## Use DSC in disconnected environments).
  

hello mobilitásiszolgáltatás hello parancssor használatával is telepíthető, és több argumentumot fogad el. Ezért kell toohave hello bináris fájlokat (ezt követően ezeket a telepítésből), és tárolja őket egy helyen, ahol helyreállíthatók a DSC-konfiguráció használatával.

## <a name="step-1-extract-binaries"></a>1. lépés: Kivonat bináris fájlok
1. a telepítéshez szükséges tooextract hello fájlokat keresse meg a toohello címtár a felügyeleti kiszolgálón a következő:

    **\Microsoft azure hely Recovery\home\svsystems\pushinstallsvc\repository**

    Ebben a mappában kell: nevű MSI-fájl

    **Microsoft-ASR_UA_version_Windows_GA_date_Release.exe**

    A következő parancs tooextract hello telepítő hello használata:

    **.\Microsoft-ASR_UA_9.1.0.0_Windows_GA_02May2016_release.exe /q /x:C:\Users\Administrator\Desktop\Mobility_Service\Extract**
2. Válassza ki az összes fájlt, és küldje el tooa tömörített mappa.

Most már rendelkezik hello bináris fájlokat, hogy kell-e a mobilitási szolgáltatás hello tooautomate hello telepítése Automation DSC használatával.

### <a name="passphrase"></a>Hozzáférési kód
A következő lépésben toodetermine kívánt tooplace a tömörített mappából. Egy Azure storage-fiók látható későbbi, toostore hello jelszót, amelyekre szüksége van az hello telepítő is használhatja. hello ügynök ezután regisztrálja az hello felügyeleti kiszolgáló hello folyamat részeként.

hello jelszót hello felügyeleti kiszolgáló telepítésekor portáltól menthető tooa szövegfájl passphrase.txt.

Mind a hello tömörített mappából, és a hello jelszót tegyen egy dedikált tárolójára hello Azure storage-fiók.

![Mappa helye](./media/site-recovery-automate-mobilitysevice-install/folder-and-passphrase-location.png)

Ha jobban szeret tookeep ezeket a fájlokat egy megosztást a hálózaton, megteheti. Egyszerűen tooensure, hogy az Ön által használt később hello DSC-erőforrás hozzáfér, és hello beállítása és a jelszót.

## <a name="step-2-create-hello-dsc-configuration"></a>2. lépés: Hello DSC-konfiguráció létrehozása
hello beállítása WMF 5.0 függ. Hello gép toosuccessfully alkalmazni hello konfigurációs keresztül Automation DSC, WMF 5.0 toobe jelen van szüksége.

hello környezetben használja a következő példa DSC-konfiguráció hello:

```powershell
configuration ASRMobilityService {

    $RemoteFile = 'https://knrecstor01.blob.core.windows.net/asr/ASR.zip'
    $RemotePassphrase = 'https://knrecstor01.blob.core.windows.net/asr/passphrase.txt'
    $RemoteAzureAgent = 'http://go.microsoft.com/fwlink/p/?LinkId=394789'
    $LocalAzureAgent = 'C:\Temp\AzureVmAgent.msi'
    $TempDestination = 'C:\Temp\asr.zip'
    $LocalPassphrase = 'C:\Temp\Mobility_service\passphrase.txt'
    $Role = 'Agent'
    $Install = 'C:\Program Files (x86)\Microsoft Azure Site Recovery'
    $CSEndpoint = '10.0.0.115'
    $Arguments = '/Role "{0}" /InstallLocation "{1}" /CSEndpoint "{2}" /PassphraseFilePath "{3}"' -f $Role,$Install,$CSEndpoint,$LocalPassphrase

    Import-DscResource -ModuleName xPSDesiredStateConfiguration

    node localhost {

        File Directory {
            DestinationPath = 'C:\Temp\ASRSetup\'
            Type = 'Directory'            
        }

        xRemoteFile Setup {
            URI = $RemoteFile
            DestinationPath = $TempDestination
            DependsOn = '[File]Directory'
        }

        xRemoteFile Passphrase {
            URI = $RemotePassphrase
            DestinationPath = $LocalPassphrase
            DependsOn = '[File]Directory'
        }

        xRemoteFile AzureAgent {
            URI = $RemoteAzureAgent
            DestinationPath = $LocalAzureAgent
            DependsOn = '[File]Directory'
        }

        Archive ASRzip {
            Path = $TempDestination
            Destination = 'C:\Temp\ASRSetup'
            DependsOn = '[xRemotefile]Setup'
        }

        Package Install {
            Path = 'C:\temp\ASRSetup\ASR\UNIFIEDAGENT.EXE'
            Ensure = 'Present'
            Name = 'Microsoft Azure Site Recovery mobility Service/Master Target Server'
            ProductId = '275197FC-14FD-4560-A5EB-38217F80CBD1'
            Arguments = $Arguments
            DependsOn = '[Archive]ASRzip'
        }

        Package AzureAgent {
            Path = 'C:\Temp\AzureVmAgent.msi'
            Ensure = 'Present'
            Name = 'Windows Azure VM Agent - 2.7.1198.735'
            ProductId = '5CF4D04A-F16C-4892-9196-6025EA61F964'
            Arguments = '/q /l "c:\temp\agentlog.txt'
            DependsOn = '[Package]Install'
        }

        Service ASRvx {
            Name = 'svagents'
            Ensure = 'Present'
            State = 'Running'
            DependsOn = '[Package]Install'
        }

        Service ASR {
            Name = 'InMage Scout Application Service'
            Ensure = 'Present'
            State = 'Running'
            DependsOn = '[Package]Install'
        }

        Service AzureAgentService {
            Name = 'WindowsAzureGuestAgent'
            Ensure = 'Present'
            State = 'Running'
            DependsOn = '[Package]AzureAgent'
        }

        Service AzureTelemetry {
            Name = 'WindowsAzureTelemetryService'
            Ensure = 'Present'
            State = 'Running'
            DependsOn = '[Package]AzureAgent'
        }
    }
}
```
hello konfigurációs hajt végre a következő hello:

* hello változók hello konfigurációs megtudhatja, hol tooget hello-e binárist hello mobilitási szolgáltatás és a hello Azure Virtuálisgép-ügynök, ha tooget hello jelszót, és ha toostore hello kimeneti.
* hello konfigurációs importálja a hello xPSDesiredStateConfiguration DSC erőforrás, így használhatja `xRemoteFile` toodownload hello fájlok adattárból hello.
* hello konfigurációs létrehoz egy könyvtárat, ahová toostore hello bináris fájljait.
* hello archív erőforrás hello tömörített mappából hello fájlt csomagolja ki.
* hello csomag telepítési erőforrás hello mobilitásiszolgáltatás telepíthessenek hello UNIFIEDAGENT. A következő EXE telepítő hello megadott argumentumokkal. (hello argumentumok összeállításához hello változók kell megváltozott toobe tooreflect meg a környezetben.)
* hello csomag AzureAgent erőforrás hello Azure Virtuálisgép-ügynök, ez az ajánlott minden Azure-ban futó virtuális gépre telepíti. hello Azure Virtuálisgép-ügynök is teszi lehetséges tooadd bővítmények toohello virtuális gép a feladatátvételt követően.
* hello szolgáltatást vagy erőforrást biztosítja, hogy hello kapcsolatos mobilitási szolgáltatásokat és hello Azure-szolgáltatások mindig futnak.

Mentés hello konfigurációt **ASRMobilityService**.

> [!NOTE]
> Ne felejtse el tooreplace hello CSIP a konfigurációs tooreflect hello tényleges felügyeleti kiszolgálóján, így hello ügynök megfelelően csatlakoztatva és hello helyes jelszót fogja használni.
>
>

## <a name="step-3-upload-tooautomation-dsc"></a>3. lépés: Töltse fel a tooAutomation DSC
Mivel végzett hello DSC-konfiguráció importálni fogja a szükséges DSC erőforrásmodul (xPSDesiredStateConfiguration), meg kell tooimport az automatizálás modult hello DSC-konfiguráció feltöltés előtt.

Tooyour Automation-fiók bejelentkezési Tallózás túl**eszközök** > **modulok**, és kattintson a **Tallózás gyűjtemény**.

Itt kereshet hello modul és importálja a tooyour fiók.

![Modul importálása](./media/site-recovery-automate-mobilitysevice-install/search-and-import-module.png)

Ha ezzel végzett, nyissa meg tooyour gép hello Azure Resource Manager-modulok telepítése esetében, és folytatható tooimport az újonnan létrehozott hello DSC-konfiguráció.

### <a name="import-cmdlets"></a>Importálási parancsmagokat
A PowerShell a bejelentkezés tooyour Azure-előfizetés. Hello parancsmagok tooreflect módosítása a környezetben, és rögzítheti az automatizálási fiók adatait egy változóban:

```powershell
$AAAccount = Get-AzureRmAutomationAccount -ResourceGroupName 'KNOMS' -Name 'KNOMSAA'
```

Töltse fel a hello konfigurációs tooAutomation DSC hello a következő parancsmag használatával:

```powershell
$ImportArgs = @{
    SourcePath = 'C:\ASR\ASRMobilityService.ps1'
    Published = $true
    Description = 'DSC Config for Mobility Service'
}
$AAAccount | Import-AzureRmAutomationDscConfiguration @ImportArgs
```

### <a name="compile-hello-configuration-in-automation-dsc"></a>Hello konfiguráció az Automation DSC-fordítási
A következő lépésben toocompile hello konfiguráció Automation DSC, így tooregister csomópontok tooit megkezdése. Érhetők el, amely hello a következő parancsmag futtatásával:

```powershell
$AAAccount | Start-AzureRmAutomationDscCompilationJob -ConfigurationName ASRMobilityService
```

Ez is igénybe vehet néhány percet, mert alapvetően telepít hello konfigurációs üzemeltetett toohello DSC lekérési szolgáltatás.

Után hello konfigurációs lefordításához hello feladatinformációkat powershellel (Get-AzureRmAutomationDscCompilationJob) vagy hello segítségével kérheti le [Azure-portálon](https://portal.azure.com/).

![Feladat beolvasása](./media/site-recovery-automate-mobilitysevice-install/retrieve-job.png)

Most már sikeresen közzé, és a DSC-konfiguráció tooAutomation DSC feltöltve.

## <a name="step-4-onboard-machines-tooautomation-dsc"></a>4. lépés: A bevezetni gépek tooAutomation DSC
> [!NOTE]
> Ez a forgatókönyv befejezése hello előfeltételei egyike, hogy a Windows-alapú gépek hello WMF legújabb verziójára frissít. Töltse le, és telepítse a hello megfelelő verzióját a platformhoz való hello [letöltőközpontból](https://www.microsoft.com/download/details.aspx?id=50395).
>
>

Most, hogy érvényesek-e tooyour csomópontok DSC létrehozandó egy metaconfig. Ennek toosucceed, kell tooretrieve hello URL-cím és hello elsődleges végpontkulcs a kijelölt Automation-fiókhoz az Azure-ban. Ezek az értékek alapján is megtalálhatja **kulcsok** a hello **összes beállítás** hello Automation-fiók paneljén.

![Értékek](./media/site-recovery-automate-mobilitysevice-install/key-values.png)

Ebben a példában egy megjeleníteni kívánt tooprotect Site Recovery segítségével fizikai Windows Server 2012 R2-kiszolgálóval rendelkezik.

### <a name="check-for-any-pending-file-rename-operations-in-hello-registry"></a>Minden függőben lévő fájlátnevezési művelet hello beállításjegyzék ellenőrzése
Mielőtt tooassociate hello server hello Automation DSC-végponthoz, azt javasoljuk, hogy ellenőrizze az összes függőben lévő fájlműveletek átnevezése hello beállításjegyzékben. A befejezési miatt előfordulhat, hogy tiltják hello beállítása tooa függőben lévő újraindítás.

Futtassa a következő parancsmag tooverify, hogy nincs-e hello kiszolgálón nincs függőben lévő újraindítás hello:

```powershell
Get-ItemProperty 'HKLM:\SYSTEM\CurrentControlSet\Control\Session Manager\' | Select-Object -Property PendingFileRenameOperations
```
Ez azt mutatja, üres, ha OK tooproceed áll. Ha nem, eleget kell tennie a karbantartási időszak alatt hello server újraindításával.

tooapply hello konfigurációs hello kiszolgálón indítsa el a hello PowerShell integrált parancsfájlkezelési környezet (ISE), és futtassa a következő parancsfájl hello. Ez a beállítás lényegében egy DSC helyi hello helyi Configuration Manager motor tooregister az Automation DSC szolgáltatás hello utasítani és hello konfigurációs (ASRMobilityService.localhost) beolvasása.

```powershell
[DSCLocalConfigurationManager()]
configuration metaconfig {
    param (
        $URL,
        $Key
    )
    node localhost {
        Settings {
            RefreshFrequencyMins = '30'
            RebootNodeIfNeeded = $true
            RefreshMode = 'PULL'
            ActionAfterReboot = 'ContinueConfiguration'
            ConfigurationMode = 'ApplyAndMonitor'
            AllowModuleOverwrite = $true
        }

        ResourceRepositoryWeb AzureAutomationDSC {
            ServerURL = $URL
            RegistrationKey = $Key
        }

        ConfigurationRepositoryWeb AzureAutomationDSC {
            ServerURL = $URL
            RegistrationKey = $Key
            ConfigurationNames = 'ASRMobilityService.localhost'
        }

        ReportServerWeb AzureAutomationDSC {
            ServerURL = $URL
            RegistrationKey = $Key
        }
    }
}
metaconfig -URL 'https://we-agentservice-prod-1.azure-automation.net/accounts/<YOURAAAccountID>' -Key '<YOURAAAccountKey>'

Set-DscLocalConfigurationManager .\metaconfig -Force -Verbose
```

Ez a konfiguráció miatt hello helyi Configuration Manager motor tooregister magát az Automation DSC Szolgáltatásban. Azt is meghatározza hello motor hogyan működjön, mit kell tenni, ha a konfigurációs eltéréseket (ApplyAndAutoCorrect), és hogyan, kell folytatni a hello konfigurációs Ha újraindításra szükség.

Ez a parancsfájl futtatása után hello csomópont tooregister kell kezdenie az Automation DSC Szolgáltatásban.

![Folyamatban van a csomópont regisztrációs](./media/site-recovery-automate-mobilitysevice-install/register-node.png)

Azure-portálon toohello vissza, ha hello újonnan regisztrált csomóponton most már megjelent a portál hello látható.

![Hello portálon regisztrált csomópont](./media/site-recovery-automate-mobilitysevice-install/registered-node.png)

Hello kiszolgálón futtatható hello PowerShell parancsmag tooverify, hogy a csomópont hello megfelelően regisztrálva van a következő:

```powershell
Get-DscLocalConfigurationManager
```

Hello konfigurációs lekért és alkalmazott toohello server után ezt hello a következő parancsmag futtatásával ellenőrizheti:

```powershell
Get-DscConfigurationStatus
```

hello az alábbiakat mutatja be, hello kiszolgálón sikeresen hívja elő a konfigurációban:

![Kimenet](./media/site-recovery-automate-mobilitysevice-install/successful-config.png)

Ezenkívül hello mobilitási szolgáltatás telepítő rendelkezik-e a saját naplóban, amely a következő *SystemDrive*\ProgramData\ASRSetupLogs.

Ennyi az egész. Most már sikeresen telepített és regisztrált hello mobilitási szolgáltatás, amelyet az tooprotect Site Recovery segítségével hello gépen. A DSC-ből fog győződjön meg arról, hogy hello szükséges szolgáltatások mindig futnak.

![Sikeres telepítés](./media/site-recovery-automate-mobilitysevice-install/successful-install.png)

Miután hello felügyeleti kiszolgáló észleli-hello sikeres telepítés, konfigurálja a védelmet, és replikáció engedélyezése gépen hello Site Recovery segítségével.

## <a name="use-dsc-in-disconnected-environments"></a>A DSC használata a kapcsolat nélküli környezetekben
Ha a gépek nem csatlakoztatott toohello Internet, továbbra is DSC toodeploy támaszkodnak és hello mobilitási szolgáltatás konfigurálása, hogy szeretné-e tooprotect hello munkaterhelések.

A környezetben a saját DSC lekérési kiszolgálójával példányosítható tooessentially biztosítanak hello Automation DSC hogy azonos képességekkel. Ez azt jelenti, hogy hello ügyfelek fogja lekérni a konfigurációs hello (regisztráció) után toohello DSC végpont. Egy másik nincs azonban toomanually leküldéses hello DSC konfigurációs tooyour gépek, helyileg vagy távolról.

Vegye figyelembe, hogy ebben a példában egy hozzáadott paraméter hello számítógép neveként. hello távoli most találhatók egy távoli megosztáson elérhetőnek kell lennie, amelyet az tooprotect hello gépek. hello parancsfájl hello vége hello konfigurációs ír elő, és ezután elindítja az tooapply hello DSC konfigurációs toohello célszámítógépen.

### <a name="prerequisites"></a>Előfeltételek
Győződjön meg arról, hogy hello xPSDesiredStateConfiguration PowerShell modul telepítve van-e. Windows-alapú gépek WMF 5.0 futtató telepítheti hello xPSDesiredStateConfiguration modul hello parancsmag hello célszámítógépen a következő futtatásával:

```powershell
Find-Module -Name xPSDesiredStateConfiguration | Install-Module
```

Is töltse le és mentse hello modul, abban az esetben toodistribute kell azt tooWindows gépeken, amelyek WMF 4.0-s verzióját. Futtassa ezt a parancsmagot egy számítógépen, ha jelen-e PowerShellGet (WMF 5.0):

```powershell
Save-Module -Name xPSDesiredStateConfiguration -Path <location>
```

Is WMF 4.0, győződjön meg arról, hogy hello [Windows 8.1 update KB2883200](https://www.microsoft.com/download/details.aspx?id=40749) a hello készülékre van telepítve.

hello következő konfigurációs továbbíthatja tooWindows gépeken, amelyek WMF 5.0 és WMF 4.0:

```powershell
configuration ASRMobilityService {
    param (
        [Parameter(Mandatory=$true)]
        [ValidateNotNullOrEmpty()]
        [System.String] $ComputerName
    )

    $RemoteFile = '\\myfileserver\share\asr.zip'
    $RemotePassphrase = '\\myfileserver\share\passphrase.txt'
    $RemoteAzureAgent = '\\myfileserver\share\AzureVmAgent.msi'
    $LocalAzureAgent = 'C:\Temp\AzureVmAgent.msi'
    $TempDestination = 'C:\Temp\asr.zip'
    $LocalPassphrase = 'C:\Temp\Mobility_service\passphrase.txt'
    $Role = 'Agent'
    $Install = 'C:\Program Files (x86)\Microsoft Azure Site Recovery'
    $CSEndpoint = '10.0.0.115'
    $Arguments = '/Role "{0}" /InstallLocation "{1}" /CSEndpoint "{2}" /PassphraseFilePath "{3}"' -f $Role,$Install,$CSEndpoint,$LocalPassphrase

    Import-DscResource -ModuleName xPSDesiredStateConfiguration

    node $ComputerName {      
        File Directory {
            DestinationPath = 'C:\Temp\ASRSetup\'
            Type = 'Directory'            
        }

        xRemoteFile Setup {
            URI = $RemoteFile
            DestinationPath = $TempDestination
            DependsOn = '[File]Directory'
        }

        xRemoteFile Passphrase {
            URI = $RemotePassphrase
            DestinationPath = $LocalPassphrase
            DependsOn = '[File]Directory'
        }

        xRemoteFile AzureAgent {
            URI = $RemoteAzureAgent
            DestinationPath = $LocalAzureAgent
            DependsOn = '[File]Directory'
        }

        Archive ASRzip {
            Path = $TempDestination
            Destination = 'C:\Temp\ASRSetup'
            DependsOn = '[xRemotefile]Setup'
        }

        Package Install {
            Path = 'C:\temp\ASRSetup\ASR\UNIFIEDAGENT.EXE'
            Ensure = 'Present'
            Name = 'Microsoft Azure Site Recovery mobility Service/Master Target Server'
            ProductId = '275197FC-14FD-4560-A5EB-38217F80CBD1'
            Arguments = $Arguments
            DependsOn = '[Archive]ASRzip'
        }

        Package AzureAgent {
            Path = 'C:\Temp\AzureVmAgent.msi'
            Ensure = 'Present'
            Name = 'Windows Azure VM Agent - 2.7.1198.735'
            ProductId = '5CF4D04A-F16C-4892-9196-6025EA61F964'
            Arguments = '/q /l "c:\temp\agentlog.txt'
            DependsOn = '[Package]Install'
        }

        Service ASRvx {
            Name = 'svagents'
            State = 'Running'
            DependsOn = '[Package]Install'
        }

        Service ASR {
            Name = 'InMage Scout Application Service'
            State = 'Running'
            DependsOn = '[Package]Install'
        }

        Service AzureAgentService {
            Name = 'WindowsAzureGuestAgent'
            State = 'Running'
            DependsOn = '[Package]AzureAgent'
        }

        Service AzureTelemetry {
            Name = 'WindowsAzureTelemetryService'
            State = 'Running'
            DependsOn = '[Package]AzureAgent'
        }
    }
}
ASRMobilityService -ComputerName 'MyTargetComputerName'

Start-DscConfiguration .\ASRMobilityService -Wait -Force -Verbose
```

Ha azt szeretné tooinstantiate saját DSC lekérési kiszolgálójával be a vállalati hálózat toomimic hello képességeket, amelyek Automation DSC letölthető, lásd: [DSC lekérési webkiszolgáló beállítása](https://msdn.microsoft.com/powershell/dsc/pullserver?f=255&MSPPError=-2147217396).

## <a name="optional-deploy-a-dsc-configuration-by-using-an-azure-resource-manager-template"></a>Választható lehetőség: Telepítését a DSC-konfiguráció Azure Resource Manager-sablonnal
Ez a cikk fordította hogyan hozhatja létre saját DSC-konfiguráció tooautomatically hello mobilitási szolgáltatás és az Azure Virtuálisgép-ügynök – hello telepítéséhez, és győződjön meg arról, hogy futnak a hello gépeken, amelyet az tooprotect. Azt is, hogy az Azure Resource Manager sablon, amely telepíti a DSC konfigurációs tooa új vagy meglévő Azure Automation-fiók. hello sablon bemeneti paraméterek toocreate Automation eszközök, amely a környezetnek hello változók fogja tartalmazni fogja használni.

Hello sablon telepítése után olvassa el a egyszerűen a Ez az útmutató tooonboard 4 toostep a gépek.

hello sablon hajt végre a következő hello:

1. Használjon meglévő Automation-fiókot, vagy hozzon létre egy újat
2. A bemeneti paraméterek igénybe, hogy:
   * ASRRemoteFile – hello mobilitási szolgáltatás beállítása tároló hello helye
   * ASRPassphrase – hello passphrase.txt fájl tároló hello helye
   * ASRCSEndpoint – hello IP-címet a felügyeleti kiszolgáló
3. Hello xPSDesiredStateConfiguration PowerShell modul importálása
4. Hozzon létre és hello DSC-konfiguráció-fordítási

A fenti lépéseket minden hello hello megfelelő sorrendben, történik meg, hogy elindíthatja bevezetése az ügyfélgépek.

hello sablon üzembe helyezés esetén utasításokkal található [GitHub](https://github.com/krnese/AzureDeploy/tree/master/OMS/MSOMS/DSC).

Hello sablon üzembe helyezése a PowerShell használatával:

```powershell
$RGDeployArgs = @{
    Name = 'DSC3'
    ResourceGroupName = 'KNOMS'
    TemplateFile = 'https://raw.githubusercontent.com/krnese/AzureDeploy/master/OMS/MSOMS/DSC/azuredeploy.json'
    OMSAutomationAccountName = 'KNOMSAA'
    ASRRemoteFile = 'https://knrecstor01.blob.core.windows.net/asr/ASR.zip'
    ASRRemotePassphrase = 'https://knrecstor01.blob.core.windows.net/asr/passphrase.txt'
    ASRCSEndpoint = '10.0.0.115'
    DSCJobGuid = [System.Guid]::NewGuid().ToString()
}
New-AzureRmResourceGroupDeployment @RGDeployArgs -Verbose
```

## <a name="next-steps"></a>Következő lépések
Miután hello mobilitási szolgáltatás ügynökök telepítésére, [engedélyezze a replikálást](site-recovery-vmware-to-azure.md) hello virtuális gépekhez.
