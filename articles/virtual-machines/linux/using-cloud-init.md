---
title: "Használja a felhő inicializálás Linux virtuális gép testreszabása |} Microsoft Docs"
description: "Felhő inicializálás használata a Linux virtuális gép testreszabása az Azure CLI 2.0 létrehozása során"
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
ms.openlocfilehash: a7a6daad34525683579e25b9591ed28f2bf29c04
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="use-cloud-init-to-customize-a-linux-vm-during-creation"></a><span data-ttu-id="ec561-103">Felhő inicializálás segítségével testre szabhatja a Linux virtuális gép létrehozása során</span><span class="sxs-lookup"><span data-stu-id="ec561-103">Use cloud-init to customize a Linux VM during creation</span></span>
<span data-ttu-id="ec561-104">Ez a cikk bemutatja, hogyan végezheti el a felhő inicializálás parancsfájl az állomásnevet, telepített csomagok frissítése, és az Azure CLI 2.0 felhasználói fiókok kezelése.</span><span class="sxs-lookup"><span data-stu-id="ec561-104">This article shows you how to make a cloud-init script to set the hostname, update installed packages, and manage user accounts with the Azure CLI 2.0.</span></span> <span data-ttu-id="ec561-105">A felhő inicializálás parancsfájlok nevezzük, amikor egy virtuális gép (VM) hoz létre az Azure parancssori felület.</span><span class="sxs-lookup"><span data-stu-id="ec561-105">The cloud-init scripts are called when you create a virtual machine (VM) from Azure CLI.</span></span> <span data-ttu-id="ec561-106">Az alkalmazások részletesebb áttekintése, további konfigurációs fájlok írása, és a Key Vault kulcsok beszúrva: [ebben az oktatóanyagban](tutorial-automate-vm-deployment.md).</span><span class="sxs-lookup"><span data-stu-id="ec561-106">For a more in-depth overview on installing applications, writing configuration files, and injecting keys from Key Vault, see [this tutorial](tutorial-automate-vm-deployment.md).</span></span> <span data-ttu-id="ec561-107">Az [Azure CLI 1.0-s](using-cloud-init-nodejs.md) verziójával is elvégezheti ezeket a lépéseket.</span><span class="sxs-lookup"><span data-stu-id="ec561-107">You can also perform these steps with the [Azure CLI 1.0](using-cloud-init-nodejs.md).</span></span>

## <a name="quick-commands"></a><span data-ttu-id="ec561-108">Gyors parancsok</span><span class="sxs-lookup"><span data-stu-id="ec561-108">Quick commands</span></span>
<span data-ttu-id="ec561-109">Hozzon létre egy felhő-init.txt parancsfájlt, amely beállítja az állomásnevet, frissíti az összes csomagot és sudo felhasználót ad hozzá Linux.</span><span class="sxs-lookup"><span data-stu-id="ec561-109">Create a cloud-init.txt script that sets the hostname, updates all packages, and adds a sudo user to Linux.</span></span>

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

<span data-ttu-id="ec561-110">Hozzon létre egy erőforráscsoportot, elindíthatja a virtuális gépek a [az csoport létrehozása](/cli/azure/group#create).</span><span class="sxs-lookup"><span data-stu-id="ec561-110">Create a resource group to launch VMs into with [az group create](/cli/azure/group#create).</span></span> <span data-ttu-id="ec561-111">Az alábbi példakód létrehozza a erőforráscsoportot *myResourceGroup*:</span><span class="sxs-lookup"><span data-stu-id="ec561-111">The following example creates the resource group named *myResourceGroup*:</span></span>

```azurecli
az group create --name myResourceGroup --location eastus
```

<span data-ttu-id="ec561-112">A Linux virtuális gép létrehozása [az virtuális gép létrehozása](/cli/azure/vm#create) felhő inicializálás segítségével konfigurálja a rendszerindítás során a `--custom-data` paraméter.</span><span class="sxs-lookup"><span data-stu-id="ec561-112">Create a Linux VM with [az vm create](/cli/azure/vm#create) using cloud-init to configure it during boot with the `--custom-data` parameter.</span></span>

```azurecli
az vm create \
    --resource-group myResourceGroup \
    --name myVM \
    --image UbuntuLTS \
    --admin-username azureuser \
    --generate-ssh-keys \
    --custom-data cloud-init.txt
```

## <a name="detailed-walkthrough"></a><span data-ttu-id="ec561-113">Részletes bemutató</span><span class="sxs-lookup"><span data-stu-id="ec561-113">Detailed walkthrough</span></span>
<span data-ttu-id="ec561-114">Amikor új Linux virtuális gép elindításához testreszabott vagy készen áll az Ön igényeinek kap egy szabványos Linux virtuális gép, és semmi ne legyen.</span><span class="sxs-lookup"><span data-stu-id="ec561-114">When you launch a new Linux VM, you are getting a standard Linux VM with nothing customized or ready for your needs.</span></span> <span data-ttu-id="ec561-115">[Felhő inicializálás](https://cloudinit.readthedocs.org) szabványos módja, szúrjon be ezt a Linux virtuális Gépet egy parancsfájl vagy a konfigurációs beállítások, az első alkalommal szolgáltatáshoz indítja.</span><span class="sxs-lookup"><span data-stu-id="ec561-115">[Cloud-init](https://cloudinit.readthedocs.org) is a standard way to inject a script or configuration settings into that Linux VM as it is booting up for the first time.</span></span>

<span data-ttu-id="ec561-116">A Azure-ban többféleképpen néhány módosításokat Linux virtuális gép, mert folyamatban van telepítve, vagy a legközelebbi.</span><span class="sxs-lookup"><span data-stu-id="ec561-116">On Azure, there are a few different ways to make changes onto a Linux VM as it is being deployed or booted.</span></span>

* <span data-ttu-id="ec561-117">Parancsfájlok felhő inicializálás használatával helyezhet el.</span><span class="sxs-lookup"><span data-stu-id="ec561-117">Inject scripts using cloud-init.</span></span>
* <span data-ttu-id="ec561-118">Az Azure használatával parancsfájlok szúrjon [VMAccess bővítmény](using-vmaccess-extension.md).</span><span class="sxs-lookup"><span data-stu-id="ec561-118">Inject scripts using the Azure [VMAccess Extension](using-vmaccess-extension.md).</span></span>
* <span data-ttu-id="ec561-119">Azure-sablon alapján felhő inicializálás használatával.</span><span class="sxs-lookup"><span data-stu-id="ec561-119">An Azure template using cloud-init.</span></span>
* <span data-ttu-id="ec561-120">Egy Azure-sablon használatával [CustomScriptExtention](extensions-customscript.md).</span><span class="sxs-lookup"><span data-stu-id="ec561-120">An Azure template using [CustomScriptExtention](extensions-customscript.md).</span></span>

<span data-ttu-id="ec561-121">A beillesztendő parancsfájlok indítás után bármikor:</span><span class="sxs-lookup"><span data-stu-id="ec561-121">To inject scripts at any time after boot:</span></span>

* <span data-ttu-id="ec561-122">SSH közvetlenül a parancsok futtatásához</span><span class="sxs-lookup"><span data-stu-id="ec561-122">SSH to run commands directly</span></span>
* <span data-ttu-id="ec561-123">Az Azure használatával parancsfájlok szúrjon [VMAccess bővítmény](using-vmaccess-extension.md), imperatively vagy az Azure-sablon alapján</span><span class="sxs-lookup"><span data-stu-id="ec561-123">Inject scripts using the Azure [VMAccess Extension](using-vmaccess-extension.md), either imperatively or in an Azure template</span></span>
* <span data-ttu-id="ec561-124">Konfigurációs felügyeleti eszközök, például Ansible, védőérték, Chef vagy Puppet.</span><span class="sxs-lookup"><span data-stu-id="ec561-124">Configuration management tools like Ansible, Salt, Chef, and Puppet.</span></span>

> [!NOTE]
> <span data-ttu-id="ec561-125">Legfelső szintű SSH használatával ugyanúgy lehet VMAccess bővítmény végrehajtása egy parancsfájlt.</span><span class="sxs-lookup"><span data-stu-id="ec561-125">VMAccess Extension executes a script as root in the same way using SSH can.</span></span> <span data-ttu-id="ec561-126">Azonban a Virtuálisgép-bővítmény használatával lehetővé teszi, hogy számos hasznos a forgatókönyvtől függően az Azure által kínált.</span><span class="sxs-lookup"><span data-stu-id="ec561-126">However, using the VM extension enables several features that Azure offers that can be useful depending upon your scenario.</span></span>

## <a name="cloud-init-availability-on-azure-vm-quick-create-image-aliases"></a><span data-ttu-id="ec561-127">Felhő inicializálás elérhetőségét a Azure virtuális gép gyors-lemezkép aliasok létrehozására:</span><span class="sxs-lookup"><span data-stu-id="ec561-127">Cloud-init availability on Azure VM quick-create image aliases:</span></span>
| <span data-ttu-id="ec561-128">Alias</span><span class="sxs-lookup"><span data-stu-id="ec561-128">Alias</span></span> | <span data-ttu-id="ec561-129">Közzétevő</span><span class="sxs-lookup"><span data-stu-id="ec561-129">Publisher</span></span> | <span data-ttu-id="ec561-130">Ajánlat</span><span class="sxs-lookup"><span data-stu-id="ec561-130">Offer</span></span> | <span data-ttu-id="ec561-131">SKU</span><span class="sxs-lookup"><span data-stu-id="ec561-131">SKU</span></span> | <span data-ttu-id="ec561-132">Verzió</span><span class="sxs-lookup"><span data-stu-id="ec561-132">Version</span></span> | <span data-ttu-id="ec561-133">felhő inicializálás</span><span class="sxs-lookup"><span data-stu-id="ec561-133">cloud-init</span></span> |
|:--- |:--- |:--- |:--- |:--- |:--- |
| <span data-ttu-id="ec561-134">CentOS</span><span class="sxs-lookup"><span data-stu-id="ec561-134">CentOS</span></span> |<span data-ttu-id="ec561-135">OpenLogic</span><span class="sxs-lookup"><span data-stu-id="ec561-135">OpenLogic</span></span> |<span data-ttu-id="ec561-136">Centos</span><span class="sxs-lookup"><span data-stu-id="ec561-136">Centos</span></span> |<span data-ttu-id="ec561-137">7.2</span><span class="sxs-lookup"><span data-stu-id="ec561-137">7.2</span></span> |<span data-ttu-id="ec561-138">legújabb</span><span class="sxs-lookup"><span data-stu-id="ec561-138">latest</span></span> |<span data-ttu-id="ec561-139">nem</span><span class="sxs-lookup"><span data-stu-id="ec561-139">no</span></span> |
| <span data-ttu-id="ec561-140">CoreOS</span><span class="sxs-lookup"><span data-stu-id="ec561-140">CoreOS</span></span> |<span data-ttu-id="ec561-141">CoreOS</span><span class="sxs-lookup"><span data-stu-id="ec561-141">CoreOS</span></span> |<span data-ttu-id="ec561-142">CoreOS</span><span class="sxs-lookup"><span data-stu-id="ec561-142">CoreOS</span></span> |<span data-ttu-id="ec561-143">Stable</span><span class="sxs-lookup"><span data-stu-id="ec561-143">Stable</span></span> |<span data-ttu-id="ec561-144">legújabb</span><span class="sxs-lookup"><span data-stu-id="ec561-144">latest</span></span> |<span data-ttu-id="ec561-145">igen</span><span class="sxs-lookup"><span data-stu-id="ec561-145">yes</span></span> |
| <span data-ttu-id="ec561-146">Debian</span><span class="sxs-lookup"><span data-stu-id="ec561-146">Debian</span></span> |<span data-ttu-id="ec561-147">credativ</span><span class="sxs-lookup"><span data-stu-id="ec561-147">credativ</span></span> |<span data-ttu-id="ec561-148">Debian</span><span class="sxs-lookup"><span data-stu-id="ec561-148">Debian</span></span> |<span data-ttu-id="ec561-149">8</span><span class="sxs-lookup"><span data-stu-id="ec561-149">8</span></span> |<span data-ttu-id="ec561-150">legújabb</span><span class="sxs-lookup"><span data-stu-id="ec561-150">latest</span></span> |<span data-ttu-id="ec561-151">nem</span><span class="sxs-lookup"><span data-stu-id="ec561-151">no</span></span> |
| <span data-ttu-id="ec561-152">openSUSE</span><span class="sxs-lookup"><span data-stu-id="ec561-152">openSUSE</span></span> |<span data-ttu-id="ec561-153">SUSE</span><span class="sxs-lookup"><span data-stu-id="ec561-153">SUSE</span></span> |<span data-ttu-id="ec561-154">openSUSE</span><span class="sxs-lookup"><span data-stu-id="ec561-154">openSUSE</span></span> |<span data-ttu-id="ec561-155">13.2</span><span class="sxs-lookup"><span data-stu-id="ec561-155">13.2</span></span> |<span data-ttu-id="ec561-156">legújabb</span><span class="sxs-lookup"><span data-stu-id="ec561-156">latest</span></span> |<span data-ttu-id="ec561-157">nem</span><span class="sxs-lookup"><span data-stu-id="ec561-157">no</span></span> |
| <span data-ttu-id="ec561-158">RHEL</span><span class="sxs-lookup"><span data-stu-id="ec561-158">RHEL</span></span> |<span data-ttu-id="ec561-159">Redhat</span><span class="sxs-lookup"><span data-stu-id="ec561-159">Redhat</span></span> |<span data-ttu-id="ec561-160">RHEL</span><span class="sxs-lookup"><span data-stu-id="ec561-160">RHEL</span></span> |<span data-ttu-id="ec561-161">7.2</span><span class="sxs-lookup"><span data-stu-id="ec561-161">7.2</span></span> |<span data-ttu-id="ec561-162">legújabb</span><span class="sxs-lookup"><span data-stu-id="ec561-162">latest</span></span> |<span data-ttu-id="ec561-163">nem</span><span class="sxs-lookup"><span data-stu-id="ec561-163">no</span></span> |
| <span data-ttu-id="ec561-164">UbuntuLTS</span><span class="sxs-lookup"><span data-stu-id="ec561-164">UbuntuLTS</span></span> |<span data-ttu-id="ec561-165">Canonical</span><span class="sxs-lookup"><span data-stu-id="ec561-165">Canonical</span></span> |<span data-ttu-id="ec561-166">UbuntuServer</span><span class="sxs-lookup"><span data-stu-id="ec561-166">UbuntuServer</span></span> |<span data-ttu-id="ec561-167">14.04.4-LTS</span><span class="sxs-lookup"><span data-stu-id="ec561-167">14.04.4-LTS</span></span> |<span data-ttu-id="ec561-168">legújabb</span><span class="sxs-lookup"><span data-stu-id="ec561-168">latest</span></span> |<span data-ttu-id="ec561-169">igen</span><span class="sxs-lookup"><span data-stu-id="ec561-169">yes</span></span> |

<span data-ttu-id="ec561-170">A Microsoft a partnerekkel együttműködve az beszerzése felhő inicializálás tartalmazza, és működik-e az Azure biztosít lemezképeket a dolgozik.</span><span class="sxs-lookup"><span data-stu-id="ec561-170">We are working with our partners to get cloud-init included and working in the images that they provide to Azure.</span></span>

## <a name="add-a-cloud-init-script-to-the-vm-creation-with-the-azure-cli"></a><span data-ttu-id="ec561-171">A felhő inicializálás parancsfájl hozzáadása a virtuális gép létrehozása az Azure parancssori felülettel</span><span class="sxs-lookup"><span data-stu-id="ec561-171">Add a cloud-init script to the VM creation with the Azure CLI</span></span>
<span data-ttu-id="ec561-172">Indítsa el a felhő inicializálás parancsfájl, amikor egy virtuális gép létrehozása az Azure-ban, adja meg a felhő inicializálás fájlt az Azure parancssori felület használatával `--custom-data` váltani.</span><span class="sxs-lookup"><span data-stu-id="ec561-172">To launch a cloud-init script when creating a VM in Azure, specify the cloud-init file using the Azure CLI `--custom-data` switch.</span></span>

<span data-ttu-id="ec561-173">Hozzon létre egy erőforráscsoportot, elindíthatja a virtuális gépek a [az csoport létrehozása](/cli/azure/group#create).</span><span class="sxs-lookup"><span data-stu-id="ec561-173">Create a resource group to launch VMs into with [az group create](/cli/azure/group#create).</span></span> <span data-ttu-id="ec561-174">Az alábbi példakód létrehozza a erőforráscsoportot *myResourceGroup*:</span><span class="sxs-lookup"><span data-stu-id="ec561-174">The following example creates the resource group named *myResourceGroup*:</span></span>

```azurecli
az group create --name myResourceGroup --location eastus
```

<span data-ttu-id="ec561-175">A Linux virtuális gép létrehozása [az virtuális gép létrehozása](/cli/azure/vm#create) felhő inicializálás segítségével konfigurálja úgy a rendszerindítás során.</span><span class="sxs-lookup"><span data-stu-id="ec561-175">Create a Linux VM with [az vm create](/cli/azure/vm#create) using cloud-init to configure it during boot.</span></span>

```azurecli
az vm create \
    --resource-group myResourceGroup \
    --name myVM \
    --image UbuntuLTS \
    --admin-username azureuser \
    --generate-ssh-keys \
    --custom-data cloud-init.txt
```

## <a name="create-a-cloud-init-script-to-set-the-hostname-of-a-linux-vm"></a><span data-ttu-id="ec561-176">Hozzon létre egy felhő-init parancsfájlt állítsa be a Linux virtuális gép állomásnevét</span><span class="sxs-lookup"><span data-stu-id="ec561-176">Create a cloud-init script to set the hostname of a Linux VM</span></span>
<span data-ttu-id="ec561-177">A legegyszerűbb és legfontosabb beállításait minden Linux virtuális gép egyik lenne az állomásnevet.</span><span class="sxs-lookup"><span data-stu-id="ec561-177">One of the simplest and most important settings for any Linux VM would be the hostname.</span></span> <span data-ttu-id="ec561-178">A Microsoft könnyedén állíthat be a felhő inicializálás használja ezt a parancsfájlt.</span><span class="sxs-lookup"><span data-stu-id="ec561-178">We can easily set this using cloud-init with this script.</span></span>  

### <a name="example-cloud-init-script-named-cloudconfighostnametxt"></a><span data-ttu-id="ec561-179">Példa felhő inicializálás parancsfájl nevű `cloud_config_hostname.txt`.</span><span class="sxs-lookup"><span data-stu-id="ec561-179">Example cloud-init script named `cloud_config_hostname.txt`.</span></span>
```yaml
#cloud-config
hostname: myservername
```

<span data-ttu-id="ec561-180">A virtuális gép kezdeti indításkor, a felhő inicializálás parancsfájl értékűre állítja be az állomásnév *myservername*.</span><span class="sxs-lookup"><span data-stu-id="ec561-180">During the initial startup of the VM, this cloud-init script sets the hostname to *myservername*.</span></span> <span data-ttu-id="ec561-181">A Linux virtuális gép létrehozása [az virtuális gép létrehozása](/cli/azure/vm#create) felhő inicializálás segítségével konfigurálja úgy a rendszerindítás során.</span><span class="sxs-lookup"><span data-stu-id="ec561-181">Create a Linux VM with [az vm create](/cli/azure/vm#create) using cloud-init to configure it during boot.</span></span>

```azurecli
az vm create \
    --resource-group myResourceGroup \
    --name myVM \
    --image UbuntuLTS \
    --admin-username azureuser \
    --generate-ssh-keys \
    --custom-data cloud-init.txt
```

<span data-ttu-id="ec561-182">Bejelentkezés, és ellenőrizze az új virtuális gép állomásnevét.</span><span class="sxs-lookup"><span data-stu-id="ec561-182">Login and verify the hostname of the new VM.</span></span>

```bash
ssh myVM
hostname
myservername
```

## <a name="create-a-cloud-init-script"></a><span data-ttu-id="ec561-183">Felhő inicializálás parancsfájl létrehozása</span><span class="sxs-lookup"><span data-stu-id="ec561-183">Create a cloud-init script</span></span>
<span data-ttu-id="ec561-184">A biztonság érdekében célszerű az Ubuntu virtuális gép frissítése a első indításakor.</span><span class="sxs-lookup"><span data-stu-id="ec561-184">For security, you want your Ubuntu VM to update on the first boot.</span></span> <span data-ttu-id="ec561-185">Felhő inicializálás is nézzük meg, attól függően, hogy a Linux-disztribúció használ, a kövesse parancsfájl használatával.</span><span class="sxs-lookup"><span data-stu-id="ec561-185">Using cloud-init we can do that with the follow script, depending on the Linux distribution you are using.</span></span>

### <a name="example-cloud-init-script-cloudconfigaptupgradetxt-for-the-debian-family"></a><span data-ttu-id="ec561-186">Példa felhő inicializálás parancsfájl `cloud_config_apt_upgrade.txt` a Debian termékcsalád</span><span class="sxs-lookup"><span data-stu-id="ec561-186">Example cloud-init script `cloud_config_apt_upgrade.txt` for the Debian Family</span></span>
```yaml
#cloud-config
apt_upgrade: true
```

<span data-ttu-id="ec561-187">Miután Linux betöltődött, a telepített csomagok segítségével frissített **apt get**.</span><span class="sxs-lookup"><span data-stu-id="ec561-187">After Linux has booted, all the installed packages are updated via **apt-get**.</span></span> <span data-ttu-id="ec561-188">A Linux virtuális gép létrehozása [az virtuális gép létrehozása](/cli/azure/vm#create) felhő inicializálás segítségével konfigurálja úgy a rendszerindítás során.</span><span class="sxs-lookup"><span data-stu-id="ec561-188">Create a Linux VM with [az vm create](/cli/azure/vm#create) using cloud-init to configure it during boot.</span></span>

```azurecli
az vm create \
    --resource-group myResourceGroup \
    --name myVM \
    --image UbuntuLTS \
    --admin-username azureuser \
    --generate-ssh-keys \
    --custom-data cloud_config_apt_upgrade.txt
```

<span data-ttu-id="ec561-189">Bejelentkezés, és ellenőrizze, hogy az összes csomag frissítése.</span><span class="sxs-lookup"><span data-stu-id="ec561-189">Login and verify all packages are updated.</span></span>

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

## <a name="create-a-cloud-init-script-to-add-a-user-to-linux"></a><span data-ttu-id="ec561-190">A felhasználó hozzáadása Linux felhő inicializálás parancsfájl létrehozása</span><span class="sxs-lookup"><span data-stu-id="ec561-190">Create a cloud-init script to add a user to Linux</span></span>
<span data-ttu-id="ec561-191">Az első feladatok számára bármely új Linux virtuális gép egyik felhasználó hozzáadása a szolgáltatást, vagy kerülje a *legfelső szintű*.</span><span class="sxs-lookup"><span data-stu-id="ec561-191">One of the first tasks on any new Linux VM is to add a user for yourself or to avoid using *root*.</span></span> <span data-ttu-id="ec561-192">SSH kulcs-e a biztonság és a használhatóság ajánlott eljárás, és hozzáadja őket a *~/.ssh/authorized_keys* a felhő inicializálás parancsfájl-fájlt.</span><span class="sxs-lookup"><span data-stu-id="ec561-192">SSH keys are best practice for security and for usability and they are added to the *~/.ssh/authorized_keys* file with this cloud-init script.</span></span>

### <a name="example-cloud-init-script-cloudconfigadduserstxt-for-debian-family"></a><span data-ttu-id="ec561-193">Példa felhő inicializálás parancsfájl `cloud_config_add_users.txt` Debian termékcsalád</span><span class="sxs-lookup"><span data-stu-id="ec561-193">Example cloud-init script `cloud_config_add_users.txt` for Debian Family</span></span>
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

<span data-ttu-id="ec561-194">Miután Linux betöltődött, a felsorolt felhasználók létrehozása és a sudo csoportba felvett.</span><span class="sxs-lookup"><span data-stu-id="ec561-194">After Linux has booted, all the listed users are created and added to the sudo group.</span></span> <span data-ttu-id="ec561-195">A Linux virtuális gép létrehozása [az virtuális gép létrehozása](/cli/azure/vm#create) felhő inicializálás segítségével konfigurálja úgy a rendszerindítás során.</span><span class="sxs-lookup"><span data-stu-id="ec561-195">Create a Linux VM with [az vm create](/cli/azure/vm#create) using cloud-init to configure it during boot.</span></span>

```azurecli
az vm create \
    --resource-group myResourceGroup \
    --name myVM \
    --image UbuntuLTS \
    --admin-username azureuser \
    --generate-ssh-keys \
    --custom-data cloud_config_add_users.txt
```

<span data-ttu-id="ec561-196">Bejelentkezés, és ellenőrizze az újonnan létrehozott felhasználó.</span><span class="sxs-lookup"><span data-stu-id="ec561-196">Login and verify the newly created user.</span></span>

```bash
ssh myVM
cat /etc/group
```

<span data-ttu-id="ec561-197">Kimenet</span><span class="sxs-lookup"><span data-stu-id="ec561-197">Output</span></span>

```bash
root:x:0:
<snip />
sudo:x:27:myCloudInitAddedAdminUser
<snip />
myCloudInitAddedAdminUser:x:1000:
```

## <a name="next-steps"></a><span data-ttu-id="ec561-198">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="ec561-198">Next steps</span></span>
<span data-ttu-id="ec561-199">Felhő inicializálás szabványos úgy lehet módosítani a Linux virtuális gép rendszerindító válik.</span><span class="sxs-lookup"><span data-stu-id="ec561-199">Cloud-init is becoming one standard way to modify your Linux VM on boot.</span></span> <span data-ttu-id="ec561-200">Azure is rendelkezik a Virtuálisgép-bővítmények, amelyek lehetővé teszik a LinuxVM rendszerindító, vagy futás közben módosítani.</span><span class="sxs-lookup"><span data-stu-id="ec561-200">Azure also has VM extensions, which allow you to modify your LinuxVM on boot or while it is running.</span></span> <span data-ttu-id="ec561-201">Például használhatja az Azure VMAccessExtension SSH vagy felhasználói adatok visszaállítására, a virtuális gép futása közben.</span><span class="sxs-lookup"><span data-stu-id="ec561-201">For example, you can use the Azure VMAccessExtension to reset SSH or user information while the VM is running.</span></span> <span data-ttu-id="ec561-202">A felhő inicializálás a jelszó alaphelyzetbe állítása újraindítás kellene.</span><span class="sxs-lookup"><span data-stu-id="ec561-202">With cloud-init, you would need a reboot to reset the password.</span></span>

[<span data-ttu-id="ec561-203">Virtuálisgép-bővítmények és szolgáltatásokkal kapcsolatban</span><span class="sxs-lookup"><span data-stu-id="ec561-203">About virtual machine extensions and features</span></span>](extensions-features.md)

[<span data-ttu-id="ec561-204">Kezelheti a felhasználókat, az SSH és az ellenőrzése vagy javítása Azure virtuális gépeken Linux a VMAccess bővítmény használatával lemezek</span><span class="sxs-lookup"><span data-stu-id="ec561-204">Manage users, SSH, and check or repair disks on Azure Linux VMs using the VMAccess Extension</span></span>](using-vmaccess-extension.md)