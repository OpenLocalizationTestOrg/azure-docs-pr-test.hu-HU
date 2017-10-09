---
title: "egy RedHat Linux virtuális gép tooan Azure Active Directory Tartományi aaaJoin |} Microsoft Docs"
description: "Hogyan toojoin egy meglévő Azure Active Directory tartományi szolgáltatások RedHat Enterprise Linux 7 virtuális gép tooan."
services: virtual-machines-linux
documentationcenter: virtual-machines-linux
author: vlivech
manager: timlt
editor: 
ms.assetid: 
ms.service: virtual-machines-linux
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure
ms.date: 12/14/2016
ms.author: v-livech
ms.openlocfilehash: f3ba3c764e253191753f1cc5fc8c3b85c53818af
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="join-a-redhat-linux-vm-tooan-azure-active-directory-domain-service"></a>Csatlakozás egy RedHat Linux virtuális gép tooan Azure Active Directory tartományi szolgáltatások

Ez a cikk bemutatja, hogyan kezeli az toojoin a Red Hat Enterprise Linux (RHEL) 7 virtuális gép tooan Azure Active Directory tartományi szolgáltatások (AADDS) a tartományhoz.  hello követelményei a következők:

- [egy Azure-fiók](https://azure.microsoft.com/pricing/free-trial/)

- [SSH nyilvános- és titkoskulcs-fájlok](mac-create-ssh-keys.md)

- [egy Azure Active Directory tartományi szolgáltatások tartományvezérlő](../../active-directory-domain-services/active-directory-ds-getting-started.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)

## <a name="quick-commands"></a>Gyors parancsok

_Cserélje le olyan saját beállításaival._

### <a name="switch-hello-azure-cli-tooclassic-deployment-mode"></a>Kapcsoló hello azure-cli tooclassic telepítési módban

```azurecli
azure config mode asm
```

### <a name="search-for-a-rhel-version-and-image"></a>Egy RHEL verziója és a lemezkép keresése

```azurecli
azure vm image list | grep "Red Hat"
```

### <a name="create-a-redhat-linux-vm"></a>Egy Redhat Linux virtuális gép létrehozása

```azurecli
azure vm create myVM \
-o a879bbefc56a43abb0ce65052aac09f3__RHEL_7_2_Standard_Azure_RHUI-20161026220742 \
-g ahmet \
-p myPassword \
-e 22 \
-t "~/.ssh/id_rsa.pub" \
-z "Small" \
-l "West US"
```

### <a name="ssh-toohello-vm"></a>SSH toohello méretű VM

```bash
ssh -i ~/.ssh/id_rsa ahmet@myVM
```

### <a name="update-yum-packages"></a>YUM-csomagok frissítése

```bash
sudo yum update
```

### <a name="install-packages-needed"></a>Szükséges csomagok telepítése

```bash
sudo yum -y install realmd sssd krb5-workstation krb5-libs
```

Most, hogy a szükséges hello csomagok hello Linux virtuális gépek vannak telepítve, hello következő feladata toojoin hello virtuális gép toohello által felügyelt tartományokhoz.

### <a name="discover-hello-aad-domain-services-managed-domain"></a>Hello AAD tartományi szolgáltatásokra által kezelt tartomány felderítése

```bash
sudo realm discover mydomain.com
```

### <a name="initialize-kerberos"></a>Kerberos inicializálása

Adjon meg egy felhasználót, aki toohello "AAD DC rendszergazdák" csoportba tartozik. Csak ezek a felhasználók kapcsolódhatnak számítógépek toohello által felügyelt tartományokhoz.

```bash
kinit ahmet@mydomain.com
```

### <a name="join-hello-machine-toohello-domain"></a>Hello gép toohello tartományhoz

```bash
sudo realm join --verbose mydomain.com -U 'ahmet@mydomain.com'
```

### <a name="verify-hello-machine-is-joined-toohello-domain"></a>Győződjön meg arról hello gép illesztett toohello tartomány

```bash
ssh -l ahmet@mydomain.com mydomain.cloudapp.net
```

## <a name="next-steps"></a>Következő lépések

* [Red Hat frissítési infrastruktúra (RHUI) igény Red Hat Enterprise Linux virtuális gépek Azure-ban](update-infrastructure-redhat.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)
* [A virtuális gépek az Azure Resource Manager Key Vault beállítása](key-vault-setup.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)
* [Központi telepítése és virtuális gépek kezelése az Azure Resource Manager-sablonok és hello Azure parancssori felület használatával](../linux/create-ssh-secured-vm-from-template.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)
