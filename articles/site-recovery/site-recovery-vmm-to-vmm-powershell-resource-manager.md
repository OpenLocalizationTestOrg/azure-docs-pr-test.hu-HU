---
title: "Hyper-V virtuális gépek VMM tooa másodlagos hely PowerShell (Azure Resource Manager) aaaReplicate |} Microsoft Docs"
description: "Ismerteti, hogyan toodeploy Azure Site Recovery tooorchestrate replikáció, feladatátvétel és a VMM-ben a Hyper-V virtuális gépek helyreállítási felhők tooa VMM másodlagos hely powershellel (Resource Manager)"
services: site-recovery
documentationcenter: 
author: sujaytalasila
manager: rochakm
editor: raynew
ms.assetid: 9d38e9c3-217c-4e44-830c-575e9a4141f2
ms.service: site-recovery
ms.workload: storage-backup-recovery
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/05/2017
ms.author: sutalasi
ms.openlocfilehash: a769dcc68d66c18b9dc47539071f4d0e0f1db70f
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="replicate-hyper-v-virtual-machines-in-vmm-clouds-tooa-secondary-vmm-site-using-powershell-resource-manager"></a>A VMM felhők tooa VMM másodlagos hely PowerShell (Resource Manager) segítségével a Hyper-V virtuális gépek replikálása
> [!div class="op_single_selector"]
> * [Azure Portal](site-recovery-vmm-to-vmm.md)
> * [Klasszikus portál](site-recovery-vmm-to-vmm-classic.md)
> * [PowerShell – Resource Manager](site-recovery-vmm-to-vmm-powershell-resource-manager.md)
>
>

Üdvözli a Site Recovery tooAzure! Ez a cikk akkor használja, ha azt szeretné, hogy tooreplicate helyszíni Hyper-V virtuális gépek kezelése a System Center Virtual Machine Manager (VMM) felhők tooa másodlagos helyen.

Ez a cikk bemutatja, hogyan toouse PowerShell tooautomate gyakori feladatokat kell tooperform beállításakor Azure Site Recovery tooreplicate Hyper-V virtuális gépek VMM-felhőkben System Center VMM felhők tooSystem Center másodlagos helyen.

hello cikk hello forgatókönyv előfeltételei tartalmazza, és megjeleníti a

* Hogyan tooset mentést a Recovery Services-tároló
* Hello Azure Site Recovery Provider telepítése hello forrás VMM-kiszolgálón, és hello cél VMM-kiszolgálóval
* Regisztrálja hello tárolóban hello VMM kiszolgáló (k)
* Replikációs szabályzat hello VMM-felhő konfigurálása. hello replikációs beállítások vannak megadva hello házirend lesz alkalmazott tooall védett virtuális gépek
* Hello virtuális gépek védelmének engedélyezése.
* Hello feladatátvételi teszt virtuális gépek egyenként vagy a helyreállítási terv toomake meg arról, hogy minden a várt módon működik.
* Hajtsa végre a tervezett vagy nem tervezett feladatátvételt a virtuális gépek egyenként vagy a helyreállítási terv toomake meg arról, hogy minden a várt módon működik részeként.

Ha ez a forgatókönyv beállítása problémát tapasztal, kérdéseit a hello [Azure Recovery Services fórumon](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr).

> [!NOTE]
> Az Azure két különböző [üzembe helyezési modellt](../azure-resource-manager/resource-manager-deployment-model.md) kínál az erőforrások létrehozására és kezelésére: az Azure Resource Manager-modellt és a klasszikus modellt. Azure a két portál – hello a klasszikus Azure portálra, hello klasszikus üzembe helyezési modellt támogató, és a két üzembe helyezési modell támogató Azure-portálon hello is rendelkezik. Ez a cikk ismerteti a hello Resource Manager üzembe helyezési modellben.
>
>

## <a name="on-premises-prerequisites"></a>Helyszíni előfeltételek
Íme a következőkre lesz szüksége a hello elsődleges és másodlagos helyszíni helyek toodeploy ebben a forgatókönyvben:

| **Előfeltételek** | **Részletek** |
| --- | --- |
| **VMM** |Azt javasoljuk, hogy a VMM-kiszolgáló hello elsődleges hely és a VMM-kiszolgáló hello másodlagos hely telepítéséhez.<br/><br/> Emellett [replikálása egy VMM-kiszolgálón felhők közötti](site-recovery-vmm-to-vmm.md#prepare-for-single-server-deployment). toodo ez szüksége lesz legalább két hello VMM-kiszolgálón konfigurált felhők.<br/><br/> VMM-kiszolgálókon legalább futnia kell a System Center 2012 SP1 hello legújabb frissítéseit.<br/><br/> Minden VMM-kiszolgálót rendelkeznie kell egy vagy több konfigurált felhők és az összes felhő beállítása hello Hyper-V kapacitás profillal kell rendelkeznie. <br/><br/>Felhő legalább egy VMM-gazdagépcsoportot kell tartalmaznia.<br/><br/>További információk a VMM-felhőkről beállításáról [konfigurálása hello VMM felhőbeli háló](https://msdn.microsoft.com/library/azure/dn469075.aspx#BKMK_Fabric), és [bemutató: magánfelhők létrehozása a System Center 2012 SP1 VMM](http://blogs.technet.com/b/keithmayer/archive/2013/04/18/walkthrough-creating-private-clouds-with-system-center-2012-sp1-virtual-machine-manager-build-your-private-cloud-in-a-month.aspx).<br/><br/> VMM-kiszolgálókon internet-hozzáféréssel kell rendelkeznie. |
| **Hyper-V** |Hyper-V kiszolgálók legalább futnia kell hello Hyper-V szerepkör és a Windows Server 2012 és a legújabb frissítések telepítve rendelkezik hello.<br/><br/> Minden Hyper-V kiszolgáló tartalmazzon legalább egy virtuális gépet.<br/><br/>  Hyper-V gazdakiszolgálók gazdagépcsoportok hello elsődleges és másodlagos VMM-felhőkben kell elhelyezkedniük.<br/><br/> Ha futtatja a Hyper-V fürt Windows Server 2012 R2 rendszeren kell telepítenie [2961977 frissítése](https://support.microsoft.com/kb/2961977)<br/><br/> Ha futtatja a Hyper-V fürt Windows Server 2012 Megjegyzés: a fürtszervező nem hozza létre automatikusan Ha egy statikus IP cím alapú fürtöt. Manuálisan kell tooconfigure hello fürtszervező. [További információk](http://social.technet.microsoft.com/wiki/contents/articles/18792.configure-replica-broker-role-cluster-to-cluster-replication.aspx). |
| **Szolgáltató** |Site Recovery üzembe helyezése során hello Azure Site Recovery Providert a VMM-kiszolgáló telepítése. hello szolgáltató HTTPS 443-as tooorchestrate replikációs keresztül kommunikál a Site Recovery. Adatok replikáció hello elsődleges és másodlagos Hyper-V kiszolgálók hello LAN keresztül vagy a VPN-kapcsolat között.<br/><br/> hello hello VMM-kiszolgálón futó Provider kell elérni toothese URL-címek: *. hypervrecoverymanager.windowsazure.com; *. accesscontrol.windows.net; *. backup.windowsazure.com; *. blob.core.windows.net; *. store.core.windows.net.<br/><br/> Ezenkívül használhatja a tűzfal kommunikációra a VMM-kiszolgálók toohello hello [Azure datacenter IP-címtartományok](https://www.microsoft.com/download/confirmation.aspx?id=41653) és hello HTTPS (443) protokollt engedélyezi. |

### <a name="network-mapping-prerequisites"></a>Hálózatleképezési előfeltételek
Hálózatleképezés kapcsolatot hoz létre hello elsődleges és másodlagos VMM-kiszolgálókon a VMM-Virtuálisgép-hálózatok között:

* Optimális helyezze el a replika virtuális gépek másodlagos Hyper-V-gazdagépek a feladatátvételt követően.
* Csatlakoztassa a replika virtuális gépek tooappropriate Virtuálisgép-hálózatok.
* Ha nem konfigurálja a hálózati leképezése a replika virtuális gépek nem csatlakoztatott tooany hálózati feladatátvételt követően.
* Ha azt szeretné, hogy a hálózat tooset leképezése során a Site Recovery telepítési itt szolgáltatás következőkre lesz szüksége:

  * Győződjön meg arról, hogy hello forrás Hyper-V gazdakiszolgálón futó virtuális gépek csatlakoztatott tooa VMM Virtuálisgép-hálózatot. Erre a hálózatra kell csatolt tooa hello felhő társított logikai hálózat.
  * Győződjön meg arról, hogy hello fogja használni a helyreállításhoz másodlagos felhő rendelkezik-e a megfelelő Virtuálisgép-hálózatot. A Virtuálisgép-hálózatnak hello másodlagos felhőhöz társított logikai hálózati csatolt tooa kell lennie.

További információ az alábbi cikkek hello VMM hálózatok konfigurálásáról

* [Hogyan tooconfigure logikai hálózatok a VMM Alkalmazásban](http://go.microsoft.com/fwlink/p/?LinkId=386307)
* [Hogyan tooconfigure Virtuálisgép-hálózatokat és átjárókat a VMM-ben](http://go.microsoft.com/fwlink/p/?LinkId=386308)

[További információk](site-recovery-vmm-to-vmm.md#prepare-for-network-mapping) a hálózatleképezés működéséről.

### <a name="powershell-prerequisites"></a>PowerShell-Előfeltételek
Győződjön meg arról, hogy készen áll az Azure PowerShell toogo. Ha már használ PowerShell, szüksége lesz a tooupgrade tooversion 0.8.10 vagy újabb. PowerShell beállításával kapcsolatos információkért lásd: hello [tooinstall útmutató, és konfigurálja az Azure Powershellt](/powershell/azureps-cmdlets-docs). Miután beállítása és PowerShell konfigurálva, megtekintheti az összes rendelkezésre álló parancsmagok hello hello szolgáltatás [Itt](/powershell/azure/overview).

toolearn tippeket, amelyik segíthet a hello parancsmag például paraméterértékeket, bemenetekhez és kimenetekhez általában kezelésének módja az Azure PowerShell használatával kapcsolatban lásd: hello [tooget Started with Azure parancsmagok útmutató](/powershell/azure/get-started-azureps).

## <a name="step-1-set-hello-subscription"></a>1. lépés: Hello előfizetés beállítása
1. Az Azure powershell, Azure-fiók bejelentkezési tooyour: hello a következő parancsmagok használatával

        $UserName = "<user@live.com>"
        $Password = "<password>"
        $SecurePassword = ConvertTo-SecureString -AsPlainText $Password -Force
        $Cred = New-Object System.Management.Automation.PSCredential -ArgumentList $UserName, $SecurePassword
        Login-AzureRmAccount #-Credential $Cred
2. Az előfizetések listájának lekérése. Ez is kilistázza hello subscriptionIDs az egyes hello előfizetések. Jegyezze fel, amelyben toocreate hello recovery services-tároló kívánja hello előfizetés hello előfizetés-azonosító    

        Get-AzureRmSubscription
3. Mely hello a recovery services-tároló: megemlíteni hello előfizetés-azonosító által létrehozott toobe hello előfizetés beállítása

        Set-AzureRmContext –SubscriptionID <subscriptionId>

## <a name="step-2-create-a-recovery-services-vault"></a>2. lépés: Recovery Services-tároló létrehozása
1. Ha már nincs ilyen, hozzon létre egy Azure Resource Manager erőforráscsoportot

        New-AzureRmResourceGroup -Name #ResourceGroupName -Location #location
2. Hozzon létre egy új Recovery Services-tároló, és mentse a létrehozott automatikus rendszer-Helyreállítás tároló objektumhoz (használandó később) változó hello. Beolvasni a hello ASR tároló objektum post létrehozása hello Get-AzureRMRecoveryServicesVault parancsmag segítségével:-

        $vault = New-AzureRmRecoveryServicesVault -Name #vaultname -ResouceGroupName #ResourceGroupName -Location #location

## <a name="step-3-set-hello-recovery-services-vault-context"></a>3. lépés: Hello Recovery Services-tároló környezet beállítása
1. Ha egy már létrehozott tároló, futtassa az alábbi parancs tooget hello tároló hello.

       $vault = Get-AzureRmRecoveryServicesVault -Name #vaultname
2. Hello tároló környezet beállítása hello alábbi parancs futtatásával.

       Set-AzureRmSiteRecoveryVaultSettings -ARSVault $vault

## <a name="step-4-install-hello-azure-site-recovery-provider"></a>4. lépés: Hello Azure Site Recovery Provider telepítése
1. Hello VMM-gépen hozzon létre egy könyvtárat hello a következő parancs futtatásával:

       New-Item c:\ASR -type directory
2. Bontsa ki a letöltött hello szolgáltató használatával hello a következő parancs futtatásával hello fájlt

       pushd C:\ASR\
       .\AzureSiteRecoveryProvider.exe /x:. /q
3. Hello szolgáltató a következő parancsok hello segítségével telepíti:

       .\SetupDr.exe /i
       $installationRegPath = "hklm:\software\Microsoft\Microsoft System Center Virtual Machine Manager Server\DRAdapter"
       do
       {
         $isNotInstalled = $true;
         if(Test-Path $installationRegPath)
         {
           $isNotInstalled = $false;
         }
       }While($isNotInstalled)

   Várjon, amíg hello telepítési toofinish.
4. Hello kiszolgáló regisztrálása hello tárolónál hello a következő parancsot:

       $BinPath = $env:SystemDrive+"\Program Files\Microsoft System Center 2012 R2\Virtual Machine Manager\bin"
       pushd $BinPath
       $encryptionFilePath = "C:\temp\".\DRConfigurator.exe /r /Credentials $VaultSettingFilePath /vmmfriendlyname $env:COMPUTERNAME /dataencryptionenabled $encryptionFilePath /startvmmservice

## <a name="step-5-create-and-associate-a-replication-policy"></a>5. lépés: Házirend létrehozása és társítása a replikációs
1. Hozzon létre egy Hyper-V 2012 R2-ben replikációs házirendet hello a következő parancs futtatásával:

        $ReplicationFrequencyInSeconds = "300";        #options are 30,300,900
        $PolicyName = “replicapolicy”
        $RepProvider = HyperVReplica2012R2
        $Recoverypoints = 24                    #specify hello number of hours tooretain recovery pints
        $AppConsistentSnapshotFrequency = 4 #specify hello frequency (in hours) at which app consistent snapshots are taken
        $AuthMode = "Kerberos"  #options are "Kerberos" or "Certificate"
        $AuthPort = "8083"  #specify hello port number that will be used for replication traffic on Hyper-V hosts
        $InitialRepMethod = "Online" #options are "Online" or "Offline"

        $policyresult = New-AzureRmSiteRecoveryPolicy -Name $policyname -ReplicationProvider $RepProvider -ReplicationFrequencyInSeconds $Replicationfrequencyinseconds -RecoveryPoints $recoverypoints -ApplicationConsistentSnapshotFrequencyInHours $AppConsistentSnapshotFrequency -Authentication $AuthMode -ReplicationPort $AuthPort -ReplicationMethod $InitialRepMethod

    > [!NOTE]
    > VMM-felhő hello (ahogy azt hello Hyper-V előfeltételei) Windows Server különböző verzióit futtató Hyper-V-gazdagépek is tartalmazhat, de hello replikációs házirend adott operációsrendszer-verzió. Ha a különböző operációs rendszereken futó különböző gazdagépeken, majd hozza létre külön replikációs házirendet minden operációs rendszer verziója. A pl.: Ha öt gazdagépeken futó Windows-kiszolgálók 2012 és Windows Server 2012 R2 három, létre kell hoznia két replikációs házirendek – egyet az egyes operációs rendszerekhez.

1. Töltse le a hello elsődleges védelmi tároló (elsődleges VMM-felhőben) és a helyreállítási védelmi tároló (helyreállítási VMM-felhőben) hello a következő parancsok futtatásával:

       $PrimaryCloud = "testprimarycloud"
       $primaryprotectionContainer = Get-AzureRmSiteRecoveryProtectionContainer -friendlyName $PrimaryCloud;  

       $RecoveryCloud = "testrecoverycloud"
       $recoveryprotectionContainer = Get-AzureRmSiteRecoveryProtectionContainer -friendlyName $RecoveryCloud;  
2. 1. lépés használatával létrehozott hello házirend beolvasása hello hello házirend rövid neve

       $policy = Get-AzureRmSiteRecoveryPolicy -FriendlyName $policyname
3. Hello társítás hello védelmi tároló (VMM-felhőben) kezdődnie hello replikációs házirendhez:

       $associationJob  = Start-AzureRmSiteRecoveryPolicyAssociationJob -Policy     $Policy -PrimaryProtectionContainer $primaryprotectionContainer -RecoveryProtectionContainer $recoveryprotectionContainer
4. Várjon, amíg hello házirend társítása feladat toocomplete. Ha a hello feladat befejeződött, a következő PowerShell-részlet hello segítségével ellenőrizheti.

       $job = Get-AzureRmSiteRecoveryJob -Job $associationJob

       if($job -eq $null -or $job.StateDescription -ne "Completed")
       {
         $isJobLeftForProcessing = $true;
       }

   Hello feladat feldolgozása után futtassa a következő parancs hello:

       if($isJobLeftForProcessing)
       {
         Start-Sleep -Seconds 60
       }
       }While($isJobLeftForProcessing)

hello művelet, toocheck hello befejezése kövesse hello [figyelési tevékenység](#monitor).

## <a name="step-6-configure-network-mapping"></a>6. lépés: Hálózatleképezés konfigurálása
1. hello első parancs beolvasása kiszolgálók hello aktuális Azure Site Recovery-tárolóban. hello parancs hello Microsoft Azure Site Recovery kiszolgálók hello $Servers tömbváltozó tárolja.

        $Servers = Get-AzureRmSiteRecoveryServer
2. alább parancsok hello futtasson hello hely helyreállítási hálózati hello forrás VMM-kiszolgálót és hello cél VMM-kiszolgálóval.

        $PrimaryNetworks = Get-AzureRmSiteRecoveryNetwork -Server $Servers[0]        

        $RecoveryNetworks = Get-AzureRmSiteRecoveryNetwork -Server $Servers[1]

    > [!NOTE]
    > hello forrás VMM-kiszolgálót is kell hello elsőt vagy hello második egy a hello kiszolgálók tömb. Ellenőrizze a hello hello VMM-kiszolgáló nevét, és hello hálózatok megfelelően beolvasása


1. hello végső parancsmag hello elsődleges és hello helyreállítási hálózat közötti leképezést hoz létre. hello parancsmag hello elsődleges hálózati hello $PrimaryNetworks és hello helyreállítási hálózaton: hello $RecoveryNetworks első elemének első eleme határozza meg.

        New-AzureRmSiteRecoveryNetworkMapping -PrimaryNetwork $PrimaryNetworks[0] -RecoveryNetwork $RecoveryNetworks[0]

## <a name="step-7-configure-storage-mapping"></a>7. lépés: Konfigurálja a tárolási leképezése
1. hello alábbi parancs beolvassa a $storageclassifications változó tárhelybesorolások hello listáját.

        $storageclassifications = Get-AzureRmSiteRecoveryStorageClassification
2. alább parancsok hello hello Forrás besorolás $SourceClassificaion változó be és a cél besorolás $TargetClassification változó be kapják meg.

        $SourceClassificaion = $storageclassifications[0]

        $TargetClassification = $storageclassifications[1]

    > [!NOTE]
    > hello forrású és besorolásokat is lehet bármely hello tömb elemei. Tekintse meg az alábbi parancs toofigure hello forrása és célja besorolások $storageclassifications tömb indexe hello toohello kimenetét.

    > Get-AzureRmSiteRecoveryStorageClassification |} Select-Object - tulajdonság FriendlyName, azonosító |} Táblázat formázása


1. alább parancsmag hello hello forrás besorolási és hello cél egymáshoz leképezéseket hoz létre.

        New-AzureRmSiteRecoveryStorageClassificationMapping -PrimaryStorageClassification $SourceClassificaion -RecoveryStorageClassification $TargetClassification

## <a name="step-8-enable-protection-for-virtual-machines"></a>8. lépés: A virtuális gépek védelmének engedélyezése
Hello kiszolgálók, felhők és hálózatok megfelelő konfigurálását követően engedélyezheti a hello felhő virtuális gépek védelmét.

1. Futtassa a következő parancs tooget hello védelmi tároló hello tooenable védelem:

          $PrimaryProtectionContainer = Get-AzureRmSiteRecoveryProtectionContainer -friendlyName $PrimaryCloudName
2. Töltse le a hello védelmi entitás (VM) hello a következő parancs futtatásával:

           $protectionEntity = Get-AzureRmSiteRecoveryProtectionEntity -friendlyName $VMName -ProtectionContainer $PrimaryProtectionContainer
3. Engedélyezze a replikációját hello VM hello a következő parancs futtatásával:

          $jobResult = Set-AzureRmSiteRecoveryProtectionEntity -ProtectionEntity $protectionentity -Protection Enable -Policy $policy

## <a name="test-your-deployment"></a>A központi telepítés tesztelése
Tervezze meg a központi telepítés futtathat egyetlen virtuális gép feladatátvételi tesztjét, vagy több virtuális gép álló és hello a feladatátvételi teszt futtatása helyreállítási terv létrehozása tootest. A feladatátvételi teszt segítségével elkülönített hálózatban próbálhatja ki a feladatátvételi és helyreállítási mechanizmusokat.

> [!NOTE]
> Az alkalmazás helyreállítási tervet hozhat létre Azure-portálon.
>
>

hello művelet, toocheck hello befejezése kövesse hello [figyelési tevékenység](#monitor).

### <a name="run-a-test-failover"></a>Feladatátvételi teszt futtatása
1. Futtassa az alábbi parancsmagok tooget hello VM hálózati toowhich hello kívánt tootest feladatátvételt a virtuális gépek számára.

       $Servers = Get-AzureRmSiteRecoveryServer
       $RecoveryNetworks = Get-AzureRmSiteRecoveryNetwork -Server $Servers[1]
2. A virtuális gépek feladatátvételi teszt végrehajtása hello következő tevékenységek végrehajtásával:

       $protectionEntity = Get-AzureRmSiteRecoveryProtectionEntity -FriendlyName $VMName -ProtectionContainer $PrimaryprotectionContainer

       $jobIDResult =  Start-AzureRmSiteRecoveryTestFailoverJob -Direction PrimaryToRecovery -ProtectionEntity $protectionEntity -VMNetwork $RecoveryNetworks[1]
3. A helyreállítási terv feladatátvételi teszt végrehajtása hello következő tevékenységek végrehajtásával:

       $recoveryplanname = "test-recovery-plan"

       $recoveryplan = Get-AzureRmSiteRecoveryRecoveryPlan -FriendlyName $recoveryplanname

       $jobIDResult =  Start-AzureRmSiteRecoveryTestFailoverJob -Direction PrimaryToRecovery -Recoveryplan $recoveryplan -VMNetwork $RecoveryNetworks[1]

### <a name="run-a-planned-failover"></a>Tervezett feladatátvétel futtatása
1. A virtuális gépek tervezett feladatátvétel végrehajtása hello következő tevékenységek végrehajtásával:

        $protectionEntity = Get-AzureRmSiteRecoveryProtectionEntity -Name $VMName -ProtectionContainer $PrimaryprotectionContainer

        $jobIDResult =  Start-AzureRmSiteRecoveryPlannedFailoverJob -Direction PrimaryToRecovery -ProtectionEntity $protectionEntity
2. A helyreállítási terv tervezett feladatátvétel végrehajtása hello következő tevékenységek végrehajtásával:

        $recoveryplanname = "test-recovery-plan"

        $recoveryplan = Get-AzureRmSiteRecoveryRecoveryPlan -FriendlyName $recoveryplanname

        $jobIDResult =  Start-AzureRmSiteRecoveryPlannedFailoverJob -Direction PrimaryToRecovery -Recoveryplan $recoveryplan

### <a name="run-an-unplanned-failover"></a>A nem tervezett feladatátvétel
1. Hajtsa végre a nem tervezett feladatátvételt a virtuális gépek hello következő tevékenységek végrehajtásával:

        $protectionEntity = Get-AzureRmSiteRecoveryProtectionEntity -Name $VMName -ProtectionContainer $PrimaryprotectionContainer

        $jobIDResult =  Start-AzureRmSiteRecoveryUnPlannedFailoverJob -Direction PrimaryToRecovery -ProtectionEntity $protectionEntity

2 a helyreállítási terv nem tervezett feladatátvétel végrehajtása hello következő tevékenységek végrehajtásával:

        $recoveryplanname = "test-recovery-plan"

        $recoveryplan = Get-AzureRmSiteRecoveryRecoveryPlan -FriendlyName $recoveryplanname

        $jobIDResult =  Start-AzureRmSiteRecoveryUnPlannedFailoverJob -Direction PrimaryToRecovery -ProtectionEntity $protectionEntity

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
[További](/powershell/module/azurerm.recoveryservices.backup/#recovery) Azure Site Recovery Azure Resource Manager PowerShell-parancsmagokkal kapcsolatos.
