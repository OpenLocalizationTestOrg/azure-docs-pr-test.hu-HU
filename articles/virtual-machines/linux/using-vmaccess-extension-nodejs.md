---
title: "aaaReset access Azure Linux virtuális gépek használata a VMAccess bővítmény hello |} Microsoft Docs"
description: "Alaphelyzetbe állítja a hozzáférés az Azure Linux virtuális gépeken futó hello VMAccess bővítmény használatával."
services: virtual-machines-linux
documentationcenter: 
author: vlivech
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: 261a9646-1f93-407e-951e-0be7226b3064
ms.service: virtual-machines-linux
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-linux
ms.devlang: na
ms.topic: article
ms.date: 10/25/2016
ms.author: v-livech
ms.openlocfilehash: 2636655f3f7d14ba30e1dc62c319e4e278521ead
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="manage-users-ssh-and-check-or-repair-disks-on-azure-linux-vms-using-hello-vmaccess-extension-with-hello-azure-cli-10"></a><span data-ttu-id="d7df4-103">Kezelheti a felhasználókat, az SSH és az ellenőrzése vagy javítása lemezek Azure Linux virtuális gépek használata a VMAccess bővítmény hello az Azure CLI 1.0 hello</span><span class="sxs-lookup"><span data-stu-id="d7df4-103">Manage users, SSH, and check or repair disks on Azure Linux VMs using hello VMAccess Extension with hello Azure CLI 1.0</span></span>
<span data-ttu-id="d7df4-104">Ez a cikk bemutatja, hogyan toouse hello Azure VMAcesss bővítmény toocheck vagy javítsa ki a lemezt, alaphelyzetbe állítja a felhasználói hozzáférés, felhasználói fiókok kezelése vagy Linux hello SSHD konfigurációt állítja vissza.</span><span class="sxs-lookup"><span data-stu-id="d7df4-104">This article shows you how toouse hello Azure VMAcesss Extension toocheck or repair a disk, reset user access, manage user accounts, or reset hello SSHD configuration on Linux.</span></span> <span data-ttu-id="d7df4-105">hello cikk van szükség:</span><span class="sxs-lookup"><span data-stu-id="d7df4-105">hello article requires:</span></span>

* <span data-ttu-id="d7df4-106">egy Azure-fiókra ([ingyenes próbaverzió beszerzése](https://azure.microsoft.com/pricing/free-trial/)),</span><span class="sxs-lookup"><span data-stu-id="d7df4-106">an Azure account ([get a free trial](https://azure.microsoft.com/pricing/free-trial/)).</span></span>
* <span data-ttu-id="d7df4-107">Hello [Azure CLI](../../cli-install-nodejs.md) bejelentkezett `azure login`.</span><span class="sxs-lookup"><span data-stu-id="d7df4-107">hello [Azure CLI](../../cli-install-nodejs.md) logged in with `azure login`.</span></span>
* <span data-ttu-id="d7df4-108">az Azure parancssori felület hello *kell* Azure Resource Manager módra `azure config mode arm`.</span><span class="sxs-lookup"><span data-stu-id="d7df4-108">hello Azure CLI *must be in* Azure Resource Manager mode `azure config mode arm`.</span></span>


## <a name="cli-versions-toocomplete-hello-task"></a><span data-ttu-id="d7df4-109">Parancssori felület verziók toocomplete hello feladat</span><span class="sxs-lookup"><span data-stu-id="d7df4-109">CLI versions toocomplete hello task</span></span>
<span data-ttu-id="d7df4-110">Hello feladat a következő parancssori felület verziók hello egyikével hajthatja végre:</span><span class="sxs-lookup"><span data-stu-id="d7df4-110">You can complete hello task using one of hello following CLI versions:</span></span>

- <span data-ttu-id="d7df4-111">[Az Azure CLI 1.0](#quick-commands)– a parancssori felületen hello klasszikus és resource management üzembe helyezési modellel (a cikk)</span><span class="sxs-lookup"><span data-stu-id="d7df4-111">[Azure CLI 1.0](#quick-commands)– our CLI for hello classic and resource management deployment models (this article)</span></span>
- <span data-ttu-id="d7df4-112">[Az Azure CLI 2.0](using-vmaccess-extension.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) -a következő generációs CLI hello erőforrás felügyeleti telepítési modell</span><span class="sxs-lookup"><span data-stu-id="d7df4-112">[Azure CLI 2.0](using-vmaccess-extension.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) - our next generation CLI for hello resource management deployment model</span></span>


## <a name="quick-commands"></a><span data-ttu-id="d7df4-113">Gyors parancsok</span><span class="sxs-lookup"><span data-stu-id="d7df4-113">Quick commands</span></span>
<span data-ttu-id="d7df4-114">Két módon toouse VMAccess a Linux virtuális gépeken van:</span><span class="sxs-lookup"><span data-stu-id="d7df4-114">There are two ways toouse VMAccess on your Linux VMs:</span></span>

* <span data-ttu-id="d7df4-115">Hello Azure CLI 1.0 és hello használata kötelező paraméterek.</span><span class="sxs-lookup"><span data-stu-id="d7df4-115">Using hello Azure CLI 1.0 and hello required parameters.</span></span>
* <span data-ttu-id="d7df4-116">Nyers VMAccess dolgozza fel, és a műveletek JSON-fájlokat használja.</span><span class="sxs-lookup"><span data-stu-id="d7df4-116">Using raw JSON files that VMAccess processes and then act on.</span></span>

<span data-ttu-id="d7df4-117">Hello gyors parancs szakaszra, fogjuk toouse hello Azure CLI 1.0 `azure vm reset-access` metódust.</span><span class="sxs-lookup"><span data-stu-id="d7df4-117">For hello quick command section, we are going toouse hello Azure CLI 1.0 `azure vm reset-access` method.</span></span> <span data-ttu-id="d7df4-118">Hello alábbi parancspéldákban cserélje le, amelyek tartalmazzák a saját környezet hello értékekkel "példa" értékeket hello.</span><span class="sxs-lookup"><span data-stu-id="d7df4-118">In hello following command examples, replace hello values that contain "example" with hello values from your own environment.</span></span>

## <a name="create-a-resource-group-and-linux-vm"></a><span data-ttu-id="d7df4-119">Egy erőforráscsoport és a Linux virtuális gép létrehozása</span><span class="sxs-lookup"><span data-stu-id="d7df4-119">Create a Resource Group and Linux VM</span></span>
```bash
azure group create myResourceGroup westus
```

## <a name="create-a-debian-vm"></a><span data-ttu-id="d7df4-120">Debian virtuális gép létrehozása</span><span class="sxs-lookup"><span data-stu-id="d7df4-120">Create a Debian VM</span></span>
```azurecli
azure vm quick-create \
  -M ~/.ssh/id_rsa.pub \
  -u myAdminUser \
  -g myResourceGroup \
  -l westus \
  -y Linux \
  -n myVM \
  -Q Debian
```

## <a name="reset-root-password"></a><span data-ttu-id="d7df4-121">Gyökér szintű jelszó alaphelyzetbe állítása</span><span class="sxs-lookup"><span data-stu-id="d7df4-121">Reset root password</span></span>
<span data-ttu-id="d7df4-122">tooreset hello gyökér szintű jelszavát:</span><span class="sxs-lookup"><span data-stu-id="d7df4-122">tooreset hello root password:</span></span>

```azurecli
azure vm reset-access \
  -g myResourceGroup \
  -n myVM \
  -u root \
  -p myNewPassword
```

## <a name="ssh-key-reset"></a><span data-ttu-id="d7df4-123">SSH-kulcs visszaállítása</span><span class="sxs-lookup"><span data-stu-id="d7df4-123">SSH key reset</span></span>
<span data-ttu-id="d7df4-124">tooreset hello SSH-kulcs nem legfelső szintű felhasználói:</span><span class="sxs-lookup"><span data-stu-id="d7df4-124">tooreset hello SSH key of a non-root user:</span></span>

```azurecli
azure vm reset-access \
  -g myResourceGroup \
  -n myVM \
  -u myAdminUser \
  -M ~/.ssh/id_rsa.pub
```

## <a name="create-a-user"></a><span data-ttu-id="d7df4-125">Felhasználó létrehozása</span><span class="sxs-lookup"><span data-stu-id="d7df4-125">Create a user</span></span>
<span data-ttu-id="d7df4-126">a felhasználó toocreate:</span><span class="sxs-lookup"><span data-stu-id="d7df4-126">toocreate a user:</span></span>

```azurecli
azure vm reset-access \
  -g myResourceGroup \
  -n myVM \
  -u myAdminUser \
  -p myAdminUserPassword
```

## <a name="remove-a-user"></a><span data-ttu-id="d7df4-127">Felhasználó eltávolítása</span><span class="sxs-lookup"><span data-stu-id="d7df4-127">Remove a user</span></span>
```azurecli
azure vm reset-access \
  -g myResourceGroup \
  -n myVM \
  -R myRemovedUser
```

## <a name="reset-sshd"></a><span data-ttu-id="d7df4-128">SSHD alaphelyzetbe állítása</span><span class="sxs-lookup"><span data-stu-id="d7df4-128">Reset SSHD</span></span>
<span data-ttu-id="d7df4-129">tooreset hello SSHD konfiguráció:</span><span class="sxs-lookup"><span data-stu-id="d7df4-129">tooreset hello SSHD configuration:</span></span>

```azurecli
azure vm reset-access \
  -g myResourceGroup \
  -n myVM
  -r
```


## <a name="detailed-walkthrough"></a><span data-ttu-id="d7df4-130">Részletes bemutató</span><span class="sxs-lookup"><span data-stu-id="d7df4-130">Detailed walkthrough</span></span>
### <a name="vmaccess-defined"></a><span data-ttu-id="d7df4-131">A vmaccess bővítmény definiálva:</span><span class="sxs-lookup"><span data-stu-id="d7df4-131">VMAccess defined:</span></span>
<span data-ttu-id="d7df4-132">a Linux virtuális gép lemezének hello hibák láthatók.</span><span class="sxs-lookup"><span data-stu-id="d7df4-132">hello disk on your Linux VM is showing errors.</span></span> <span data-ttu-id="d7df4-133">Valamilyen módon alaphelyzetbe hello gyökér szintű jelszavát a Linux virtuális Gépet, vagy véletlenül törli a titkos SSH-kulcsot.</span><span class="sxs-lookup"><span data-stu-id="d7df4-133">You somehow reset hello root password for your Linux VM or accidentally deleted your SSH private key.</span></span> <span data-ttu-id="d7df4-134">Vissza hello napban hello Datacenter bekövetkezett, ha meg szeretné toodrive van szüksége, és nyisson meg hello KVM tooget hello kiszolgáló konzolján.</span><span class="sxs-lookup"><span data-stu-id="d7df4-134">If that happened back in hello days of hello datacenter, you would need toodrive there and then open hello KVM tooget at hello server console.</span></span> <span data-ttu-id="d7df4-135">Hello Azure VMAccess bővítmény gondol adott KVM kapcsoló, amely lehetővé teszi, hogy Ön tooaccess konzol tooreset hozzáférés tooLinux hello, vagy végezzen szint szerint.</span><span class="sxs-lookup"><span data-stu-id="d7df4-135">Think of hello Azure VMAccess extension as that KVM switch that allows you tooaccess hello console tooreset access tooLinux or perform disk level maintenance.</span></span>

<span data-ttu-id="d7df4-136">Részletes útmutatást hello fogjuk toouse hello hosszú alak a vmaccess bővítmény, amely nyers JSON-fájlokat használja.</span><span class="sxs-lookup"><span data-stu-id="d7df4-136">For hello detailed walkthrough, we are going toouse hello long form of VMAccess, which uses raw JSON files.</span></span>  <span data-ttu-id="d7df4-137">Ezeket a vmaccess bővítmény JSON-fájlokat az Azure-sablonok alapján is hívható.</span><span class="sxs-lookup"><span data-stu-id="d7df4-137">These VMAccess JSON files can also be called from Azure templates.</span></span>

### <a name="using-vmaccess-toocheck-or-repair-hello-disk-of-a-linux-vm"></a><span data-ttu-id="d7df4-138">VMAccess toocheck vagy javítása hello lemez a Linux virtuális gépek használatával</span><span class="sxs-lookup"><span data-stu-id="d7df4-138">Using VMAccess toocheck or repair hello disk of a Linux VM</span></span>
<span data-ttu-id="d7df4-139">Vmaccess bővítmény használatával elvégezhető egy fsck futtatása a Linux virtuális Gépet a hello lemezen.</span><span class="sxs-lookup"><span data-stu-id="d7df4-139">Using VMAccess you can do a fsck run on hello disk under your Linux VM.</span></span>  <span data-ttu-id="d7df4-140">Megteheti azt is, a lemez-ellenőrzés és egy lemezjavítás a vmaccess bővítmény használatával.</span><span class="sxs-lookup"><span data-stu-id="d7df4-140">You can also do a disk check and a disk repair using a VMAccess.</span></span>

<span data-ttu-id="d7df4-141">toocheck, és hello lemez a vmaccess bővítmény parancsprogram használata:</span><span class="sxs-lookup"><span data-stu-id="d7df4-141">toocheck, and then repair hello disk use this VMAccess script:</span></span>

`disk_check_repair.json`

```json
{
  "check_disk": "true",
  "repair_disk": "true, user-disk-name"
}
```

<span data-ttu-id="d7df4-142">Hello VMAccess parancsprogram végrehajtása:</span><span class="sxs-lookup"><span data-stu-id="d7df4-142">Execute hello VMAccess script with:</span></span>

```azurecli
azure vm extension set \
  myResourceGroup \
  myVM \
  VMAccessForLinux \
  Microsoft.OSTCExtensions * \
  --private-config-path disk_check_repair.json
```

### <a name="using-vmaccess-tooreset-user-access-toolinux"></a><span data-ttu-id="d7df4-143">Használja a vmaccess bővítmény tooreset felhasználói hozzáférés tooLinux</span><span class="sxs-lookup"><span data-stu-id="d7df4-143">Using VMAccess tooreset user access tooLinux</span></span>
<span data-ttu-id="d7df4-144">Ha hozzáférési tooroot elvesztette a Linux virtuális gépre, indítja el a vmaccess bővítmény parancsfájl tooreset hello gyökér szintű jelszavát.</span><span class="sxs-lookup"><span data-stu-id="d7df4-144">If you have lost access tooroot on your Linux VM, you can launch a VMAccess script tooreset hello root password.</span></span>

<span data-ttu-id="d7df4-145">tooreset hello gyökér szintű jelszavát a vmaccess bővítmény parancsfájllal:</span><span class="sxs-lookup"><span data-stu-id="d7df4-145">tooreset hello root password, use this VMAccess script:</span></span>

`reset_root_password.json`

```json
{
  "username":"root",
  "password":"myNewPassword"
}
```

<span data-ttu-id="d7df4-146">Hello VMAccess parancsprogram végrehajtása:</span><span class="sxs-lookup"><span data-stu-id="d7df4-146">Execute hello VMAccess script with:</span></span>

```azurecli
azure vm extension set \
  myResourceGroup \
  myVM \
  VMAccessForLinux \
  Microsoft.OSTCExtensions * \
  --private-config-path reset_root_password.json
```

<span data-ttu-id="d7df4-147">tooreset hello SSH-kulcs a nem rendszergazda felhasználó a vmaccess bővítmény parancsprogram használata:</span><span class="sxs-lookup"><span data-stu-id="d7df4-147">tooreset hello SSH key of a non-root user, use this VMAccess script:</span></span>

`reset_ssh_key.json`

```json
{
  "username":"myAdminUser",
  "ssh_key":"ssh-rsa AAAAB3NzaC1yc2EAAAADAQABAAACAQCZ3S7gGp3rcbKmG2Y4vGZFMuMZCwoUzZNG1vHY7P2XV2x9FfAhy8iGD+lF8UdjFX3t5ebMm6BnnMh8fHwkTRdOt3LDQq8o8ElTBrZaKPxZN2thMZnODs5Hlemb2UX0oRIGRcvWqsd4oJmxsXa/Si98Wa6RHWbc9QZhw80KAcOVhmndZAZAGR+Wq6yslNo5TMOr1/ZyQAook5C4FtcSGn3Y+WczaoGWIxG4ZaWk128g79VIeJcIQqOjPodHvQAhll7qDlItVvBfMOben3GyhYTm7k4YwlEdkONm4yV/UIW0la1rmyztSBQIm9sZmSq44XXgjVmDHNF8UfCZ1ToE4r2SdwTmZv00T2i5faeYnHzxiLPA3Enub7iUo5IdwFArnqad7MO1SY1kLemhX9eFjLWN4mJe56Fu4NiWJkR9APSZQrYeKaqru4KUC68QpVasNJHbuxPSf/PcjF3cjO1+X+4x6L1H5HTPuqUkyZGgDO4ynUHbko4dhlanALcriF7tIfQR9i2r2xOyv5gxJEW/zztGqWma/d4rBoPjnf6tO7rLFHXMt/DVTkAfn5woYtLDwkn5FMyvThRmex3BDf0gujoI1y6cOWLe9Y5geNX0oj+MXg/W0cXAtzSFocstV1PoVqy883hNoeQZ3mIGB3Q0rIUm5d9MA2bMMt31m1g3Sin6EQ== myAdminUser@myVM" 
}
```

<span data-ttu-id="d7df4-148">Hello VMAccess parancsprogram végrehajtása:</span><span class="sxs-lookup"><span data-stu-id="d7df4-148">Execute hello VMAccess script with:</span></span>

```azurecli
azure vm extension set \
  myResourceGroup \
  myVM \
  VMAccessForLinux \
  Microsoft.OSTCExtensions * \
  --private-config-path reset_ssh_key.json
```

### <a name="using-vmaccess-toomanage-user-accounts-on-linux"></a><span data-ttu-id="d7df4-149">Linux VMAccess toomanage felhasználói fiókok használatával</span><span class="sxs-lookup"><span data-stu-id="d7df4-149">Using VMAccess toomanage user accounts on Linux</span></span>
<span data-ttu-id="d7df4-150">VMAccess egy Python-parancsfájl, amely használt toomanage felhasználók a Linux virtuális gépén sudo vagy hello root fiókkal és bejelentkezés nélkül.</span><span class="sxs-lookup"><span data-stu-id="d7df4-150">VMAccess is a Python script that can be used toomanage users on your Linux VM without logging in and using sudo or hello root account.</span></span>

<span data-ttu-id="d7df4-151">a felhasználó toocreate a vmaccess bővítmény parancsprogram használata:</span><span class="sxs-lookup"><span data-stu-id="d7df4-151">toocreate a user, use this VMAccess script:</span></span>

`create_new_user.json`

```json
{
  "username":"myNewUser",
  "ssh_key":"ssh-rsa AAAAB3NzaC1yc2EAAAADAQABAAACAQCZ3S7gGp3rcbKmG2Y4vGZFMuMZCwoUzZNG1vHY7P2XV2x9FfAhy8iGD+lF8UdjFX3t5ebMm6BnnMh8fHwkTRdOt3LDQq8o8ElTBrZaKPxZN2thMZnODs5Hlemb2UX0oRIGRcvWqsd4oJmxsXa/Si98Wa6RHWbc9QZhw80KAcOVhmndZAZAGR+Wq6yslNo5TMOr1/ZyQAook5C4FtcSGn3Y+WczaoGWIxG4ZaWk128g79VIeJcIQqOjPodHvQAhll7qDlItVvBfMOben3GyhYTm7k4YwlEdkONm4yV/UIW0la1rmyztSBQIm9sZmSq44XXgjVmDHNF8UfCZ1ToE4r2SdwTmZv00T2i5faeYnHzxiLPA3Enub7iUo5IdwFArnqad7MO1SY1kLemhX9eFjLWN4mJe56Fu4NiWJkR9APSZQrYeKaqru4KUC68QpVasNJHbuxPSf/PcjF3cjO1+X+4x6L1H5HTPuqUkyZGgDO4ynUHbko4dhlanALcriF7tIfQR9i2r2xOyv5gxJEW/zztGqWma/d4rBoPjnf6tO7rLFHXMt/DVTkAfn5woYtLDwkn5FMyvThRmex3BDf0gujoI1y6cOWLe9Y5geNX0oj+MXg/W0cXAtzSFocstV1PoVqy883hNoeQZ3mIGB3Q0rIUm5d9MA2bMMt31m1g3Sin6EQ== myNewUser@myVM",
  "password":"myNewUserPassword"
}
```

<span data-ttu-id="d7df4-152">Hello VMAccess parancsprogram végrehajtása:</span><span class="sxs-lookup"><span data-stu-id="d7df4-152">Execute hello VMAccess script with:</span></span>

```azurecli
azure vm extension set \
  myResourceGroup \
  myVM \
  VMAccessForLinux \
  Microsoft.OSTCExtensions * \
  --private-config-path create_new_user.json
```

<span data-ttu-id="d7df4-153">a felhasználó toodelete a vmaccess bővítmény parancsprogram használata:</span><span class="sxs-lookup"><span data-stu-id="d7df4-153">toodelete a user, use this VMAccess script:</span></span>

`remove_user.json`

```json
{
  "remove_user":"myDeletedUser"
}
```

<span data-ttu-id="d7df4-154">Hello VMAccess parancsprogram végrehajtása:</span><span class="sxs-lookup"><span data-stu-id="d7df4-154">Execute hello VMAccess script with:</span></span>

```azurecli
azure vm extension set \
  myResourceGroup \
  myVM \
  VMAccessForLinux \
  Microsoft.OSTCExtensions * \
  --private-config-path remove_user.json
```

### <a name="using-vmaccess-tooreset-hello-sshd-configuration"></a><span data-ttu-id="d7df4-155">VMAccess tooreset hello SSHD konfiguráció</span><span class="sxs-lookup"><span data-stu-id="d7df4-155">Using VMAccess tooreset hello SSHD configuration</span></span>
<span data-ttu-id="d7df4-156">Ha módosítások toohello Linux virtuális gépek SSHD konfigurációs és Bezárás hello SSH-kapcsolat ellenőrzése hello módosítások előtt, akkor előfordulhat, hogy meg kell akadályoznia SSH'ing vissza.</span><span class="sxs-lookup"><span data-stu-id="d7df4-156">If you make changes toohello Linux VMs SSHD configuration and close hello SSH connection before verifying hello changes, you may be prevented from SSH'ing back in.</span></span>  <span data-ttu-id="d7df4-157">VMAccess használt tooreset hello SSHD konfigurációs hátsó tooa ismert helyes konfigurációra eseményazonosítójú SSH-n keresztül nélkül is lehet.</span><span class="sxs-lookup"><span data-stu-id="d7df4-157">VMAccess can be used tooreset hello SSHD configuration back tooa known good configuration without being logged in over SSH.</span></span>

<span data-ttu-id="d7df4-158">tooreset hello SSHD konfigurációs használja ezt a vmaccess bővítmény parancsfájlt:</span><span class="sxs-lookup"><span data-stu-id="d7df4-158">tooreset hello SSHD configuration use this VMAccess script:</span></span>

`reset_sshd.json`

```json
{
  "reset_ssh": true
}
```

<span data-ttu-id="d7df4-159">Hello VMAccess parancsprogram végrehajtása:</span><span class="sxs-lookup"><span data-stu-id="d7df4-159">Execute hello VMAccess script with:</span></span>

```azurecli
azure vm extension set \
  myResourceGroup \
  myVM \
  VMAccessForLinux \
  Microsoft.OSTCExtensions * \
  --private-config-path reset_sshd.json
```

## <a name="next-steps"></a><span data-ttu-id="d7df4-160">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="d7df4-160">Next steps</span></span>
<span data-ttu-id="d7df4-161">Frissítése Linux Azure VMAccess bővítmény használata a Linux virtuális gép egy metódus toomake módosításait.</span><span class="sxs-lookup"><span data-stu-id="d7df4-161">Updating Linux using Azure VMAccess Extensions is one method toomake changes on a running Linux VM.</span></span>  <span data-ttu-id="d7df4-162">Használhatja például a felhő inicializálás és az Azure-sablonok toomodify eszközök a Linux virtuális Gépet a rendszerindító.</span><span class="sxs-lookup"><span data-stu-id="d7df4-162">You can also use tools like cloud-init and Azure Templates toomodify your Linux VM on boot.</span></span>

[<span data-ttu-id="d7df4-163">Virtuálisgép-bővítmények és szolgáltatásokkal kapcsolatban</span><span class="sxs-lookup"><span data-stu-id="d7df4-163">About virtual machine extensions and features</span></span>](../windows/extensions-features.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)

[<span data-ttu-id="d7df4-164">Linux Virtuálisgép-bővítmények az Azure Resource Manager sablonok készítése</span><span class="sxs-lookup"><span data-stu-id="d7df4-164">Authoring Azure Resource Manager templates with Linux VM extensions</span></span>](../windows/template-description.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)

[<span data-ttu-id="d7df4-165">Használatával a felhő inicializálás toocustomize Linux virtuális gép létrehozása során</span><span class="sxs-lookup"><span data-stu-id="d7df4-165">Using cloud-init toocustomize a Linux VM during creation</span></span>](using-cloud-init.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)

