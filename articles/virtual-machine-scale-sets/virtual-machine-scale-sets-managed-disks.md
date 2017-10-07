---
title: "aaaUsing felügyelt lemezt az Azure virtuális gép méretezési csoportok |} Microsoft Docs"
description: "Ismerje meg, miért és hogyan toouse kezeli-e a lemezek, amelyek a virtuálisgép-méretezési csoportok"
services: virtual-machine-scale-sets
documentationcenter: 
author: gatneil
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: 76ac7fd7-2e05-4762-88ca-3b499e87906e
ms.service: virtual-machine-scale-sets
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 6/01/2017
ms.author: negat
ms.openlocfilehash: 0e2a21e9f8b114ae1c8b81e1e6124621366f5643
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="azure-vm-scale-sets-and-managed-disks"></a>Azure-beli virtuálisgép-méretezési csoportok és felügyelt lemezek

Az Azure-beli [virtuálisgép-méretezési csoportok](/azure/virtual-machine-scale-sets/) támogatják a felügyelt lemezekkel rendelkező virtuális gépeket. A felügyelt lemezek méretezési csoportokkal való használatának több előnye van, például:

* Már nincs szüksége toopre-létrehozásához és kezeléséhez fiókok – toostore hello OS tárolólemezek hello méretezési virtuális gépek számára.

* Felügyelt adatok lemezek toohello méretezési csatolható.

* Felügyelt lemezzel a méretezési csoportok kapacitása akár platformlemezkép-alapú 1000 vagy 100 egyéni lemezképen alapuló virtuális gép lehet.

## <a name="get-started"></a>Bevezetés

Egyszerűen felügyelt lemezes méretezési csoportok használatába tooget toodeploy a hello Azure-portálon. További információkért tekintse meg [ezt a cikket](./virtual-machine-scale-sets-portal-create.md). Egy másik egyszerűen lépések tooget toouse [Azure CLI 2.0](https://docs.microsoft.com/cli/azure/install-az-cli2) toodeploy egy méretezési csoportban. hello következő példa bemutatja, hogyan toocreate egy Ubuntu alapuló méretezési készletben 10 olyan 50 GB-os és 100 GB-os adatokat tartalmazó virtuális:

```azurecli
az group create -l southcentralus -n dsktest
az vmss create -g dsktest -n dskvmss --image ubuntults --instance-count 10 --data-disk-sizes-gb 50 100
```

Azt is megteheti, hogy hello hely sikerült [Azure gyors üzembe helyezési sablonokat GitHub-tárház](https://github.com/Azure/azure-quickstart-templates) tartalmazó mappák `vmss` toosee előzetesen elkészített példák a sablonok, amelyek telepítése a méretezési készlet. mely sablonok már használja a felügyelt lemezek tootell, olvassa el a túl[ebben a listában](https://github.com/Azure/azure-quickstart-templates/blob/master/managed-disk-support-list.md).

## <a name="next-steps"></a>Következő lépések

A felügyelt lemezekkel kapcsolatban [ebben a cikkben](../virtual-machines/windows/managed-disks-overview.md) talál további információt.

toosee hogyan egy Resource Manager sablon tooprovision skálázási készletekben rendelkező tooconvert által kezelt lemezeken, lásd: [Ez a cikk](./virtual-machine-scale-sets-convert-template-to-md.md). hello azonos módosítások toohello Resource Manager-sablonok alkalmazása toohello Azure REST API-t is.

toolearn több felügyelt adatlemezek méretezési csoportok használatáról lásd: [Ez a cikk](./virtual-machine-scale-sets-attached-disks.md).

túl nagy méretű méretezési csoportok használata toobegin tekintse meg[Ez a cikk](./virtual-machine-scale-sets-placement-groups.md).


