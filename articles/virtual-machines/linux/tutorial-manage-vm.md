---
title: "aaaCreate és a Linux virtuális gépek kezelése a hello Azure parancssori Felülettel |} Microsoft Docs"
description: "Az oktatóanyag - létrehozása és kezelése a Linux virtuális gépek a hello Azure parancssori felület"
services: virtual-machines-linux
documentationcenter: virtual-machines
author: neilpeterson
manager: timlt
editor: tysonn
tags: azure-service-management
ms.assetid: 
ms.service: virtual-machines-linux
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure
ms.date: 05/02/2017
ms.author: nepeters
ms.custom: mvc
ms.openlocfilehash: 05f7c1cf860f809bc13f110778d3bddd619ac6f1
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="create-and-manage-linux-vms-with-hello-azure-cli"></a><span data-ttu-id="b10c1-103">Hozzon létre, és a Linux virtuális gépek kezelése a hello Azure parancssori felület</span><span class="sxs-lookup"><span data-stu-id="b10c1-103">Create and Manage Linux VMs with hello Azure CLI</span></span>

<span data-ttu-id="b10c1-104">Az Azure virtuális gépek adjon meg egy teljes mértékben konfigurálhatók és rugalmas számítási környezet.</span><span class="sxs-lookup"><span data-stu-id="b10c1-104">Azure virtual machines provide a fully configurable and flexible computing environment.</span></span> <span data-ttu-id="b10c1-105">Ez az oktatóanyag ismerteti alapszintű Azure-beli virtuális gép telepítési elemek, például Virtuálisgép-méret kiválasztása, Virtuálisgép-lemezkép kiválasztása és a virtuális gépek telepítése.</span><span class="sxs-lookup"><span data-stu-id="b10c1-105">This tutorial covers basic Azure virtual machine deployment items such as selecting a VM size, selecting a VM image, and deploying a VM.</span></span> <span data-ttu-id="b10c1-106">Az alábbiak végrehajtásának módját ismerheti meg:</span><span class="sxs-lookup"><span data-stu-id="b10c1-106">You learn how to:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="b10c1-107">Hozzon létre, és csatlakozzon a virtuális gép tooa</span><span class="sxs-lookup"><span data-stu-id="b10c1-107">Create and connect tooa VM</span></span>
> * <span data-ttu-id="b10c1-108">Válassza ki és használja a Virtuálisgép-rendszerképek</span><span class="sxs-lookup"><span data-stu-id="b10c1-108">Select and use VM images</span></span>
> * <span data-ttu-id="b10c1-109">Megtekintése és használata a megadott Virtuálisgép-méretek</span><span class="sxs-lookup"><span data-stu-id="b10c1-109">View and use specific VM sizes</span></span>
> * <span data-ttu-id="b10c1-110">Virtuális gép átméretezése</span><span class="sxs-lookup"><span data-stu-id="b10c1-110">Resize a VM</span></span>
> * <span data-ttu-id="b10c1-111">Megtekinteni és megérteni a virtuális gép állapota</span><span class="sxs-lookup"><span data-stu-id="b10c1-111">View and understand VM state</span></span>


[!INCLUDE [cloud-shell-try-it.md](../../../includes/cloud-shell-try-it.md)]

<span data-ttu-id="b10c1-112">Ha Ön tooinstall kiválasztása és hello CLI helyileg, ez az oktatóanyag van szükség, hogy verzióját hello Azure CLI 2.0.4 vagy újabb.</span><span class="sxs-lookup"><span data-stu-id="b10c1-112">If you choose tooinstall and use hello CLI locally, this tutorial requires that you are running hello Azure CLI version 2.0.4 or later.</span></span> <span data-ttu-id="b10c1-113">Futtatás `az --version` toofind hello verziója.</span><span class="sxs-lookup"><span data-stu-id="b10c1-113">Run `az --version` toofind hello version.</span></span> <span data-ttu-id="b10c1-114">Ha tooinstall vagy frissítés van szüksége, tekintse meg [Azure CLI 2.0 telepítése]( /cli/azure/install-azure-cli).</span><span class="sxs-lookup"><span data-stu-id="b10c1-114">If you need tooinstall or upgrade, see [Install Azure CLI 2.0]( /cli/azure/install-azure-cli).</span></span> 

## <a name="create-resource-group"></a><span data-ttu-id="b10c1-115">Erőforráscsoport létrehozása</span><span class="sxs-lookup"><span data-stu-id="b10c1-115">Create resource group</span></span>

<span data-ttu-id="b10c1-116">Hozzon létre egy erőforráscsoportot hello [az csoport létrehozása](https://docs.microsoft.com/cli/azure/group#create) parancsot.</span><span class="sxs-lookup"><span data-stu-id="b10c1-116">Create a resource group with hello [az group create](https://docs.microsoft.com/cli/azure/group#create) command.</span></span> 

<span data-ttu-id="b10c1-117">Az Azure-erőforráscsoport olyan logikai tároló, amelybe a rendszer üzembe helyezi és kezeli az Azure-erőforrásokat.</span><span class="sxs-lookup"><span data-stu-id="b10c1-117">An Azure resource group is a logical container into which Azure resources are deployed and managed.</span></span> <span data-ttu-id="b10c1-118">Egy erőforráscsoportot a virtuális gépek előtt létre kell hozni.</span><span class="sxs-lookup"><span data-stu-id="b10c1-118">A resource group must be created before a virtual machine.</span></span> <span data-ttu-id="b10c1-119">Ebben a példában az erőforráscsoport neve *myResourceGroupVM* jön létre az hello *eastus* régióban.</span><span class="sxs-lookup"><span data-stu-id="b10c1-119">In this example, a resource group named *myResourceGroupVM* is created in hello *eastus* region.</span></span> 

```azurecli-interactive 
az group create --name myResourceGroupVM --location eastus
```

<span data-ttu-id="b10c1-120">hello erőforráscsoport létrehozása vagy módosítása egy virtuális Gépet, amelynek ez az oktatóanyag teljes látható során van megadva.</span><span class="sxs-lookup"><span data-stu-id="b10c1-120">hello resource group is specified when creating or modifying a VM, which can be seen throughout this tutorial.</span></span>

## <a name="create-virtual-machine"></a><span data-ttu-id="b10c1-121">Virtuális gép létrehozása</span><span class="sxs-lookup"><span data-stu-id="b10c1-121">Create virtual machine</span></span>

<span data-ttu-id="b10c1-122">Hozzon létre egy virtuális gép hello [az virtuális gép létrehozása](https://docs.microsoft.com/cli/azure/vm#create) parancsot.</span><span class="sxs-lookup"><span data-stu-id="b10c1-122">Create a virtual machine with hello [az vm create](https://docs.microsoft.com/cli/azure/vm#create) command.</span></span> 

<span data-ttu-id="b10c1-123">A virtuális gép létrehozásakor több lehetőség is elérhető, például az operációs rendszer lemezképét, a lemez méretezés és a felügyeleti hitelesítő adatokat.</span><span class="sxs-lookup"><span data-stu-id="b10c1-123">When creating a virtual machine, several options are available such as operating system image, disk sizing, and administrative credentials.</span></span> <span data-ttu-id="b10c1-124">Ebben a példában egy virtuális gép létrehozása nevű *myVM* Ubuntu Server operációs rendszert futtató.</span><span class="sxs-lookup"><span data-stu-id="b10c1-124">In this example, a virtual machine is created with a name of *myVM* running Ubuntu Server.</span></span> 

```azurecli-interactive 
az vm create --resource-group myResourceGroupVM --name myVM --image UbuntuLTS --generate-ssh-keys
```

<span data-ttu-id="b10c1-125">Egyszer hello a virtuális gép létrehozása, hello Azure CLI kimenete hello VM kapcsolatos információkat.</span><span class="sxs-lookup"><span data-stu-id="b10c1-125">Once hello VM has been created, hello Azure CLI outputs information about hello VM.</span></span> <span data-ttu-id="b10c1-126">Jegyezze fel a hello `publicIpAddress`, ez a cím lehet használt tooaccess hello virtuális géphez...</span><span class="sxs-lookup"><span data-stu-id="b10c1-126">Take note of hello `publicIpAddress`, this address can be used tooaccess hello virtual machine..</span></span> 

```azurecli-interactive 
{
  "fqdns": "",
  "id": "/subscriptions/d5b9d4b7-6fc1-0000-0000-000000000000/resourceGroups/myResourceGroupVM/providers/Microsoft.Compute/virtualMachines/myVM",
  "location": "eastus",
  "macAddress": "00-0D-3A-23-9A-49",
  "powerState": "VM running",
  "privateIpAddress": "10.0.0.4",
  "publicIpAddress": "52.174.34.95",
  "resourceGroup": "myResourceGroupVM"
}
```

## <a name="connect-toovm"></a><span data-ttu-id="b10c1-127">Csatlakozás tooVM</span><span class="sxs-lookup"><span data-stu-id="b10c1-127">Connect tooVM</span></span>

<span data-ttu-id="b10c1-128">Csatlakoztathatja toohello virtuális gép SSH használatával.</span><span class="sxs-lookup"><span data-stu-id="b10c1-128">You can now connect toohello VM using SSH.</span></span> <span data-ttu-id="b10c1-129">Cserélje le hello példa IP-cím hello `publicIpAddress` hello előző lépésben feljegyzett.</span><span class="sxs-lookup"><span data-stu-id="b10c1-129">Replace hello example IP address with hello `publicIpAddress` noted in hello previous step.</span></span>

```bash
ssh 52.174.34.95
```

<span data-ttu-id="b10c1-130">Miután befejeződött a virtuális gép hello, zárja be a hello SSH-munkamenetet.</span><span class="sxs-lookup"><span data-stu-id="b10c1-130">Once finished with hello VM, close hello SSH session.</span></span> 

```bash
exit
```

## <a name="understand-vm-images"></a><span data-ttu-id="b10c1-131">Virtuálisgép-rendszerképek ismertetése</span><span class="sxs-lookup"><span data-stu-id="b10c1-131">Understand VM images</span></span>

<span data-ttu-id="b10c1-132">hello Azure piactér számos rendszerképet, amely használt toocreate virtuális gépeket tartalmaz.</span><span class="sxs-lookup"><span data-stu-id="b10c1-132">hello Azure marketplace includes many images that can be used toocreate VMs.</span></span> <span data-ttu-id="b10c1-133">Hello előző lépésekben egy virtuális gép létrehozása egy Ubuntu lemezképpel.</span><span class="sxs-lookup"><span data-stu-id="b10c1-133">In hello previous steps, a virtual machine was created using an Ubuntu image.</span></span> <span data-ttu-id="b10c1-134">Ebben a lépésben hello Azure CLI a CentOS képet, majd használt toosearch hello piactérre toodeploy egy második virtuális gép használja.</span><span class="sxs-lookup"><span data-stu-id="b10c1-134">In this step, hello Azure CLI is used toosearch hello marketplace for a CentOS image, which is then used toodeploy a second virtual machine.</span></span>  

<span data-ttu-id="b10c1-135">hello listája toosee leggyakrabban használt képek, használja a hello [az vm képlistában](/cli/azure/vm/image#list) parancsot.</span><span class="sxs-lookup"><span data-stu-id="b10c1-135">toosee a list of hello most commonly used images, use hello [az vm image list](/cli/azure/vm/image#list) command.</span></span>

```azurecli-interactive 
az vm image list --output table
```

<span data-ttu-id="b10c1-136">hello parancs kimenetében hello legnépszerűbb Virtuálisgép-rendszerképek az Azure ad vissza.</span><span class="sxs-lookup"><span data-stu-id="b10c1-136">hello command output returns hello most popular VM images on Azure.</span></span>

```bash
Offer          Publisher               Sku                 Urn                                                             UrnAlias             Version
-------------  ----------------------  ------------------  --------------------------------------------------------------  -------------------  ---------
WindowsServer  MicrosoftWindowsServer  2016-Datacenter     MicrosoftWindowsServer:WindowsServer:2016-Datacenter:latest     Win2016Datacenter    latest
WindowsServer  MicrosoftWindowsServer  2012-R2-Datacenter  MicrosoftWindowsServer:WindowsServer:2012-R2-Datacenter:latest  Win2012R2Datacenter  latest
WindowsServer  MicrosoftWindowsServer  2008-R2-SP1         MicrosoftWindowsServer:WindowsServer:2008-R2-SP1:latest         Win2008R2SP1         latest
WindowsServer  MicrosoftWindowsServer  2012-Datacenter     MicrosoftWindowsServer:WindowsServer:2012-Datacenter:latest     Win2012Datacenter    latest
UbuntuServer   Canonical               16.04-LTS           Canonical:UbuntuServer:16.04-LTS:latest                         UbuntuLTS            latest
CentOS         OpenLogic               7.3                 OpenLogic:CentOS:7.3:latest                                     CentOS               latest
openSUSE-Leap  SUSE                    42.2                SUSE:openSUSE-Leap:42.2:latest                                  openSUSE-Leap        latest
RHEL           RedHat                  7.3                 RedHat:RHEL:7.3:latest                                          RHEL                 latest
SLES           SUSE                    12-SP2              SUSE:SLES:12-SP2:latest                                         SLES                 latest
Debian         credativ                8                   credativ:Debian:8:latest                                        Debian               latest
CoreOS         CoreOS                  Stable              CoreOS:CoreOS:Stable:latest                                     CoreOS               latest
```

<span data-ttu-id="b10c1-137">Teljes listája látható hello hozzáadásával `--all` argumentum.</span><span class="sxs-lookup"><span data-stu-id="b10c1-137">A full list can be seen by adding hello `--all` argument.</span></span> <span data-ttu-id="b10c1-138">hello képlistában is alapján szűrhetők `--publisher` vagy `–-offer`.</span><span class="sxs-lookup"><span data-stu-id="b10c1-138">hello image list can also be filtered by `--publisher` or `–-offer`.</span></span> <span data-ttu-id="b10c1-139">Ebben a példában az ajánlat, amely megfelel az összes képekkel szűrt hello lista *CentOS*.</span><span class="sxs-lookup"><span data-stu-id="b10c1-139">In this example, hello list is filtered for all images with an offer that matches *CentOS*.</span></span> 

```azurecli-interactive 
az vm image list --offer CentOS --all --output table
```

<span data-ttu-id="b10c1-140">Részleges kimenete:</span><span class="sxs-lookup"><span data-stu-id="b10c1-140">Partial output:</span></span>

```azurecli-interactive 
Offer             Publisher         Sku   Urn                                     Version
----------------  ----------------  ----  --------------------------------------  -----------
CentOS            OpenLogic         6.5   OpenLogic:CentOS:6.5:6.5.201501         6.5.201501
CentOS            OpenLogic         6.5   OpenLogic:CentOS:6.5:6.5.201503         6.5.201503
CentOS            OpenLogic         6.5   OpenLogic:CentOS:6.5:6.5.201506         6.5.201506
CentOS            OpenLogic         6.5   OpenLogic:CentOS:6.5:6.5.20150904       6.5.20150904
CentOS            OpenLogic         6.5   OpenLogic:CentOS:6.5:6.5.20160309       6.5.20160309
CentOS            OpenLogic         6.5   OpenLogic:CentOS:6.5:6.5.20170207       6.5.20170207
```

<span data-ttu-id="b10c1-141">egy virtuális gép egy adott lemezképpel toodeploy hello értéket vegye figyelembe hello *Urn* oszlop.</span><span class="sxs-lookup"><span data-stu-id="b10c1-141">toodeploy a VM using a specific image, take note of hello value in hello *Urn* column.</span></span> <span data-ttu-id="b10c1-142">Hello kép megadásakor hello kép verziószám lehet cserélni "legújabb" hello hello terjesztési legújabb verzióját választja ki, amelyek.</span><span class="sxs-lookup"><span data-stu-id="b10c1-142">When specifying hello image, hello image version number can be replaced with “latest”, which selects hello latest version of hello distribution.</span></span> <span data-ttu-id="b10c1-143">Ebben a példában hello `--image` argumentum használt toospecify hello legújabb verzióját a CentOS 6.5-lemezkép.</span><span class="sxs-lookup"><span data-stu-id="b10c1-143">In this example, hello `--image` argument is used toospecify hello latest version of a CentOS 6.5 image.</span></span>  

```azurecli-interactive 
az vm create --resource-group myResourceGroupVM --name myVM2 --image OpenLogic:CentOS:6.5:latest --generate-ssh-keys
```

## <a name="understand-vm-sizes"></a><span data-ttu-id="b10c1-144">Virtuálisgép-méretek ismertetése</span><span class="sxs-lookup"><span data-stu-id="b10c1-144">Understand VM sizes</span></span>

<span data-ttu-id="b10c1-145">A virtuális gép méretét hello számítási erőforrásokat, elérhető toohello virtuális gépen végrehajtott például a CPU, grafikus Processzor és memória mennyisége határozza meg.</span><span class="sxs-lookup"><span data-stu-id="b10c1-145">A virtual machine size determines hello amount of compute resources such as CPU, GPU, and memory that are made available toohello virtual machine.</span></span> <span data-ttu-id="b10c1-146">Virtuális gépek várt hello munkaterhelés méretének toobe kell.</span><span class="sxs-lookup"><span data-stu-id="b10c1-146">Virtual machines need toobe sized appropriately for hello expected work load.</span></span> <span data-ttu-id="b10c1-147">Ha növeli a munkaterhelés, egy meglévő virtuális gépet is átméretezhetők.</span><span class="sxs-lookup"><span data-stu-id="b10c1-147">If workload increases, an existing virtual machine can be resized.</span></span>

### <a name="vm-sizes"></a><span data-ttu-id="b10c1-148">Virtuálisgép-méretek</span><span class="sxs-lookup"><span data-stu-id="b10c1-148">VM Sizes</span></span>

<span data-ttu-id="b10c1-149">a következő táblázat hello méretek kategorizálja a használati esetek.</span><span class="sxs-lookup"><span data-stu-id="b10c1-149">hello following table categorizes sizes into use cases.</span></span>  

| <span data-ttu-id="b10c1-150">Típus</span><span class="sxs-lookup"><span data-stu-id="b10c1-150">Type</span></span>                     | <span data-ttu-id="b10c1-151">Méretek</span><span class="sxs-lookup"><span data-stu-id="b10c1-151">Sizes</span></span>           |    <span data-ttu-id="b10c1-152">Leírás</span><span class="sxs-lookup"><span data-stu-id="b10c1-152">Description</span></span>       |
|--------------------------|-------------------|------------------------------------------------------------------------------------------------------------------------------------|
| [<span data-ttu-id="b10c1-153">Általános célú</span><span class="sxs-lookup"><span data-stu-id="b10c1-153">General purpose</span></span>](sizes-general.md)         |<span data-ttu-id="b10c1-154">Dsv3, Dv3, DSv2, Dv2, DS, D, Av2, A0-7</span><span class="sxs-lookup"><span data-stu-id="b10c1-154">Dsv3, Dv3, DSv2, Dv2, DS, D, Av2, A0-7</span></span>| <span data-ttu-id="b10c1-155">Elosztott terhelésű CPU-memória.</span><span class="sxs-lookup"><span data-stu-id="b10c1-155">Balanced CPU-to-memory.</span></span> <span data-ttu-id="b10c1-156">Épp ezért tökéletes választás a fejlesztési / tesztelési és kis toomedium alkalmazások és adatok megoldások.</span><span class="sxs-lookup"><span data-stu-id="b10c1-156">Ideal for dev / test and small toomedium applications and data solutions.</span></span>  |
| [<span data-ttu-id="b10c1-157">Számításra optimalizált</span><span class="sxs-lookup"><span data-stu-id="b10c1-157">Compute optimized</span></span>](sizes-compute.md)   | <span data-ttu-id="b10c1-158">FS, F</span><span class="sxs-lookup"><span data-stu-id="b10c1-158">Fs, F</span></span>             | <span data-ttu-id="b10c1-159">Magas CPU-memória.</span><span class="sxs-lookup"><span data-stu-id="b10c1-159">High CPU-to-memory.</span></span> <span data-ttu-id="b10c1-160">Jó közepes forgalom alkalmazásokat, hálózati berendezéseket és kötegelt folyamatok.</span><span class="sxs-lookup"><span data-stu-id="b10c1-160">Good for medium traffic applications, network appliances, and batch processes.</span></span>        |
| [<span data-ttu-id="b10c1-161">Memóriaoptimalizált</span><span class="sxs-lookup"><span data-stu-id="b10c1-161">Memory optimized</span></span>](../virtual-machines-windows-sizes-memory.md)    | <span data-ttu-id="b10c1-162">Esv3, Ev3, M, GS, G, DSv2, DS, Dv2, D</span><span class="sxs-lookup"><span data-stu-id="b10c1-162">Esv3, Ev3, M, GS, G, DSv2, DS, Dv2, D</span></span>   | <span data-ttu-id="b10c1-163">Magas memória-core.</span><span class="sxs-lookup"><span data-stu-id="b10c1-163">High memory-to-core.</span></span> <span data-ttu-id="b10c1-164">A relációs adatbázisok, közepes méretű toolarge gyorsítótárak és memórián belüli analytics nagy.</span><span class="sxs-lookup"><span data-stu-id="b10c1-164">Great for relational databases, medium toolarge caches, and in-memory analytics.</span></span>                 |
| [<span data-ttu-id="b10c1-165">Tárolásra optimalizált</span><span class="sxs-lookup"><span data-stu-id="b10c1-165">Storage optimized</span></span>](../virtual-machines-windows-sizes-storage.md)      | <span data-ttu-id="b10c1-166">Ls</span><span class="sxs-lookup"><span data-stu-id="b10c1-166">Ls</span></span>                | <span data-ttu-id="b10c1-167">Magas lemez-adatátviteli és I/O-műveleti jellemzők.</span><span class="sxs-lookup"><span data-stu-id="b10c1-167">High disk throughput and IO.</span></span> <span data-ttu-id="b10c1-168">Ideális Big Data-, SQL- és NoSQL-adatbázisok esetén.</span><span class="sxs-lookup"><span data-stu-id="b10c1-168">Ideal for Big Data, SQL, and NoSQL databases.</span></span>                                                         |
| [<span data-ttu-id="b10c1-169">GPU</span><span class="sxs-lookup"><span data-stu-id="b10c1-169">GPU</span></span>](sizes-gpu.md)          | <span data-ttu-id="b10c1-170">PORTOK HV, NC</span><span class="sxs-lookup"><span data-stu-id="b10c1-170">NV, NC</span></span>            | <span data-ttu-id="b10c1-171">Nagy mennyiségű grafikus megjelenítési és videó szerkesztése szánt speciális virtuális gépeket.</span><span class="sxs-lookup"><span data-stu-id="b10c1-171">Specialized VMs targeted for heavy graphic rendering and video editing.</span></span>       |
| [<span data-ttu-id="b10c1-172">Magas teljesítmény</span><span class="sxs-lookup"><span data-stu-id="b10c1-172">High performance</span></span>](sizes-hpc.md) | <span data-ttu-id="b10c1-173">H, A8-11</span><span class="sxs-lookup"><span data-stu-id="b10c1-173">H, A8-11</span></span>          | <span data-ttu-id="b10c1-174">A leghatékonyabb CPU rendelkező virtuális gépek nagy átviteli további hálózati adapterek (RDMA).</span><span class="sxs-lookup"><span data-stu-id="b10c1-174">Our most powerful CPU VMs with optional high-throughput network interfaces (RDMA).</span></span> 


### <a name="find-available-vm-sizes"></a><span data-ttu-id="b10c1-175">Elérhető Virtuálisgép-méretek keresése</span><span class="sxs-lookup"><span data-stu-id="b10c1-175">Find available VM sizes</span></span>

<span data-ttu-id="b10c1-176">virtuális gép listájának toosee méretének egy adott régióban, használja a hello [az vm-méretek listázása](/cli/azure/vm#list-sizes) parancsot.</span><span class="sxs-lookup"><span data-stu-id="b10c1-176">toosee a list of VM sizes available in a particular region, use hello [az vm list-sizes](/cli/azure/vm#list-sizes) command.</span></span> 

```azurecli-interactive 
az vm list-sizes --location eastus --output table
```

<span data-ttu-id="b10c1-177">Részleges kimenete:</span><span class="sxs-lookup"><span data-stu-id="b10c1-177">Partial output:</span></span>

```azurecli-interactive 
  MaxDataDiskCount    MemoryInMb  Name                      NumberOfCores    OsDiskSizeInMb    ResourceDiskSizeInMb
------------------  ------------  ----------------------  ---------------  ----------------  ----------------------
                 2          3584  Standard_DS1                          1           1047552                    7168
                 4          7168  Standard_DS2                          2           1047552                   14336
                 8         14336  Standard_DS3                          4           1047552                   28672
                16         28672  Standard_DS4                          8           1047552                   57344
                 4         14336  Standard_DS11                         2           1047552                   28672
                 8         28672  Standard_DS12                         4           1047552                   57344
                16         57344  Standard_DS13                         8           1047552                  114688
                32        114688  Standard_DS14                        16           1047552                  229376
                 1           768  Standard_A0                           1           1047552                   20480
                 2          1792  Standard_A1                           1           1047552                   71680
                 4          3584  Standard_A2                           2           1047552                  138240
                 8          7168  Standard_A3                           4           1047552                  291840
                 4         14336  Standard_A5                           2           1047552                  138240
                16         14336  Standard_A4                           8           1047552                  619520
                 8         28672  Standard_A6                           4           1047552                  291840
                16         57344  Standard_A7                           8           1047552                  619520
```

### <a name="create-vm-with-specific-size"></a><span data-ttu-id="b10c1-178">Meghatározott méretű virtuális gép létrehozása</span><span class="sxs-lookup"><span data-stu-id="b10c1-178">Create VM with specific size</span></span>

<span data-ttu-id="b10c1-179">Hello előző virtuális gép létrehozása a példában a mérete nem adta meg, melyik eredmények alapértelmezett mérete.</span><span class="sxs-lookup"><span data-stu-id="b10c1-179">In hello previous VM creation example, a size was not provided, which results in a default size.</span></span> <span data-ttu-id="b10c1-180">A Virtuálisgép-méret választható létrehozási ideje az [az virtuális gép létrehozása](/cli/azure/vm#create) és hello `--size` argumentum.</span><span class="sxs-lookup"><span data-stu-id="b10c1-180">A VM size can be selected at creation time using [az vm create](/cli/azure/vm#create) and hello `--size` argument.</span></span> 

```azurecli-interactive 
az vm create \
    --resource-group myResourceGroupVM \
    --name myVM3 \
    --image UbuntuLTS \
    --size Standard_F4s \
    --generate-ssh-keys
```

### <a name="resize-a-vm"></a><span data-ttu-id="b10c1-181">Virtuális gép átméretezése</span><span class="sxs-lookup"><span data-stu-id="b10c1-181">Resize a VM</span></span>

<span data-ttu-id="b10c1-182">A virtuális gépek telepítése után az átméretezett tooincrease kell, vagy erőforrás-elosztás csökkentése.</span><span class="sxs-lookup"><span data-stu-id="b10c1-182">After a VM has been deployed, it can be resized tooincrease or decrease resource allocation.</span></span>

<span data-ttu-id="b10c1-183">A virtuális gépek átméretezésével előtt ellenőrizze, hogy hello kívánt mérete hello jelenlegi Azure-fürttel érhető el.</span><span class="sxs-lookup"><span data-stu-id="b10c1-183">Before resizing a VM, check if hello desired size is available on hello current Azure cluster.</span></span> <span data-ttu-id="b10c1-184">Hello [az vm-vm-átméretezési-beállításai](/cli/azure/vm#list-vm-resize-options) parancs beolvasása hello méretének megtekintéséhez.</span><span class="sxs-lookup"><span data-stu-id="b10c1-184">hello [az vm list-vm-resize-options](/cli/azure/vm#list-vm-resize-options) command returns hello list of sizes.</span></span> 

```azurecli-interactive 
az vm list-vm-resize-options --resource-group myResourceGroupVM --name myVM --query [].name
```
<span data-ttu-id="b10c1-185">Hello szükséges méret érhető el, ha hello VM átméretezhető az alkalmazás bekapcsolja a állapotból, azonban hello művelet során újraindítása után.</span><span class="sxs-lookup"><span data-stu-id="b10c1-185">If hello desired size is available, hello VM can be resized from a powered-on state, however it is rebooted during hello operation.</span></span> <span data-ttu-id="b10c1-186">Használjon hello [az vm átméretezése]( /cli/azure/vm#resize) parancs tooperform hello méretezze át.</span><span class="sxs-lookup"><span data-stu-id="b10c1-186">Use hello [az vm resize]( /cli/azure/vm#resize) command tooperform hello resize.</span></span>

```azurecli-interactive 
az vm resize --resource-group myResourceGroupVM --name myVM --size Standard_DS4_v2
```

<span data-ttu-id="b10c1-187">Hello méretének igény nincs fürtön hello aktuális hello virtuális gép igényeinek felszabadítása előtt hello átméretezése művelet toobe akkor fordulhat elő.</span><span class="sxs-lookup"><span data-stu-id="b10c1-187">If hello desired size is not on hello current cluster, hello VM needs toobe deallocated before hello resize operation can occur.</span></span> <span data-ttu-id="b10c1-188">Használjon hello [az virtuális gép felszabadítása]( /cli/azure/vm#deallocate) toostop parancsot, és hello VM felszabadítani.</span><span class="sxs-lookup"><span data-stu-id="b10c1-188">Use hello [az vm deallocate]( /cli/azure/vm#deallocate) command toostop and deallocate hello VM.</span></span> <span data-ttu-id="b10c1-189">Vegye figyelembe, ha hello VM regisztráló vissza, eltávolíthatja a hello ideiglenes lemezen lévő adatokhoz.</span><span class="sxs-lookup"><span data-stu-id="b10c1-189">Note, when hello VM is powered back on, any data on hello temp disk may be removed.</span></span> <span data-ttu-id="b10c1-190">hello nyilvános IP-cím is megváltozik, kivéve, ha a statikus IP-cím használatban van.</span><span class="sxs-lookup"><span data-stu-id="b10c1-190">hello public IP address also changes unless a static IP address is being used.</span></span> 

```azurecli-interactive 
az vm deallocate --resource-group myResourceGroupVM --name myVM
```

<span data-ttu-id="b10c1-191">Miután felszabadítása. lehetséges, hello átméretezési akkor fordulhat elő.</span><span class="sxs-lookup"><span data-stu-id="b10c1-191">Once deallocated, hello resize can occur.</span></span> 

```azurecli-interactive 
az vm resize --resource-group myResourceGroupVM --name myVM --size Standard_GS1
```

<span data-ttu-id="b10c1-192">Miután hello átméretezéséhez hello VM indíthatók el.</span><span class="sxs-lookup"><span data-stu-id="b10c1-192">After hello resize, hello VM can be started.</span></span>

```azurecli-interactive 
az vm start --resource-group myResourceGroupVM --name myVM
```

## <a name="vm-power-states"></a><span data-ttu-id="b10c1-193">Virtuális gép energiaállapotokat</span><span class="sxs-lookup"><span data-stu-id="b10c1-193">VM power states</span></span>

<span data-ttu-id="b10c1-194">Egy Azure virtuális gép sok energiaállapotát egyike lehet.</span><span class="sxs-lookup"><span data-stu-id="b10c1-194">An Azure VM can have one of many power states.</span></span> <span data-ttu-id="b10c1-195">Ebben az állapotban hello hello virtuális gép aktuális állapotát jeleníti meg hello hipervizor hello tükrözik.</span><span class="sxs-lookup"><span data-stu-id="b10c1-195">This state represents hello current state of hello VM from hello standpoint of hello hypervisor.</span></span> 

### <a name="power-states"></a><span data-ttu-id="b10c1-196">Energiaállapotokat</span><span class="sxs-lookup"><span data-stu-id="b10c1-196">Power states</span></span>

| <span data-ttu-id="b10c1-197">Energiaállapot</span><span class="sxs-lookup"><span data-stu-id="b10c1-197">Power State</span></span> | <span data-ttu-id="b10c1-198">Leírás</span><span class="sxs-lookup"><span data-stu-id="b10c1-198">Description</span></span>
|----|----|
| <span data-ttu-id="b10c1-199">Indulás alatt</span><span class="sxs-lookup"><span data-stu-id="b10c1-199">Starting</span></span> | <span data-ttu-id="b10c1-200">Azt jelzi, hogy hello virtuális gép elindul.</span><span class="sxs-lookup"><span data-stu-id="b10c1-200">Indicates hello virtual machine is being started.</span></span> |
| <span data-ttu-id="b10c1-201">Fut</span><span class="sxs-lookup"><span data-stu-id="b10c1-201">Running</span></span> | <span data-ttu-id="b10c1-202">Azt jelzi, hogy hello a virtuális gép futása.</span><span class="sxs-lookup"><span data-stu-id="b10c1-202">Indicates that hello virtual machine is running.</span></span> |
| <span data-ttu-id="b10c1-203">Leállítás</span><span class="sxs-lookup"><span data-stu-id="b10c1-203">Stopping</span></span> | <span data-ttu-id="b10c1-204">Azt jelzi, hogy hello a virtuális gép leáll.</span><span class="sxs-lookup"><span data-stu-id="b10c1-204">Indicates that hello virtual machine is being stopped.</span></span> | 
| <span data-ttu-id="b10c1-205">Leállítva</span><span class="sxs-lookup"><span data-stu-id="b10c1-205">Stopped</span></span> | <span data-ttu-id="b10c1-206">Azt jelzi, hogy hello a virtuális gép le van állítva.</span><span class="sxs-lookup"><span data-stu-id="b10c1-206">Indicates that hello virtual machine is stopped.</span></span> <span data-ttu-id="b10c1-207">Hello leállt állapotban lévő virtuális gépek továbbra is számítási díjakat fel Önnek.</span><span class="sxs-lookup"><span data-stu-id="b10c1-207">Virtual machines in hello stopped state still incur compute charges.</span></span>  |
| <span data-ttu-id="b10c1-208">Felszabadítása</span><span class="sxs-lookup"><span data-stu-id="b10c1-208">Deallocating</span></span> | <span data-ttu-id="b10c1-209">Azt jelzi, hogy hello a virtuális gép felszabadítása.</span><span class="sxs-lookup"><span data-stu-id="b10c1-209">Indicates that hello virtual machine is being deallocated.</span></span> |
| <span data-ttu-id="b10c1-210">Felszabadítása</span><span class="sxs-lookup"><span data-stu-id="b10c1-210">Deallocated</span></span> | <span data-ttu-id="b10c1-211">Azt jelzi, hogy hello a virtuális gép hello hipervizor eltávolították, de továbbra is elérhetők a hello vezérlő vezérlősík.</span><span class="sxs-lookup"><span data-stu-id="b10c1-211">Indicates that hello virtual machine is removed from hello hypervisor but still available in hello control plane.</span></span> <span data-ttu-id="b10c1-212">Hello Deallocated állapotban lévő virtuális gépek nem számítunk költségeket.</span><span class="sxs-lookup"><span data-stu-id="b10c1-212">Virtual machines in hello Deallocated state do not incur compute charges.</span></span> |
| - | <span data-ttu-id="b10c1-213">Azt jelzi, hogy hello power hello virtuális gép állapota ismeretlen.</span><span class="sxs-lookup"><span data-stu-id="b10c1-213">Indicates that hello power state of hello virtual machine is unknown.</span></span> |

### <a name="find-power-state"></a><span data-ttu-id="b10c1-214">Energiaállapot keresése</span><span class="sxs-lookup"><span data-stu-id="b10c1-214">Find power state</span></span>

<span data-ttu-id="b10c1-215">egy adott virtuális Gépet, használjon hello tooretrieve hello állapotának [az virtuálisgép-példányokat tartalmazó nézet beolvasása](/cli/azure/vm#get-instance-view) parancsot.</span><span class="sxs-lookup"><span data-stu-id="b10c1-215">tooretrieve hello state of a particular VM, use hello [az vm get instance-view](/cli/azure/vm#get-instance-view) command.</span></span> <span data-ttu-id="b10c1-216">Lehet, hogy toospecify egy érvényes nevet a virtuális gép és erőforráscsoport.</span><span class="sxs-lookup"><span data-stu-id="b10c1-216">Be sure toospecify a valid name for a virtual machine and resource group.</span></span> 

```azurecli-interactive 
az vm get-instance-view \
    --name myVM \
    --resource-group myResourceGroupVM \
    --query instanceView.statuses[1] --output table
```

<span data-ttu-id="b10c1-217">Kimenet:</span><span class="sxs-lookup"><span data-stu-id="b10c1-217">Output:</span></span>

```azurecli-interactive 
ode                DisplayStatus    Level
------------------  ---------------  -------
PowerState/running  VM running       Info
```

## <a name="management-tasks"></a><span data-ttu-id="b10c1-218">Felügyeleti feladatok</span><span class="sxs-lookup"><span data-stu-id="b10c1-218">Management tasks</span></span>

<span data-ttu-id="b10c1-219">Során hello életciklus-egy virtuális gép érdemes lehet toorun felügyeleti feladatokhoz, mint az indítása, leállítása vagy egy virtuális gép törlése.</span><span class="sxs-lookup"><span data-stu-id="b10c1-219">During hello life-cycle of a virtual machine, you may want toorun management tasks such as starting, stopping, or deleting a virtual machine.</span></span> <span data-ttu-id="b10c1-220">Emellett érdemes lehet toocreate parancsfájlok tooautomate ismétlődő vagy összetett feladatokat.</span><span class="sxs-lookup"><span data-stu-id="b10c1-220">Additionally, you may want toocreate scripts tooautomate repetitive or complex tasks.</span></span> <span data-ttu-id="b10c1-221">Hello Azure parancssori felület használatával, számos gyakori felügyeleti feladatokat is futtatható hello parancssorból vagy parancsfájlokban.</span><span class="sxs-lookup"><span data-stu-id="b10c1-221">Using hello Azure CLI, many common management tasks can be run from hello command line or in scripts.</span></span> 

### <a name="get-ip-address"></a><span data-ttu-id="b10c1-222">IP-cím beszerzése</span><span class="sxs-lookup"><span data-stu-id="b10c1-222">Get IP address</span></span>

<span data-ttu-id="b10c1-223">Ez a parancs visszaadja a hello privát és nyilvános IP-címek egy virtuális gép.</span><span class="sxs-lookup"><span data-stu-id="b10c1-223">This command returns hello private and public IP addresses of a virtual machine.</span></span>  

```azurecli-interactive 
az vm list-ip-addresses --resource-group myResourceGroupVM --name myVM --output table
```

### <a name="stop-virtual-machine"></a><span data-ttu-id="b10c1-224">Virtuális gép leállítása</span><span class="sxs-lookup"><span data-stu-id="b10c1-224">Stop virtual machine</span></span>

```azurecli-interactive 
az vm stop --resource-group myResourceGroupVM --name myVM
```

### <a name="start-virtual-machine"></a><span data-ttu-id="b10c1-225">Virtuális gép elindítása</span><span class="sxs-lookup"><span data-stu-id="b10c1-225">Start virtual machine</span></span>

```azurecli-interactive 
az vm start --resource-group myResourceGroupVM --name myVM
```

### <a name="delete-resource-group"></a><span data-ttu-id="b10c1-226">Erőforráscsoport törlése</span><span class="sxs-lookup"><span data-stu-id="b10c1-226">Delete resource group</span></span>

<span data-ttu-id="b10c1-227">Erőforráscsoport törlésekor a belül található összes erőforrást is törlődnek.</span><span class="sxs-lookup"><span data-stu-id="b10c1-227">Deleting a resource group also deletes all resources contained within.</span></span>

```azurecli-interactive 
az group delete --name myResourceGroupVM --no-wait --yes
```

## <a name="next-steps"></a><span data-ttu-id="b10c1-228">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="b10c1-228">Next steps</span></span>

<span data-ttu-id="b10c1-229">Ebben az oktatóprogramban megismerte alapvető virtuális gép létrehozása és kezelése például hogyan:</span><span class="sxs-lookup"><span data-stu-id="b10c1-229">In this tutorial, you learned about basic VM creation and management such as how to:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="b10c1-230">Hozzon létre, és csatlakozzon a virtuális gép tooa</span><span class="sxs-lookup"><span data-stu-id="b10c1-230">Create and connect tooa VM</span></span>
> * <span data-ttu-id="b10c1-231">Válassza ki és használja a Virtuálisgép-rendszerképek</span><span class="sxs-lookup"><span data-stu-id="b10c1-231">Select and use VM images</span></span>
> * <span data-ttu-id="b10c1-232">Megtekintése és használata a megadott Virtuálisgép-méretek</span><span class="sxs-lookup"><span data-stu-id="b10c1-232">View and use specific VM sizes</span></span>
> * <span data-ttu-id="b10c1-233">Virtuális gép átméretezése</span><span class="sxs-lookup"><span data-stu-id="b10c1-233">Resize a VM</span></span>
> * <span data-ttu-id="b10c1-234">Megtekinteni és megérteni a virtuális gép állapota</span><span class="sxs-lookup"><span data-stu-id="b10c1-234">View and understand VM state</span></span>

<span data-ttu-id="b10c1-235">Virtuális gép lemezeivel kapcsolatos útmutató toolearn következő toohello előzetes.</span><span class="sxs-lookup"><span data-stu-id="b10c1-235">Advance toohello next tutorial toolearn about VM disks.</span></span>  

> [!div class="nextstepaction"]
> [<span data-ttu-id="b10c1-236">Hozzon létre és kezelheti a virtuális gépek lemezei</span><span class="sxs-lookup"><span data-stu-id="b10c1-236">Create and Manage VM disks</span></span>](./tutorial-manage-disks.md)
