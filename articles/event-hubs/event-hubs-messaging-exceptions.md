---
title: "aaaAzure Event Hubs üzenetküldési kivételek |} Microsoft Docs"
description: "Azure Event Hubs üzenetküldési kivételeket és a javasolt műveletek listáját."
services: event-hubs
documentationcenter: na
author: sethmanheim
manager: timlt
editor: 
ms.assetid: 2c6273de-0106-47e5-b45d-59040e51f2c5
ms.service: event-hubs
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 08/15/2017
ms.author: sethm
ms.openlocfilehash: 9c164e76612c26607219f08407f689aaab4050a5
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="event-hubs-messaging-exceptions"></a>Az Event Hubs üzenetküldési kivételei
Ez a cikk soroljuk hello kivételek hello Azure Service Bus által generált API-k, többek között az Event Hubs üzenetküldési. Ez a hivatkozás tulajdonos toochange, így biztonsági frissítések keresése.

## <a name="exception-categories"></a>Kivétel kategóriák
hello Event Hubs API-k hozhat létre kivételeket, amelyek a következő kategóriák hello is tartozik, kapcsolódó hello művelet együtt is igénybe vehet tootry toofix őket.

1. Kódolási hiba felhasználói: [System.ArgumentException](https://msdn.microsoft.com/library/system.argumentexception.aspx), [System.InvalidOperationException](https://msdn.microsoft.com/library/system.invalidoperationexception.aspx), [System.OperationCanceledException](https://msdn.microsoft.com/library/system.operationcanceledexception.aspx), [ System.Runtime.Serialization.SerializationException](https://msdn.microsoft.com/library/system.runtime.serialization.serializationexception.aspx). Általános művelet: próbálja toofix hello kód a folytatás előtt.
2. A telepítő-konfigurációs hiba: [Microsoft.ServiceBus.Messaging.MessagingEntityNotFoundException](/dotnet/api/microsoft.servicebus.messaging.messagingentitynotfoundexception), [Microsoft.Azure.EventHubs.MessagingEntityNotFoundException](/dotnet/api/microsoft.azure.eventhubs.messagingentitynotfoundexception), [ System.UnauthorizedAccessException](https://msdn.microsoft.com/library/system.unauthorizedaccessexception.aspx). Általános művelet: Ellenőrizze a konfigurációt, és szükség esetén módosítsa.
3. Átmeneti kivételek: [Microsoft.ServiceBus.Messaging.MessagingException](/dotnet/api/microsoft.servicebus.messaging.messagingexception), [Microsoft.ServiceBus.Messaging.ServerBusyException](#serverbusyexception), [ Microsoft.Azure.EventHubs.ServerBusyException](#serverbusyexception), [Microsoft.ServiceBus.Messaging.MessagingCommunicationException](/dotnet/api/microsoft.servicebus.messaging.messagingcommunicationexception). Általános művelet: újra hello művelettel, vagy értesítse a felhasználókat.
4. Más kivételek: [System.Transactions.TransactionException](https://msdn.microsoft.com/library/system.transactions.transactionexception.aspx), [System.TimeoutException](#timeoutexception), [Microsoft.ServiceBus.Messaging.MessageLockLostException](/dotnet/api/microsoft.servicebus.messaging.messagelocklostexception), [Microsoft.ServiceBus.Messaging.SessionLockLostException](/dotnet/api/microsoft.servicebus.messaging.sessionlocklostexception). Általános művelet: adott toohello kivételtípus; Tekintse meg a következő szakasz hello toohello táblájában. 

## <a name="exception-types"></a>Kivétel típusa
hello következő táblázatban üzenetkezelési kivétel típusait, és az okait és a megjegyzések javasolt művelet is igénybe vehet.

| **Kivétel típusa** | **Leírás/OK/példák** | **Javasolt művelet** | **Jegyezze fel az automatikus azonnali újrapróbálkozási** |
| --- | --- | --- | --- |
| [TimeoutException](https://msdn.microsoft.com/library/system.timeoutexception.aspx) |hello kiszolgáló nem válaszol a kért toohello belül hello művelet a megadott idő, amely vezérli [OperationTimeout](/dotnet/api/microsoft.servicebus.messaging.messagingfactorysettings#Microsoft_ServiceBus_Messaging_MessagingFactorySettings_OperationTimeout). Előfordulhat, hogy végrehajtotta a hello server hello a kért műveletet. Ez akkor fordulhat elő, toonetwork vagy más infrastruktúra késleltetése miatt. |Ellenőrizze a rendszerállapot hello konzisztencia, és szükség esetén próbálja megismételni.<br /> Lásd: [TimeoutException](#timeoutexception). |Újrapróbálkozási segíthet bizonyos esetekben; újrapróbálkozási logika toocode hozzáadása. |
| [InvalidOperationException](https://msdn.microsoft.com/library/system.invalidoperationexception.aspx) |a kért hello felhasználói művelet nem engedélyezett a következőben hello kiszolgálóra vagy szolgáltatásba. További részletek hello kivétel üzenet jelenik meg. Például [Complete](/dotnet/api/microsoft.servicebus.messaging.brokeredmessage#Microsoft_ServiceBus_Messaging_BrokeredMessage_Complete) ezt a kivételt állít elő, ha hello üzenet érkezett a [ReceiveAndDelete](/dotnet/api/microsoft.servicebus.messaging.receivemode) mód. |Ellenőrizze a hello kódot és hello dokumentációját. Ellenőrizze, hogy hello a kért művelet esetében érvényes. |Nem nyújt az újra gombra. |
| [OperationCanceledException](https://msdn.microsoft.com/library/system.operationcanceledexception.aspx) |Kísérlet az végrehajtott tooinvoke egy művelet olyan objektum, amely már lezárták, megszakítva vagy elvetése megtörtént. Ritka esetekben hello környezeti tranzakció már eldobták. |Ellenőrizze hello kódot, és győződjön meg arról, hogy nem lép működésbe műveleteket végez el egy teljesített objektum. |Nem nyújt az újra gombra. |
| [UnauthorizedAccessException](https://msdn.microsoft.com/library/system.unauthorizedaccessexception.aspx) |Hello [TokenProvider](/dotnet/api/microsoft.servicebus.tokenprovider) objektumot nem sikerült megszerezni a jogkivonatot, hello lexikális elem érvénytelen vagy hello jogkivonat nem tartalmaz hello jogcímek szükséges tooperform hello műveletet. |Ellenőrizze, hogy a jogkivonat-szolgáltató hello jön létre a hello helyes az értékük. A hozzáférés-vezérlés szolgáltatás hello hello konfigurációjának ellenőrzése. |Újrapróbálkozási segíthet bizonyos esetekben; újrapróbálkozási logika toocode hozzáadása. |
| [ArgumentException](https://msdn.microsoft.com/library/system.argumentexception.aspx)<br /> [ArgumentNullException](https://msdn.microsoft.com/library/system.argumentnullexception.aspx)<br />[ArgumentOutOfRangeException](https://msdn.microsoft.com/library/system.argumentoutofrangeexception.aspx) |Egy vagy több megadott argumentumokat toohello metódus nem érvényesek. hello URI megadva túl[NamespaceManager](/dotnet/api/microsoft.servicebus.namespacemanager) vagy [létrehozása](/dotnet/api/microsoft.servicebus.messaging.messagingfactory#Microsoft_ServiceBus_Messaging_MessagingFactory_Create_System_Collections_Generic_IEnumerable_System_Uri__) elérési szegmenssel tartalmazza. hello URI-séma túl megadott[NamespaceManager](/dotnet/api/microsoft.servicebus.namespacemanager) vagy [létrehozása](/dotnet/api/microsoft.servicebus.messaging.messagingfactory#Microsoft_ServiceBus_Messaging_MessagingFactory_Create_System_Collections_Generic_IEnumerable_System_Uri__) érvénytelen. hello tulajdonság értéke nagyobb, mint 32 KB lehet. |Hello hívó kód ellenőrzése, illetve hogy hello argumentumok megfelelőek. |Nem nyújt az újra gombra. |
| [Microsoft.ServiceBus.Messaging.MessagingEntityNotFoundException](/dotnet/api/microsoft.servicebus.messaging.messagingentitynotfoundexception) <br /> [Microsoft.Azure.EventHubs.MessagingEntityNotFoundException](/dotnet/api/microsoft.azure.eventhubs.messagingentitynotfoundexception) |Hello művelethez társított entitás nem létezik, vagy már törölték. |Ellenőrizze, hogy hello entitás. |Nem nyújt az újra gombra. |
| [MessageNotFoundException](/dotnet/api/microsoft.servicebus.messaging.messagenotfoundexception) |Kísérlet történt egy üzenet tooreceive adott sorozatszáma. Ez az üzenet nem található. |Ellenőrizze, hogy hello üzenet már nem érkezett. Ha üdvözlőüzenetére deadlettered, ellenőrizze a hello kézbesítetlen levelek várólista toosee. |Nem nyújt az újra gombra. |
| [MessagingCommunicationException](/dotnet/api/microsoft.servicebus.messaging.messagingcommunicationexception) |Az ügyfél nem tud tooestablish egy kapcsolat tooEvent Hub áll. |Győződjön meg arról, hello megadott állomásnév helyességéről, valamint hello állomás érhető el. |Újrapróbálkozási segíthet, ha időszakos csatlakozási hibák. |
| [Microsoft.ServiceBus.Messaging.ServerBusyException](/dotnet/api/microsoft.servicebus.messaging.serverbusyexception) <br /> [Microsoft.Azure.EventHubs.ServerBusyException](/dotnet/api/microsoft.azure.eventhubs.serverbusyexception) |Szolgáltatás jelenleg nem képes tooprocess hello kérelem. |Ügyfél Várjon egy ideig, majd próbálja megismételni a műveletet hello. <br /> Lásd: [ServerBusyException](#serverbusyexception). |Ügyfél bizonyos időköz után újrapróbálkozik. Ha ismételt próbálkozással egy másik kivétel eredményez, ellenőrizze az újrapróbálási viselkedése, hogy a kivétel. |
| [MessageLockLostException](/dotnet/api/microsoft.servicebus.messaging.messagelocklostexception) |Üdvözlőüzenetére társított zárolási-token érvényessége lejárt, vagy hello zárolási jogkivonat nem található. |Üdvözlőüzenetére eldobásakor. |Nem nyújt az újra gombra. |
| [SessionLockLostException](/dotnet/api/microsoft.servicebus.messaging.sessionlocklostexception) |Ehhez a munkamenethez társított zárolási elvész. | Megszakítás hello [MessageSession](/dotnet/api/microsoft.servicebus.messaging.messagesession) objektum. |Nem nyújt az újra gombra. |
| [MessagingException](/dotnet/api/microsoft.servicebus.messaging.messagingexception) |Általános kivétel, előfordulhat, hogy fel kell a következő esetekben hello üzenetküldési: toocreate tett kísérlet egy [QueueClient](/dotnet/api/microsoft.servicebus.messaging.queueclient) neve vagy elérési útja, amelyhez tartozik tooa különböző entitástípus (például egy témakör) segítségével. Kísérlet az végrehajtott toosend üzenet 256KB-nál nagyobb. hello kiszolgáló vagy a szolgáltatás hibát észlelt a hello kérés feldolgozása során. Ellenőrizze a részleteket hello Kivételüzenet. Ez általában az átmeneti kivétel. |Hello kód ellenőrzése elemre, és győződjön meg arról, hogy csak szerializálható objektumok hello üzenettörzs használja (vagy egy egyéni szerializáló használja). Ellenőrizze és csak használja a támogatott típusok hello támogatott érték esetében hello tulajdonságok hello dokumentációját. Ellenőrizze a hello [IsTransient](/dotnet/api/microsoft.servicebus.messaging.messagingexception#Microsoft_ServiceBus_Messaging_MessagingException_IsTransient) tulajdonság. Ha **igaz**, megpróbálkozhat a hello műveletet. |Újrapróbálási viselkedése nincs definiálva, és nem segítséget. |
| [MessagingEntityAlreadyExistsException](/dotnet/api/microsoft.servicebus.messaging.messagingentityalreadyexistsexception) |Kísérlet történt toocreate egy entitás, amelynek neve már használatban van egy másik entitás az adott szolgáltatás névtere. |Hello meglévő entitás törlése, vagy válasszon másik nevet a hello entitás toobe létrehozott. |Nem nyújt az újra gombra. |
| [QuotaExceededException](/dotnet/api/microsoft.servicebus.messaging.quotaexceededexception) |üzenetküldési entitás hello elérte a megengedett maximális méretet. Ez akkor fordulhat elő, ha hello maximális számú fogadó (amely 5) már meg van nyitva a fogyasztó-csoport szinten. |Hozzon létre terület hello entitásból fogadó üzenetek vagy az alárendelt várólisták hello entitás. <br /> Lásd: [QuotaExceededException](#quotaexceededexception) |Újrapróbálkozási segíthet, ha üzenetek el lettek távolítva a hello addig is. |
|  | | | |
| [SessionCannotBeLockedException](/dotnet/api/microsoft.servicebus.messaging.sessioncannotbelockedexception) |Kísérlet történt egy munkamenetet egy adott munkamenet-Azonosítót, de hello munkamenet jelenleg zárolva van egy másik ügyfél tooaccept. |Ellenőrizze, hogy hello munkamenet szerint más ügyfelek nincs zárolva. |Ha hello munkamenet már elérhető a közbenső hello újrapróbálkozási segítséget. |
| [TransactionSizeExceededException](/dotnet/api/microsoft.servicebus.messaging.transactionsizeexceededexception) |Túl sok művelet hello tranzakció részét képezik. |Csökkentse a tranzakció részét képező operations hello számát. |Nem nyújt az újra gombra. |
| [MessagingEntityDisabledException](/dotnet/api/microsoft.servicebus.messaging.messagingentitydisabledexception) |Egy futtatási művelet a letiltott entitásra vonatkozó kérést. |Hello entitás aktiválása. |Újrapróbálkozási segíthet, ha hello entitás ideiglenes hello az aktív. |
| [Microsoft.ServiceBus.Messaging.MessageSizeExceededException](/dotnet/api/microsoft.servicebus.messaging.messagesizeexceededexception) <br /> [Microsoft.Azure.EventHubs.MessageSizeExceededException](/dotnet/api/microsoft.azure.eventhubs.messagesizeexceededexception) | Üzenetadatokat meghaladja hello 256 KB-os korlátot. Vegye figyelembe, hogy hello 256 KB-os korlát hello teljes üzenet mérete, ami magában foglalhatja a Rendszertulajdonságok és bármely .NET terhelés mellett. |Hello üzenetadatokat hello méretének csökkentésére, majd próbálja megismételni a műveletet hello. |Nem nyújt az újra gombra. |
| [TransactionException](https://msdn.microsoft.com/library/system.transactions.transactionexception.aspx) |hello környezeti tranzakció (*Transaction.Current*) érvénytelen. Előfordulhat, hogy befejeződött vagy megszakadt. További információt tartalmaznak a belső kivételben találhatók. | |Nem nyújt az újra gombra. |
| [TransactionInDoubtException](https://msdn.microsoft.com/library/system.transactions.transactionindoubtexception.aspx) |Egy művelet kísérletet egy tranzakció kimenetele kétséges, vagy toocommit hello tranzakció tett kísérlet, és hello tranzakció bizonytalan válik. |Az alkalmazás kezelni hello tranzakció már lehetett véglegesíteni kell (a egy különleges esetben), ezt a kivételt. |- |

## <a name="quotaexceededexception"></a>QuotaExceededException
[QuotaExceededException](/dotnet/api/microsoft.servicebus.messaging.quotaexceededexception) azt jelzi, hogy egy adott entitás kvóta túl lett lépve.

Ez akkor fordulhat elő, ha a fogadók (5) maximális száma hello már meg van nyitva a fogyasztó-csoport szinten.

### <a name="event-hubs"></a>Event Hubs
Az Event Hubs maximális hossza 20 felhasználói csoportot az Event Hubs egy rendelkezik. További toocreate kísérli meg, amikor egy [QuotaExceededException](/dotnet/api/microsoft.servicebus.messaging.quotaexceededexception). 

## <a name="timeoutexception"></a>TimeoutException
A [TimeoutException](https://msdn.microsoft.com/library/system.timeoutexception.aspx) azt jelzi, hogy egy felhasználó által kezdeményezett művelet a vártnál hello művelet időkorlátja lejár. 

Az Event Hubs számára megadott a hello időkorlát, vagy részeként hello kapcsolati karakterláncot, vagy keresztül [ServiceBusConnectionStringBuilder](/dotnet/api/microsoft.servicebus.servicebusconnectionstringbuilder). hello hibaüzenet maga is eltérők lehetnek, azonban mindig tartalmaz az aktuális hello a művelethez megadott hello időtúllépési érték. 

### <a name="common-causes"></a>Általános okok
Két közös Ez a hiba oka: helytelen konfiguráció, vagy a átmeneti hibája.

1. **Helytelen konfiguráció** hello művelet időkorlátja túl kicsi a hello működési feltétel lehet. hello alapértelmezett hello művelet időtúllépése hello ügyfél SDK értéke 60 másodperc. Annak ellenőrzése, ha a kód értéke hello toosee beállítva toosomething túl kicsi. Vegye figyelembe, hogy hello feltétele hello hálózati és a CPU-használat hatással lehet a hello időt egy művelet toocomplete, hello művelet időtúllépése nem állítható tooa nagyon kis értéket.
2. **Szolgáltatások átmeneti hiba** néha hello Event Hubs szolgáltatás is tapasztal késést, a kérelmek feldolgozásának, például nagy forgalom idején. Ebben az esetben akkor is újra a művelettel késleltetés után, amíg hello művelet sikeres nem lesz. Ha hello műveletet továbbra sem sikerül több próbálkozást követően, látogasson el a hello [Azure szolgáltatás állapota hely](https://azure.microsoft.com/status/) toosee, ha bármely ismert szolgáltatáskimaradások.

## <a name="serverbusyexception"></a>ServerBusyException

A [Microsoft.ServiceBus.Messaging.ServerBusyException](/dotnet/api/microsoft.servicebus.messaging.serverbusyexception) vagy [Microsoft.Azure.EventHubs.ServerBusyException](/dotnet/api/microsoft.azure.eventhubs.serverbusyexception) azt jelzi, hogy a kiszolgáló túlterhelt. Ehhez a kivételhez két kapcsolódó hibakódok van.

### <a name="error-code-50002"></a>50002. Hibakód:

Ez a hiba akkor fordulhat elő, két okai:

1. hello betöltése nem egyenletes eloszlású összes partíciójára hello Eseményközpont, és egy partíciót találatok hello helyi átviteli egység korlátozás.
    
    Megoldás: Hello partíció terjesztési stratégia módosítása, vagy próbálja meg elérni [EventHubClient.Send(eventDataWithOutPartitionKey)](/dotnet/api/microsoft.servicebus.messaging.eventhubclient#Microsoft_ServiceBus_Messaging_EventHubClient_Send_Microsoft_ServiceBus_Messaging_EventData_) segítséget.

2. hello Event Hubs névtér nem rendelkezik elegendő átviteli egységek (ellenőrizheti a hello **metrikák** panel az Event Hubs névtér panelen a hello [Azure-portálon](https://portal.azure.com) tooconfirm). Fontos megjegyezni, hogy hello portal összesített (1 perc) kapcsolatos adatokat jeleníti meg, hogy csak egy becsült valós idejű – hello átviteli mérjük.

    Megoldás: Hello átviteli egységek hello névtéren segíthet növelését. Ehhez a hello portálon, a hello **méretezési** panel hello Event Hubs névtér panelről.

### <a name="error-code-50001"></a>50001. Hibakód:

Ez a hiba a kell ritkán fordul elő. Akkor történik, ha a névtér kód futtatásával hello tároló nincs elég CPU – legfeljebb pár másodpercen hello előtt Eseményközpontok betöltése terheléselosztó kezdődik.


## <a name="next-steps"></a>Következő lépések
További információ az Event Hubs érhetők el a következő hivatkozások hello:

* [Event Hubs – áttekintés](event-hubs-what-is-event-hubs.md)
* [Event Hub létrehozása](event-hubs-create.md)
* [Event Hubs – gyakori kérdések](event-hubs-faq.md)
