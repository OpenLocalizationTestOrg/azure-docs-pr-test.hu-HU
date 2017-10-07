---
title: "aaaReplicate Hyper-V virtuális gépek tooAzure hello klasszikus portál a PowerShell-lel |} Microsoft Docs"
description: "Automatizálhatja a hello replikálást a Hyper-V virtuális gépek VMM-felhőkben hello klasszikus portál helyreállítás és a PowerShell használatával"
services: site-recovery
documentationcenter: 
author: bsiva
manager: abhiag
editor: tysonn
ms.assetid: 9011f567-e0b4-4306-951a-b30da19f5db6
ms.service: site-recovery
ms.workload: storage-backup-recovery
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/31/2017
ms.author: bsiva
ms.openlocfilehash: d6847b46ac227209e6890de4ab603b23f827360f
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="replicate-hyper-v-vms-tooazure-with-powershell-in-hello-classic-portal"></a>A klasszikus portálon hello a PowerShell segítségével a Hyper-V virtuális gépek tooAzure replikálása
> [!div class="op_single_selector"]
> * [Azure Portal](site-recovery-vmm-to-azure.md)
> * [PowerShell – Resource Manager](site-recovery-vmm-to-azure-powershell-resource-manager.md)
> * [Klasszikus portál](site-recovery-vmm-to-azure-classic.md)
> * [PowerShell – Klasszikus](site-recovery-deploy-with-powershell.md)
>
>

## <a name="overview"></a>Áttekintés
Az Azure Site Recovery hozzájárul tooyour üzleti folytonossági és vészhelyreállítási (BCDR) helyreállítási stratégia megvalósításában replikációjának, feladatátvételének és helyreállítási virtuális gép a különböző telepítési forgatókönyvek esetén. Egy teljes listája központi telepítési forgatókönyvek: hello [Azure Site Recovery áttekintését](site-recovery-overview.md).

Ez a cikk bemutatja, hogyan toouse PowerShell tooautomate gyakori feladatokat kell tooperform Azure Site Recovery tooreplicate Hyper-V virtuális gépek a System Center VMM-felhők tooAzure tárolási beállításakor.

hello cikkből hello forgatókönyvet, és bemutatja, hogyan telepítheti a tooset Site Recovery-tároló, be Azure Site Recovery Provider hello hello forrás VMM-kiszolgálón előfeltételeit, regisztrálja hello kiszolgálót hello tárolóban, Azure-tárfiók hozzáadása, hello Azure telepítéséhez Recovery Services Agent ügynököt a Hyper-V gazdakiszolgálók, adja meg, amely alkalmazott tooall védett virtuális gépek fog, majd engedélyezze a védelmet azon virtuális gépek VMM-felhő védelmi beállításait. Végül tesztelési hello feladatátvételi toomake meg arról, hogy minden a várt módon működik.

Ha ez a forgatókönyv beállítása problémát tapasztal, kérdéseit a hello [Azure Recovery Services fórumon](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr).

> [!NOTE]
> Azure az erőforrások létrehozására és kezelésére két különböző üzembe helyezési modellel rendelkezik: [Resource Manager és klasszikus](../azure-resource-manager/resource-manager-deployment-model.md). Ez a cikk hello klasszikus telepítési modell használatát bemutatja.
>
>

## <a name="before-you-start"></a>Előkészületek
Győződjön meg arról, hogy az előfeltételek teljesülnek:

### <a name="azure-prerequisites"></a>Azure-előfeltételek
* Szüksége lesz egy [Microsoft Azure](https://azure.microsoft.com/)-fiókra. Kezdésként használhatja az [ingyenes próbaverziót](https://azure.microsoft.com/pricing/free-trial/) is.
* Szüksége lesz egy Azure storage fiók toostore replikált adatokat. hello fiókot kell georeplikáció engedélyezve van. A hello és hello Azure Site Recovery-tárolónak ugyanabban a régióban, és társítani kell hello ugyanahhoz az előfizetéshez. [További tudnivalók az Azure storage](../storage/common/storage-introduction.md).
* Szüksége lesz a toomake meg arról, hogy tooprotect kívánt virtuális gépek ahhoz [Azure virtuális gép Előfeltételek](site-recovery-support-matrix-to-azure.md#failed-over-azure-vm-requirements).

### <a name="vmm-prerequisites"></a>A VMM-Előfeltételek
* A System Center 2012 R2 rendszeren futó VMM-kiszolgáló lesz szüksége.
* A VMM-kiszolgálónak tooprotect hello legalább egy felhő lesz szüksége. hello felhők tartalmazzák:
  * Legalább egy VMM-gazdagépcsoport.
  * Egy vagy több Hyper-V gazdakiszolgálók vagy fürtök minden gazdagépcsoportban.
  * Egy vagy több virtuális gépek hello forrás Hyper-V kiszolgálón.

### <a name="hyper-v-prerequisites"></a>Hyper-V előfeltételei
* hello Hyper-V gazdakiszolgálók futnia kell az legalább **Windows Server 2012** Hyper-V szerepkörrel vagy **Microsoft Hyper-V Server 2012** és a legújabb frissítések telepítve rendelkezik hello.
* Ha fürtben futtatja a Hyper-V-t, ne feledje, hogy ha statikus IP-címen alapuló fürtöt használ, a rendszer nem hozza létre automatikusan a fürtszervezőt. Manuálisan kell tooconfigure hello fürtszervező. toodo ezt, a Kiszolgálókezelő > Feladatátvevőfürt-kezelőt, csatlakoztassa toohello fürtöt, kattintson a **szerepkör konfigurálása** válassza ki **Hyper-V Replikaszervező** a hello **Szerepkörválasztás**hello magas rendelkezésre állás varázsló képernyőjén.
* Bármely Hyper-V gazdakiszolgáló vagy fürt legyen toomanage védelmi szerepelnie kell egy VMM-felhőben.

### <a name="network-mapping-prerequisites"></a>Hálózatleképezési előfeltételek
Ha virtuális gépet az Azure-hálózat hozzárendelés véd felel meg a Virtuálisgép-hálózatok hello forrás VMM-kiszolgálón és a cél Azure-hálózatok tooenable hello következő között:

* Feladatok átadása a hello azonos gépeire hálózati kapcsolódhatnak tooeach más, függetlenül a helyreállítási terv.
* Ha egy hálózati átjáró működik hello cél Azure-hálózatot, virtuális gépek tooother a helyszíni virtuális gépek is elérheti.
* Ha nem adja meg a hálózati leképezése csak virtuális gépek feladatátvételt hello azonos helyreállítási terv feladatátvételi tooAzure után lesznek képesek tooconnect tooeach más.

Ha azt szeretné, hogy toodeploy hálózatra való leképezés, hello következőkre lesz szüksége:

* azt szeretné, hogy a forrás VMM-kiszolgálón hello tooprotect hello virtuális gépek csatlakoztatott tooa Virtuálisgép-hálózathoz kell lennie. Erre a hálózatra kell csatolt tooa hello felhő társított logikai hálózat.
* Egy Azure-hálózat toowhich replikált virtuális gépek a feladatátvételt követően csatlakozni tudnak. Ezt a hálózatot a feladatátvételi hello időpontjában válasszon ki. hello hello hálózat legyen ugyanabban a régióban, az Azure Site Recovery-előfizetést.

### <a name="powershell-prerequisites"></a>PowerShell-Előfeltételek
Győződjön meg arról, hogy készen áll az Azure PowerShell toogo. Ha már használ PowerShell, szüksége lesz a tooupgrade tooversion 0.8.10 vagy újabb. PowerShell beállításával kapcsolatos információkért lásd: [hogyan tooinstall és konfigurálja az Azure Powershellt](/powershell/azureps-cmdlets-docs). Miután beállítása és PowerShell konfigurálva, megtekintheti az összes rendelkezésre álló parancsmagok hello hello szolgáltatás [Itt](/powershell/azure/overview).

toolearn tippeket, amelyik segíthet a hello parancsmag például paraméterértékeket, bemenetekhez és kimenetekhez általában kezelésének módja az Azure PowerShell használatával kapcsolatban lásd: [Ismerkedés az Azure-parancsmagok](/powershell/azure/get-started-azureps).

## <a name="step-1-set-hello-subscription"></a>1. lépés: Hello előfizetés beállítása
A PowerShellben futtassa a parancsmagokat:

```
$UserName = "<user@live.com>"
$Password = "<password>"
$AzureSubscriptionName = "prod_sub1"

$SecurePassword = ConvertTo-SecureString -AsPlainText $Password -Force
$Cred = New-Object System.Management.Automation.PSCredential -ArgumentList $UserName, $securePassword
Add-AzureAccount -Credential $Cred;
$AzureSubscription = Select-AzureSubscription -SubscriptionName $AzureSubscriptionName

```

Hello elemek hello "< >" belül cserélje le a kért adatokat.

## <a name="step-2-create-a-site-recovery-vault"></a>2. lépés: A Site Recovery-tároló létrehozása
A PowerShellben hello elemek hello "< >" belül cserélje le a kért adatokat, és futtassa az alábbi parancsokat:

```

$VaultName = "<testvault123>"
$VaultGeo  = "<Southeast Asia>"
$OutputPathForSettingsFile = "<c:\>"

```


```
New-AzureSiteRecoveryVault -Location $VaultGeo -Name $VaultName;
$vault = Get-AzureSiteRecoveryVault -Name $VaultName;

```

## <a name="step-3-generate-a-vault-registration-key"></a>3. lépés: A tároló regisztrációs kulcsának létrehozása
Hozzon létre egy regisztrációs kulcsot hello tárolóban lévő állapottal. Miután hello Azure Site Recovery Provider letöltése, és telepítse a VMM-kiszolgálón hello, a kulcs tooregister hello VMM-kiszolgáló hello tárolóban fogja használni.

1. Hello tároló beállítási fájl beolvasása és hello környezet beállítása:

   ```

   $VaultName = "<testvault123>"
   $VaultGeo  = "<Southeast Asia>"
   $OutputPathForSettingsFile = "<c:\>"

   $VaultSetingsFile = Get-AzureSiteRecoveryVaultSettingsFile -Location $VaultGeo -Name $VaultName -Path $OutputPathForSettingsFile;

   ```
2. Hello tároló környezet beállítása hello a következő parancsok futtatásával:

   ```

   $VaultSettingFilePath = $vaultSetingsFile.FilePath
   $VaultContext = Import-AzureSiteRecoveryVaultSettingsFile -Path $VaultSettingFilePath -ErrorAction Stop

   ```

## <a name="step-4-install-hello-azure-site-recovery-provider"></a>4. lépés: Hello Azure Site Recovery Provider telepítése
1. Hello VMM-gépen hozzon létre egy könyvtárat hello a következő parancs futtatásával:

   ```

   pushd C:\ASR\

   ```
2. Bontsa ki a letöltött hello szolgáltató használatával hello a következő parancs futtatásával hello fájlt

   ```

   AzureSiteRecoveryProvider.exe /x:. /q

   ```
3. Hello szolgáltató a következő parancsok hello segítségével telepíti:

   ```

   .\SetupDr.exe /i

   ```

   ```

   $installationRegPath = "hklm:\software\Microsoft\Microsoft System Center Virtual Machine Manager Server\DRAdapter"
   do
   {
     $isNotInstalled = $true;
     if(Test-Path $installationRegPath)
     {
         $isNotInstalled = $false;
     }
   }While($isNotInstalled)

   ```

   Várjon, amíg hello telepítési toofinish.
4. Hello kiszolgáló regisztrálása hello tárolónál hello a következő parancsot:

   ```

   $BinPath = $env:SystemDrive+"\Program Files\Microsoft System Center 2012 R2\Virtual Machine Manager\bin"
   pushd $BinPath
   $encryptionFilePath = "C:\temp\"
   .\DRConfigurator.exe /r /Credentials $VaultSettingFilePath /vmmfriendlyname $env:COMPUTERNAME /dataencryptionenabled $encryptionFilePath /startvmmservice

   ```

## <a name="step-5-create-an-azure-storage-account"></a>5. lépés: Az Azure storage-fiók létrehozása
Ha nem rendelkezik Azure-tárfiók, hozzon létre egy engedélyezett a georeplikáció fiókot hello a következő parancs futtatásával:

```

$StorageAccountName = "teststorageacc1"
$StorageAccountGeo  = "Southeast Asia"

New-AzureStorageAccount -StorageAccountName $StorageAccountName -Label $StorageAccountName -Location $StorageAccountGeo;

```

Vegye figyelembe, hogy hello tárfiókot kell lennie a hello és hello Azure Site Recovery szolgáltatásnak ugyanabban a régióban, és társítva hello ugyanahhoz az előfizetéshez.

## <a name="step-6-install-hello-azure-recovery-services-agent"></a>6. lépés: Hello Azure Recovery Services Agent telepítése
Hello Azure-portálon, minden Hyper-V gazdagép-kiszolgálón található VMM-felhőkben hello telepítés hello Azure Recovery Services Agent ügynököt, tooprotect szeretné.

Futtassa az alábbi a parancs minden VMM-gazdagép hello:

```

marsagentinstaller.exe /q /nu

```


## <a name="step-7-configure-cloud-protection-settings"></a>7. lépés: Konfigurálja a felhő védelmi beállításait
1. Hozza létre a felhő védelmi profil tooAzure hello a következő parancs futtatásával:

   ```

   $ReplicationFrequencyInSeconds = "300";
   $ProfileResult = New-AzureSiteRecoveryProtectionProfileObject -ReplicationProvider HyperVReplica -RecoveryAzureSubscription $AzureSubscriptionName -RecoveryAzureStorageAccount $StorageAccountName -ReplicationFrequencyInSeconds     $ReplicationFrequencyInSeconds;

   ```
2. Töltse le a védelmi tároló hello a következő parancsok futtatásával:

   ```

   $PrimaryCloud = "testcloud"
   $protectionContainer = Get-AzureSiteRecoveryProtectionContainer -Name $PrimaryCloud;

   ```
3. Indítsa el a hello társítás hello védelmi tároló hello felhő:

   ```

   $associationJob = Start-AzureSiteRecoveryProtectionProfileAssociationJob -ProtectionProfile $profileResult -PrimaryProtectionContainer $protectionContainer;        

   ```
4. Hello feladat befejeződését követően futtassa a következő parancs hello:

        $job = Get-AzureSiteRecoveryJob -Id $associationJob.JobId;
        if($job -eq $null -or $job.StateDescription -ne "Completed")
        {
        $isJobLeftForProcessing = $true;
        }


1. Hello feladat feldolgozása után futtassa a következő parancs hello:

        Do
        {
        $job = Get-AzureSiteRecoveryJob -Id $associationJob.JobId;
        Write-Host "Job State:{0}, StateDescription:{1}" -f Job.State, $job.StateDescription;
        if($job -eq $null -or $job.StateDescription -ne "Completed")
        {
            $isJobLeftForProcessing = $true;
        }

        if($isJobLeftForProcessing)
        {
            Start-Sleep -Seconds 60
        }
        }While($isJobLeftForProcessing)



hello művelet, toocheck hello befejezése kövesse hello [figyelési tevékenység](#monitor).

## <a name="step-8-configure-network-mapping"></a>8. lépés: Hálózatleképezés konfigurálása
Mielőtt elkezdené hálózatleképezés ellenőrizze, hogy hello forrás VMM-kiszolgálón található virtuális gépek csatlakoztatott tooa Virtuálisgép-hálózatot. Továbbá hozzon létre legalább egy Azure virtuális hálózatot. Vegye figyelembe, hogy több Virtuálisgép-hálózatot is csatlakoztatott tooa egyetlen Azure-hálózatra.

Vegye figyelembe, hogy ha hello célhálózatban már több alhálózat működik, és ezek egyikének rendelkezik hello neve megegyezik a virtual machine hello forrás alhálózati található, akkor hello replika virtuális gép lesz a cél alhálózathoz csatlakoztatott toothat feladatátvételt követően. Ha nincs egyező nevű cél alhálózat, hello virtuális gép lesz a csatlakoztatott toohello hello hálózat első alhálózatát.

hello első parancs beolvasása kiszolgálók hello aktuális Azure Site Recovery-tárolóban. hello parancs hello Microsoft Azure Site Recovery kiszolgálók hello $Servers tömbváltozó tárolja.

    $Servers = Get-AzureSiteRecoveryServer


hello második parancs hello hely helyreállítási hálózati hello első kiszolgáló kéri le a hello $Servers tömbben. hello parancs hello hálózatok hello $Networks változó tárolja.

    $Networks = Get-AzureSiteRecoveryNetwork -Server $Servers[0]

hello harmadik parancs az Azure-előfizetések lekérdezi a Get-AzureSubscription hello parancsmag használatával, és majd hello $Subscriptions változó tárolja ezt az értéket.

    $Subscriptions = Get-AzureSubscription



hello negyedik parancs hello $AzureVmNetworks változó a Get-AzureVNetSite hello parancsmagot, és ezt az értéket segítségével kéri le egy Azure virtuális hálózatot.

    $AzureVmNetworks = Get-AzureVNetSite



hello végső parancsmag hello elsődleges hálózati és hello Azure virtuális gép hálózati közötti leképezést hoz létre. hello parancsmag hello elsődleges hálózati $Networks hello első eleme határozza meg. hello parancsmag határozza meg a virtuálisgép-hálózat első elemének hello $AzureVmNetworks azonosítójú használatával hello parancs tartalmazza az Azure-előfizetés-azonosítójával.

    New-AzureSiteRecoveryNetworkMapping -PrimaryNetwork $Networks[0] -AzureSubscriptionId $Subscriptions[0].SubscriptionId -AzureVMNetworkId $AzureVmNetworks[0].Id


## <a name="step-9-enable-protection-for-virtual-machines"></a>9. lépés: Virtuális gépek védelmének engedélyezése
Kiszolgálók, felhők és hálózatok megfelelő konfigurálását követően engedélyezheti a hello felhő virtuális gépek védelmét. Vegye figyelembe a következőket hello:

A virtuális gépeknek meg kell felelniük [Azure virtuális gép Előfeltételek](site-recovery-support-matrix-to-azure.md#failed-over-azure-vm-requirements).

hello virtuális gép tooenable védelmi hello operációsrendszer- és operációsrendszer-lemez tulajdonságait kell állítani. Ha egy virtuális gép létrehozása a VMM-virtuálisgép-sablon használatával beállíthatja hello tulajdonság. Ezeket a tulajdonságokat a meglévő virtuális gépek állítsa be a hello **általános** és **hardverkonfiguráció** hello virtuális gép tulajdonságok lapján. Ha ezeket a tulajdonságokat a VMM-ben nem állít be fog tudni tooconfigure őket hello Azure Site Recovery portálon.

1. Futtassa a következő parancs tooget hello védelmi tároló hello tooenable védelem:

     $ProtectionContainer = get-AzureSiteRecoveryProtectionContainer-$CloudName neve
2. Töltse le a hello védelmi entitás (VM) hello a következő parancs futtatásával:

        $protectionEntity = Get-AzureSiteRecoveryProtectionEntity -Name $VMName -ProtectionContainer $protectionContainer



1. A virtuális gép hello vész-Helyreállítási hello engedélyezése hello a következő parancs futtatásával:

        $jobResult = Set-AzureSiteRecoveryProtectionEntity -ProtectionEntity $protectionEntity     -Protection Enable -Force



## <a name="test-your-deployment"></a>A központi telepítés tesztelése
Tervezze meg a központi telepítés futtathat egyetlen virtuális gép feladatátvételi tesztjét, vagy több virtuális gép álló és hello a feladatátvételi teszt futtatása helyreállítási terv létrehozása tootest. A feladatátvételi teszt segítségével elkülönített hálózatban próbálhatja ki a feladatátvételi és helyreállítási mechanizmusokat. Vegye figyelembe:

* Ha tooconnect toohello virtuális gépet az Azure-ban a távoli asztal hello feladatátvétel után, engedélyezze a távoli asztali kapcsolat hello virtuális gépen, hello a feladatátvételi teszt futtatása előtt.
* A feladatátvétel után a nyilvános IP cím tooconnect toohello virtuális gépek fogja használni a távoli asztali kapcsolat segítségével. Ha meg akarja toodo, győződjön meg arról, nincs érvényben tartományi szabályzatok, amelyek meggátolják, kapcsolódó tooa virtuális gép nyilvános cím segítségével.

hello művelet, toocheck hello befejezése kövesse hello [figyelési tevékenység](#monitor).

### <a name="create-a-recovery-plan"></a>Helyreállítási terv létrehozása
1. Hozzon létre egy .xml fájlt a helyreállítási terv hello adatok az alábbi sablont, és elmentse "C:\RPTemplatePath.xml".
2. Hello RecoveryPlan csomópont azonosító, név, PrimaryServerId és SecondaryServerId módosítása.
3. Hello ProtectionEntity csomópont PrimaryProtectionEntityId (a VMM-ből vmid) módosítása.
4. További virtuális gépeket adhat hozzá további ProtectionEntity csomópontok hozzáadásával.

        <#
        <?xml version="1.0" encoding="utf-16"?>
        <RecoveryPlan Id="d0323b26-5be2-471b-addc-0a8742796610" Name="rp-test"     PrimaryServerId="9350a530-d5af-435b-9f2b-b941b5d9fcd5"     SecondaryServerId="21a9403c-6ec1-44f2-b744-b4e50b792387" Description=""     Version="V2014_07">
          <Actions />
          <ActionGroups>
            <ShutdownAllActionGroup Id="ShutdownAllActionGroup">
              <PreActionSequence />
              <PostActionSequence />
            </ShutdownAllActionGroup>
            <FailoverAllActionGroup Id="FailoverAllActionGroup">
              <PreActionSequence />
              <PostActionSequence />
            </FailoverAllActionGroup>
            <BootActionGroup Id="DefaultActionGroup">
              <PreActionSequence />
              <PostActionSequence />
              <ProtectionEntity PrimaryProtectionEntityId="d4c8ce92-a613-4c63-9b03-    cf163cc36ef8" />
            </BootActionGroup>
          </ActionGroups>
          <ActionGroupSequence>
            <ActionGroup Id="ShutdownAllActionGroup" ActionId="ShutdownAllActionGroup"     Before="FailoverAllActionGroup" />
            <ActionGroup Id="FailoverAllActionGroup" ActionId="FailoverAllActionGroup"     After="ShutdownAllActionGroup" Before="DefaultActionGroup" />
            <ActionGroup Id="DefaultActionGroup" ActionId="DefaultActionGroup" After="FailoverAllActionGroup"/>
          </ActionGroupSequence>
        </RecoveryPlan>
        #>



1. Töltse ki hello sablon hello adatokat:

        $TemplatePath = "C:\RPTemplatePath.xml";



1. Hello RecoveryPlan létrehozása:

        $RPCreationJob = New-AzureSiteRecoveryRecoveryPlan -File $TemplatePath -WaitForCompletion;

### <a name="run-a-test-failover"></a>Feladatátvételi teszt futtatása
1. Hello RecoveryPlan objektum beolvasása hello a következő parancs futtatásával:

     $RPObject = get-AzureSiteRecoveryRecoveryPlan-Name $RPName;
2. Indítsa el a hello feladatátvételi teszthez hello a következő parancs futtatásával:

        $jobIDResult = Start-AzureSiteRecoveryTestFailoverJob -RecoveryPlan $RPObject -Direction PrimaryToRecovery;


## <a name=monitor></a>Figyelési tevékenység
A következő parancsok toomonitor hello tevékenység hello használata. Vegye figyelembe, hogy rendelkezik-e toowait Between hello feldolgozási toofinish feladataihoz.

    Do
    {
            $job = Get-AzureSiteRecoveryJob -Id $associationJob.JobId;
            Write-Host "Job State:{0}, StateDescription:{1}" -f Job.State, $job.StateDescription;
            if($job -eq $null -or $job.StateDescription -ne "Completed")
            {
                $isJobLeftForProcessing = $true;
            }

        if($isJobLeftForProcessing)
            {
                Start-Sleep -Seconds 60
            }
    }While($isJobLeftForProcessing)



## <a name="next-steps"></a>Következő lépések
[További](/powershell/azure/overview) Azure Site Recovery PowerShell-parancsmagokkal. </a>.
