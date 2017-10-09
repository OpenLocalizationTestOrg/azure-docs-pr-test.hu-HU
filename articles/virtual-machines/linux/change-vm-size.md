---
title: "Linux virtuális gép hello Azure CLI 2.0 és aaaHow tooresize |} Microsoft Docs"
description: "Hogyan tooscale fel vagy le egy Linux virtuális gépet, módosításával méretezési hello Virtuálisgép-méretet."
services: virtual-machines-linux
documentationcenter: na
author: mikewasson
manager: timlt
editor: 
tags: 
ms.assetid: e163f878-b919-45c5-9f5a-75a64f3b14a0
ms.service: virtual-machines-linux
ms.devlang: azurecli
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 02/10/2017
ms.author: mwasson
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: e8fba485b5bcc7824f546de5cf3df77624a28008
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="resize-a-linux-virtual-machine-using-cli-20"></a>A Linux virtuális gépek CLI 2.0 átméretezése

(VM) virtuális gép kiépítése után méretezheti hello VM felfelé vagy lefelé hello módosításával [Virtuálisgép-méretet][vm-sizes]. Néhány esetben először hello VM kell felszabadítani. Toodeallocate hello virtuális gép mérete nem érhető el a virtuális gép hello üzemeltető hello hardver fürt hello igény kell. Ez a cikk részletesen, hogyan tooresize rendelkező Linux virtuális gépet a hello Azure CLI 2.0. Is elvégezheti ezeket a lépéseket hello [Azure CLI 1.0](change-vm-size-nodejs.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).

## <a name="resize-a-vm"></a>Virtuális gép átméretezése
a virtuális gépek tooresize, kell hello legújabb [Azure CLI 2.0](/cli/azure/install-az-cli2) telepítve, és bejelentkezett tooan Azure-fiók használatával [az bejelentkezési](/cli/azure/#login).

1. Rendelkezésre álló virtuális gép listájának megtekintése hello méretének hello hardveres fürtre, ahol hello virtuális gép tárolása a [az vm-vm-átméretezési-beállításai](/cli/azure/vm#list-vm-resize-options). hello alábbi példa felsorolja hello nevű virtuális gép Virtuálisgép-méretek `myVM` hello erőforráscsoportban `myResourceGroup` régió:
   
    ```azurecli
    az vm list-vm-resize-options --resource-group myResourceGroup --name myVM --output table
    ```

2. Igény hello szerepel a Virtuálisgép-méretet, méretezze át a virtuális gép hello [az vm átméretezése](/cli/azure/vm#resize). a következő példa átméretezi hello hello nevű virtuális gép `myVM` toohello `Standard_DS3_v2` mérete:
   
    ```azurecli
    az vm resize --resource-group myResourceGroup --name myVM --size Standard_DS3_v2
    ```
   
    hello virtuális gép újraindul, a folyamat során. Hello az újraindítás után a meglévő operációs rendszer és az adatlemezek újra társítani. Bármi hello ideiglenes lemezen elvész.

3. Hello szükséges Virtuálisgép-méret nem szerepel a listában, ha szüksége van-e toofirst felszabadítani a virtuális gép és hello [az virtuális gép felszabadítása](/cli/azure/vm#deallocate). Ez a folyamat lehetővé teszi, hogy hello VM toothen átméretezett tooany méret érhető el, amely támogatja a régió hello, majd el. hello lépések felszabadítani, méretezze át, és indítsa el hello nevű virtuális gép `myVM` nevű hello erőforráscsoportban `myResourceGroup`:
   
    ```azurecli
    az vm deallocate --resource-group myResourceGroup --name myVM
    az vm resize --resource-group myResourceGroup --name myVM --size Standard_DS3_v2
    az vm start --resource-group myResourceGroup --name myVM
    ```
   
   > [!WARNING]
   > Virtuális gép hello felszabadítása is dinamikus IP-címek hozzárendelése a virtuális gép toohello kiadását. az operációs rendszer hello és adatlemezek nem érintettek.

## <a name="next-steps"></a>Következő lépések
További méretezhetőséget, a virtuális gép több példányának futtatása, és kiterjesztése. További információkért lásd: [automatikus méretezése a virtuálisgép-méretezési csoportban lévő Linux-gépek][scale-set]. 

<!-- links -->
[boot-diagnostics]: https://azure.microsoft.com/en-us/blog/boot-diagnostics-for-virtual-machines-v2/
[scale-set]: ../../virtual-machine-scale-sets/virtual-machine-scale-sets-linux-autoscale.md 
[vm-sizes]:sizes.md
