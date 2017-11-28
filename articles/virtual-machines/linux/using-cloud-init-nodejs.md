---
title: "aaaUsing felhő inicializálás toocustomize Linux virtuális gép létrehozása az Azure-ban |} Microsoft Docs"
description: "Hogyan toouse felhő inicializálás toocustomize a Linux virtuális gép létrehozása során hello Azure CLI 1.0"
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
ms.openlocfilehash: b9f480bd04029956d0593bbef931795733cbc2f6
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="use-cloud-init-toocustomize-a-linux-vm-during-creation-with-hello-azure-cli-10"></a><span data-ttu-id="4badd-103">Linux virtuális gép felhőalapú inicializálás toocustomize közbeni hello Azure CLI 1.0 létrehozása</span><span class="sxs-lookup"><span data-stu-id="4badd-103">Use cloud-init toocustomize a Linux VM during creation with hello Azure CLI 1.0</span></span>
<span data-ttu-id="4badd-104">Ez a cikk bemutatja, hogyan toomake egy felhő-init parancsfájl tooset állomásnév hello telepített csomagok frissítése és felhasználói fiókok kezelése.</span><span class="sxs-lookup"><span data-stu-id="4badd-104">This article shows how toomake a cloud-init script tooset hello hostname, update installed packages, and manage user accounts.</span></span>  <span data-ttu-id="4badd-105">hello felhő inicializálás parancsfájlok során a virtuális gép létrehozása az Azure CLI hello nevezzük.</span><span class="sxs-lookup"><span data-stu-id="4badd-105">hello cloud-init scripts are called during hello VM creation from Azure CLI.</span></span>  <span data-ttu-id="4badd-106">hello cikk van szükség:</span><span class="sxs-lookup"><span data-stu-id="4badd-106">hello article requires:</span></span>

* <span data-ttu-id="4badd-107">egy Azure-fiókra ([ingyenes próbaverzió beszerzése](https://azure.microsoft.com/pricing/free-trial/)),</span><span class="sxs-lookup"><span data-stu-id="4badd-107">an Azure account ([get a free trial](https://azure.microsoft.com/pricing/free-trial/)).</span></span>
* <span data-ttu-id="4badd-108">Hello [Azure CLI](../../cli-install-nodejs.md) bejelentkezett `azure login`.</span><span class="sxs-lookup"><span data-stu-id="4badd-108">hello [Azure CLI](../../cli-install-nodejs.md) logged in with `azure login`.</span></span>
* <span data-ttu-id="4badd-109">az Azure parancssori felület hello *kell* Azure Resource Manager módra `azure config mode arm`.</span><span class="sxs-lookup"><span data-stu-id="4badd-109">hello Azure CLI *must be in* Azure Resource Manager mode `azure config mode arm`.</span></span>

## <a name="cli-versions-toocomplete-hello-task"></a><span data-ttu-id="4badd-110">Parancssori felület verziók toocomplete hello feladat</span><span class="sxs-lookup"><span data-stu-id="4badd-110">CLI versions toocomplete hello task</span></span>
<span data-ttu-id="4badd-111">Hello feladat a következő parancssori felület verziók hello egyikével hajthatja végre:</span><span class="sxs-lookup"><span data-stu-id="4badd-111">You can complete hello task using one of hello following CLI versions:</span></span>

- <span data-ttu-id="4badd-112">[Az Azure CLI 1.0](#quick-commands) – a parancssori felületen hello klasszikus és resource management üzembe helyezési modellel (a cikk)</span><span class="sxs-lookup"><span data-stu-id="4badd-112">[Azure CLI 1.0](#quick-commands) – our CLI for hello classic and resource management deployment models (this article)</span></span>
- <span data-ttu-id="4badd-113">[Az Azure CLI 2.0](using-cloud-init.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) -a következő generációs CLI hello erőforrás felügyeleti telepítési modell</span><span class="sxs-lookup"><span data-stu-id="4badd-113">[Azure CLI 2.0](using-cloud-init.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) - our next generation CLI for hello resource management deployment model</span></span>

## <a name="quick-commands"></a><span data-ttu-id="4badd-114">Gyors parancsok</span><span class="sxs-lookup"><span data-stu-id="4badd-114">Quick Commands</span></span>
<span data-ttu-id="4badd-115">Hozzon létre egy felhő-init.txt parancsfájlt, amely beállítja a hello állomásnév, frissíti az összes csomag, a sudo felhasználói tooLinux hozzáadja.</span><span class="sxs-lookup"><span data-stu-id="4badd-115">Create a cloud-init.txt script that sets hello hostname, updates all packages, and adds a sudo user tooLinux.</span></span>

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
<span data-ttu-id="4badd-116">Hozzon létre egy erőforrás csoport toolaunch a virtuális gépek.</span><span class="sxs-lookup"><span data-stu-id="4badd-116">Create a resource group toolaunch VMs into.</span></span>

```azurecli
azure group create myResourceGroup westus
```

<span data-ttu-id="4badd-117">Linux virtuális gép létrehozása felhőben inicializálás tooconfigure azt a rendszerindítás során.</span><span class="sxs-lookup"><span data-stu-id="4badd-117">Create a Linux VM using cloud-init tooconfigure it during boot.</span></span>

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

## <a name="detailed-walkthrough"></a><span data-ttu-id="4badd-118">Részletes bemutató</span><span class="sxs-lookup"><span data-stu-id="4badd-118">Detailed walkthrough</span></span>
### <a name="introduction"></a><span data-ttu-id="4badd-119">Bevezetés</span><span class="sxs-lookup"><span data-stu-id="4badd-119">Introduction</span></span>
<span data-ttu-id="4badd-120">Amikor új Linux virtuális gép elindításához testreszabott vagy készen áll az Ön igényeinek kap egy szabványos Linux virtuális gép, és semmi ne legyen.</span><span class="sxs-lookup"><span data-stu-id="4badd-120">When you launch a new Linux VM, you are getting a standard Linux VM with nothing customized or ready for your needs.</span></span> <span data-ttu-id="4badd-121">[Felhő inicializálás](https://cloudinit.readthedocs.org) megegyezik a szokásos módon tooinject egy parancsfájl vagy a konfigurációs beállításokat a Linux virtuális gép akkor indítja az hello szolgáltatáshoz első alkalommal.</span><span class="sxs-lookup"><span data-stu-id="4badd-121">[Cloud-init](https://cloudinit.readthedocs.org) is a standard way tooinject a script or configuration settings into that Linux VM as it is booting up for hello first time.</span></span>

<span data-ttu-id="4badd-122">A Azure-ban módosulnak három különböző módon toomake Linux virtuális gép, mert folyamatban van telepítve, vagy a legközelebbi.</span><span class="sxs-lookup"><span data-stu-id="4badd-122">On Azure, there are a three different ways toomake changes onto a Linux VM as it is being deployed or booted.</span></span>

* <span data-ttu-id="4badd-123">Parancsfájlok felhő inicializálás használatával helyezhet el.</span><span class="sxs-lookup"><span data-stu-id="4badd-123">Inject scripts using cloud-init.</span></span>
* <span data-ttu-id="4badd-124">Parancsfájlok hello Azure használatával szúrjon [VMAccess bővítmény](using-vmaccess-extension.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="4badd-124">Inject scripts using hello Azure [VMAccess Extension](using-vmaccess-extension.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span></span>
* <span data-ttu-id="4badd-125">Azure-sablon alapján felhő inicializálás használatával.</span><span class="sxs-lookup"><span data-stu-id="4badd-125">An Azure template using cloud-init.</span></span>
* <span data-ttu-id="4badd-126">Egy Azure-sablon használatával [CustomScriptExtention](extensions-customscript.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="4badd-126">An Azure template using [CustomScriptExtention](extensions-customscript.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span></span>

<span data-ttu-id="4badd-127">indítás után bármikor parancsfájlok tooinject:</span><span class="sxs-lookup"><span data-stu-id="4badd-127">tooinject scripts at any time after boot:</span></span>

* <span data-ttu-id="4badd-128">SSH toorun közvetlenül parancsok</span><span class="sxs-lookup"><span data-stu-id="4badd-128">SSH toorun commands directly</span></span>
* <span data-ttu-id="4badd-129">Parancsfájlok hello Azure használatával szúrjon [VMAccess bővítmény](using-vmaccess-extension.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json), imperatively vagy az Azure-sablon alapján</span><span class="sxs-lookup"><span data-stu-id="4badd-129">Inject scripts using hello Azure [VMAccess Extension](using-vmaccess-extension.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json), either imperatively or in an Azure template</span></span>
* <span data-ttu-id="4badd-130">Konfigurációs felügyeleti eszközök, például Ansible, védőérték, Chef vagy Puppet.</span><span class="sxs-lookup"><span data-stu-id="4badd-130">Configuration management tools like Ansible, Salt, Chef, and Puppet.</span></span>

> [!NOTE]
> <span data-ttu-id="4badd-131">: A VMAccess bővítmény hajt végre egy parancsfájl hello a legfelső szintű azonos módon tudja módon az SSH használatával.</span><span class="sxs-lookup"><span data-stu-id="4badd-131">: VMAccess Extension executes a script as root in hello same way using SSH can.</span></span>  <span data-ttu-id="4badd-132">Azonban hello Virtuálisgép-bővítmény használatával lehetővé teszi, hogy számos hasznos a forgatókönyvtől függően az Azure által kínált.</span><span class="sxs-lookup"><span data-stu-id="4badd-132">However, using hello VM extension enables several features that Azure offers that can be useful depending upon your scenario.</span></span>
> 
> 

## <a name="cloud-init-availability-on-azure-vm-quick-create-image-aliases"></a><span data-ttu-id="4badd-133">Felhő inicializálás elérhetőségét a Azure virtuális gép gyors-lemezkép aliasok létrehozására:</span><span class="sxs-lookup"><span data-stu-id="4badd-133">Cloud-init availability on Azure VM quick-create image aliases:</span></span>
| <span data-ttu-id="4badd-134">Alias</span><span class="sxs-lookup"><span data-stu-id="4badd-134">Alias</span></span> | <span data-ttu-id="4badd-135">Közzétevő</span><span class="sxs-lookup"><span data-stu-id="4badd-135">Publisher</span></span> | <span data-ttu-id="4badd-136">Ajánlat</span><span class="sxs-lookup"><span data-stu-id="4badd-136">Offer</span></span> | <span data-ttu-id="4badd-137">SKU</span><span class="sxs-lookup"><span data-stu-id="4badd-137">SKU</span></span> | <span data-ttu-id="4badd-138">Verzió</span><span class="sxs-lookup"><span data-stu-id="4badd-138">Version</span></span> | <span data-ttu-id="4badd-139">felhő inicializálás</span><span class="sxs-lookup"><span data-stu-id="4badd-139">cloud-init</span></span> |
|:--- |:--- |:--- |:--- |:--- |:--- |
| <span data-ttu-id="4badd-140">CentOS</span><span class="sxs-lookup"><span data-stu-id="4badd-140">CentOS</span></span> |<span data-ttu-id="4badd-141">OpenLogic</span><span class="sxs-lookup"><span data-stu-id="4badd-141">OpenLogic</span></span> |<span data-ttu-id="4badd-142">Centos</span><span class="sxs-lookup"><span data-stu-id="4badd-142">Centos</span></span> |<span data-ttu-id="4badd-143">7.2</span><span class="sxs-lookup"><span data-stu-id="4badd-143">7.2</span></span> |<span data-ttu-id="4badd-144">legújabb</span><span class="sxs-lookup"><span data-stu-id="4badd-144">latest</span></span> |<span data-ttu-id="4badd-145">nem</span><span class="sxs-lookup"><span data-stu-id="4badd-145">no</span></span> |
| <span data-ttu-id="4badd-146">CoreOS</span><span class="sxs-lookup"><span data-stu-id="4badd-146">CoreOS</span></span> |<span data-ttu-id="4badd-147">CoreOS</span><span class="sxs-lookup"><span data-stu-id="4badd-147">CoreOS</span></span> |<span data-ttu-id="4badd-148">CoreOS</span><span class="sxs-lookup"><span data-stu-id="4badd-148">CoreOS</span></span> |<span data-ttu-id="4badd-149">Stable</span><span class="sxs-lookup"><span data-stu-id="4badd-149">Stable</span></span> |<span data-ttu-id="4badd-150">legújabb</span><span class="sxs-lookup"><span data-stu-id="4badd-150">latest</span></span> |<span data-ttu-id="4badd-151">igen</span><span class="sxs-lookup"><span data-stu-id="4badd-151">yes</span></span> |
| <span data-ttu-id="4badd-152">Debian</span><span class="sxs-lookup"><span data-stu-id="4badd-152">Debian</span></span> |<span data-ttu-id="4badd-153">credativ</span><span class="sxs-lookup"><span data-stu-id="4badd-153">credativ</span></span> |<span data-ttu-id="4badd-154">Debian</span><span class="sxs-lookup"><span data-stu-id="4badd-154">Debian</span></span> |<span data-ttu-id="4badd-155">8</span><span class="sxs-lookup"><span data-stu-id="4badd-155">8</span></span> |<span data-ttu-id="4badd-156">legújabb</span><span class="sxs-lookup"><span data-stu-id="4badd-156">latest</span></span> |<span data-ttu-id="4badd-157">nem</span><span class="sxs-lookup"><span data-stu-id="4badd-157">no</span></span> |
| <span data-ttu-id="4badd-158">openSUSE</span><span class="sxs-lookup"><span data-stu-id="4badd-158">openSUSE</span></span> |<span data-ttu-id="4badd-159">SUSE</span><span class="sxs-lookup"><span data-stu-id="4badd-159">SUSE</span></span> |<span data-ttu-id="4badd-160">openSUSE</span><span class="sxs-lookup"><span data-stu-id="4badd-160">openSUSE</span></span> |<span data-ttu-id="4badd-161">13.2</span><span class="sxs-lookup"><span data-stu-id="4badd-161">13.2</span></span> |<span data-ttu-id="4badd-162">legújabb</span><span class="sxs-lookup"><span data-stu-id="4badd-162">latest</span></span> |<span data-ttu-id="4badd-163">nem</span><span class="sxs-lookup"><span data-stu-id="4badd-163">no</span></span> |
| <span data-ttu-id="4badd-164">RHEL</span><span class="sxs-lookup"><span data-stu-id="4badd-164">RHEL</span></span> |<span data-ttu-id="4badd-165">Redhat</span><span class="sxs-lookup"><span data-stu-id="4badd-165">Redhat</span></span> |<span data-ttu-id="4badd-166">RHEL</span><span class="sxs-lookup"><span data-stu-id="4badd-166">RHEL</span></span> |<span data-ttu-id="4badd-167">7.2</span><span class="sxs-lookup"><span data-stu-id="4badd-167">7.2</span></span> |<span data-ttu-id="4badd-168">legújabb</span><span class="sxs-lookup"><span data-stu-id="4badd-168">latest</span></span> |<span data-ttu-id="4badd-169">nem</span><span class="sxs-lookup"><span data-stu-id="4badd-169">no</span></span> |
| <span data-ttu-id="4badd-170">UbuntuLTS</span><span class="sxs-lookup"><span data-stu-id="4badd-170">UbuntuLTS</span></span> |<span data-ttu-id="4badd-171">Canonical</span><span class="sxs-lookup"><span data-stu-id="4badd-171">Canonical</span></span> |<span data-ttu-id="4badd-172">UbuntuServer</span><span class="sxs-lookup"><span data-stu-id="4badd-172">UbuntuServer</span></span> |<span data-ttu-id="4badd-173">14.04.4-LTS</span><span class="sxs-lookup"><span data-stu-id="4badd-173">14.04.4-LTS</span></span> |<span data-ttu-id="4badd-174">legújabb</span><span class="sxs-lookup"><span data-stu-id="4badd-174">latest</span></span> |<span data-ttu-id="4badd-175">igen</span><span class="sxs-lookup"><span data-stu-id="4badd-175">yes</span></span> |

<span data-ttu-id="4badd-176">A Microsoft a partnerek tooget felhő inicializálás része azokkal, illetve működik, hogy ezek biztosítanak tooAzure hello képek.</span><span class="sxs-lookup"><span data-stu-id="4badd-176">Microsoft is working with our partners tooget cloud-init included and working in hello images that they provide tooAzure.</span></span>

## <a name="adding-a-cloud-init-script-toohello-vm-creation-with-hello-azure-cli"></a><span data-ttu-id="4badd-177">A felhő inicializálás parancsfájl toohello virtuális gép létrehozása hello Azure parancssori felület Hozzáadás</span><span class="sxs-lookup"><span data-stu-id="4badd-177">Adding a cloud-init script toohello VM creation with hello Azure CLI</span></span>
<span data-ttu-id="4badd-178">toolaunch felhő inicializálás parancsfájl az Azure virtuális gép létrehozásakor adja meg a hello felhő inicializálás fájlt hello Azure parancssori felület használatával `--custom-data` váltani.</span><span class="sxs-lookup"><span data-stu-id="4badd-178">toolaunch a cloud-init script when creating a VM in Azure, specify hello cloud-init file using hello Azure CLI `--custom-data` switch.</span></span>

<span data-ttu-id="4badd-179">Hozzon létre egy erőforrás csoport toolaunch a virtuális gépek.</span><span class="sxs-lookup"><span data-stu-id="4badd-179">Create a resource group toolaunch VMs into.</span></span>

```azurecli
azure group create myResourceGroup westus
```

<span data-ttu-id="4badd-180">Linux virtuális gép létrehozása felhőben inicializálás tooconfigure azt a rendszerindítás során.</span><span class="sxs-lookup"><span data-stu-id="4badd-180">Create a Linux VM using cloud-init tooconfigure it during boot.</span></span>

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

## <a name="creating-a-cloud-init-script-tooset-hello-hostname-of-a-linux-vm"></a><span data-ttu-id="4badd-181">A felhő inicializálás parancsfájl tooset hello állomásnevét Linux virtuális gép létrehozása</span><span class="sxs-lookup"><span data-stu-id="4badd-181">Creating a cloud-init script tooset hello hostname of a Linux VM</span></span>
<span data-ttu-id="4badd-182">Egyik legegyszerűbb hello és a Linux virtuális gép számára legfontosabb beállítások hello állomásnév lenne.</span><span class="sxs-lookup"><span data-stu-id="4badd-182">One of hello simplest and most important settings for any Linux VM would be hello hostname.</span></span> <span data-ttu-id="4badd-183">A Microsoft könnyedén állíthat be a felhő inicializálás használja ezt a parancsfájlt.</span><span class="sxs-lookup"><span data-stu-id="4badd-183">We can easily set this using cloud-init with this script.</span></span>  

### <a name="example-cloud-init-script-named-cloudconfighostnametxt"></a><span data-ttu-id="4badd-184">Példa felhő inicializálás parancsfájl nevű `cloud_config_hostname.txt`.</span><span class="sxs-lookup"><span data-stu-id="4badd-184">Example cloud-init script named `cloud_config_hostname.txt`.</span></span>
```sh
#cloud-config
hostname: myservername
```

<span data-ttu-id="4badd-185">Hello virtuális gép kezdeti indításkor hello, a felhő inicializálás parancsprogram beállítja hello állomásnév túl`myservername`.</span><span class="sxs-lookup"><span data-stu-id="4badd-185">During hello initial startup of hello VM, this cloud-init script sets hello hostname too`myservername`.</span></span>

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

<span data-ttu-id="4badd-186">Bejelentkezés, és ellenőrizze a hello hello állomásnevét új virtuális Gépet.</span><span class="sxs-lookup"><span data-stu-id="4badd-186">Login and verify hello hostname of hello new VM.</span></span>

```bash
ssh myVM
hostname
myservername
```

## <a name="creating-a-cloud-init-script-tooupdate-linux"></a><span data-ttu-id="4badd-187">A felhő inicializálás parancsfájl tooupdate Linux létrehozása</span><span class="sxs-lookup"><span data-stu-id="4badd-187">Creating a cloud-init script tooupdate Linux</span></span>
<span data-ttu-id="4badd-188">A biztonság érdekében célszerű az Ubuntu virtuális gép tooupdate hello első indításakor.</span><span class="sxs-lookup"><span data-stu-id="4badd-188">For security, you want your Ubuntu VM tooupdate on hello first boot.</span></span>  <span data-ttu-id="4badd-189">Használatával a felhő inicializálás tehetünk ennek, hogy a hello hajtsa végre a parancsfájl hello Linux telepítési módjától függően.</span><span class="sxs-lookup"><span data-stu-id="4badd-189">Using cloud-init we can do that with hello follow script, depending on hello Linux distribution you are using.</span></span>

### <a name="example-cloud-init-script-cloudconfigaptupgradetxt-for-hello-debian-family"></a><span data-ttu-id="4badd-190">Példa felhő inicializálás parancsfájl `cloud_config_apt_upgrade.txt` a hello Debian termékcsalád</span><span class="sxs-lookup"><span data-stu-id="4badd-190">Example cloud-init script `cloud_config_apt_upgrade.txt` for hello Debian Family</span></span>
```sh
#cloud-config
apt_upgrade: true
```

<span data-ttu-id="4badd-191">Miután Linux betöltődött, az összes telepített hello csomagok segítségével frissített `apt-get`.</span><span class="sxs-lookup"><span data-stu-id="4badd-191">After Linux has booted, all hello installed packages are updated via `apt-get`.</span></span>

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

<span data-ttu-id="4badd-192">Bejelentkezés, és ellenőrizze, hogy az összes csomag frissítése.</span><span class="sxs-lookup"><span data-stu-id="4badd-192">Login and verify all packages are updated.</span></span>

```bash
ssh myUbuntuVM
sudo apt-get upgrade
Reading package lists... Done
Building dependency tree
Reading state information... Done
Calculating upgrade... Done
hello following packages have been kept back:
  linux-generic linux-headers-generic linux-image-generic
0 upgraded, 0 newly installed, 0 tooremove and 0 not upgraded.
```

## <a name="creating-a-cloud-init-script-tooadd-a-user-toolinux"></a><span data-ttu-id="4badd-193">A felhő inicializálás parancsfájl tooadd egy felhasználó tooLinux létrehozása</span><span class="sxs-lookup"><span data-stu-id="4badd-193">Creating a cloud-init script tooadd a user tooLinux</span></span>
<span data-ttu-id="4badd-194">Első feladatai hello bármely új Linux virtuális gép egyik tooadd a felhasználó saját kezűleg, illetve tooavoid használata `root`.</span><span class="sxs-lookup"><span data-stu-id="4badd-194">One of hello first tasks on any new Linux VM is tooadd a user for yourself or tooavoid using `root`.</span></span> <span data-ttu-id="4badd-195">SSH kulcsok akkor ajánlott a biztonság és a használhatóság és hozzáadásuk után toohello `~/.ssh/authorized_keys` a felhő inicializálás parancsfájl-fájlt.</span><span class="sxs-lookup"><span data-stu-id="4badd-195">SSH keys are best practice for security and for usability and they are added toohello `~/.ssh/authorized_keys` file with this cloud-init script.</span></span>

### <a name="example-cloud-init-script-cloudconfigadduserstxt-for-debian-family"></a><span data-ttu-id="4badd-196">Példa felhő inicializálás parancsfájl `cloud_config_add_users.txt` Debian termékcsalád</span><span class="sxs-lookup"><span data-stu-id="4badd-196">Example cloud-init script `cloud_config_add_users.txt` for Debian Family</span></span>
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

<span data-ttu-id="4badd-197">Miután Linux betöltődött, senki felsorolt hello a létrehozott és a hozzáadott toohello sudo csoport sem.</span><span class="sxs-lookup"><span data-stu-id="4badd-197">After Linux has booted, all hello listed users are created and added toohello sudo group.</span></span>

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

<span data-ttu-id="4badd-198">Bejelentkezés és ellenőrzésére, hogy az újonnan létrehozott hello felhasználó.</span><span class="sxs-lookup"><span data-stu-id="4badd-198">Login and verify hello newly created user.</span></span>

```bash
ssh myVM
cat /etc/group
```

<span data-ttu-id="4badd-199">Kimenet</span><span class="sxs-lookup"><span data-stu-id="4badd-199">Output</span></span>

```bash
root:x:0:
<snip />
sudo:x:27:myCloudInitAddedAdminUser
<snip />
myCloudInitAddedAdminUser:x:1000:
```

## <a name="next-steps"></a><span data-ttu-id="4badd-200">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="4badd-200">Next Steps</span></span>
<span data-ttu-id="4badd-201">Felhő inicializálás a Linux virtuális gép rendszerindító válik egy szabványos módon toomodify.</span><span class="sxs-lookup"><span data-stu-id="4badd-201">Cloud-init is becoming one standard way toomodify your Linux VM on boot.</span></span> <span data-ttu-id="4badd-202">Azure is rendelkezik Virtuálisgép-bővítmények, amelyek lehetővé teszik toomodify a LinuxVM rendszerindító vagy futtatása során.</span><span class="sxs-lookup"><span data-stu-id="4badd-202">Azure also has VM extensions, which allow you toomodify your LinuxVM on boot or while it is running.</span></span> <span data-ttu-id="4badd-203">Például használható hello Azure VMAccessExtension tooreset SSH vagy felhasználói adatok hello virtuális gép futása közben.</span><span class="sxs-lookup"><span data-stu-id="4badd-203">For example, you can use hello Azure VMAccessExtension tooreset SSH or user information while hello VM is running.</span></span> <span data-ttu-id="4badd-204">A felhő inicializálás újraindítás tooreset hello jelszót kellene.</span><span class="sxs-lookup"><span data-stu-id="4badd-204">With cloud-init, you would need a reboot tooreset hello password.</span></span>

[<span data-ttu-id="4badd-205">Virtuálisgép-bővítmények és szolgáltatásokkal kapcsolatban</span><span class="sxs-lookup"><span data-stu-id="4badd-205">About virtual machine extensions and features</span></span>](../windows/extensions-features.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)

[<span data-ttu-id="4badd-206">Kezelheti a felhasználókat, az SSH és az ellenőrzése vagy javítása lemezek Azure Linux virtuális gépek használatával hello VMAccess bővítmény</span><span class="sxs-lookup"><span data-stu-id="4badd-206">Manage users, SSH, and check or repair disks on Azure Linux VMs using hello VMAccess Extension</span></span>](using-vmaccess-extension.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)

