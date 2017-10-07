---
title: "Linux virtuális gép - Azure adatlemezét aaaDetach |} Microsoft Docs"
description: "Ismerje meg, hogy a virtuális gép az Azure-ban a parancssori felület 2.0-s vagy hello Azure-portálon adatlemezt toodetach."
services: virtual-machines-linux
documentationcenter: 
author: cynthn
manager: timlt
editor: 
tags: azure-service-management
ms.assetid: 
ms.service: virtual-machines-linux
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-windows
ms.devlang: azurecli
ms.topic: article
ms.date: 03/21/2017
ms.author: cynthn
ms.openlocfilehash: 1c6145fc97f13179457225e93e0fb7adc261a65b
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="how-toodetach-a-data-disk-from-a-linux-virtual-machine"></a>Hogyan toodetach az adatok lemezre egy Linux virtuális gép

Ha már nincs szüksége, amely virtuális géphez csatolt tooa adatlemezt, könnyen leválasztás. Hello lemez eltávolítása a hello virtuális gépről, de ez nem távolítja el a tárolóból. 

> [!WARNING]
> A lemez leválasztása esetén nem automatikusan törlődik. Ha tooPremium tárolási, Ön továbbra tooincur tárolási költségek hello lemezhez. További információt talál a túl[árak és számlázás prémium szintű Storage használatakor](../../storage/common/storage-premium-storage.md#pricing-and-billing). 
> 
> 

Ha újra toouse hello meglévő hello a lemezen lévő adatokat, akkor is csatlakoztassa újra toohello ugyanahhoz a virtuális géphez, vagy egy másikra.  

## <a name="detach-a-data-disk-using-cli-20"></a>Parancssori felület 2.0 használatával adatlemez leválasztása

```azurecli
az vm disk detach -g myResourceGroup --vm-name myVm -n myDataDisk
```

hello lemez tárolási megmarad, de már nem csatlakoztatott tooa virtuális gépet.


## <a name="detach-a-data-disk-using-hello-portal"></a>Hello portálon adatlemez leválasztása
1. Hello portál központban, válassza ki a **virtuális gépek**.
2. Válassza ki a hello virtuális gépekről, amelyek hello adatlemez toodetach ki, majd kattintson **leállítása** toodeallocate hello virtuális gép.
3. Hello virtuális gép panelén válassza **lemezek**.
4. Hello hello tetején **lemezek** panelen válassza **szerkesztése**.
5. A hello **lemezek** panelen toohello hello adatlemez, hogy szeretné-e toodetach, a jobb szélén kattintson hello ![leválasztási gomb képe](./media/detach-disk/detach.png) leválasztani gombra.
5. Hello lemez eltávolítása után kattintson a Mentés gombra hello felül hello panelről.
6. Hello virtuális gép paneljén kattintson **áttekintése** majd hello **Start** hello panel toorestart hello VM hello tetején gombra.

hello lemez tárolási megmarad, de már nem csatlakoztatott tooa virtuális gépet.








## <a name="next-steps"></a>Következő lépések
Ha azt szeretné, hogy tooreuse hello adatlemez, most [tooanother virtuális gép csatlakoztatása](add-disk.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).

