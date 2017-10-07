---
title: "egy OpenSUSE virtuális gépen MySQL aaaInstall |} Microsoft Docs"
description: "Tooinstall MySQL az Azure-ban OpenSUSE Linux VMirtual géphez tudnivalók."
services: virtual-machines-linux
documentationcenter: 
author: cynthn
manager: timlt
editor: 
tags: azure-service-management
ms.assetid: 1594e10e-c314-455a-9efb-a89441de364b
ms.service: virtual-machines-linux
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-linux
ms.devlang: na
ms.topic: article
ms.date: 07/19/2016
ms.author: cynthn
ms.openlocfilehash: 8f96d29f29cb9c466dd7fdaf92b378783fdaacd6
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="install-mysql-on-a-virtual-machine-running-opensuse-linux-in-azure"></a>A MySQL telepítése Azure-ban működő, OpenSUSE Linux rendszerű virtuális gépen
[MySQL] [ MySQL] egy népszerű, nyílt forráskódú SQL-adatbázis. Az oktatóanyag bemutatja, hogyan toocreate OpenSUSE Linuxot futtató virtuális gép telepítse MySQL.

> [!IMPORTANT] 
> Azure az erőforrások létrehozására és kezelésére két különböző üzembe helyezési modellel rendelkezik: [Resource Manager és klasszikus](../../../resource-manager-deployment-model.md). Ez a cikk hello klasszikus telepítési modell használatát bemutatja. A Microsoft azt javasolja, hogy az új telepítések esetén hello Resource Manager modellt használja.

<br>

## <a name="create-a-virtual-machine-running-opensuse-linux"></a>OpenSUSE Linuxot futtató virtuális gép létrehozása
[!INCLUDE [create-and-configure-opensuse-vm-in-portal](../../../../includes/create-and-configure-opensuse-vm-in-portal.md)]

## <a name="install-and-run-mysql-on-hello-virtual-machine"></a>Telepítése és futtatása a MySQL hello virtuális gépen
[!INCLUDE [install-and-run-mysql-on-opensuse-vm](../../../../includes/install-and-run-mysql-on-opensuse-vm.md)]

## <a name="next-steps"></a>Következő lépések
MySQL kapcsolatos részletekért lásd: hello [MySQL dokumentáció][MySQLDocs].

[MySQLDocs]:http://dev.mysql.com/doc/index-topic.html
[MySQL]:http://www.mysql.com

