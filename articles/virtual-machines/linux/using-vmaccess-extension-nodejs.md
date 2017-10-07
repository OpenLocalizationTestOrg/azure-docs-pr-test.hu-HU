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
# <a name="manage-users-ssh-and-check-or-repair-disks-on-azure-linux-vms-using-hello-vmaccess-extension-with-hello-azure-cli-10"></a>Kezelheti a felhasználókat, az SSH és az ellenőrzése vagy javítása lemezek Azure Linux virtuális gépek használata a VMAccess bővítmény hello az Azure CLI 1.0 hello
Ez a cikk bemutatja, hogyan toouse hello Azure VMAcesss bővítmény toocheck vagy javítsa ki a lemezt, alaphelyzetbe állítja a felhasználói hozzáférés, felhasználói fiókok kezelése vagy Linux hello SSHD konfigurációt állítja vissza. hello cikk van szükség:

* egy Azure-fiókra ([ingyenes próbaverzió beszerzése](https://azure.microsoft.com/pricing/free-trial/)),
* Hello [Azure CLI](../../cli-install-nodejs.md) bejelentkezett `azure login`.
* az Azure parancssori felület hello *kell* Azure Resource Manager módra `azure config mode arm`.


## <a name="cli-versions-toocomplete-hello-task"></a>Parancssori felület verziók toocomplete hello feladat
Hello feladat a következő parancssori felület verziók hello egyikével hajthatja végre:

- [Az Azure CLI 1.0](#quick-commands)– a parancssori felületen hello klasszikus és resource management üzembe helyezési modellel (a cikk)
- [Az Azure CLI 2.0](using-vmaccess-extension.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) -a következő generációs CLI hello erőforrás felügyeleti telepítési modell


## <a name="quick-commands"></a>Gyors parancsok
Két módon toouse VMAccess a Linux virtuális gépeken van:

* Hello Azure CLI 1.0 és hello használata kötelező paraméterek.
* Nyers VMAccess dolgozza fel, és a műveletek JSON-fájlokat használja.

Hello gyors parancs szakaszra, fogjuk toouse hello Azure CLI 1.0 `azure vm reset-access` metódust. Hello alábbi parancspéldákban cserélje le, amelyek tartalmazzák a saját környezet hello értékekkel "példa" értékeket hello.

## <a name="create-a-resource-group-and-linux-vm"></a>Egy erőforráscsoport és a Linux virtuális gép létrehozása
```bash
azure group create myResourceGroup westus
```

## <a name="create-a-debian-vm"></a>Debian virtuális gép létrehozása
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

## <a name="reset-root-password"></a>Gyökér szintű jelszó alaphelyzetbe állítása
tooreset hello gyökér szintű jelszavát:

```azurecli
azure vm reset-access \
  -g myResourceGroup \
  -n myVM \
  -u root \
  -p myNewPassword
```

## <a name="ssh-key-reset"></a>SSH-kulcs visszaállítása
tooreset hello SSH-kulcs nem legfelső szintű felhasználói:

```azurecli
azure vm reset-access \
  -g myResourceGroup \
  -n myVM \
  -u myAdminUser \
  -M ~/.ssh/id_rsa.pub
```

## <a name="create-a-user"></a>Felhasználó létrehozása
a felhasználó toocreate:

```azurecli
azure vm reset-access \
  -g myResourceGroup \
  -n myVM \
  -u myAdminUser \
  -p myAdminUserPassword
```

## <a name="remove-a-user"></a>Felhasználó eltávolítása
```azurecli
azure vm reset-access \
  -g myResourceGroup \
  -n myVM \
  -R myRemovedUser
```

## <a name="reset-sshd"></a>SSHD alaphelyzetbe állítása
tooreset hello SSHD konfiguráció:

```azurecli
azure vm reset-access \
  -g myResourceGroup \
  -n myVM
  -r
```


## <a name="detailed-walkthrough"></a>Részletes bemutató
### <a name="vmaccess-defined"></a>A vmaccess bővítmény definiálva:
a Linux virtuális gép lemezének hello hibák láthatók. Valamilyen módon alaphelyzetbe hello gyökér szintű jelszavát a Linux virtuális Gépet, vagy véletlenül törli a titkos SSH-kulcsot. Vissza hello napban hello Datacenter bekövetkezett, ha meg szeretné toodrive van szüksége, és nyisson meg hello KVM tooget hello kiszolgáló konzolján. Hello Azure VMAccess bővítmény gondol adott KVM kapcsoló, amely lehetővé teszi, hogy Ön tooaccess konzol tooreset hozzáférés tooLinux hello, vagy végezzen szint szerint.

Részletes útmutatást hello fogjuk toouse hello hosszú alak a vmaccess bővítmény, amely nyers JSON-fájlokat használja.  Ezeket a vmaccess bővítmény JSON-fájlokat az Azure-sablonok alapján is hívható.

### <a name="using-vmaccess-toocheck-or-repair-hello-disk-of-a-linux-vm"></a>VMAccess toocheck vagy javítása hello lemez a Linux virtuális gépek használatával
Vmaccess bővítmény használatával elvégezhető egy fsck futtatása a Linux virtuális Gépet a hello lemezen.  Megteheti azt is, a lemez-ellenőrzés és egy lemezjavítás a vmaccess bővítmény használatával.

toocheck, és hello lemez a vmaccess bővítmény parancsprogram használata:

`disk_check_repair.json`

```json
{
  "check_disk": "true",
  "repair_disk": "true, user-disk-name"
}
```

Hello VMAccess parancsprogram végrehajtása:

```azurecli
azure vm extension set \
  myResourceGroup \
  myVM \
  VMAccessForLinux \
  Microsoft.OSTCExtensions * \
  --private-config-path disk_check_repair.json
```

### <a name="using-vmaccess-tooreset-user-access-toolinux"></a>Használja a vmaccess bővítmény tooreset felhasználói hozzáférés tooLinux
Ha hozzáférési tooroot elvesztette a Linux virtuális gépre, indítja el a vmaccess bővítmény parancsfájl tooreset hello gyökér szintű jelszavát.

tooreset hello gyökér szintű jelszavát a vmaccess bővítmény parancsfájllal:

`reset_root_password.json`

```json
{
  "username":"root",
  "password":"myNewPassword"
}
```

Hello VMAccess parancsprogram végrehajtása:

```azurecli
azure vm extension set \
  myResourceGroup \
  myVM \
  VMAccessForLinux \
  Microsoft.OSTCExtensions * \
  --private-config-path reset_root_password.json
```

tooreset hello SSH-kulcs a nem rendszergazda felhasználó a vmaccess bővítmény parancsprogram használata:

`reset_ssh_key.json`

```json
{
  "username":"myAdminUser",
  "ssh_key":"ssh-rsa AAAAB3NzaC1yc2EAAAADAQABAAACAQCZ3S7gGp3rcbKmG2Y4vGZFMuMZCwoUzZNG1vHY7P2XV2x9FfAhy8iGD+lF8UdjFX3t5ebMm6BnnMh8fHwkTRdOt3LDQq8o8ElTBrZaKPxZN2thMZnODs5Hlemb2UX0oRIGRcvWqsd4oJmxsXa/Si98Wa6RHWbc9QZhw80KAcOVhmndZAZAGR+Wq6yslNo5TMOr1/ZyQAook5C4FtcSGn3Y+WczaoGWIxG4ZaWk128g79VIeJcIQqOjPodHvQAhll7qDlItVvBfMOben3GyhYTm7k4YwlEdkONm4yV/UIW0la1rmyztSBQIm9sZmSq44XXgjVmDHNF8UfCZ1ToE4r2SdwTmZv00T2i5faeYnHzxiLPA3Enub7iUo5IdwFArnqad7MO1SY1kLemhX9eFjLWN4mJe56Fu4NiWJkR9APSZQrYeKaqru4KUC68QpVasNJHbuxPSf/PcjF3cjO1+X+4x6L1H5HTPuqUkyZGgDO4ynUHbko4dhlanALcriF7tIfQR9i2r2xOyv5gxJEW/zztGqWma/d4rBoPjnf6tO7rLFHXMt/DVTkAfn5woYtLDwkn5FMyvThRmex3BDf0gujoI1y6cOWLe9Y5geNX0oj+MXg/W0cXAtzSFocstV1PoVqy883hNoeQZ3mIGB3Q0rIUm5d9MA2bMMt31m1g3Sin6EQ== myAdminUser@myVM" 
}
```

Hello VMAccess parancsprogram végrehajtása:

```azurecli
azure vm extension set \
  myResourceGroup \
  myVM \
  VMAccessForLinux \
  Microsoft.OSTCExtensions * \
  --private-config-path reset_ssh_key.json
```

### <a name="using-vmaccess-toomanage-user-accounts-on-linux"></a>Linux VMAccess toomanage felhasználói fiókok használatával
VMAccess egy Python-parancsfájl, amely használt toomanage felhasználók a Linux virtuális gépén sudo vagy hello root fiókkal és bejelentkezés nélkül.

a felhasználó toocreate a vmaccess bővítmény parancsprogram használata:

`create_new_user.json`

```json
{
  "username":"myNewUser",
  "ssh_key":"ssh-rsa AAAAB3NzaC1yc2EAAAADAQABAAACAQCZ3S7gGp3rcbKmG2Y4vGZFMuMZCwoUzZNG1vHY7P2XV2x9FfAhy8iGD+lF8UdjFX3t5ebMm6BnnMh8fHwkTRdOt3LDQq8o8ElTBrZaKPxZN2thMZnODs5Hlemb2UX0oRIGRcvWqsd4oJmxsXa/Si98Wa6RHWbc9QZhw80KAcOVhmndZAZAGR+Wq6yslNo5TMOr1/ZyQAook5C4FtcSGn3Y+WczaoGWIxG4ZaWk128g79VIeJcIQqOjPodHvQAhll7qDlItVvBfMOben3GyhYTm7k4YwlEdkONm4yV/UIW0la1rmyztSBQIm9sZmSq44XXgjVmDHNF8UfCZ1ToE4r2SdwTmZv00T2i5faeYnHzxiLPA3Enub7iUo5IdwFArnqad7MO1SY1kLemhX9eFjLWN4mJe56Fu4NiWJkR9APSZQrYeKaqru4KUC68QpVasNJHbuxPSf/PcjF3cjO1+X+4x6L1H5HTPuqUkyZGgDO4ynUHbko4dhlanALcriF7tIfQR9i2r2xOyv5gxJEW/zztGqWma/d4rBoPjnf6tO7rLFHXMt/DVTkAfn5woYtLDwkn5FMyvThRmex3BDf0gujoI1y6cOWLe9Y5geNX0oj+MXg/W0cXAtzSFocstV1PoVqy883hNoeQZ3mIGB3Q0rIUm5d9MA2bMMt31m1g3Sin6EQ== myNewUser@myVM",
  "password":"myNewUserPassword"
}
```

Hello VMAccess parancsprogram végrehajtása:

```azurecli
azure vm extension set \
  myResourceGroup \
  myVM \
  VMAccessForLinux \
  Microsoft.OSTCExtensions * \
  --private-config-path create_new_user.json
```

a felhasználó toodelete a vmaccess bővítmény parancsprogram használata:

`remove_user.json`

```json
{
  "remove_user":"myDeletedUser"
}
```

Hello VMAccess parancsprogram végrehajtása:

```azurecli
azure vm extension set \
  myResourceGroup \
  myVM \
  VMAccessForLinux \
  Microsoft.OSTCExtensions * \
  --private-config-path remove_user.json
```

### <a name="using-vmaccess-tooreset-hello-sshd-configuration"></a>VMAccess tooreset hello SSHD konfiguráció
Ha módosítások toohello Linux virtuális gépek SSHD konfigurációs és Bezárás hello SSH-kapcsolat ellenőrzése hello módosítások előtt, akkor előfordulhat, hogy meg kell akadályoznia SSH'ing vissza.  VMAccess használt tooreset hello SSHD konfigurációs hátsó tooa ismert helyes konfigurációra eseményazonosítójú SSH-n keresztül nélkül is lehet.

tooreset hello SSHD konfigurációs használja ezt a vmaccess bővítmény parancsfájlt:

`reset_sshd.json`

```json
{
  "reset_ssh": true
}
```

Hello VMAccess parancsprogram végrehajtása:

```azurecli
azure vm extension set \
  myResourceGroup \
  myVM \
  VMAccessForLinux \
  Microsoft.OSTCExtensions * \
  --private-config-path reset_sshd.json
```

## <a name="next-steps"></a>Következő lépések
Frissítése Linux Azure VMAccess bővítmény használata a Linux virtuális gép egy metódus toomake módosításait.  Használhatja például a felhő inicializálás és az Azure-sablonok toomodify eszközök a Linux virtuális Gépet a rendszerindító.

[Virtuálisgép-bővítmények és szolgáltatásokkal kapcsolatban](../windows/extensions-features.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)

[Linux Virtuálisgép-bővítmények az Azure Resource Manager sablonok készítése](../windows/template-description.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)

[Használatával a felhő inicializálás toocustomize Linux virtuális gép létrehozása során](using-cloud-init.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)

