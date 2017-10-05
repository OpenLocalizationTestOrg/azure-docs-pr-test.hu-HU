---
title: "Azure DNS-sel más Azure-szolgáltatásokkal |} Microsoft Docs"
description: "Azure DNS használatával oldja meg a nevet az egyéb Azure-szolgáltatások ismertetése"
services: dns
documentationcenter: na
author: georgewallace
manager: timlt
editor: 
tags: azure dns
ms.assetid: e9b5eb94-7984-4640-9930-564bb9e82b78
ms.service: dns
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.custom: H1Hack27Feb2017
ms.workload: infrastructure-services
ms.date: 09/21/2016
ms.author: gwallace
ms.openlocfilehash: 5b95d5455c08d09a088c740e9df9605d3d7f719b
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/03/2017
---
# <a name="how-azure-dns-works-with-other-azure-services"></a>Azure DNS működése más Azure-szolgáltatásokkal

Az Azure DNS egy olyan üzemeltetett DNS felügyeleti és name névfeloldási szolgáltatás. Ez lehetővé teszi, hogy az egyéb alkalmazások és szolgáltatások telepítése a nyilvános DNS-nevek létrehozása az Azure-ban. Az egyéni tartomány létrehozása az Azure-szolgáltatások nevét más dolga, mint a szolgáltatás a megfelelő típusú rekord hozzáadása.

* Dinamikusan kiosztott IP-címeket létre kell hoznia egy DNS CNAME-rekordot, amely a DNS-neve, amely a szolgáltatás számára létrehozott Azure van leképezve. DNS-szabványokból, hogy az egy olyan CNAME rekordot a zóna felső pontja.
* Statikusan lefoglalt IP-címekhez, bármely név használatával DNS A rekordot hozhat létre például egy *csupasz tartomány* nevű zóna tetején.

Az alábbi táblázat ismerteti a támogatott típusok különböző Azure-szolgáltatásokat is használható. Ahogy látja, ebből a táblázatból, Azure DNS-ben csak támogatja DNS-rekordok internetre irányuló hálózati erőforrásokhoz. Az Azure DNS névfeloldás belső, személyes címek nem használható.

| Azure-szolgáltatás | Hálózati adapter | Leírás |
| --- | --- | --- |
| Application Gateway |[Előtérbeli nyilvános IP-cím](dns-custom-domain.md#public-ip-address) |A DNS A vagy CNAME rekordot hozhat létre. |
| Load Balancer |[Előtérbeli nyilvános IP-cím](dns-custom-domain.md#public-ip-address)  |A DNS A vagy CNAME rekordot hozhat létre. Terheléselosztó lehet dinamikusan hozzárendelt IPv6 nyilvános IP-címet. Ezért az IPv6-címet egy olyan CNAME rekordot kell létrehoznia. |
| Traffic Manager |Nyilvános neve |Csak létrehozhat egy olyan CNAME REKORDOT, amely a Traffic Manager-profil rendelt trafficmanager.net neve van leképezve. További információkért lásd: [Traffic Manager-hogyan működik](../traffic-manager/traffic-manager-overview.md#traffic-manager-example). |
| A felhőalapú szolgáltatás |[Nyilvános IP-cím](dns-custom-domain.md#public-ip-address) |Statikusan lefoglalt IP-címet, a DNS A rekordot hozhat létre. Dinamikusan kiosztott IP-címeket, létre kell hoznia egy CNAME rekordot, amely leképezhető a *cloudapp.net* nevét. Ez a szabály vonatkozik, a virtuális gépeknek a klasszikus portálon létrehozni, mert a felhő alapú szolgáltatásként központilag telepítették azokat. További információkért lásd: [állítson be egy egyéni tartománynevet a felhőalapú szolgáltatások](../cloud-services/cloud-services-custom-domain-name-portal.md). |
| App Service | [Külső IP](dns-custom-domain.md#app-service-web-apps) |A külső IP-címek DNS A rekordot hozhat létre. Máskülönben hozzon létre egy CNAME rekordot, amely a azurewebsites.net neve van leképezve. További információkért lásd: [egy egyéni tartománynevet hozzárendelése az Azure-alkalmazás](../app-service-web/web-sites-custom-domain-name.md) |
| Erőforrás-kezelő virtuális gépek |[Nyilvános IP-cím](dns-custom-domain.md#public-ip-address) |Erőforrás-kezelő virtuális gépek nyilvános IP-címek is rendelkezik. Nyilvános IP-címek egy virtuális gép is lehet a terheléselosztó mögött. DNS A vagy CNAME rekordot a nyilvános cím hozhat létre. Az egyéni nevet a VIP-nek a terheléselosztón megkerülésére használható. |
| A klasszikus virtuális gépeket |[Nyilvános IP-cím](dns-custom-domain.md#public-ip-address) |Klasszikus virtuális gépeinek létrehozása a PowerShell használatával, vagy a CLI (dinamikus vagy statikus fenntartott) virtuális címek konfigurálhatók. Vagy is létrehozhat egy DNS CNAME rekord, illetve. |
