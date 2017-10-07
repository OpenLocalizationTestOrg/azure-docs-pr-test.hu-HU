---
title: "egy Azure Linux virtuális gép aaaHow tootag |} Microsoft Docs"
description: "További információk a címkézés egy Azure Linux virtuális gép létrehozása az Azure-ban hello Resource Manager üzembe helyezési modellben."
services: virtual-machines-linux
documentationcenter: 
author: mmccrory
manager: timlt
editor: tysonn
tags: azure-resource-manager
ms.assetid: ca0e17e5-d78e-42e6-9dad-c1e8f1c58027
ms.service: virtual-machines-linux
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure-services
ms.date: 02/28/2017
ms.author: memccror
ms.openlocfilehash: 456b226af4495c3b446cb79c99cf9494dde9fca5
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="how-tootag-a-linux-virtual-machine-in-azure"></a>Hogyan tootag egy Linux virtuális gép az Azure-ban
Ez a cikk ismerteti a különböző módokon tootag Linux virtuális gépek Azure-ban keresztül hello Resource Manager üzembe helyezési modellben. Címke található a felhasználó által definiált kulcs/érték párok, amely lehet tenni közvetlenül egy erőforrás vagy egy erőforráscsoportot. Azure jelenleg legfeljebb too15 címkék erőforrás pedig erőforráscsoportban támogat. Címke lehet, hogy helyezni egy erőforrás létrehozása a hello időpontjában, vagy hozzá tooan meglévő erőforrást. Ne feledje, az erőforrások hello Resource Manager üzembe helyezési modellben csak létrehozott címkék használhatók.

[!INCLUDE [virtual-machines-common-tag](../../../includes/virtual-machines-common-tag.md)]

## <a name="tagging-with-azure-cli"></a>Az Azure CLI-címkézés
toobegin, kell hello legújabb [Azure CLI 2.0 (előzetes verzió)](/cli/azure/install-az-cli2) telepítve, és bejelentkezett tooan Azure-fiók használatával [az bejelentkezési](/cli/azure/#login).

Is elvégezheti ezeket a lépéseket hello [Azure CLI 1.0](tag-nodejs.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).

Az összes tulajdonság egy adott virtuális gép, beleértve a hello címkék, ezzel a paranccsal tekintheti meg:

        az vm show --resource-group MyResourceGroup --name MyTestVM

tooadd hello Azure CLI segítségével új virtuális gép címke, hello használható `azure vm update` parancs együtt hello tag paraméter **--beállítása**:

        az vm update --resource-group MyResourceGroup --name MyTestVM –-set tags.myNewTagName1=myNewTagValue1 tags.myNewTagName2=myNewTagValue2

tooremove címkék hello használható **--eltávolítása** hello paraméterének `azure vm update` parancsot.

        az vm update –-resource-group MyResourceGroup –-name MyTestVM --remove tags.myNewTagName1


Most, hogy azt címkék tooour erőforrások az Azure parancssori felület alkalmazott, és a portál hello, most vessen egy pillantást hello használati részletek toosee hello címkék hello számlázási portálon.

[!INCLUDE [virtual-machines-common-tag-usage](../../../includes/virtual-machines-common-tag-usage.md)]

## <a name="next-steps"></a>Következő lépések
* toolearn az Azure-erőforrások, címkézéssel kapcsolatos további információkért lásd: [Azure Resource Manager áttekintése] [ Azure Resource Manager Overview] és [tooorganize címkék használata az Azure-erőforrások] [ Using Tags tooorganize your Azure Resources].
* toosee hogyan címkék segítségével kezelése az Azure-erőforrások, lásd: [ismertetése a Azure számlázási] [ Understanding your Azure Bill] és [betekintést nyerhet a Microsoft Azure erőforrás-felhasználás] [Gain insights into your Microsoft Azure resource consumption].

[Azure CLI environment]: ../../azure-resource-manager/xplat-cli-azure-resource-manager.md
[Azure Resource Manager Overview]: ../../azure-resource-manager/resource-group-overview.md
[Using Tags tooorganize your Azure Resources]: ../../azure-resource-manager/resource-group-using-tags.md
[Understanding your Azure Bill]: ../../billing/billing-understand-your-bill.md
[Gain insights into your Microsoft Azure resource consumption]: ../../billing/billing-usage-rate-card-overview.md
