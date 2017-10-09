---
title: "parancssori felület minták aaaAzure |} Microsoft Docs"
description: "Az Azure CLI-minták"
services: virtual-network
documentationcenter: virtual-network
author: KumudD
manager: timlt
editor: tysonn
tags: 
ms.assetid: 
ms.service: virtual-network
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: 
ms.workload: infrastructure
ms.date: 04/25/2017
ms.author: kumud
ms.openlocfilehash: 8001b7e72480cfd0122325f7fb81c32aaad072d3
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="azure-cli-samples-for-networking"></a>Hálózati Azure CLI-minták

hello alábbi táblázat tartalmaz hivatkozásokat toobash parancsfájlokat hello Azure parancssori felület használatával készített.

| | |
|-|-|
|**Azure-erőforrások közötti kapcsolat**||
| [A többrétegű alkalmazások virtuális hálózat létrehozása](./scripts/virtual-network-cli-sample-multi-tier-application.md?toc=%2fazure%2fnetworking%2ftoc.json) | Előtér- és alhálózatok hoz létre a virtuális hálózat. Forgalmi toohello előtér-alhálózat korlátozott tooHTTP és SSH, a forgalom toohello közben háttér-alhálózat korlátozott tooMySQL, port 3306. |
| [A két partner virtuális hálózatok](./scripts/virtual-network-cli-sample-peer-two-virtual-networks.md?toc=%2fazure%2fnetworking%2ftoc.json) | Hoz létre, és csatlakozik a hello két virtuális hálózat ugyanabban a régióban. |
| [A hálózati virtuális készülék-útvonal forgalmát](./scripts/virtual-network-cli-sample-route-traffic-through-nva.md?toc=%2fazure%2fnetworking%2ftoc.json) | Hoz létre egy virtuális hálózati előtér- és alhálózatok és virtuális gép, amely képes tooroute forgalom hello két alhálózat között. |
| [Bejövő és kimenő virtuális gép hálózati forgalom szűrésére](./scripts/virtual-network-filter-network-traffic.md?toc=%2fazure%2fnetworking%2ftoc.json) | Előtér- és alhálózatok hoz létre a virtuális hálózat. Bejövő hálózati forgalom toohello előtér-alhálózat korlátozva tooHTTP, HTTPS és SSH. Kimenő forgalom toohello Internet hello háttér-alhálózatból nem engedélyezett. |
|**Terheléselosztás és a forgalom iránya betöltése**||
| [Egyenleg forgalom tooVMs a magas rendelkezésre állású betöltése](./scripts/load-balancer-linux-cli-sample-nlb.md?toc=%2fazure%2fnetworking%2ftoc.json) | Több magas rendelkezésre állású virtuális gépek és az elosztott terhelésű konfigurációs hoz létre. |
| [Terhelésének elosztása több webhely virtuális gépeken](./scripts/load-balancer-linux-cli-load-balance-multiple-websites-vm.md?toc=%2fazure%2fnetworking%2ftoc.json) | Hoz létre két virtuális gépek több IP-konfigurációk, illesztett tooan Azure rendelkezésre állási csoport, az Azure Load Balancer keresztül érhető el. |
| [A magas rendelkezésre állásának különféle régiókban közvetlen forgalom](./scripts/traffic-manager-cli-websites-high-availability.md?toc=%2fazure%2fnetworking%2ftoc.json) |  Két app service-csomagokról, a két web apps, a traffic manager-profil és a két traffic manager-végpont létrehozása |
| | |
