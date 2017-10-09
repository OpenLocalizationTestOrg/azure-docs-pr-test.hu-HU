---
title: "PowerShell-példák aaaAzure |} Microsoft Docs"
description: "Az Azure PowerShell-példák"
services: virtual-network
documentationcenter: virtual-network
author: georgewallace
manager: timlt
editor: tysonn
tags: 
ms.assetid: 
ms.service: virtual-network
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: 
ms.workload: infrastructure
ms.date: 05/24/2017
ms.author: georgewallace
ms.openlocfilehash: 130a6e755691b46a9549cad5acaa5bde4fe38e95
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="azure-powershell-samples-for-networking"></a>Hálózat az Azure PowerShell-példák

hello következő táblázat tartalmazza az Azure PowerShell használatával készített hivatkozások tooscripts.

| | |
|-|-|
|**Azure-erőforrások közötti kapcsolat**||
| [A többrétegű alkalmazások virtuális hálózat létrehozása](./scripts/virtual-network-powershell-sample-multi-tier-application.md?toc=%2fazure%2fnetworking%2ftoc.json) | Előtér- és alhálózatok hoz létre a virtuális hálózat. Forgalmi toohello előtér-alhálózat korlátozott tooHTTP, a forgalom toohello közben háttér-alhálózat korlátozott tooSQL, 1433-as port. |
| [A két partner virtuális hálózatok](./scripts/virtual-network-powershell-sample-peer-two-virtual-networks.md?toc=%2fazure%2fnetworking%2ftoc.json) | Hoz létre, és csatlakozik a hello két virtuális hálózat ugyanabban a régióban. |
| [A hálózati virtuális készülék-útvonal forgalmát](./scripts/virtual-network-powershell-sample-route-traffic-through-nva.md?toc=%2fazure%2fnetworking%2ftoc.json) | Hoz létre egy virtuális hálózati előtér- és alhálózatok és virtuális gép, amely képes tooroute forgalom hello két alhálózat között. |
| [Bejövő és kimenő virtuális gép hálózati forgalom szűrésére](./scripts/virtual-network-powershell-filter-network-traffic.md?toc=%2fazure%2fnetworking%2ftoc.json) | Előtér- és alhálózatok hoz létre a virtuális hálózat. Bejövő hálózati forgalom toohello előtér-alhálózat, korlátozott tooHTTP és a HTTPS... Kimenő forgalom toohello Internet hello háttér-alhálózatból nem engedélyezett. |
|**Terheléselosztás és a forgalom iránya betöltése**||
| [Egyenleg forgalom tooVMs a magas rendelkezésre állású betöltése](./scripts/load-balancer-windows-powershell-sample-nlb.md?toc=%2fazure%2fnetworking%2ftoc.json) | Több magas rendelkezésre állású virtuális gépek és az elosztott terhelésű konfigurációs hoz létre. |
| [Terhelésének elosztása több webhely virtuális gépeken](./scripts/load-balancer-windows-powershell-load-balance-multiple-websites-vm.md?toc=%2fazure%2fnetworking%2ftoc.json) | Hoz létre két virtuális gépek több IP-konfigurációk, illesztett tooan Azure rendelkezésre állási csoport, az Azure Load Balancer keresztül érhető el. |
| [A magas rendelkezésre állásának különféle régiókban közvetlen forgalom](./scripts/traffic-manager-powershell-websites-high-availability.md?toc=%2fazure%2fnetworking%2ftoc.json) |  Két app service-csomagokról, a két web apps, a traffic manager-profil és a két traffic manager-végpont létrehozása |
| | |
