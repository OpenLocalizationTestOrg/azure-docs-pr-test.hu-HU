---
title: "Mi az Azure Relay, és mire használható? – áttekintés | Microsoft Docs"
description: "Az Azure Relay áttekintése"
services: service-bus-relay
documentationcenter: .net
author: sethmanheim
manager: timlt
editor: 
ms.assetid: 1e3e971d-2a24-4f96-a88a-ce3ea2b1a1cd
ms.service: service-bus-relay
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: multiple
ms.topic: get-started-article
ms.date: 12/20/2017
ms.author: sethm
ms.openlocfilehash: d1b1c0661458669dc8f05a49037943320de2ecb3
ms.sourcegitcommit: b5c6197f997aa6858f420302d375896360dd7ceb
ms.translationtype: HT
ms.contentlocale: hu-HU
ms.lasthandoff: 12/21/2017
---
# <a name="what-is-azure-relay"></a>Mi az az Azure Relay?

Az Azure Relay szolgáltatás úgy segíti a hibrid alkalmazásokat, hogy biztonságosan lehetővé teszi a vállalati hálózaton belüli szolgáltatások közzétételét a nyilvános felhőben anélkül, hogy meg kellene nyitni egy tűzfalkapcsolatot, vagy zavaró módosításokat kellene végrehajtani a vállalati hálózati infrastruktúrában. A Relay számos különböző átviteli protokollt és webszolgáltatási szabványt támogat.

A továbbítási szolgáltatás támogatja a hagyományos egyirányú, a kérelem/válasz típusú és a társközi forgalmat. Támogatja az események terjesztését is az internetes tartományban, így a pontok közötti nagyobb hatékonyság érdekében lehetővé válnak a közzétételi/előfizetési forgatókönyvek és a kétirányú szoftvercsatornás kommunikáció. 

A továbbítón keresztüli adatátviteli mintában a helyszíni szolgáltatás csatlakozik a továbbítási szolgáltatáshoz egy kimenő porton keresztül, majd létrehoz egy adott szinkronizálási címhez kötött kétirányú szoftvercsatornát a kommunikációhoz. Az ügyfél ezután kommunikálhat a helyszíni szolgáltatással, ha a szinkronizálási címet megcélozva forgalmat küld a továbbítási szolgáltatásnak. A továbbítási szolgáltatás ezután továbbítja az adatokat a helyszíni szolgáltatásba az egyes ügyfelek számára kijelölt kétirányú szoftvercsatornán keresztül. Az ügyfélnek nincs szüksége közvetlen kapcsolatra a helyszíni szolgáltatással, és azt sem kell tudnia, hol található, a helyszíni szolgáltatásnak pedig nincs szüksége megnyitott bejövő portra a tűzfalon.

A Relay által biztosított kulcsfontosságú képesség a TCP-szerű szabályozással, végpontészleléssel, kapcsolati állapottal és átfedő végpontvédelemmel rendelkező, hálózati határok között történő kétirányú, nem pufferelt kommunikációban rejlik. A továbbítási képességek abban térnek el az olyan hálózati szintű integrációs technológiáktól mint például a VPN, hogy a továbbítás egyetlen alkalmazásvégpontra is fókuszálható a gépen, míg a VPN technológia ennél jóval agresszívabb, hiszen a hálózati környezet módosítására támaszkodik.

Az Azure Relay két funkciója:

1. [Hibrid kapcsolatok](#hybrid-connections) – A többplatformos forgatókönyveket lehetővé tevő WebSockets nyílt szabványt használja.
2. [WCF-továbbítók](#wcf-relays) – A Windows Communication Foundation (WCF) használatával engedélyezi a távoli eljáráshívásokat. A WCF-továbbító egy olyan örökölt Relay-ajánlat, amelyet számos ügyfél már használ a WCF-programozási modelljeivel.

A Hibrid kapcsolatok és a WCF-továbbítók egyaránt lehetővé teszik a vállalati hálózaton belüli eszközökhöz való biztonságos csatlakozást. Az Ön igényein múlik, hogy melyik lehetőséget választja, ahogy az az alábbi táblázatban látható:

|  | WCF-továbbító | Hibrid kapcsolatok |
| --- |:---:|:---:|
| **WCF** |x | |
| **.NET Core** | |x |
| **.NET-keretrendszer** |x |x |
| **JavaScript/NodeJS** | |x |
| **Szabvány-alapú nyílt protokoll** | |x |
| **Többszörös RPC programozási modellek** | |x |

## <a name="hybrid-connections"></a>Hibrid kapcsolatok

Az [Azure Relay Hibrid kapcsolatok képesség](relay-hybrid-connections-protocol.md) egy biztonságos, nyílt protokoll szerinti továbbfejlesztése a létező továbbítófunkcióknak, amely bármely platformon, illetve bármely olyan nyelvben is alkalmazható, amely rendelkezik alapszintű WebSocket képességgel, ha kifejezetten tartalmazza a WebSocket API-t a gyakori internetes böngészőkben. A Hibrid kapcsolatok a HTTP-n és a WebSockets szabványon alapul.

### <a name="service-history"></a>Szolgáltatási előzmények

A Hibrid kapcsolatok pótolja a korábbi, hasonló nevű „BizTalk Services” funkciót, amely az Azure Service Bus WCF-továbbítóra épült. Az új Hibrid kapcsolatok képesség kiegészíti a már meglévő WCF-továbbítót, és a két szolgáltatási képesség egymás mellett fog működni az Azure Relay szolgáltatásban. E szolgáltatások közös átjáróval rendelkeznek, de ettől eltekintve különböző megvalósításról van szó.

## <a name="wcf-relay"></a>WCF-továbbító

A WCF-továbbító a teljes .NET-keretrendszerrel (NETFX) és a WCF-fel is használható. A kapcsolatot a helyszíni és a továbbítási szolgáltatás között kezdeményezheti egy WCF továbbító kötéskészlet használatával. A színfalak mögött a továbbítási kötéseket a rendszer a Service Busszal a felhőben integrálódó WCF-csatornaösszetevők létrehozására tervezett új átviteli kötőelemekké képezi le. További információ: [Ismerkedés a WCF-továbbító szolgáltatással](relay-wcf-dotnet-get-started.md).

## <a name="architecture-processing-of-incoming-relay-requests"></a>Architektúra: Bejövő továbbítási kérelmek feldolgozása

Amikor egy ügyfél kérelmet küld az [Azure Relay](/azure/service-bus-relay/) szolgáltatásnak, az Azure Load Balancer továbbítja azt valamelyik átjárócsomópontnak. Ha kérelem figyelési kérelem, az átjárócsomópont létrehoz egy új továbbítót. Ha kérelem egy adott továbbítóhoz való kapcsolódásra irányul, az átjárócsomópont továbbítja a kapcsolódási kérelmet annak az átjárócsomópontnak, amely a továbbító tulajdonosa. A továbbítót birtokló átjárócsomópont küld egy szinkronizálási kérelmet a figyelő ügyfélnek, amelyben arra kéri a figyelőt, hogy hozzon létre egy ideiglenes csatornát ahhoz az átjárócsomóponthoz, amely a kapcsolódási kérelmet kapta.

Ha a továbbítási kapcsolat létrejött, az ügyfelek üzeneteket válthatnak a szinkronizálásra használt átjárócsomóponton keresztül.

![Bejövő WCF Relay továbbítási kérelmek feldolgozása](./media/relay-what-is-it/ic690645.png)

## <a name="next-steps"></a>Következő lépések

* [Relay – gyakori kérdések](relay-faq.md)
* [Névtér létrehozása](relay-create-namespace-portal.md)
* [Ismerkedés a .NET-tel](relay-hybrid-connections-dotnet-get-started.md)
* [Bevezetés a Node használatába](relay-hybrid-connections-node-get-started.md)

