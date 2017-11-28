---
title: "aaaReset hozzáférés tooan Azure Linux virtuális gép |} Microsoft Docs"
description: "Hogyan toomanage felhasználók és a visszaállítási hozzáférés Linux virtuális gépek használata a VMAccess bővítmény hello és hello Azure CLI 2.0"
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
ms.openlocfilehash: 2f8db01b9fac20bf547d8b1926e5c0b3c5d18280
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="manage-users-ssh-and-check-or-repair-disks-on-linux-vms-using-hello-vmaccess-extension-with-hello-azure-cli-20"></a><span data-ttu-id="76c23-103">Kezelheti a felhasználókat, az SSH és az ellenőrzése vagy javítása lemezek Linux virtuális gépek használata a VMAccess bővítmény hello a hello Azure CLI 2.0</span><span class="sxs-lookup"><span data-stu-id="76c23-103">Manage users, SSH, and check or repair disks on Linux VMs using hello VMAccess Extension with hello Azure CLI 2.0</span></span>
<span data-ttu-id="76c23-104">a Linux virtuális gép lemezének hello hibák láthatók.</span><span class="sxs-lookup"><span data-stu-id="76c23-104">hello disk on your Linux VM is showing errors.</span></span> <span data-ttu-id="76c23-105">Valamilyen módon alaphelyzetbe hello gyökér szintű jelszavát a Linux virtuális Gépet, vagy véletlenül törli a titkos SSH-kulcsot.</span><span class="sxs-lookup"><span data-stu-id="76c23-105">You somehow reset hello root password for your Linux VM or accidentally deleted your SSH private key.</span></span> <span data-ttu-id="76c23-106">Vissza hello napban hello Datacenter bekövetkezett, ha meg szeretné toodrive van szüksége, és nyisson meg hello KVM tooget hello kiszolgáló konzolján.</span><span class="sxs-lookup"><span data-stu-id="76c23-106">If that happened back in hello days of hello datacenter, you would need toodrive there and then open hello KVM tooget at hello server console.</span></span> <span data-ttu-id="76c23-107">Hello Azure VMAccess bővítmény gondol adott KVM kapcsoló, amely lehetővé teszi, hogy Ön tooaccess konzol tooreset hozzáférés tooLinux hello, vagy végezzen szint szerint.</span><span class="sxs-lookup"><span data-stu-id="76c23-107">Think of hello Azure VMAccess extension as that KVM switch that allows you tooaccess hello console tooreset access tooLinux or perform disk level maintenance.</span></span>

<span data-ttu-id="76c23-108">Ez a cikk bemutatja, hogyan toouse hello Azure VMAccess bővítmény toocheck vagy javítsa ki a lemez, alaphelyzetbe állítja a felhasználói hozzáférés, felhasználói fiókok kezelése, vagy visszaállítja hello Linux SSH-konfigurációt.</span><span class="sxs-lookup"><span data-stu-id="76c23-108">This article shows you how toouse hello Azure VMAccess Extension toocheck or repair a disk, reset user access, manage user accounts, or reset hello SSH configuration on Linux.</span></span> <span data-ttu-id="76c23-109">Is elvégezheti ezeket a lépéseket hello [Azure CLI 1.0](using-vmaccess-extension-nodejs.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="76c23-109">You can also perform these steps with hello [Azure CLI 1.0](using-vmaccess-extension-nodejs.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span></span>


## <a name="ways-toouse-hello-vmaccess-extension"></a><span data-ttu-id="76c23-110">Többféleképpen toouse hello VMAccess bővítmény</span><span class="sxs-lookup"><span data-stu-id="76c23-110">Ways toouse hello VMAccess Extension</span></span>
<span data-ttu-id="76c23-111">Két módon használható hello VMAccess bővítmény a Linux virtuális gépeken:</span><span class="sxs-lookup"><span data-stu-id="76c23-111">There are two ways that you can use hello VMAccess Extension on your Linux VMs:</span></span>

* <span data-ttu-id="76c23-112">Hello Azure CLI 2.0 és a szükséges hello paraméterek használata.</span><span class="sxs-lookup"><span data-stu-id="76c23-112">Use hello Azure CLI 2.0 and hello required parameters.</span></span>
* <span data-ttu-id="76c23-113">[Használjon nyers JSON-fájlokat a VMAccess bővítmény folyamat hello](#use-json-files-and-the-vmaccess-extension) és majd rájuk.</span><span class="sxs-lookup"><span data-stu-id="76c23-113">[Use raw JSON files that hello VMAccess Extension process](#use-json-files-and-the-vmaccess-extension) and then act on.</span></span>

<span data-ttu-id="76c23-114">a következő példák használata hello [az vm felhasználói](/cli/azure/vm/user) parancsok.</span><span class="sxs-lookup"><span data-stu-id="76c23-114">hello following examples use [az vm user](/cli/azure/vm/user) commands.</span></span> <span data-ttu-id="76c23-115">Ezek a lépések tooperform, kell hello legújabb [Azure CLI 2.0](/cli/azure/install-az-cli2) telepítve, és bejelentkezett tooan Azure-fiók használatával [az bejelentkezési](/cli/azure/#login).</span><span class="sxs-lookup"><span data-stu-id="76c23-115">tooperform these steps, you need hello latest [Azure CLI 2.0](/cli/azure/install-az-cli2) installed and logged in tooan Azure account using [az login](/cli/azure/#login).</span></span>

## <a name="reset-ssh-key"></a><span data-ttu-id="76c23-116">SSH-kulcs visszaállítása</span><span class="sxs-lookup"><span data-stu-id="76c23-116">Reset SSH key</span></span>
<span data-ttu-id="76c23-117">hello alábbi példa visszaállítja hello SSH-kulcs hello felhasználói `azureuser` hello nevű virtuális gép a `myVM`:</span><span class="sxs-lookup"><span data-stu-id="76c23-117">hello following example resets hello SSH key for hello user `azureuser` on hello VM named `myVM`:</span></span>

```azurecli
az vm user update \
  --resource-group myResourceGroup \
  --name myVM \
  --username azureuser \
  --ssh-key-value ~/.ssh/id_rsa.pub
```

## <a name="reset-password"></a><span data-ttu-id="76c23-118">Új jelszó létrehozása</span><span class="sxs-lookup"><span data-stu-id="76c23-118">Reset password</span></span>
<span data-ttu-id="76c23-119">hello alábbi példa jelszavának alaphelyzetbe állítása hello hello felhasználó `azureuser` hello nevű virtuális gép a `myVM`:</span><span class="sxs-lookup"><span data-stu-id="76c23-119">hello following example resets hello password for hello user `azureuser` on hello VM named `myVM`:</span></span>

```azurecli
az vm user update \
  --resource-group myResourceGroup \
  --name myVM \
  --username azureuser \
  --password myNewPassword
```

## <a name="restart-ssh"></a><span data-ttu-id="76c23-120">Indítsa újra az SSH</span><span class="sxs-lookup"><span data-stu-id="76c23-120">Restart SSH</span></span>
<span data-ttu-id="76c23-121">hello alábbi példa újraindítja hello SSH démon, és alaphelyzetbe állítását hello SSH konfigurációs toodefault értékek a nevű virtuális gép `myVM`:</span><span class="sxs-lookup"><span data-stu-id="76c23-121">hello following example restarts hello SSH daemon and resets hello SSH configuration toodefault values on a VM named `myVM`:</span></span>

```azurecli
az vm user reset-ssh \
  --resource-group myResourceGroup \
  --name myVM
```

## <a name="create-a-user"></a><span data-ttu-id="76c23-122">Felhasználó létrehozása</span><span class="sxs-lookup"><span data-stu-id="76c23-122">Create a user</span></span>
<span data-ttu-id="76c23-123">hello alábbi példa létrehoz egy megnevezett felhasználó `myNewUser` hello nevű virtuális gép hitelesítéséhez SSH-kulcs használata `myVM`:</span><span class="sxs-lookup"><span data-stu-id="76c23-123">hello following example creates a user named `myNewUser` using an SSH key for authentication on hello VM named `myVM`:</span></span>

```azurecli
az vm user update \
  --resource-group myResourceGroup \
  --name myVM \
  --username myNewUser \
  --ssh-key-value ~/.ssh/id_rsa.pub
```

## <a name="delete-a-user"></a><span data-ttu-id="76c23-124">Felhasználó törlése</span><span class="sxs-lookup"><span data-stu-id="76c23-124">Delete a user</span></span>
<span data-ttu-id="76c23-125">hello alábbi példa törli nevű felhasználó `myNewUser` hello nevű virtuális gép a `myVM`:</span><span class="sxs-lookup"><span data-stu-id="76c23-125">hello following example deletes a user named `myNewUser` on hello VM named `myVM`:</span></span>

```azurecli
az vm user delete \
  --resource-group myResourceGroup \
  --name myVM \
  --username myNewUser
```


## <a name="use-json-files-and-hello-vmaccess-extension"></a><span data-ttu-id="76c23-126">JSON-fájlokat használ, és a VMAccess bővítmény hello</span><span class="sxs-lookup"><span data-stu-id="76c23-126">Use JSON files and hello VMAccess Extension</span></span>
<span data-ttu-id="76c23-127">a következő példák hello nyers JSON-fájlokat használja.</span><span class="sxs-lookup"><span data-stu-id="76c23-127">hello following examples use raw JSON files.</span></span> <span data-ttu-id="76c23-128">Használjon [az virtuálisgép-bővítmény készlet](/cli/azure/vm/extension#set) toothen hívja a JSON-fájlokat.</span><span class="sxs-lookup"><span data-stu-id="76c23-128">Use [az vm extension set](/cli/azure/vm/extension#set) toothen call your JSON files.</span></span> <span data-ttu-id="76c23-129">A JSON-fájlok az Azure-sablonok alapján is hívható.</span><span class="sxs-lookup"><span data-stu-id="76c23-129">These JSON files can also be called from Azure templates.</span></span> 

### <a name="reset-user-access"></a><span data-ttu-id="76c23-130">Felhasználói hozzáférés alaphelyzetbe állítása</span><span class="sxs-lookup"><span data-stu-id="76c23-130">Reset user access</span></span>
<span data-ttu-id="76c23-131">Hozzáférés tooroot elvesztette a Linux virtuális gépre, ha a felhasználó az SSH-kulcsot vagy jelszót indítja el a vmaccess bővítmény parancsfájl tooreset.</span><span class="sxs-lookup"><span data-stu-id="76c23-131">If you have lost access tooroot on your Linux VM, you can launch a VMAccess script tooreset a user's SSH key or password.</span></span>

<span data-ttu-id="76c23-132">tooreset hello SSH nyilvános kulcsát egy olyan felhasználó, hozzon létre egy fájlt `reset_ssh_key.json` és beállítások hozzáadása a formátum a következő hello.</span><span class="sxs-lookup"><span data-stu-id="76c23-132">tooreset hello SSH public key of a user, create a file named `reset_ssh_key.json` and add settings in hello following format.</span></span> <span data-ttu-id="76c23-133">Helyettesítse a saját értékeit hello `username` és `ssh_key` paraméterek:</span><span class="sxs-lookup"><span data-stu-id="76c23-133">Substitute your own values for hello `username` and `ssh_key` parameters:</span></span>

```json
{
  "username":"azureuser",
  "ssh_key":"ssh-rsa AAAAB3NzaC1yc2EAAAADAQABAAACAQCZ3S7gGp3rcbKmG2Y4vGZFMuMZCwoUzZNGxxxxxx2XV2x9FfAhy8iGD+lF8UdjFX3t5ebMm6BnnMh8fHwkTRdOt3LDQq8o8ElTBrZaKPxZN2thMZnODs5Hlemb2UX0oRIGRcvWqsd4oJmxsXa/Si98Wa6RHWbc9QZhw80KAcOVhmndZAZAGR+Wq6yslNo5TMOr1/ZyQAook5C4FtcSGn3Y+WczaoGWIxG4ZaWk128g79VIeJcIQqOjPodHvQAhll7qDlItVvBfMOben3GyhYTm7k4YwlEdkONm4yV/UIW0la1rmyztSBQIm9sZmSq44XXgjVmDHNF8UfCZ1ToE4r2SdwTmZv00T2i5faeYnHzxiLPA3Enub7xxxxxxwFArnqad7MO1SY1kLemhX9eFjLWN4mJe56Fu4NiWJkR9APSZQrYeKaqru4KUC68QpVasNJHbuxPSf/PcjF3cjO1+X+4x6L1H5HTPuqUkyZGgDO4ynUHbko4dhlanALcriF7tIfQR9i2r2xOyv5gxJEW/zztGqWma/d4rBoPjnf6tO7rLFHXMt/DVTkAfn5wxxtLDwkn5FMyvThRmex3BDf0gujoI1y6cOWLe9Y5geNX0oj+MXg/W0cXAtzSFocstV1PoVqy883hNoeQZ3mIGB3Q0rIUm5d9MA2bMMt31m1g3Sin6EQ== azureuser@myVM"
}
```

<span data-ttu-id="76c23-134">Hello VMAccess parancsprogram végrehajtása:</span><span class="sxs-lookup"><span data-stu-id="76c23-134">Execute hello VMAccess script with:</span></span>

```azurecli
az vm extension set \
  --resource-group myResourceGroup \
  --vm-name myVM \
  --name VMAccessForLinux \
  --publisher Microsoft.OSTCExtensions \
  --version 1.4 \
  --protected-settings reset_ssh_key.json
```

<span data-ttu-id="76c23-135">tooreset felhasználói jelszó, hozzon létre egy fájlt `reset_user_password.json` és beállítások hozzáadása a formátum a következő hello.</span><span class="sxs-lookup"><span data-stu-id="76c23-135">tooreset a user password, create a file named `reset_user_password.json` and add settings in hello following format.</span></span> <span data-ttu-id="76c23-136">Helyettesítse a saját értékeit hello `username` és `password` paraméterek:</span><span class="sxs-lookup"><span data-stu-id="76c23-136">Substitute your own values for hello `username` and `password` parameters:</span></span>

```json
{
  "username":"azureuser",
  "password":"myNewPassword" 
}
```

<span data-ttu-id="76c23-137">Hello VMAccess parancsprogram végrehajtása:</span><span class="sxs-lookup"><span data-stu-id="76c23-137">Execute hello VMAccess script with:</span></span>

```azurecli
az vm extension set \
  --resource-group myResourceGroup \
  --vm-name myVM \
  --name VMAccessForLinux \
  --publisher Microsoft.OSTCExtensions \
  --version 1.4 \
  --protected-settings reset_user_password.json
```

### <a name="restart-ssh"></a><span data-ttu-id="76c23-138">Indítsa újra az SSH</span><span class="sxs-lookup"><span data-stu-id="76c23-138">Restart SSH</span></span>
<span data-ttu-id="76c23-139">toorestart SSH démon hello és hello SSH konfigurációs toodefault értékek visszaállítása, hozzon létre egy fájlt `reset_sshd.json`.</span><span class="sxs-lookup"><span data-stu-id="76c23-139">toorestart hello SSH daemon and reset hello SSH configuration toodefault values, create a file named `reset_sshd.json`.</span></span> <span data-ttu-id="76c23-140">Adja hozzá a következő tartalmat hello:</span><span class="sxs-lookup"><span data-stu-id="76c23-140">Add hello following content:</span></span>

```json
{
  "reset_ssh": true
}
```

<span data-ttu-id="76c23-141">Hello VMAccess parancsprogram végrehajtása:</span><span class="sxs-lookup"><span data-stu-id="76c23-141">Execute hello VMAccess script with:</span></span>

```azurecli
az vm extension set \
  --resource-group myResourceGroup \
  --vm-name myVM \
  --name VMAccessForLinux \
  --publisher Microsoft.OSTCExtensions \
  --version 1.4 \
  --protected-settings reset_sshd.json
```

### <a name="manage-users"></a><span data-ttu-id="76c23-142">Felhasználók kezelése</span><span class="sxs-lookup"><span data-stu-id="76c23-142">Manage users</span></span>

<span data-ttu-id="76c23-143">toocreate egy olyan felhasználó, egy SSH-kulcsot használ a hitelesítéshez, hozzon létre egy fájlt `create_new_user.json` és beállítások hozzáadása a formátum a következő hello.</span><span class="sxs-lookup"><span data-stu-id="76c23-143">toocreate a user that uses an SSH key for authentication, create a file named `create_new_user.json` and add settings in hello following format.</span></span> <span data-ttu-id="76c23-144">Helyettesítse a saját értékeit hello `username` és `ssh_key` paraméterek:</span><span class="sxs-lookup"><span data-stu-id="76c23-144">Substitute your own values for hello `username` and `ssh_key` parameters:</span></span>

```json
{
  "username":"myNewUser",
  "ssh_key":"ssh-rsa AAAAB3NzaC1yc2EAAAADAQABAAACAQCZ3S7gGp3rcbKmG2Y4vGZFMuMZCwoUzZNG1vHY7P2XV2x9FfAhy8iGD+lF8UdjFX3t5ebMm6BnnMh8fHwkTRdOt3LDQq8o8ElTBrZaKPxZN2thMZnODs5Hlemb2UX0oRIGRcvWqsd4oJmxsXa/Si98Wa6RHWbc9QZhw80KAcOVhmndZAZAGR+Wq6yslNo5TMOr1/ZyQAook5C4FtcSGn3Y+WczaoGWIxG4ZaWk128g79VIeJcIQqOjPodHvQAhll7qDlItVvBfMOben3GyhYTm7k4YwlEdkONm4yV/UIW0la1rmyztSBQIm9sZmSq44XXgjVmDHNF8UfCZ1ToE4r2SdwTmZv00T2i5faeYnHzxiLPA3Enub7iUo5IdwFArnqad7MO1SY1kLemhX9eFjLWN4mJe56Fu4NiWJkR9APSZQrYeKaqru4KUC68QpVasNJHbuxPSf/PcjF3cjO1+X+4x6L1H5HTPuqUkyZGgDO4ynUHbko4dhlanALcriF7tIfQR9i2r2xOyv5gxJEW/zztGqWma/d4rBoPjnf6tO7rLFHXMt/DVTkAfn5woYtLDwkn5FMyvThRmex3BDf0gujoI1y6cOWLe9Y5geNX0oj+MXg/W0cXAtzSFocstV1PoVqy883hNoeQZ3mIGB3Q0rIUm5d9MA2bMMt31m1g3Sin6EQ== myNewUser@myVM",
  "password":"myNewUserPassword"
}
```

<span data-ttu-id="76c23-145">Hello VMAccess parancsprogram végrehajtása:</span><span class="sxs-lookup"><span data-stu-id="76c23-145">Execute hello VMAccess script with:</span></span>

```azurecli
az vm extension set \
  --resource-group myResourceGroup \
  --vm-name myVM \
  --name VMAccessForLinux \
  --publisher Microsoft.OSTCExtensions \
  --version 1.4 \
  --protected-settings create_new_user.json
```

<span data-ttu-id="76c23-146">a felhasználó toodelete hozzon létre egy fájlt `delete_user.json` , és adja hozzá a tartalom a következő hello.</span><span class="sxs-lookup"><span data-stu-id="76c23-146">toodelete a user, create a file named `delete_user.json` and add hello following content.</span></span> <span data-ttu-id="76c23-147">Helyettesítse a saját értéke hello `remove_user` paraméter:</span><span class="sxs-lookup"><span data-stu-id="76c23-147">Substitute your own value for hello `remove_user` parameter:</span></span>

```json
{
  "remove_user":"myNewUser"
}
```

<span data-ttu-id="76c23-148">Hello VMAccess parancsprogram végrehajtása:</span><span class="sxs-lookup"><span data-stu-id="76c23-148">Execute hello VMAccess script with:</span></span>

```azurecli
az vm extension set \
  --resource-group myResourceGroup \
  --vm-name myVM \
  --name VMAccessForLinux \
  --publisher Microsoft.OSTCExtensions \
  --version 1.4 \
  --protected-settings delete_user.json
```

### <a name="check-or-repair-hello-disk"></a><span data-ttu-id="76c23-149">Ellenőrizze, vagy javítsa ki hello lemez</span><span class="sxs-lookup"><span data-stu-id="76c23-149">Check or repair hello disk</span></span>
<span data-ttu-id="76c23-150">Vmaccess bővítmény használatával is ellenőrizze és javítsa ki, hogy hozzáadta a Linux virtuális gép toohello lemezt.</span><span class="sxs-lookup"><span data-stu-id="76c23-150">Using VMAccess you can also check and repair a disk that you added toohello Linux VM.</span></span>

<span data-ttu-id="76c23-151">toocheck és hello lemezt, majd hozzon létre egy fájlt `disk_check_repair.json` és beállítások hozzáadása a formátum a következő hello.</span><span class="sxs-lookup"><span data-stu-id="76c23-151">toocheck and then repair hello disk, create a file named `disk_check_repair.json` and add settings in hello following format.</span></span> <span data-ttu-id="76c23-152">Helyettesítse a saját hello nevét a következő `repair_disk`:</span><span class="sxs-lookup"><span data-stu-id="76c23-152">Substitute your own value for hello name of `repair_disk`:</span></span>

```json
{
  "check_disk": "true",
  "repair_disk": "true, mydiskname"
}
```

<span data-ttu-id="76c23-153">Hello VMAccess parancsprogram végrehajtása:</span><span class="sxs-lookup"><span data-stu-id="76c23-153">Execute hello VMAccess script with:</span></span>

```azurecli
az vm extension set \
  --resource-group myResourceGroup \
  --vm-name myVM \
  --name VMAccessForLinux \
  --publisher Microsoft.OSTCExtensions \
  --version 1.4 \
  --protected-settings disk_check_repair.json
```

## <a name="next-steps"></a><span data-ttu-id="76c23-154">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="76c23-154">Next steps</span></span>
<span data-ttu-id="76c23-155">Linux frissítése hello Azure VMAccess bővítmény használata a Linux virtuális gép egy metódus toomake módosításait.</span><span class="sxs-lookup"><span data-stu-id="76c23-155">Updating Linux using hello Azure VMAccess Extension is one method toomake changes on a running Linux VM.</span></span> <span data-ttu-id="76c23-156">Használhatja például a felhő inicializálás és az Azure Resource Manager sablonok toomodify eszközök a Linux virtuális Gépet a rendszerindító.</span><span class="sxs-lookup"><span data-stu-id="76c23-156">You can also use tools like cloud-init and Azure Resource Manager templates toomodify your Linux VM on boot.</span></span>

[<span data-ttu-id="76c23-157">Virtuálisgép-bővítmények és a Linux funkcióit</span><span class="sxs-lookup"><span data-stu-id="76c23-157">Virtual machine extensions and features for Linux</span></span>](extensions-features.md)

[<span data-ttu-id="76c23-158">Linux Virtuálisgép-bővítmények az Azure Resource Manager sablonok készítése</span><span class="sxs-lookup"><span data-stu-id="76c23-158">Authoring Azure Resource Manager templates with Linux VM extensions</span></span>](../windows/template-description.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)

[<span data-ttu-id="76c23-159">Használatával a felhő inicializálás toocustomize Linux virtuális gép létrehozása során</span><span class="sxs-lookup"><span data-stu-id="76c23-159">Using cloud-init toocustomize a Linux VM during creation</span></span>](using-cloud-init.md)

