---
title: "Az Azure CLI 1.0 Linux virtuális gép létrehozásának különböző módszerei |} Microsoft Docs"
description: "Ez a cikk a Linux virtuális gépek Azure-ban való létrehozásának különböző módszereit ismerteti, és minden módszer esetén további eszközökre és oktatóanyagokra mutató hivatkozásokat tartalmaz."
services: virtual-machines-linux
documentationcenter: 
author: iainfoulds
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: f38f8a44-6c88-4490-a84a-46388212d24c
ms.service: virtual-machines-linux
ms.devlang: na
ms.topic: 
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure-services
ms.date: 05/11/2017
ms.author: iainfou
ms.openlocfilehash: 1eb90d44797d66f3e09811918ce5a7f4ad4287c6
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/03/2017
---
# <a name="different-ways-to-create-a-linux-virtual-machine-in-azure"></a><span data-ttu-id="78881-103">Linux virtuális gépek létrehozásának különböző módszerei az Azure-ban</span><span class="sxs-lookup"><span data-stu-id="78881-103">Different ways to create a Linux virtual machine in Azure</span></span>
<span data-ttu-id="78881-104">Az Azure-ban rugalmasan hozhat létre Linux virtuális gépeket olyan eszközökkel és munkafolyamatokkal, amelyeket szívesen használ.</span><span class="sxs-lookup"><span data-stu-id="78881-104">You have the flexibility in Azure to create a Linux virtual machine (VM) using tools and workflows comfortable to you.</span></span> <span data-ttu-id="78881-105">Ez a cikk a Linux virtuális gépek létrehozásának ezen különböző módszereit és példáit foglalja össze.</span><span class="sxs-lookup"><span data-stu-id="78881-105">This article summarizes these differences and examples for creating your Linux VMs.</span></span>

## <a name="azure-cli"></a><span data-ttu-id="78881-106">Azure CLI</span><span class="sxs-lookup"><span data-stu-id="78881-106">Azure CLI</span></span>
<span data-ttu-id="78881-107">A következő CLI-verziók egyikével hozhat létre virtuális gépeket az Azure-ban:</span><span class="sxs-lookup"><span data-stu-id="78881-107">You can create VMs in Azure using one of the following CLI versions:</span></span>

- <span data-ttu-id="78881-108">Azure CLI 1.0 – parancssori felületünk a klasszikus és a Resource Management üzemi modellekhez (a jelen cikkben)</span><span class="sxs-lookup"><span data-stu-id="78881-108">Azure CLI 1.0 – our CLI for the classic and resource management deployment models (this article)</span></span>
- <span data-ttu-id="78881-109">[Azure CLI 2.0](../windows/creation-choices.md) – a Resource Management üzemi modellhez tartozó parancssori felületek következő generációját képviseli.</span><span class="sxs-lookup"><span data-stu-id="78881-109">[Azure CLI 2.0](../windows/creation-choices.md) - our next generation CLI for the resource management deployment model</span></span>

<span data-ttu-id="78881-110">Az Azure CLI 1.0 több platformon elérhető egy npm-csomagon, disztribúció által biztosított csomagokon vagy a Docker-tárolón keresztül.</span><span class="sxs-lookup"><span data-stu-id="78881-110">The Azure CLI 1.0 is available across platforms via an npm package, distro-provided packages, or Docker container.</span></span> <span data-ttu-id="78881-111">Az [Azure CLI telepítésével és konfigurálásával kapcsolatban itt](../../cli-install-nodejs.md) olvashat további információt.</span><span class="sxs-lookup"><span data-stu-id="78881-111">You can read more about [how to install and configure the Azure CLI](../../cli-install-nodejs.md).</span></span> <span data-ttu-id="78881-112">Az alábbi oktatóanyagok az Azure CLI 1.0 használatával kapcsolatos példákat biztosítanak.</span><span class="sxs-lookup"><span data-stu-id="78881-112">The following tutorials provide examples on using the Azure CLI 1.0.</span></span> <span data-ttu-id="78881-113">Olvassa el az alábbi cikkeket a bemutatott CLI gyors üzembe helyezési parancsok további részleteivel kapcsolatban:</span><span class="sxs-lookup"><span data-stu-id="78881-113">Read each article for more details on the CLI quick-start commands shown:</span></span>

* [<span data-ttu-id="78881-114">Linux virtuális gép létrehozása az Azure parancssori felületen fejlesztéshez és teszteléshez</span><span class="sxs-lookup"><span data-stu-id="78881-114">Create a Linux VM from the Azure CLI for dev and test</span></span>](quick-create-cli-nodejs.md)
  
  * <span data-ttu-id="78881-115">Az alábbi példakód létrehozza a CoreOS virtuális gépek nevű nyilvános kulcs *azure_id_rsa.pub*:</span><span class="sxs-lookup"><span data-stu-id="78881-115">The following example creates a CoreOS VM using a public key named *azure_id_rsa.pub*:</span></span>
    
    ```azurecli
    azure vm quick-create -ssh-publickey-file ~/.ssh/azure_id_rsa.pub \
      --image-urn CoreOS
    ```
* [<span data-ttu-id="78881-116">Biztonságos Linux virtuális gép létrehozása Azure-sablon alapján</span><span class="sxs-lookup"><span data-stu-id="78881-116">Create a secured Linux VM using an Azure template</span></span>](create-ssh-secured-vm-from-template.md)
  
  * <span data-ttu-id="78881-117">Az alábbi példa egy virtuális gépet hoz létre a GitHubon tárolt sablonnal:</span><span class="sxs-lookup"><span data-stu-id="78881-117">The following example creates a VM using a template stored on GitHub:</span></span>
    
    ```azurecli
    azure group create --name myResourceGroup --location eastus 
      --template-uri https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/101-vm-sshkey/azuredeploy.json
    ```
* [<span data-ttu-id="78881-118">Teljes Linux-környezet létrehozása az Azure parancssori felülettel</span><span class="sxs-lookup"><span data-stu-id="78881-118">Create a complete Linux environment using the Azure CLI</span></span>](create-cli-complete-nodejs.md)
  
  * <span data-ttu-id="78881-119">Magában foglalja egy terheléselosztó és több virtuális gép létrehozását egy rendelkezésre állási csoportban.</span><span class="sxs-lookup"><span data-stu-id="78881-119">Includes creating a load balancer and multiple VMs in an availability set.</span></span>
* [<span data-ttu-id="78881-120">Add a disk to a Linux VM (Lemez hozzáadása Linux rendszerű virtuális géphez)</span><span class="sxs-lookup"><span data-stu-id="78881-120">Add a disk to a Linux VM</span></span>](add-disk.md)
  
  * <span data-ttu-id="78881-121">A következő példa egy *5* Gb lemezterület nevű egy meglévő virtuális gép *myVM*:</span><span class="sxs-lookup"><span data-stu-id="78881-121">The following example adds a *5* Gb disk to an existing VM named *myVM*:</span></span>
    
    ```azurecli
    azure vm disk attach-new \
        --resource-group myResourceGroup \
        --vm-name myVM \
        --size-in-GB 5
    ```

## <a name="azure-portal"></a><span data-ttu-id="78881-122">Azure Portal</span><span class="sxs-lookup"><span data-stu-id="78881-122">Azure portal</span></span>
<span data-ttu-id="78881-123">Az [Azure portálon](https://portal.azure.com) gyorsan létrehozhat egy virtuális gépet, mivel semmit nem kell telepítenie a rendszerre.</span><span class="sxs-lookup"><span data-stu-id="78881-123">The [Azure portal](https://portal.azure.com) allows you to quickly create a VM since there is nothing to install on your system.</span></span> <span data-ttu-id="78881-124">A virtuális gép létrehozása az Azure Portallal:</span><span class="sxs-lookup"><span data-stu-id="78881-124">Use the Azure portal to create the VM:</span></span>

* [<span data-ttu-id="78881-125">Linux virtuális gép létrehozása az Azure Portal használatával</span><span class="sxs-lookup"><span data-stu-id="78881-125">Create a Linux VM using the Azure portal</span></span>](quick-create-portal.md) 

## <a name="operating-system-and-image-choices"></a><span data-ttu-id="78881-126">Választható operációs rendszerek és rendszerképek</span><span class="sxs-lookup"><span data-stu-id="78881-126">Operating system and image choices</span></span>
<span data-ttu-id="78881-127">A virtuális gépek létrehozásakor egy rendszerképet választ ki a futtatni kívánt operációs rendszer alapján.</span><span class="sxs-lookup"><span data-stu-id="78881-127">When creating a VM, you choose an image based on the operating system you want to run.</span></span> <span data-ttu-id="78881-128">Az Azure és a partnerei számos rendszerképet kínálnak, amelyek némelyike előre telepített alkalmazásokat és eszközöket tartalmaz.</span><span class="sxs-lookup"><span data-stu-id="78881-128">Azure and its partners offer many images, some of which include applications and tools pre-installed.</span></span> <span data-ttu-id="78881-129">Feltöltheti az egyik saját rendszerképét is (lásd [a következő szakaszt](#use-your-own-image)).</span><span class="sxs-lookup"><span data-stu-id="78881-129">Or, upload one of your own images (see [the following section](#use-your-own-image)).</span></span>

### <a name="azure-images"></a><span data-ttu-id="78881-130">Azure-rendszerképek</span><span class="sxs-lookup"><span data-stu-id="78881-130">Azure images</span></span>
<span data-ttu-id="78881-131">Az `azure vm image` CLI-parancsok használatával megtekintheti az elérhető elemeket közzétevő, disztribúciós kiadás, illetve build szerint.</span><span class="sxs-lookup"><span data-stu-id="78881-131">Use the `azure vm image` CLI commands to see what's available by publisher, distro release, and builds.</span></span>

<span data-ttu-id="78881-132">A következőképpen listázhatja az elérhető közzétevőket:</span><span class="sxs-lookup"><span data-stu-id="78881-132">List available publishers as follows:</span></span>

```azurecli
azure vm image list-publishers --location eastus
```

<span data-ttu-id="78881-133">A következőképpen listázhatja egy adott közzétevő elérhető termékeit (ajánlatait):</span><span class="sxs-lookup"><span data-stu-id="78881-133">List available products (offers) for a given publisher as follows:</span></span>

```azurecli
azure vm image list-offers --location eastus --publisher Canonical
```

<span data-ttu-id="78881-134">A következőképpen listázhatja egy adott ajánlat elérhető termékváltozatait (disztribúciós kiadásait):</span><span class="sxs-lookup"><span data-stu-id="78881-134">List available SKUs (distro releases) of a given offer as follows:</span></span>

```azurecli
azure vm image list-skus --location eastus --publisher Canonical --offer UbuntuServer
```

<span data-ttu-id="78881-135">A következőképpen listázhatja egy adott kiadás összes elérhető rendszerképét:</span><span class="sxs-lookup"><span data-stu-id="78881-135">List all available images for a given release follows:</span></span>

```azurecli
azure vm image list --location eastus --publisher Canonical --offer UbuntuServer --sku 16.04.0-LTS
```

<span data-ttu-id="78881-136">Az `azure vm quick-create` és az `azure vm create` parancs is rendelkezik aliasokkal, amelyek segítségével gyorsan hozzáférhet a leggyakoribb disztribúciókhoz és azok legújabb kiadásaihoz.</span><span class="sxs-lookup"><span data-stu-id="78881-136">The `azure vm quick-create` and `azure vm create` commands have aliases you can use to quickly access the more common distros and their latest releases.</span></span> <span data-ttu-id="78881-137">Az aliasok használata gyakran gyorsabb, mintha meg kellene adnia a közzétevőt, ajánlatot, termékváltozatot és verziót, valahányszor létrehoz egy virtuális gépet:</span><span class="sxs-lookup"><span data-stu-id="78881-137">Using aliases is often quicker than specifying the publisher, offer, SKU, and version each time you create a VM:</span></span>

| <span data-ttu-id="78881-138">Alias</span><span class="sxs-lookup"><span data-stu-id="78881-138">Alias</span></span> | <span data-ttu-id="78881-139">Közzétevő</span><span class="sxs-lookup"><span data-stu-id="78881-139">Publisher</span></span> | <span data-ttu-id="78881-140">Ajánlat</span><span class="sxs-lookup"><span data-stu-id="78881-140">Offer</span></span> | <span data-ttu-id="78881-141">SKU</span><span class="sxs-lookup"><span data-stu-id="78881-141">SKU</span></span> | <span data-ttu-id="78881-142">Verzió</span><span class="sxs-lookup"><span data-stu-id="78881-142">Version</span></span> |
|:--- |:--- |:--- |:--- |:--- |
| <span data-ttu-id="78881-143">CentOS</span><span class="sxs-lookup"><span data-stu-id="78881-143">CentOS</span></span> |<span data-ttu-id="78881-144">OpenLogic</span><span class="sxs-lookup"><span data-stu-id="78881-144">OpenLogic</span></span> |<span data-ttu-id="78881-145">Centos</span><span class="sxs-lookup"><span data-stu-id="78881-145">Centos</span></span> |<span data-ttu-id="78881-146">7.2</span><span class="sxs-lookup"><span data-stu-id="78881-146">7.2</span></span> |<span data-ttu-id="78881-147">legújabb</span><span class="sxs-lookup"><span data-stu-id="78881-147">latest</span></span> |
| <span data-ttu-id="78881-148">CoreOS</span><span class="sxs-lookup"><span data-stu-id="78881-148">CoreOS</span></span> |<span data-ttu-id="78881-149">CoreOS</span><span class="sxs-lookup"><span data-stu-id="78881-149">CoreOS</span></span> |<span data-ttu-id="78881-150">CoreOS</span><span class="sxs-lookup"><span data-stu-id="78881-150">CoreOS</span></span> |<span data-ttu-id="78881-151">Stable</span><span class="sxs-lookup"><span data-stu-id="78881-151">Stable</span></span> |<span data-ttu-id="78881-152">legújabb</span><span class="sxs-lookup"><span data-stu-id="78881-152">latest</span></span> |
| <span data-ttu-id="78881-153">Debian</span><span class="sxs-lookup"><span data-stu-id="78881-153">Debian</span></span> |<span data-ttu-id="78881-154">credativ</span><span class="sxs-lookup"><span data-stu-id="78881-154">credativ</span></span> |<span data-ttu-id="78881-155">Debian</span><span class="sxs-lookup"><span data-stu-id="78881-155">Debian</span></span> |<span data-ttu-id="78881-156">8</span><span class="sxs-lookup"><span data-stu-id="78881-156">8</span></span> |<span data-ttu-id="78881-157">legújabb</span><span class="sxs-lookup"><span data-stu-id="78881-157">latest</span></span> |
| <span data-ttu-id="78881-158">openSUSE</span><span class="sxs-lookup"><span data-stu-id="78881-158">openSUSE</span></span> |<span data-ttu-id="78881-159">SUSE</span><span class="sxs-lookup"><span data-stu-id="78881-159">SUSE</span></span> |<span data-ttu-id="78881-160">openSUSE</span><span class="sxs-lookup"><span data-stu-id="78881-160">openSUSE</span></span> |<span data-ttu-id="78881-161">13.2</span><span class="sxs-lookup"><span data-stu-id="78881-161">13.2</span></span> |<span data-ttu-id="78881-162">legújabb</span><span class="sxs-lookup"><span data-stu-id="78881-162">latest</span></span> |
| <span data-ttu-id="78881-163">RHEL</span><span class="sxs-lookup"><span data-stu-id="78881-163">RHEL</span></span> |<span data-ttu-id="78881-164">Redhat</span><span class="sxs-lookup"><span data-stu-id="78881-164">Redhat</span></span> |<span data-ttu-id="78881-165">RHEL</span><span class="sxs-lookup"><span data-stu-id="78881-165">RHEL</span></span> |<span data-ttu-id="78881-166">7.2</span><span class="sxs-lookup"><span data-stu-id="78881-166">7.2</span></span> |<span data-ttu-id="78881-167">legújabb</span><span class="sxs-lookup"><span data-stu-id="78881-167">latest</span></span> |
| <span data-ttu-id="78881-168">SLES</span><span class="sxs-lookup"><span data-stu-id="78881-168">SLES</span></span> |<span data-ttu-id="78881-169">SLES</span><span class="sxs-lookup"><span data-stu-id="78881-169">SLES</span></span> |<span data-ttu-id="78881-170">SLES</span><span class="sxs-lookup"><span data-stu-id="78881-170">SLES</span></span> |<span data-ttu-id="78881-171">12-SP1</span><span class="sxs-lookup"><span data-stu-id="78881-171">12-SP1</span></span> |<span data-ttu-id="78881-172">legújabb</span><span class="sxs-lookup"><span data-stu-id="78881-172">latest</span></span> |
| <span data-ttu-id="78881-173">UbuntuLTS</span><span class="sxs-lookup"><span data-stu-id="78881-173">UbuntuLTS</span></span> |<span data-ttu-id="78881-174">Canonical</span><span class="sxs-lookup"><span data-stu-id="78881-174">Canonical</span></span> |<span data-ttu-id="78881-175">UbuntuServer</span><span class="sxs-lookup"><span data-stu-id="78881-175">UbuntuServer</span></span> |<span data-ttu-id="78881-176">14.04.4-LTS</span><span class="sxs-lookup"><span data-stu-id="78881-176">14.04.4-LTS</span></span> |<span data-ttu-id="78881-177">legújabb</span><span class="sxs-lookup"><span data-stu-id="78881-177">latest</span></span> |

### <a name="use-your-own-image"></a><span data-ttu-id="78881-178">Saját rendszerkép használata</span><span class="sxs-lookup"><span data-stu-id="78881-178">Use your own image</span></span>
<span data-ttu-id="78881-179">Ha speciális egyéni beállításokra van szüksége, használhat egy meglévő Azure virtuális gépen alapuló rendszerképet a virtuális gép *rögzítésével*.</span><span class="sxs-lookup"><span data-stu-id="78881-179">If you require specific customizations, you can use an image based on an existing Azure VM by *capturing* that VM.</span></span> <span data-ttu-id="78881-180">Emellett feltölthet egy helyszínen létrehozott rendszerképet is.</span><span class="sxs-lookup"><span data-stu-id="78881-180">You can also upload an image created on-premises.</span></span> <span data-ttu-id="78881-181">A támogatott disztribúciókkal és a saját rendszerképek használatával kapcsolatban az alábbi cikkekben tekinthet meg további információt:</span><span class="sxs-lookup"><span data-stu-id="78881-181">For more information on supported distros and how to use your own images, see the following articles:</span></span>

* [<span data-ttu-id="78881-182">Azure által támogatott disztribúciók</span><span class="sxs-lookup"><span data-stu-id="78881-182">Azure endorsed distributions</span></span>](endorsed-distros.md)
* [<span data-ttu-id="78881-183">Nem támogatott disztribúciókkal kapcsolatos tudnivalók</span><span class="sxs-lookup"><span data-stu-id="78881-183">Information for non-endorsed distributions</span></span>](create-upload-generic.md)
* [<span data-ttu-id="78881-184">Linuxos virtuális gép feltöltése és létrehozása egyéni rendszerképből</span><span class="sxs-lookup"><span data-stu-id="78881-184">Upload and create a Linux VM from custom disk image</span></span>](upload-vhd.md)
* <span data-ttu-id="78881-185">[How to capture a Linux virtual machine as a Resource Manager template](capture-image.md) (Linux virtuális gép rögzítése Resource Manager-sablonként).</span><span class="sxs-lookup"><span data-stu-id="78881-185">[How to capture a Linux virtual machine as a Resource Manager template](capture-image.md).</span></span>
  
  * <span data-ttu-id="78881-186">Gyors üzembe helyezési példaparancsok egy meglévő virtuális gép rögzítésére:</span><span class="sxs-lookup"><span data-stu-id="78881-186">Quick-start example commands to capture an existing VM:</span></span>
    
    ```azurecli
    azure vm deallocate --resource-group myResourceGroup --vm-name myVM
    azure vm generalize --resource-group myResourceGroup --vm-name myVM
    azure vm capture --resource-group myResourceGroup --vm-name myVM --vhd-name-prefix myCapturedVM
    ```

## <a name="next-steps"></a><span data-ttu-id="78881-187">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="78881-187">Next steps</span></span>
* <span data-ttu-id="78881-188">Hozzon létre egy Linux virtuális gépet a [portálon](quick-create-portal.md) a [parancssori felülettel](quick-create-cli.md) vagy [Azure Resource Manager-sablonnal](../windows/cli-deploy-templates.md).</span><span class="sxs-lookup"><span data-stu-id="78881-188">Create a Linux VM from the [portal](quick-create-portal.md), with the [CLI](quick-create-cli.md), or using an [Azure Resource Manager template](../windows/cli-deploy-templates.md).</span></span>
* <span data-ttu-id="78881-189">A Linux virtuális gép létrehozása után [adjon hozzá egy adatlemezt](add-disk.md).</span><span class="sxs-lookup"><span data-stu-id="78881-189">After creating a Linux VM, [add a data disk](add-disk.md).</span></span>
* <span data-ttu-id="78881-190">Gyors lépések [jelszó vagy SSH-kulcsok alaphelyzetbe állításához és felhasználók kezeléséhez](using-vmaccess-extension.md)</span><span class="sxs-lookup"><span data-stu-id="78881-190">Quick steps to [reset a password or SSH keys and manage users](using-vmaccess-extension.md)</span></span>

