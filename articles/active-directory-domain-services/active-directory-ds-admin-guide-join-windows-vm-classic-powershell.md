---
title: "Az Azure Active Directory tartományi szolgáltatások: Felügyeleti útmutató |} Microsoft Docs"
description: "Csatlakozás a Windows virtuális gép tooa felügyelt tartományhoz Azure PowerShell és a hello klasszikus telepítési modell használatával."
services: active-directory-ds
documentationcenter: 
author: mahesh-unnikrishnan
manager: stevenpo
editor: curtand
ms.assetid: 9143b843-7327-43c3-baab-6e24a18db25e
ms.service: active-directory-ds
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/06/2017
ms.author: maheshu
ms.openlocfilehash: 21bc5930d84c5368a120f9d81f09320e2a0168fb
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="join-a-windows-server-virtual-machine-tooa-managed-domain-using-powershell"></a>Csatlakozás a Windows Server virtuális gép tooa felügyelt tartományhoz PowerShell használatával
> [!div class="op_single_selector"]
> * [Klasszikus Azure portál – Windows](active-directory-ds-admin-guide-join-windows-vm.md)
> * [PowerShell - Windows](active-directory-ds-admin-guide-join-windows-vm-classic-powershell.md)
>
>

<br>

> [!IMPORTANT]
> Az Azure két különböző üzembe helyezési modellel rendelkezik az erőforrások létrehozásához és használatához: [Resource Manager és klasszikus](../azure-resource-manager/resource-manager-deployment-model.md). Ez a cikk hello klasszikus telepítési modell használatát bemutatja. Azure AD tartományi szolgáltatások jelenleg nem támogatja a hello Resource Manager modellt.
>
>

Ezen lépések bemutatják, hogyan toocustomize Azure PowerShell készlete parancsok, amelyek létrehozása, és előre konfigurálása a Windows-alapú Azure virtuális gép egy építőelem módszer használatával. Az alábbi lépések segítségével egy Windows-alapú Azure virtuális gép létrehozása és tooan Azure AD tartományi szolgáltatások által kezelt tartomány csatlakozzon hozzá.

Ezeket a lépéseket hajtsa végre az Azure PowerShell-parancs készletek létrehozásához a fill-a-a-üres cellákat megközelítést. Ezt a módszert akkor lehet hasznos, ha új tooPowerShell vagy kívánt tooknow milyen értékeket toospecify sikeres konfigurációhoz. Speciális PowerShell felhasználók hello parancsok igénybe, és a hello változók (hello sorok kezdve a "$") a saját értékeit helyettesítse.

Ha még nem tette meg, használja a hello utasításait [hogyan tooinstall és konfigurálja az Azure Powershellt](/powershell/azure/overview) tooinstall Azure PowerShell, a helyi számítógépen. Ezután nyissa meg a Windows PowerShell-parancssort.

## <a name="step-1-add-your-account"></a>1. lépés: A fiók hozzáadása
1. Hello PowerShell parancssorába írja be a **Add-AzureAccount** kattintson **Enter**.
2. Írja be az Azure-előfizetéshez társított hello e-mail címét, és kattintson a **Folytatás**.
3. Adja meg a fiókhoz tartozó jelszót hello.
4. Kattintson a **bejelentkezés**.

## <a name="step-2-set-your-subscription-and-storage-account"></a>2. lépés: Az előfizetés és a storage-fiók beállítása
Ezek a parancsok futtatásával hello Windows PowerShell parancssorába állítsa be a Azure-előfizetés és a storage-fiók. Cserélje le mindent, ami hello idézőjelek között, beleértve a hello < és > karakter, helyes nevek hello.

    $subscr="<subscription name>"
    $staccount="<storage account name>"
    Select-AzureSubscription -SubscriptionName $subscr –Current
    Set-AzureSubscription -SubscriptionName $subscr -CurrentStorageAccountName $staccount

Hello megfelelő előfizetés neve letölthető hello hello hello kimenete SubscriptionName tulajdonságának **Get-AzureSubscription** parancsot. Hello helyes-e a tárfiók nevének beolvasása hello hello hello kimenete Label tulajdonságának **Get-AzureStorageAccount** parancsot, miután lefuttatta a hello **válasszon-AzureSubscription** parancsot.

## <a name="step-3-step-by-step-walkthrough---provision-hello-virtual-machine-and-join-it-toohello-managed-domain"></a>3. lépés: Részletes útmutató - rendszerű hello virtuális gép, és hozzá toohello által felügyelt tartományokhoz
Ez hello megfelelő Azure PowerShell paranccsal állítsa toocreate a virtuális gépre, az üres sorok közötti valamennyi blokkja olvashatóság érdekében.

Adjon meg információt hello Windows virtuális gép toobe kiépítve.

    $family="Windows Server 2012 R2 Datacenter"
    $vmname="Contoso100-test"
    $vmsize="ExtraSmall"

Hello InstanceSize D-, DS- vagy értékeit G sorozatú virtuális gépek, lásd: [virtuális gépek és Felhőszolgáltatások mérete az Azure-](https://msdn.microsoft.com/library/azure/dn197896.aspx).

Hello által kezelt tartomány ismertetik.

    $domaindns="contoso100.com"
    $domacctdomain="contoso100"

Adja meg a felhőalapú szolgáltatás hello hello nevét.

    $svcname="Contoso100-test"

Adja meg hello hello virtuális hálózati toowhich hello kell csatlakoztatni a virtuális gép nevét. Győződjön meg arról, hogy hello felügyelt AAD-DS-tartomány érhető el a virtuális hálózaton található.

    $vnetname="MyPreviewVnet"

Válassza ki a virtuális gép lemezképének használt toobe tooprovision hello VM hello.

    $image=Get-AzureVMImage | where { $_.ImageFamily -eq $family } | sort PublishedDate -Descending | select -ExpandProperty ImageName -First 1

Konfigurálja a VM - VM neve, hello használt példány méret és a lemezkép toobe.

    $vm1=New-AzureVMConfig -Name $vmname -InstanceSize $vmsize -ImageName $image

Szerezze be a virtuális gép hello helyi rendszergazdai hitelesítő adatokat. Írjon be egy erős helyi rendszergazdai jelszót.

    $localadmincred=Get-Credential –Message "Type hello name and password of hello local administrator account."

Szerezze be tartozó too'AAD DC rendszergazdák csoport toojoin VM toohello kezelt tartományi felhasználói fiók hitelesítő adatait. Ne adjon hello tartománynév - példány, a jelen példában adja meg, hogy "Belinszky" hello felhasználónévként.

    $domainadmincred=Get-Credential –Message "Now type hello name (DO NOT INCLUDE hello DOMAIN) and password of an account in hello AAD DC Administrators group, that has permission tooadd hello machine toohello domain."

Konfigurálja a virtuális gép hello – adja meg a tartományhoz csatlakoztatás követelmény & szükséges hitelesítő adatokat.

    $vm1 | Add-AzureProvisioningConfig -AdminUsername $localadmincred.Username -Password $localadmincred.GetNetworkCredential().Password -WindowsDomain -Domain $domacctdomain -DomainUserName $domainadmincred.Username -DomainPassword $domainadmincred.GetNetworkCredential().Password -JoinDomain $domaindns

Állítsa be a hello Virtuálisgép-alhálózatot.

    $vm1 | Set-AzureSubnet -SubnetNames "Subnet-1"

Választható lehetőség: Pont toohello IP-cím hello tartomány. Ha IP-címei hello hello Azure AD tartományi szolgáltatások által kezelt tartomány toobe hello hello virtuális hálózat DNS-kiszolgálók, a lépésre nincs szükség.

    $dns = New-AzureDns -Name 'contoso100-dc1' -IPAddress '10.0.0.4'

Most kiépítés hello tartományhoz csatlakozó windowsos virtuális gép.

    New-AzureVM –ServiceName $svcname -VMs $vm1 -VNetName $vnetname -Location "Central US" -DnsSettings $dns

<br>

## <a name="script-tooprovision-a-windows-vm-and-automatically-join-it-tooan-aad-domain-services-managed-domain"></a>Parancsfájl-tooprovision egy Windows virtuális Gépet, és automatikusan hozzá tooan AAD tartományi szolgáltatásokra által felügyelt tartományokhoz
A PowerShell parancskészlethez létrehoz egy virtuális gépet egy üzleti kiszolgáló, amely:

* Hello Windows Server 2012 R2 Datacenter rendszerképet használja.
* Ez egy nagyon kicsi virtuális gép.
* Contoso100-teszt hello névvel rendelkezik.
* A rendszer automatikusan tartományhoz csatlakoztatott toohello contoso100 által felügyelt tartományokhoz.
* Toohello hozzáadott hello megegyező virtuális hálózatban felügyelt tartomány.

Ez a teljes minta parancsprogram toocreate hello Windows rendszerű virtuális gép hello, és automatikusan hozzá toohello Azure AD tartományi szolgáltatások által felügyelt tartományokhoz.

    $family="Windows Server 2012 R2 Datacenter"
    $vmname="Contoso100-test"
    $vmsize="ExtraSmall"

    $domaindns="contoso100.com"
    $domacctdomain="contoso100"

    $svcname="Contoso100-test"
    $vnetname="MyPreviewVnet"

    $image=Get-AzureVMImage | where { $_.ImageFamily -eq $family } | sort PublishedDate -Descending | select -ExpandProperty ImageName -First 1

    $vm1=New-AzureVMConfig -Name $vmname -InstanceSize $vmsize -ImageName $image

    $localadmincred=Get-Credential –Message "Type hello name and password of hello local administrator account."

    $domainadmincred=Get-Credential –Message "Now type hello name (not including hello domain) and password of an account in hello AAD DC Administrators group, that has permission tooadd hello machine toohello domain."

    $vm1 | Add-AzureProvisioningConfig -AdminUsername $localadmincred.Username -Password $localadmincred.GetNetworkCredential().Password -WindowsDomain -Domain $domacctdomain -DomainUserName $domainadmincred.Username -DomainPassword $domainadmincred.GetNetworkCredential().Password -JoinDomain $domaindns

    $vm1 | Set-AzureSubnet -SubnetNames "Subnet-1"

    $dns = New-AzureDns -Name 'contoso100-dc1' -IPAddress '10.0.0.4'

    New-AzureVM –ServiceName $svcname -VMs $vm1 -VNetName $vnetname -Location "Central US" -DnsSettings $dns

<br>

## <a name="related-content"></a>Kapcsolódó tartalom
* [Azure AD tartományi szolgáltatások – első lépések útmutató](active-directory-ds-getting-started.md)
* [Azure AD tartományi szolgáltatások által kezelt tartomány felügyelete](active-directory-ds-admin-guide-administer-domain.md)
