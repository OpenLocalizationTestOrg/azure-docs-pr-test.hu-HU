---
title: "Linux virtuális gép hello Azure CLI 1.0 és aaaHow tooresize |} Microsoft Docs"
description: "Hogyan tooscale fel vagy le egy Linux virtuális gépet, módosításával méretezési hello Virtuálisgép-méretet."
services: virtual-machines-linux
documentationcenter: na
author: mikewasson
manager: timlt
editor: 
tags: 
ms.assetid: 
ms.service: virtual-machines-linux
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 05/16/2016
ms.author: mwasson
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 43dd955dc2f2dd9d1b2da07ecbfbf2459bcaa4d2
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="resize-a-linux-vm-with-azure-cli-10"></a>Linux virtuális gép az Azure CLI 1.0 és átméretezése

## <a name="overview"></a>Áttekintés

(VM) virtuális gép kiépítése után méretezheti hello VM felfelé vagy lefelé hello módosításával [Virtuálisgép-méretet][vm-sizes]. Néhány esetben először hello VM kell felszabadítani. Ez akkor fordulhat elő, ha hello új mérete nem érhető el a virtuális gép hello üzemeltető hello hardver fürt.

Ez a cikk bemutatja, hogyan tooresize a Linux virtuális gép hello [Azure CLI][azure-cli].

[!INCLUDE [learn-about-deployment-models](../../../includes/learn-about-deployment-models-rm-include.md)]

## <a name="cli-versions-toocomplete-hello-task"></a>Parancssori felület verziók toocomplete hello feladat
Hello feladat a következő parancssori felület verziók hello egyikével hajthatja végre:

- [Az Azure CLI 1.0](#resize-a-linux-vm) – a parancssori felületen hello klasszikus és resource management üzembe helyezési modellel (a cikk)
- [Az Azure CLI 2.0](change-vm-size.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) -a következő generációs CLI hello erőforrás felügyeleti telepítési modell


## <a name="resize-a-linux-vm"></a>Linux virtuális gép átméretezése
tooresize egy virtuális Gépet, hajtsa végre a lépéseket követve hello.

1. Futtassa a következő parancsot a CLI hello. Ez a parancs kilistázza hello Virtuálisgép-méretek elérhető hello hardver fürtön, amelyen hello virtuális gép található.
   
    ```azurecli
    azure vm sizes -g myResourceGroup --vm-name myVM
    ```
2. Ha hello szükséges méret szerepel, futtassa a következő parancs tooresize hello VM hello.
   
    ```azurecli
    azure vm set -g myResourceGroup --vm-size <new-vm-size> -n myVM  \
        --enable-boot-diagnostics
        --boot-diagnostics-storage-uri https://mystorageaccount.blob.core.windows.net/ 
    ```
   
    hello virtuális gép újraindul, a folyamat során. Hello az újraindítás után a meglévő operációs rendszer és az adatlemezek fogja képezni. Bármi hello ideiglenes lemezen elvesznek.
   
    Használjon hello `--enable-boot-diagnostics` beállítás lehetővé teszi, hogy [rendszerindítási diagnosztika][boot-diagnostics], toolog bármely hibák kapcsolódó toostartup.
3. Ha hello szükséges méret nem szerepel a listában, egyébként futtassa a következő parancsok toodeallocate hello VM, méretezze át, és indítsa újra hello VM hello.
   
    ```azurecli
    azure vm deallocate -g myResourceGroup myVM
    azure vm set -g myResourceGroup --vm-size <new-vm-size> -n myVM \
        --enable-boot-diagnostics --boot-diagnostics-storage-uri \
        https://mystorageaccount.blob.core.windows.net/ 
    azure vm start -g myResourceGroup myVM
    ```
   
   > [!WARNING]
   > Virtuális gép hello felszabadítása is dinamikus IP-címek hozzárendelése a virtuális gép toohello kiadását. az operációs rendszer hello és adatlemezek nem érintettek.
   > 
   > 

## <a name="next-steps"></a>Következő lépések
További méretezhetőséget, a virtuális gép több példányának futtatása, és kiterjesztése. További információkért lásd: [automatikus méretezése a virtuálisgép-méretezési csoportban lévő Linux-gépek][scale-set]. 

<!-- links -->

[azure-cli]:../../cli-install-nodejs.md
[boot-diagnostics]: https://azure.microsoft.com/en-us/blog/boot-diagnostics-for-virtual-machines-v2/
[scale-set]: ../../virtual-machine-scale-sets/virtual-machine-scale-sets-linux-autoscale.md 
[vm-sizes]:sizes.md
