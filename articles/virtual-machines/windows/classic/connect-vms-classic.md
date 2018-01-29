---
title: "Csatlakozás a Windows virtuális gépek felhőszolgáltatásban |} Microsoft Docs"
description: "Csatlakozás a Windows virtuális gépek a klasszikus üzembe helyezési modellt az Azure-felhőszolgáltatásban vagy a virtuális hálózat létrehozva."
services: virtual-machines-windows
documentationcenter: 
author: cynthn
manager: timlt
editor: 
tags: azure-service-management
ROBOTS: NOINDEX
ms.assetid: c1cbc802-4352-4d2e-9e49-4ccbd955324b
ms.service: virtual-machines-windows
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-windows
ms.devlang: na
ms.topic: article
ms.date: 06/06/2017
ms.author: cynthn
ms.openlocfilehash: f30c111afa31db9bec16ba5a9ad46ea20d64f285
ms.sourcegitcommit: 176c575aea7602682afd6214880aad0be6167c52
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 01/09/2018
---
# <a name="connect-windows-virtual-machines-created-with-the-classic-deployment-model-with-a-virtual-network-or-cloud-service"></a>Csatlakozás a Windows virtuális gépek létrehozása a klasszikus üzembe helyezési modellel egy virtuális hálózat vagy a felhőalapú szolgáltatáshoz
> [!IMPORTANT]
> Azure az erőforrások létrehozására és kezelésére két különböző üzembe helyezési modellel rendelkezik: [Resource Manager és klasszikus](../../../resource-manager-deployment-model.md). Ez a cikk a klasszikus telepítési modell használatát bemutatja. A Microsoft azt javasolja, hogy az új telepítések esetén a Resource Manager modellt használja.

A klasszikus üzembe helyezési modellel létrehozott Windows virtuális gépek felhőszolgáltatásban kerülnek. A felhőalapú szolgáltatás úgy működik, mint egy tárolót, és egy egyedi nyilvános DNS-nevet, egy nyilvános IP-cím és az interneten keresztül a virtuális gép eléréséhez végpontok készlete. A felhőszolgáltatás lehet egy virtuális hálózatban, de ez nem követelmény.

Ha egy felhőalapú szolgáltatás nem része virtuális hálózatnak, hívott egy *önálló* felhőalapú szolgáltatás. A virtuális gépek önálló felhőalapú szolgáltatásként kommunikálnak más virtuális gépekkel más virtuális gépeket nyilvános DNS-nevek használatával, és a forgalom halad az interneten keresztül. Ha egy virtuális hálózatban a virtuális gépek egy felhőalapú szolgáltatás abban, hogy a felhőalapú szolgáltatás képes kommunikálni a virtuális hálózat más virtuális gépek összes forgalom küldése az interneten keresztül nélkül.

Ha a virtuális gépeket helyez az azonos önálló felhőalapú szolgáltatás, terheléselosztás és a rendelkezésre állási csoportok továbbra is használhatja. További információkért lásd: [terheléselosztási virtuális gépek](../../virtual-machines-windows-load-balance.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) és [virtuális gépek rendelkezésre állásának kezelése](../../virtual-machines-windows-manage-availability.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json). Azonban nem a virtuális gépek alhálózatok rendszerezése vagy önálló felhőalapú szolgáltatásként csatlakozni a helyi hálózatra. Íme egy példa:

[!INCLUDE [virtual-machines-common-classic-connect-vms](../../../../includes/virtual-machines-common-classic-connect-vms.md)]

## <a name="next-steps"></a>További lépések
A virtuális gép létrehozása után már jó ötlet [hozzá adatlemezt](attach-disk.md) , a szolgáltatások és a munkafolyamatok jogosult az adatok tárolási helyét.
