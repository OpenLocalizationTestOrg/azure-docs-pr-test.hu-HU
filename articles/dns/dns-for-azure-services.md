---
title: "Azure DNS aaaUsing más Azure-szolgáltatásokkal |} Microsoft Docs"
description: "Hogyan toouse Azure DNS tooresolve nevet más Azure-szolgáltatások ismertetése"
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
ms.openlocfilehash: 791f93d6aff2c638c08518e9f1e8ab89ac8de3f2
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="how-azure-dns-works-with-other-azure-services"></a>Azure DNS működése más Azure-szolgáltatásokkal

Az Azure DNS egy olyan üzemeltetett DNS felügyeleti és name névfeloldási szolgáltatás. Ez lehetővé teszi, hogy Ön toocreate nyilvános DNS-neveit hello egyéb alkalmazások és szolgáltatások telepítése az Azure-ban. Egyszerűen hozzáadása a szolgáltatás hello megfelelő típusú rekord létrehozása az Azure-szolgáltatások egy nevet az egyéni tartomány.

* Dinamikusan kiosztott IP-címeket létre kell hoznia egy DNS CNAME-rekordot adott maps toohello DNS-név, Azure hozott létre a szolgáltatás. DNS-szabványokból újrafelhasználásának megtiltása egy olyan CNAME rekordot a hello zóna felső pontja.
* Statikusan lefoglalt IP-címekhez, bármely név használatával DNS A rekordot hozhat létre például egy *csupasz tartomány* hello zóna csúcsán nevét.

hello következő tábla vázol fel hello támogatott, amelyeket használhat a különböző Azure-szolgáltatásokhoz. Ahogy látja, ebből a táblázatból, Azure DNS-ben csak támogatja DNS-rekordok internetre irányuló hálózati erőforrásokhoz. Az Azure DNS névfeloldás belső, személyes címek nem használható.

| Azure-szolgáltatás | Hálózati adapter | Leírás |
| --- | --- | --- |
| Application Gateway |[Előtérbeli nyilvános IP-cím](dns-custom-domain.md#public-ip-address) |A DNS A vagy CNAME rekordot hozhat létre. |
| Load Balancer |[Előtérbeli nyilvános IP-cím](dns-custom-domain.md#public-ip-address)  |A DNS A vagy CNAME rekordot hozhat létre. Terheléselosztó lehet dinamikusan hozzárendelt IPv6 nyilvános IP-címet. Ezért az IPv6-címet egy olyan CNAME rekordot kell létrehoznia. |
| Traffic Manager |Nyilvános neve |Csak létrehozhat egy olyan CNAME REKORDOT, amely leképezi a Traffic Manager-profil tooyour hozzárendelt toohello trafficmanager.net nevet. További információkért lásd: [Traffic Manager-hogyan működik](../traffic-manager/traffic-manager-overview.md#traffic-manager-example). |
| A felhőalapú szolgáltatás |[Nyilvános IP-cím](dns-custom-domain.md#public-ip-address) |Statikusan lefoglalt IP-címet, a DNS A rekordot hozhat létre. Dinamikusan kiosztott IP-címeket, létre kell hoznia egy CNAME rekordot, amely leképezhető toohello *cloudapp.net* nevét. Ez a szabály vonatkozik tooVMs hello klasszikus portál létrehozni, mert a felhő alapú szolgáltatásként központilag telepítették azokat. További információkért lásd: [állítson be egy egyéni tartománynevet a felhőalapú szolgáltatások](../cloud-services/cloud-services-custom-domain-name-portal.md). |
| App Service | [Külső IP](dns-custom-domain.md#app-service-web-apps) |A külső IP-címek DNS A rekordot hozhat létre. Máskülönben hozzon létre egy CNAME rekordot, amely leképezhető toohello azurewebsites.net nevét. További információkért lásd: [képezi le egy egyéni tartomány nevét tooan Azure-alkalmazás](../app-service-web/web-sites-custom-domain-name.md) |
| Erőforrás-kezelő virtuális gépek |[Nyilvános IP-cím](dns-custom-domain.md#public-ip-address) |Erőforrás-kezelő virtuális gépek nyilvános IP-címek is rendelkezik. Nyilvános IP-címek egy virtuális gép is lehet a terheléselosztó mögött. A DNS A vagy CNAME rekordot hello nyilvános cím hozhat létre. Az egyéni neve lehet használt toobypass hello VIP hello terheléselosztón. |
| A klasszikus virtuális gépeket |[Nyilvános IP-cím](dns-custom-domain.md#public-ip-address) |Klasszikus virtuális gépeinek létrehozása a PowerShell használatával, vagy a CLI (dinamikus vagy statikus fenntartott) virtuális címek konfigurálhatók. Vagy is létrehozhat egy DNS CNAME rekord, illetve. |
