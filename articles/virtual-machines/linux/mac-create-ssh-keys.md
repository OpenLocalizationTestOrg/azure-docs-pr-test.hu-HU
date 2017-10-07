---
title: "aaaCreate és az SSH használata a Linux virtuális gépek Azure-ban pár kulcs |} Microsoft Docs"
description: "Hogyan toocreate és az SSH nyilvános és titkos kulcsból álló kulcspárt. használja az Azure tooimprove Linux virtuális gépek hello biztonsági hello hitelesítési folyamatot."
services: virtual-machines-linux
documentationcenter: 
author: iainfoulds
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: 34ae9482-da3e-4b2d-9d0d-9d672aa42498
ms.service: virtual-machines-linux
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-linux
ms.devlang: na
ms.topic: get-started-article
ms.date: 08/14/2017
ms.author: iainfou
ms.openlocfilehash: 7fb94841d34d5bc006f3134adf91102ddce5f174
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="how-toocreate-and-use-an-ssh-public-and-private-key-pair-for-linux-vms-in-azure"></a>Hogyan toocreate és az SSH nyilvános és titkos kulcs használata párosítsa Linux virtuális gépek Azure-ban
A secure shell (SSH) kulcspár Azure-ban SSH-kulcsok használata a hitelesítéshez, így nem kell hello jelszavak toolog a virtuális gépek (VM) hozhat létre. Ez a cikk bemutatja, hogyan a tooquickly hozhat létre, és az SSH protokoll 2-es RSA nyilvános és titkos kulcsot tartalmazó fájlt pár használhat Linux virtuális gépekhez. További lépések és további példákat: [részletes lépéseket toocreate SSH-kulcspár és tanúsítványok](create-ssh-keys-detailed.md).

## <a name="create-an-ssh-key-pair"></a>SSH-kulcs létrehozása
Használjon hello `ssh-keygen` parancs toocreate SSH nyilvános és titkos kulcs fájlok hello létrehozott alapértelmezés szerint `~/.ssh` könyvtár, de megadhat egy másik helyet és további jelszót (jelszó tooaccess hello titkos kulcsfájlt) során a rendszer kéri. Futtassa a parancsot a rendszerhéjakba követően, a saját hello kérdések megválaszolásával hello.

```bash
ssh-keygen -t rsa -b 2048
```

## <a name="use-hello-ssh-key-pair"></a>Hello SSH-kulcspárral használata
hello nyilvános kulcsot, amelyet a Linux virtuális Gépet az Azure-ban tárolt alapértelmezés szerint ki van `~/.ssh/id_rsa.pub`, kivéve, ha módosította hello hely létrehozása után azokat. Hello használatakor [Azure CLI 2.0](/cli/azure) toocreate a virtuális gép hello használata esetén a nyilvános kulcs hello hely megadása [az virtuális gép létrehozása](/cli/azure/vm#create) a hello `--ssh-key-path` lehetőséget. Ha másolja és illessze be a nyilvános kulcsot tartalmazó fájlt toouse hello hello tartalmát hello Azure-portálon vagy a Resource Manager-sablon, ügyeljen arra, hogy nem minden további szóköz. Például az operációs rendszer X használatakor átadható hello nyilvános kulcsot tartalmazó fájlt (alapértelmezés szerint **~/.ssh/id_rsa.pub**) túl**pbcopy** (más Linux olyan programok érhetők el, amely ugyanaz, mint például a hellotoocopyhellotartalma`xclip`).

Ha nem ismeri a nyilvános SSH-kulcsokat, megnézheti a saját nyilvános kulcsát a `cat` parancs futtatásával az alábbiak szerint. A `~/.ssh/id_rsa.pub` helyet cserélje le a saját nyilvános kulcsának helyére:

```bash
cat ~/.ssh/id_rsa.pub
```

Hello nyilvános kulcsot az Azure virtuális gép, használó SSH tooyour VM használatával hello IP-cím vagy a virtuális Gépet DNS-nevét (ne feledje tooreplace `azureuser` és `myvm.westus.cloudapp.azure.com` alatt a hello rendszergazda felhasználónevét és a hello teljesen minősített tartománynév – vagy az IP-cím):

```bash
ssh azureuser@myvm.westus.cloudapp.azure.com
```

Ha egy hozzáférési kódot adta meg a kulcspár létrehozásakor, írja be a hello jelszót hello bejelentkezési folyamat során. (hello kiszolgáló hozzá van adva tooyour `~/.ssh/known_hosts` mappa, és nem kell adnia tooconnect újra amíg hello nyilvános kulcsot az Azure virtuális gép változásairól vagy hello kiszolgáló nevét a rendszer eltávolítja a `~/.ssh/known_hosts`.)

## <a name="next-steps"></a>Következő lépések

SSH-kulcsok használatával létrehozott virtuális gépek alapértelmezés szerint úgy konfigurálja a jelszavak le van tiltva, jelentősen drágább, és ezért nehezen toomake találgatásos módszeren alapuló találgatás kísérletek. Ez a témakör a gyors használatra készített egyszerű SSH-kulcspár létrehozását tárgyalja. Ha az SSH-kulcspárral létrehozásához további segítségre van szüksége, vagy szükség további tanúsítványokra, lásd: [részletes lépéseket toocreate SSH-kulcspár és tanúsítványok](create-ssh-keys-detailed.md).

Virtuális gépek, amelyek az SSH-kulcspárral, hello Azure-portálon, a parancssori felület és a sablonok segítségével hozhatja létre:

* [Biztonságos Linux virtuális gép létrehozása hello Azure-portálon](quick-create-portal.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)
* [Biztonságos Linux virtuális gép létrehozása hello Azure CLI 2.0-s)](quick-create-cli.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)
* [Biztonságos Linux virtuális gép létrehozása Azure-sablon alapján](create-ssh-secured-vm-from-template.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)
