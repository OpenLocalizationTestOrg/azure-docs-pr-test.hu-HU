---
title: "virtuális gép több hálózati adapter - Azure PowerShell és aaaCreate |} Microsoft Docs"
description: "Megtudhatja, hogyan toocreate PowerShell segítségével több hálózati adapterrel rendelkező virtuális gép."
services: virtual-network
documentationcenter: na
author: jimdial
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: 88880483-8f9e-4eeb-b783-64b8613407d9
ms.service: virtual-network
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 02/02/2016
ms.author: jdial
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 507a413510da3ee69aefed324977ee40e442268b
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="create-a-vm-with-multiple-nics-using-powershell"></a><span data-ttu-id="15876-103">PowerShell-lel több hálózati adapterrel rendelkező virtuális gép létrehozása</span><span class="sxs-lookup"><span data-stu-id="15876-103">Create a VM with multiple NICs using PowerShell</span></span>

> [!div class="op_single_selector"]
> * [<span data-ttu-id="15876-104">PowerShell</span><span class="sxs-lookup"><span data-stu-id="15876-104">PowerShell</span></span>](virtual-network-deploy-multinic-arm-ps.md)
> * [<span data-ttu-id="15876-105">Azure CLI</span><span class="sxs-lookup"><span data-stu-id="15876-105">Azure CLI</span></span>](virtual-network-deploy-multinic-arm-cli.md)
> * [<span data-ttu-id="15876-106">Sablon</span><span class="sxs-lookup"><span data-stu-id="15876-106">Template</span></span>](virtual-network-deploy-multinic-arm-template.md)
> * [<span data-ttu-id="15876-107">PowerShell (klasszikus)</span><span class="sxs-lookup"><span data-stu-id="15876-107">PowerShell (Classic)</span></span>](virtual-network-deploy-multinic-classic-ps.md)
> * [<span data-ttu-id="15876-108">Az Azure CLI (klasszikus)</span><span class="sxs-lookup"><span data-stu-id="15876-108">Azure CLI (Classic)</span></span>](virtual-network-deploy-multinic-classic-cli.md)

[!INCLUDE [virtual-network-deploy-multinic-intro-include.md](../../includes/virtual-network-deploy-multinic-intro-include.md)]

> [!NOTE]
> <span data-ttu-id="15876-109">Az Azure két különböző üzembe helyezési modellel rendelkezik az erőforrások létrehozásához és használatához: [Resource Manager és klasszikus](../resource-manager-deployment-model.md).</span><span class="sxs-lookup"><span data-stu-id="15876-109">Azure has two different deployment models for creating and working with resources:  [Resource Manager and classic](../resource-manager-deployment-model.md).</span></span>  <span data-ttu-id="15876-110">Ez a cikk ismerteti a használatával a Microsoft azt javasolja, a legtöbb új központi telepítés helyett hello hello Resource Manager üzembe helyezési modellben [klasszikus üzembe helyezési modellel](virtual-network-deploy-multinic-classic-ps.md).</span><span class="sxs-lookup"><span data-stu-id="15876-110">This article covers using hello Resource Manager deployment model, which Microsoft recommends for most new deployments instead of hello [classic deployment model](virtual-network-deploy-multinic-classic-ps.md).</span></span>
>

[!INCLUDE [virtual-network-deploy-multinic-scenario-include.md](../../includes/virtual-network-deploy-multinic-scenario-include.md)]

<span data-ttu-id="15876-111">hello következő lépések használják nevű erőforráscsoport *IaaSStory* hello webkiszolgálók és az erőforráscsoport neve *IaaSStory-háttérrendszer* hello DB kiszolgálók.</span><span class="sxs-lookup"><span data-stu-id="15876-111">hello following steps use a resource group named *IaaSStory* for hello WEB servers and a resource group named *IaaSStory-BackEnd* for hello DB servers.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="15876-112">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="15876-112">Prerequisites</span></span>
<span data-ttu-id="15876-113">Mielőtt hello DB kiszolgálók hozhat létre, meg kell-e toocreate hello *IaaSStory* erőforráscsoport összes hello szükséges erőforrások ehhez a forgatókönyvhöz.</span><span class="sxs-lookup"><span data-stu-id="15876-113">Before you can create hello DB servers, you need toocreate hello *IaaSStory* resource group with all hello necessary resources for this scenario.</span></span> <span data-ttu-id="15876-114">toocreate ezeket az erőforrásokat, végezze el hello a következő lépéseket:</span><span class="sxs-lookup"><span data-stu-id="15876-114">toocreate these resources, complete hello following steps:</span></span>

1. <span data-ttu-id="15876-115">Keresse meg a túl[hello sablonlap](https://github.com/Azure/azure-quickstart-templates/tree/master/IaaS-Story/11-MultiNIC).</span><span class="sxs-lookup"><span data-stu-id="15876-115">Navigate too[hello template page](https://github.com/Azure/azure-quickstart-templates/tree/master/IaaS-Story/11-MultiNIC).</span></span>
2. <span data-ttu-id="15876-116">Hello sablon lapon jobbra toohello **szülő erőforráscsoport**, kattintson a **tooAzure telepítése**.</span><span class="sxs-lookup"><span data-stu-id="15876-116">In hello template page, toohello right of **Parent resource group**, click **Deploy tooAzure**.</span></span>
3. <span data-ttu-id="15876-117">Szükség esetén módosítsa a hello paraméterértékeket, majd hello lépésekkel hello Azure betekintő portál toodeploy hello erőforráscsoportban.</span><span class="sxs-lookup"><span data-stu-id="15876-117">If needed, change hello parameter values, then follow hello steps in hello Azure preview portal toodeploy hello resource group.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="15876-118">Győződjön meg arról, hogy a tárfiókok neve egyedi.</span><span class="sxs-lookup"><span data-stu-id="15876-118">Make sure your storage account names are unique.</span></span> <span data-ttu-id="15876-119">Ismétlődő tárfiókok neve nem lehet az Azure-ban.</span><span class="sxs-lookup"><span data-stu-id="15876-119">You cannot have duplicate storage account names in Azure.</span></span>
> 

[!INCLUDE [azure-ps-prerequisites-include.md](../../includes/azure-ps-prerequisites-include.md)]

## <a name="create-hello-back-end-vms"></a><span data-ttu-id="15876-120">Hello háttér virtuális gépek létrehozása</span><span class="sxs-lookup"><span data-stu-id="15876-120">Create hello back-end VMs</span></span>
<span data-ttu-id="15876-121">hello háttér virtuális gépek hello létrehozása a következő erőforrások hello függ:</span><span class="sxs-lookup"><span data-stu-id="15876-121">hello back-end VMs depend on hello creation of hello following resources:</span></span>

* <span data-ttu-id="15876-122">**Az adatlemezek tárfiók**.</span><span class="sxs-lookup"><span data-stu-id="15876-122">**Storage account for data disks**.</span></span> <span data-ttu-id="15876-123">A jobb teljesítmény érdekében a hello adatbázis-kiszolgálóin hello adatlemezek tartós állapotú meghajtót (SSD) technológiát, amely a prémium szintű tárfiók szükséges fogja használni.</span><span class="sxs-lookup"><span data-stu-id="15876-123">For better performance, hello data disks on hello database servers will use solid state drive (SSD) technology, which requires a premium storage account.</span></span> <span data-ttu-id="15876-124">Győződjön meg arról, hogy hello Azure-beli hely toosupport prémium szintű storage telepít.</span><span class="sxs-lookup"><span data-stu-id="15876-124">Make sure hello Azure location you deploy toosupport premium storage.</span></span>
* <span data-ttu-id="15876-125">**Hálózati adapter**.</span><span class="sxs-lookup"><span data-stu-id="15876-125">**NICs**.</span></span> <span data-ttu-id="15876-126">Minden virtuális gép lesz a két hálózati adapterrel, egy adatbázis-hozzáférési és felügyeleti egyet.</span><span class="sxs-lookup"><span data-stu-id="15876-126">Each VM will have two NICs, one for database access, and one for management.</span></span>
* <span data-ttu-id="15876-127">**A rendelkezésre állási csoport**.</span><span class="sxs-lookup"><span data-stu-id="15876-127">**Availability set**.</span></span> <span data-ttu-id="15876-128">Minden adatbázis-kiszolgálók megkapja tooa egyetlen rendelkezésre állási csoportot, hello virtuális gépek közül legalább egy tooensure megfelelően működik, és karbantartás során.</span><span class="sxs-lookup"><span data-stu-id="15876-128">All database servers will be added tooa single availability set, tooensure at least one of hello VMs is up and running during maintenance.</span></span>  

### <a name="step-1---start-your-script"></a><span data-ttu-id="15876-129">1. lépés – a parancsfájl futtatásához</span><span class="sxs-lookup"><span data-stu-id="15876-129">Step 1 - Start your script</span></span>
<span data-ttu-id="15876-130">Letöltheti a hello használt teljes PowerShell parancsfájl [Itt](https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/IaaS-Story/11-MultiNIC/arm/virtual-network-deploy-multinic-arm-ps.ps1).</span><span class="sxs-lookup"><span data-stu-id="15876-130">You can download hello full PowerShell script used [here](https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/IaaS-Story/11-MultiNIC/arm/virtual-network-deploy-multinic-arm-ps.ps1).</span></span> <span data-ttu-id="15876-131">Kövesse az alábbi toochange hello parancsfájl toowork környezetében hello lépéseket.</span><span class="sxs-lookup"><span data-stu-id="15876-131">Follow hello steps below toochange hello script toowork in your environment.</span></span>

1. <span data-ttu-id="15876-132">Hello hello-változók értékeit az alábbi a meglévő erőforráscsoport üzembe helyezett fent alapján módosítása [Előfeltételek](#Prerequisites).</span><span class="sxs-lookup"><span data-stu-id="15876-132">Change hello values of hello variables below based on your existing resource group deployed above in [Prerequisites](#Prerequisites).</span></span>

    ```powershell
    $existingRGName        = "IaaSStory"
    $location              = "West US"
    $vnetName              = "WTestVNet"
    $backendSubnetName     = "BackEnd"
    $remoteAccessNSGName   = "NSG-RemoteAccess"
    $stdStorageAccountName = "wtestvnetstoragestd"
    ```

2. <span data-ttu-id="15876-133">Hello értékek módosítása hello alábbi változók hello értékek alapján kívánt toouse a háttér-telepítéshez.</span><span class="sxs-lookup"><span data-stu-id="15876-133">Change hello values of hello variables below based on hello values you want toouse for your backend deployment.</span></span>

    ```powershell
    $backendRGName         = "IaaSStory-Backend"
    $prmStorageAccountName = "wtestvnetstorageprm"
    $avSetName             = "ASDB"
    $vmSize                = "Standard_DS3"
    $publisher             = "MicrosoftSQLServer"
    $offer                 = "SQL2014SP1-WS2012R2"
    $sku                   = "Standard"
    $version               = "latest"
    $vmNamePrefix          = "DB"
    $osDiskPrefix          = "osdiskdb"
    $dataDiskPrefix        = "datadisk"
    $diskSize               = "120"    
    $nicNamePrefix         = "NICDB"
    $ipAddressPrefix       = "192.168.2."
    $numberOfVMs           = 2
    ```
3. <span data-ttu-id="15876-134">Az üzembe helyezéshez szükséges hello meglévő erőforrásokat lekérni.</span><span class="sxs-lookup"><span data-stu-id="15876-134">Retrieve hello existing resources needed for your deployment.</span></span>

    ```powershell
    $vnet                  = Get-AzureRmVirtualNetwork -Name $vnetName -ResourceGroupName $existingRGName
    $backendSubnet         = $vnet.Subnets|?{$_.Name -eq $backendSubnetName}
    $remoteAccessNSG       = Get-AzureRmNetworkSecurityGroup -Name $remoteAccessNSGName -ResourceGroupName $existingRGName
    $stdStorageAccount     = Get-AzureRmStorageAccount -Name $stdStorageAccountName -ResourceGroupName $existingRGName
    ```

### <a name="step-2---create-necessary-resources-for-your-vms"></a><span data-ttu-id="15876-135">2. lépés - a szükséges erőforrásokat létrehozni a virtuális géphez</span><span class="sxs-lookup"><span data-stu-id="15876-135">Step 2 - Create necessary resources for your VMs</span></span>
<span data-ttu-id="15876-136">Egy új erőforráscsoportot, egy tárfiókot a hello adatlemezek, toocreate van szüksége, és minden virtuális gép rendelkezésre állási készlet.</span><span class="sxs-lookup"><span data-stu-id="15876-136">You need toocreate a new resource group, a storage account for hello data disks, and an availability set for all VMs.</span></span> <span data-ttu-id="15876-137">Módosíthat kell hello helyi rendszergazdai fiók hitelesítő adatait az egyes virtuális gépek.</span><span class="sxs-lookup"><span data-stu-id="15876-137">You alos need hello local administrator account credentials for each VM.</span></span> <span data-ttu-id="15876-138">toocreate ezekkel az erőforrásokkal hajtható végre hello a következő lépéseket.</span><span class="sxs-lookup"><span data-stu-id="15876-138">toocreate these resources, execute hello following steps.</span></span>

1. <span data-ttu-id="15876-139">Hozzon létre egy új erőforráscsoportot.</span><span class="sxs-lookup"><span data-stu-id="15876-139">Create a new resource group.</span></span>

    ```powershell
    New-AzureRmResourceGroup -Name $backendRGName -Location $location
    ```
2. <span data-ttu-id="15876-140">Új prémium szintű storage-fiók létrehozása a fenti létrehozott hello erőforráscsoportban.</span><span class="sxs-lookup"><span data-stu-id="15876-140">Create a new premium storage account in hello resource group created above.</span></span>

    ```powershell
    $prmStorageAccount = New-AzureRmStorageAccount -Name $prmStorageAccountName `
    -ResourceGroupName $backendRGName -Type Premium_LRS -Location $location
    ```
3. <span data-ttu-id="15876-141">Hozzon létre egy új rendelkezésre állási csoportot.</span><span class="sxs-lookup"><span data-stu-id="15876-141">Create a new availability set.</span></span>

    ```powershell
    $avSet = New-AzureRmAvailabilitySet -Name $avSetName -ResourceGroupName $backendRGName -Location $location
    ```
4. <span data-ttu-id="15876-142">Hello helyi rendszergazdai fiók hitelesítő adatait toobe az egyes virtuális gépek használt beolvasása.</span><span class="sxs-lookup"><span data-stu-id="15876-142">Get hello local administrator account credentials toobe used for each VM.</span></span>

    ```powershell
    $cred = Get-Credential -Message "Type hello name and password for hello local administrator account."
    ```

### <a name="step-3---create-hello-nics-and-back-end-vms"></a><span data-ttu-id="15876-143">3. lépés – hello hálózati adapter és a háttér-virtuális gépek létrehozása</span><span class="sxs-lookup"><span data-stu-id="15876-143">Step 3 - Create hello NICs and back-end VMs</span></span>
<span data-ttu-id="15876-144">Szüksége van egy hurok toocreate toouse, sok virtuális gép, és hozzon létre hello szükséges hálózati adapterek és virtuális gépek hello hurkon belül.</span><span class="sxs-lookup"><span data-stu-id="15876-144">You need toouse a loop toocreate as many VMs as you want, and create hello necessary NICs and VMs within hello loop.</span></span> <span data-ttu-id="15876-145">toocreate hello hálózati adapterek és virtuális gépek, hajtható végre a lépéseket követve hello.</span><span class="sxs-lookup"><span data-stu-id="15876-145">toocreate hello NICs and VMs, execute hello following steps.</span></span>

1. <span data-ttu-id="15876-146">Indítsa el a `for` hurok toorepeat hello parancsok toocreate egy virtuális Gépet, és két hálózati adaptert, szükség esetén hányszor hello az alapján hello `$numberOfVMs` változó.</span><span class="sxs-lookup"><span data-stu-id="15876-146">Start a `for` loop toorepeat hello commands toocreate a VM and two NICs as many times as necessary, based on hello value of hello `$numberOfVMs` variable.</span></span>
   
    ```powershell
    for ($suffixNumber = 1; $suffixNumber -le $numberOfVMs; $suffixNumber++){
    ```

2. <span data-ttu-id="15876-147">Az adatbázis eléréséhez használt hálózati hello létrehozása.</span><span class="sxs-lookup"><span data-stu-id="15876-147">Create hello NIC used for database access.</span></span>

    ```powershell
    $nic1Name = $nicNamePrefix + $suffixNumber + "-DA"
    $ipAddress1 = $ipAddressPrefix + ($suffixNumber + 3)
    $nic1 = New-AzureRmNetworkInterface -Name $nic1Name -ResourceGroupName $backendRGName `
    -Location $location -SubnetId $backendSubnet.Id -PrivateIpAddress $ipAddress1
    ```

3. <span data-ttu-id="15876-148">A távoli hozzáféréshez használt hálózati hello létrehozása.</span><span class="sxs-lookup"><span data-stu-id="15876-148">Create hello NIC used for remote access.</span></span> <span data-ttu-id="15876-149">Figyelje meg, hogyan ehhez a hálózati Adapterhez van egy társított NSG-t tooit.</span><span class="sxs-lookup"><span data-stu-id="15876-149">Notice how this NIC has an NSG associated tooit.</span></span>

    ```powershell
    $nic2Name = $nicNamePrefix + $suffixNumber + "-RA"
    $ipAddress2 = $ipAddressPrefix + (53 + $suffixNumber)
    $nic2 = New-AzureRmNetworkInterface -Name $nic2Name -ResourceGroupName $backendRGName `
    -Location $location -SubnetId $backendSubnet.Id -PrivateIpAddress $ipAddress2 `
    -NetworkSecurityGroupId $remoteAccessNSG.Id
    ```

4. <span data-ttu-id="15876-150">Hozzon létre `vmConfig` objektum.</span><span class="sxs-lookup"><span data-stu-id="15876-150">Create `vmConfig` object.</span></span>

    ```powershell
    $vmName = $vmNamePrefix + $suffixNumber
    $vmConfig = New-AzureRmVMConfig -VMName $vmName -VMSize $vmSize -AvailabilitySetId $avSet.Id
    ```

5. <span data-ttu-id="15876-151">Hozzon létre két adatlemezek virtuális gépenként.</span><span class="sxs-lookup"><span data-stu-id="15876-151">Create two data disks per VM.</span></span> <span data-ttu-id="15876-152">Figyelje meg, hogy hello adatlemezek vannak a korábban létrehozott hello prémium szintű tárfiók.</span><span class="sxs-lookup"><span data-stu-id="15876-152">Notice that hello data disks are in hello premium storage account created earlier.</span></span>

    ```powershell
    $dataDisk1Name = $vmName + "-" + $osDiskPrefix + "-1"
    $data1VhdUri = $prmStorageAccount.PrimaryEndpoints.Blob.ToString() + "vhds/" + $dataDisk1Name + ".vhd"
    Add-AzureRmVMDataDisk -VM $vmConfig -Name $dataDisk1Name -DiskSizeInGB $diskSize `
    -VhdUri $data1VhdUri -CreateOption empty -Lun 0

    $dataDisk2Name = $vmName + "-" + $dataDiskPrefix + "-2"
    $data2VhdUri = $prmStorageAccount.PrimaryEndpoints.Blob.ToString() + "vhds/" + $dataDisk2Name + ".vhd"
    Add-AzureRmVMDataDisk -VM $vmConfig -Name $dataDisk2Name -DiskSizeInGB $diskSize `
    -VhdUri $data2VhdUri -CreateOption empty -Lun 1
    ```

6. <span data-ttu-id="15876-153">Hello operációs rendszer és a virtuális gép hello használt kép toobe konfigurálása</span><span class="sxs-lookup"><span data-stu-id="15876-153">Configure hello operating system, and image toobe used for hello VM.</span></span>

    ```powershell
    $vmConfig = Set-AzureRmVMOperatingSystem -VM $vmConfig -Windows -ComputerName $vmName -Credential $cred -ProvisionVMAgent -EnableAutoUpdate
    $vmConfig = Set-AzureRmVMSourceImage -VM $vmConfig -PublisherName $publisher -Offer $offer -Skus $sku -Version $version
    ```

7. <span data-ttu-id="15876-154">A fenti toohello létrehozott hello két hálózati adapterek hozzáadása `vmConfig` objektum.</span><span class="sxs-lookup"><span data-stu-id="15876-154">Add hello two NICs created above toohello `vmConfig` object.</span></span>

    ```powershell
    $vmConfig = Add-AzureRmVMNetworkInterface -VM $vmConfig -Id $nic1.Id -Primary
    $vmConfig = Add-AzureRmVMNetworkInterface -VM $vmConfig -Id $nic2.Id
    ```

8. <span data-ttu-id="15876-155">Hello operációsrendszer-lemez létrehozása, és hozza létre a virtuális gép hello.</span><span class="sxs-lookup"><span data-stu-id="15876-155">Create hello OS disk and create hello VM.</span></span> <span data-ttu-id="15876-156">Értesítés hello `}` hello befejezési `for` hurok.</span><span class="sxs-lookup"><span data-stu-id="15876-156">Notice hello `}` ending hello `for` loop.</span></span>

    ```powershell
    $osDiskName = $vmName + "-" + $osDiskSuffix
    $osVhdUri = $stdStorageAccount.PrimaryEndpoints.Blob.ToString() + "vhds/" + $osDiskName + ".vhd"
    $vmConfig = Set-AzureRmVMOSDisk -VM $vmConfig -Name $osDiskName -VhdUri $osVhdUri -CreateOption fromImage
    New-AzureRmVM -VM $vmConfig -ResourceGroupName $backendRGName -Location $location
    }
    ```

### <a name="step-4---run-hello-script"></a><span data-ttu-id="15876-157">4. lépés: hello parancsfájl futtatása</span><span class="sxs-lookup"><span data-stu-id="15876-157">Step 4 - Run hello script</span></span>
<span data-ttu-id="15876-158">Most, hogy a letöltött és módosított hello parancsfájl igényei szerint, ő parancsfájl toocreate hello háttérbeli adatbázis több hálózati adapterrel rendelkező virtuális gépek runt.</span><span class="sxs-lookup"><span data-stu-id="15876-158">Now that you downloaded and changed hello script based on your needs, runt he script toocreate hello back end database VMs with multiple NICs.</span></span>

1. <span data-ttu-id="15876-159">Mentse a parancsfájlt, és futtassa a hello **PowerShell** parancssort, vagy **PowerShell ISE**.</span><span class="sxs-lookup"><span data-stu-id="15876-159">Save your script and run it from hello **PowerShell** command prompt, or **PowerShell ISE**.</span></span> <span data-ttu-id="15876-160">Hello kezdeti kimeneti, az alábbiak szerint jelenik meg:</span><span class="sxs-lookup"><span data-stu-id="15876-160">You will see hello initial output, as follows:</span></span>

        ResourceGroupName : IaaSStory-Backend
        Location          : westus
        ProvisioningState : Succeeded
        Tags              :
        Permissions       :
            Actions  NotActions
            =======  ==========
                *                  

        ResourceId        : /subscriptions/[Subscription ID]/resourceGroups/IaaSStory-Backend

2. <span data-ttu-id="15876-161">Néhány perc elteltével töltse ki a hello hitelesítő adatokat kér, és kattintson **OK**.</span><span class="sxs-lookup"><span data-stu-id="15876-161">After a few minutes, fill out hello credentials prompt and click **OK**.</span></span> <span data-ttu-id="15876-162">az alábbi hello kimeneti egy virtuális jelöli.</span><span class="sxs-lookup"><span data-stu-id="15876-162">hello output below represents a single VM.</span></span> <span data-ttu-id="15876-163">Értesítés hello teljes folyamat 8 perc toocomplete vett igénybe.</span><span class="sxs-lookup"><span data-stu-id="15876-163">Notice hello entire process took 8 minutes toocomplete.</span></span>

        ResourceGroupName            :
        Id                           :
        Name                         : DB2
        Type                         :
        Location                     :
        Tags                         :
        TagsText                     : null
        AvailabilitySetReference     : Microsoft.Azure.Management.Compute.Models.AvailabilitySetReference
        AvailabilitySetReferenceText :  {
                                    "ReferenceUri": "/subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/IaaSStory-Backend/providers/Microsoft.Compute/availabilitySets/ASDB"
                                    }
        Extensions                   :
        ExtensionsText               : null
        HardwareProfile              : Microsoft.Azure.Management.Compute.Models.HardwareProfile
        HardwareProfileText          : {
                                        "VirtualMachineSize": "Standard_DS3"
                                       }
        InstanceView                 :
        InstanceViewText             : null
        NetworkProfile               :
        NetworkProfileText           : null
        OSProfile                    :
        OSProfileText                : null
        Plan                         :
        PlanText                     : null
        ProvisioningState            :
        StorageProfile               : Microsoft.Azure.Management.Compute.Models.StorageProfile
        StorageProfileText           : {
                                         "DataDisks": [
                                           {
                                             "Lun": 0,
                                             "Caching": null,
                                             "CreateOption": "empty",
                                             "DiskSizeGB": 127,
                                             "Name": "DB2-datadisk-1",
                                             "SourceImage": null,
                                             "VirtualHardDisk": {
                                               "Uri": "https://wtestvnetstorageprm.blob.core.windows.net/vhds/DB2-datadisk-1.vhd"
                                             }
                                           }
                                         ],
                                         "ImageReference": null,
                                         "OSDisk": null
                                       }
        DataDiskNames                : {DB2-datadisk-1}
        NetworkInterfaceIDs          :
        RequestId                    :
        StatusCode                   : 0

        ResourceGroupName            :
        Id                           :
        Name                         : DB2
        Type                         :
        Location                     :
        Tags                         :
        TagsText                     : null
        AvailabilitySetReference     : Microsoft.Azure.Management.Compute.Models.AvailabilitySetReference
        AvailabilitySetReferenceText : {
                                         "ReferenceUri": "/subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/IaaSStory-Backend/providers/
                                       Microsoft.Compute/availabilitySets/ASDB"
                                       }
        Extensions                   :
        ExtensionsText               : null
        HardwareProfile              : Microsoft.Azure.Management.Compute.Models.HardwareProfile
        HardwareProfileText          : {
                                         "VirtualMachineSize": "Standard_DS3"
                                       }
        InstanceView                 :
        InstanceViewText             : null
        NetworkProfile               :
        NetworkProfileText           : null
        OSProfile                    :
        OSProfileText                : null
        Plan                         :
        PlanText                     : null
        ProvisioningState            :
        StorageProfile               : Microsoft.Azure.Management.Compute.Models.StorageProfile
        StorageProfileText           : {
                                         "DataDisks": [
                                           {
                                             "Lun": 0,
                                             "Caching": null,
                                             "CreateOption": "empty",
                                             "DiskSizeGB": 127,
                                             "Name": "DB2-datadisk-1",
                                             "SourceImage": null,
                                             "VirtualHardDisk": {
                                               "Uri": "https://wtestvnetstorageprm.blob.core.windows.net/vhds/DB2-datadisk-1.vhd"
                                             }
                                           },
                                           {
                                             "Lun": 1,
                                             "Caching": null,
                                             "CreateOption": "empty",
                                             "DiskSizeGB": 127,
                                             "Name": "DB2-datadisk-2",
                                             "SourceImage": null,
                                             "VirtualHardDisk": {
                                               "Uri": "https://wtestvnetstorageprm.blob.core.windows.net/vhds/DB2-datadisk-2.vhd"
                                             }
                                           }
                                         ],
                                         "ImageReference": null,
                                         "OSDisk": null
                                       }
        DataDiskNames                : {DB2-datadisk-1, DB2-datadisk-2}
        NetworkInterfaceIDs          :
        RequestId                    :
        StatusCode                   : 0
        EndTime             : [Date] [Time]
        Error               :
        Output              :
        StartTime           : [Date] [Time]
        Status              : Succeeded
        TrackingOperationId : xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx
        RequestId           : xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx
        StatusCode          : OK
