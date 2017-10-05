---
title: "Hozzon létre egy virtuális gép (klasszikus) több hálózati adapter - Azure PowerShell |} Microsoft Docs"
description: "Tudnivalók a PowerShell segítségével több hálózati adapterrel rendelkező virtuális gép (klasszikus) létrehozása."
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
ms.openlocfilehash: 923d4817d96399fc423b0a89cbf88f8d397f1af0
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/03/2017
---
# <a name="create-a-vm-classic-with-multiple-nics-using-powershell"></a><span data-ttu-id="c2431-103">PowerShell-lel több hálózati adapterrel rendelkező virtuális gép (klasszikus) létrehozása</span><span class="sxs-lookup"><span data-stu-id="c2431-103">Create a VM (Classic) with multiple NICs using PowerShell</span></span>

[!INCLUDE [virtual-network-deploy-multinic-classic-selectors-include.md](../../includes/virtual-network-deploy-multinic-classic-selectors-include.md)]

<span data-ttu-id="c2431-104">Virtuális gépek (VM) létrehozása az Azure-ban, és csatlakoztassa a virtuális gépek mindegyikének több hálózati adapterek (NIC).</span><span class="sxs-lookup"><span data-stu-id="c2431-104">You can create virtual machines (VMs) in Azure and attach multiple network interfaces (NICs) to each of your VMs.</span></span> <span data-ttu-id="c2431-105">Több hálózati adapter választhatók szét a forgalomtípusok engedélyezze a hálózati adapter között.</span><span class="sxs-lookup"><span data-stu-id="c2431-105">Multiple NICs enable separation of traffic types across NICs.</span></span> <span data-ttu-id="c2431-106">Például egy hálózati adapter előfordulhat, hogy az internetes kommunikációt folytató, miközben egy másik kommunikál, csak a belső erőforrások nem csatlakozik az internethez.</span><span class="sxs-lookup"><span data-stu-id="c2431-106">For example, one NIC might communicate with the Internet, while another communicates only with internal resources not connected to the Internet.</span></span> <span data-ttu-id="c2431-107">Azon különálló hálózati forgalom több hálózati adapter között számos hálózati virtuális készülékeket, például az alkalmazások biztosításán és WAN-optimalizálást megoldások szükség.</span><span class="sxs-lookup"><span data-stu-id="c2431-107">The ability to separate network traffic across multiple NICs is required for many network virtual appliances, such as application delivery and WAN optimization solutions.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="c2431-108">Az Azure két különböző üzembe helyezési modellel rendelkezik az erőforrások létrehozásához és használatához: [Resource Manager és klasszikus](../resource-manager-deployment-model.md).</span><span class="sxs-lookup"><span data-stu-id="c2431-108">Azure has two different deployment models for creating and working with resources:  [Resource Manager and classic](../resource-manager-deployment-model.md).</span></span> <span data-ttu-id="c2431-109">Ez a cikk a klasszikus üzembehelyezési modellt ismerteti.</span><span class="sxs-lookup"><span data-stu-id="c2431-109">This article covers using the classic deployment model.</span></span> <span data-ttu-id="c2431-110">A Microsoft azt javasolja, hogy az új telepítések esetén a Resource Manager modellt használja.</span><span class="sxs-lookup"><span data-stu-id="c2431-110">Microsoft recommends that most new deployments use the Resource Manager model.</span></span> <span data-ttu-id="c2431-111">Útmutató: a következő lépések segítségével a [Resource Manager üzembe helyezési modellben](virtual-network-deploy-multinic-arm-ps.md).</span><span class="sxs-lookup"><span data-stu-id="c2431-111">Learn how to perform these steps using the [Resource Manager deployment model](virtual-network-deploy-multinic-arm-ps.md).</span></span>

[!INCLUDE [virtual-network-deploy-multinic-scenario-include.md](../../includes/virtual-network-deploy-multinic-scenario-include.md)]

<span data-ttu-id="c2431-112">Az alábbi lépéseket használja nevű erőforráscsoport *IaaSStory* a webkiszolgálók és az erőforráscsoport neve *IaaSStory-háttérrendszer* adatbázis-kiszolgálók.</span><span class="sxs-lookup"><span data-stu-id="c2431-112">The following steps use a resource group named *IaaSStory* for the WEB servers and a resource group named *IaaSStory-BackEnd* for the DB servers.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="c2431-113">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="c2431-113">Prerequisites</span></span>

<span data-ttu-id="c2431-114">Az adatbázis-kiszolgálók létrehozása előtt kell létrehoznia a *IaaSStory* erőforráscsoport ehhez a forgatókönyvhöz szükséges minden erőforráshoz.</span><span class="sxs-lookup"><span data-stu-id="c2431-114">Before you can create the DB servers, you need to create the *IaaSStory* resource group with all the necessary resources for this scenario.</span></span> <span data-ttu-id="c2431-115">Ezek az erőforrások létrehozásához hajtsa végre az alábbi.</span><span class="sxs-lookup"><span data-stu-id="c2431-115">To create these resources, complete the steps that follow.</span></span> <span data-ttu-id="c2431-116">Virtuális hálózat létrehozása a lépések a [hozzon létre egy virtuális hálózatot](virtual-networks-create-vnet-classic-netcfg-ps.md) cikk.</span><span class="sxs-lookup"><span data-stu-id="c2431-116">Create a virtual network by following the steps in the [Create a virtual network](virtual-networks-create-vnet-classic-netcfg-ps.md) article.</span></span>

[!INCLUDE [azure-ps-prerequisites-include.md](../../includes/azure-ps-prerequisites-include.md)]

## <a name="create-the-back-end-vms"></a><span data-ttu-id="c2431-117">A háttér-virtuális gépek létrehozása</span><span class="sxs-lookup"><span data-stu-id="c2431-117">Create the back-end VMs</span></span>
<span data-ttu-id="c2431-118">A háttér-virtuális gépek létrehozását a következő erőforrások függ:</span><span class="sxs-lookup"><span data-stu-id="c2431-118">The back-end VMs depend on the creation of the following resources:</span></span>

* <span data-ttu-id="c2431-119">**Backend alhálózathoz**.</span><span class="sxs-lookup"><span data-stu-id="c2431-119">**Backend subnet**.</span></span> <span data-ttu-id="c2431-120">Az adatbázis-kiszolgálókhoz külön alhálózathoz, hogy áthaladó forgalmat leválasszanak része lesz.</span><span class="sxs-lookup"><span data-stu-id="c2431-120">The database servers will be part of a separate subnet, to segregate traffic.</span></span> <span data-ttu-id="c2431-121">Az alábbi parancsfájl vár az alhálózat léteznie egy nevű vnetet a *WTestVnet*.</span><span class="sxs-lookup"><span data-stu-id="c2431-121">The script below expects this subnet to exist in a vnet named *WTestVnet*.</span></span>
* <span data-ttu-id="c2431-122">**Az adatlemezek tárfiók**.</span><span class="sxs-lookup"><span data-stu-id="c2431-122">**Storage account for data disks**.</span></span> <span data-ttu-id="c2431-123">A jobb teljesítmény érdekében az adatlemezek az adatbázis-kiszolgálók a tartós állapotú meghajtót (SSD) technológiát, amely a prémium szintű tárfiók szükséges fogja használni.</span><span class="sxs-lookup"><span data-stu-id="c2431-123">For better performance, the data disks on the database servers will use solid state drive (SSD) technology, which requires a premium storage account.</span></span> <span data-ttu-id="c2431-124">Győződjön meg arról, hogy az Azure-hely támogatja a prémium szintű storage telepít.</span><span class="sxs-lookup"><span data-stu-id="c2431-124">Make sure the Azure location you deploy to support premium storage.</span></span>
* <span data-ttu-id="c2431-125">**A rendelkezésre állási csoport**.</span><span class="sxs-lookup"><span data-stu-id="c2431-125">**Availability set**.</span></span> <span data-ttu-id="c2431-126">Minden adatbázis-kiszolgálók egyetlen rendelkezésre állási értékre, akkor ellenőrizze, hogy a virtuális gépek közül legalább egy, és a karbantartás során fut hozzáadandó.</span><span class="sxs-lookup"><span data-stu-id="c2431-126">All database servers will be added to a single availability set, to ensure at least one of the VMs is up and running during maintenance.</span></span>

### <a name="step-1---start-your-script"></a><span data-ttu-id="c2431-127">1. lépés – a parancsfájl futtatásához</span><span class="sxs-lookup"><span data-stu-id="c2431-127">Step 1 - Start your script</span></span>
<span data-ttu-id="c2431-128">Letöltheti használt teljes PowerShell-parancsfájl [Itt](https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/IaaS-Story/11-MultiNIC/classic/virtual-network-deploy-multinic-classic-ps.ps1).</span><span class="sxs-lookup"><span data-stu-id="c2431-128">You can download the full PowerShell script used [here](https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/IaaS-Story/11-MultiNIC/classic/virtual-network-deploy-multinic-classic-ps.ps1).</span></span> <span data-ttu-id="c2431-129">Módosíthatja a parancsfájlnak a környezetben az alábbi lépésekkel.</span><span class="sxs-lookup"><span data-stu-id="c2431-129">Follow the steps below to change the script to work in your environment.</span></span>

1. <span data-ttu-id="c2431-130">A meglévő erőforráscsoport üzembe helyezett fent alapján az alábbi változók értékeinek módosítása [Előfeltételek](#Prerequisites).</span><span class="sxs-lookup"><span data-stu-id="c2431-130">Change the values of the variables below based on your existing resource group deployed above in [Prerequisites](#Prerequisites).</span></span>

    ```powershell
    $location              = "West US"
    $vnetName              = "WTestVNet"
    $backendSubnetName     = "BackEnd"
    ```

2. <span data-ttu-id="c2431-131">A háttérrendszer telepítéshez használni kívánt értékek alapján az alábbi változók értékeinek módosítása.</span><span class="sxs-lookup"><span data-stu-id="c2431-131">Change the values of the variables below based on the values you want to use for your backend deployment.</span></span>

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

### <a name="step-2---create-necessary-resources-for-your-vms"></a><span data-ttu-id="c2431-132">2. lépés - a szükséges erőforrásokat létrehozni a virtuális géphez</span><span class="sxs-lookup"><span data-stu-id="c2431-132">Step 2 - Create necessary resources for your VMs</span></span>
<span data-ttu-id="c2431-133">Az összes virtuális gépet egy új felhőalapú szolgáltatás és az adatlemezek tárfiók létrehozásához szükséges.</span><span class="sxs-lookup"><span data-stu-id="c2431-133">You need to create a new cloud service and a storage account for the data disks for all VMs.</span></span> <span data-ttu-id="c2431-134">Meg kell adnia a képet, és egy helyi rendszergazdai fiók a virtuális gépek esetén is.</span><span class="sxs-lookup"><span data-stu-id="c2431-134">You also need to specify an image, and a local administrator account for the VMs.</span></span> <span data-ttu-id="c2431-135">Ezek az erőforrások létrehozásához kövesse az alábbi lépéseket:</span><span class="sxs-lookup"><span data-stu-id="c2431-135">To create these resources, complete the following steps:</span></span>

1. <span data-ttu-id="c2431-136">Új felhőalapú szolgáltatás létrehozása.</span><span class="sxs-lookup"><span data-stu-id="c2431-136">Create a new cloud service.</span></span>

    ```powershell
    New-AzureService -ServiceName $backendCSName -Location $location
    ```

2. <span data-ttu-id="c2431-137">Hozzon létre egy új prémium szintű storage-fiók.</span><span class="sxs-lookup"><span data-stu-id="c2431-137">Create a new premium storage account.</span></span>

    ```powershell
    New-AzureStorageAccount -StorageAccountName $prmStorageAccountName `
    -Location $location -Type Premium_LRS
    ```
3. <span data-ttu-id="c2431-138">Állítsa be az előfizetéshez tartozó aktuális tárfiókkal a fenti létrehozott tárfiók.</span><span class="sxs-lookup"><span data-stu-id="c2431-138">Set the storage account created above as the current storage account for your subscription.</span></span>

    ```powershell
    $subscription = Get-AzureSubscription | where {$_.IsCurrent -eq $true}  
    Set-AzureSubscription -SubscriptionName $subscription.SubscriptionName `
    -CurrentStorageAccountName $prmStorageAccountName
    ```

4. <span data-ttu-id="c2431-139">Kép kiválasztása a virtuális gép számára.</span><span class="sxs-lookup"><span data-stu-id="c2431-139">Select an image for the VM.</span></span>

    ```powershell
    $image = Get-AzureVMImage `
    | where{$_.ImageFamily -eq "SQL Server 2014 RTM Web on Windows Server 2012 R2"} `
    | sort PublishedDate -Descending `
    | select -ExpandProperty ImageName -First 1
    ```

5. <span data-ttu-id="c2431-140">Állítsa be a helyi rendszergazdai fiók hitelesítő adatait.</span><span class="sxs-lookup"><span data-stu-id="c2431-140">Set the local administrator account credentials.</span></span>

    ```powershell
    $cred = Get-Credential -Message "Enter username and password for local admin account"
    ```

### <a name="step-3---create-vms"></a><span data-ttu-id="c2431-141">3. lépés – a virtuális gépek létrehozása</span><span class="sxs-lookup"><span data-stu-id="c2431-141">Step 3 - Create VMs</span></span>
<span data-ttu-id="c2431-142">Hurok segítségével létrehozott egy tetszőleges számú virtuális gépet, és a szükséges hálózati adapterek és virtuális gépek létrehozása a hurkon belül kell.</span><span class="sxs-lookup"><span data-stu-id="c2431-142">You need to use a loop to create as many VMs as you want, and create the necessary NICs and VMs within the loop.</span></span> <span data-ttu-id="c2431-143">A hálózati adapterek és a virtuális gépek létrehozásához hajtsa végre az alábbi lépéseket.</span><span class="sxs-lookup"><span data-stu-id="c2431-143">To create the NICs and VMs, execute the following steps.</span></span>

1. <span data-ttu-id="c2431-144">Indítsa el a `for` hozhatnak létre a virtuális gépek és két hálózati adaptert annyiszor szükséges, ismételje meg a hurok értéke alapján a `$numberOfVMs` változó.</span><span class="sxs-lookup"><span data-stu-id="c2431-144">Start a `for` loop to repeat the commands to create a VM and two NICs as many times as necessary, based on the value of the `$numberOfVMs` variable.</span></span>

    ```powershell
    for ($suffixNumber = 1; $suffixNumber -le $numberOfVMs; $suffixNumber++){
    ```

2. <span data-ttu-id="c2431-145">Hozzon létre egy `VMConfig` objektumot adja meg a lemezkép mérete és rendelkezésre állási csoportot a virtuális gép számára.</span><span class="sxs-lookup"><span data-stu-id="c2431-145">Create a `VMConfig` object specifying the image, size, and availability set for the VM.</span></span>

    ```powershell
    $vmName = $vmNamePrefix + $suffixNumber
    $vmConfig = New-AzureVMConfig -Name $vmName `
        -ImageName $image `
        -InstanceSize $vmSize `
        -AvailabilitySetName $avSetName
    ```

3. <span data-ttu-id="c2431-146">A virtuális gép telepítéséhez, egy Windows virtuális gép.</span><span class="sxs-lookup"><span data-stu-id="c2431-146">Provision the VM as a Windows VM.</span></span>

    ```powershell
    Add-AzureProvisioningConfig -VM $vmConfig -Windows `
        -AdminUsername $cred.UserName `
        -Password $cred.GetNetworkCredential().Password
    ```

4. <span data-ttu-id="c2431-147">Az alapértelmezett hálózati adapter, és rendelje hozzá egy statikus IP-címet.</span><span class="sxs-lookup"><span data-stu-id="c2431-147">Set the default NIC and assign it a static IP address.</span></span>

    ```powershell
    Set-AzureSubnet         -SubnetNames $backendSubnetName -VM $vmConfig
    Set-AzureStaticVNetIP   -IPAddress ($ipAddressPrefix+$suffixNumber+3) -VM $vmConfig
    ```

5. <span data-ttu-id="c2431-148">Adja hozzá a második hálózati adapter az egyes virtuális gépek.</span><span class="sxs-lookup"><span data-stu-id="c2431-148">Add a second NIC for each VM.</span></span>

    ```powershell
    Add-AzureNetworkInterfaceConfig -Name ("RemoteAccessNIC"+$suffixNumber) `
    -SubnetName $backendSubnetName `
    -StaticVNetIPAddress ($ipAddressPrefix+(53+$suffixNumber)) `
    -VM $vmConfig
    ```

6. <span data-ttu-id="c2431-149">Hozzon létre egy adatlemezt az egyes virtuális gépek.</span><span class="sxs-lookup"><span data-stu-id="c2431-149">Create to data disks for each VM.</span></span>

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

7. <span data-ttu-id="c2431-150">Minden virtuális gép létrehozása, és a hurok végződnie.</span><span class="sxs-lookup"><span data-stu-id="c2431-150">Create each VM, and end the loop.</span></span>

    ```powershell
    New-AzureVM -VM $vmConfig `
    -ServiceName $backendCSName `
    -Location $location `
    -VNetName $vnetName
    }
    ```

### <a name="step-4---run-the-script"></a><span data-ttu-id="c2431-151">4. lépés: a parancsfájl futtatása</span><span class="sxs-lookup"><span data-stu-id="c2431-151">Step 4 - Run the script</span></span>
<span data-ttu-id="c2431-152">Letöltött, és a igények alapján a parancsfájl módosított, runt ő parancsfájl létrehozásához a háttérbeli adatbázis virtuális gépek több hálózati adapterrel rendelkező.</span><span class="sxs-lookup"><span data-stu-id="c2431-152">Now that you downloaded and changed the script based on your needs, runt he script to create the back end database VMs with multiple NICs.</span></span>

1. <span data-ttu-id="c2431-153">Mentse a parancsfájlt, és futtassa azt a **PowerShell** parancssort, vagy **PowerShell ISE**.</span><span class="sxs-lookup"><span data-stu-id="c2431-153">Save your script and run it from the **PowerShell** command prompt, or **PowerShell ISE**.</span></span> <span data-ttu-id="c2431-154">A kezdeti kimenetet fog látni, alább látható módon.</span><span class="sxs-lookup"><span data-stu-id="c2431-154">You will see the initial output, as shown below.</span></span>

        OperationDescription    OperationId                          OperationStatus

        New-AzureService        xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx Succeeded
        New-AzureStorageAccount xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx Succeeded
        
        WARNING: No deployment found in service: 'IaaSStory-Backend'.
2. <span data-ttu-id="c2431-155">Töltse ki a hitelesítő adatokat kér, majd kattintson a szükséges információkat **OK**.</span><span class="sxs-lookup"><span data-stu-id="c2431-155">Fill out the information needed in the credentials prompt and click **OK**.</span></span> <span data-ttu-id="c2431-156">Az alábbi a kimenetet visszaadja.</span><span class="sxs-lookup"><span data-stu-id="c2431-156">The output below is returned.</span></span>

        New-AzureVM             xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx Succeeded
        New-AzureVM             xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx Succeeded
