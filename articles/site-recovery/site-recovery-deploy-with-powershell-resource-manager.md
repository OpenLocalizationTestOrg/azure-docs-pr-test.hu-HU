---
title: "Hyper-V virtuális gépek PowerShell és az Azure Resource Manager aaaReplicate |} Microsoft Docs"
description: "A Hyper-V virtuális gépek tooAzure hello replikációs automatizálhatja az Azure Site Recovery PowerShell és az Azure Resource Manager használatával."
services: site-recovery
documentationcenter: 
author: bsiva
manager: abhiag
editor: 
ms.assetid: 05e0d76e-c3f5-4845-8052-094019b6d102
ms.service: site-recovery
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: storage-backup-recovery
ms.date: 05/31/2017
ms.author: bsiva
ms.openlocfilehash: 4fb15ce2e9ad54f1dd6f54ff769eb912aa4b0272
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="replicate-between-on-premises-hyper-v-virtual-machines-and-azure-by-using-powershell-and-azure-resource-manager"></a>PowerShell és az Azure Resource Manager használatával a helyszíni Hyper-V virtuális gépek és az Azure közötti replikáció
> [!div class="op_single_selector"]
> * [Azure Portal](site-recovery-hyper-v-site-to-azure.md)
> * [PowerShell – Resource Manager](site-recovery-deploy-with-powershell-resource-manager.md)
> * [Klasszikus portál](site-recovery-hyper-v-site-to-azure-classic.md)
>
>

## <a name="overview"></a>Áttekintés
Az Azure Site Recovery hozzájárul tooyour üzleti folytonossági és vészhelyreállítási helyreállítási stratégia megvalósításában a replikáció, feladatátvétel és a különböző telepítési forgatókönyvek esetén a virtuális gépek helyreállítása. Központi telepítési forgatókönyvek teljes listáját lásd: hello [Azure Site Recovery áttekintését](site-recovery-overview.md).

Az Azure PowerShell egy modul, amely a parancsmagok toomanage Azure, a Windows PowerShell segítségével. Együtt tudjon működni az kétféle modulok: hello Azure profil modul vagy hello Azure Resource Manager modul.

Site Recovery PowerShell parancsmagokat, elérhető az Azure PowerShell az Azure Resource Manager segítségével védelme és helyreállítása a kiszolgálók az Azure-ban.

Ez a cikk ismerteti, hogyan toouse Windows PowerShell, együtt Azure Resource Manager, a Site Recovery tooconfigure toodeploy és levezényléséhez server védelmi tooAzure. a cikk ezt használja hello példa bemutatja, hogyan tooprotect, a feladatátvétel, és helyreállítani a virtuális gépeket a Hyper-V-gazdagép tooAzure, az Azure Resource Manager Azure PowerShell használatával.

> [!NOTE]
> hello Site Recovery PowerShell parancsmagokat jelenleg lehetővé teszik a következő tooconfigure hello: egy a Virtual Machine Manager-hely tooanother, egy a Virtual Machine Manager-hely tooAzure és a Hyper-V hely tooAzure.
>
>

Nem kell a PowerShell szakértői toouse toobe ebben a cikkben, de toounderstand hello alapszintű fogalmakkal, mint a modulok, a parancsmagok és a munkamenetek van szüksége. További információ a Windows PowerShellről: [Ismerkedés a Windows PowerShell-lel](http://technet.microsoft.com/library/hh857337.aspx).

Akkor is olvashat további tudnivalókat [az Azure PowerShell használata Azure Resource Managerrel](../powershell-azure-resource-manager.md).

> [!NOTE]
> Microsoft-partnerek hello Cloud Solution Provider (CSP) program részét képező konfigurálható és kezelhető az ügyfelek kiszolgálók védelmét tootheir ügyfelek megfelelő CSP előfizetések (bérlői előfizetések).
>
>

## <a name="before-you-start"></a>Előkészületek
Győződjön meg arról, hogy az előfeltételek teljesülnek:

* A [Microsoft Azure](https://azure.microsoft.com/) fiók. Kezdésként használhatja az [ingyenes próbaverziót](https://azure.microsoft.com/pricing/free-trial/) is. Emellett áttekintheti az [Azure Site Recovery Manager árképzési](https://azure.microsoft.com/pricing/details/site-recovery/).
* Azure PowerShell 1.0. Ebben a kiadásban információt, és hogyan tooinstall, lásd: [Azure PowerShell 1.0.](https://azure.microsoft.com/)
* Hello [AzureRM.SiteRecovery](https://www.powershellgallery.com/packages/AzureRM.SiteRecovery/) és [AzureRM.RecoveryServices](https://www.powershellgallery.com/packages/AzureRM.RecoveryServices/) modulok. Ezek a modulok hello legújabb verziója letölthető hello [PowerShell-galériában](https://www.powershellgallery.com/)

Ez a cikk bemutatja, hogyan toouse Azure PowerShell-lel rendelkező Azure Resource Manager tooconfigure és a kiszolgálók a védelem kezeléséhez. a cikk ezt használja hello példa bemutatja, hogyan tooprotect egy olyan virtuális géphez, a Hyper-V gazdagépen tooAzure. a következő előfeltételek hello adott toothis példa. Átfogóbb állítja be a hello követelményei a Site Recovery különböző lehetőségeket, tekintse meg a vonatkozó toothat forgatókönyv toohello dokumentációját.

* A Hyper-V gazdagépen fut a Windows Server 2012 R2 vagy Microsoft Hyper-V Server 2012 R2 tartalmazó egy vagy több virtuális géphez.
* Hyper-V kiszolgálók toohello Internet, csatlakoztatva, közvetlen vagy proxyn keresztül.
* hello tooprotect kívánt virtuális gépek meg kell felelnie az [a virtuális gép előfeltételei](site-recovery-support-matrix-to-azure.md#failed-over-azure-vm-requirements).

## <a name="step-1-sign-in-tooyour-azure-account"></a>1. lépés: Bejelentkezés tooyour Azure-fiók
1. Nyisson meg egy PowerShell-konzolt, és futtassa a parancsot toosign tooyour Azure-fiók. hello parancsmag megnyitása egy weblap, amelyen kérni fogja a hitelesítő adatait.

        Login-AzureRmAccount

    Alternatív megoldásként akkor is lehetnek a fiók hitelesítő adataival mint egy paraméterrel toohello `Login-AzureRmAccount` parancsmag hello segítségével `-Credential` paraméter.

    Ha működik a bérlő nevében CSP partner, adja meg hello ügyfél a bérlők a tenantID vagy bérlői elsődleges tartománynév használatával.

        Login-AzureRmAccount -Tenant "fabrikam.com"
2. Egy fiók több előfizetések van így toouse hello fiókkal szeretne hello előfizetés társítania kell.

        Select-AzureRmSubscription -SubscriptionName $SubscriptionName
3. Győződjön meg arról, hogy az előfizetéshez regisztrált toouse hello szolgáltatók az Azure Recovery Services és a hely helyreállítását követően hello a következő parancsok használatával:

   * `Get-AzureRmResourceProvider -ProviderNamespace  Microsoft.RecoveryServices`
   * `Get-AzureRmResourceProvider -ProviderNamespace  Microsoft.SiteRecovery`

   A parancsok, ha hello hello kimenetéből **RegistrationState** értéke túl**regisztrált**, folytathatja a tooStep 2. Ha nem, hello hiányzó szolgáltató regisztrálnia kell az előfizetésben.

   tooregister hello Azure provider a Site Recovery és helyreállítási szolgáltatásokhoz, futtassa a következő parancsok hello:

       Register-AzureRmResourceProvider -ProviderNamespace Microsoft.SiteRecovery
       Register-AzureRmResourceProvider -ProviderNamespace Microsoft.RecoveryServices

   Győződjön meg arról, hogy hello szolgáltatók regisztrálása sikeresen hello a következő parancsok használatával: `Get-AzureRmResourceProvider -ProviderNamespace  Microsoft.RecoveryServices` és `Get-AzureRmResourceProvider -ProviderNamespace  Microsoft.SiteRecovery`.

## <a name="step-2-set-up-hello-recovery-services-vault"></a>2. lépés: A Recovery Services-tároló hello beállítása
1. Hozzon létre egy Azure Resource Manager-csoportot, amelyben meg kell hello tároló létrehozása, vagy használjon egy meglévő erőforráscsoportot. Létrehozhat egy új erőforráscsoportot hello a következő parancs használatával:

        New-AzureRmResourceGroup -Name $ResourceGroupName -Location $Geo  

    hello $ResourceGroupName változó hello erőforráscsoport hello nevét tartalmazza, ahol azt szeretné, hogy toocreate, és hello $Geo változót tartalmaz hello Azure régióban mely toocreate hello erőforráscsoportban (például "Dél-Brazília").

    Ezt úgy szerezheti be az erőforráscsoportok listáját az előfizetésében hello segítségével `Get-AzureRmResourceGroup` parancsmag.
2. Hozzon létre egy új Azure Recovery Services-tároló az alábbiak szerint:

        $vault = New-AzureRmRecoveryServicesVault -Name <string> -ResourceGroupName <string> -Location <string>

    Hello segítségével kérheti le a meglévő tárolók listája `Get-AzureRmRecoveryServicesVault` parancsmag.

> [!NOTE]
> Ha a klasszikus portál hello vagy hello Azure Service Management PowerShell-modul segítségével létrehozni a Site Recovery-tárolók tooperform műveleteket, hello segítségével kérheti le az ilyen tárolók listája `Get-AzureRmSiteRecoveryVault` parancsmag. Hozzon létre egy új Recovery Services-tároló minden új műveletnél. korábban létrehozott hello Site Recovery tárolók támogatottak, de nem rendelkezik a legújabb funkciókat hello.
>
>

## <a name="step-3-set-hello-recovery-services-vault-context"></a>3. lépés: Hello Recovery Services-tároló környezet beállítása
1. Hello tároló környezet beállítása hello a következő parancs futtatásával:

       Set-AzureRmSiteRecoveryVaultSettings -ARSVault $vault

## <a name="step-4-create-a-hyper-v-site-and-generate-a-new-vault-registration-key-for-hello-site"></a>4. lépés: Hyper-V-hely létrehozása, és hozzon létre egy új tároló regisztrációs kulcsot hello helyhez.
1. Hozzon létre egy új Hyper-V hely az alábbiak szerint:

        $sitename = "MySite"                #Specify site friendly name
        New-AzureRmSiteRecoverySite -Name $sitename

    Ez a parancsmag elindítja a Site Recovery feladat toocreate hello helyet, és a Site Recovery feladat objektum beállítása/beolvasása. Hello feladat toocomplete várja meg, és győződjön meg arról, hogy hello feladat sikeresen befejeződött.

    Hello feladatobjektum beolvashatja, és ezáltal állapotának hello aktuális hello feladat hello Get-AzureRmSiteRecoveryJob parancsmag használatával.
2. Létrehozni, és töltse le a regisztrációs kulcs hello helyhez, az alábbiak szerint:

        $SiteIdentifier = Get-AzureRmSiteRecoverySite -Name $sitename | Select -ExpandProperty SiteIdentifier
        Get-AzureRmRecoveryServicesVaultSettingsFile -Vault $vault -SiteIdentifier $SiteIdentifier -SiteFriendlyName $sitename -Path $Path

    Másolás hello letöltött kulcs toohello Hyper-V gazdagépen. Hello kulcs tooregister hello Hyper-V gazdagép toohello hely van szüksége.

## <a name="step-5-install-hello-azure-site-recovery-provider-and-azure-recovery-services-agent-on-your-hyper-v-host"></a>5. lépés: Hello Azure Site Recovery provider és az Azure Recovery Services Agent telepítése a Hyper-V gazdagépen
1. Hello telepítő hello hello szolgáltató legújabb verziójának letöltése [Microsoft](https://aka.ms/downloaddra).
2. A Hyper-V gazdagépen, és hello végén lévő hello telepítés futtatási hello telepítő toohello regisztrációs lépésében továbbra is.
3. Amikor a rendszer kéri, adja meg a hello letöltött hely regisztrációs kulcsot, és hello Hyper-V gazdagép toohello hely regisztráció elvégzésére.
4. Győződjön meg arról, hogy hello Hyper-V gazdagép regisztrált toohello hely hello a következő parancs használatával:

        $server =  Get-AzureRmSiteRecoveryServer -FriendlyName $server-friendlyname

## <a name="step-6-create-a-replication-policy-and-associate-it-with-hello-protection-container"></a>6. lépés: Hozzon létre egy replikációs házirendet, és társítsa azt az üdvözlő védelmi tároló
1. Egy replikációs házirend létrehozásához az alábbiak szerint:

        $ReplicationFrequencyInSeconds = "300";        #options are 30,300,900
        $PolicyName = “replicapolicy”
        $Recoverypoints = 6                    #specify hello number of recovery points
        $storageaccountID = Get-AzureRmStorageAccount -Name "mystorea" -ResourceGroupName "MyRG" | Select -ExpandProperty Id

        $PolicyResult = New-AzureRmSiteRecoveryPolicy -Name $PolicyName -ReplicationProvider “HyperVReplicaAzure” -ReplicationFrequencyInSeconds $ReplicationFrequencyInSeconds  -RecoveryPoints $Recoverypoints -ApplicationConsistentSnapshotFrequencyInHours 1 -RecoveryAzureStorageAccountId $storageaccountID

    Jelölőnégyzet hello visszaadott feladat tooensure hello replikációs házirend létrehozása sikeres.

   > [!IMPORTANT]
   > hello megadott tárfiók kell lennie a hello és a Recovery Services-tárolónak ugyanazon Azure-régiót, és rendelkeznie kell a georeplikáció engedélyezve van.
   >
   > * Hello tárolási fiók típusa (klasszikus) Azure Storage nem helyreállítási megadása esetén a hello feladatainak átvétele védett gépek helyreállítás hello gép tooAzure IaaS (klasszikus).
   > * Hello megadott helyreállítási tárfiók típusú Azure Storage (az Azure Resource Manager), ha a hello feladatainak átvétele védett gépek helyreállítás hello gép tooAzure IaaS (Azure Resource Manager).
   >
   >
2. Hello védelmi tároló megfelelő toohello helyen, töltse le a következőképpen:

        $protectionContainer = Get-AzureRmSiteRecoveryProtectionContainer
3. Kezdje hello társítás hello védelmi tároló hello replikációs házirend, az alábbiak szerint:

     $Policy = get-AzureRmSiteRecoveryPolicy - FriendlyName $PolicyName $associationJob = a Start-AzureRmSiteRecoveryPolicyAssociationJob-házirend $Policy - PrimaryProtectionContainer $protectionContainer

   Hello társítás feladat toocomplete várja meg, és győződjön meg arról, hogy sikeresen befejeződött-e.

## <a name="step-7-enable-protection-for-virtual-machines"></a>7. lépés: Virtuális gépek védelmének engedélyezése
1. Hello védelmi entitás megfelelő toohello tooprotect, az alábbiak szerint kívánt virtuális gép beolvasása:

        $VMFriendlyName = "Fabrikam-app"                    #Name of hello VM
        $protectionEntity = Get-AzureRmSiteRecoveryProtectionEntity -ProtectionContainer $protectionContainer -FriendlyName $VMFriendlyName
2. Hello virtuális gépet, az alábbiak szerint védelmének megkezdéséhez:

        $Ostype = "Windows"                                 # "Windows" or "Linux"
        $DRjob = Set-AzureRmSiteRecoveryProtectionEntity -ProtectionEntity $protectionEntity -Policy $Policy -Protection Enable -RecoveryAzureStorageAccountId $storageaccountID  -OS $OStype -OSDiskName $protectionEntity.Disks[0].Name

   > [!IMPORTANT]
   > hello megadott tárfiók kell lennie a hello és a Recovery Services-tárolónak ugyanazon Azure-régiót, és rendelkeznie kell a georeplikáció engedélyezve van.
   >
   > * Hello tárolási fiók típusa (klasszikus) Azure Storage nem helyreállítási megadása esetén a hello feladatainak átvétele védett gépek helyreállítás hello gép tooAzure IaaS (klasszikus).
   > * Hello megadott helyreállítási tárfiók típusú Azure Storage (az Azure Resource Manager), ha a hello feladatainak átvétele védett gépek helyreállítás hello gép tooAzure IaaS (Azure Resource Manager).
   >
   > Ha egynél több tooit csatlakoztatott lemez, adja meg a hello operációsrendszer-lemez hello használatával védett virtuális gép hello tartalmaz *OSDiskName* paraméter.
   >
   >
3. Várja meg a virtuális gépek tooreach hello védett állapotban hello kezdeti replikálás után. Ez eltarthat egy ideig, például a replikált adatok toobe hello mennyisége tényezőktől függően és felsőbb rétegbeli sávszélességgel tooAzure hello. hello feladatállapotot és StateDescription frissítve lett, az alábbiak szerint hello virtuális gép védett állapotban elérése során.

        PS C:\> $DRjob = Get-AzureRmSiteRecoveryJob -Job $DRjob

        PS C:\> $DRjob | Select-Object -ExpandProperty State
        Succeeded

        PS C:\> $DRjob | Select-Object -ExpandProperty StateDescription
        Completed
4. Helyreállítási tulajdonságait, például Virtuálisgép-szerepkör méretéhez hello és hello Azure hálózati tooattach hello virtuális gép hálózati illesztő kártyák tooupon feladatátvételi frissítése.

        PS C:\> $nw1 = Get-AzureRmVirtualNetwork -Name "FailoverNw" -ResourceGroupName "MyRG"

        PS C:\> $VMFriendlyName = "Fabrikam-App"

        PS C:\> $VM = Get-AzureRmSiteRecoveryVM -ProtectionContainer $protectionContainer -FriendlyName $VMFriendlyName

        PS C:\> $UpdateJob = Set-AzureRmSiteRecoveryVM -VirtualMachine $VM -PrimaryNic $VM.NicDetailsList[0].NicId -RecoveryNetworkId $nw1.Id -RecoveryNicSubnetName $nw1.Subnets[0].Name

        PS C:\> $UpdateJob = Get-AzureRmSiteRecoveryJob -Job $UpdateJob

        PS C:\> $UpdateJob

        Name             : b8a647e0-2cb9-40d1-84c4-d0169919e2c5
        ID               : /Subscriptions/a731825f-4bf2-4f81-a611-c331b272206e/resourceGroups/MyRG/providers/Microsoft.RecoveryServices/vault
                           s/MyVault/replicationJobs/b8a647e0-2cb9-40d1-84c4-d0169919e2c5
        Type             : Microsoft.RecoveryServices/vaults/replicationJobs
        JobType          : UpdateVmProperties
        DisplayName      : Update hello virtual machine
        ClientRequestId  : 805a22a3-be86-441c-9da8-f32685673112-2015-12-10 17:55:51Z-P
        State            : Succeeded
        StateDescription : Completed
        StartTime        : 10-12-2015 17:55:53 +00:00
        EndTime          : 10-12-2015 17:55:54 +00:00
        TargetObjectId   : 289682c6-c5e6-42dc-a1d2-5f9621f78ae6
        TargetObjectType : ProtectionEntity
        TargetObjectName : Fabrikam-App
        AllowedActions   : {Restart}
        Tasks            : {UpdateVmPropertiesTask}
        Errors           : {}



## <a name="step-8-run-a-test-failover"></a>8. lépés: Feladatátvételi teszt futtatása
1. A tesztfeladat feladatátvételt futtassa a következőképpen:

        $nw = Get-AzureRmVirtualNetwork -Name "TestFailoverNw" -ResourceGroupName "MyRG" #Specify Azure vnet name and resource group

        $protectionEntity = Get-AzureRmSiteRecoveryProtectionEntity -FriendlyName $VMFriendlyName -ProtectionContainer $protectionContainer

        $TFjob = Start-AzureRmSiteRecoveryTestFailoverJob -ProtectionEntity $protectionEntity -Direction PrimaryToRecovery -AzureVMNetworkId $nw.Id
2. Győződjön meg arról, hogy hello teszt virtuális gép létrehozása az Azure-ban. (hello tesztfeladat feladatátvételt fel van függesztve, az Azure-ban hello teszteléshez használt virtuális gép létrehozása után. hello feladat befejeződik által létrehozott hello vakpróbát hello feladat folytatása után törölje a hello következő lépésben ismertetett módon.)
3. Teljes hello tesztelni, az alábbiak szerint:

        $TFjob = Resume-AzureRmSiteRecoveryJob -Job $TFjob

## <a name="next-steps"></a>Következő lépések
[További](https://msdn.microsoft.com/library/azure/mt637930.aspx) Azure Site Recovery Azure Resource Manager PowerShell-parancsmagokkal kapcsolatos.
