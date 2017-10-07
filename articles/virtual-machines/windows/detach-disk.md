---
title: "Windowsos VM – Azure adatlemezt aaaDetach |} Microsoft Docs"
description: "Ismerje meg, hogy a virtuális gép az Azure-ban hello Resource Manager üzembe helyezési modellben adatlemezt toodetach."
services: virtual-machines-windows
documentationcenter: 
author: cynthn
manager: timlt
editor: 
tags: azure-service-management
ms.assetid: 13180343-ac49-4a3a-85d8-0ead95e2028c
ms.service: virtual-machines-windows
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-windows
ms.devlang: na
ms.topic: article
ms.date: 03/21/2017
ms.author: cynthn
ms.openlocfilehash: f3f581d3f33329db2ecb7d25a68bc59af7361aad
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="how-toodetach-a-data-disk-from-a-windows-virtual-machine"></a>Hogyan toodetach az adatok lemezre Windows virtuális gépről
Ha már nincs szüksége, amely virtuális géphez csatolt tooa adatlemezt, könnyen leválasztás. Hello lemez eltávolítása a hello virtuális gépről, de ez nem távolítja el a tárolóból.

> [!WARNING]
> A lemez leválasztása esetén nem automatikusan törlődik. Ha tooPremium tárolási, Ön továbbra tooincur tárolási költségek hello lemezhez. További információt talál a túl[árak és számlázás prémium szintű Storage használatakor](../../storage/common/storage-premium-storage.md#pricing-and-billing).
>
>

Ha újra toouse hello meglévő hello a lemezen lévő adatokat, akkor is csatlakoztassa újra toohello ugyanahhoz a virtuális géphez, vagy egy másikra.

## <a name="detach-a-data-disk-using-hello-portal"></a>Hello portálon adatlemez leválasztása
1. Hello portál központban, válassza ki a **virtuális gépek**.
2. Válassza ki a hello virtuális gépekről, amelyek hello adatlemez toodetach ki, majd kattintson **leállítása** toodeallocate hello virtuális gép.
3. Hello virtuális gép panelén válassza **lemezek**.
4. Hello hello tetején **lemezek** panelen válassza **szerkesztése**.
5. A hello **lemezek** panelen toohello hello adatlemez, hogy szeretné-e toodetach, a jobb szélén kattintson hello ![leválasztási gomb képe](./media/detach-disk/detach.png) leválasztani gombra.
5. Hello lemez eltávolítása után kattintson a Mentés gombra hello felül hello panelről.
6. Hello virtuális gép paneljén kattintson **áttekintése** majd hello **Start** hello panel toorestart hello VM hello tetején gombra.



hello lemez tárolási megmarad, de már nem csatlakoztatott tooa virtuális gépet.

## <a name="detach-a-data-disk-using-powershell"></a>PowerShell-lel adatlemez leválasztása
Ebben a példában hello lekérdezi hello nevű virtuális gép első parancs **MyVM07** a hello **RG11** erőforráscsoport hello Get-AzureRmVM parancsmag használatával. tárolja a virtuális gép hello hello parancs hello **$VirtualMachine** változó.

hello második parancs eltávolítja az hello adatlemez DataDisk3 nevű hello virtuális gépről.

hello utolsó parancs frissíti hello virtuális gép toocomplete hello folyamat hello adatlemez eltávolításával hello állapotát.

```powershell
$VirtualMachine = Get-AzureRmVM -ResourceGroupName "RG11" -Name "MyVM07"
Remove-AzureRmVMDataDisk -VM $VirtualMachine -Name "DataDisk3"
Update-AzureRmVM -ResourceGroupName "RG11" -Name "MyVM07" -VM $VirtualMachine
```

További információkért lásd: [Remove-AzureRmVMDataDisk](/powershell/module/azurerm.compute/remove-azurermvmdatadisk).

## <a name="next-steps"></a>Következő lépések
Ha azt szeretné, hogy tooreuse hello adatlemez, most [tooanother virtuális gép csatlakoztatása](attach-managed-disk-portal.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json)

