---
title: "Hozzáférés az Azure Linux virtuális gép visszaállítása |} Microsoft Docs"
description: "Hogyan kezelheti a felhasználók, és alaphelyzetbe állítja a hozzáférés a Linux virtuális gépeken a VMAccess bővítmény és az Azure CLI 2.0 használatával"
services: virtual-machines-linux
documentationcenter: 
author: dlepow
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: 261a9646-1f93-407e-951e-0be7226b3064
ms.service: virtual-machines-linux
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-linux
ms.devlang: azurecli
ms.topic: article
ms.date: 08/04/2017
ms.author: danlep
ms.openlocfilehash: 587c73278a9a92776276a811c5c4c8d3db773de3
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/18/2017
---
# <a name="manage-users-ssh-and-check-or-repair-disks-on-linux-vms-using-the-vmaccess-extension-with-the-azure-cli-20"></a><span data-ttu-id="8e113-103">Kezelheti a felhasználókat, az SSH és az ellenőrzése, vagy javítsa ki a Linux virtuális gépeken a VMAccess bővítmény használata az Azure CLI 2.0 lemezek</span><span class="sxs-lookup"><span data-stu-id="8e113-103">Manage users, SSH, and check or repair disks on Linux VMs using the VMAccess Extension with the Azure CLI 2.0</span></span>
<span data-ttu-id="8e113-104">A lemezt a Linux virtuális Gépet a hibák láthatók.</span><span class="sxs-lookup"><span data-stu-id="8e113-104">The disk on your Linux VM is showing errors.</span></span> <span data-ttu-id="8e113-105">Valamilyen módon alaphelyzetbe állítja a gyökér szintű jelszavát a Linux virtuális gép számára, vagy véletlenül törli a titkos SSH-kulcsot.</span><span class="sxs-lookup"><span data-stu-id="8e113-105">You somehow reset the root password for your Linux VM or accidentally deleted your SSH private key.</span></span> <span data-ttu-id="8e113-106">Ha vissza a datacenter napban bekövetkezett, meg kell meghajtó van, és nyissa meg a kiszolgáló konzolján beolvasandó KVM.</span><span class="sxs-lookup"><span data-stu-id="8e113-106">If that happened back in the days of the datacenter, you would need to drive there and then open the KVM to get at the server console.</span></span> <span data-ttu-id="8e113-107">Az Azure VMAccess bővítmény gondol adott KVM kapcsolóéval, amely lehetővé teszi a hozzáférést a következőre Linux, vagy végezzen szintű konzol eléréséhez.</span><span class="sxs-lookup"><span data-stu-id="8e113-107">Think of the Azure VMAccess extension as that KVM switch that allows you to access the console to reset access to Linux or perform disk level maintenance.</span></span>

<span data-ttu-id="8e113-108">Ez a cikk bemutatja, hogyan ellenőrizze vagy javítsa ki a lemezt, alaphelyzetbe állítja a felhasználói hozzáférés, a felhasználói fiókok kezelése vagy a Linux SSH-konfigurációjának visszaállítása az Azure VMAccess bővítmény használatával.</span><span class="sxs-lookup"><span data-stu-id="8e113-108">This article shows you how to use the Azure VMAccess Extension to check or repair a disk, reset user access, manage user accounts, or reset the SSH configuration on Linux.</span></span> <span data-ttu-id="8e113-109">Az [Azure CLI 1.0-s](using-vmaccess-extension-nodejs.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) verziójával is elvégezheti ezeket a lépéseket.</span><span class="sxs-lookup"><span data-stu-id="8e113-109">You can also perform these steps with the [Azure CLI 1.0](using-vmaccess-extension-nodejs.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span></span>


## <a name="ways-to-use-the-vmaccess-extension"></a><span data-ttu-id="8e113-110">A VMAccess bővítmény használatának módjai</span><span class="sxs-lookup"><span data-stu-id="8e113-110">Ways to use the VMAccess Extension</span></span>
<span data-ttu-id="8e113-111">Kétféleképpen használható a VMAccess bővítmény a Linux virtuális gépeken:</span><span class="sxs-lookup"><span data-stu-id="8e113-111">There are two ways that you can use the VMAccess Extension on your Linux VMs:</span></span>

* <span data-ttu-id="8e113-112">Használja az Azure CLI 2.0 és a szükséges paramétereket.</span><span class="sxs-lookup"><span data-stu-id="8e113-112">Use the Azure CLI 2.0 and the required parameters.</span></span>
* <span data-ttu-id="8e113-113">[Használja a VMAccess bővítmény feldolgozó nyers JSON-fájlok](#use-json-files-and-the-vmaccess-extension) és majd rájuk.</span><span class="sxs-lookup"><span data-stu-id="8e113-113">[Use raw JSON files that the VMAccess Extension process](#use-json-files-and-the-vmaccess-extension) and then act on.</span></span>

<span data-ttu-id="8e113-114">A következő példákban [az vm felhasználói](/cli/azure/vm/user) parancsok.</span><span class="sxs-lookup"><span data-stu-id="8e113-114">The following examples use [az vm user](/cli/azure/vm/user) commands.</span></span> <span data-ttu-id="8e113-115">A következő lépésekkel lesz szüksége a legújabb [Azure CLI 2.0](/cli/azure/install-az-cli2) telepítve, és bejelentkezett az Azure-fiók használatával [az bejelentkezési](/cli/azure/#login).</span><span class="sxs-lookup"><span data-stu-id="8e113-115">To perform these steps, you need the latest [Azure CLI 2.0](/cli/azure/install-az-cli2) installed and logged in to an Azure account using [az login](/cli/azure/#login).</span></span>

## <a name="reset-ssh-key"></a><span data-ttu-id="8e113-116">SSH-kulcs visszaállítása</span><span class="sxs-lookup"><span data-stu-id="8e113-116">Reset SSH key</span></span>
<span data-ttu-id="8e113-117">Az alábbi példa visszaállítja az SSH-kulcs a felhasználó `azureuser` nevű virtuális gépen `myVM`:</span><span class="sxs-lookup"><span data-stu-id="8e113-117">The following example resets the SSH key for the user `azureuser` on the VM named `myVM`:</span></span>

```azurecli
az vm user update \
  --resource-group myResourceGroup \
  --name myVM \
  --username azureuser \
  --ssh-key-value ~/.ssh/id_rsa.pub
```

## <a name="reset-password"></a><span data-ttu-id="8e113-118">Új jelszó létrehozása</span><span class="sxs-lookup"><span data-stu-id="8e113-118">Reset password</span></span>
<span data-ttu-id="8e113-119">Az alábbi példában a felhasználó jelszava alaphelyzetbe állítása `azureuser` nevű virtuális gépen `myVM`:</span><span class="sxs-lookup"><span data-stu-id="8e113-119">The following example resets the password for the user `azureuser` on the VM named `myVM`:</span></span>

```azurecli
az vm user update \
  --resource-group myResourceGroup \
  --name myVM \
  --username azureuser \
  --password myNewPassword
```

## <a name="restart-ssh"></a><span data-ttu-id="8e113-120">Indítsa újra az SSH</span><span class="sxs-lookup"><span data-stu-id="8e113-120">Restart SSH</span></span>
<span data-ttu-id="8e113-121">A következő példa az SSH démon újraindul, és visszaállítja az SSH-konfigurációt az alapértelmezett értékekre a nevű virtuális gép `myVM`:</span><span class="sxs-lookup"><span data-stu-id="8e113-121">The following example restarts the SSH daemon and resets the SSH configuration to default values on a VM named `myVM`:</span></span>

```azurecli
az vm user reset-ssh \
  --resource-group myResourceGroup \
  --name myVM
```

## <a name="create-a-user"></a><span data-ttu-id="8e113-122">Felhasználó létrehozása</span><span class="sxs-lookup"><span data-stu-id="8e113-122">Create a user</span></span>
<span data-ttu-id="8e113-123">Az alábbi példakód létrehozza a felhasználó nevű `myNewUser` nevű virtuális gép hitelesítéséhez SSH-kulcs használata `myVM`:</span><span class="sxs-lookup"><span data-stu-id="8e113-123">The following example creates a user named `myNewUser` using an SSH key for authentication on the VM named `myVM`:</span></span>

```azurecli
az vm user update \
  --resource-group myResourceGroup \
  --name myVM \
  --username myNewUser \
  --ssh-key-value ~/.ssh/id_rsa.pub
```

## <a name="delete-a-user"></a><span data-ttu-id="8e113-124">Felhasználó törlése</span><span class="sxs-lookup"><span data-stu-id="8e113-124">Delete a user</span></span>
<span data-ttu-id="8e113-125">A következő példa egy megnevezett felhasználó törli `myNewUser` nevű virtuális gépen `myVM`:</span><span class="sxs-lookup"><span data-stu-id="8e113-125">The following example deletes a user named `myNewUser` on the VM named `myVM`:</span></span>

```azurecli
az vm user delete \
  --resource-group myResourceGroup \
  --name myVM \
  --username myNewUser
```


## <a name="use-json-files-and-the-vmaccess-extension"></a><span data-ttu-id="8e113-126">JSON-fájlok és a VMAccess bővítmény</span><span class="sxs-lookup"><span data-stu-id="8e113-126">Use JSON files and the VMAccess Extension</span></span>
<span data-ttu-id="8e113-127">Az alábbi példák nyers JSON-fájlokat használja.</span><span class="sxs-lookup"><span data-stu-id="8e113-127">The following examples use raw JSON files.</span></span> <span data-ttu-id="8e113-128">Használjon [az virtuálisgép-bővítmény készlet](/cli/azure/vm/extension#set) majd hívni a JSON-fájlokat.</span><span class="sxs-lookup"><span data-stu-id="8e113-128">Use [az vm extension set](/cli/azure/vm/extension#set) to then call your JSON files.</span></span> <span data-ttu-id="8e113-129">A JSON-fájlok az Azure-sablonok alapján is hívható.</span><span class="sxs-lookup"><span data-stu-id="8e113-129">These JSON files can also be called from Azure templates.</span></span> 

### <a name="reset-user-access"></a><span data-ttu-id="8e113-130">Felhasználói hozzáférés alaphelyzetbe állítása</span><span class="sxs-lookup"><span data-stu-id="8e113-130">Reset user access</span></span>
<span data-ttu-id="8e113-131">Ha elvesztette a hozzáférést, legfelső szintű a Linux virtuális gépre, a felhasználó az SSH-kulcsot, vagy a jelszó alaphelyzetbe állítása a vmaccess bővítmény parancsfájl indíthatja el.</span><span class="sxs-lookup"><span data-stu-id="8e113-131">If you have lost access to root on your Linux VM, you can launch a VMAccess script to reset a user's SSH key or password.</span></span>

<span data-ttu-id="8e113-132">Alaphelyzetbe állítja a nyilvános SSH-kulcs egy olyan felhasználó, hozzon létre egy fájlt `reset_ssh_key.json` , és adja hozzá a beállítások a következő formátumban.</span><span class="sxs-lookup"><span data-stu-id="8e113-132">To reset the SSH public key of a user, create a file named `reset_ssh_key.json` and add settings in the following format.</span></span> <span data-ttu-id="8e113-133">A saját értékeit helyettesítse a `username` és `ssh_key` paraméterek:</span><span class="sxs-lookup"><span data-stu-id="8e113-133">Substitute your own values for the `username` and `ssh_key` parameters:</span></span>

```json
{
  "username":"azureuser",
  "ssh_key":"ssh-rsa AAAAB3NzaC1yc2EAAAADAQABAAACAQCZ3S7gGp3rcbKmG2Y4vGZFMuMZCwoUzZNGxxxxxx2XV2x9FfAhy8iGD+lF8UdjFX3t5ebMm6BnnMh8fHwkTRdOt3LDQq8o8ElTBrZaKPxZN2thMZnODs5Hlemb2UX0oRIGRcvWqsd4oJmxsXa/Si98Wa6RHWbc9QZhw80KAcOVhmndZAZAGR+Wq6yslNo5TMOr1/ZyQAook5C4FtcSGn3Y+WczaoGWIxG4ZaWk128g79VIeJcIQqOjPodHvQAhll7qDlItVvBfMOben3GyhYTm7k4YwlEdkONm4yV/UIW0la1rmyztSBQIm9sZmSq44XXgjVmDHNF8UfCZ1ToE4r2SdwTmZv00T2i5faeYnHzxiLPA3Enub7xxxxxxwFArnqad7MO1SY1kLemhX9eFjLWN4mJe56Fu4NiWJkR9APSZQrYeKaqru4KUC68QpVasNJHbuxPSf/PcjF3cjO1+X+4x6L1H5HTPuqUkyZGgDO4ynUHbko4dhlanALcriF7tIfQR9i2r2xOyv5gxJEW/zztGqWma/d4rBoPjnf6tO7rLFHXMt/DVTkAfn5wxxtLDwkn5FMyvThRmex3BDf0gujoI1y6cOWLe9Y5geNX0oj+MXg/W0cXAtzSFocstV1PoVqy883hNoeQZ3mIGB3Q0rIUm5d9MA2bMMt31m1g3Sin6EQ== azureuser@myVM"
}
```

<span data-ttu-id="8e113-134">A vmaccess bővítmény parancsprogram végrehajtása:</span><span class="sxs-lookup"><span data-stu-id="8e113-134">Execute the VMAccess script with:</span></span>

```azurecli
az vm extension set \
  --resource-group myResourceGroup \
  --vm-name myVM \
  --name VMAccessForLinux \
  --publisher Microsoft.OSTCExtensions \
  --version 1.4 \
  --protected-settings reset_ssh_key.json
```

<span data-ttu-id="8e113-135">Felhasználói jelszó alaphelyzetbe állítása, hozzon létre egy fájlt `reset_user_password.json` , és adja hozzá a beállítások a következő formátumban.</span><span class="sxs-lookup"><span data-stu-id="8e113-135">To reset a user password, create a file named `reset_user_password.json` and add settings in the following format.</span></span> <span data-ttu-id="8e113-136">A saját értékeit helyettesítse a `username` és `password` paraméterek:</span><span class="sxs-lookup"><span data-stu-id="8e113-136">Substitute your own values for the `username` and `password` parameters:</span></span>

```json
{
  "username":"azureuser",
  "password":"myNewPassword" 
}
```

<span data-ttu-id="8e113-137">A vmaccess bővítmény parancsprogram végrehajtása:</span><span class="sxs-lookup"><span data-stu-id="8e113-137">Execute the VMAccess script with:</span></span>

```azurecli
az vm extension set \
  --resource-group myResourceGroup \
  --vm-name myVM \
  --name VMAccessForLinux \
  --publisher Microsoft.OSTCExtensions \
  --version 1.4 \
  --protected-settings reset_user_password.json
```

### <a name="restart-ssh"></a><span data-ttu-id="8e113-138">Indítsa újra az SSH</span><span class="sxs-lookup"><span data-stu-id="8e113-138">Restart SSH</span></span>
<span data-ttu-id="8e113-139">Indítsa újra az SSH démon, és visszaállítja az SSH-konfigurációt az alapértelmezett értékekre, hozzon létre egy fájlt `reset_sshd.json`.</span><span class="sxs-lookup"><span data-stu-id="8e113-139">To restart the SSH daemon and reset the SSH configuration to default values, create a file named `reset_sshd.json`.</span></span> <span data-ttu-id="8e113-140">Adja hozzá a következőket:</span><span class="sxs-lookup"><span data-stu-id="8e113-140">Add the following content:</span></span>

```json
{
  "reset_ssh": true
}
```

<span data-ttu-id="8e113-141">A vmaccess bővítmény parancsprogram végrehajtása:</span><span class="sxs-lookup"><span data-stu-id="8e113-141">Execute the VMAccess script with:</span></span>

```azurecli
az vm extension set \
  --resource-group myResourceGroup \
  --vm-name myVM \
  --name VMAccessForLinux \
  --publisher Microsoft.OSTCExtensions \
  --version 1.4 \
  --protected-settings reset_sshd.json
```

### <a name="manage-users"></a><span data-ttu-id="8e113-142">Felhasználók kezelése</span><span class="sxs-lookup"><span data-stu-id="8e113-142">Manage users</span></span>

<span data-ttu-id="8e113-143">Hozzon létre egy felhasználót, egy SSH-kulcsot használ, hozzon létre egy fájlt `create_new_user.json` , és adja hozzá a beállítások a következő formátumban.</span><span class="sxs-lookup"><span data-stu-id="8e113-143">To create a user that uses an SSH key for authentication, create a file named `create_new_user.json` and add settings in the following format.</span></span> <span data-ttu-id="8e113-144">A saját értékeit helyettesítse a `username` és `ssh_key` paraméterek:</span><span class="sxs-lookup"><span data-stu-id="8e113-144">Substitute your own values for the `username` and `ssh_key` parameters:</span></span>

```json
{
  "username":"myNewUser",
  "ssh_key":"ssh-rsa AAAAB3NzaC1yc2EAAAADAQABAAACAQCZ3S7gGp3rcbKmG2Y4vGZFMuMZCwoUzZNG1vHY7P2XV2x9FfAhy8iGD+lF8UdjFX3t5ebMm6BnnMh8fHwkTRdOt3LDQq8o8ElTBrZaKPxZN2thMZnODs5Hlemb2UX0oRIGRcvWqsd4oJmxsXa/Si98Wa6RHWbc9QZhw80KAcOVhmndZAZAGR+Wq6yslNo5TMOr1/ZyQAook5C4FtcSGn3Y+WczaoGWIxG4ZaWk128g79VIeJcIQqOjPodHvQAhll7qDlItVvBfMOben3GyhYTm7k4YwlEdkONm4yV/UIW0la1rmyztSBQIm9sZmSq44XXgjVmDHNF8UfCZ1ToE4r2SdwTmZv00T2i5faeYnHzxiLPA3Enub7iUo5IdwFArnqad7MO1SY1kLemhX9eFjLWN4mJe56Fu4NiWJkR9APSZQrYeKaqru4KUC68QpVasNJHbuxPSf/PcjF3cjO1+X+4x6L1H5HTPuqUkyZGgDO4ynUHbko4dhlanALcriF7tIfQR9i2r2xOyv5gxJEW/zztGqWma/d4rBoPjnf6tO7rLFHXMt/DVTkAfn5woYtLDwkn5FMyvThRmex3BDf0gujoI1y6cOWLe9Y5geNX0oj+MXg/W0cXAtzSFocstV1PoVqy883hNoeQZ3mIGB3Q0rIUm5d9MA2bMMt31m1g3Sin6EQ== myNewUser@myVM",
  "password":"myNewUserPassword"
}
```

<span data-ttu-id="8e113-145">A vmaccess bővítmény parancsprogram végrehajtása:</span><span class="sxs-lookup"><span data-stu-id="8e113-145">Execute the VMAccess script with:</span></span>

```azurecli
az vm extension set \
  --resource-group myResourceGroup \
  --vm-name myVM \
  --name VMAccessForLinux \
  --publisher Microsoft.OSTCExtensions \
  --version 1.4 \
  --protected-settings create_new_user.json
```

<span data-ttu-id="8e113-146">Felhasználó törlése, hozzon létre egy fájlt `delete_user.json` , és adja hozzá a következő tartalmat.</span><span class="sxs-lookup"><span data-stu-id="8e113-146">To delete a user, create a file named `delete_user.json` and add the following content.</span></span> <span data-ttu-id="8e113-147">Helyettesítse a saját értéke a `remove_user` paraméter:</span><span class="sxs-lookup"><span data-stu-id="8e113-147">Substitute your own value for the `remove_user` parameter:</span></span>

```json
{
  "remove_user":"myNewUser"
}
```

<span data-ttu-id="8e113-148">A vmaccess bővítmény parancsprogram végrehajtása:</span><span class="sxs-lookup"><span data-stu-id="8e113-148">Execute the VMAccess script with:</span></span>

```azurecli
az vm extension set \
  --resource-group myResourceGroup \
  --vm-name myVM \
  --name VMAccessForLinux \
  --publisher Microsoft.OSTCExtensions \
  --version 1.4 \
  --protected-settings delete_user.json
```

### <a name="check-or-repair-the-disk"></a><span data-ttu-id="8e113-149">Ellenőrizze, vagy javítsa ki a lemez</span><span class="sxs-lookup"><span data-stu-id="8e113-149">Check or repair the disk</span></span>
<span data-ttu-id="8e113-150">Vmaccess bővítmény használatával ellenőrizze, és javítsa ki egy lemezt, a Linux virtuális gép hozzáadott-e.</span><span class="sxs-lookup"><span data-stu-id="8e113-150">Using VMAccess you can also check and repair a disk that you added to the Linux VM.</span></span>

<span data-ttu-id="8e113-151">Ellenőrizze és javítsa ki a lemezt, hozzon létre egy fájlt `disk_check_repair.json` , és adja hozzá a beállítások a következő formátumban.</span><span class="sxs-lookup"><span data-stu-id="8e113-151">To check and then repair the disk, create a file named `disk_check_repair.json` and add settings in the following format.</span></span> <span data-ttu-id="8e113-152">Helyettesítse a saját nevét a következő `repair_disk`:</span><span class="sxs-lookup"><span data-stu-id="8e113-152">Substitute your own value for the name of `repair_disk`:</span></span>

```json
{
  "check_disk": "true",
  "repair_disk": "true, mydiskname"
}
```

<span data-ttu-id="8e113-153">A vmaccess bővítmény parancsprogram végrehajtása:</span><span class="sxs-lookup"><span data-stu-id="8e113-153">Execute the VMAccess script with:</span></span>

```azurecli
az vm extension set \
  --resource-group myResourceGroup \
  --vm-name myVM \
  --name VMAccessForLinux \
  --publisher Microsoft.OSTCExtensions \
  --version 1.4 \
  --protected-settings disk_check_repair.json
```

## <a name="next-steps"></a><span data-ttu-id="8e113-154">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="8e113-154">Next steps</span></span>
<span data-ttu-id="8e113-155">Az Azure VMAccess bővítmény használatával Linux frissítése a módosításokat a futó Linux virtuális gép módszerrel.</span><span class="sxs-lookup"><span data-stu-id="8e113-155">Updating Linux using the Azure VMAccess Extension is one method to make changes on a running Linux VM.</span></span> <span data-ttu-id="8e113-156">Eszközök, például a felhő inicializálás és az Azure Resource Manager-sablonok segítségével módosíthatja a Linux virtuális gép rendszerindító.</span><span class="sxs-lookup"><span data-stu-id="8e113-156">You can also use tools like cloud-init and Azure Resource Manager templates to modify your Linux VM on boot.</span></span>

[<span data-ttu-id="8e113-157">Virtuálisgép-bővítmények és a Linux funkcióit</span><span class="sxs-lookup"><span data-stu-id="8e113-157">Virtual machine extensions and features for Linux</span></span>](extensions-features.md)

[<span data-ttu-id="8e113-158">Linux Virtuálisgép-bővítmények az Azure Resource Manager sablonok készítése</span><span class="sxs-lookup"><span data-stu-id="8e113-158">Authoring Azure Resource Manager templates with Linux VM extensions</span></span>](../windows/template-description.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)

[<span data-ttu-id="8e113-159">Felhő inicializálás segítségével testre szabhatja a Linux virtuális gép létrehozása során</span><span class="sxs-lookup"><span data-stu-id="8e113-159">Using cloud-init to customize a Linux VM during creation</span></span>](using-cloud-init.md)

