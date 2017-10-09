---
title: "virtuális gép (klasszikus) és több hálózati adapter - Azure PowerShell aaaCreate |} Microsoft Docs"
description: "Megtudhatja, hogyan toocreate PowerShell segítségével több hálózati adapterrel rendelkező virtuális gép (klasszikus)."
services: virtual-network
documentationcenter: na
author: jimdial
manager: timlt
editor: 
tags: azure-service-management
ms.assetid: 6e50f39a-2497-4845-a5d4-7332dbc203c5
ms.service: virtual-network
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 02/02/2016
ms.author: jdial
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 90c967929bb418042c3fb7079e0f69246faac53c
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="create-a-vm-classic-with-multiple-nics-using-powershell"></a>PowerShell-lel több hálózati adapterrel rendelkező virtuális gép (klasszikus) létrehozása

[!INCLUDE [virtual-network-deploy-multinic-classic-selectors-include.md](../../includes/virtual-network-deploy-multinic-classic-selectors-include.md)]

Virtuális gépek (VM) létrehozása az Azure-ban, és csatlakoztassa a virtuális gépek több hálózati adapterek (NIC) tooeach. Több hálózati adapter választhatók szét a forgalomtípusok engedélyezze a hálózati adapter között. Például egy hálózati adapter kommunikálhat a hello Internet, miközben egy másik kommunikál csak a belső erőforrásokhoz nem kapcsolódó toohello Internet. hello képességét tooseparate hálózati forgalom több hálózati adapter között számos hálózati virtuális készülékeket, például az alkalmazások biztosításán és WAN-optimalizálást megoldások szükség.

> [!IMPORTANT]
> Az Azure két különböző üzembe helyezési modellel rendelkezik az erőforrások létrehozásához és használatához: [Resource Manager és klasszikus](../resource-manager-deployment-model.md). Ez a cikk hello klasszikus telepítési modell használatát bemutatja. A Microsoft azt javasolja, hogy az új telepítések esetén hello Resource Manager modellt használja. Megtudhatja, hogyan tooperform hello használata a lépések [Resource Manager üzembe helyezési modellben](virtual-network-deploy-multinic-arm-ps.md).

[!INCLUDE [virtual-network-deploy-multinic-scenario-include.md](../../includes/virtual-network-deploy-multinic-scenario-include.md)]

hello következő lépések használják nevű erőforráscsoport *IaaSStory* hello webkiszolgálók és az erőforráscsoport neve *IaaSStory-háttérrendszer* hello DB kiszolgálók.

## <a name="prerequisites"></a>Előfeltételek

Mielőtt hello DB kiszolgálók hozhat létre, meg kell-e toocreate hello *IaaSStory* erőforráscsoport összes hello szükséges erőforrások ehhez a forgatókönyvhöz. Ezeket az erőforrásokat, befejeződött a következő lépések hello toocreate. Hozzon létre egy virtuális hálózatot hello hello utasításait követve [hozzon létre egy virtuális hálózatot](virtual-networks-create-vnet-classic-netcfg-ps.md) cikk.

[!INCLUDE [azure-ps-prerequisites-include.md](../../includes/azure-ps-prerequisites-include.md)]

## <a name="create-hello-back-end-vms"></a>Hello háttér virtuális gépek létrehozása
hello háttér virtuális gépek hello létrehozása a következő erőforrások hello függ:

* **Backend alhálózathoz**. hello adatbázis-kiszolgálókhoz külön alhálózathoz, toosegregate forgalom része lesz. hello az alábbi parancsfájl a alhálózati tooexist vár egy nevű vnetet *WTestVnet*.
* **Az adatlemezek tárfiók**. A jobb teljesítmény érdekében a hello adatbázis-kiszolgálóin hello adatlemezek tartós állapotú meghajtót (SSD) technológiát, amely a prémium szintű tárfiók szükséges fogja használni. Győződjön meg arról, hogy hello Azure-beli hely toosupport prémium szintű storage telepít.
* **A rendelkezésre állási csoport**. Minden adatbázis-kiszolgálók megkapja tooa egyetlen rendelkezésre állási csoportot, hello virtuális gépek közül legalább egy tooensure megfelelően működik, és karbantartás során.

### <a name="step-1---start-your-script"></a>1. lépés – a parancsfájl futtatásához
Letöltheti a hello használt teljes PowerShell parancsfájl [Itt](https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/IaaS-Story/11-MultiNIC/classic/virtual-network-deploy-multinic-classic-ps.ps1). Kövesse az alábbi toochange hello parancsfájl toowork környezetében hello lépéseket.

1. Hello hello-változók értékeit az alábbi a meglévő erőforráscsoport üzembe helyezett fent alapján módosítása [Előfeltételek](#Prerequisites).

    ```powershell
    $location              = "West US"
    $vnetName              = "WTestVNet"
    $backendSubnetName     = "BackEnd"
    ```

2. Hello értékek módosítása hello alábbi változók hello értékek alapján kívánt toouse a háttér-telepítéshez.

    ```powershell
    $backendCSName         = "IaaSStory-Backend"
    $prmStorageAccountName = "iaasstoryprmstorage"
    $avSetName             = "ASDB"
    $vmSize                = "Standard_DS3"
    $diskSize              = 127
    $vmNamePrefix          = "DB"
    $dataDiskSuffix        = "datadisk"
    $ipAddressPrefix       = "192.168.2."
    $numberOfVMs           = 2
    ```

### <a name="step-2---create-necessary-resources-for-your-vms"></a>2. lépés - a szükséges erőforrásokat létrehozni a virtuális géphez
Új felhőalapú szolgáltatás és a tárolási fiók hello adatlemezek virtuális toocreate van szüksége. Szüksége is toospecify lemezkép, és egy helyi rendszergazdai fiók hello virtuális gépeket. toocreate ezeket az erőforrásokat, végezze el hello a következő lépéseket:

1. Új felhőalapú szolgáltatás létrehozása.

    ```powershell
    New-AzureService -ServiceName $backendCSName -Location $location
    ```

2. Hozzon létre egy új prémium szintű storage-fiók.

    ```powershell
    New-AzureStorageAccount -StorageAccountName $prmStorageAccountName `
    -Location $location -Type Premium_LRS
    ```
3. Set hello létrehozott tárfiókban fent hello az előfizetéshez tartozó aktuális tárfiókkal.

    ```powershell
    $subscription = Get-AzureSubscription | where {$_.IsCurrent -eq $true}  
    Set-AzureSubscription -SubscriptionName $subscription.SubscriptionName `
    -CurrentStorageAccountName $prmStorageAccountName
    ```

4. Válassza ki egy hello VM lemezképfájlt.

    ```powershell
    $image = Get-AzureVMImage `
    | where{$_.ImageFamily -eq "SQL Server 2014 RTM Web on Windows Server 2012 R2"} `
    | sort PublishedDate -Descending `
    | select -ExpandProperty ImageName -First 1
    ```

5. Állítsa be a hello helyi rendszergazdai fiók hitelesítő adatait.

    ```powershell
    $cred = Get-Credential -Message "Enter username and password for local admin account"
    ```

### <a name="step-3---create-vms"></a>3. lépés – a virtuális gépek létrehozása
Szüksége van egy hurok toocreate toouse, sok virtuális gép, és hozzon létre hello szükséges hálózati adapterek és virtuális gépek hello hurkon belül. toocreate hello hálózati adapterek és virtuális gépek, hajtható végre a lépéseket követve hello.

1. Indítsa el a `for` hurok toorepeat hello parancsok toocreate egy virtuális Gépet, és két hálózati adaptert, szükség esetén hányszor hello az alapján hello `$numberOfVMs` változó.

    ```powershell
    for ($suffixNumber = 1; $suffixNumber -le $numberOfVMs; $suffixNumber++){
    ```

2. Hozzon létre egy `VMConfig` hello rendszerkép, méret és egyéb rendelkezésre állási készlet hello VM objektum.

    ```powershell
    $vmName = $vmNamePrefix + $suffixNumber
    $vmConfig = New-AzureVMConfig -Name $vmName `
        -ImageName $image `
        -InstanceSize $vmSize `
        -AvailabilitySetName $avSetName
    ```

3. Kiépítés hello VM a Windows virtuális gépként.

    ```powershell
    Add-AzureProvisioningConfig -VM $vmConfig -Windows `
        -AdminUsername $cred.UserName `
        -Password $cred.GetNetworkCredential().Password
    ```

4. Hello alapértelmezett hálózati adapter, és rendelje hozzá egy statikus IP-címet.

    ```powershell
    Set-AzureSubnet         -SubnetNames $backendSubnetName -VM $vmConfig
    Set-AzureStaticVNetIP   -IPAddress ($ipAddressPrefix+$suffixNumber+3) -VM $vmConfig
    ```

5. Adja hozzá a második hálózati adapter az egyes virtuális gépek.

    ```powershell
    Add-AzureNetworkInterfaceConfig -Name ("RemoteAccessNIC"+$suffixNumber) `
    -SubnetName $backendSubnetName `
    -StaticVNetIPAddress ($ipAddressPrefix+(53+$suffixNumber)) `
    -VM $vmConfig
    ```

6. Az egyes virtuális gépek toodata lemezek létrehozásához.

    ```powershell
    $dataDisk1Name = $vmName + "-" + $dataDiskSuffix + "-1"    
    Add-AzureDataDisk -CreateNew -VM $vmConfig `
    -DiskSizeInGB $diskSize `
    -DiskLabel $dataDisk1Name `
    -LUN 0

    $dataDisk2Name = $vmName + "-" + $dataDiskSuffix + "-2"   
    Add-AzureDataDisk -CreateNew -VM $vmConfig `
    -DiskSizeInGB $diskSize `
    -DiskLabel $dataDisk2Name `
    -LUN 1
    ```

7. Minden virtuális gép és a záró hello hurok létrehozása.

    ```powershell
    New-AzureVM -VM $vmConfig `
    -ServiceName $backendCSName `
    -Location $location `
    -VNetName $vnetName
    }
    ```

### <a name="step-4---run-hello-script"></a>4. lépés: hello parancsfájl futtatása
Most, hogy a letöltött és módosított hello parancsfájl igényei szerint, ő parancsfájl toocreate hello háttérbeli adatbázis több hálózati adapterrel rendelkező virtuális gépek runt.

1. Mentse a parancsfájlt, és futtassa a hello **PowerShell** parancssort, vagy **PowerShell ISE**. Hello kezdeti kimenetben alább látható módon jelenik meg.

        OperationDescription    OperationId                          OperationStatus

        New-AzureService        xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx Succeeded
        New-AzureStorageAccount xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx Succeeded
        
        WARNING: No deployment found in service: 'IaaSStory-Backend'.
2. Hello hitelesítő adatokat kér, és kattintson a szükséges hello adatok **OK**. az alábbi hello kimeneti adja vissza.

        New-AzureVM             xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx Succeeded
        New-AzureVM             xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx Succeeded
