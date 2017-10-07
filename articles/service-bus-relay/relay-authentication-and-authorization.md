---
title: "aaaAzure továbbítják a hitelesítési és engedélyezési |} Microsoft Docs"
description: "Az Azure-továbbítási közös hozzáférésű Jogosultságkód (SAS) hitelesítési áttekintése"
services: service-bus-relay
documentationcenter: na
author: sethmanheim
manager: timlt
editor: 
ms.assetid: 
ms.service: service-bus-relay
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 07/03/2017
ms.author: sethm
ms.openlocfilehash: b27914672ce968da2bddba8dafc5683ebf3834ac
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="azure-relay-authentication-and-authorization"></a>Az Azure továbbítási hitelesítés és engedélyezés
Alkalmazások hitelesítheti tooAzure továbbítják a közös hozzáférésű Jogosultságkód (SAS)-hitelesítéssel. Hasonló túl[Service Bus üzenetkezelés](../service-bus-messaging/service-bus-authentication-and-authorization.md), SAS hitelesítési lehetővé teszi, hogy az alkalmazások tooauthenticate toohello hello továbbítási névtér konfigurált hozzáférési kulcs használata Azure továbbítási szolgáltatás. A kulcs toogenerate egy közös hozzáférésű Jogosultságkód-jogkivonatot, hogy ügyfelek használhatják-e tooauthenticate toohello továbbítási szolgáltatás használhatja.

## <a name="shared-access-signature-authentication"></a>Közös hozzáférésű Jogosultságkód hitelesítés
[SAS hitelesítési](../service-bus-messaging/service-bus-sas.md) lehetővé teszi, hogy a hogy toogrant egy felhasználó tooAzure továbbítási erőforrások eléréséhez megadott jogosultságokkal. SAS hitelesítési magában foglalja a hello konfigurálása egy titkosítási kulccsal rendelkező erőforrás társított jogosultságok. Ügyfelek majd férhet hozzáférés toothat erőforrás segítségével egy SAS-jogkivonatot, amely hello erőforrás URI férnek hozzá a áll, és hello aláírva elévülés kulcs konfigurálva.

A biztonsági Társítások kulcsok továbbítási névtérben állíthatja be. Service Bus üzenetkezelés, eltérően [hibrid kapcsolatok](relay-hybrid-connections-protocol.md) nem hitelesített vagy névtelen feladók támogatja. Engedélyezheti a névtelen hozzáférést hello entitás létrehozásakor, ahogy az alábbi képernyőfelvétel a hello portálról hello:

![][0]

toouse SAS, konfigurálhat egy [SharedAccessAuthorizationRule](/dotnet/api/microsoft.servicebus.messaging.sharedaccessauthorizationrule) objektum továbbítási névtérben, amely hello következő tevődik össze:

* *Kulcsnév* , amely azonosítja a hello szabály.
* *PrimaryKey* a titkosítási kulcs használt SAS-tokenje toosign/ellenőrzése.
* *Másodlagos kulcs* a titkosítási kulcs használt SAS-tokenje toosign/ellenőrzése.
* *Jogok* képviselő figyelési hello gyűjteménye, küldjön, vagy engedélyek kezeléséhez.

Az engedélyezési szabályok hello névtér szintjén állítható be hozzáférést biztosíthat tooall továbbító-kapcsolatok a névtérben az ügyfelek jogkivonatokkal hello megfelelő kulccsal aláírva. Too12 fel ilyen engedélyezési szabályok is lehet konfigurálni a továbbítási névtér. Alapértelmezés szerint egy [SharedAccessAuthorizationRule](/dotnet/api/microsoft.servicebus.messaging.sharedaccessauthorizationrule) jogosultságok mindegyikével rendelkező rendszer konfigurálja minden névtér esetében először ki van építve.

egy entitás tooaccess, hello ügyfélhez egy adott generálta SAS-jogkivonat [SharedAccessAuthorizationRule](/dotnet/api/microsoft.servicebus.messaging.sharedaccessauthorizationrule). hello SAS-jogkivonat generálta hello HMAC-SHA256-kivonata egy erőforrás-karakterlánc alkotó hello erőforrás URI toowhich hozzáférést igényelnek, és egy lejárati hello engedélyezési szabály társított titkosítási kulccsal.

SAS-hitelesítési támogatás a Azure továbbítási megtalálható hello Azure .NET SDK 2.0-s verziója és újabb verziók. SAS támogatását is magában foglalja a [SharedAccessAuthorizationRule](/dotnet/api/microsoft.servicebus.messaging.sharedaccessauthorizationrule). Fogadja el a kapcsolati karakterlánc paraméterként API-k közé tartozik a SAS-kapcsolati karakterláncok támogatása.

## <a name="next-steps"></a>Következő lépések
- Továbbra is olvasási [Service Bus hitelesítési megosztott hozzáférési aláírásokkal](../service-bus-messaging/service-bus-sas.md) SAS kapcsolatos további részletekért.
- Lásd: hello [Azure hibrid kapcsolatok protokoll útmutató](relay-hybrid-connections-protocol.md) hello hibrid kapcsolatok képességgel kapcsolatos részletes információkat.
- Service Bus üzenetkezelés hitelesítési és engedélyezési megfelelő információ: [Service Bus hitelesítési és engedélyezési](../service-bus-messaging/service-bus-authentication-and-authorization.md). 

[0]: ./media/relay-authentication-and-authorization/hcanon.png