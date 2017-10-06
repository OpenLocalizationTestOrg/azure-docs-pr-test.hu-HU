---
title: "a Service Bus aaaAzure hitelesítési és engedélyezési |} Microsoft Docs"
description: "Alkalmazások tooService Bus közös hozzáférésű Jogosultságkód (SAS) hitelesítéssel hitelesíteni."
services: service-bus-messaging
documentationcenter: na
author: sethmanheim
manager: timlt
editor: 
ms.assetid: 18bad0ed-1cee-4a5c-a377-facc4785c8c9
ms.service: service-bus-messaging
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 08/09/2017
ms.author: sethm
ms.openlocfilehash: 898a2144c99d8fac074b6d85604f710bf512e71e
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="service-bus-authentication-and-authorization"></a>Service Bus-hitelesítés és -engedélyezés

Alkalmazások hitelesítheti tooAzure Service Bus használatával a közös hozzáférésű Jogosultságkód (SAS) hitelesítési. Közös hozzáférésű Jogosultságkód hitelesítés lehetővé teszi, hogy alkalmazások tooauthenticate tooService Bus hello névtérben, vagy hello entitás, amelyhez az adott jogosultságok társított hozzáférési kulcs használata. A kulcs toogenerate egy közös hozzáférésű Jogosultságkód-jogkivonatot, hogy az ügyfelek használhatják tooauthenticate tooService Bus használhatja.

> [!IMPORTANT]
> ACS elavult az SAS használjon helyett az Azure Active Directory hozzáférés-vezérlés (más néven hozzáférés-vezérlési szolgáltatásban vagy ACS). SAS biztosít a Service Bus egy egyszerű, rugalmas és könnyen használható hitelesítési sémát. Alkalmazások használhatják a SAS forgatókönyvekben, ahol nincs szükségük toomanage hello fogalmát "jogosultsága." További információkért lásd: [ebben a blogbejegyzésben](https://blogs.msdn.microsoft.com/servicebus/2017/06/01/upcoming-changes-to-acs-enabled-namespaces/).

## <a name="shared-access-signature-authentication"></a>Közös hozzáférésű Jogosultságkód hitelesítés

[SAS hitelesítési](service-bus-sas.md) lehetővé teszi, hogy a hogy toogrant a felhasználói hozzáférés tooService Bus erőforrásainak adott jogosultságokkal. A Service Bus SAS hitelesítési magában foglalja a hello konfigurálása a Service Bus erőforrás társított jogosultságok titkosítási kulcsot. Ügyfelek majd férhet hozzáférés toothat erőforrás segítségével egy SAS-jogkivonatot, amely hello erőforrás URI férnek hozzá a áll, és hello aláírva elévülés kulcs konfigurálva.

A biztonsági Társítások kulcsok állíthatja be a Service Bus-névtér. hello kulcs tooall üzenetküldési entitások, hogy a névtér vonatkozik. A Service Bus-üzenetsorok és témakörök beállíthatók a kulcsokat. SAS is támogatott [Azure továbbítási](../service-bus-relay/relay-authentication-and-authorization.md).

toouse SAS, konfigurálhat egy [SharedAccessAuthorizationRule](/dotnet/api/microsoft.servicebus.messaging.sharedaccessauthorizationrule) objektum a névtér, üzenetsor vagy témakör. Ez a szabály a következő elemek hello áll:

* *Kulcsnév* , amely azonosítja a hello szabály.
* *PrimaryKey* a titkosítási kulcs használt SAS-tokenje toosign/ellenőrzése.
* *Másodlagos kulcs* a titkosítási kulcs használt SAS-tokenje toosign/ellenőrzése.
* *Jogok* képviselő figyelési hello gyűjteménye, küldjön, vagy engedélyek kezeléséhez.

Az engedélyezési szabályok hello névtér szintjén állítható be hozzáférést biztosíthat tooall entitások névtérben levő ügyfelek jogkivonatokkal hello megfelelő kulccsal aláírva. Too12 fel ilyen engedélyezési szabályok is lehet konfigurálni egy Service Bus-névtér, üzenetsor vagy témakör. Alapértelmezés szerint egy [SharedAccessAuthorizationRule](/dotnet/api/microsoft.servicebus.messaging.sharedaccessauthorizationrule) jogosultságok mindegyikével rendelkező rendszer konfigurálja minden névtér esetében először ki van építve.

egy entitás tooaccess, hello ügyfélhez egy adott generálta SAS-jogkivonat [SharedAccessAuthorizationRule](/dotnet/api/microsoft.servicebus.messaging.sharedaccessauthorizationrule). hello SAS-jogkivonat generálta hello HMAC-SHA256-kivonata egy erőforrás-karakterlánc alkotó hello erőforrás URI toowhich hozzáférést igényelnek, és egy lejárati hello engedélyezési szabály társított titkosítási kulccsal.

SAS-hitelesítési támogatás a Service Bus megtalálható hello Azure .NET SDK 2.0-s verziója és újabb verziók. SAS támogatását is magában foglalja a [SharedAccessAuthorizationRule](https://docs.microsoft.com/dotnet/api/microsoft.servicebus.messaging.sharedaccessauthorizationrule). Fogadja el a kapcsolati karakterlánc paraméterként API-k közé tartozik a SAS-kapcsolati karakterláncok támogatása.

## <a name="next-steps"></a>Következő lépések

- Továbbra is olvasási [Service Bus hitelesítési megosztott hozzáférési aláírásokkal](service-bus-sas.md) SAS kapcsolatos további részletekért.
- [Módosítja a tooACS engedélyezett névterek.](https://blogs.msdn.microsoft.com/servicebus/2017/06/01/upcoming-changes-to-acs-enabled-namespaces/)
- Azure-továbbítási hitelesítési és engedélyezési megfelelő információ: [Azure továbbítási hitelesítési és engedélyezési](../service-bus-relay/relay-authentication-and-authorization.md). 

