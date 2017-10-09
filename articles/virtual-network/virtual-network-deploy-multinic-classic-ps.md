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
# <a name="create-a-vm-classic-with-multiple-nics-using-powershell"></a><span data-ttu-id="57b88-103">PowerShell-lel több hálózati adapterrel rendelkező virtuális gép (klasszikus) létrehozása</span><span class="sxs-lookup"><span data-stu-id="57b88-103">Create a VM (Classic) with multiple NICs using PowerShell</span></span>

[!INCLUDE [virtual-network-deploy-multinic-classic-selectors-include.md](../../includes/virtual-network-deploy-multinic-classic-selectors-include.md)]

<span data-ttu-id="57b88-104">Virtuális gépek (VM) létrehozása az Azure-ban, és csatlakoztassa a virtuális gépek több hálózati adapterek (NIC) tooeach.</span><span class="sxs-lookup"><span data-stu-id="57b88-104">You can create virtual machines (VMs) in Azure and attach multiple network interfaces (NICs) tooeach of your VMs.</span></span> <span data-ttu-id="57b88-105">Több hálózati adapter választhatók szét a forgalomtípusok engedélyezze a hálózati adapter között.</span><span class="sxs-lookup"><span data-stu-id="57b88-105">Multiple NICs enable separation of traffic types across NICs.</span></span> <span data-ttu-id="57b88-106">Például egy hálózati adapter kommunikálhat a hello Internet, miközben egy másik kommunikál csak a belső erőforrásokhoz nem kapcsolódó toohello Internet.</span><span class="sxs-lookup"><span data-stu-id="57b88-106">For example, one NIC might communicate with hello Internet, while another communicates only with internal resources not connected toohello Internet.</span></span> <span data-ttu-id="57b88-107">hello képességét tooseparate hálózati forgalom több hálózati adapter között számos hálózati virtuális készülékeket, például az alkalmazások biztosításán és WAN-optimalizálást megoldások szükség.</span><span class="sxs-lookup"><span data-stu-id="57b88-107">hello ability tooseparate network traffic across multiple NICs is required for many network virtual appliances, such as application delivery and WAN optimization solutions.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="57b88-108">Az Azure két különböző üzembe helyezési modellel rendelkezik az erőforrások létrehozásához és használatához: [Resource Manager és klasszikus](../resource-manager-deployment-model.md).</span><span class="sxs-lookup"><span data-stu-id="57b88-108">Azure has two different deployment models for creating and working with resources:  [Resource Manager and classic](../resource-manager-deployment-model.md).</span></span> <span data-ttu-id="57b88-109">Ez a cikk hello klasszikus telepítési modell használatát bemutatja.</span><span class="sxs-lookup"><span data-stu-id="57b88-109">This article covers using hello classic deployment model.</span></span> <span data-ttu-id="57b88-110">A Microsoft azt javasolja, hogy az új telepítések esetén hello Resource Manager modellt használja.</span><span class="sxs-lookup"><span data-stu-id="57b88-110">Microsoft recommends that most new deployments use hello Resource Manager model.</span></span> <span data-ttu-id="57b88-111">Megtudhatja, hogyan tooperform hello használata a lépések [Resource Manager üzembe helyezési modellben](virtual-network-deploy-multinic-arm-ps.md).</span><span class="sxs-lookup"><span data-stu-id="57b88-111">Learn how tooperform these steps using hello [Resource Manager deployment model](virtual-network-deploy-multinic-arm-ps.md).</span></span>

[!INCLUDE [virtual-network-deploy-multinic-scenario-include.md](../../includes/virtual-network-deploy-multinic-scenario-include.md)]

<span data-ttu-id="57b88-112">hello következő lépések használják nevű erőforráscsoport *IaaSStory* hello webkiszolgálók és az erőforráscsoport neve *IaaSStory-háttérrendszer* hello DB kiszolgálók.</span><span class="sxs-lookup"><span data-stu-id="57b88-112">hello following steps use a resource group named *IaaSStory* for hello WEB servers and a resource group named *IaaSStory-BackEnd* for hello DB servers.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="57b88-113">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="57b88-113">Prerequisites</span></span>

<span data-ttu-id="57b88-114">Mielőtt hello DB kiszolgálók hozhat létre, meg kell-e toocreate hello *IaaSStory* erőforráscsoport összes hello szükséges erőforrások ehhez a forgatókönyvhöz.</span><span class="sxs-lookup"><span data-stu-id="57b88-114">Before you can create hello DB servers, you need toocreate hello *IaaSStory* resource group with all hello necessary resources for this scenario.</span></span> <span data-ttu-id="57b88-115">Ezeket az erőforrásokat, befejeződött a következő lépések hello toocreate.</span><span class="sxs-lookup"><span data-stu-id="57b88-115">toocreate these resources, complete hello steps that follow.</span></span> <span data-ttu-id="57b88-116">Hozzon létre egy virtuális hálózatot hello hello utasításait követve [hozzon létre egy virtuális hálózatot](virtual-networks-create-vnet-classic-netcfg-ps.md) cikk.</span><span class="sxs-lookup"><span data-stu-id="57b88-116">Create a virtual network by following hello steps in hello [Create a virtual network](virtual-networks-create-vnet-classic-netcfg-ps.md) article.</span></span>

[!INCLUDE [azure-ps-prerequisites-include.md](../../includes/azure-ps-prerequisites-include.md)]

## <a name="create-hello-back-end-vms"></a><span data-ttu-id="57b88-117">Hello háttér virtuális gépek létrehozása</span><span class="sxs-lookup"><span data-stu-id="57b88-117">Create hello back-end VMs</span></span>
<span data-ttu-id="57b88-118">hello háttér virtuális gépek hello létrehozása a következő erőforrások hello függ:</span><span class="sxs-lookup"><span data-stu-id="57b88-118">hello back-end VMs depend on hello creation of hello following resources:</span></span>

* <span data-ttu-id="57b88-119">**Backend alhálózathoz**.</span><span class="sxs-lookup"><span data-stu-id="57b88-119">**Backend subnet**.</span></span> <span data-ttu-id="57b88-120">hello adatbázis-kiszolgálókhoz külön alhálózathoz, toosegregate forgalom része lesz.</span><span class="sxs-lookup"><span data-stu-id="57b88-120">hello database servers will be part of a separate subnet, toosegregate traffic.</span></span> <span data-ttu-id="57b88-121">hello az alábbi parancsfájl a alhálózati tooexist vár egy nevű vnetet *WTestVnet*.</span><span class="sxs-lookup"><span data-stu-id="57b88-121">hello script below expects this subnet tooexist in a vnet named *WTestVnet*.</span></span>
* <span data-ttu-id="57b88-122">**Az adatlemezek tárfiók**.</span><span class="sxs-lookup"><span data-stu-id="57b88-122">**Storage account for data disks**.</span></span> <span data-ttu-id="57b88-123">A jobb teljesítmény érdekében a hello adatbázis-kiszolgálóin hello adatlemezek tartós állapotú meghajtót (SSD) technológiát, amely a prémium szintű tárfiók szükséges fogja használni.</span><span class="sxs-lookup"><span data-stu-id="57b88-123">For better performance, hello data disks on hello database servers will use solid state drive (SSD) technology, which requires a premium storage account.</span></span> <span data-ttu-id="57b88-124">Győződjön meg arról, hogy hello Azure-beli hely toosupport prémium szintű storage telepít.</span><span class="sxs-lookup"><span data-stu-id="57b88-124">Make sure hello Azure location you deploy toosupport premium storage.</span></span>
* <span data-ttu-id="57b88-125">**A rendelkezésre állási csoport**.</span><span class="sxs-lookup"><span data-stu-id="57b88-125">**Availability set**.</span></span> <span data-ttu-id="57b88-126">Minden adatbázis-kiszolgálók megkapja tooa egyetlen rendelkezésre állási csoportot, hello virtuális gépek közül legalább egy tooensure megfelelően működik, és karbantartás során.</span><span class="sxs-lookup"><span data-stu-id="57b88-126">All database servers will be added tooa single availability set, tooensure at least one of hello VMs is up and running during maintenance.</span></span>

### <a name="step-1---start-your-script"></a><span data-ttu-id="57b88-127">1. lépés – a parancsfájl futtatásához</span><span class="sxs-lookup"><span data-stu-id="57b88-127">Step 1 - Start your script</span></span>
<span data-ttu-id="57b88-128">Letöltheti a hello használt teljes PowerShell parancsfájl [Itt](https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/IaaS-Story/11-MultiNIC/classic/virtual-network-deploy-multinic-classic-ps.ps1).</span><span class="sxs-lookup"><span data-stu-id="57b88-128">You can download hello full PowerShell script used [here](https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/IaaS-Story/11-MultiNIC/classic/virtual-network-deploy-multinic-classic-ps.ps1).</span></span> <span data-ttu-id="57b88-129">Kövesse az alábbi toochange hello parancsfájl toowork környezetében hello lépéseket.</span><span class="sxs-lookup"><span data-stu-id="57b88-129">Follow hello steps below toochange hello script toowork in your environment.</span></span>

1. <span data-ttu-id="57b88-130">Hello hello-változók értékeit az alábbi a meglévő erőforráscsoport üzembe helyezett fent alapján módosítása [Előfeltételek](#Prerequisites).</span><span class="sxs-lookup"><span data-stu-id="57b88-130">Change hello values of hello variables below based on your existing resource group deployed above in [Prerequisites](#Prerequisites).</span></span>

    ```powershell
    $location              = "West US"
    $vnetName              = "WTestVNet"
    $backendSubnetName     = "BackEnd"
    ```

2. <span data-ttu-id="57b88-131">Hello értékek módosítása hello alábbi változók hello értékek alapján kívánt toouse a háttér-telepítéshez.</span><span class="sxs-lookup"><span data-stu-id="57b88-131">Change hello values of hello variables below based on hello values you want toouse for your backend deployment.</span></span>

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

### <a name="step-2---create-necessary-resources-for-your-vms"></a><span data-ttu-id="57b88-132">2. lépés - a szükséges erőforrásokat létrehozni a virtuális géphez</span><span class="sxs-lookup"><span data-stu-id="57b88-132">Step 2 - Create necessary resources for your VMs</span></span>
<span data-ttu-id="57b88-133">Új felhőalapú szolgáltatás és a tárolási fiók hello adatlemezek virtuális toocreate van szüksége.</span><span class="sxs-lookup"><span data-stu-id="57b88-133">You need toocreate a new cloud service and a storage account for hello data disks for all VMs.</span></span> <span data-ttu-id="57b88-134">Szüksége is toospecify lemezkép, és egy helyi rendszergazdai fiók hello virtuális gépeket.</span><span class="sxs-lookup"><span data-stu-id="57b88-134">You also need toospecify an image, and a local administrator account for hello VMs.</span></span> <span data-ttu-id="57b88-135">toocreate ezeket az erőforrásokat, végezze el hello a következő lépéseket:</span><span class="sxs-lookup"><span data-stu-id="57b88-135">toocreate these resources, complete hello following steps:</span></span>

1. <span data-ttu-id="57b88-136">Új felhőalapú szolgáltatás létrehozása.</span><span class="sxs-lookup"><span data-stu-id="57b88-136">Create a new cloud service.</span></span>

    ```powershell
    New-AzureService -ServiceName $backendCSName -Location $location
    ```

2. <span data-ttu-id="57b88-137">Hozzon létre egy új prémium szintű storage-fiók.</span><span class="sxs-lookup"><span data-stu-id="57b88-137">Create a new premium storage account.</span></span>

    ```powershell
    New-AzureStorageAccount -StorageAccountName $prmStorageAccountName `
    -Location $location -Type Premium_LRS
    ```
3. <span data-ttu-id="57b88-138">Set hello létrehozott tárfiókban fent hello az előfizetéshez tartozó aktuális tárfiókkal.</span><span class="sxs-lookup"><span data-stu-id="57b88-138">Set hello storage account created above as hello current storage account for your subscription.</span></span>

    ```powershell
    $subscription = Get-AzureSubscription | where {$_.IsCurrent -eq $true}  
    Set-AzureSubscription -SubscriptionName $subscription.SubscriptionName `
    -CurrentStorageAccountName $prmStorageAccountName
    ```

4. <span data-ttu-id="57b88-139">Válassza ki egy hello VM lemezképfájlt.</span><span class="sxs-lookup"><span data-stu-id="57b88-139">Select an image for hello VM.</span></span>

    ```powershell
    $image = Get-AzureVMImage `
    | where{$_.ImageFamily -eq "SQL Server 2014 RTM Web on Windows Server 2012 R2"} `
    | sort PublishedDate -Descending `
    | select -ExpandProperty ImageName -First 1
    ```

5. <span data-ttu-id="57b88-140">Állítsa be a hello helyi rendszergazdai fiók hitelesítő adatait.</span><span class="sxs-lookup"><span data-stu-id="57b88-140">Set hello local administrator account credentials.</span></span>

    ```powershell
    $cred = Get-Credential -Message "Enter username and password for local admin account"
    ```

### <a name="step-3---create-vms"></a><span data-ttu-id="57b88-141">3. lépés – a virtuális gépek létrehozása</span><span class="sxs-lookup"><span data-stu-id="57b88-141">Step 3 - Create VMs</span></span>
<span data-ttu-id="57b88-142">Szüksége van egy hurok toocreate toouse, sok virtuális gép, és hozzon létre hello szükséges hálózati adapterek és virtuális gépek hello hurkon belül.</span><span class="sxs-lookup"><span data-stu-id="57b88-142">You need toouse a loop toocreate as many VMs as you want, and create hello necessary NICs and VMs within hello loop.</span></span> <span data-ttu-id="57b88-143">toocreate hello hálózati adapterek és virtuális gépek, hajtható végre a lépéseket követve hello.</span><span class="sxs-lookup"><span data-stu-id="57b88-143">toocreate hello NICs and VMs, execute hello following steps.</span></span>

1. <span data-ttu-id="57b88-144">Indítsa el a `for` hurok toorepeat hello parancsok toocreate egy virtuális Gépet, és két hálózati adaptert, szükség esetén hányszor hello az alapján hello `$numberOfVMs` változó.</span><span class="sxs-lookup"><span data-stu-id="57b88-144">Start a `for` loop toorepeat hello commands toocreate a VM and two NICs as many times as necessary, based on hello value of hello `$numberOfVMs` variable.</span></span>

    ```powershell
    for ($suffixNumber = 1; $suffixNumber -le $numberOfVMs; $suffixNumber++){
    ```

2. <span data-ttu-id="57b88-145">Hozzon létre egy `VMConfig` hello rendszerkép, méret és egyéb rendelkezésre állási készlet hello VM objektum.</span><span class="sxs-lookup"><span data-stu-id="57b88-145">Create a `VMConfig` object specifying hello image, size, and availability set for hello VM.</span></span>

    ```powershell
    $vmName = $vmNamePrefix + $suffixNumber
    $vmConfig = New-AzureVMConfig -Name $vmName `
        -ImageName $image `
        -InstanceSize $vmSize `
        -AvailabilitySetName $avSetName
    ```

3. <span data-ttu-id="57b88-146">Kiépítés hello VM a Windows virtuális gépként.</span><span class="sxs-lookup"><span data-stu-id="57b88-146">Provision hello VM as a Windows VM.</span></span>

    ```powershell
    Add-AzureProvisioningConfig -VM $vmConfig -Windows `
        -AdminUsername $cred.UserName `
        -Password $cred.GetNetworkCredential().Password
    ```

4. <span data-ttu-id="57b88-147">Hello alapértelmezett hálózati adapter, és rendelje hozzá egy statikus IP-címet.</span><span class="sxs-lookup"><span data-stu-id="57b88-147">Set hello default NIC and assign it a static IP address.</span></span>

    ```powershell
    Set-AzureSubnet         -SubnetNames $backendSubnetName -VM $vmConfig
    Set-AzureStaticVNetIP   -IPAddress ($ipAddressPrefix+$suffixNumber+3) -VM $vmConfig
    ```

5. <span data-ttu-id="57b88-148">Adja hozzá a második hálózati adapter az egyes virtuális gépek.</span><span class="sxs-lookup"><span data-stu-id="57b88-148">Add a second NIC for each VM.</span></span>

    ```powershell
    Add-AzureNetworkInterfaceConfig -Name ("RemoteAccessNIC"+$suffixNumber) `
    -SubnetName $backendSubnetName `
    -StaticVNetIPAddress ($ipAddressPrefix+(53+$suffixNumber)) `
    -VM $vmConfig
    ```

6. <span data-ttu-id="57b88-149">Az egyes virtuális gépek toodata lemezek létrehozásához.</span><span class="sxs-lookup"><span data-stu-id="57b88-149">Create toodata disks for each VM.</span></span>

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

7. <span data-ttu-id="57b88-150">Minden virtuális gép és a záró hello hurok létrehozása.</span><span class="sxs-lookup"><span data-stu-id="57b88-150">Create each VM, and end hello loop.</span></span>

    ```powershell
    New-AzureVM -VM $vmConfig `
    -ServiceName $backendCSName `
    -Location $location `
    -VNetName $vnetName
    }
    ```

### <a name="step-4---run-hello-script"></a><span data-ttu-id="57b88-151">4. lépés: hello parancsfájl futtatása</span><span class="sxs-lookup"><span data-stu-id="57b88-151">Step 4 - Run hello script</span></span>
<span data-ttu-id="57b88-152">Most, hogy a letöltött és módosított hello parancsfájl igényei szerint, ő parancsfájl toocreate hello háttérbeli adatbázis több hálózati adapterrel rendelkező virtuális gépek runt.</span><span class="sxs-lookup"><span data-stu-id="57b88-152">Now that you downloaded and changed hello script based on your needs, runt he script toocreate hello back end database VMs with multiple NICs.</span></span>

1. <span data-ttu-id="57b88-153">Mentse a parancsfájlt, és futtassa a hello **PowerShell** parancssort, vagy **PowerShell ISE**.</span><span class="sxs-lookup"><span data-stu-id="57b88-153">Save your script and run it from hello **PowerShell** command prompt, or **PowerShell ISE**.</span></span> <span data-ttu-id="57b88-154">Hello kezdeti kimenetben alább látható módon jelenik meg.</span><span class="sxs-lookup"><span data-stu-id="57b88-154">You will see hello initial output, as shown below.</span></span>

        OperationDescription    OperationId                          OperationStatus

        New-AzureService        xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx Succeeded
        New-AzureStorageAccount xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx Succeeded
        
        WARNING: No deployment found in service: 'IaaSStory-Backend'.
2. <span data-ttu-id="57b88-155">Hello hitelesítő adatokat kér, és kattintson a szükséges hello adatok **OK**.</span><span class="sxs-lookup"><span data-stu-id="57b88-155">Fill out hello information needed in hello credentials prompt and click **OK**.</span></span> <span data-ttu-id="57b88-156">az alábbi hello kimeneti adja vissza.</span><span class="sxs-lookup"><span data-stu-id="57b88-156">hello output below is returned.</span></span>

        New-AzureVM             xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx Succeeded
        New-AzureVM             xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx Succeeded
