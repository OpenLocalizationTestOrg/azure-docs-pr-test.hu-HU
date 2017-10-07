---
title: "virtuális gép több hálózati adapter - Azure CLI 1.0 és aaaCreate |} Microsoft Docs"
description: "Ismerje meg, hogyan használja több hálózati adapterrel rendelkező virtuális gép toocreate hello Azure CLI 1.0."
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
ms.openlocfilehash: 07c660b632bcdc004365a6f910ecf8a5c13cbc6d
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="create-a-vm-with-multiple-nics-using-hello-azure-cli-10"></a><span data-ttu-id="b73af-103">Azure CLI 1.0 hello segítségével több hálózati adapterrel rendelkező virtuális gép létrehozása</span><span class="sxs-lookup"><span data-stu-id="b73af-103">Create a VM with multiple NICs using hello Azure CLI 1.0</span></span>

[!INCLUDE [virtual-network-deploy-multinic-arm-selectors-include.md](../../includes/virtual-network-deploy-multinic-arm-selectors-include.md)]

[!INCLUDE [virtual-network-deploy-multinic-intro-include.md](../../includes/virtual-network-deploy-multinic-intro-include.md)]

> [!NOTE]
> <span data-ttu-id="b73af-104">Az Azure két különböző üzembe helyezési modellel rendelkezik az erőforrások létrehozásához és használatához: [Resource Manager és klasszikus](../resource-manager-deployment-model.md).</span><span class="sxs-lookup"><span data-stu-id="b73af-104">Azure has two different deployment models for creating and working with resources:  [Resource Manager and classic](../resource-manager-deployment-model.md).</span></span>  <span data-ttu-id="b73af-105">Ez a cikk ismerteti a használatával a Microsoft azt javasolja, a legtöbb új központi telepítés helyett hello hello Resource Manager üzembe helyezési modellben [klasszikus üzembe helyezési modellel](virtual-network-deploy-multinic-classic-cli.md).</span><span class="sxs-lookup"><span data-stu-id="b73af-105">This article covers using hello Resource Manager deployment model, which Microsoft recommends for most new deployments instead of hello [classic deployment model](virtual-network-deploy-multinic-classic-cli.md).</span></span>
>

[!INCLUDE [virtual-network-deploy-multinic-scenario-include.md](../../includes/virtual-network-deploy-multinic-scenario-include.md)]

<span data-ttu-id="b73af-106">hello következő lépések használják nevű erőforráscsoport *IaaSStory* hello webkiszolgálók és az erőforráscsoport neve *IaaSStory-háttérrendszer* hello DB kiszolgálók.</span><span class="sxs-lookup"><span data-stu-id="b73af-106">hello following steps use a resource group named *IaaSStory* for hello WEB servers and a resource group named *IaaSStory-BackEnd* for hello DB servers.</span></span> <span data-ttu-id="b73af-107">Hajthatja végre ezt a feladatot az Azure CLI 1.0 (Ez a cikk) hello vagy hello [Azure CLI 2.0](virtual-network-deploy-static-pip-arm-cli.md).</span><span class="sxs-lookup"><span data-stu-id="b73af-107">You can complete this task using hello Azure CLI 1.0 (this article) or hello [Azure CLI 2.0](virtual-network-deploy-static-pip-arm-cli.md).</span></span> <span data-ttu-id="b73af-108">az értékek hello "" hello lépések hello változók erőforrások létre hello forgatókönyvet beállításokkal.</span><span class="sxs-lookup"><span data-stu-id="b73af-108">hello values in "" for hello variables in hello steps that follow create resources with settings from hello scenario.</span></span> <span data-ttu-id="b73af-109">Hello értékek, a környezetének megfelelő módosítására.</span><span class="sxs-lookup"><span data-stu-id="b73af-109">Change hello values, as appropriate, for your environment.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="b73af-110">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="b73af-110">Prerequisites</span></span>
<span data-ttu-id="b73af-111">Mielőtt hello DB kiszolgálók hozhat létre, meg kell-e toocreate hello *IaaSStory* erőforráscsoport összes hello szükséges erőforrások ehhez a forgatókönyvhöz.</span><span class="sxs-lookup"><span data-stu-id="b73af-111">Before you can create hello DB servers, you need toocreate hello *IaaSStory* resource group with all hello necessary resources for this scenario.</span></span> <span data-ttu-id="b73af-112">toocreate ezeket az erőforrásokat, végezze el hello a következő lépéseket:</span><span class="sxs-lookup"><span data-stu-id="b73af-112">toocreate these resources, complete hello following steps:</span></span>

1. <span data-ttu-id="b73af-113">Keresse meg a túl[hello sablonlap](https://github.com/Azure/azure-quickstart-templates/tree/master/IaaS-Story/11-MultiNIC).</span><span class="sxs-lookup"><span data-stu-id="b73af-113">Navigate too[hello template page](https://github.com/Azure/azure-quickstart-templates/tree/master/IaaS-Story/11-MultiNIC).</span></span>
2. <span data-ttu-id="b73af-114">Hello sablon lapon jobbra toohello **szülő erőforráscsoport**, kattintson a **tooAzure telepítése**.</span><span class="sxs-lookup"><span data-stu-id="b73af-114">In hello template page, toohello right of **Parent resource group**, click **Deploy tooAzure**.</span></span>
3. <span data-ttu-id="b73af-115">Szükség esetén módosítsa a hello paraméterértékeket, majd hello lépésekkel hello Azure betekintő portál toodeploy hello erőforráscsoportban.</span><span class="sxs-lookup"><span data-stu-id="b73af-115">If needed, change hello parameter values, then follow hello steps in hello Azure preview portal toodeploy hello resource group.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="b73af-116">Győződjön meg arról, hogy a tárfiókok neve egyedi.</span><span class="sxs-lookup"><span data-stu-id="b73af-116">Make sure your storage account names are unique.</span></span> <span data-ttu-id="b73af-117">Ismétlődő tárfiókok neve nem lehet az Azure-ban.</span><span class="sxs-lookup"><span data-stu-id="b73af-117">You cannot have duplicate storage account names in Azure.</span></span>
> 

[!INCLUDE [azure-cli-prerequisites-include.md](../../includes/azure-cli-prerequisites-include.md)]

## <a name="create-hello-back-end-vms"></a><span data-ttu-id="b73af-118">Hello háttér virtuális gépek létrehozása</span><span class="sxs-lookup"><span data-stu-id="b73af-118">Create hello back-end VMs</span></span>
<span data-ttu-id="b73af-119">hello háttér virtuális gépek hello létrehozása a következő erőforrások hello függ:</span><span class="sxs-lookup"><span data-stu-id="b73af-119">hello back-end VMs depend on hello creation of hello following resources:</span></span>

* <span data-ttu-id="b73af-120">**Az adatlemezek tárfiók**.</span><span class="sxs-lookup"><span data-stu-id="b73af-120">**Storage account for data disks**.</span></span> <span data-ttu-id="b73af-121">A jobb teljesítmény érdekében a hello adatbázis-kiszolgálóin hello adatlemezek tartós állapotú meghajtót (SSD) technológiát, amely a prémium szintű tárfiók szükséges fogja használni.</span><span class="sxs-lookup"><span data-stu-id="b73af-121">For better performance, hello data disks on hello database servers will use solid state drive (SSD) technology, which requires a premium storage account.</span></span> <span data-ttu-id="b73af-122">Győződjön meg arról, hogy hello Azure-beli hely toosupport prémium szintű storage telepít.</span><span class="sxs-lookup"><span data-stu-id="b73af-122">Make sure hello Azure location you deploy toosupport premium storage.</span></span>
* <span data-ttu-id="b73af-123">**Hálózati adapter**.</span><span class="sxs-lookup"><span data-stu-id="b73af-123">**NICs**.</span></span> <span data-ttu-id="b73af-124">Minden virtuális gép lesz a két hálózati adapterrel, egy adatbázis-hozzáférési és felügyeleti egyet.</span><span class="sxs-lookup"><span data-stu-id="b73af-124">Each VM will have two NICs, one for database access, and one for management.</span></span>
* <span data-ttu-id="b73af-125">**A rendelkezésre állási csoport**.</span><span class="sxs-lookup"><span data-stu-id="b73af-125">**Availability set**.</span></span> <span data-ttu-id="b73af-126">Minden adatbázis-kiszolgálók megkapja tooa egyetlen rendelkezésre állási csoportot, hello virtuális gépek közül legalább egy tooensure megfelelően működik, és karbantartás során.</span><span class="sxs-lookup"><span data-stu-id="b73af-126">All database servers will be added tooa single availability set, tooensure at least one of hello VMs is up and running during maintenance.</span></span>

### <a name="step-1---start-your-script"></a><span data-ttu-id="b73af-127">1. lépés – a parancsfájl futtatásához</span><span class="sxs-lookup"><span data-stu-id="b73af-127">Step 1 - Start your script</span></span>
<span data-ttu-id="b73af-128">Letöltheti használt hello teljes bash parancsfájl [Itt](https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/IaaS-Story/11-MultiNIC/arm/virtual-network-deploy-multinic-arm-cli.sh).</span><span class="sxs-lookup"><span data-stu-id="b73af-128">You can download hello full bash script used [here](https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/IaaS-Story/11-MultiNIC/arm/virtual-network-deploy-multinic-arm-cli.sh).</span></span> <span data-ttu-id="b73af-129">Kövesse az alábbi toochange hello parancsfájl toowork környezetében hello lépéseket.</span><span class="sxs-lookup"><span data-stu-id="b73af-129">Follow hello steps below toochange hello script toowork in your environment.</span></span>

1. <span data-ttu-id="b73af-130">Hello hello-változók értékeit az alábbi a meglévő erőforráscsoport üzembe helyezett fent alapján módosítása [Előfeltételek](#Prerequisites).</span><span class="sxs-lookup"><span data-stu-id="b73af-130">Change hello values of hello variables below based on your existing resource group deployed above in [Prerequisites](#Prerequisites).</span></span>

    ```azurecli
    existingRGName="IaaSStory"
    location="westus"
    vnetName="WTestVNet"
    backendSubnetName="BackEnd"
    remoteAccessNSGName="NSG-RemoteAccess"
    ```
2. <span data-ttu-id="b73af-131">Hello értékek módosítása hello alábbi változók hello értékek alapján kívánt toouse a háttér-telepítéshez.</span><span class="sxs-lookup"><span data-stu-id="b73af-131">Change hello values of hello variables below based on hello values you want toouse for your backend deployment.</span></span>

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

3. <span data-ttu-id="b73af-132">Beolvasni hello hello Azonosítóját `BackEnd` alhálózati, ahol a virtuális gépek hello létrejön.</span><span class="sxs-lookup"><span data-stu-id="b73af-132">Retrieve hello ID for hello `BackEnd` subnet where hello VMs will be created.</span></span> <span data-ttu-id="b73af-133">Szüksége toodo mivel hello hálózati adapterek tartozó toobe toothis alhálózat egy másik erőforráscsoportban található.</span><span class="sxs-lookup"><span data-stu-id="b73af-133">You need toodo this since hello NICs toobe associated toothis subnet are in a different resource group.</span></span>

    ```azurecli
    subnetId="$(azure network vnet subnet show --resource-group $existingRGName \
            --vnet-name $vnetName \
            --name $backendSubnetName|grep Id)"
    subnetId=${subnetId#*/}
    ```

   > [!TIP]
   > <span data-ttu-id="b73af-134">első parancs által használt fent hello [grep](http://tldp.org/LDP/Bash-Beginners-Guide/html/sect_04_02.html) és [karakterlánc-manipuláció](http://tldp.org/LDP/abs/html/string-manipulation.html) (pontosabban a substring eltávolítása).</span><span class="sxs-lookup"><span data-stu-id="b73af-134">hello first command above uses [grep](http://tldp.org/LDP/Bash-Beginners-Guide/html/sect_04_02.html) and [string manipulation](http://tldp.org/LDP/abs/html/string-manipulation.html) (more specifically, substring removal).</span></span>
   >

4. <span data-ttu-id="b73af-135">Beolvasni hello hello Azonosítóját `NSG-RemoteAccess` NSG.</span><span class="sxs-lookup"><span data-stu-id="b73af-135">Retrieve hello ID for hello `NSG-RemoteAccess` NSG.</span></span> <span data-ttu-id="b73af-136">Toodo ez szükséges hálózati adapterek toobe hello társított NSG van egy másik erőforráscsoportban található toothis óta.</span><span class="sxs-lookup"><span data-stu-id="b73af-136">You need toodo this since hello NICs toobe associated toothis NSG are in a different resource group.</span></span>

    ```azurecli
    nsgId="$(azure network nsg show --resource-group $existingRGName \
        --name $remoteAccessNSGName|grep Id)"
        nsgId=${nsgId#*/}
    ```

### <a name="step-2---create-necessary-resources-for-your-vms"></a><span data-ttu-id="b73af-137">2. lépés - a szükséges erőforrásokat létrehozni a virtuális géphez</span><span class="sxs-lookup"><span data-stu-id="b73af-137">Step 2 - Create necessary resources for your VMs</span></span>

1. <span data-ttu-id="b73af-138">Hozzon létre egy új erőforráscsoportot összes háttér-erőforrás.</span><span class="sxs-lookup"><span data-stu-id="b73af-138">Create a new resource group for all backend resources.</span></span> <span data-ttu-id="b73af-139">Értesítés hello használata hello `$backendRGName` hello az erőforráscsoport neve, a változó és `$location` a hello Azure-régiót.</span><span class="sxs-lookup"><span data-stu-id="b73af-139">Notice hello use of hello `$backendRGName` variable for hello resource group name, and `$location` for hello Azure region.</span></span>

    ```azurecli
    azure group create $backendRGName $location
    ```

2. <span data-ttu-id="b73af-140">Hozzon létre egy prémium szintű storage-fiók hello az operációs rendszer és a saját virtuális gépek által használt adatok lemezek toobe.</span><span class="sxs-lookup"><span data-stu-id="b73af-140">Create a premium storage account for hello OS and data disks toobe used by yours VMs.</span></span>

    ```azurecli
    azure storage account create $prmStorageAccountName \
        --resource-group $backendRGName \
        --location $location \
        --type PLRS
    ```

3. <span data-ttu-id="b73af-141">Rendelkezésre állási készlet hello virtuális gépek létrehozása.</span><span class="sxs-lookup"><span data-stu-id="b73af-141">Create an availability set for hello VMs.</span></span>

    ```azurecli
    azure availset create --resource-group $backendRGName \
        --location $location \
        --name $avSetName
    ```

### <a name="step-3---create-hello-nics-and-back-end-vms"></a><span data-ttu-id="b73af-142">3. lépés – hello hálózati adapter és a háttér-virtuális gépek létrehozása</span><span class="sxs-lookup"><span data-stu-id="b73af-142">Step 3 - Create hello NICs and back-end VMs</span></span>

1. <span data-ttu-id="b73af-143">Indítsa el a hurok toocreate több virtuális géphez, hello alapján `numberOfVMs` változók.</span><span class="sxs-lookup"><span data-stu-id="b73af-143">Start a loop toocreate multiple VMs, based on hello `numberOfVMs` variables.</span></span>

    ```azurecli
    for ((suffixNumber=1;suffixNumber<=numberOfVMs;suffixNumber++));
    do
    ```

2. <span data-ttu-id="b73af-144">Az egyes virtuális gépek hozzon létre egy hálózati Adaptert, az adatbázis eléréséhez.</span><span class="sxs-lookup"><span data-stu-id="b73af-144">For each VM, create a NIC for database access.</span></span>

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

3. <span data-ttu-id="b73af-145">Az egyes virtuális gépek hozzon létre egy hálózati Adaptert a távoli hozzáféréshez.</span><span class="sxs-lookup"><span data-stu-id="b73af-145">For each VM, create a NIC for remote access.</span></span> <span data-ttu-id="b73af-146">Értesítés hello `--network-security-group` paraméter, a használt tooassociate hello NIC tooan NSG.</span><span class="sxs-lookup"><span data-stu-id="b73af-146">Notice hello `--network-security-group` parameter, used tooassociate hello NIC tooan NSG.</span></span>

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

4. <span data-ttu-id="b73af-147">Hello virtuális gép létrehozása.</span><span class="sxs-lookup"><span data-stu-id="b73af-147">Create hello VM.</span></span>

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

5. <span data-ttu-id="b73af-148">Az egyes virtuális gépek, hozzon létre két adatok lemezek és záró hello hurok hello `done` parancsot.</span><span class="sxs-lookup"><span data-stu-id="b73af-148">For each VM, create two data disks, and end hello loop with hello `done` command.</span></span>

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

### <a name="step-4---run-hello-script"></a><span data-ttu-id="b73af-149">4. lépés: hello parancsfájl futtatása</span><span class="sxs-lookup"><span data-stu-id="b73af-149">Step 4 - Run hello script</span></span>
<span data-ttu-id="b73af-150">Most, hogy a letöltött és módosított hello parancsfájl futtatása vissza hello parancsfájl toocreate hello igényei szerint végén adatbázis virtuális gépek több hálózati adapterrel rendelkező.</span><span class="sxs-lookup"><span data-stu-id="b73af-150">Now that you downloaded and changed hello script based on your needs, run hello script toocreate hello back end database VMs with multiple NICs.</span></span>

1. <span data-ttu-id="b73af-151">Mentse a parancsfájlt, és futtassa azt a **Bash** terminál.</span><span class="sxs-lookup"><span data-stu-id="b73af-151">Save your script and run it from your **Bash** terminal.</span></span> <span data-ttu-id="b73af-152">Hello kezdeti kimenetben alább látható módon jelenik meg.</span><span class="sxs-lookup"><span data-stu-id="b73af-152">You will see hello initial output, as shown below.</span></span>
   
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
        info:    Looking up hello availability set "ASDB"
        info:    Creating availability set "ASDB"
        info:    availset create command OK
        info:    Executing command network nic create
        info:    Looking up hello network interface "NICDB1-DA"
        info:    Creating network interface "NICDB1-DA"
        info:    Looking up hello network interface "NICDB1-DA"
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
        info:    Looking up hello network interface "NICDB1-RA"
        info:    Creating network interface "NICDB1-RA"
        info:    Looking up hello network interface "NICDB1-RA"
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
        info:    Looking up hello VM "DB1"
        info:    Using hello VM Size "Standard_DS3"
        info:    hello [OS, Data] Disk or image configuration requires storage account
        info:    Looking up hello storage account wtestvnetstorageprm
        info:    Looking up hello availability set "ASDB"
        info:    Found an Availability set "ASDB"
        info:    Looking up hello NIC "NICDB1-DA"
        info:    Looking up hello NIC "NICDB1-RA"
        info:    Creating VM "DB1"
2. <span data-ttu-id="b73af-153">Néhány perc elteltével hello végrehajtása leáll, és látni fogja a hello részeinek hello kimeneti alább látható módon.</span><span class="sxs-lookup"><span data-stu-id="b73af-153">After a few minutes, hello execution will end and you will see hello rest of hello output as shown below.</span></span>
   
        info:    vm create command OK
        info:    Executing command vm disk attach-new
        info:    Looking up hello VM "DB1"
        info:    Looking up hello storage account wtestvnetstorageprm
        info:    New data disk location: https://wtestvnetstorageprm.blob.core.windows.net/vhds/datadisk1-1.vhd
        info:    Updating VM "DB1"
        info:    vm disk attach-new command OK
        info:    Executing command vm disk attach-new
        info:    Looking up hello VM "DB1"
        info:    Looking up hello storage account wtestvnetstorageprm
        info:    New data disk location: https://wtestvnetstorageprm.blob.core.windows.net/vhds/datadisk1-2.vhd
        info:    Updating VM "DB1"
        info:    vm disk attach-new command OK
        info:    Executing command network nic create
        info:    Looking up hello network interface "NICDB2-DA"
        info:    Creating network interface "NICDB2-DA"
        info:    Looking up hello network interface "NICDB2-DA"
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
        info:    Looking up hello network interface "NICDB2-RA"
        info:    Creating network interface "NICDB2-RA"
        info:    Looking up hello network interface "NICDB2-RA"
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
        info:    Looking up hello VM "DB2"
        info:    Using hello VM Size "Standard_DS3"
        info:    hello [OS, Data] Disk or image configuration requires storage account
        info:    Looking up hello storage account wtestvnetstorageprm
        info:    Looking up hello availability set "ASDB"
        info:    Found an Availability set "ASDB"
        info:    Looking up hello NIC "NICDB2-DA"
        info:    Looking up hello NIC "NICDB2-RA"
        info:    Creating VM "DB2"
        info:    vm create command OK
        info:    Executing command vm disk attach-new
        info:    Looking up hello VM "DB2"
        info:    Looking up hello storage account wtestvnetstorageprm
        info:    New data disk location: https://wtestvnetstorageprm.blob.core.windows.net/vhds/datadisk2-1.vhd
        info:    Updating VM "DB2"
        info:    vm disk attach-new command OK
        info:    Executing command vm disk attach-new
        info:    Looking up hello VM "DB2"
        info:    Looking up hello storage account wtestvnetstorageprm
        info:    New data disk location: https://wtestvnetstorageprm.blob.core.windows.net/vhds/datadisk2-2.vhd
        info:    Updating VM "DB2"
        info:    vm disk attach-new command OK

