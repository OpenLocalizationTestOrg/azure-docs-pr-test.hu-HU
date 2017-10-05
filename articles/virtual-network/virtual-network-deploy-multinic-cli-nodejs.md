---
title: "Hozzon létre egy virtuális gép több hálózati adapter - Azure CLI 1.0 |} Microsoft Docs"
description: "Útmutató az Azure CLI 1.0 használatával több hálózati adapterrel rendelkező virtuális gép létrehozása."
services: virtual-network
documentationcenter: na
author: jimdial
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: 8e906a4b-8583-4a97-9416-ee34cfa09a98
ms.service: virtual-network
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 02/02/2016
ms.author: jdial
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: b95bcb38664718bf25ec6981c803415790c6da3d
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="create-a-vm-with-multiple-nics-using-the-azure-cli-10"></a><span data-ttu-id="2e8ad-103">Az Azure CLI 1.0 használatával több hálózati adapterrel rendelkező virtuális gép létrehozása</span><span class="sxs-lookup"><span data-stu-id="2e8ad-103">Create a VM with multiple NICs using the Azure CLI 1.0</span></span>

[!INCLUDE [virtual-network-deploy-multinic-arm-selectors-include.md](../../includes/virtual-network-deploy-multinic-arm-selectors-include.md)]

[!INCLUDE [virtual-network-deploy-multinic-intro-include.md](../../includes/virtual-network-deploy-multinic-intro-include.md)]

> [!NOTE]
> <span data-ttu-id="2e8ad-104">Az Azure két különböző üzembe helyezési modellel rendelkezik az erőforrások létrehozásához és használatához: [Resource Manager és klasszikus](../resource-manager-deployment-model.md).</span><span class="sxs-lookup"><span data-stu-id="2e8ad-104">Azure has two different deployment models for creating and working with resources:  [Resource Manager and classic](../resource-manager-deployment-model.md).</span></span>  <span data-ttu-id="2e8ad-105">Ez a cikk a Resource Manager-alapú üzemi modell használatát ismerteti, amelyet a Microsoft a legtöbb új telepítéshez a [klasszikus üzemi modell](virtual-network-deploy-multinic-classic-cli.md) helyett javasol.</span><span class="sxs-lookup"><span data-stu-id="2e8ad-105">This article covers using the Resource Manager deployment model, which Microsoft recommends for most new deployments instead of the [classic deployment model](virtual-network-deploy-multinic-classic-cli.md).</span></span>
>

[!INCLUDE [virtual-network-deploy-multinic-scenario-include.md](../../includes/virtual-network-deploy-multinic-scenario-include.md)]

<span data-ttu-id="2e8ad-106">Az alábbi lépéseket használja nevű erőforráscsoport *IaaSStory* a webkiszolgálók és az erőforráscsoport neve *IaaSStory-háttérrendszer* adatbázis-kiszolgálók.</span><span class="sxs-lookup"><span data-stu-id="2e8ad-106">The following steps use a resource group named *IaaSStory* for the WEB servers and a resource group named *IaaSStory-BackEnd* for the DB servers.</span></span> <span data-ttu-id="2e8ad-107">Hajthatja végre ezt a feladatot az Azure CLI 1.0 (Ez a cikk) vagy a [Azure CLI 2.0](virtual-network-deploy-static-pip-arm-cli.md).</span><span class="sxs-lookup"><span data-stu-id="2e8ad-107">You can complete this task using the Azure CLI 1.0 (this article) or the [Azure CLI 2.0](virtual-network-deploy-static-pip-arm-cli.md).</span></span> <span data-ttu-id="2e8ad-108">Az értékeket a "" a következő lépések a változók létre erőforrásokat tudja kihozni a forgatókönyvből beállításokkal.</span><span class="sxs-lookup"><span data-stu-id="2e8ad-108">The values in "" for the variables in the steps that follow create resources with settings from the scenario.</span></span> <span data-ttu-id="2e8ad-109">Módosítsa az értékeket, a környezetének megfelelő.</span><span class="sxs-lookup"><span data-stu-id="2e8ad-109">Change the values, as appropriate, for your environment.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="2e8ad-110">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="2e8ad-110">Prerequisites</span></span>
<span data-ttu-id="2e8ad-111">Az adatbázis-kiszolgálók létrehozása előtt kell létrehoznia a *IaaSStory* erőforráscsoport ehhez a forgatókönyvhöz szükséges minden erőforráshoz.</span><span class="sxs-lookup"><span data-stu-id="2e8ad-111">Before you can create the DB servers, you need to create the *IaaSStory* resource group with all the necessary resources for this scenario.</span></span> <span data-ttu-id="2e8ad-112">Ezek az erőforrások létrehozásához kövesse az alábbi lépéseket:</span><span class="sxs-lookup"><span data-stu-id="2e8ad-112">To create these resources, complete the following steps:</span></span>

1. <span data-ttu-id="2e8ad-113">Navigáljon a [sablonlap](https://github.com/Azure/azure-quickstart-templates/tree/master/IaaS-Story/11-MultiNIC).</span><span class="sxs-lookup"><span data-stu-id="2e8ad-113">Navigate to [the template page](https://github.com/Azure/azure-quickstart-templates/tree/master/IaaS-Story/11-MultiNIC).</span></span>
2. <span data-ttu-id="2e8ad-114">A sablon lap jobb oldalán lévő **szülő erőforráscsoport**, kattintson a **az Azure telepítéséhez**.</span><span class="sxs-lookup"><span data-stu-id="2e8ad-114">In the template page, to the right of **Parent resource group**, click **Deploy to Azure**.</span></span>
3. <span data-ttu-id="2e8ad-115">Ha szükséges, paraméterértékek módosításához, majd kövesse a telepítéséhez az erőforráscsoportot az Azure betekintő portálon.</span><span class="sxs-lookup"><span data-stu-id="2e8ad-115">If needed, change the parameter values, then follow the steps in the Azure preview portal to deploy the resource group.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="2e8ad-116">Győződjön meg arról, hogy a tárfiókok neve egyedi.</span><span class="sxs-lookup"><span data-stu-id="2e8ad-116">Make sure your storage account names are unique.</span></span> <span data-ttu-id="2e8ad-117">Ismétlődő tárfiókok neve nem lehet az Azure-ban.</span><span class="sxs-lookup"><span data-stu-id="2e8ad-117">You cannot have duplicate storage account names in Azure.</span></span>
> 

[!INCLUDE [azure-cli-prerequisites-include.md](../../includes/azure-cli-prerequisites-include.md)]

## <a name="create-the-back-end-vms"></a><span data-ttu-id="2e8ad-118">A háttér-virtuális gépek létrehozása</span><span class="sxs-lookup"><span data-stu-id="2e8ad-118">Create the back-end VMs</span></span>
<span data-ttu-id="2e8ad-119">A háttér-virtuális gépek létrehozását a következő erőforrások függ:</span><span class="sxs-lookup"><span data-stu-id="2e8ad-119">The back-end VMs depend on the creation of the following resources:</span></span>

* <span data-ttu-id="2e8ad-120">**Az adatlemezek tárfiók**.</span><span class="sxs-lookup"><span data-stu-id="2e8ad-120">**Storage account for data disks**.</span></span> <span data-ttu-id="2e8ad-121">A jobb teljesítmény érdekében az adatlemezek az adatbázis-kiszolgálók a tartós állapotú meghajtót (SSD) technológiát, amely a prémium szintű tárfiók szükséges fogja használni.</span><span class="sxs-lookup"><span data-stu-id="2e8ad-121">For better performance, the data disks on the database servers will use solid state drive (SSD) technology, which requires a premium storage account.</span></span> <span data-ttu-id="2e8ad-122">Győződjön meg arról, hogy az Azure-hely támogatja a prémium szintű storage telepít.</span><span class="sxs-lookup"><span data-stu-id="2e8ad-122">Make sure the Azure location you deploy to support premium storage.</span></span>
* <span data-ttu-id="2e8ad-123">**Hálózati adapter**.</span><span class="sxs-lookup"><span data-stu-id="2e8ad-123">**NICs**.</span></span> <span data-ttu-id="2e8ad-124">Minden virtuális gép lesz a két hálózati adapterrel, egy adatbázis-hozzáférési és felügyeleti egyet.</span><span class="sxs-lookup"><span data-stu-id="2e8ad-124">Each VM will have two NICs, one for database access, and one for management.</span></span>
* <span data-ttu-id="2e8ad-125">**A rendelkezésre állási csoport**.</span><span class="sxs-lookup"><span data-stu-id="2e8ad-125">**Availability set**.</span></span> <span data-ttu-id="2e8ad-126">Minden adatbázis-kiszolgálók egyetlen rendelkezésre állási értékre, akkor ellenőrizze, hogy a virtuális gépek közül legalább egy, és a karbantartás során fut hozzáadandó.</span><span class="sxs-lookup"><span data-stu-id="2e8ad-126">All database servers will be added to a single availability set, to ensure at least one of the VMs is up and running during maintenance.</span></span>

### <a name="step-1---start-your-script"></a><span data-ttu-id="2e8ad-127">1. lépés – a parancsfájl futtatásához</span><span class="sxs-lookup"><span data-stu-id="2e8ad-127">Step 1 - Start your script</span></span>
<span data-ttu-id="2e8ad-128">Letöltheti használt teljes bash parancsfájl [Itt](https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/IaaS-Story/11-MultiNIC/arm/virtual-network-deploy-multinic-arm-cli.sh).</span><span class="sxs-lookup"><span data-stu-id="2e8ad-128">You can download the full bash script used [here](https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/IaaS-Story/11-MultiNIC/arm/virtual-network-deploy-multinic-arm-cli.sh).</span></span> <span data-ttu-id="2e8ad-129">Módosíthatja a parancsfájlnak a környezetben az alábbi lépésekkel.</span><span class="sxs-lookup"><span data-stu-id="2e8ad-129">Follow the steps below to change the script to work in your environment.</span></span>

1. <span data-ttu-id="2e8ad-130">A meglévő erőforráscsoport üzembe helyezett fent alapján az alábbi változók értékeinek módosítása [Előfeltételek](#Prerequisites).</span><span class="sxs-lookup"><span data-stu-id="2e8ad-130">Change the values of the variables below based on your existing resource group deployed above in [Prerequisites](#Prerequisites).</span></span>

    ```azurecli
    existingRGName="IaaSStory"
    location="westus"
    vnetName="WTestVNet"
    backendSubnetName="BackEnd"
    remoteAccessNSGName="NSG-RemoteAccess"
    ```
2. <span data-ttu-id="2e8ad-131">A háttérrendszer telepítéshez használni kívánt értékek alapján az alábbi változók értékeinek módosítása.</span><span class="sxs-lookup"><span data-stu-id="2e8ad-131">Change the values of the variables below based on the values you want to use for your backend deployment.</span></span>

    ```azurecli
    backendRGName="IaaSStory-Backend"
    prmStorageAccountName="wtestvnetstorageprm"
    avSetName="ASDB"
    vmSize="Standard_DS3"
    diskSize=127
    publisher="Canonical"
    offer="UbuntuServer"
    sku="14.04.2-LTS"
    version="latest"
    vmNamePrefix="DB"
    osDiskName="osdiskdb"
    dataDiskName="datadisk"
    nicNamePrefix="NICDB"
    ipAddressPrefix="192.168.2."
    username='adminuser'
    password='adminP@ssw0rd'
    numberOfVMs=2
    ```

3. <span data-ttu-id="2e8ad-132">Az azonosító lekérése a `BackEnd` alhálózati, ahol a virtuális gépek létrejön.</span><span class="sxs-lookup"><span data-stu-id="2e8ad-132">Retrieve the ID for the `BackEnd` subnet where the VMs will be created.</span></span> <span data-ttu-id="2e8ad-133">Mivel a kell lennie az alhálózathoz társított hálózati adapter egy másik erőforráscsoportban található ehhez szüksége.</span><span class="sxs-lookup"><span data-stu-id="2e8ad-133">You need to do this since the NICs to be associated to this subnet are in a different resource group.</span></span>

    ```azurecli
    subnetId="$(azure network vnet subnet show --resource-group $existingRGName \
            --vnet-name $vnetName \
            --name $backendSubnetName|grep Id)"
    subnetId=${subnetId#*/}
    ```

   > [!TIP]
   > <span data-ttu-id="2e8ad-134">Az első parancs által használt fent [grep](http://tldp.org/LDP/Bash-Beginners-Guide/html/sect_04_02.html) és [karakterlánc-manipuláció](http://tldp.org/LDP/abs/html/string-manipulation.html) (pontosabban a substring eltávolítása).</span><span class="sxs-lookup"><span data-stu-id="2e8ad-134">The first command above uses [grep](http://tldp.org/LDP/Bash-Beginners-Guide/html/sect_04_02.html) and [string manipulation](http://tldp.org/LDP/abs/html/string-manipulation.html) (more specifically, substring removal).</span></span>
   >

4. <span data-ttu-id="2e8ad-135">Az azonosító lekérése a `NSG-RemoteAccess` NSG.</span><span class="sxs-lookup"><span data-stu-id="2e8ad-135">Retrieve the ID for the `NSG-RemoteAccess` NSG.</span></span> <span data-ttu-id="2e8ad-136">Mivel az NSG társítandó a hálózati adaptert egy másik erőforráscsoportban található ehhez szüksége.</span><span class="sxs-lookup"><span data-stu-id="2e8ad-136">You need to do this since the NICs to be associated to this NSG are in a different resource group.</span></span>

    ```azurecli
    nsgId="$(azure network nsg show --resource-group $existingRGName \
        --name $remoteAccessNSGName|grep Id)"
        nsgId=${nsgId#*/}
    ```

### <a name="step-2---create-necessary-resources-for-your-vms"></a><span data-ttu-id="2e8ad-137">2. lépés - a szükséges erőforrásokat létrehozni a virtuális géphez</span><span class="sxs-lookup"><span data-stu-id="2e8ad-137">Step 2 - Create necessary resources for your VMs</span></span>

1. <span data-ttu-id="2e8ad-138">Hozzon létre egy új erőforráscsoportot összes háttér-erőforrás.</span><span class="sxs-lookup"><span data-stu-id="2e8ad-138">Create a new resource group for all backend resources.</span></span> <span data-ttu-id="2e8ad-139">Figyelje meg a a `$backendRGName` az erőforráscsoport neve, a változó és `$location` az Azure-régiót.</span><span class="sxs-lookup"><span data-stu-id="2e8ad-139">Notice the use of the `$backendRGName` variable for the resource group name, and `$location` for the Azure region.</span></span>

    ```azurecli
    azure group create $backendRGName $location
    ```

2. <span data-ttu-id="2e8ad-140">Az operációs rendszer és az adatlemezek saját virtuális gépek által használható egy prémium szintű storage-fiók létrehozása.</span><span class="sxs-lookup"><span data-stu-id="2e8ad-140">Create a premium storage account for the OS and data disks to be used by yours VMs.</span></span>

    ```azurecli
    azure storage account create $prmStorageAccountName \
        --resource-group $backendRGName \
        --location $location \
        --type PLRS
    ```

3. <span data-ttu-id="2e8ad-141">Állítsa be a virtuális gépek rendelkezésre állási létrehozása.</span><span class="sxs-lookup"><span data-stu-id="2e8ad-141">Create an availability set for the VMs.</span></span>

    ```azurecli
    azure availset create --resource-group $backendRGName \
        --location $location \
        --name $avSetName
    ```

### <a name="step-3---create-the-nics-and-back-end-vms"></a><span data-ttu-id="2e8ad-142">3. lépés - a hálózati adapterek és a háttér-virtuális gépek létrehozása</span><span class="sxs-lookup"><span data-stu-id="2e8ad-142">Step 3 - Create the NICs and back-end VMs</span></span>

1. <span data-ttu-id="2e8ad-143">Indítsa el a hurok alapján több virtuális gép létrehozásához a `numberOfVMs` változók.</span><span class="sxs-lookup"><span data-stu-id="2e8ad-143">Start a loop to create multiple VMs, based on the `numberOfVMs` variables.</span></span>

    ```azurecli
    for ((suffixNumber=1;suffixNumber<=numberOfVMs;suffixNumber++));
    do
    ```

2. <span data-ttu-id="2e8ad-144">Az egyes virtuális gépek hozzon létre egy hálózati Adaptert, az adatbázis eléréséhez.</span><span class="sxs-lookup"><span data-stu-id="2e8ad-144">For each VM, create a NIC for database access.</span></span>

    ```azurecli
    nic1Name=$nicNamePrefix$suffixNumber-DA
    x=$((suffixNumber+3))
    ipAddress1=$ipAddressPrefix$x
    azure network nic create --name $nic1Name \
        --resource-group $backendRGName \
        --location $location \
        --private-ip-address $ipAddress1 \
        --subnet-id $subnetId
    ```

3. <span data-ttu-id="2e8ad-145">Az egyes virtuális gépek hozzon létre egy hálózati Adaptert a távoli hozzáféréshez.</span><span class="sxs-lookup"><span data-stu-id="2e8ad-145">For each VM, create a NIC for remote access.</span></span> <span data-ttu-id="2e8ad-146">Figyelje meg a `--network-security-group` paraméter, a hálózati adapterről egy NSG hozzárendelésére.</span><span class="sxs-lookup"><span data-stu-id="2e8ad-146">Notice the `--network-security-group` parameter, used to associate the NIC to an NSG.</span></span>

    ```azurecli
    nic2Name=$nicNamePrefix$suffixNumber-RA
    x=$((suffixNumber+53))
    ipAddress2=$ipAddressPrefix$x
    azure network nic create --name $nic2Name \
        --resource-group $backendRGName \
        --location $location \
        --private-ip-address $ipAddress2 \
        --subnet-id $subnetId $vnetName \
        --network-security-group-id $nsgId
    ```

4. <span data-ttu-id="2e8ad-147">A virtuális gép létrehozása.</span><span class="sxs-lookup"><span data-stu-id="2e8ad-147">Create the VM.</span></span>

    ```azurecli
    azure vm create --resource-group $backendRGName \
        --name $vmNamePrefix$suffixNumber \
        --location $location \
        --vm-size $vmSize \
        --subnet-id $subnetId \
        --availset-name $avSetName \
        --nic-names $nic1Name,$nic2Name \
        --os-type linux \
        --image-urn $publisher:$offer:$sku:$version \
        --storage-account-name $prmStorageAccountName \
        --storage-account-container-name vhds \
        --os-disk-vhd $osDiskName$suffixNumber.vhd \
        --admin-username $username \
        --admin-password $password
    ```

5. <span data-ttu-id="2e8ad-148">Az egyes virtuális gépek, hozzon létre két adatlemezek, és a hurok végén a `done` parancsot.</span><span class="sxs-lookup"><span data-stu-id="2e8ad-148">For each VM, create two data disks, and end the loop with the `done` command.</span></span>

    ```azurecli
    azure vm disk attach-new --resource-group $backendRGName \
        --vm-name $vmNamePrefix$suffixNumber \
        --storage-account-name $prmStorageAccountName \
        --storage-account-container-name vhds \
        --vhd-name $dataDiskName$suffixNumber-1.vhd \
        --size-in-gb $diskSize \
        --lun 0

    azure vm disk attach-new --resource-group $backendRGName \
        --vm-name $vmNamePrefix$suffixNumber \        
        --storage-account-name $prmStorageAccountName \
        --storage-account-container-name vhds \
        --vhd-name $dataDiskName$suffixNumber-2.vhd \
        --size-in-gb $diskSize \
        --lun 1
        done
    ```

### <a name="step-4---run-the-script"></a><span data-ttu-id="2e8ad-149">4. lépés: a parancsfájl futtatása</span><span class="sxs-lookup"><span data-stu-id="2e8ad-149">Step 4 - Run the script</span></span>
<span data-ttu-id="2e8ad-150">Most, hogy a letöltött és a parancsfájl a igények alapján megváltozott, futtassa a hozzon létre a háttérbeli adatbázis virtuális gépek több hálózati adapter.</span><span class="sxs-lookup"><span data-stu-id="2e8ad-150">Now that you downloaded and changed the script based on your needs, run the script to create the back end database VMs with multiple NICs.</span></span>

1. <span data-ttu-id="2e8ad-151">Mentse a parancsfájlt, és futtassa azt a **Bash** terminál.</span><span class="sxs-lookup"><span data-stu-id="2e8ad-151">Save your script and run it from your **Bash** terminal.</span></span> <span data-ttu-id="2e8ad-152">A kezdeti kimenetet fog látni, alább látható módon.</span><span class="sxs-lookup"><span data-stu-id="2e8ad-152">You will see the initial output, as shown below.</span></span>
   
        info:    Executing command group create
        info:    Getting resource group IaaSStory-Backend
        info:    Creating resource group IaaSStory-Backend
        info:    Created resource group IaaSStory-Backend
        data:    Id:                  /subscriptions/[Subscription ID]/resourceGroups/IaaSStory-Backend
        data:    Name:                IaaSStory-Backend
        data:    Location:            westus
        data:    Provisioning State:  Succeeded
        data:    Tags: null
        data:
        info:    group create command OK
        info:    Executing command storage account create
        info:    Creating storage account
        info:    storage account create command OK
        info:    Executing command availset create
        info:    Looking up the availability set "ASDB"
        info:    Creating availability set "ASDB"
        info:    availset create command OK
        info:    Executing command network nic create
        info:    Looking up the network interface "NICDB1-DA"
        info:    Creating network interface "NICDB1-DA"
        info:    Looking up the network interface "NICDB1-DA"
        data:    Id                              : /subscriptions/[Subscription ID]/resourceGroups/IaaSStory-Backend/providers/Microsoft.Network/networkInterfaces/NICDB1-DA
        data:    Name                            : NICDB1-DA
        data:    Type                            : Microsoft.Network/networkInterfaces
        data:    Location                        : westus
        data:    Provisioning state              : Succeeded
        data:    Enable IP forwarding            : false
        data:    IP configurations:
        data:      Name                          : NIC-config
        data:      Provisioning state            : Succeeded
        data:      Private IP address            : 192.168.2.4
        data:      Private IP Allocation Method  : Static
        data:      Subnet                        : /subscriptions/[Subscription ID]/resourceGroups/IaaSStory/providers/Microsoft.Network/virtualNetworks/WTestVNet/subnets/BackEnd
        data:
        info:    network nic create command OK
        info:    Executing command network nic create
        info:    Looking up the network interface "NICDB1-RA"
        info:    Creating network interface "NICDB1-RA"
        info:    Looking up the network interface "NICDB1-RA"
        data:    Id                              : /subscriptions/[Subscription ID]/resourceGroups/IaaSStory-Backend/providers/Microsoft.Network/networkInterfaces/NICDB1-RA
        data:    Name                            : NICDB1-RA
        data:    Type                            : Microsoft.Network/networkInterfaces
        data:    Location                        : westus
        data:    Provisioning state              : Succeeded
        data:    Enable IP forwarding            : false
        data:    Network security group          : /subscriptions/[Subscription ID]/resourceGroups/IaaSStory/providers/Microsoft.Network/networkSecurityGroups/NSG-RemoteAccess
        data:    IP configurations:
        data:      Name                          : NIC-config
        data:      Provisioning state            : Succeeded
        data:      Private IP address            : 192.168.2.54
        data:      Private IP Allocation Method  : Static
        data:      Subnet                        : /subscriptions/[Subscription ID]/resourceGroups/IaaSStory/providers/Microsoft.Network/virtualNetworks/WTestVNet/subnets/BackEnd
        data:
        info:    network nic create command OK
        info:    Executing command vm create
        info:    Looking up the VM "DB1"
        info:    Using the VM Size "Standard_DS3"
        info:    The [OS, Data] Disk or image configuration requires storage account
        info:    Looking up the storage account wtestvnetstorageprm
        info:    Looking up the availability set "ASDB"
        info:    Found an Availability set "ASDB"
        info:    Looking up the NIC "NICDB1-DA"
        info:    Looking up the NIC "NICDB1-RA"
        info:    Creating VM "DB1"
2. <span data-ttu-id="2e8ad-153">Néhány perc múlva a végrehajtása leáll, és látni fogja a kimeneti részeinek alább látható módon.</span><span class="sxs-lookup"><span data-stu-id="2e8ad-153">After a few minutes, the execution will end and you will see the rest of the output as shown below.</span></span>
   
        info:    vm create command OK
        info:    Executing command vm disk attach-new
        info:    Looking up the VM "DB1"
        info:    Looking up the storage account wtestvnetstorageprm
        info:    New data disk location: https://wtestvnetstorageprm.blob.core.windows.net/vhds/datadisk1-1.vhd
        info:    Updating VM "DB1"
        info:    vm disk attach-new command OK
        info:    Executing command vm disk attach-new
        info:    Looking up the VM "DB1"
        info:    Looking up the storage account wtestvnetstorageprm
        info:    New data disk location: https://wtestvnetstorageprm.blob.core.windows.net/vhds/datadisk1-2.vhd
        info:    Updating VM "DB1"
        info:    vm disk attach-new command OK
        info:    Executing command network nic create
        info:    Looking up the network interface "NICDB2-DA"
        info:    Creating network interface "NICDB2-DA"
        info:    Looking up the network interface "NICDB2-DA"
        data:    Id                              : /subscriptions/[Subscription ID]/resourceGroups/IaaSStory-Backend/providers/Microsoft.Network/networkInterfaces/NICDB2-DA
        data:    Name                            : NICDB2-DA
        data:    Type                            : Microsoft.Network/networkInterfaces
        data:    Location                        : westus
        data:    Provisioning state              : Succeeded
        data:    Enable IP forwarding            : false
        data:    IP configurations:
        data:      Name                          : NIC-config
        data:      Provisioning state            : Succeeded
        data:      Private IP address            : 192.168.2.5
        data:      Private IP Allocation Method  : Static
        data:      Subnet                        : /subscriptions/[Subscription ID]/resourceGroups/IaaSStory/providers/Microsoft.Network/virtualNetworks/WTestVNet/subnets/BackEnd
        data:
        info:    network nic create command OK
        info:    Executing command network nic create
        info:    Looking up the network interface "NICDB2-RA"
        info:    Creating network interface "NICDB2-RA"
        info:    Looking up the network interface "NICDB2-RA"
        data:    Id                              : /subscriptions/[Subscription ID]/resourceGroups/IaaSStory-Backend/providers/Microsoft.Network/networkInterfaces/NICDB2-RA
        data:    Name                            : NICDB2-RA
        data:    Type                            : Microsoft.Network/networkInterfaces
        data:    Location                        : westus
        data:    Provisioning state              : Succeeded
        data:    Enable IP forwarding            : false
        data:    Network security group          : /subscriptions/[Subscription ID]/resourceGroups/IaaSStory/providers/Microsoft.Network/networkSecurityGroups/NSG-RemoteAccess
        data:    IP configurations:
        data:      Name                          : NIC-config
        data:      Provisioning state            : Succeeded
        data:      Private IP address            : 192.168.2.55
        data:      Private IP Allocation Method  : Static
        data:      Subnet                        : /subscriptions/[Subscription ID]/resourceGroups/IaaSStory/providers/Microsoft.Network/virtualNetworks/WTestVNet/subnets/BackEnd
        data:
        info:    network nic create command OK
        info:    Executing command vm create
        info:    Looking up the VM "DB2"
        info:    Using the VM Size "Standard_DS3"
        info:    The [OS, Data] Disk or image configuration requires storage account
        info:    Looking up the storage account wtestvnetstorageprm
        info:    Looking up the availability set "ASDB"
        info:    Found an Availability set "ASDB"
        info:    Looking up the NIC "NICDB2-DA"
        info:    Looking up the NIC "NICDB2-RA"
        info:    Creating VM "DB2"
        info:    vm create command OK
        info:    Executing command vm disk attach-new
        info:    Looking up the VM "DB2"
        info:    Looking up the storage account wtestvnetstorageprm
        info:    New data disk location: https://wtestvnetstorageprm.blob.core.windows.net/vhds/datadisk2-1.vhd
        info:    Updating VM "DB2"
        info:    vm disk attach-new command OK
        info:    Executing command vm disk attach-new
        info:    Looking up the VM "DB2"
        info:    Looking up the storage account wtestvnetstorageprm
        info:    New data disk location: https://wtestvnetstorageprm.blob.core.windows.net/vhds/datadisk2-2.vhd
        info:    Updating VM "DB2"
        info:    vm disk attach-new command OK

