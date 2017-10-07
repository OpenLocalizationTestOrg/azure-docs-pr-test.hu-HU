---
title: "végpontok a klasszikus Linux virtuális gép mentése aaaSet |} Microsoft Docs"
description: "Ismerje meg a végpontok mentése tooset rendelkező Linux virtuális gépek Azure-ban az Azure klasszikus portál tooallow kommunikációs hello Linux virtuális"
services: virtual-machines-linux
documentationcenter: 
author: cynthn
manager: timlt
editor: 
tags: azure-service-management
ms.assetid: f3749738-1109-4a1d-8635-40e4bd220e91
ms.service: virtual-machines-linux
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-linux
ms.devlang: na
ms.topic: article
ms.date: 06/09/2017
ms.author: cynthn
ms.openlocfilehash: 1c959d10dd1e20228fa4a20e1cc0205c1d12f185
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="how-tooset-up-endpoints-on-a-linux-classic-virtual-machine-in-azure"></a>Hogyan tooset be Linux klasszikus virtuális gépre az Azure-végpontok
Minden Linux virtuális gépek az Azure-ban hello klasszikus üzembe helyezési modellel létrehozott automatikusan kommunikálhatnak más virtuális gépekkel hello a magánhálózat-csatornán keresztül ugyanaz a felhőalapú szolgáltatás, vagy a virtuális hálózati. Hello Internet vagy egyéb virtuális hálózaton lévő számítógépek azonban végpontok toodirect hello bejövő hálózati forgalom tooa virtuális gép szükséges. Ez a cikk érhető el is [Windows virtuális gépek](../../windows/classic/setup-endpoints.md?toc=%2fazure%2fvirtual-machines%2fwindows%2fclassic%2ftoc.json).

> [!IMPORTANT]
> Azure az erőforrások létrehozására és kezelésére két különböző üzembe helyezési modellel rendelkezik: [Resource Manager és klasszikus](../../../resource-manager-deployment-model.md). Ez a cikk hello klasszikus telepítési modell használatát bemutatja. A Microsoft azt javasolja, hogy az új telepítések esetén hello Resource Manager modellt használja.

A hello **erőforrás-kezelő** üzembe helyezési modelljével végpontok használatával konfigurálhatók **hálózati biztonsági csoportokkal (NSG-k)**. További információkért lásd: [portok, valamint a végpontok](../nsg-quickstart.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).

Hello Azure-portálon, a Linux virtuális gép létrehozásakor a végpont a Secure Shell (SSH) általában, automatikusan létrejön. További végpontok hello virtuális gép létrehozása során vagy azt követően szükség szerint konfigurálhatja.

[!INCLUDE [virtual-machines-common-classic-setup-endpoints](../../../../includes/virtual-machines-common-classic-setup-endpoints.md)]

## <a name="next-steps"></a>Következő lépések
* Hello segítségével is létrehozhat egy virtuális gép végpontjának [Azure parancssori felület](https://docs.microsoft.com/cli/azure/get-started-with-az-cli2). Futtassa a hello **azure virtuálisgép-végpont létrehozása** parancsot.
* Ha létrehozott egy virtuális gép hello Resource Manager üzembe helyezési modellel, használhatja az Azure parancssori felület hello erőforrás-kezelő módban túl[hálózati biztonsági csoportok létrehozása](../../../virtual-network/virtual-networks-create-nsg-arm-cli.md) toocontrol forgalom toohello virtuális gép.
