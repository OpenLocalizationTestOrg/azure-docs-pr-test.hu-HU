---
title: "aaaWhat Azure továbbítási, és miért érdemes használni áttekintése |} Microsoft Docs"
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
ms.date: 08/23/2017
ms.author: sethm
ms.openlocfilehash: 4cfd77048210a435c446b908b7896737cad0edbf
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="what-is-azure-relay"></a>Mi az az Azure Relay?

hello Azure továbbítási szolgáltatás lehetővé teszi hibrid alkalmazások azáltal, hogy toosecurely tesz közzé a szolgáltatásokhoz, amelyek a vállalati hálózati toohello nyilvános felhőben, anélkül, hogy egy tűzfalkapcsolatot, tooopen találhatók, vagy zavaró szükséges tooa módosítása vállalati hálózati infrastruktúrában. A Relay számos különböző átviteli protokollt és webszolgáltatási szabványt támogat.

hello továbbítási szolgáltatás támogatja a hagyományos egyirányú, kérelem/válasz és a társ-társ forgalmat. Nagyobb pontok közötti nagyobb hatékonyság az internetes tartományban tooenable közzétételi/előfizetési forgatókönyvek és a kétirányú szoftvercsatornás kommunikáció események terjesztését is támogatja. 

Továbbítón keresztüli hello adatok átvitel mintában a helyszíni szolgáltatásnak toohello továbbítási szolgáltatáshoz egy kimenő porton keresztül csatlakozik, és a kétirányú szoftvercsatornás kommunikáció kötött tooa adott szinkronizálási címhez hoz létre. hello ügyfél küldött forgalmat toohello továbbítási szolgáltatás hello szinkronizálási címhez célcsoportkezelést majd kommunikál hello a helyszíni szolgáltatással. hello továbbítási szolgáltatás majd "szegélyhálózatába, az" adatok toohello a helyszíni szolgáltatás a kétirányú szoftvercsatornás dedikált tooeach ügyfélen keresztül. hello ügyfélnek nincs szüksége egy közvetlen kapcsolat toohello helyszíni szolgáltatással, nem szükséges tooknow, ahol hello szolgáltatás található, és hello helyszíni szolgáltatást nem kell bejövő portra a hello tűzfal nyissa meg.

Továbbító által biztosított hello kulcs funkció elemeket kétirányú, a nem pufferelt kommunikációs határozzon meg hálózati határok TCP-szerű szabályozás, a végpont felderítése, a kapcsolati állapot között, és átfedett végpontok közötti védelem. hello továbbítási képességei eltérnek a hálózati szintű integrációs technológiák, például a VPN, az, hogy a továbbító lehet hatókörön belüli tooa egyetlen alkalmazás végpontjának egyetlen számítógépen, a VPN-technológia sokkal tolakodó módon támaszkodnak az hello hálózati környezet módosítása közben .

Az Azure Relay két funkciója:

1. [Hibrid kapcsolatok](#hybrid-connections) - használ hello nyissa meg szabványos webes szoftvercsatornák többplatformos forgatókönyv engedélyezése.
2. [WCF-továbbítók](#wcf-relays) -használ a Windows Communication Foundation (WCF) tooenable távoli eljáráshívásokat. WCF továbbító hello örökölt továbbítási, hogy sok ügyfél már használjon a WCF programozási modellt kínál.

Hibrid kapcsolatok és a WCF-továbbítók is lehetővé teszik a biztonságos kapcsolat tooassets, amely a vállalati hálózaton belül. Egy másik hello használata az adott igényeknek megfelelően a függő hello a következő táblázatban leírtak szerint:

|  | WCF-továbbító | Hibrid kapcsolatok |
| --- |:---:|:---:|
| **WCF** |x | |
| **.NET Core** | |x |
| **.NET-keretrendszer** |x |x |
| **JavaScript/NodeJS** | |x |
| **Szabvány-alapú nyílt protokoll** | |x |
| **Többszörös RPC programozási modellek** | |x |

## <a name="hybrid-connections"></a>Hibrid kapcsolatok

Hello [Azure hibrid kapcsolatok](relay-hybrid-connections-protocol.md) képesség bármilyen platformon, és bármilyen nyelven, egy alapszintű WebSocket alkalmas implementálhatók továbbítási funkciókat meglévő hello biztonságos, nyílt-protokoll fejlődéséhez, amely hello WebSocket API explicit módon szerepel közös webböngésző. A Hibrid kapcsolatok a HTTP-n és a WebSockets szabványon alapul.

### <a name="service-history"></a>Szolgáltatási előzmények

Hibrid kapcsolatok kiszorítva helyéről, azt a hello volt, hasonlóképpen nevű hello Azure Service Bus WCF-továbbító a épülő "BizTalk szolgáltatás" funkció. hello az új hibrid kapcsolatok funkcióval kiegészíti hello meglévő WCF továbbító szolgáltatás, és e két szolgáltatás jellemzőinek-mellé a hello Azure továbbítási szolgáltatás létezik. E szolgáltatások közös átjáróval rendelkeznek, de ettől eltekintve különböző megvalósításról van szó.

## <a name="wcf-relays"></a>WCF-továbbítók

hello WCF továbbító működik hello teljes .NET-keretrendszer (NETFX) és a WCF. A helyszíni és a WCF "továbbítása" kötéskészlet használatával hello továbbítási szolgáltatás között hello kapcsolatot kezdeményez. Hello háttérben hello továbbítási kötéseket a toonew átviteli kötés elemek toocreate WCF-csatornaösszetevők Service Bus hello felhőben integrálódó képezi.

## <a name="architecture-processing-of-incoming-relay-requests"></a>Architektúra: Bejövő továbbítási kérelmek feldolgozása
Amikor egy ügyfél küld egy kérelem toohello [Azure továbbítási](/azure/service-bus-relay/) szolgáltatás, hello Azure load balancer továbbítja azt tooany hello átjáró csomópontok. Ha hello kérelem figyelési kérelem, hello átjárócsomópont létrehoz egy új továbbítót. Ha hello kérés kapcsolódási kérelem tooa megtalálhatják, a hello átjárócsomópont továbbítja hello kapcsolódási kérelem toohello birtokló átjárócsomópont hello továbbító. hello továbbítót birtokló átjárócsomópont hello elküld egy szinkronizálási kérelmet toohello figyelő ügyfélnek, kérő figyelő toocreate hello egy ideiglenes csatornát toohello átjárócsomópontnak, amely hello kapcsolódási kérelmet kapott.

Amikor hello továbbítási kapcsolat létrejött, hello ügyfelek továbbíthatja az hello szinkronizálási használt hello átjárócsomópont keresztül.

![Bejövő WCF Relay továbbítási kérelmek feldolgozása](./media/relay-what-is-it/ic690645.png)

## <a name="next-steps"></a>Következő lépések

* [Relay – gyakori kérdések](relay-faq.md)
* [Névtér létrehozása](relay-create-namespace-portal.md)
* [Ismerkedés a .NET-tel](relay-hybrid-connections-dotnet-get-started.md)
* [Bevezetés a Node használatába](relay-hybrid-connections-node-get-started.md)

