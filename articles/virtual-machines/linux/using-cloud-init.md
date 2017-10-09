---
title: "aaaUse felhő inicializálás toocustomize Linux virtuális gép |} Microsoft Docs"
description: "Hogyan toouse felhő inicializálás toocustomize a Linux virtuális gép létrehozása során hello Azure CLI 2.0"
services: virtual-machines-linux
documentationcenter: 
author: iainfoulds
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: 195c22cd-4629-4582-9ee3-9749493f1d72
ms.service: virtual-machines-linux
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-linux
ms.devlang: azurecli
ms.topic: article
ms.date: 05/11/2017
ms.author: iainfou
ms.openlocfilehash: e7297e162fc73f0da42f195bec2fcbe23b310c1e
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="use-cloud-init-toocustomize-a-linux-vm-during-creation"></a><span data-ttu-id="4ec72-103">Használja a felhő inicializálás toocustomize Linux virtuális gép létrehozása során</span><span class="sxs-lookup"><span data-stu-id="4ec72-103">Use cloud-init toocustomize a Linux VM during creation</span></span>
<span data-ttu-id="4ec72-104">Ez a cikk bemutatja, hogyan toomake egy felhő-init parancsfájl tooset állomásnév hello telepített csomagok frissítése és az Azure CLI 2.0 hello felhasználói fiókok kezelése.</span><span class="sxs-lookup"><span data-stu-id="4ec72-104">This article shows you how toomake a cloud-init script tooset hello hostname, update installed packages, and manage user accounts with hello Azure CLI 2.0.</span></span> <span data-ttu-id="4ec72-105">hello felhő inicializálás parancsfájlok nevezzük, amikor egy virtuális gép (VM) hoz létre az Azure parancssori felület.</span><span class="sxs-lookup"><span data-stu-id="4ec72-105">hello cloud-init scripts are called when you create a virtual machine (VM) from Azure CLI.</span></span> <span data-ttu-id="4ec72-106">Az alkalmazások részletesebb áttekintése, további konfigurációs fájlok írása, és a Key Vault kulcsok beszúrva: [ebben az oktatóanyagban](tutorial-automate-vm-deployment.md).</span><span class="sxs-lookup"><span data-stu-id="4ec72-106">For a more in-depth overview on installing applications, writing configuration files, and injecting keys from Key Vault, see [this tutorial](tutorial-automate-vm-deployment.md).</span></span> <span data-ttu-id="4ec72-107">Is elvégezheti ezeket a lépéseket hello [Azure CLI 1.0](using-cloud-init-nodejs.md).</span><span class="sxs-lookup"><span data-stu-id="4ec72-107">You can also perform these steps with hello [Azure CLI 1.0](using-cloud-init-nodejs.md).</span></span>

## <a name="quick-commands"></a><span data-ttu-id="4ec72-108">Gyors parancsok</span><span class="sxs-lookup"><span data-stu-id="4ec72-108">Quick commands</span></span>
<span data-ttu-id="4ec72-109">Hozzon létre egy felhő-init.txt parancsfájlt, amely beállítja a hello állomásnév, frissíti az összes csomag, a sudo felhasználói tooLinux hozzáadja.</span><span class="sxs-lookup"><span data-stu-id="4ec72-109">Create a cloud-init.txt script that sets hello hostname, updates all packages, and adds a sudo user tooLinux.</span></span>

```yaml
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

<span data-ttu-id="4ec72-110">Hozzon létre egy erőforrás csoport toolaunch történő rendelkező virtuális gépek [az csoport létrehozása](/cli/azure/group#create).</span><span class="sxs-lookup"><span data-stu-id="4ec72-110">Create a resource group toolaunch VMs into with [az group create](/cli/azure/group#create).</span></span> <span data-ttu-id="4ec72-111">hello alábbi példakód létrehozza hello erőforráscsoportot *myResourceGroup*:</span><span class="sxs-lookup"><span data-stu-id="4ec72-111">hello following example creates hello resource group named *myResourceGroup*:</span></span>

```azurecli
az group create --name myResourceGroup --location eastus
```

<span data-ttu-id="4ec72-112">A Linux virtuális gép létrehozása [az virtuális gép létrehozása](/cli/azure/vm#create) használatával a felhő inicializálás tooconfigure hello rendszerindítás során `--custom-data` paraméter.</span><span class="sxs-lookup"><span data-stu-id="4ec72-112">Create a Linux VM with [az vm create](/cli/azure/vm#create) using cloud-init tooconfigure it during boot with hello `--custom-data` parameter.</span></span>

```azurecli
az vm create \
    --resource-group myResourceGroup \
    --name myVM \
    --image UbuntuLTS \
    --admin-username azureuser \
    --generate-ssh-keys \
    --custom-data cloud-init.txt
```

## <a name="detailed-walkthrough"></a><span data-ttu-id="4ec72-113">Részletes bemutató</span><span class="sxs-lookup"><span data-stu-id="4ec72-113">Detailed walkthrough</span></span>
<span data-ttu-id="4ec72-114">Amikor új Linux virtuális gép elindításához testreszabott vagy készen áll az Ön igényeinek kap egy szabványos Linux virtuális gép, és semmi ne legyen.</span><span class="sxs-lookup"><span data-stu-id="4ec72-114">When you launch a new Linux VM, you are getting a standard Linux VM with nothing customized or ready for your needs.</span></span> <span data-ttu-id="4ec72-115">[Felhő inicializálás](https://cloudinit.readthedocs.org) megegyezik a szokásos módon tooinject egy parancsfájl vagy a konfigurációs beállításokat a Linux virtuális gép akkor indítja az hello szolgáltatáshoz első alkalommal.</span><span class="sxs-lookup"><span data-stu-id="4ec72-115">[Cloud-init](https://cloudinit.readthedocs.org) is a standard way tooinject a script or configuration settings into that Linux VM as it is booting up for hello first time.</span></span>

<span data-ttu-id="4ec72-116">Azure, amelyeket többféleképpen toomake módosítások alakzatot Linux virtuális gép folyamatban van telepítve, vagy a legközelebbi.</span><span class="sxs-lookup"><span data-stu-id="4ec72-116">On Azure, there are a few different ways toomake changes onto a Linux VM as it is being deployed or booted.</span></span>

* <span data-ttu-id="4ec72-117">Parancsfájlok felhő inicializálás használatával helyezhet el.</span><span class="sxs-lookup"><span data-stu-id="4ec72-117">Inject scripts using cloud-init.</span></span>
* <span data-ttu-id="4ec72-118">Parancsfájlok hello Azure használatával szúrjon [VMAccess bővítmény](using-vmaccess-extension.md).</span><span class="sxs-lookup"><span data-stu-id="4ec72-118">Inject scripts using hello Azure [VMAccess Extension](using-vmaccess-extension.md).</span></span>
* <span data-ttu-id="4ec72-119">Azure-sablon alapján felhő inicializálás használatával.</span><span class="sxs-lookup"><span data-stu-id="4ec72-119">An Azure template using cloud-init.</span></span>
* <span data-ttu-id="4ec72-120">Egy Azure-sablon használatával [CustomScriptExtention](extensions-customscript.md).</span><span class="sxs-lookup"><span data-stu-id="4ec72-120">An Azure template using [CustomScriptExtention](extensions-customscript.md).</span></span>

<span data-ttu-id="4ec72-121">indítás után bármikor parancsfájlok tooinject:</span><span class="sxs-lookup"><span data-stu-id="4ec72-121">tooinject scripts at any time after boot:</span></span>

* <span data-ttu-id="4ec72-122">SSH toorun közvetlenül parancsok</span><span class="sxs-lookup"><span data-stu-id="4ec72-122">SSH toorun commands directly</span></span>
* <span data-ttu-id="4ec72-123">Parancsfájlok hello Azure használatával szúrjon [VMAccess bővítmény](using-vmaccess-extension.md), imperatively vagy az Azure-sablon alapján</span><span class="sxs-lookup"><span data-stu-id="4ec72-123">Inject scripts using hello Azure [VMAccess Extension](using-vmaccess-extension.md), either imperatively or in an Azure template</span></span>
* <span data-ttu-id="4ec72-124">Konfigurációs felügyeleti eszközök, például Ansible, védőérték, Chef vagy Puppet.</span><span class="sxs-lookup"><span data-stu-id="4ec72-124">Configuration management tools like Ansible, Salt, Chef, and Puppet.</span></span>

> [!NOTE]
> <span data-ttu-id="4ec72-125">Végrehajtja a VMAccess bővítmény hello a legfelső szintű azonos parancsfájl tudja módon az SSH használatával.</span><span class="sxs-lookup"><span data-stu-id="4ec72-125">VMAccess Extension executes a script as root in hello same way using SSH can.</span></span> <span data-ttu-id="4ec72-126">Azonban hello Virtuálisgép-bővítmény használatával lehetővé teszi, hogy számos hasznos a forgatókönyvtől függően az Azure által kínált.</span><span class="sxs-lookup"><span data-stu-id="4ec72-126">However, using hello VM extension enables several features that Azure offers that can be useful depending upon your scenario.</span></span>

## <a name="cloud-init-availability-on-azure-vm-quick-create-image-aliases"></a><span data-ttu-id="4ec72-127">Felhő inicializálás elérhetőségét a Azure virtuális gép gyors-lemezkép aliasok létrehozására:</span><span class="sxs-lookup"><span data-stu-id="4ec72-127">Cloud-init availability on Azure VM quick-create image aliases:</span></span>
| <span data-ttu-id="4ec72-128">Alias</span><span class="sxs-lookup"><span data-stu-id="4ec72-128">Alias</span></span> | <span data-ttu-id="4ec72-129">Közzétevő</span><span class="sxs-lookup"><span data-stu-id="4ec72-129">Publisher</span></span> | <span data-ttu-id="4ec72-130">Ajánlat</span><span class="sxs-lookup"><span data-stu-id="4ec72-130">Offer</span></span> | <span data-ttu-id="4ec72-131">SKU</span><span class="sxs-lookup"><span data-stu-id="4ec72-131">SKU</span></span> | <span data-ttu-id="4ec72-132">Verzió</span><span class="sxs-lookup"><span data-stu-id="4ec72-132">Version</span></span> | <span data-ttu-id="4ec72-133">felhő inicializálás</span><span class="sxs-lookup"><span data-stu-id="4ec72-133">cloud-init</span></span> |
|:--- |:--- |:--- |:--- |:--- |:--- |
| <span data-ttu-id="4ec72-134">CentOS</span><span class="sxs-lookup"><span data-stu-id="4ec72-134">CentOS</span></span> |<span data-ttu-id="4ec72-135">OpenLogic</span><span class="sxs-lookup"><span data-stu-id="4ec72-135">OpenLogic</span></span> |<span data-ttu-id="4ec72-136">Centos</span><span class="sxs-lookup"><span data-stu-id="4ec72-136">Centos</span></span> |<span data-ttu-id="4ec72-137">7.2</span><span class="sxs-lookup"><span data-stu-id="4ec72-137">7.2</span></span> |<span data-ttu-id="4ec72-138">legújabb</span><span class="sxs-lookup"><span data-stu-id="4ec72-138">latest</span></span> |<span data-ttu-id="4ec72-139">nem</span><span class="sxs-lookup"><span data-stu-id="4ec72-139">no</span></span> |
| <span data-ttu-id="4ec72-140">CoreOS</span><span class="sxs-lookup"><span data-stu-id="4ec72-140">CoreOS</span></span> |<span data-ttu-id="4ec72-141">CoreOS</span><span class="sxs-lookup"><span data-stu-id="4ec72-141">CoreOS</span></span> |<span data-ttu-id="4ec72-142">CoreOS</span><span class="sxs-lookup"><span data-stu-id="4ec72-142">CoreOS</span></span> |<span data-ttu-id="4ec72-143">Stable</span><span class="sxs-lookup"><span data-stu-id="4ec72-143">Stable</span></span> |<span data-ttu-id="4ec72-144">legújabb</span><span class="sxs-lookup"><span data-stu-id="4ec72-144">latest</span></span> |<span data-ttu-id="4ec72-145">igen</span><span class="sxs-lookup"><span data-stu-id="4ec72-145">yes</span></span> |
| <span data-ttu-id="4ec72-146">Debian</span><span class="sxs-lookup"><span data-stu-id="4ec72-146">Debian</span></span> |<span data-ttu-id="4ec72-147">credativ</span><span class="sxs-lookup"><span data-stu-id="4ec72-147">credativ</span></span> |<span data-ttu-id="4ec72-148">Debian</span><span class="sxs-lookup"><span data-stu-id="4ec72-148">Debian</span></span> |<span data-ttu-id="4ec72-149">8</span><span class="sxs-lookup"><span data-stu-id="4ec72-149">8</span></span> |<span data-ttu-id="4ec72-150">legújabb</span><span class="sxs-lookup"><span data-stu-id="4ec72-150">latest</span></span> |<span data-ttu-id="4ec72-151">nem</span><span class="sxs-lookup"><span data-stu-id="4ec72-151">no</span></span> |
| <span data-ttu-id="4ec72-152">openSUSE</span><span class="sxs-lookup"><span data-stu-id="4ec72-152">openSUSE</span></span> |<span data-ttu-id="4ec72-153">SUSE</span><span class="sxs-lookup"><span data-stu-id="4ec72-153">SUSE</span></span> |<span data-ttu-id="4ec72-154">openSUSE</span><span class="sxs-lookup"><span data-stu-id="4ec72-154">openSUSE</span></span> |<span data-ttu-id="4ec72-155">13.2</span><span class="sxs-lookup"><span data-stu-id="4ec72-155">13.2</span></span> |<span data-ttu-id="4ec72-156">legújabb</span><span class="sxs-lookup"><span data-stu-id="4ec72-156">latest</span></span> |<span data-ttu-id="4ec72-157">nem</span><span class="sxs-lookup"><span data-stu-id="4ec72-157">no</span></span> |
| <span data-ttu-id="4ec72-158">RHEL</span><span class="sxs-lookup"><span data-stu-id="4ec72-158">RHEL</span></span> |<span data-ttu-id="4ec72-159">Redhat</span><span class="sxs-lookup"><span data-stu-id="4ec72-159">Redhat</span></span> |<span data-ttu-id="4ec72-160">RHEL</span><span class="sxs-lookup"><span data-stu-id="4ec72-160">RHEL</span></span> |<span data-ttu-id="4ec72-161">7.2</span><span class="sxs-lookup"><span data-stu-id="4ec72-161">7.2</span></span> |<span data-ttu-id="4ec72-162">legújabb</span><span class="sxs-lookup"><span data-stu-id="4ec72-162">latest</span></span> |<span data-ttu-id="4ec72-163">nem</span><span class="sxs-lookup"><span data-stu-id="4ec72-163">no</span></span> |
| <span data-ttu-id="4ec72-164">UbuntuLTS</span><span class="sxs-lookup"><span data-stu-id="4ec72-164">UbuntuLTS</span></span> |<span data-ttu-id="4ec72-165">Canonical</span><span class="sxs-lookup"><span data-stu-id="4ec72-165">Canonical</span></span> |<span data-ttu-id="4ec72-166">UbuntuServer</span><span class="sxs-lookup"><span data-stu-id="4ec72-166">UbuntuServer</span></span> |<span data-ttu-id="4ec72-167">14.04.4-LTS</span><span class="sxs-lookup"><span data-stu-id="4ec72-167">14.04.4-LTS</span></span> |<span data-ttu-id="4ec72-168">legújabb</span><span class="sxs-lookup"><span data-stu-id="4ec72-168">latest</span></span> |<span data-ttu-id="4ec72-169">igen</span><span class="sxs-lookup"><span data-stu-id="4ec72-169">yes</span></span> |

<span data-ttu-id="4ec72-170">Folyamatban, a partnerek tooget felhő inicializálás része azokkal, valamint, hogy ezek biztosítanak tooAzure hello képek működik.</span><span class="sxs-lookup"><span data-stu-id="4ec72-170">We are working with our partners tooget cloud-init included and working in hello images that they provide tooAzure.</span></span>

## <a name="add-a-cloud-init-script-toohello-vm-creation-with-hello-azure-cli"></a><span data-ttu-id="4ec72-171">Egy felhő inicializálás parancsfájl toohello virtuális gép létrehozása az Azure CLI hello hozzáadása</span><span class="sxs-lookup"><span data-stu-id="4ec72-171">Add a cloud-init script toohello VM creation with hello Azure CLI</span></span>
<span data-ttu-id="4ec72-172">toolaunch felhő inicializálás parancsfájl az Azure virtuális gép létrehozásakor adja meg a hello felhő inicializálás fájlt hello Azure parancssori felület használatával `--custom-data` váltani.</span><span class="sxs-lookup"><span data-stu-id="4ec72-172">toolaunch a cloud-init script when creating a VM in Azure, specify hello cloud-init file using hello Azure CLI `--custom-data` switch.</span></span>

<span data-ttu-id="4ec72-173">Hozzon létre egy erőforrás csoport toolaunch történő rendelkező virtuális gépek [az csoport létrehozása](/cli/azure/group#create).</span><span class="sxs-lookup"><span data-stu-id="4ec72-173">Create a resource group toolaunch VMs into with [az group create](/cli/azure/group#create).</span></span> <span data-ttu-id="4ec72-174">hello alábbi példakód létrehozza hello erőforráscsoportot *myResourceGroup*:</span><span class="sxs-lookup"><span data-stu-id="4ec72-174">hello following example creates hello resource group named *myResourceGroup*:</span></span>

```azurecli
az group create --name myResourceGroup --location eastus
```

<span data-ttu-id="4ec72-175">A Linux virtuális gép létrehozása [az virtuális gép létrehozása](/cli/azure/vm#create) felhő inicializálás tooconfigure használatával, a rendszerindítás során.</span><span class="sxs-lookup"><span data-stu-id="4ec72-175">Create a Linux VM with [az vm create](/cli/azure/vm#create) using cloud-init tooconfigure it during boot.</span></span>

```azurecli
az vm create \
    --resource-group myResourceGroup \
    --name myVM \
    --image UbuntuLTS \
    --admin-username azureuser \
    --generate-ssh-keys \
    --custom-data cloud-init.txt
```

## <a name="create-a-cloud-init-script-tooset-hello-hostname-of-a-linux-vm"></a><span data-ttu-id="4ec72-176">A felhő inicializálás parancsfájl tooset hello állomásnevét Linux virtuális gép létrehozása</span><span class="sxs-lookup"><span data-stu-id="4ec72-176">Create a cloud-init script tooset hello hostname of a Linux VM</span></span>
<span data-ttu-id="4ec72-177">Egyik legegyszerűbb hello és a Linux virtuális gép számára legfontosabb beállítások hello állomásnév lenne.</span><span class="sxs-lookup"><span data-stu-id="4ec72-177">One of hello simplest and most important settings for any Linux VM would be hello hostname.</span></span> <span data-ttu-id="4ec72-178">A Microsoft könnyedén állíthat be a felhő inicializálás használja ezt a parancsfájlt.</span><span class="sxs-lookup"><span data-stu-id="4ec72-178">We can easily set this using cloud-init with this script.</span></span>  

### <a name="example-cloud-init-script-named-cloudconfighostnametxt"></a><span data-ttu-id="4ec72-179">Példa felhő inicializálás parancsfájl nevű `cloud_config_hostname.txt`.</span><span class="sxs-lookup"><span data-stu-id="4ec72-179">Example cloud-init script named `cloud_config_hostname.txt`.</span></span>
```yaml
#cloud-config
hostname: myservername
```

<span data-ttu-id="4ec72-180">Hello virtuális gép kezdeti indításkor hello, a felhő inicializálás parancsprogram beállítja hello állomásnév túl*myservername*.</span><span class="sxs-lookup"><span data-stu-id="4ec72-180">During hello initial startup of hello VM, this cloud-init script sets hello hostname too*myservername*.</span></span> <span data-ttu-id="4ec72-181">A Linux virtuális gép létrehozása [az virtuális gép létrehozása](/cli/azure/vm#create) felhő inicializálás tooconfigure használatával, a rendszerindítás során.</span><span class="sxs-lookup"><span data-stu-id="4ec72-181">Create a Linux VM with [az vm create](/cli/azure/vm#create) using cloud-init tooconfigure it during boot.</span></span>

```azurecli
az vm create \
    --resource-group myResourceGroup \
    --name myVM \
    --image UbuntuLTS \
    --admin-username azureuser \
    --generate-ssh-keys \
    --custom-data cloud-init.txt
```

<span data-ttu-id="4ec72-182">Bejelentkezés, és ellenőrizze a hello hello állomásnevét új virtuális Gépet.</span><span class="sxs-lookup"><span data-stu-id="4ec72-182">Login and verify hello hostname of hello new VM.</span></span>

```bash
ssh myVM
hostname
myservername
```

## <a name="create-a-cloud-init-script"></a><span data-ttu-id="4ec72-183">Felhő inicializálás parancsfájl létrehozása</span><span class="sxs-lookup"><span data-stu-id="4ec72-183">Create a cloud-init script</span></span>
<span data-ttu-id="4ec72-184">A biztonság érdekében célszerű az Ubuntu virtuális gép tooupdate hello első indításakor.</span><span class="sxs-lookup"><span data-stu-id="4ec72-184">For security, you want your Ubuntu VM tooupdate on hello first boot.</span></span> <span data-ttu-id="4ec72-185">Használatával a felhő inicializálás tehetünk ennek, hogy a hello hajtsa végre a parancsfájl hello Linux telepítési módjától függően.</span><span class="sxs-lookup"><span data-stu-id="4ec72-185">Using cloud-init we can do that with hello follow script, depending on hello Linux distribution you are using.</span></span>

### <a name="example-cloud-init-script-cloudconfigaptupgradetxt-for-hello-debian-family"></a><span data-ttu-id="4ec72-186">Példa felhő inicializálás parancsfájl `cloud_config_apt_upgrade.txt` a hello Debian termékcsalád</span><span class="sxs-lookup"><span data-stu-id="4ec72-186">Example cloud-init script `cloud_config_apt_upgrade.txt` for hello Debian Family</span></span>
```yaml
#cloud-config
apt_upgrade: true
```

<span data-ttu-id="4ec72-187">Miután Linux betöltődött, az összes telepített hello csomagok segítségével frissített **apt get**.</span><span class="sxs-lookup"><span data-stu-id="4ec72-187">After Linux has booted, all hello installed packages are updated via **apt-get**.</span></span> <span data-ttu-id="4ec72-188">A Linux virtuális gép létrehozása [az virtuális gép létrehozása](/cli/azure/vm#create) felhő inicializálás tooconfigure használatával, a rendszerindítás során.</span><span class="sxs-lookup"><span data-stu-id="4ec72-188">Create a Linux VM with [az vm create](/cli/azure/vm#create) using cloud-init tooconfigure it during boot.</span></span>

```azurecli
az vm create \
    --resource-group myResourceGroup \
    --name myVM \
    --image UbuntuLTS \
    --admin-username azureuser \
    --generate-ssh-keys \
    --custom-data cloud_config_apt_upgrade.txt
```

<span data-ttu-id="4ec72-189">Bejelentkezés, és ellenőrizze, hogy az összes csomag frissítése.</span><span class="sxs-lookup"><span data-stu-id="4ec72-189">Login and verify all packages are updated.</span></span>

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

## <a name="create-a-cloud-init-script-tooadd-a-user-toolinux"></a><span data-ttu-id="4ec72-190">Hozzon létre egy felhő-init parancsfájl tooadd egy felhasználó tooLinux</span><span class="sxs-lookup"><span data-stu-id="4ec72-190">Create a cloud-init script tooadd a user tooLinux</span></span>
<span data-ttu-id="4ec72-191">Első feladatai hello bármely új Linux virtuális gép egyik tooadd a felhasználó saját kezűleg, illetve tooavoid használata *legfelső szintű*.</span><span class="sxs-lookup"><span data-stu-id="4ec72-191">One of hello first tasks on any new Linux VM is tooadd a user for yourself or tooavoid using *root*.</span></span> <span data-ttu-id="4ec72-192">SSH kulcsok akkor ajánlott a biztonság és a használhatóság és hozzáadásuk után toohello *~/.ssh/authorized_keys* a felhő inicializálás parancsfájl-fájlt.</span><span class="sxs-lookup"><span data-stu-id="4ec72-192">SSH keys are best practice for security and for usability and they are added toohello *~/.ssh/authorized_keys* file with this cloud-init script.</span></span>

### <a name="example-cloud-init-script-cloudconfigadduserstxt-for-debian-family"></a><span data-ttu-id="4ec72-193">Példa felhő inicializálás parancsfájl `cloud_config_add_users.txt` Debian termékcsalád</span><span class="sxs-lookup"><span data-stu-id="4ec72-193">Example cloud-init script `cloud_config_add_users.txt` for Debian Family</span></span>
```yaml
#cloud-config
users:
  - name: myCloudInitAddedAdminUser
    groups: sudo
    shell: /bin/bash
    sudo: ['ALL=(ALL) NOPASSWD:ALL']
    ssh-authorized-keys:
      - ssh-rsa AAAAB3<snip>==myAdminUser@myUbuntuVM
```

<span data-ttu-id="4ec72-194">Miután Linux betöltődött, senki felsorolt hello a létrehozott és a hozzáadott toohello sudo csoport sem.</span><span class="sxs-lookup"><span data-stu-id="4ec72-194">After Linux has booted, all hello listed users are created and added toohello sudo group.</span></span> <span data-ttu-id="4ec72-195">A Linux virtuális gép létrehozása [az virtuális gép létrehozása](/cli/azure/vm#create) felhő inicializálás tooconfigure használatával, a rendszerindítás során.</span><span class="sxs-lookup"><span data-stu-id="4ec72-195">Create a Linux VM with [az vm create](/cli/azure/vm#create) using cloud-init tooconfigure it during boot.</span></span>

```azurecli
az vm create \
    --resource-group myResourceGroup \
    --name myVM \
    --image UbuntuLTS \
    --admin-username azureuser \
    --generate-ssh-keys \
    --custom-data cloud_config_add_users.txt
```

<span data-ttu-id="4ec72-196">Bejelentkezés és ellenőrzésére, hogy az újonnan létrehozott hello felhasználó.</span><span class="sxs-lookup"><span data-stu-id="4ec72-196">Login and verify hello newly created user.</span></span>

```bash
ssh myVM
cat /etc/group
```

<span data-ttu-id="4ec72-197">Kimenet</span><span class="sxs-lookup"><span data-stu-id="4ec72-197">Output</span></span>

```bash
root:x:0:
<snip />
sudo:x:27:myCloudInitAddedAdminUser
<snip />
myCloudInitAddedAdminUser:x:1000:
```

## <a name="next-steps"></a><span data-ttu-id="4ec72-198">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="4ec72-198">Next steps</span></span>
<span data-ttu-id="4ec72-199">Felhő inicializálás a Linux virtuális gép rendszerindító válik egy szabványos módon toomodify.</span><span class="sxs-lookup"><span data-stu-id="4ec72-199">Cloud-init is becoming one standard way toomodify your Linux VM on boot.</span></span> <span data-ttu-id="4ec72-200">Azure is rendelkezik Virtuálisgép-bővítmények, amelyek lehetővé teszik toomodify a LinuxVM rendszerindító vagy futtatása során.</span><span class="sxs-lookup"><span data-stu-id="4ec72-200">Azure also has VM extensions, which allow you toomodify your LinuxVM on boot or while it is running.</span></span> <span data-ttu-id="4ec72-201">Például használható hello Azure VMAccessExtension tooreset SSH vagy felhasználói adatok hello virtuális gép futása közben.</span><span class="sxs-lookup"><span data-stu-id="4ec72-201">For example, you can use hello Azure VMAccessExtension tooreset SSH or user information while hello VM is running.</span></span> <span data-ttu-id="4ec72-202">A felhő inicializálás újraindítás tooreset hello jelszót kellene.</span><span class="sxs-lookup"><span data-stu-id="4ec72-202">With cloud-init, you would need a reboot tooreset hello password.</span></span>

[<span data-ttu-id="4ec72-203">Virtuálisgép-bővítmények és szolgáltatásokkal kapcsolatban</span><span class="sxs-lookup"><span data-stu-id="4ec72-203">About virtual machine extensions and features</span></span>](extensions-features.md)

[<span data-ttu-id="4ec72-204">Kezelheti a felhasználókat, az SSH és az ellenőrzése vagy javítása lemezek Azure Linux virtuális gépek használatával hello VMAccess bővítmény</span><span class="sxs-lookup"><span data-stu-id="4ec72-204">Manage users, SSH, and check or repair disks on Azure Linux VMs using hello VMAccess Extension</span></span>](using-vmaccess-extension.md)
