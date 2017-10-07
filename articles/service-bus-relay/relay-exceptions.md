---
title: "aaaAzure továbbítási kivételeket, és hogyan tooresolve őket |} Microsoft Docs"
description: "Az Azure-továbbítási kivételek listáját, és a javasolt elvégezhető műveletekhez nyújtanak toohelp megoldásukkal együtt."
services: service-bus-relay
documentationcenter: na
author: sethmanheim
manager: timlt
editor: 
ms.assetid: 5f9dd02c-cce0-43b3-8eb8-744f0c27f38c
ms.service: service-bus-relay
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 08/23/2017
ms.author: sethm
ms.openlocfilehash: de417c8e9e43407ef355fd44f6170cf2cdc46d6d
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="azure-relay-exceptions"></a>Az Azure továbbítási kivételek

Ez a cikk hello Azure továbbítási API-k által előfordulhat, hogy létrehozott kivételek sorolja fel. Ez a hivatkozás tulajdonos toochange, így biztonsági frissítések keresése.

## <a name="exception-categories"></a>Kivétel kategóriák

hello továbbítási API-k létrehozása kivételeket, amelyek lehet, hogy a következő kategóriák hello esnek. Javasolt, hogy elvégezhető műveletek toohelp resolve hello kivételeket is listában megtalálhatók.

*   **Kódolási hiba felhasználói**: [System.ArgumentException](https://msdn.microsoft.com/library/system.argumentexception.aspx), [System.InvalidOperationException](https://msdn.microsoft.com/library/system.invalidoperationexception.aspx), [System.OperationCanceledException](https://msdn.microsoft.com/library/system.operationcanceledexception.aspx), [ System.Runtime.Serialization.SerializationException](https://msdn.microsoft.com/library/system.runtime.serialization.serializationexception.aspx). 

    **Általános művelet**: próbálja toofix hello kódot, mielőtt továbblép.
*   **A telepítő-konfigurációs hiba**: [System.UnauthorizedAccessException](https://msdn.microsoft.com/library/system.unauthorizedaccessexception.aspx). 

    **Általános művelet**: tekintse át a konfigurációt. Ha szükséges, módosítsa a hello konfigurációs.
*   **Átmeneti kivételek**: [Microsoft.ServiceBus.Messaging.MessagingException](/dotnet/api/microsoft.servicebus.messaging.messagingexception), [Microsoft.ServiceBus.Messaging.ServerBusyException](/dotnet/api/microsoft.servicebus.messaging.serverbusyexception), [ Microsoft.ServiceBus.Messaging.MessagingCommunicationException](/dotnet/api/microsoft.servicebus.messaging.messagingcommunicationexception). 

    **Általános művelet**: újra hello művelettel, vagy értesítse a felhasználókat.
*   **Más kivételek**: [System.Transactions.TransactionException](https://msdn.microsoft.com/library/system.transactions.transactionexception.aspx), [System.TimeoutException](https://msdn.microsoft.com/library/system.timeoutexception.aspx). 

    **Általános művelet**: adott toohello kivétel típusa. A következő szakasz hello hello táblázatban találja. 

## <a name="exception-types"></a>Kivétel típusa

hello következő táblázatban üzenetkezelési kivétel típusa és az okaik. Azt is jelzi javasolt elvégezhető műveletekhez nyújtanak toohelp resolve hello kivételek.

| **Kivétel típusa** | **Leírás** | **Javasolt művelet** | **Jegyezze fel az automatikus vagy azonnali újrapróbálkozási** |
| --- | --- | --- | --- |
| [Időtúllépés](https://msdn.microsoft.com/library/system.timeoutexception.aspx) |hello kiszolgáló nem válaszol a kért toohello belül hello művelet a megadott idő, amely vezérli [OperationTimeout](/dotnet/api/microsoft.servicebus.messaging.messagingfactorysettings.operationtimeout). hello kiszolgáló, előfordulhat, hogy befejezett hello kért műveletet. Ez akkor fordulhat elő, toonetwork vagy más infrastruktúra késleltetése miatt. |Ellenőrizze a rendszerállapot hello konzisztencia, majd próbálkozzon újra, ha szükséges. Lásd: [TimeoutException](#timeoutexception). |Újrapróbálkozási segíthet bizonyos esetekben; újrapróbálkozási logika toocode hozzáadása. |
| [Érvénytelen művelet](https://msdn.microsoft.com/library/system.invalidoperationexception.aspx) |a kért hello felhasználói művelet nem engedélyezett a következőben hello kiszolgálóra vagy szolgáltatásba. További részletek hello kivétel üzenet jelenik meg. |Ellenőrizze a hello kódot és hello dokumentációját. Győződjön meg arról, hogy hello a kért művelet esetében érvényes. |Nem nyújt az újra gombra. |
| [A művelet megszakítva](https://msdn.microsoft.com/library/system.operationcanceledexception.aspx) |Kísérlet az végrehajtott tooinvoke egy művelet olyan objektum, amely már lezárták, megszakadt, vagy elvetése megtörtént. Ritka esetekben hello környezeti tranzakció már eldobták. |Ellenőrizze hello kódot, és győződjön meg arról, hogy nem lép működésbe műveleteket végez el egy teljesített objektum. |Nem nyújt az újra gombra. |
| [Jogosulatlan hozzáférés](https://msdn.microsoft.com/library/system.unauthorizedaccessexception.aspx) |Hello [TokenProvider](/dotnet/api/microsoft.servicebus.tokenprovider) objektumot nem sikerült megszerezni a jogkivonatot, hello lexikális elem érvénytelen vagy hello jogkivonat nem tartalmaz hello jogcímek szükséges tooperform hello műveletet. |Győződjön meg arról, hogy hello jogkivonat-szolgáltató jön létre a hello helyes az értékük. A hozzáférés-vezérlés szolgáltatás hello hello konfigurációjának ellenőrzése. |Újrapróbálkozási segíthet bizonyos esetekben; újrapróbálkozási logika toocode hozzáadása. |
| [Argumentum kivétel](https://msdn.microsoft.com/library/system.argumentexception.aspx),<br /> [Argumentum Null](https://msdn.microsoft.com/library/system.argumentnullexception.aspx),<br />[Argumentum az engedélyezett tartományon kívül esik](https://msdn.microsoft.com/library/system.argumentoutofrangeexception.aspx) |Egy vagy több hello következő történt:<br />Egy vagy több megadott argumentumokat toohello metódus nem érvényesek.<br /> hello URI megadva túl[NamespaceManager](/dotnet/api/microsoft.servicebus.namespacemanager) vagy [létrehozása](/dotnet/api/microsoft.servicebus.messaging.messagingfactory.create) egy vagy több szegmenst tartalmaz.<br />hello URI-séma túl megadott[NamespaceManager](/dotnet/api/microsoft.servicebus.namespacemanager) vagy [létrehozása](/dotnet/api/microsoft.servicebus.messaging.messagingfactory.create) érvénytelen. <br />hello tulajdonság értéke nagyobb, mint 32 KB lehet. |Hello hívó kód ellenőrzése, illetve hogy hello argumentumok megfelelőek. |Nem nyújt az újra gombra. |
| [A kiszolgáló foglalt](/dotnet/api/microsoft.servicebus.messaging.serverbusyexception) |Szolgáltatás jelenleg nem képes tooprocess hello kérelem. |hello ügyfél Várjon egy ideig, majd próbálja megismételni a műveletet hello. |hello ügyfél egy adott időtartam után újra. Ha egy másik kivétel újrapróbálkozási eredményez, ellenőrizze a hello újrapróbálási viselkedése, hogy a kivétel. |
| [Túllépte a kvótát.](/dotnet/api/microsoft.servicebus.messaging.quotaexceededexception) |üzenetküldési entitás hello elérte a megengedett maximális méretet. |Hozzon létre terület hello entitásból fogadó üzenetek vagy a alsorokon hello entitás. Lásd: [QuotaExceededException](#quotaexceededexception). |Újrapróbálkozási segíthet, ha üzenetek el lettek távolítva a hello addig is. |
| [Üzenet mérete meghaladta](/dotnet/api/microsoft.servicebus.messaging.messagesizeexceededexception) |Üzenetadatokat meghaladja hello 256 KB-os korlátot. Vegye figyelembe, hogy hello 256 KB-os korlát hello összes üzenet mérete. hello összes üzenet mérete Rendszertulajdonságok és a Microsoft .NET-terhelés tartalmazhatnak. |Hello üzenetadatokat hello méretének csökkentésére, majd próbálja megismételni a műveletet hello. |Nem nyújt az újra gombra. |

## <a name="quotaexceededexception"></a>QuotaExceededException

[QuotaExceededException](/dotnet/api/microsoft.servicebus.messaging.quotaexceededexception) azt jelzi, hogy egy adott entitás kvóta túl lett lépve.

A továbbítási, ez a kivétel becsomagolja hello [System.ServiceModel.QuotaExceededException](https://msdn.microsoft.com/library/system.servicemodel.quotaexceededexception.aspx), ami azt jelenti, hogy a figyelők hello maximális számát ezen a végponton túllépve. Ez jelzi a hello **MaximumListenersPerEndpoint** hello Kivételüzenet értékét.

## <a name="timeoutexception"></a>TimeoutException
A [TimeoutException](https://msdn.microsoft.com/library/system.timeoutexception.aspx) azt jelzi, hogy egy felhasználó által kezdeményezett művelet a vártnál hello művelet időkorlátja lejár. 

Ellenőrizze a hello hello értékének [ServicePointManager.DefaultConnectionLimit](https://msdn.microsoft.com/library/system.net.servicepointmanager.defaultconnectionlimit) tulajdonság. Szerezze meg ezt a korlátozást is okozhatnak a [TimeoutException](https://msdn.microsoft.com/library/system.timeoutexception.aspx).

A továbbító időtúllépési kivétel fordulhat elő, egy közvetítő küldő kapcsolat első megnyitásakor. Ehhez a kivételhez két gyakori oka is van:

*   Hello [OpenTimeout](https://msdn.microsoft.com/library/wcf.opentimeout.aspx) értéke túl kicsi (Ha még által másodperc töredéke alatt) lehet.
* Lehet, hogy egy helyszíni továbbítási figyelőt nem válaszoló (vagy tűzfal szabályok kapcsolatos problémák figyelői kapcsolatokat fogadjon előforduló), és hello [OpenTimeout](https://msdn.microsoft.com/library/wcf.opentimeout.aspx) értéke kisebb, mint körülbelül 20 másodperc.

Példa:

```
'System.TimeoutException’: hello operation did not complete within hello allotted timeout of 00:00:10.
hello time allotted toothis operation may have been a portion of a longer timeout.
```

### <a name="common-causes"></a>Általános okok
A hiba gyakori okai két van:

*   **Helytelen konfiguráció**
    
    Előfordulhat, hogy túl kicsi a hello működési feltétel hello művelet időkorlátja lejár. hello alapértelmezett hello művelet időtúllépése hello ügyfél SDK értéke 60 másodperc. Toosee ellenőrizze, hogy hello a kódban értéke toosomething túl kicsi. Vegye figyelembe, hogy a Processzor kihasználtsága és hello feltétele hello hálózati hello időt egy művelet toocomplete hatással lehet. Jó ötlet nem tooset hello művelet tooa nagyon kis időkorlátot.
*   **Szolgáltatások átmeneti hiba**

    Alkalmanként hello továbbítási szolgáltatás tapasztalhat késést kérelmek feldolgozásához. Ez azért fordulhat elő, például nagy forgalom idején. Ilyen esetben újra a művelettel késleltetés után, amíg a hello művelet sikeres. Ha hello ugyanaz a művelet továbbra is toofail több próbálkozást követően, ellenőrizze a hello [Azure szolgáltatás állapota hely](https://azure.microsoft.com/status/) toosee, ha a szolgáltatás-kimaradások számát ismertek.

## <a name="next-steps"></a>Következő lépések
* [Az Azure továbbító – gyakori kérdések](relay-faq.md)
* [Továbbító névtér létrehozása](relay-create-namespace-portal.md)
* [Ismerkedés az Azure és a .NET](relay-hybrid-connections-dotnet-get-started.md)
* [Ismerkedés az Azure és a csomópont](relay-hybrid-connections-node-get-started.md)

