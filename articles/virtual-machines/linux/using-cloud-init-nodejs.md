---
title: "Felhő inicializálás segítségével testre szabhatja a Linux virtuális gép létrehozása az Azure-ban |} Microsoft Docs"
description: "Felhő inicializálás használata során az Azure CLI 1.0 létrehozása Linux virtuális gép testreszabása"
services: virtual-machines-linux
documentationcenter: 
author: vlivech
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: 
ms.service: virtual-machines-linux
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-linux
ms.devlang: na
ms.topic: article
ms.date: 10/26/2016
ms.author: v-livech
ms.openlocfilehash: 0b6150bca333188666935b3c9aa02c4b33690db9
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="use-cloud-init-to-customize-a-linux-vm-during-creation-with-the-azure-cli-10"></a><span data-ttu-id="6db7f-103">Felhő inicializálás segítségével testre szabhatja a Linux virtuális gép létrehozása az Azure CLI 1.0 során</span><span class="sxs-lookup"><span data-stu-id="6db7f-103">Use cloud-init to customize a Linux VM during creation with the Azure CLI 1.0</span></span>
<span data-ttu-id="6db7f-104">Ez a cikk bemutatja, hogyan végezheti el a felhő inicializálás parancsfájl az állomásnév, a telepített csomagokat, és felhasználói fiókok kezelése.</span><span class="sxs-lookup"><span data-stu-id="6db7f-104">This article shows how to make a cloud-init script to set the hostname, update installed packages, and manage user accounts.</span></span>  <span data-ttu-id="6db7f-105">A felhő inicializálás parancsfájlok során a virtuális gép létrehozása az Azure CLI nevezzük.</span><span class="sxs-lookup"><span data-stu-id="6db7f-105">The cloud-init scripts are called during the VM creation from Azure CLI.</span></span>  <span data-ttu-id="6db7f-106">A cikkben foglaltak végrehajtásához szükség van:</span><span class="sxs-lookup"><span data-stu-id="6db7f-106">The article requires:</span></span>

* <span data-ttu-id="6db7f-107">egy Azure-fiókra ([ingyenes próbaverzió beszerzése](https://azure.microsoft.com/pricing/free-trial/)),</span><span class="sxs-lookup"><span data-stu-id="6db7f-107">an Azure account ([get a free trial](https://azure.microsoft.com/pricing/free-trial/)).</span></span>
* <span data-ttu-id="6db7f-108">és be kell jelentkeznie az [Azure parancssori felületre](../../cli-install-nodejs.md) a következővel: `azure login`.</span><span class="sxs-lookup"><span data-stu-id="6db7f-108">the [Azure CLI](../../cli-install-nodejs.md) logged in with `azure login`.</span></span>
* <span data-ttu-id="6db7f-109">Az Azure parancssori felületnek `azure config mode arm` Azure Resource Manager módban *kell lennie*.</span><span class="sxs-lookup"><span data-stu-id="6db7f-109">the Azure CLI *must be in* Azure Resource Manager mode `azure config mode arm`.</span></span>

## <a name="cli-versions-to-complete-the-task"></a><span data-ttu-id="6db7f-110">A feladat befejezéséhez használható CLI-verziók</span><span class="sxs-lookup"><span data-stu-id="6db7f-110">CLI versions to complete the task</span></span>
<span data-ttu-id="6db7f-111">A következő CLI-verziók egyikével elvégezheti a feladatot:</span><span class="sxs-lookup"><span data-stu-id="6db7f-111">You can complete the task using one of the following CLI versions:</span></span>

- <span data-ttu-id="6db7f-112">[Az Azure CLI 1.0](#quick-commands) – a parancssori felületen a klasszikus és resource management üzembe helyezési modellel (a cikk)</span><span class="sxs-lookup"><span data-stu-id="6db7f-112">[Azure CLI 1.0](#quick-commands) – our CLI for the classic and resource management deployment models (this article)</span></span>
- <span data-ttu-id="6db7f-113">[Azure CLI 2.0](using-cloud-init.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) – a Resource Management üzemi modellhez tartozó parancssori felületek következő generációját képviseli.</span><span class="sxs-lookup"><span data-stu-id="6db7f-113">[Azure CLI 2.0](using-cloud-init.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) - our next generation CLI for the resource management deployment model</span></span>

## <a name="quick-commands"></a><span data-ttu-id="6db7f-114">Gyors parancsok</span><span class="sxs-lookup"><span data-stu-id="6db7f-114">Quick Commands</span></span>
<span data-ttu-id="6db7f-115">Hozzon létre egy felhő-init.txt parancsfájlt, amely beállítja az állomásnevet, frissíti az összes csomagot és sudo felhasználót ad hozzá Linux.</span><span class="sxs-lookup"><span data-stu-id="6db7f-115">Create a cloud-init.txt script that sets the hostname, updates all packages, and adds a sudo user to Linux.</span></span>

```sh
#cloud-config
hostname: myVMhostname
apt_upgrade: true
users:
  - name: myNewAdminUser
    groups: sudo
    shell: /bin/bash
    sudo: ['ALL=(ALL) NOPASSWD:ALL']
    ssh-authorized-keys:
      - ssh-rsa AAAAB3<snip>==myAdminUser@myVM
```
<span data-ttu-id="6db7f-116">Hozzon létre egy erőforráscsoportot, a virtuális gépek elindítása.</span><span class="sxs-lookup"><span data-stu-id="6db7f-116">Create a resource group to launch VMs into.</span></span>

```azurecli
azure group create myResourceGroup westus
```

<span data-ttu-id="6db7f-117">Linux virtuális gép létrehozása felhőben inicializálás konfigurálásának rendszerindítás során.</span><span class="sxs-lookup"><span data-stu-id="6db7f-117">Create a Linux VM using cloud-init to configure it during boot.</span></span>

```azurecli
azure vm create \
  -g myResourceGroup \
  -n myVM \
  -l westus \
  -y Linux \
  -f myVMnic \
  -F myVNet \
  -P 10.0.0.0/22 \
  -j mySubnet \
  -k 10.0.0.0/24 \
  -Q canonical:ubuntuserver:14.04.2-LTS:latest \
  -M ~/.ssh/id_rsa.pub \
  -u myAdminUser \
  -C cloud-init.txt
```

## <a name="detailed-walkthrough"></a><span data-ttu-id="6db7f-118">Részletes bemutató</span><span class="sxs-lookup"><span data-stu-id="6db7f-118">Detailed walkthrough</span></span>
### <a name="introduction"></a><span data-ttu-id="6db7f-119">Bevezetés</span><span class="sxs-lookup"><span data-stu-id="6db7f-119">Introduction</span></span>
<span data-ttu-id="6db7f-120">Amikor új Linux virtuális gép elindításához testreszabott vagy készen áll az Ön igényeinek kap egy szabványos Linux virtuális gép, és semmi ne legyen.</span><span class="sxs-lookup"><span data-stu-id="6db7f-120">When you launch a new Linux VM, you are getting a standard Linux VM with nothing customized or ready for your needs.</span></span> <span data-ttu-id="6db7f-121">[Felhő inicializálás](https://cloudinit.readthedocs.org) szabványos módja, szúrjon be ezt a Linux virtuális Gépet egy parancsfájl vagy a konfigurációs beállítások, az első alkalommal szolgáltatáshoz indítja.</span><span class="sxs-lookup"><span data-stu-id="6db7f-121">[Cloud-init](https://cloudinit.readthedocs.org) is a standard way to inject a script or configuration settings into that Linux VM as it is booting up for the first time.</span></span>

<span data-ttu-id="6db7f-122">Azure, a három különböző módja van módosításokat Linux virtuális gép, mert folyamatban van telepítve, vagy a legközelebbi.</span><span class="sxs-lookup"><span data-stu-id="6db7f-122">On Azure, there are a three different ways to make changes onto a Linux VM as it is being deployed or booted.</span></span>

* <span data-ttu-id="6db7f-123">Parancsfájlok felhő inicializálás használatával helyezhet el.</span><span class="sxs-lookup"><span data-stu-id="6db7f-123">Inject scripts using cloud-init.</span></span>
* <span data-ttu-id="6db7f-124">Az Azure használatával parancsfájlok szúrjon [VMAccess bővítmény](using-vmaccess-extension.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="6db7f-124">Inject scripts using the Azure [VMAccess Extension](using-vmaccess-extension.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span></span>
* <span data-ttu-id="6db7f-125">Azure-sablon alapján felhő inicializálás használatával.</span><span class="sxs-lookup"><span data-stu-id="6db7f-125">An Azure template using cloud-init.</span></span>
* <span data-ttu-id="6db7f-126">Egy Azure-sablon használatával [CustomScriptExtention](extensions-customscript.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="6db7f-126">An Azure template using [CustomScriptExtention](extensions-customscript.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span></span>

<span data-ttu-id="6db7f-127">A beillesztendő parancsfájlok indítás után bármikor:</span><span class="sxs-lookup"><span data-stu-id="6db7f-127">To inject scripts at any time after boot:</span></span>

* <span data-ttu-id="6db7f-128">SSH közvetlenül a parancsok futtatásához</span><span class="sxs-lookup"><span data-stu-id="6db7f-128">SSH to run commands directly</span></span>
* <span data-ttu-id="6db7f-129">Az Azure használatával parancsfájlok szúrjon [VMAccess bővítmény](using-vmaccess-extension.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json), imperatively vagy az Azure-sablon alapján</span><span class="sxs-lookup"><span data-stu-id="6db7f-129">Inject scripts using the Azure [VMAccess Extension](using-vmaccess-extension.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json), either imperatively or in an Azure template</span></span>
* <span data-ttu-id="6db7f-130">Konfigurációs felügyeleti eszközök, például Ansible, védőérték, Chef vagy Puppet.</span><span class="sxs-lookup"><span data-stu-id="6db7f-130">Configuration management tools like Ansible, Salt, Chef, and Puppet.</span></span>

> [!NOTE]
> <span data-ttu-id="6db7f-131">: A VMAccess bővítmény legfelső szintű SSH használatával ugyanúgy lehet egy parancsfájl végrehajtása.</span><span class="sxs-lookup"><span data-stu-id="6db7f-131">: VMAccess Extension executes a script as root in the same way using SSH can.</span></span>  <span data-ttu-id="6db7f-132">Azonban a Virtuálisgép-bővítmény használatával lehetővé teszi, hogy számos hasznos a forgatókönyvtől függően az Azure által kínált.</span><span class="sxs-lookup"><span data-stu-id="6db7f-132">However, using the VM extension enables several features that Azure offers that can be useful depending upon your scenario.</span></span>
> 
> 

## <a name="cloud-init-availability-on-azure-vm-quick-create-image-aliases"></a><span data-ttu-id="6db7f-133">Felhő inicializálás elérhetőségét a Azure virtuális gép gyors-lemezkép aliasok létrehozására:</span><span class="sxs-lookup"><span data-stu-id="6db7f-133">Cloud-init availability on Azure VM quick-create image aliases:</span></span>
| <span data-ttu-id="6db7f-134">Alias</span><span class="sxs-lookup"><span data-stu-id="6db7f-134">Alias</span></span> | <span data-ttu-id="6db7f-135">Közzétevő</span><span class="sxs-lookup"><span data-stu-id="6db7f-135">Publisher</span></span> | <span data-ttu-id="6db7f-136">Ajánlat</span><span class="sxs-lookup"><span data-stu-id="6db7f-136">Offer</span></span> | <span data-ttu-id="6db7f-137">SKU</span><span class="sxs-lookup"><span data-stu-id="6db7f-137">SKU</span></span> | <span data-ttu-id="6db7f-138">Verzió</span><span class="sxs-lookup"><span data-stu-id="6db7f-138">Version</span></span> | <span data-ttu-id="6db7f-139">felhő inicializálás</span><span class="sxs-lookup"><span data-stu-id="6db7f-139">cloud-init</span></span> |
|:--- |:--- |:--- |:--- |:--- |:--- |
| <span data-ttu-id="6db7f-140">CentOS</span><span class="sxs-lookup"><span data-stu-id="6db7f-140">CentOS</span></span> |<span data-ttu-id="6db7f-141">OpenLogic</span><span class="sxs-lookup"><span data-stu-id="6db7f-141">OpenLogic</span></span> |<span data-ttu-id="6db7f-142">Centos</span><span class="sxs-lookup"><span data-stu-id="6db7f-142">Centos</span></span> |<span data-ttu-id="6db7f-143">7.2</span><span class="sxs-lookup"><span data-stu-id="6db7f-143">7.2</span></span> |<span data-ttu-id="6db7f-144">legújabb</span><span class="sxs-lookup"><span data-stu-id="6db7f-144">latest</span></span> |<span data-ttu-id="6db7f-145">nem</span><span class="sxs-lookup"><span data-stu-id="6db7f-145">no</span></span> |
| <span data-ttu-id="6db7f-146">CoreOS</span><span class="sxs-lookup"><span data-stu-id="6db7f-146">CoreOS</span></span> |<span data-ttu-id="6db7f-147">CoreOS</span><span class="sxs-lookup"><span data-stu-id="6db7f-147">CoreOS</span></span> |<span data-ttu-id="6db7f-148">CoreOS</span><span class="sxs-lookup"><span data-stu-id="6db7f-148">CoreOS</span></span> |<span data-ttu-id="6db7f-149">Stable</span><span class="sxs-lookup"><span data-stu-id="6db7f-149">Stable</span></span> |<span data-ttu-id="6db7f-150">legújabb</span><span class="sxs-lookup"><span data-stu-id="6db7f-150">latest</span></span> |<span data-ttu-id="6db7f-151">igen</span><span class="sxs-lookup"><span data-stu-id="6db7f-151">yes</span></span> |
| <span data-ttu-id="6db7f-152">Debian</span><span class="sxs-lookup"><span data-stu-id="6db7f-152">Debian</span></span> |<span data-ttu-id="6db7f-153">credativ</span><span class="sxs-lookup"><span data-stu-id="6db7f-153">credativ</span></span> |<span data-ttu-id="6db7f-154">Debian</span><span class="sxs-lookup"><span data-stu-id="6db7f-154">Debian</span></span> |<span data-ttu-id="6db7f-155">8</span><span class="sxs-lookup"><span data-stu-id="6db7f-155">8</span></span> |<span data-ttu-id="6db7f-156">legújabb</span><span class="sxs-lookup"><span data-stu-id="6db7f-156">latest</span></span> |<span data-ttu-id="6db7f-157">nem</span><span class="sxs-lookup"><span data-stu-id="6db7f-157">no</span></span> |
| <span data-ttu-id="6db7f-158">openSUSE</span><span class="sxs-lookup"><span data-stu-id="6db7f-158">openSUSE</span></span> |<span data-ttu-id="6db7f-159">SUSE</span><span class="sxs-lookup"><span data-stu-id="6db7f-159">SUSE</span></span> |<span data-ttu-id="6db7f-160">openSUSE</span><span class="sxs-lookup"><span data-stu-id="6db7f-160">openSUSE</span></span> |<span data-ttu-id="6db7f-161">13.2</span><span class="sxs-lookup"><span data-stu-id="6db7f-161">13.2</span></span> |<span data-ttu-id="6db7f-162">legújabb</span><span class="sxs-lookup"><span data-stu-id="6db7f-162">latest</span></span> |<span data-ttu-id="6db7f-163">nem</span><span class="sxs-lookup"><span data-stu-id="6db7f-163">no</span></span> |
| <span data-ttu-id="6db7f-164">RHEL</span><span class="sxs-lookup"><span data-stu-id="6db7f-164">RHEL</span></span> |<span data-ttu-id="6db7f-165">Redhat</span><span class="sxs-lookup"><span data-stu-id="6db7f-165">Redhat</span></span> |<span data-ttu-id="6db7f-166">RHEL</span><span class="sxs-lookup"><span data-stu-id="6db7f-166">RHEL</span></span> |<span data-ttu-id="6db7f-167">7.2</span><span class="sxs-lookup"><span data-stu-id="6db7f-167">7.2</span></span> |<span data-ttu-id="6db7f-168">legújabb</span><span class="sxs-lookup"><span data-stu-id="6db7f-168">latest</span></span> |<span data-ttu-id="6db7f-169">nem</span><span class="sxs-lookup"><span data-stu-id="6db7f-169">no</span></span> |
| <span data-ttu-id="6db7f-170">UbuntuLTS</span><span class="sxs-lookup"><span data-stu-id="6db7f-170">UbuntuLTS</span></span> |<span data-ttu-id="6db7f-171">Canonical</span><span class="sxs-lookup"><span data-stu-id="6db7f-171">Canonical</span></span> |<span data-ttu-id="6db7f-172">UbuntuServer</span><span class="sxs-lookup"><span data-stu-id="6db7f-172">UbuntuServer</span></span> |<span data-ttu-id="6db7f-173">14.04.4-LTS</span><span class="sxs-lookup"><span data-stu-id="6db7f-173">14.04.4-LTS</span></span> |<span data-ttu-id="6db7f-174">legújabb</span><span class="sxs-lookup"><span data-stu-id="6db7f-174">latest</span></span> |<span data-ttu-id="6db7f-175">igen</span><span class="sxs-lookup"><span data-stu-id="6db7f-175">yes</span></span> |

<span data-ttu-id="6db7f-176">A Microsoft a partnerekkel együttműködve az beszerzése felhő inicializálás tartalmazza, és működik-e az Azure biztosít lemezképeket a dolgozik.</span><span class="sxs-lookup"><span data-stu-id="6db7f-176">Microsoft is working with our partners to get cloud-init included and working in the images that they provide to Azure.</span></span>

## <a name="adding-a-cloud-init-script-to-the-vm-creation-with-the-azure-cli"></a><span data-ttu-id="6db7f-177">A felhő inicializálás parancsfájl hozzáadása a virtuális gép létrehozása az Azure parancssori felülettel</span><span class="sxs-lookup"><span data-stu-id="6db7f-177">Adding a cloud-init script to the VM creation with the Azure CLI</span></span>
<span data-ttu-id="6db7f-178">Indítsa el a felhő inicializálás parancsfájl, amikor egy virtuális gép létrehozása az Azure-ban, adja meg a felhő inicializálás fájlt az Azure parancssori felület használatával `--custom-data` váltani.</span><span class="sxs-lookup"><span data-stu-id="6db7f-178">To launch a cloud-init script when creating a VM in Azure, specify the cloud-init file using the Azure CLI `--custom-data` switch.</span></span>

<span data-ttu-id="6db7f-179">Hozzon létre egy erőforráscsoportot, a virtuális gépek elindítása.</span><span class="sxs-lookup"><span data-stu-id="6db7f-179">Create a resource group to launch VMs into.</span></span>

```azurecli
azure group create myResourceGroup westus
```

<span data-ttu-id="6db7f-180">Linux virtuális gép létrehozása felhőben inicializálás konfigurálásának rendszerindítás során.</span><span class="sxs-lookup"><span data-stu-id="6db7f-180">Create a Linux VM using cloud-init to configure it during boot.</span></span>

```azurecli
azure vm create \
  --resource-group myResourceGroup \
  --name myVM \
  --location westus \
  --os-type Linux \
  --nic-name myVMnic \
  --vnet-name myVNet \
  --vnet-address-prefix 10.0.0.0/22 \
  --vnet-subnet-name mySubnet \
  --vnet-subnet-address-prefix 10.0.0.0/24 \
  --image-urn canonical:ubuntuserver:14.04.2-LTS:latest \
  --ssh-publickey-file ~/.ssh/id_rsa.pub \
  --admin-username myAdminUser \
  --custom-data cloud-init.txt
```

## <a name="creating-a-cloud-init-script-to-set-the-hostname-of-a-linux-vm"></a><span data-ttu-id="6db7f-181">Állítsa be a Linux virtuális gép állomásnevét felhő inicializálás parancsfájl létrehozása</span><span class="sxs-lookup"><span data-stu-id="6db7f-181">Creating a cloud-init script to set the hostname of a Linux VM</span></span>
<span data-ttu-id="6db7f-182">A legegyszerűbb és legfontosabb beállításait minden Linux virtuális gép egyik lenne az állomásnevet.</span><span class="sxs-lookup"><span data-stu-id="6db7f-182">One of the simplest and most important settings for any Linux VM would be the hostname.</span></span> <span data-ttu-id="6db7f-183">A Microsoft könnyedén állíthat be a felhő inicializálás használja ezt a parancsfájlt.</span><span class="sxs-lookup"><span data-stu-id="6db7f-183">We can easily set this using cloud-init with this script.</span></span>  

### <a name="example-cloud-init-script-named-cloudconfighostnametxt"></a><span data-ttu-id="6db7f-184">Példa felhő inicializálás parancsfájl nevű `cloud_config_hostname.txt`.</span><span class="sxs-lookup"><span data-stu-id="6db7f-184">Example cloud-init script named `cloud_config_hostname.txt`.</span></span>
```sh
#cloud-config
hostname: myservername
```

<span data-ttu-id="6db7f-185">A virtuális gép kezdeti indításkor, a felhő inicializálás parancsfájl értékűre állítja be az állomásnév `myservername`.</span><span class="sxs-lookup"><span data-stu-id="6db7f-185">During the initial startup of the VM, this cloud-init script sets the hostname to `myservername`.</span></span>

```azurecli
azure vm create \
  --resource-group myResourceGroup \
  --name myVM \
  --location westus \
  --os-type Linux \
  --nic-name myVMnic \
  --vnet-name myVNet \
  --vnet-address-prefix 10.0.0.0/22 \
  --vnet-subnet-name mySubNet \
  --vnet-subnet-address-prefix 10.0.0.0/24 \
  --image-urn canonical:ubuntuserver:14.04.2-LTS:latest \
  --ssh-publickey-file ~/.ssh/id_rsa.pub \
  --admin-username myAdminUser \
  --custom-data cloud_config_hostname.txt
```

<span data-ttu-id="6db7f-186">Bejelentkezés, és ellenőrizze az új virtuális gép állomásnevét.</span><span class="sxs-lookup"><span data-stu-id="6db7f-186">Login and verify the hostname of the new VM.</span></span>

```bash
ssh myVM
hostname
myservername
```

## <a name="creating-a-cloud-init-script-to-update-linux"></a><span data-ttu-id="6db7f-187">Linux frissítése felhő inicializálás parancsfájl létrehozása</span><span class="sxs-lookup"><span data-stu-id="6db7f-187">Creating a cloud-init script to update Linux</span></span>
<span data-ttu-id="6db7f-188">A biztonság érdekében célszerű az Ubuntu virtuális gép frissítése a első indításakor.</span><span class="sxs-lookup"><span data-stu-id="6db7f-188">For security, you want your Ubuntu VM to update on the first boot.</span></span>  <span data-ttu-id="6db7f-189">Felhő inicializálás is nézzük meg, attól függően, hogy a Linux-disztribúció használ, a kövesse parancsfájl használatával.</span><span class="sxs-lookup"><span data-stu-id="6db7f-189">Using cloud-init we can do that with the follow script, depending on the Linux distribution you are using.</span></span>

### <a name="example-cloud-init-script-cloudconfigaptupgradetxt-for-the-debian-family"></a><span data-ttu-id="6db7f-190">Példa felhő inicializálás parancsfájl `cloud_config_apt_upgrade.txt` a Debian termékcsalád</span><span class="sxs-lookup"><span data-stu-id="6db7f-190">Example cloud-init script `cloud_config_apt_upgrade.txt` for the Debian Family</span></span>
```sh
#cloud-config
apt_upgrade: true
```

<span data-ttu-id="6db7f-191">Miután Linux betöltődött, a telepített csomagok segítségével frissített `apt-get`.</span><span class="sxs-lookup"><span data-stu-id="6db7f-191">After Linux has booted, all the installed packages are updated via `apt-get`.</span></span>

```azurecli
azure vm create \
  --resource-group myResourceGroup \
  --name myVM \
  --location westus \
  --os-type Linux \
  --nic-name myVMnic \
  --vnet-name myVNet \
  --vnet-address-prefix 10.0.0.0/22 \
  --vnet-subnet-name mySubNet \
  --vnet-subnet-address-prefix 10.0.0.0/24 \
  --image-urn canonical:ubuntuserver:14.04.2-LTS:latest \
  --ssh-publickey-file ~/.ssh/id_rsa.pub \
  --admin-username myAdminUser \
  --custom-data cloud_config_apt_upgrade.txt
```

<span data-ttu-id="6db7f-192">Bejelentkezés, és ellenőrizze, hogy az összes csomag frissítése.</span><span class="sxs-lookup"><span data-stu-id="6db7f-192">Login and verify all packages are updated.</span></span>

```bash
ssh myUbuntuVM
sudo apt-get upgrade
Reading package lists... Done
Building dependency tree
Reading state information... Done
Calculating upgrade... Done
The following packages have been kept back:
  linux-generic linux-headers-generic linux-image-generic
0 upgraded, 0 newly installed, 0 to remove and 0 not upgraded.
```

## <a name="creating-a-cloud-init-script-to-add-a-user-to-linux"></a><span data-ttu-id="6db7f-193">Felhasználó hozzáadása Linux felhő inicializálás parancsfájl létrehozása</span><span class="sxs-lookup"><span data-stu-id="6db7f-193">Creating a cloud-init script to add a user to Linux</span></span>
<span data-ttu-id="6db7f-194">Az első feladatok számára bármely új Linux virtuális gép egyik felhasználó hozzáadása a szolgáltatást, vagy kerülje a `root`.</span><span class="sxs-lookup"><span data-stu-id="6db7f-194">One of the first tasks on any new Linux VM is to add a user for yourself or to avoid using `root`.</span></span> <span data-ttu-id="6db7f-195">SSH kulcs-e a biztonság és a használhatóság ajánlott eljárás, és hozzáadja őket a `~/.ssh/authorized_keys` a felhő inicializálás parancsfájl-fájlt.</span><span class="sxs-lookup"><span data-stu-id="6db7f-195">SSH keys are best practice for security and for usability and they are added to the `~/.ssh/authorized_keys` file with this cloud-init script.</span></span>

### <a name="example-cloud-init-script-cloudconfigadduserstxt-for-debian-family"></a><span data-ttu-id="6db7f-196">Példa felhő inicializálás parancsfájl `cloud_config_add_users.txt` Debian termékcsalád</span><span class="sxs-lookup"><span data-stu-id="6db7f-196">Example cloud-init script `cloud_config_add_users.txt` for Debian Family</span></span>
```sh
#cloud-config
users:
  - name: myCloudInitAddedAdminUser
    groups: sudo
    shell: /bin/bash
    sudo: ['ALL=(ALL) NOPASSWD:ALL']
    ssh-authorized-keys:
      - ssh-rsa AAAAB3<snip>==myAdminUser@myUbuntuVM
```

<span data-ttu-id="6db7f-197">Miután Linux betöltődött, a felsorolt felhasználók létrehozása és a sudo csoportba felvett.</span><span class="sxs-lookup"><span data-stu-id="6db7f-197">After Linux has booted, all the listed users are created and added to the sudo group.</span></span>

```azurecli
azure vm create \
  --resource-group myResourceGroup \
  --name myVM \
  --location westus \
  --os-type Linux \
  --nic-name myVMnic \
  --vnet-name myVNet \
  --vnet-address-prefix 10.0.0.0/22 \
  --vnet-subnet-name mySubNet \
  --vnet-subnet-address-prefix 10.0.0.0/24 \
  --image-urn canonical:ubuntuserver:14.04.2-LTS:latest \
  --ssh-publickey-file ~/.ssh/id_rsa.pub \
  --admin-username myAdminUser \
  --custom-data cloud_config_add_users.txt
```

<span data-ttu-id="6db7f-198">Bejelentkezés, és ellenőrizze az újonnan létrehozott felhasználó.</span><span class="sxs-lookup"><span data-stu-id="6db7f-198">Login and verify the newly created user.</span></span>

```bash
ssh myVM
cat /etc/group
```

<span data-ttu-id="6db7f-199">Kimenet</span><span class="sxs-lookup"><span data-stu-id="6db7f-199">Output</span></span>

```bash
root:x:0:
<snip />
sudo:x:27:myCloudInitAddedAdminUser
<snip />
myCloudInitAddedAdminUser:x:1000:
```

## <a name="next-steps"></a><span data-ttu-id="6db7f-200">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="6db7f-200">Next Steps</span></span>
<span data-ttu-id="6db7f-201">Felhő inicializálás szabványos úgy lehet módosítani a Linux virtuális gép rendszerindító válik.</span><span class="sxs-lookup"><span data-stu-id="6db7f-201">Cloud-init is becoming one standard way to modify your Linux VM on boot.</span></span> <span data-ttu-id="6db7f-202">Azure is rendelkezik a Virtuálisgép-bővítmények, amelyek lehetővé teszik a LinuxVM rendszerindító, vagy futás közben módosítani.</span><span class="sxs-lookup"><span data-stu-id="6db7f-202">Azure also has VM extensions, which allow you to modify your LinuxVM on boot or while it is running.</span></span> <span data-ttu-id="6db7f-203">Például használhatja az Azure VMAccessExtension SSH vagy felhasználói adatok visszaállítására, a virtuális gép futása közben.</span><span class="sxs-lookup"><span data-stu-id="6db7f-203">For example, you can use the Azure VMAccessExtension to reset SSH or user information while the VM is running.</span></span> <span data-ttu-id="6db7f-204">A felhő inicializálás a jelszó alaphelyzetbe állítása újraindítás kellene.</span><span class="sxs-lookup"><span data-stu-id="6db7f-204">With cloud-init, you would need a reboot to reset the password.</span></span>

[<span data-ttu-id="6db7f-205">Virtuálisgép-bővítmények és szolgáltatásokkal kapcsolatban</span><span class="sxs-lookup"><span data-stu-id="6db7f-205">About virtual machine extensions and features</span></span>](../windows/extensions-features.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)

[<span data-ttu-id="6db7f-206">Kezelheti a felhasználókat, az SSH és az ellenőrzése vagy javítása Azure virtuális gépeken Linux a VMAccess bővítmény használatával lemezek</span><span class="sxs-lookup"><span data-stu-id="6db7f-206">Manage users, SSH, and check or repair disks on Azure Linux VMs using the VMAccess Extension</span></span>](using-vmaccess-extension.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)

