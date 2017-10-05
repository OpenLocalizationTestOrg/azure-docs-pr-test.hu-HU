---
title: "Linux virtuális gépek Azure-ban való létrehozásának módszerei | Microsoft Azure"
description: "Ez a cikk a Linux virtuális gépek Azure-ban való létrehozásának különböző módszereit ismerteti, és minden módszer esetén további eszközökre és oktatóanyagokra mutató hivatkozásokat tartalmaz."
services: virtual-machines-linux
documentationcenter: 
author: iainfoulds
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: f38f8a44-6c88-4490-a84a-46388212d24c
ms.service: virtual-machines-linux
ms.devlang: azurecli
ms.topic: get-started-article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure-services
ms.date: 05/11/2017
ms.author: iainfou
ms.openlocfilehash: b2f93579eb1c7a69d0dbd1b0ef112aed9b2168c3
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="different-ways-to-create-a-linux-vm"></a><span data-ttu-id="bd7f3-103">Különböző módszerek Linux rendszerű virtuális gépek létrehozásához</span><span class="sxs-lookup"><span data-stu-id="bd7f3-103">Different ways to create a Linux VM</span></span>
<span data-ttu-id="bd7f3-104">Az Azure-ban rugalmasan hozhat létre Linux virtuális gépeket olyan eszközökkel és munkafolyamatokkal, amelyeket szívesen használ.</span><span class="sxs-lookup"><span data-stu-id="bd7f3-104">You have the flexibility in Azure to create a Linux virtual machine (VM) using tools and workflows comfortable to you.</span></span> <span data-ttu-id="bd7f3-105">Ez a cikk a Linux-alapú virtuális gépek létrehozásának ezen különböző módszereit és példáit foglalja össze, az Azure CLI 2.0-s verzióját is beleértve.</span><span class="sxs-lookup"><span data-stu-id="bd7f3-105">This article summarizes these differences and examples for creating your Linux VMs, including the Azure CLI 2.0.</span></span> <span data-ttu-id="bd7f3-106">Emellett megtekintheti a létrehozási lehetőségeket, beleértve az [Azure CLI 1.0](creation-choices-nodejs.md) használatát.</span><span class="sxs-lookup"><span data-stu-id="bd7f3-106">You can also view creation choices including the [Azure CLI 1.0](creation-choices-nodejs.md).</span></span>

<span data-ttu-id="bd7f3-107">Az [Azure CLI 2.0](/cli/azure/install-az-cli2) több platformon elérhető egy npm-csomagon, disztribúció által biztosított csomagokon vagy a Docker-tárolón keresztül.</span><span class="sxs-lookup"><span data-stu-id="bd7f3-107">The [Azure CLI 2.0](/cli/azure/install-az-cli2) is available across platforms via an npm package, distro-provided packages, or Docker container.</span></span> <span data-ttu-id="bd7f3-108">Telepítheti a környezet számára legmegfelelőbb buildet, és bejelentkezhet egy Azure-fiókba az [az login](/cli/azure/#login) paranccsal</span><span class="sxs-lookup"><span data-stu-id="bd7f3-108">Install the most appropriate build for your environment and log in to an Azure account using [az login](/cli/azure/#login)</span></span>

* [<span data-ttu-id="bd7f3-109">Linux-alapú virtuális gép létrehozása az Azure CLI 2.0-s verziójával</span><span class="sxs-lookup"><span data-stu-id="bd7f3-109">Create a Linux VM with the Azure CLI 2.0</span></span>](quick-create-cli.md)
  
  * <span data-ttu-id="bd7f3-110">Hozzon létre egy *myResourceGroup* nevű erőforráscsoportot az [az group create](/cli/azure/group#create) paranccsal:</span><span class="sxs-lookup"><span data-stu-id="bd7f3-110">Create a resource group with [az group create](/cli/azure/group#create) named *myResourceGroup*:</span></span> 
   
    ```azurecli
    az group create --name myResourceGroup --location eastus
    ```
    
  * <span data-ttu-id="bd7f3-111">Hozzon létre egy *myVM* nevű virtuális gépet az [az vm create](/cli/azure/vm#create) paranccsal a legújabb *UbuntuLTS* rendszerkép használatával, majd hozzon létre SSH-kulcsokat, ha azok még nem léteznek az *~/.ssh* mappában:</span><span class="sxs-lookup"><span data-stu-id="bd7f3-111">Create a VM with [az vm create](/cli/azure/vm#create) named *myVM* using the latest *UbuntuLTS* image and generate SSH keys if they do not already exist in *~/.ssh*:</span></span>

    ```azurecli
    az vm create \
        --resource-group myResourceGroup \
        --name myVM \
        --image UbuntuLTS \
        --generate-ssh-keys
    ```

* [<span data-ttu-id="bd7f3-112">Linux virtuális gép létrehozása Azure-sablon alapján</span><span class="sxs-lookup"><span data-stu-id="bd7f3-112">Create a Linux VM with an Azure template</span></span>](create-ssh-secured-vm-from-template.md)
  
  * <span data-ttu-id="bd7f3-113">Az alábbi példa egy virtuális gépet hoz létre a GitHubon tárolt sablonból az [az group deployment create](/cli/azure/group/deployment#create) paranccsal:</span><span class="sxs-lookup"><span data-stu-id="bd7f3-113">The following example uses [az group deployment create](/cli/azure/group/deployment#create) to create a VM from a template stored on GitHub:</span></span>
    
    ```azurecli
    az group deployment create --resource-group myResourceGroup \ 
      --template-uri https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/101-vm-sshkey/azuredeploy.json \
      --parameters @myparameters.json
    ```
* [<span data-ttu-id="bd7f3-114">Linux virtuális gép létrehozása és testreszabása a cloud-init paranccsal</span><span class="sxs-lookup"><span data-stu-id="bd7f3-114">Create a Linux VM and customize with cloud-init</span></span>](tutorial-automate-vm-deployment.md)

* [<span data-ttu-id="bd7f3-115">Elosztott terhelésű és magas rendelkezésre állású alkalmazás létrehozása több Linux virtuális gépen</span><span class="sxs-lookup"><span data-stu-id="bd7f3-115">Create a load balanced and highly available application on multiple Linux VMs</span></span>](tutorial-load-balancer.md)


## <a name="azure-portal"></a><span data-ttu-id="bd7f3-116">Azure Portal</span><span class="sxs-lookup"><span data-stu-id="bd7f3-116">Azure portal</span></span>
<span data-ttu-id="bd7f3-117">Az [Azure portálon](https://portal.azure.com) gyorsan létrehozhat egy virtuális gépet, mivel semmit nem kell telepítenie a rendszerre.</span><span class="sxs-lookup"><span data-stu-id="bd7f3-117">The [Azure portal](https://portal.azure.com) allows you to quickly create a VM since there is nothing to install on your system.</span></span> <span data-ttu-id="bd7f3-118">A virtuális gép létrehozása az Azure Portallal:</span><span class="sxs-lookup"><span data-stu-id="bd7f3-118">Use the Azure portal to create the VM:</span></span>

* [<span data-ttu-id="bd7f3-119">Linux virtuális gép létrehozása az Azure Portal használatával</span><span class="sxs-lookup"><span data-stu-id="bd7f3-119">Create a Linux VM using the Azure portal</span></span>](quick-create-portal.md) 


## <a name="operating-system-and-image-choices"></a><span data-ttu-id="bd7f3-120">Választható operációs rendszerek és rendszerképek</span><span class="sxs-lookup"><span data-stu-id="bd7f3-120">Operating system and image choices</span></span>
<span data-ttu-id="bd7f3-121">A virtuális gépek létrehozásakor egy rendszerképet választ ki a futtatni kívánt operációs rendszer alapján.</span><span class="sxs-lookup"><span data-stu-id="bd7f3-121">When creating a VM, you choose an image based on the operating system you want to run.</span></span> <span data-ttu-id="bd7f3-122">Az Azure és a partnerei számos rendszerképet kínálnak, amelyek némelyike előre telepített alkalmazásokat és eszközöket tartalmaz.</span><span class="sxs-lookup"><span data-stu-id="bd7f3-122">Azure and its partners offer many images, some of which include applications and tools pre-installed.</span></span> <span data-ttu-id="bd7f3-123">Feltöltheti az egyik saját rendszerképét is (lásd [a következő szakaszt](#use-your-own-image)).</span><span class="sxs-lookup"><span data-stu-id="bd7f3-123">Or, upload one of your own images (see [the following section](#use-your-own-image)).</span></span>

### <a name="azure-images"></a><span data-ttu-id="bd7f3-124">Azure-rendszerképek</span><span class="sxs-lookup"><span data-stu-id="bd7f3-124">Azure images</span></span>
<span data-ttu-id="bd7f3-125">Az [az vm image](/cli/azure/vm/image) paranccsal megtekintheti az elérhető elemeket közzétevő, disztribúciós kiadás, illetve build szerint.</span><span class="sxs-lookup"><span data-stu-id="bd7f3-125">Use the [az vm image](/cli/azure/vm/image) commands to see what's available by publisher, distro release, and builds.</span></span>

<span data-ttu-id="bd7f3-126">Az elérhető közzétevők listázása:</span><span class="sxs-lookup"><span data-stu-id="bd7f3-126">List available publishers:</span></span>

```azurecli
az vm image list-publishers --location eastus
```

<span data-ttu-id="bd7f3-127">Egy adott közzétevő elérhető termékeinek (ajánlatainak) listázása:</span><span class="sxs-lookup"><span data-stu-id="bd7f3-127">List available products (offers) for a given publisher:</span></span>

```azurecli
az vm image list-offers --publisher Canonical --location eastus
```

<span data-ttu-id="bd7f3-128">Egy adott ajánlat elérhető termékváltozatainak (disztribúciós kiadásainak) listázása:</span><span class="sxs-lookup"><span data-stu-id="bd7f3-128">List available SKUs (distro releases) of a given offer:</span></span>

```azurecli
az vm image list-skus --publisher Canonical --offer UbuntuServer --location eastus
```

<span data-ttu-id="bd7f3-129">Egy adott kiadás összes elérhető rendszerképének listázása:</span><span class="sxs-lookup"><span data-stu-id="bd7f3-129">List all available images for a given release:</span></span>

```azurecli
az vm image list --publisher Canonical --offer UbuntuServer --sku 16.04.0-LTS --location eastus
```

<span data-ttu-id="bd7f3-130">Az elérhető rendszerképek tallózásával és használatával kapcsolatos további példák: [Navigate and select Azure virtual machine images with the Azure CLI](cli-ps-findimage.md) (Azure virtuális gépek rendszerképének keresése és kiválasztása az Azure parancssori felülettel).</span><span class="sxs-lookup"><span data-stu-id="bd7f3-130">For more examples on browsing and using available images, see [Navigate and select Azure virtual machine images with the Azure CLI](cli-ps-findimage.md).</span></span>

<span data-ttu-id="bd7f3-131">Az [az vm create](/cli/azure/vm#create) parancs rendelkezik aliasokkal, amelyek segítségével gyorsan hozzáférhet a leggyakoribb disztribúciókhoz és azok legújabb kiadásaihoz.</span><span class="sxs-lookup"><span data-stu-id="bd7f3-131">The [az vm create](/cli/azure/vm#create) command has aliases you can use to quickly access the more common distros and their latest releases.</span></span> <span data-ttu-id="bd7f3-132">Az aliasok használata gyakran gyorsabb, mintha meg kellene adnia a közzétevőt, ajánlatot, termékváltozatot és verziót, valahányszor létrehoz egy virtuális gépet:</span><span class="sxs-lookup"><span data-stu-id="bd7f3-132">Using aliases is often quicker than specifying the publisher, offer, SKU, and version each time you create a VM:</span></span>

| <span data-ttu-id="bd7f3-133">Alias</span><span class="sxs-lookup"><span data-stu-id="bd7f3-133">Alias</span></span> | <span data-ttu-id="bd7f3-134">Közzétevő</span><span class="sxs-lookup"><span data-stu-id="bd7f3-134">Publisher</span></span> | <span data-ttu-id="bd7f3-135">Ajánlat</span><span class="sxs-lookup"><span data-stu-id="bd7f3-135">Offer</span></span> | <span data-ttu-id="bd7f3-136">SKU</span><span class="sxs-lookup"><span data-stu-id="bd7f3-136">SKU</span></span> | <span data-ttu-id="bd7f3-137">Verzió</span><span class="sxs-lookup"><span data-stu-id="bd7f3-137">Version</span></span> |
|:--- |:--- |:--- |:--- |:--- |
| <span data-ttu-id="bd7f3-138">CentOS</span><span class="sxs-lookup"><span data-stu-id="bd7f3-138">CentOS</span></span> |<span data-ttu-id="bd7f3-139">OpenLogic</span><span class="sxs-lookup"><span data-stu-id="bd7f3-139">OpenLogic</span></span> |<span data-ttu-id="bd7f3-140">Centos</span><span class="sxs-lookup"><span data-stu-id="bd7f3-140">Centos</span></span> |<span data-ttu-id="bd7f3-141">7.2</span><span class="sxs-lookup"><span data-stu-id="bd7f3-141">7.2</span></span> |<span data-ttu-id="bd7f3-142">legújabb</span><span class="sxs-lookup"><span data-stu-id="bd7f3-142">latest</span></span> |
| <span data-ttu-id="bd7f3-143">CoreOS</span><span class="sxs-lookup"><span data-stu-id="bd7f3-143">CoreOS</span></span> |<span data-ttu-id="bd7f3-144">CoreOS</span><span class="sxs-lookup"><span data-stu-id="bd7f3-144">CoreOS</span></span> |<span data-ttu-id="bd7f3-145">CoreOS</span><span class="sxs-lookup"><span data-stu-id="bd7f3-145">CoreOS</span></span> |<span data-ttu-id="bd7f3-146">Stable</span><span class="sxs-lookup"><span data-stu-id="bd7f3-146">Stable</span></span> |<span data-ttu-id="bd7f3-147">legújabb</span><span class="sxs-lookup"><span data-stu-id="bd7f3-147">latest</span></span> |
| <span data-ttu-id="bd7f3-148">Debian</span><span class="sxs-lookup"><span data-stu-id="bd7f3-148">Debian</span></span> |<span data-ttu-id="bd7f3-149">credativ</span><span class="sxs-lookup"><span data-stu-id="bd7f3-149">credativ</span></span> |<span data-ttu-id="bd7f3-150">Debian</span><span class="sxs-lookup"><span data-stu-id="bd7f3-150">Debian</span></span> |<span data-ttu-id="bd7f3-151">8</span><span class="sxs-lookup"><span data-stu-id="bd7f3-151">8</span></span> |<span data-ttu-id="bd7f3-152">legújabb</span><span class="sxs-lookup"><span data-stu-id="bd7f3-152">latest</span></span> |
| <span data-ttu-id="bd7f3-153">openSUSE</span><span class="sxs-lookup"><span data-stu-id="bd7f3-153">openSUSE</span></span> |<span data-ttu-id="bd7f3-154">SUSE</span><span class="sxs-lookup"><span data-stu-id="bd7f3-154">SUSE</span></span> |<span data-ttu-id="bd7f3-155">openSUSE</span><span class="sxs-lookup"><span data-stu-id="bd7f3-155">openSUSE</span></span> |<span data-ttu-id="bd7f3-156">13.2</span><span class="sxs-lookup"><span data-stu-id="bd7f3-156">13.2</span></span> |<span data-ttu-id="bd7f3-157">legújabb</span><span class="sxs-lookup"><span data-stu-id="bd7f3-157">latest</span></span> |
| <span data-ttu-id="bd7f3-158">RHEL</span><span class="sxs-lookup"><span data-stu-id="bd7f3-158">RHEL</span></span> |<span data-ttu-id="bd7f3-159">Redhat</span><span class="sxs-lookup"><span data-stu-id="bd7f3-159">Redhat</span></span> |<span data-ttu-id="bd7f3-160">RHEL</span><span class="sxs-lookup"><span data-stu-id="bd7f3-160">RHEL</span></span> |<span data-ttu-id="bd7f3-161">7.2</span><span class="sxs-lookup"><span data-stu-id="bd7f3-161">7.2</span></span> |<span data-ttu-id="bd7f3-162">legújabb</span><span class="sxs-lookup"><span data-stu-id="bd7f3-162">latest</span></span> |
| <span data-ttu-id="bd7f3-163">SLES</span><span class="sxs-lookup"><span data-stu-id="bd7f3-163">SLES</span></span> |<span data-ttu-id="bd7f3-164">SLES</span><span class="sxs-lookup"><span data-stu-id="bd7f3-164">SLES</span></span> |<span data-ttu-id="bd7f3-165">SLES</span><span class="sxs-lookup"><span data-stu-id="bd7f3-165">SLES</span></span> |<span data-ttu-id="bd7f3-166">12-SP1</span><span class="sxs-lookup"><span data-stu-id="bd7f3-166">12-SP1</span></span> |<span data-ttu-id="bd7f3-167">legújabb</span><span class="sxs-lookup"><span data-stu-id="bd7f3-167">latest</span></span> |
| <span data-ttu-id="bd7f3-168">UbuntuLTS</span><span class="sxs-lookup"><span data-stu-id="bd7f3-168">UbuntuLTS</span></span> |<span data-ttu-id="bd7f3-169">Canonical</span><span class="sxs-lookup"><span data-stu-id="bd7f3-169">Canonical</span></span> |<span data-ttu-id="bd7f3-170">UbuntuServer</span><span class="sxs-lookup"><span data-stu-id="bd7f3-170">UbuntuServer</span></span> |<span data-ttu-id="bd7f3-171">14.04.4-LTS</span><span class="sxs-lookup"><span data-stu-id="bd7f3-171">14.04.4-LTS</span></span> |<span data-ttu-id="bd7f3-172">legújabb</span><span class="sxs-lookup"><span data-stu-id="bd7f3-172">latest</span></span> |

### <a name="use-your-own-image"></a><span data-ttu-id="bd7f3-173">Saját rendszerkép használata</span><span class="sxs-lookup"><span data-stu-id="bd7f3-173">Use your own image</span></span>
<span data-ttu-id="bd7f3-174">Ha speciális egyéni beállításokra van szüksége, használhat egy meglévő Azure virtuális gépen alapuló rendszerképet a virtuális gép rögzítésével.</span><span class="sxs-lookup"><span data-stu-id="bd7f3-174">If you require specific customizations, you can use an image based on an existing Azure VM by capturing that VM.</span></span> <span data-ttu-id="bd7f3-175">Emellett feltölthet egy helyszínen létrehozott rendszerképet is.</span><span class="sxs-lookup"><span data-stu-id="bd7f3-175">You can also upload an image created on-premises.</span></span> <span data-ttu-id="bd7f3-176">A támogatott disztribúciókkal és a saját rendszerképek használatával kapcsolatban az alábbi cikkekben tekinthet meg további információt:</span><span class="sxs-lookup"><span data-stu-id="bd7f3-176">For more information on supported distros and how to use your own images, see the following articles:</span></span>

* [<span data-ttu-id="bd7f3-177">Azure által támogatott disztribúciók</span><span class="sxs-lookup"><span data-stu-id="bd7f3-177">Azure endorsed distributions</span></span>](endorsed-distros.md)
* [<span data-ttu-id="bd7f3-178">Nem támogatott disztribúciókkal kapcsolatos tudnivalók</span><span class="sxs-lookup"><span data-stu-id="bd7f3-178">Information for non-endorsed distributions</span></span>](create-upload-generic.md)
* <span data-ttu-id="bd7f3-179">[Rendszerkép létrehozása meglévő Azure virtuális gépből](tutorial-custom-images.md).</span><span class="sxs-lookup"><span data-stu-id="bd7f3-179">[How to create an image from an existing Azure VM](tutorial-custom-images.md).</span></span>
  
  * <span data-ttu-id="bd7f3-180">Gyors üzembehelyezési példaparancsok rendszerkép meglévő Azure virtuális gépből való létrehozására:</span><span class="sxs-lookup"><span data-stu-id="bd7f3-180">Quick-start example commands to create an image from an existing Azure VM:</span></span>
    
    ```azurecli
    az vm deallocate --resource-group myResourceGroup --name myVM
    az vm generalize --resource-group myResourceGroup --name myVM
    az vm image create --resource-group myResourceGroup --source myVM --name myImage
    ```

## <a name="next-steps"></a><span data-ttu-id="bd7f3-181">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="bd7f3-181">Next steps</span></span>
* <span data-ttu-id="bd7f3-182">Hozzon létre egy Linux virtuális gépet a [parancssori felülettel](quick-create-cli.md), a [portálon](quick-create-portal.md) vagy [Azure Resource Manager-sablonnal](../windows/cli-deploy-templates.md).</span><span class="sxs-lookup"><span data-stu-id="bd7f3-182">Create a Linux VM with the [CLI](quick-create-cli.md), from the [portal](quick-create-portal.md), or using an [Azure Resource Manager template](../windows/cli-deploy-templates.md).</span></span>
* <span data-ttu-id="bd7f3-183">A Linux virtuális gép létrehozása után [ismerje meg az Azure lemezeket és tárolót](tutorial-manage-disks.md).</span><span class="sxs-lookup"><span data-stu-id="bd7f3-183">After creating a Linux VM, [learn about Azure disks and storage](tutorial-manage-disks.md).</span></span>
* <span data-ttu-id="bd7f3-184">Gyors lépések [jelszó vagy SSH-kulcsok alaphelyzetbe állításához és felhasználók kezeléséhez](using-vmaccess-extension.md).</span><span class="sxs-lookup"><span data-stu-id="bd7f3-184">Quick steps to [reset a password or SSH keys and manage users](using-vmaccess-extension.md).</span></span>
