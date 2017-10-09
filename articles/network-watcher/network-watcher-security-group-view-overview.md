---
title: "az Azure hálózati figyelőt aaaIntroduction toosecurity csoport nézetében |} Microsoft Docs"
description: "Ezen a lapon hello hálózati figyelőt biztonsági nézet képességek áttekintése"
services: network-watcher
documentationcenter: na
author: georgewallace
manager: timlt
editor: 
ms.assetid: ad27ab85-9d84-4759-b2b9-e861ef8ea8d8
ms.service: network-watcher
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 04/26/2017
ms.author: gwallace
ms.openlocfilehash: c2f6dbbffd0aedbb9db4b69d1758f2e66dd7abb8
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="introduction-toonetwork-security-group-view-in-azure-network-watcher"></a>Bevezetés toonetwork biztonsági csoport megtekintése az Azure hálózati figyelőt

Hálózati biztonsági csoportok társított alhálózati szinten, vagy egy hálózati adapter szintjén. Ha tartozó alhálózati szinten, tooall hello Virtuálisgép-példány hello alhálózati érvényes. Hálózati biztonsági csoport megtekintése összes hello beállított NSG-ket és egy virtuális gép hello konfigurációs betekintést biztosít a hálózati és alhálózati szinten társított szabályokat adja vissza. Továbbá a hello hatékony biztonsági szabályok visszaadott minden hello hálózati adapterek virtuális gépen. Hálózati biztonsági csoport használatával nézet felmérheti a virtuális gépek, a hálózati biztonsági réseket, például a megnyitott portok. Ha a hálózati biztonsági csoport a várt módon működik alapján is ellenőrzéséhez egy [hello összehasonlítása konfigurálva és hatékony biztonsági szabályok hello](network-watcher-nsg-auditing-powershell.md).

Több kiterjesztett használati eset biztonsági, megfelelőségi és naplózási van. Biztonsági irányításhoz modellként előíró meghatározott biztonsági szabályok adhat meg a szervezetében. Egy rendszeres megfelelőségi naplózási programozott módon valósítható hello hatékony szabályok hello előíró szabályokkal összehasonlítva az egyes hello virtuális gépeket a hálózaton.

Hello a hatályos, a alhálózat, a hálózati illesztő és az alapértelmezett portál szabályok vannak osztva. Ez hello szabálya tooa virtuális géppé egyszerű nézetét jeleníti meg. A Letöltés gombra kerül a tooeasily hello lapon függetlenül minden hello biztonsági szabályok letöltése CSV-fájlba.

![Biztonsági csoport megtekintése][1]

Szabályok választhatók ki, és tooshow hello hálózati biztonsági csoport és a forrás és a cél-előtagok megnyílik egy új panelen. Ezen a panelen navigálhat közvetlenül toohello hálózati biztonsági csoport erőforrás.

![Részletezési][2]

### <a name="next-steps"></a>Következő lépések

Megtudhatja, hogyan tooaudit a hálózati biztonsági csoport beállításainak ellátogatva [naplózási hálózati biztonsági csoport beállításait a PowerShell használatával](network-watcher-nsg-auditing-powershell.md)

[1]: ./media/network-watcher-security-group-view-overview/securitygroupview.png
[2]: ./media/network-watcher-security-group-view-overview/figure1.png









