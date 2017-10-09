---
title: "a klasszikus Linux virtuális gép használatával aaaCreate hello Azure CLI 1.0 |} Microsoft Docs"
description: "Ismerje meg, hogyan toocreate hello Azure CLI 1.0 használata Linux virtuális gépek hello klasszikus telepítési modell"
services: virtual-machines-linux
documentationcenter: 
author: iainfoulds
manager: timlt
editor: tysonn
tags: azure-service-management
ms.assetid: f8071a2e-ed91-4f77-87d9-519a37e5364f
ms.service: virtual-machines-linux
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-linux
ms.devlang: na
ms.topic: article
ms.date: 02/09/2017
ms.author: iainfou
ms.openlocfilehash: 5c3c54e5d1444a79e8e609c76d04a3a621c8d03f
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="how-toocreate-a-classic-linux-vm-with-hello-azure-cli-10"></a>Hogyan tooCreate rendelkező klasszikus Linux virtuális gépet a hello Azure CLI 1.0
> [!IMPORTANT] 
> Azure az erőforrások létrehozására és kezelésére két különböző üzembe helyezési modellel rendelkezik: [Resource Manager és klasszikus](../../../resource-manager-deployment-model.md). Ez a cikk hello klasszikus telepítési modell használatát bemutatja. A Microsoft azt javasolja, hogy az új telepítések esetén hello Resource Manager modellt használja. Hello Resource Manager-verziójáért lásd: [Itt](../create-cli-complete.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).

Ez a témakör ismerteti, hogyan toocreate egy Linux virtuális gép (VM) hello Azure CLI 1.0 használatával hello klasszikus telepítési modellt. A rendelkezésre álló hello Linux lemezkép használjuk **képek** az Azure-on. hello Azure CLI 1.0 parancsok konfigurációs beállításokkal, többek között a következő hello biztosítják:

* Hello VM tooa virtuális hálózati csatlakozás
* Hello VM tooan meglévő felhőalapú szolgáltatás hozzáadása
* Hello VM tooan meglévő tárfiók hozzáadása
* Hello VM tooan rendelkezésre állási csoport vagy a hely hozzáadása

> [!IMPORTANT]
> Ha azt szeretné, hogy a virtuális hálózat tooit állomásnév vagy állítsa be a létesítmények közötti kapcsolatok által közvetlenül is elérheti, ellenőrizze, hogy virtuális gép toouse hello virtuális hálózati meg hello virtuális gép létrehozásakor. A virtuális gép lehet konfigurált toojoin virtuális hálózat csak hello virtuális gép létrehozásakor. A virtuális hálózatokon, lásd: [Azure virtuális hálózat áttekintése](http://go.microsoft.com/fwlink/p/?LinkID=294063).
> 
> 

## <a name="how-toocreate-a-linux-vm-using-hello-classic-deployment-model"></a>Hogyan toocreate a Linux virtuális gép hello klasszikus telepítési modell
[!INCLUDE [virtual-machines-create-LinuxVM](../../../../includes/virtual-machines-create-linuxvm.md)]

