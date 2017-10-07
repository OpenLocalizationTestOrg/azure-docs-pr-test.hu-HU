---
title: "Azure hálózati figyelőt - Azure-portálon aaaCheck kapcsolatot |} Microsoft Docs"
description: "Ez a lap azt ismerteti, hogyan toouse kapcsolatot ellenőrzi a hálózati figyelőt hello Azure-portál használatával"
services: network-watcher
documentationcenter: na
author: georgewallace
manager: timlt
editor: 
ms.service: network-watcher
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 08/03/2017
ms.author: gwallace
ms.openlocfilehash: ef6ecccd688f06f70003a5b59771c15bcbe8f3e2
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="check-connectivity-with-azure-network-watcher-using-hello-azure-portal"></a>Ellenőrizze a kapcsolatot az Azure hálózati figyelőt hello Azure-portál használatával

> [!div class="op_single_selector"]
> - [Portál](network-watcher-connectivity-portal.md)
> - [PowerShell](network-watcher-connectivity-powershell.md)
> - [CLI 2.0](network-watcher-connectivity-cli.md)
> - [Az Azure REST API-n](network-watcher-connectivity-rest.md)

Ismerje meg, hogyan hozhatók létre a toouse kapcsolat tooverify, ha egy virtuális gép tooa megadott végpont a közvetlen TCP-kapcsolatot.

## <a name="before-you-begin"></a>Előkészületek

Ez a cikk feltételezi, hogy rendelkezik-e a következő erőforrások hello:

* Egy példány toocheck kapcsolat kívánt hálózati figyelőt hello régióban.

* Virtuális gépek toocheck kapcsolattal.

> [!IMPORTANT]
> Kapcsolat ellenőrzése van szükség a virtuálisgép-bővítmény `AzureNetworkWatcherExtension`. A virtuális gép Windows hello-bővítmény telepítése a Microsoft [a Windows Azure hálózati figyelő ügynök virtuálisgép-bővítmény](../virtual-machines/windows/extensions-nwa.md) és a Linux virtuális gép helyezést [Azure hálózati figyelő ügynök virtuálisgép-bővítmény Linux](../virtual-machines/linux/extensions-nwa.md).

## <a name="check-connectivity-tooa-virtual-machine"></a>Ellenőrizze a kapcsolat tooa virtuális gép

Ebben a példában kapcsolat tooa cél virtuális gép ellenőrzi a 80-as porton keresztül.

Nyissa meg a tooyour hálózati figyelőt, és kattintson a **kapcsolat ellenőrzése (előzetes verzió)**. Válassza ki a hello virtuális gép toocheck közötti kapcsolatot. A hello **cél** rész válassza **válasszon ki egy virtuális gépet** és hello megfelelő virtuális gép és a port tootest kiválasztása.

Miután rákattintott **ellenőrizze**, megadott hello port hello virtuális gépek közötti hello kapcsolatot ellenőrzi. Hello példában hello cél virtuális gép nem érhető el, útválasztók ugrásainak listája látható.

![A virtuális gép kapcsolat eredmények ellenőrzése][1]

## <a name="check-remote-endpoint-connectivity"></a>Ellenőrizze a kapcsolatot a távoli végpont

toocheck hello kapcsolat és a késés tooa távoli végpont, válassza a hello **adja meg manuálisan** hello választógomb **cél** szakaszt, bemeneti hello URL-cím és hello port, és kattintson **ellenőrzése** .  Például a webhelyek és a tárolási végpontok távoli végpontok szolgál.

![A webhely csatlakozási eredmények ellenőrzése][2]

## <a name="next-steps"></a>Következő lépések

Ismerje meg, hogyan tooautomate csomagrögzítéseket virtuális gép riasztások megtekintésével [riasztási kiváltott csomagrögzítéssel létrehozása](network-watcher-alert-triggered-packet-capture.md)

Keresése, ha bizonyos adatforgalom engedélyezett a virtuális gép kívül vagy belül ellátogatva [ellenőrizze IP folyamat ellenőrzése](network-watcher-check-ip-flow-verify-portal.md)

[1]: ./media/network-watcher-connectivity-portal/figure1.png
[2]: ./media/network-watcher-connectivity-portal/figure2.png
