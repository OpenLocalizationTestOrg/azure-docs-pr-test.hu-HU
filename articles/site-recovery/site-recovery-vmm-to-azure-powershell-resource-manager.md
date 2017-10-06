---
title: "aaaReplicate Hyper-V virtuális gépek VMM-felhőkben Azure Site Recovery és a PowerShell használatával (Resource Manager) |} Microsoft Docs"
description: "Hyper-V virtuális gépek replikálása az Azure Site Recovery és a PowerShell használatával a VMM-felhők"
services: site-recovery
documentationcenter: 
author: Rajani-Janaki-Ram
manager: rochakm
editor: raynew
ms.assetid: 6ac509ad-5024-43d8-b621-d8fec019b9a9
ms.service: site-recovery
ms.workload: storage-backup-recovery
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/05/2017
ms.author: rajanaki
ms.openlocfilehash: b0f641de4e10a600ead415ceb9bd488fb4d1659d
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="replicate-hyper-v-virtual-machines-in-vmm-clouds-tooazure-using-powershell-and-azure-resource-manager"></a>A VMM-felhők tooAzure PowerShell és az Azure Resource Manager használatával Hyper-V virtuális gépek replikálása
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

hello cikk hello forgatókönyv előfeltételei tartalmazza, és megjeleníti a

* Hogyan tooset mentést a Recovery Services-tároló
* A forrás VMM-kiszolgálón hello hello Azure Site Recovery Provider telepítése
* Regisztrálja hello kiszolgálót hello tárolóban, az Azure-tárfiók hozzáadása
* Hello Azure Recovery Services agent telepítése a Hyper-V gazdakiszolgálókra
* Adja meg a VMM-felhő, lesz alkalmazott tooall védett virtuális gép védelmi beállításait
* Ezen virtuális gépek védelmének engedélyezése.
* Tesztelje a hello feladatátvételi toomake meg arról, hogy minden a várt módon működik.

Ha ez a forgatókönyv beállítása problémát tapasztal, kérdéseit a hello [Azure Recovery Services fórumon](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr).

> [!NOTE]
> Azure az erőforrások létrehozására és kezelésére két különböző üzembe helyezési modellel rendelkezik: [Resource Manager és klasszikus](../azure-resource-manager/resource-manager-deployment-model.md). Ez a cikk hello Resource Manager telepítési modell használatát bemutatja.
>
>

## <a name="before-you-start"></a>Előkészületek
Győződjön meg arról, hogy az előfeltételek teljesülnek:

### <a name="azure-prerequisites"></a>Azure-előfeltételek
* Szüksége lesz egy [Microsoft Azure](https://azure.microsoft.com/)-fiókra. Ha még nincs fiókja, kezdje a [ingyenes fiókot](https://azure.microsoft.com/free). Emellett áttekintheti az hello [Azure Site Recovery Manager árképzési](https://azure.microsoft.com/pricing/details/site-recovery/).
* Hello replikációs tooa CSP előfizetés forgatókönyvben, ha szüksége lesz egy CSP-előfizetést. További információ a hello CSP programról [hogyan hello CSP programban tooenroll](https://msdn.microsoft.com/library/partnercenter/mt156995.aspx).
* Szüksége lesz egy Azure v2 (erőforrás-kezelő) tárolási fiók toostore replikált adatok tooAzure. hello fiókot kell georeplikáció engedélyezve van. Lehetséges értékek: hello és hello Azure Site Recovery szolgáltatásnak ugyanabban a régióban, és hello társítható ugyanahhoz az előfizetéshez vagy hello CSP előfizetés. További információ az Azure storage beállítása toolearn lásd: hello [Azure Storage bemutatása tooMicrosoft](../storage/common/storage-introduction.md) referenciaként.
* Szüksége lesz arra, hogy a virtuális gépeknek meg tooprotect megfelelnek-e hello toomake [Azure virtuális gép Előfeltételek](site-recovery-support-matrix-to-azure.md#failed-over-azure-vm-requirements).

> [!NOTE]
> Csak a virtuális gép szintű műveletek jelenleg Powershellen keresztül lehetséges. Helyreállítási terv szintű műveletek támogatása hamarosan fog történni.  Most korlátozott tooperforming sikertelenek-garantál csak a "védett virtuális gép" lépésköz és nem a helyreállítási terv szintű áll.
>
>

### <a name="vmm-prerequisites"></a>A VMM-Előfeltételek
* A System Center 2012 R2 rendszeren futó VMM-kiszolgáló lesz szüksége.
* A virtuális gépeket tartalmazó VMM-kiszolgáló kívánt tooprotect hello Azure Site Recovery Provider kell futnia. Ez hello Azure Site Recovery üzembe helyezése során telepítve van.
* A VMM-kiszolgálónak tooprotect hello legalább egy felhő lesz szüksége. hello felhők tartalmazzák:
  * Legalább egy VMM-gazdagépcsoport.
  * Minden gazdagépcsoportban legalább egy Hyper-V gazdakiszolgáló vagy fürt.
  * Egy vagy több virtuális gépek hello forrás Hyper-V kiszolgálón.
* További információ a VMM-felhőkben beállítása:
  * További információk a VMM-magánfelhőkben [újdonságai a System Center 2012 R2 VMM Magánfelhő](http://go.microsoft.com/fwlink/?LinkId=324952) és a [VMM 2012 és a hello](http://go.microsoft.com/fwlink/?LinkId=324956).
  * További tudnivalók [konfigurálása hello VMM felhőbeli háló](https://msdn.microsoft.com/library/azure/dn469075.aspx#BKMK_Fabric)
  * A magánfelhők létrehozásával kapcsolatos további követően a felhőbeli háló elemek helyen [magánfelhő létrehozása a VMM Alkalmazásban](http://go.microsoft.com/fwlink/p/?LinkId=324953) és a [bemutató: magánfelhők létrehozása a System Center 2012 SP1 VMM](http://go.microsoft.com/fwlink/p/?LinkId=324954).

### <a name="hyper-v-prerequisites"></a>Hyper-V előfeltételei
* hello Hyper-V gazdakiszolgálók futnia kell az legalább **Windows Server 2012** Hyper-V szerepkörrel vagy **Microsoft Hyper-V Server 2012** és a legújabb frissítések telepítve rendelkezik hello.
* Ha fürtben futtatja a Hyper-V-t, ne feledje, hogy ha statikus IP-címen alapuló fürtöt használ, a rendszer nem hozza létre automatikusan a fürtszervezőt. Manuálisan kell tooconfigure hello fürtszervező. A
* Útmutatásért lásd: [hogyan tooConfigure Hyper-V Replikaszervező](http://blogs.technet.com/b/haroldwong/archive/2013/03/27/server-virtualization-series-hyper-v-replica-broker-explained-part-15-of-20-by-yung-chou.aspx).
* Bármely Hyper-V gazdakiszolgáló vagy fürt legyen toomanage védelmi szerepelnie kell egy VMM-felhőben.

### <a name="network-mapping-prerequisites"></a>Hálózatleképezési előfeltételek
Ha a virtuális gépek Azure-ban, hello hálózatleképezés kapcsolatot hoz létre a virtuálisgép-hálózatok hello hello forrás VMM-kiszolgálón és a cél Azure-hálózatok tooenable hello következő:

* Feladatok átadása a hello azonos gépeire hálózati kapcsolódhatnak tooeach más, függetlenül a helyreállítási terv.
* Ha egy hálózati átjáró működik hello cél Azure-hálózatot, virtuális gépek tooother a helyszíni virtuális gépek is elérheti.
* Ha nem adja meg a hálózatleképezés, csak hello feladatátvételt hello azonos virtuális gépek helyreállítási terv feladatátvételi tooAzure után lesznek képesek tooconnect tooeach más.

Ha azt szeretné, hogy toodeploy hálózatra való leképezés, hello következőkre lesz szüksége:

* azt szeretné, hogy a forrás VMM-kiszolgálón hello tooprotect hello virtuális gépek csatlakoztatott tooa Virtuálisgép-hálózathoz kell lennie. Erre a hálózatra kell csatolt tooa hello felhő társított logikai hálózat.
* Egy Azure-hálózat toowhich replikált virtuális gépek feladatátvételi után kapcsolódhatnak. Ezt a hálózatot a feladatátvételi hello időpontjában válasszon ki. hello hello hálózat legyen ugyanabban a régióban, az Azure Site Recovery-előfizetést.

További információ a hálózatra való leképezés

* [Hogyan tooconfigure logikai hálózatok a VMM Alkalmazásban](http://go.microsoft.com/fwlink/p/?LinkId=386307)
* [Hogyan tooconfigure Virtuálisgép-hálózatokat és átjárókat a VMM-ben](http://go.microsoft.com/fwlink/p/?LinkId=386308)
* [Hogyan tooconfigure és a figyelő virtuális hálózatok az Azure-ban](https://azure.microsoft.com/documentation/services/virtual-network/)

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
1. Hozzon létre egy erőforráscsoportot az Azure Resource Manager Ha még nem rendelkezik már

        New-AzureRmResourceGroup -Name #ResourceGroupName -Location #location
2. Hozzon létre egy új Recovery Services-tároló, és mentse a létrehozott automatikus rendszer-Helyreállítás tároló objektumhoz (használandó később) változó hello. Beolvasni a hello ASR tároló objektum post létrehozása hello Get-AzureRMRecoveryServicesVault parancsmag segítségével:-

        $vault = New-AzureRmRecoveryServicesVault -Name #vaultname -ResouceGroupName #ResourceGroupName -Location #location

## <a name="step-3-set-hello-recovery-services-vault-context"></a>3. lépés: Hello Recovery Services-tároló környezet beállítása

Hello tároló környezet beállítása hello alábbi parancs futtatásával.

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

## <a name="step-5-create-an-azure-storage-account"></a>5. lépés: Az Azure storage-fiók létrehozása

Egy Azure storage-fiók nem rendelkezik, ha engedélyezett a georeplikáció-fiók létrehozása a hello hello azonos tárfiókéval tároló hello a következő parancs futtatásával:

        $StorageAccountName = "teststorageacc1"    #StorageAccountname
        $StorageAccountGeo  = "Southeast Asia"     
        $ResourceGroupName =  “myRG”             #ResourceGroupName
        $RecoveryStorageAccount = New-AzureRmStorageAccount -ResourceGroupName $ResourceGroupName -Name $StorageAccountName -Type “StandardGRS” -Location $StorageAccountGeo

Vegye figyelembe, hogy hello tárfiókot kell lennie a hello és hello Azure Site Recovery szolgáltatásnak ugyanabban a régióban, és társítva hello ugyanahhoz az előfizetéshez.

## <a name="step-6-install-hello-azure-recovery-services-agent"></a>6. lépés: Hello Azure Recovery Services Agent telepítése
1. Töltse le a hello Azure Recovery Services Agent ügynököt a [http://aka.ms/latestmarsagent](http://aka.ms/latestmarsagent) , és telepítse azt minden Hyper-V gazdagép-kiszolgálón található hello VMM felhők, tooprotect.
2. Futtassa az alábbi a parancs minden VMM-gazdagép hello:

       marsagentinstaller.exe /q /nu

## <a name="step-7-configure-cloud-protection-settings"></a>7. lépés: Konfigurálja a felhő védelmi beállításait
1. Hozzon létre egy replikációs házirend tooAzure hello a következő parancs futtatásával:

        $ReplicationFrequencyInSeconds = "300";        #options are 30,300,900
        $PolicyName = “replicapolicy”
        $Recoverypoints = 6                    #specify hello number of recovery points

        $policryresult = New-AzureRmSiteRecoveryPolicy -Name $policyname -ReplicationProvider HyperVReplicaAzure -ReplicationFrequencyInSeconds $replicationfrequencyinseconds -RecoveryPoints $recoverypoints -ApplicationConsistentSnapshotFrequencyInHours 1 -RecoveryAzureStorageAccountId "/subscriptions/q1345667/resourceGroups/test/providers/Microsoft.Storage/storageAccounts/teststorageacc1"

1. Töltse le a védelmi tároló hello a következő parancsok futtatásával:

       $PrimaryCloud = "testcloud"
       $protectionContainer = Get-AzureRmSiteRecoveryProtectionContainer -friendlyName $PrimaryCloud;  
2. Hello házirend részletek tooa változó feldolgozással hello létrehozott és hello rövid házirendnév megemlíteni olvasható be:

       $policy = Get-AzureRmSiteRecoveryPolicy -FriendlyName $policyname
3. Hello társítás hello védelmi tároló kezdődnie hello replikációs házirendhez:

       $associationJob  = Start-AzureRmSiteRecoveryPolicyAssociationJob -Policy     $Policy -PrimaryProtectionContainer $protectionContainer  
4. Hello feladat befejeződését követően futtassa a következő parancs hello:

       $job = Get-AzureRmSiteRecoveryJob -Job $associationJob

       if($job -eq $null -or $job.StateDescription -ne "Completed")
       {
         $isJobLeftForProcessing = $true;
       }
5. Hello feladat feldolgozása után futtassa a következő parancs hello:

       if($isJobLeftForProcessing)
       {
         Start-Sleep -Seconds 60
       }
       }While($isJobLeftForProcessing)

hello művelet, toocheck hello befejezése kövesse hello [figyelési tevékenység](#monitor).

## <a name="step-8-configure-network-mapping"></a>8. lépés: Hálózatleképezés konfigurálása
Mielőtt elkezdené hálózatleképezés ellenőrizze, hogy hello forrás VMM-kiszolgálón található virtuális gépek csatlakoztatott tooa Virtuálisgép-hálózatot. Továbbá hozzon létre legalább egy Azure virtuális hálózatot.

További tudnivalók hogyan toocreate a virtuális hálózat Azure Resource Manager és a PowerShell, a [virtuális hálózat létrehozása az Azure Resource Manager és a PowerShell használatával pont-pont VPN-kapcsolattal](../vpn-gateway/vpn-gateway-create-site-to-site-rm-powershell.md)

Vegye figyelembe, hogy több virtuálisgép-hálózat is csatlakoztatott tooa egyetlen Azure-hálózatra. Ha hello célhálózatban már több alhálózat működik, és ezek egyikének rendelkezik hello neve megegyezik a virtual machine hello forrás alhálózati található, akkor hello replika virtuális gép feladatátvételi után lesz csatlakoztatott toothat cél alhálózathoz. Ha nincsenek egyező nevű cél alhálózathoz, hello virtuális gép lesz a csatlakoztatott toohello hello hálózat első alhálózatát.

1. hello első parancs beolvasása kiszolgálók hello aktuális Azure Site Recovery-tárolóban. hello parancs hello Microsoft Azure Site Recovery kiszolgálók hello $Servers tömbváltozó tárolja.

        $Servers = Get-AzureRmSiteRecoveryServer
2. hello második parancs hello hely helyreállítási hálózati hello első kiszolgáló kéri le a hello $Servers tömbben. hello parancs hello hálózatok hello $Networks változó tárolja.

        $Networks = Get-AzureRmSiteRecoveryNetwork -Server $Servers[0]

1. hello harmadik parancs a hello $AzureVmNetworks változóval lekérdezi az Azure virtuális hálózataihoz, és ezt az értéket.

        $AzureVmNetworks =  Get-AzureRmVirtualNetwork
2. hello végső parancsmag hello elsődleges hálózati és hello Azure virtuális gép hálózati közötti leképezést hoz létre. hello parancsmag hello elsődleges hálózati $Networks hello első eleme határozza meg. hello parancsmag egy virtuális gép hálózati $AzureVmNetworks hello első eleme határozza meg.

        New-AzureRmSiteRecoveryNetworkMapping -PrimaryNetwork $Networks[0] -AzureVMNetworkId $AzureVmNetworks[0]

## <a name="step-9-enable-protection-for-virtual-machines"></a>9. lépés: Virtuális gépek védelmének engedélyezése
Hello kiszolgálók, felhők és hálózatok megfelelő konfigurálását követően engedélyezheti a hello felhő virtuális gépek védelmét.

 Vegye figyelembe a következőket hello:

* Virtuális gépek Azure-követelményeknek kell megfelelniük. Ellenőrizze, hogy ezek szerepel [Előfeltételek és a támogatás](site-recovery-support-matrix-to-azure.md#failed-over-azure-vm-requirements) hello tervezési útmutató.
* hello virtuális gép tooenable védelmi, hello operációsrendszer- és operációsrendszer-lemez tulajdonságait kell állítani. Ha egy virtuális gép létrehozása a VMM-virtuálisgép-sablon használatával beállíthatja hello tulajdonság. Ezeket a tulajdonságokat a meglévő virtuális gépek állítsa be a hello **általános** és **hardverkonfiguráció** hello virtuális gép tulajdonságok lapján. Ha ezeket a tulajdonságokat a VMM-ben nem állít be fog tudni tooconfigure őket hello Azure Site Recovery portálon.

1. Futtassa a következő parancs tooget hello védelmi tároló hello tooenable védelem:

          $ProtectionContainer = Get-AzureRmSiteRecoveryProtectionContainer -friendlyName $CloudName
2. Töltse le a hello védelmi entitás (VM) hello a következő parancs futtatásával:

           $protectionEntity = Get-AzureRmSiteRecoveryProtectionEntity -friendlyName $VMName -ProtectionContainer $protectionContainer
3. A virtuális gép hello vész-Helyreállítási hello engedélyezése hello a következő parancs futtatásával:

          $jobResult = Set-AzureRmSiteRecoveryProtectionEntity -ProtectionEntity $protectionentity -Protection Enable –Force -Policy $policy -RecoveryAzureStorageAccountId  $storageID "/subscriptions/217653172865hcvkchgvd/resourceGroups/rajanirgps/providers/Microsoft.Storage/storageAccounts/teststorageacc1

## <a name="test-your-deployment"></a>A központi telepítés tesztelése
tootest a központi telepítés egy vizsgálat futtatása egy egyetlen virtuális gép feladatátvételi hozhat létre több virtuális gépet tartalmazó helyreállítási tervet, illetve a teszt feladatátvételi hello terv futtatása. Feladatátvételi teszt elszigetelt hálózatot a feladatátvételi és helyreállítási mechanizmusokat szimulálja. Vegye figyelembe:

* Ha azt szeretné, hogy a távoli asztali kapcsolat segítségével után hello feladatátvételi tooconnect toohello virtuális gépet, engedélyezze a távoli asztali kapcsolat hello virtuális gépen, hello a feladatátvételi teszt futtatása előtt.
* Után feladatátvételi egy nyilvános IP cím tooconnect toohello virtuális gépet fog használni a távoli asztali kapcsolat segítségével. Ha meg akarja toodo, győződjön meg arról, nincs érvényben tartományi szabályzatok, amelyek meggátolják, kapcsolódó tooa virtuális gép nyilvános cím segítségével.

hello művelet, toocheck hello befejezése kövesse hello [figyelési tevékenység](#monitor).

### <a name="run-a-test-failover"></a>Feladatátvételi teszt futtatása
- Indítsa el a hello feladatátvételi teszthez hello a következő parancs futtatásával:

       $protectionEntity = Get-AzureRmSiteRecoveryProtectionEntity -Name $VMName -ProtectionContainer $protectionContainer

       $jobIDResult =  Start-AzureRmSiteRecoveryTestFailoverJob -Direction PrimaryToRecovery -ProtectionEntity $protectionEntity -AzureVMNetworkId <string>  

### <a name="run-a-planned-failover"></a>Tervezett feladatátvétel futtatása
- Indítsa el a hello tervezett feladatátvétel hello a következő parancs futtatásával:

        $protectionEntity = Get-AzureRmSiteRecoveryProtectionEntity -Name $VMName -ProtectionContainer $protectionContainer

        $jobIDResult =  Start-AzureRmSiteRecoveryPlannedFailoverJob -Direction PrimaryToRecovery -ProtectionEntity $protectionEntity -AzureVMNetworkId <string>  

### <a name="run-an-unplanned-failover"></a>A nem tervezett feladatátvétel
- Indítsa el a nem tervezett feladatátvétel hello hello a következő parancs futtatásával:

        $protectionEntity = Get-AzureRmSiteRecoveryProtectionEntity -Name $VMName -ProtectionContainer $protectionContainer

        $jobIDResult =  Start-AzureRmSiteRecoveryUnPlannedFailoverJob -Direction PrimaryToRecovery -ProtectionEntity $protectionEntity -AzureVMNetworkId <string>  

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
