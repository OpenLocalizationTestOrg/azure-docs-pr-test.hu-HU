---
title: a Windows Azure-ban a MongoDB aaaInstall |} Microsoft Docs
description: "Ismerje meg, hogyan tooinstall egy Azure virtuális gépen MongoDB hozza létre a Windows Server rendszert futtató hello klasszikus üzembe helyezési modellel."
services: virtual-machines-windows
documentationcenter: 
author: iainfoulds
manager: timlt
editor: tysonn
tags: azure-service-management
ms.assetid: 4095df41-bb69-4bbe-9c1c-70923b0d84ba
ms.service: virtual-machines-windows
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-windows
ms.devlang: na
ms.topic: article
ms.date: 03/07/2017
ms.author: iainfou
ms.openlocfilehash: aafd86b4c49c6a97ae8a1a2191ae375b6c47483a
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="install-mongodb-on-a-windows-vm-in-azure"></a>A MongoDB telepítése a Windows Azure-ban
> [!IMPORTANT]
> Azure az erőforrások létrehozására és kezelésére két különböző üzembe helyezési modellel rendelkezik: [Resource Manager és klasszikus](../../../resource-manager-deployment-model.md).  Ez a cikk hello klasszikus telepítési modell használatát bemutatja. A Microsoft azt javasolja, hogy az új telepítések esetén hello Resource Manager modellt használja. tooinstall és hello erőforrás-kezelő telepítési modellel MongoDB konfigurálása, lásd: [Ez a cikk](../install-mongodb.md).

[MongoDB] [ MongoDB] egy népszerű nyílt forráskódú, nagy teljesítményű NoSQL-adatbázis. Ez a cikk végigvezeti a Windows Server virtuális gép (VM) hello lépésein [Azure-portálon][AzurePortal]. Kell létrehozni és csatolni a adatok lemez toohello VM telepítése és konfigurálása a MongoDB előtt. Ha egy meglévő virtuális Gépen, hogy szeretné-e toouse Azure-ban, de is ugorhat rögtön túl[telepítése és konfigurálása a MongoDB](#install-and-run-mongodb-on-the-virtual-machine).

## <a name="create-a-virtual-machine-running-windows-server"></a>Windows Servert futtató virtuális gép létrehozása
Hajtsa végre az ezen utasításokat toocreate egy virtuális gépet.

[!INCLUDE [virtual-machines-create-WindowsVM](../../../../includes/virtual-machines-create-windowsvm.md)]

> [!NOTE]
> Végpont hozzáadása a mongodb-protokolltámogatással hello virtuális gép létrehozása során, és konfigurálja az alábbiak szerint: nevezze el, mint a **Mongo**, használjon **TCP** , hello protokoll, és állítsa be mindkét hello nyilvános vagy privát portokkal túl**27017**.
>
>

## <a name="attach-a-data-disk"></a>Adatlemez csatolása
tooprovide tárolási hello virtuális gép esetén adatlemezzel és majd inicializálnia, hogy a Windows használni tudja. Ha már rendelkezik adatlemezt, csatolhat a meglévő lemezt, vagy csatolhat egy üres lemez.

[!INCLUDE [howto-attach-disk-windows-linux](../../../../includes/howto-attach-disk-windows-linux.md)]

Hello lemez inicializálása, lásd: "How to: a Windows Server új adatlemezt inicializálása" a [hogyan tooattach az adatok lemezre tooa Windows rendszerű virtuális gép](attach-disk.md).

## <a name="install-and-run-mongodb-on-hello-virtual-machine"></a>Telepítése és futtatása a MongoDB hello virtuális gépen
[!INCLUDE [install-and-run-mongo-on-win2k8-vm](../../../../includes/install-and-run-mongo-on-win2k8-vm.md)]

## <a name="summary"></a>Összefoglalás
Ebben az oktatóanyagban megtanulta, hogyan toocreate egy Windows Server rendszerű virtuális gép távolról csatlakozzon tooit, és adatlemezt csatolni.  Azt is megtanulta, hogyan tooinstall, és konfigurálja a MongoDB hello Windows-alapú virtuális gépen. Most már elérheti az MongoDB hello Windows-alapú virtuális gépen, a következő speciális hello témakörei hello [MongoDB dokumentációt][MongoDocs].

[MongoDocs]: http://docs.mongodb.org/manual/
[MongoDB]: http://www.mongodb.org/
[AzurePortal]: https://portal.azure.com/

