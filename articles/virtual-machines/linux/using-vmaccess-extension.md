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
# <a name="manage-users-ssh-and-check-or-repair-disks-on-linux-vms-using-hello-vmaccess-extension-with-hello-azure-cli-20"></a>Kezelheti a felhasználókat, az SSH és az ellenőrzése vagy javítása lemezek Linux virtuális gépek használata a VMAccess bővítmény hello a hello Azure CLI 2.0
a Linux virtuális gép lemezének hello hibák láthatók. Valamilyen módon alaphelyzetbe hello gyökér szintű jelszavát a Linux virtuális Gépet, vagy véletlenül törli a titkos SSH-kulcsot. Vissza hello napban hello Datacenter bekövetkezett, ha meg szeretné toodrive van szüksége, és nyisson meg hello KVM tooget hello kiszolgáló konzolján. Hello Azure VMAccess bővítmény gondol adott KVM kapcsoló, amely lehetővé teszi, hogy Ön tooaccess konzol tooreset hozzáférés tooLinux hello, vagy végezzen szint szerint.

Ez a cikk bemutatja, hogyan toouse hello Azure VMAccess bővítmény toocheck vagy javítsa ki a lemez, alaphelyzetbe állítja a felhasználói hozzáférés, felhasználói fiókok kezelése, vagy visszaállítja hello Linux SSH-konfigurációt. Is elvégezheti ezeket a lépéseket hello [Azure CLI 1.0](using-vmaccess-extension-nodejs.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).


## <a name="ways-toouse-hello-vmaccess-extension"></a>Többféleképpen toouse hello VMAccess bővítmény
Két módon használható hello VMAccess bővítmény a Linux virtuális gépeken:

* Hello Azure CLI 2.0 és a szükséges hello paraméterek használata.
* [Használjon nyers JSON-fájlokat a VMAccess bővítmény folyamat hello](#use-json-files-and-the-vmaccess-extension) és majd rájuk.

a következő példák használata hello [az vm felhasználói](/cli/azure/vm/user) parancsok. Ezek a lépések tooperform, kell hello legújabb [Azure CLI 2.0](/cli/azure/install-az-cli2) telepítve, és bejelentkezett tooan Azure-fiók használatával [az bejelentkezési](/cli/azure/#login).

## <a name="reset-ssh-key"></a>SSH-kulcs visszaállítása
hello alábbi példa visszaállítja hello SSH-kulcs hello felhasználói `azureuser` hello nevű virtuális gép a `myVM`:

```azurecli
az vm user update \
  --resource-group myResourceGroup \
  --name myVM \
  --username azureuser \
  --ssh-key-value ~/.ssh/id_rsa.pub
```

## <a name="reset-password"></a>Új jelszó létrehozása
hello alábbi példa jelszavának alaphelyzetbe állítása hello hello felhasználó `azureuser` hello nevű virtuális gép a `myVM`:

```azurecli
az vm user update \
  --resource-group myResourceGroup \
  --name myVM \
  --username azureuser \
  --password myNewPassword
```

## <a name="restart-ssh"></a>Indítsa újra az SSH
hello alábbi példa újraindítja hello SSH démon, és alaphelyzetbe állítását hello SSH konfigurációs toodefault értékek a nevű virtuális gép `myVM`:

```azurecli
az vm user reset-ssh \
  --resource-group myResourceGroup \
  --name myVM
```

## <a name="create-a-user"></a>Felhasználó létrehozása
hello alábbi példa létrehoz egy megnevezett felhasználó `myNewUser` hello nevű virtuális gép hitelesítéséhez SSH-kulcs használata `myVM`:

```azurecli
az vm user update \
  --resource-group myResourceGroup \
  --name myVM \
  --username myNewUser \
  --ssh-key-value ~/.ssh/id_rsa.pub
```

## <a name="delete-a-user"></a>Felhasználó törlése
hello alábbi példa törli nevű felhasználó `myNewUser` hello nevű virtuális gép a `myVM`:

```azurecli
az vm user delete \
  --resource-group myResourceGroup \
  --name myVM \
  --username myNewUser
```


## <a name="use-json-files-and-hello-vmaccess-extension"></a>JSON-fájlokat használ, és a VMAccess bővítmény hello
a következő példák hello nyers JSON-fájlokat használja. Használjon [az virtuálisgép-bővítmény készlet](/cli/azure/vm/extension#set) toothen hívja a JSON-fájlokat. A JSON-fájlok az Azure-sablonok alapján is hívható. 

### <a name="reset-user-access"></a>Felhasználói hozzáférés alaphelyzetbe állítása
Hozzáférés tooroot elvesztette a Linux virtuális gépre, ha a felhasználó az SSH-kulcsot vagy jelszót indítja el a vmaccess bővítmény parancsfájl tooreset.

tooreset hello SSH nyilvános kulcsát egy olyan felhasználó, hozzon létre egy fájlt `reset_ssh_key.json` és beállítások hozzáadása a formátum a következő hello. Helyettesítse a saját értékeit hello `username` és `ssh_key` paraméterek:

```json
{
  "username":"azureuser",
  "ssh_key":"ssh-rsa AAAAB3NzaC1yc2EAAAADAQABAAACAQCZ3S7gGp3rcbKmG2Y4vGZFMuMZCwoUzZNGxxxxxx2XV2x9FfAhy8iGD+lF8UdjFX3t5ebMm6BnnMh8fHwkTRdOt3LDQq8o8ElTBrZaKPxZN2thMZnODs5Hlemb2UX0oRIGRcvWqsd4oJmxsXa/Si98Wa6RHWbc9QZhw80KAcOVhmndZAZAGR+Wq6yslNo5TMOr1/ZyQAook5C4FtcSGn3Y+WczaoGWIxG4ZaWk128g79VIeJcIQqOjPodHvQAhll7qDlItVvBfMOben3GyhYTm7k4YwlEdkONm4yV/UIW0la1rmyztSBQIm9sZmSq44XXgjVmDHNF8UfCZ1ToE4r2SdwTmZv00T2i5faeYnHzxiLPA3Enub7xxxxxxwFArnqad7MO1SY1kLemhX9eFjLWN4mJe56Fu4NiWJkR9APSZQrYeKaqru4KUC68QpVasNJHbuxPSf/PcjF3cjO1+X+4x6L1H5HTPuqUkyZGgDO4ynUHbko4dhlanALcriF7tIfQR9i2r2xOyv5gxJEW/zztGqWma/d4rBoPjnf6tO7rLFHXMt/DVTkAfn5wxxtLDwkn5FMyvThRmex3BDf0gujoI1y6cOWLe9Y5geNX0oj+MXg/W0cXAtzSFocstV1PoVqy883hNoeQZ3mIGB3Q0rIUm5d9MA2bMMt31m1g3Sin6EQ== azureuser@myVM"
}
```

Hello VMAccess parancsprogram végrehajtása:

```azurecli
az vm extension set \
  --resource-group myResourceGroup \
  --vm-name myVM \
  --name VMAccessForLinux \
  --publisher Microsoft.OSTCExtensions \
  --version 1.4 \
  --protected-settings reset_ssh_key.json
```

tooreset felhasználói jelszó, hozzon létre egy fájlt `reset_user_password.json` és beállítások hozzáadása a formátum a következő hello. Helyettesítse a saját értékeit hello `username` és `password` paraméterek:

```json
{
  "username":"azureuser",
  "password":"myNewPassword" 
}
```

Hello VMAccess parancsprogram végrehajtása:

```azurecli
az vm extension set \
  --resource-group myResourceGroup \
  --vm-name myVM \
  --name VMAccessForLinux \
  --publisher Microsoft.OSTCExtensions \
  --version 1.4 \
  --protected-settings reset_user_password.json
```

### <a name="restart-ssh"></a>Indítsa újra az SSH
toorestart SSH démon hello és hello SSH konfigurációs toodefault értékek visszaállítása, hozzon létre egy fájlt `reset_sshd.json`. Adja hozzá a következő tartalmat hello:

```json
{
  "reset_ssh": true
}
```

Hello VMAccess parancsprogram végrehajtása:

```azurecli
az vm extension set \
  --resource-group myResourceGroup \
  --vm-name myVM \
  --name VMAccessForLinux \
  --publisher Microsoft.OSTCExtensions \
  --version 1.4 \
  --protected-settings reset_sshd.json
```

### <a name="manage-users"></a>Felhasználók kezelése

toocreate egy olyan felhasználó, egy SSH-kulcsot használ a hitelesítéshez, hozzon létre egy fájlt `create_new_user.json` és beállítások hozzáadása a formátum a következő hello. Helyettesítse a saját értékeit hello `username` és `ssh_key` paraméterek:

```json
{
  "username":"myNewUser",
  "ssh_key":"ssh-rsa AAAAB3NzaC1yc2EAAAADAQABAAACAQCZ3S7gGp3rcbKmG2Y4vGZFMuMZCwoUzZNG1vHY7P2XV2x9FfAhy8iGD+lF8UdjFX3t5ebMm6BnnMh8fHwkTRdOt3LDQq8o8ElTBrZaKPxZN2thMZnODs5Hlemb2UX0oRIGRcvWqsd4oJmxsXa/Si98Wa6RHWbc9QZhw80KAcOVhmndZAZAGR+Wq6yslNo5TMOr1/ZyQAook5C4FtcSGn3Y+WczaoGWIxG4ZaWk128g79VIeJcIQqOjPodHvQAhll7qDlItVvBfMOben3GyhYTm7k4YwlEdkONm4yV/UIW0la1rmyztSBQIm9sZmSq44XXgjVmDHNF8UfCZ1ToE4r2SdwTmZv00T2i5faeYnHzxiLPA3Enub7iUo5IdwFArnqad7MO1SY1kLemhX9eFjLWN4mJe56Fu4NiWJkR9APSZQrYeKaqru4KUC68QpVasNJHbuxPSf/PcjF3cjO1+X+4x6L1H5HTPuqUkyZGgDO4ynUHbko4dhlanALcriF7tIfQR9i2r2xOyv5gxJEW/zztGqWma/d4rBoPjnf6tO7rLFHXMt/DVTkAfn5woYtLDwkn5FMyvThRmex3BDf0gujoI1y6cOWLe9Y5geNX0oj+MXg/W0cXAtzSFocstV1PoVqy883hNoeQZ3mIGB3Q0rIUm5d9MA2bMMt31m1g3Sin6EQ== myNewUser@myVM",
  "password":"myNewUserPassword"
}
```

Hello VMAccess parancsprogram végrehajtása:

```azurecli
az vm extension set \
  --resource-group myResourceGroup \
  --vm-name myVM \
  --name VMAccessForLinux \
  --publisher Microsoft.OSTCExtensions \
  --version 1.4 \
  --protected-settings create_new_user.json
```

a felhasználó toodelete hozzon létre egy fájlt `delete_user.json` , és adja hozzá a tartalom a következő hello. Helyettesítse a saját értéke hello `remove_user` paraméter:

```json
{
  "remove_user":"myNewUser"
}
```

Hello VMAccess parancsprogram végrehajtása:

```azurecli
az vm extension set \
  --resource-group myResourceGroup \
  --vm-name myVM \
  --name VMAccessForLinux \
  --publisher Microsoft.OSTCExtensions \
  --version 1.4 \
  --protected-settings delete_user.json
```

### <a name="check-or-repair-hello-disk"></a>Ellenőrizze, vagy javítsa ki hello lemez
Vmaccess bővítmény használatával is ellenőrizze és javítsa ki, hogy hozzáadta a Linux virtuális gép toohello lemezt.

toocheck és hello lemezt, majd hozzon létre egy fájlt `disk_check_repair.json` és beállítások hozzáadása a formátum a következő hello. Helyettesítse a saját hello nevét a következő `repair_disk`:

```json
{
  "check_disk": "true",
  "repair_disk": "true, mydiskname"
}
```

Hello VMAccess parancsprogram végrehajtása:

```azurecli
az vm extension set \
  --resource-group myResourceGroup \
  --vm-name myVM \
  --name VMAccessForLinux \
  --publisher Microsoft.OSTCExtensions \
  --version 1.4 \
  --protected-settings disk_check_repair.json
```

## <a name="next-steps"></a>Következő lépések
Linux frissítése hello Azure VMAccess bővítmény használata a Linux virtuális gép egy metódus toomake módosításait. Használhatja például a felhő inicializálás és az Azure Resource Manager sablonok toomodify eszközök a Linux virtuális Gépet a rendszerindító.

[Virtuálisgép-bővítmények és a Linux funkcióit](extensions-features.md)

[Linux Virtuálisgép-bővítmények az Azure Resource Manager sablonok készítése](../windows/template-description.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)

[Használatával a felhő inicializálás toocustomize Linux virtuális gép létrehozása során](using-cloud-init.md)

