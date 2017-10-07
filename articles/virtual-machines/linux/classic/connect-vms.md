---
title: "Linux virtuális gépek aaaConnect felhőszolgáltatásban |} Microsoft Docs"
description: "Linux virtuális gépek hello klasszikus telepítési modell tooan Azure-felhőszolgáltatásban vagy a virtuális hálózathoz csatlakozzon."
services: virtual-machines-linux
documentationcenter: 
author: cynthn
manager: timlt
editor: 
tags: azure-service-management
ms.assetid: 2fd23055-6b34-4ef0-88a8-fc19e32fb3c9
ms.service: virtual-machines-linux
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-linux
ms.devlang: na
ms.topic: article
ms.date: 06/06/2017
ms.author: cynthn
ms.openlocfilehash: 323baf04390d53ffb2810e24a24d175c08e60cd7
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="connect-linux-virtual-machines-created-with-hello-classic-deployment-model-with-a-virtual-network-or-cloud-service"></a>Csatlakozás egy virtuális hálózat vagy a felhőalapú szolgáltatással hello klasszikus telepítési modellel létrehozott Linux virtuális gépek
> [!IMPORTANT]
> Azure az erőforrások létrehozására és kezelésére két különböző üzembe helyezési modellel rendelkezik: [Resource Manager és klasszikus](../../../resource-manager-deployment-model.md). Ez a cikk hello klasszikus telepítési modell használatát bemutatja. A Microsoft azt javasolja, hogy az új telepítések esetén hello Resource Manager modellt használja.

Hello klasszikus telepítési modellel létrehozott Linux virtuális gépek felhőszolgáltatásban kerülnek. hello felhőalapú szolgáltatás úgy működik, mint egy tárolót, és egy egyedi nyilvános DNS-nevet, egy nyilvános IP-cím és végpontok tooaccess hello virtuális gép készlete biztosít hello interneten keresztül. hello felhőszolgáltatás lehet egy virtuális hálózatban, de ez nem követelmény. Emellett [csatlakozás Windows virtuális gépek virtuális hálózati vagy felhőalapú szolgáltatásként](../../windows/classic/connect-vms.md?toc=%2fazure%2fvirtual-machines%2fwindows%2fclassic%2ftoc.json).

Ha egy felhőalapú szolgáltatás nem része virtuális hálózatnak, hívott egy *önálló* felhőalapú szolgáltatás. hello virtuális gépek önálló felhőalapú szolgáltatásként kommunikálnak más virtuális gépekkel használatával hello más virtuális gépek nyilvános DNS-neveit, és hello forgalom választ hello interneten keresztül. Ha egy virtuális hálózatban hello virtuális gépek egy felhőalapú szolgáltatás abban, hogy a felhőszolgáltatás hello virtuális hálózat más virtuális gépek kommunikálhatnak összes forgalom küldése hello interneten keresztül nélkül.

Ha a virtuális gépek hello azonos önálló felhőalapú szolgáltatás, továbbra is használhatja terheléselosztás és a rendelkezésre állási készletek. További információkért lásd: [terheléselosztási virtuális gépek](../../virtual-machines-linux-load-balance.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) és [hello virtuális gépek rendelkezésre állásának kezelése](../manage-availability.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json). Azonban nem hello virtuális gépek alhálózatok rendszerezése és önálló felhőalapú szolgáltatás tooyour helyszíni hálózatot. Íme egy példa:

[!INCLUDE [virtual-machines-common-classic-connect-vms](../../../../includes/virtual-machines-common-classic-connect-vms.md)]

## <a name="next-steps"></a>Következő lépések
Miután létrehozott egy virtuális gépet, érdemes egy túl[hozzá adatlemezt](attach-disk.md) , a szolgáltatások és a munkafolyamatok a hely toostore adatokkal rendelkeznek.
