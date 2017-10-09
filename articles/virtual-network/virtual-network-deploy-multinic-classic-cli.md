---
title: "virtuális gép (klasszikus) és több hálózati adapter - Azure CLI 1.0 aaaCreate |} Microsoft Docs"
description: "Ismerje meg, hogyan használja több hálózati adapterrel rendelkező virtuális gép (klasszikus) toocreate hello Azure parancssori felület (CLI) 1.0-s."
services: virtual-network
documentationcenter: na
author: jimdial
manager: timlt
editor: 
tags: azure-service-management
ms.assetid: b436e41e-866c-439f-a7c7-7b4b041725ef
ms.service: virtual-network
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 02/02/2016
ms.author: jdial
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 181bfb28027caff33410ca94744e79206a2a0d0c
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="create-a-vm-classic-with-multiple-nics-using-hello-azure-cli-10"></a><span data-ttu-id="ea12c-103">Hozzon létre egy virtuális gép (klasszikus) Azure CLI 1.0 hello segítségével több hálózati adapter</span><span class="sxs-lookup"><span data-stu-id="ea12c-103">Create a VM (Classic) with multiple NICs using hello Azure CLI 1.0</span></span>

[!INCLUDE [virtual-network-deploy-multinic-classic-selectors-include.md](../../includes/virtual-network-deploy-multinic-classic-selectors-include.md)]

<span data-ttu-id="ea12c-104">Virtuális gépek (VM) létrehozása az Azure-ban, és csatlakoztassa a virtuális gépek több hálózati adapterek (NIC) tooeach.</span><span class="sxs-lookup"><span data-stu-id="ea12c-104">You can create virtual machines (VMs) in Azure and attach multiple network interfaces (NICs) tooeach of your VMs.</span></span> <span data-ttu-id="ea12c-105">Több hálózati adapter választhatók szét a forgalomtípusok engedélyezze a hálózati adapter között.</span><span class="sxs-lookup"><span data-stu-id="ea12c-105">Multiple NICs enable separation of traffic types across NICs.</span></span> <span data-ttu-id="ea12c-106">Például egy hálózati adapter kommunikálhat a hello Internet, miközben egy másik kommunikál csak a belső erőforrásokhoz nem kapcsolódó toohello Internet.</span><span class="sxs-lookup"><span data-stu-id="ea12c-106">For example, one NIC might communicate with hello Internet, while another communicates only with internal resources not connected toohello Internet.</span></span> <span data-ttu-id="ea12c-107">hello képességét tooseparate hálózati forgalom több hálózati adapter között számos hálózati virtuális készülékeket, például az alkalmazások biztosításán és WAN-optimalizálást megoldások szükség.</span><span class="sxs-lookup"><span data-stu-id="ea12c-107">hello ability tooseparate network traffic across multiple NICs is required for many network virtual appliances, such as application delivery and WAN optimization solutions.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="ea12c-108">Az Azure két különböző üzembe helyezési modellel rendelkezik az erőforrások létrehozásához és használatához: [Resource Manager és klasszikus](../resource-manager-deployment-model.md).</span><span class="sxs-lookup"><span data-stu-id="ea12c-108">Azure has two different deployment models for creating and working with resources:  [Resource Manager and classic](../resource-manager-deployment-model.md).</span></span> <span data-ttu-id="ea12c-109">Ez a cikk hello klasszikus telepítési modell használatát bemutatja.</span><span class="sxs-lookup"><span data-stu-id="ea12c-109">This article covers using hello classic deployment model.</span></span> <span data-ttu-id="ea12c-110">A Microsoft azt javasolja, hogy az új telepítések esetén hello Resource Manager modellt használja.</span><span class="sxs-lookup"><span data-stu-id="ea12c-110">Microsoft recommends that most new deployments use hello Resource Manager model.</span></span> <span data-ttu-id="ea12c-111">Megtudhatja, hogyan tooperform hello használata a lépések [Resource Manager üzembe helyezési modellben](virtual-network-deploy-multinic-arm-cli.md).</span><span class="sxs-lookup"><span data-stu-id="ea12c-111">Learn how tooperform these steps using hello [Resource Manager deployment model](virtual-network-deploy-multinic-arm-cli.md).</span></span>

[!INCLUDE [virtual-network-deploy-multinic-scenario-include.md](../../includes/virtual-network-deploy-multinic-scenario-include.md)]

<span data-ttu-id="ea12c-112">hello következő lépések használják nevű erőforráscsoport *IaaSStory* hello webkiszolgálók és az erőforráscsoport neve *IaaSStory-háttérrendszer* hello DB kiszolgálók.</span><span class="sxs-lookup"><span data-stu-id="ea12c-112">hello following steps use a resource group named *IaaSStory* for hello WEB servers and a resource group named *IaaSStory-BackEnd* for hello DB servers.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="ea12c-113">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="ea12c-113">Prerequisites</span></span>
<span data-ttu-id="ea12c-114">Mielőtt hello DB kiszolgálók hozhat létre, meg kell-e toocreate hello *IaaSStory* erőforráscsoport összes hello szükséges erőforrások ehhez a forgatókönyvhöz.</span><span class="sxs-lookup"><span data-stu-id="ea12c-114">Before you can create hello DB servers, you need toocreate hello *IaaSStory* resource group with all hello necessary resources for this scenario.</span></span> <span data-ttu-id="ea12c-115">Ezeket az erőforrásokat, befejeződött a következő lépések hello toocreate.</span><span class="sxs-lookup"><span data-stu-id="ea12c-115">toocreate these resources, complete hello steps that follow.</span></span> <span data-ttu-id="ea12c-116">Hozzon létre egy virtuális hálózatot hello hello utasításait követve [hozzon létre egy virtuális hálózatot](virtual-networks-create-vnet-classic-cli.md) cikk.</span><span class="sxs-lookup"><span data-stu-id="ea12c-116">Create a virtual network by following hello steps in hello [Create a virtual network](virtual-networks-create-vnet-classic-cli.md) article.</span></span>

[!INCLUDE [azure-cli-prerequisites-include.md](../../includes/azure-cli-prerequisites-include.md)]

## <a name="deploy-hello-back-end-vms"></a><span data-ttu-id="ea12c-117">Hello háttér-telepítése virtuális gépeken</span><span class="sxs-lookup"><span data-stu-id="ea12c-117">Deploy hello back-end VMs</span></span>
<span data-ttu-id="ea12c-118">hello háttér virtuális gépek hello létrehozása a következő erőforrások hello függ:</span><span class="sxs-lookup"><span data-stu-id="ea12c-118">hello back-end VMs depend on hello creation of hello following resources:</span></span>

* <span data-ttu-id="ea12c-119">**Az adatlemezek tárfiók**.</span><span class="sxs-lookup"><span data-stu-id="ea12c-119">**Storage account for data disks**.</span></span> <span data-ttu-id="ea12c-120">A jobb teljesítmény érdekében a hello adatbázis-kiszolgálóin hello adatlemezek tartós állapotú meghajtót (SSD) technológiát, amely a prémium szintű tárfiók szükséges fogja használni.</span><span class="sxs-lookup"><span data-stu-id="ea12c-120">For better performance, hello data disks on hello database servers will use solid state drive (SSD) technology, which requires a premium storage account.</span></span> <span data-ttu-id="ea12c-121">Győződjön meg arról, hogy hello Azure-beli hely toosupport prémium szintű storage telepít.</span><span class="sxs-lookup"><span data-stu-id="ea12c-121">Make sure hello Azure location you deploy toosupport premium storage.</span></span>
* <span data-ttu-id="ea12c-122">**Hálózati adapter**.</span><span class="sxs-lookup"><span data-stu-id="ea12c-122">**NICs**.</span></span> <span data-ttu-id="ea12c-123">Minden virtuális gép lesz a két hálózati adapterrel, egy adatbázis-hozzáférési és felügyeleti egyet.</span><span class="sxs-lookup"><span data-stu-id="ea12c-123">Each VM will have two NICs, one for database access, and one for management.</span></span>
* <span data-ttu-id="ea12c-124">**A rendelkezésre állási csoport**.</span><span class="sxs-lookup"><span data-stu-id="ea12c-124">**Availability set**.</span></span> <span data-ttu-id="ea12c-125">Minden adatbázis-kiszolgálók megkapja tooa egyetlen rendelkezésre állási csoportot, hello virtuális gépek közül legalább egy tooensure megfelelően működik, és karbantartás során.</span><span class="sxs-lookup"><span data-stu-id="ea12c-125">All database servers will be added tooa single availability set, tooensure at least one of hello VMs is up and running during maintenance.</span></span>

### <a name="step-1---start-your-script"></a><span data-ttu-id="ea12c-126">1. lépés – a parancsfájl futtatásához</span><span class="sxs-lookup"><span data-stu-id="ea12c-126">Step 1 - Start your script</span></span>
<span data-ttu-id="ea12c-127">Letöltheti használt hello teljes bash parancsfájl [Itt](https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/IaaS-Story/11-MultiNIC/classic/virtual-network-deploy-multinic-classic-cli.sh).</span><span class="sxs-lookup"><span data-stu-id="ea12c-127">You can download hello full bash script used [here](https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/IaaS-Story/11-MultiNIC/classic/virtual-network-deploy-multinic-classic-cli.sh).</span></span> <span data-ttu-id="ea12c-128">Hajtsa végre a következő lépéseket toochange hello parancsfájl toowork környezetében hello:</span><span class="sxs-lookup"><span data-stu-id="ea12c-128">Complete hello following steps toochange hello script toowork in your environment:</span></span>

1. <span data-ttu-id="ea12c-129">Hello hello-változók értékeit az alábbi a meglévő erőforráscsoport üzembe helyezett fent alapján módosítása [Előfeltételek](#Prerequisites).</span><span class="sxs-lookup"><span data-stu-id="ea12c-129">Change hello values of hello variables below based on your existing resource group deployed above in [Prerequisites](#Prerequisites).</span></span>

    ```azurecli
    location="useast2"
    vnetName="WTestVNet"
    backendSubnetName="BackEnd"
    ```
2. <span data-ttu-id="ea12c-130">Hello értékek módosítása hello alábbi változók hello értékek alapján kívánt toouse a háttér-telepítéshez.</span><span class="sxs-lookup"><span data-stu-id="ea12c-130">Change hello values of hello variables below based on hello values you want toouse for your backend deployment.</span></span>

    ```azurecli
    backendCSName="IaaSStory-Backend"
    prmStorageAccountName="iaasstoryprmstorage"
    image="0b11de9248dd4d87b18621318e037d37__RightImage-Ubuntu-14.04-x64-v14.2.1"
    avSetName="ASDB"
    vmSize="Standard_DS3"
    diskSize=127
    vmNamePrefix="DB"
    osDiskName="osdiskdb"
    dataDiskPrefix="db"
    dataDiskName="datadisk"
    ipAddressPrefix="192.168.2."
    username='adminuser'
    password='adminP@ssw0rd'
    numberOfVMs=2
    ```

### <a name="step-2---create-necessary-resources-for-your-vms"></a><span data-ttu-id="ea12c-131">2. lépés - a szükséges erőforrásokat létrehozni a virtuális géphez</span><span class="sxs-lookup"><span data-stu-id="ea12c-131">Step 2 - Create necessary resources for your VMs</span></span>
1. <span data-ttu-id="ea12c-132">Hozzon létre egy új felhőszolgáltatás háttér virtuális gépen.</span><span class="sxs-lookup"><span data-stu-id="ea12c-132">Create a new cloud service for all backend VMs.</span></span> <span data-ttu-id="ea12c-133">Értesítés hello használata hello `$backendCSName` hello az erőforráscsoport neve, a változó és `$location` a hello Azure-régiót.</span><span class="sxs-lookup"><span data-stu-id="ea12c-133">Notice hello use of hello `$backendCSName` variable for hello resource group name, and `$location` for hello Azure region.</span></span>

    ```azurecli
    azure service create --serviceName $backendCSName \
        --location $location
    ```

2. <span data-ttu-id="ea12c-134">Hozzon létre egy prémium szintű storage-fiók hello az operációs rendszer és a saját virtuális gépek által használt adatok lemezek toobe.</span><span class="sxs-lookup"><span data-stu-id="ea12c-134">Create a premium storage account for hello OS and data disks toobe used by yours VMs.</span></span>

    ```azurecli
    azure storage account create $prmStorageAccountName \
        --location $location \
        --type PLRS
    ```

### <a name="step-3---create-vms-with-multiple-nics"></a><span data-ttu-id="ea12c-135">3. lépés - a több hálózati adapterrel rendelkező virtuális gépek létrehozása</span><span class="sxs-lookup"><span data-stu-id="ea12c-135">Step 3 - Create VMs with multiple NICs</span></span>
1. <span data-ttu-id="ea12c-136">Indítsa el a hurok toocreate több virtuális géphez, hello alapján `numberOfVMs` változók.</span><span class="sxs-lookup"><span data-stu-id="ea12c-136">Start a loop toocreate multiple VMs, based on hello `numberOfVMs` variables.</span></span>

    ```azurecli
    for ((suffixNumber=1;suffixNumber<=numberOfVMs;suffixNumber++));
    do
    ```

2. <span data-ttu-id="ea12c-137">Az egyes virtuális gépek adja meg a hello és az egyes hello két hálózati adapter IP-címet.</span><span class="sxs-lookup"><span data-stu-id="ea12c-137">For each VM, specify hello name and IP address of each of hello two NICs.</span></span>

    ```azurecli
    nic1Name=$vmNamePrefix$suffixNumber-DA
    x=$((suffixNumber+3))
    ipAddress1=$ipAddressPrefix$x

    nic2Name=$vmNamePrefix$suffixNumber-RA
    x=$((suffixNumber+53))
    ipAddress2=$ipAddressPrefix$x
    ```

3. <span data-ttu-id="ea12c-138">Hello virtuális gép létrehozása.</span><span class="sxs-lookup"><span data-stu-id="ea12c-138">Create hello VM.</span></span> <span data-ttu-id="ea12c-139">Figyelje meg hello hello használata `--nic-config` paramétert, az összes hálózati adapter neve, alhálózatot és IP-cím listáját tartalmazó.</span><span class="sxs-lookup"><span data-stu-id="ea12c-139">Notice hello usage of hello `--nic-config` parameter, containing a list of all NICs with name, subnet, and IP address.</span></span>

    ```azurecli
    azure vm create $backendCSName $image $username $password \
        --connect $backendCSName \
        --vm-name $vmNamePrefix$suffixNumber \
        --vm-size $vmSize \
        --availability-set $avSetName \
        --blob-url $prmStorageAccountName.blob.core.windows.net/vhds/$osDiskName$suffixNumber.vhd \
        --virtual-network-name $vnetName \
        --subnet-names $backendSubnetName \
        --nic-config $nic1Name:$backendSubnetName:$ipAddress1::,$nic2Name:$backendSubnetName:$ipAddress2::
    ```

4. <span data-ttu-id="ea12c-140">Az egyes virtuális gépek hozzon létre két adatlemezek.</span><span class="sxs-lookup"><span data-stu-id="ea12c-140">For each VM, create two data disks.</span></span>

    ```azurecli
    azure vm disk attach-new $vmNamePrefix$suffixNumber \
        $diskSize \
        vhds/$dataDiskPrefix$suffixNumber$dataDiskName-1.vhd

    azure vm disk attach-new $vmNamePrefix$suffixNumber \
        $diskSize \
        vhds/$dataDiskPrefix$suffixNumber$dataDiskName-2.vhd
    done
    ```

### <a name="step-4---run-hello-script"></a><span data-ttu-id="ea12c-141">4. lépés: hello parancsfájl futtatása</span><span class="sxs-lookup"><span data-stu-id="ea12c-141">Step 4 - Run hello script</span></span>
<span data-ttu-id="ea12c-142">Most, hogy a letöltött és módosított hello parancsfájl futtatása vissza hello parancsfájl toocreate hello igényei szerint végén adatbázis virtuális gépek több hálózati adapterrel rendelkező.</span><span class="sxs-lookup"><span data-stu-id="ea12c-142">Now that you downloaded and changed hello script based on your needs, run hello script toocreate hello back end database VMs with multiple NICs.</span></span>

1. <span data-ttu-id="ea12c-143">Mentse a parancsfájlt, és futtassa azt a **Bash** terminál.</span><span class="sxs-lookup"><span data-stu-id="ea12c-143">Save your script and run it from your **Bash** terminal.</span></span> <span data-ttu-id="ea12c-144">Hello kezdeti kimenetben alább látható módon jelenik meg.</span><span class="sxs-lookup"><span data-stu-id="ea12c-144">You will see hello initial output, as shown below.</span></span>

        info:    Executing command service create
        info:    Creating cloud service
        data:    Cloud service name IaaSStory-Backend
        info:    service create command OK
        info:    Executing command storage account create
        info:    Creating storage account
        info:    storage account create command OK
        info:    Executing command vm create
        info:    Looking up image 0b11de9248dd4d87b18621318e037d37__RightImage-Ubuntu-14.04-x64-v14.2.1
        info:    Looking up virtual network
        info:    Looking up cloud service
        info:    Getting cloud service properties
        info:    Looking up deployment
        info:    Creating VM

2. <span data-ttu-id="ea12c-145">Néhány perc elteltével hello végrehajtása leáll, és látni fogja a hello részeinek hello kimeneti alább látható módon.</span><span class="sxs-lookup"><span data-stu-id="ea12c-145">After a few minutes, hello execution will end and you will see hello rest of hello output as shown below.</span></span>

        info:    OK
        info:    vm create command OK
        info:    Executing command vm disk attach-new
        info:    Getting virtual machines
        info:    Adding Data-Disk
        info:    vm disk attach-new command OK
        info:    Executing command vm disk attach-new
        info:    Getting virtual machines
        info:    Adding Data-Disk
        info:    vm disk attach-new command OK
        info:    Executing command vm create
        info:    Looking up image 0b11de9248dd4d87b18621318e037d37__RightImage-Ubuntu-14.04-x64-v14.2.1
        info:    Looking up virtual network
        info:    Looking up cloud service
        info:    Getting cloud service properties
        info:    Looking up deployment
        info:    Creating VM
        info:    OK
        info:    vm create command OK
        info:    Executing command vm disk attach-new
        info:    Getting virtual machines
        info:    Adding Data-Disk
        info:    vm disk attach-new command OK
        info:    Executing command vm disk attach-new
        info:    Getting virtual machines
        info:    Adding Data-Disk
        info:    vm disk attach-new command OK
