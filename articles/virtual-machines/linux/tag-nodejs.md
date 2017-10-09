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
ms.openlocfilehash: 0e99ea66a87b7e00eb21a2f72dd2bce8673778dd
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="how-tootag-a-linux-virtual-machine-in-azure"></a>Hogyan tootag egy Linux virtuális gép az Azure-ban
Ez a cikk ismerteti a különböző módokon tootag Linux virtuális gépek Azure-ban keresztül hello Resource Manager üzembe helyezési modellben. Címke található a felhasználó által definiált kulcs/érték párok, amely lehet tenni közvetlenül egy erőforrás vagy egy erőforráscsoportot. Azure jelenleg legfeljebb too15 címkék erőforrás pedig erőforráscsoportban támogat. Címke lehet, hogy helyezni egy erőforrás létrehozása a hello időpontjában, vagy hozzá tooan meglévő erőforrást. Ne feledje, az erőforrások hello Resource Manager üzembe helyezési modellben csak létrehozott címkék használhatók.

[!INCLUDE [virtual-machines-common-tag](../../../includes/virtual-machines-common-tag.md)]

## <a name="tagging-with-azure-cli"></a>Az Azure CLI-címkézés
toobegin, [telepítése és konfigurálása az Azure parancssori felület hello](../../xplat-cli-azure-resource-manager.md) , és győződjön meg arról, hogy az erőforrás-kezelő módban (`azure config mode arm`).

Az összes tulajdonság egy adott virtuális gép, beleértve a hello címkék, ezzel a paranccsal tekintheti meg:

        azure vm show -g MyResourceGroup -n MyTestVM

tooadd hello Azure CLI segítségével új virtuális gép címke, hello használható `azure vm set` parancs együtt hello tag paraméter **-t**:

        azure vm set -g MyResourceGroup -n MyTestVM –t myNewTagName1=myNewTagValue1;myNewTagName2=myNewTagValue2

minden tooremove címkéket, használhatja a hello **-T** hello paraméterének `azure vm set` parancs.

        azure vm set – g MyResourceGroup –n MyTestVM -T


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
