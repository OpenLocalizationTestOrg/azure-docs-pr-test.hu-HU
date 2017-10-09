---
title: "a Service Bus üzenetküldési kivételek aaaAzure |} Microsoft Docs"
description: "A Service Bus üzenetküldési kivételeket és a javasolt műveletek listáját."
services: service-bus-messaging
documentationcenter: na
author: sethmanheim
manager: timlt
editor: 
ms.assetid: 3d8526fe-6e47-4119-9f3e-c56d916a98f9
ms.service: service-bus-messaging
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 06/06/2017
ms.author: sethm
ms.openlocfilehash: 0a206b7bbc808c1190044c1dfd6ffd47b9d454fb
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="service-bus-messaging-exceptions"></a>A Service Bus-alapú üzenetkezelés kivételei
Ez a cikk felsorolja hello Microsoft Azure Service Bus által létrehozott kivételek üzenetküldési API-k. Ez a hivatkozás tulajdonos toochange, így biztonsági frissítések keresése.

## <a name="exception-categories"></a>Kivétel kategóriák
hello üzenettovábbítási API-kat hozhat létre kivételeket, amelyek a következő kategóriák hello is tartozik, kapcsolódó hello művelet együtt is igénybe vehet tootry toofix őket. Vegye figyelembe, hogy hello, ami azt jelenti, és a kivétel leállítja üzenetküldési entitás (várólisták/témakörök vagy az Event Hubs) hello típusától függően eltérőek lehetnek:

1. Kódolási hiba felhasználói ([System.ArgumentException](https://msdn.microsoft.com/library/system.argumentexception.aspx), [System.InvalidOperationException](https://msdn.microsoft.com/library/system.invalidoperationexception.aspx), [System.OperationCanceledException](https://msdn.microsoft.com/library/system.operationcanceledexception.aspx), [System.Runtime.Serialization.SerializationException](https://msdn.microsoft.com/library/system.runtime.serialization.serializationexception.aspx)). Általános művelet: próbálja toofix hello kód a folytatás előtt.
2. A telepítő-konfigurációs hiba ([Microsoft.ServiceBus.Messaging.MessagingEntityNotFoundException](/dotnet/api/microsoft.servicebus.messaging.messagingentitynotfoundexception), [System.UnauthorizedAccessException](https://msdn.microsoft.com/library/system.unauthorizedaccessexception.aspx). Általános művelet: Ellenőrizze a konfigurációt, és szükség esetén módosítsa.
3. Átmeneti kivételek ([Microsoft.ServiceBus.Messaging.MessagingException](/dotnet/api/microsoft.servicebus.messaging.messagingexception), [Microsoft.ServiceBus.Messaging.ServerBusyException](/dotnet/api/microsoft.servicebus.messaging.serverbusyexception), [Microsoft.ServiceBus.Messaging.MessagingCommunicationException](/dotnet/api/microsoft.servicebus.messaging.messagingcommunicationexception)). Általános művelet: újra hello művelettel, vagy értesítse a felhasználókat.
4. Más kivételek ([System.Transactions.TransactionException](https://msdn.microsoft.com/library/system.transactions.transactionexception.aspx), [System.TimeoutException](https://msdn.microsoft.com/library/system.timeoutexception.aspx), [Microsoft.ServiceBus.Messaging.MessageLockLostException](/dotnet/api/microsoft.servicebus.messaging.messagelocklostexception), [Microsoft.ServiceBus.Messaging.SessionLockLostException](/dotnet/api/microsoft.servicebus.messaging.sessionlocklostexception)). Általános művelet: adott toohello kivételtípus; Tekintse meg a következő szakasz hello toohello táblájában. 

## <a name="exception-types"></a>Kivétel típusa
hello következő táblázatban üzenetkezelési kivétel típusait, és az okait és a megjegyzések javasolt művelet is igénybe vehet.

| **Kivétel típusa** | **Leírás/OK/példák** | **Javasolt művelet** | **Jegyezze fel az automatikus azonnali újrapróbálkozási** |
| --- | --- | --- | --- |
| [TimeoutException](https://msdn.microsoft.com/library/system.timeoutexception.aspx) |hello kiszolgáló nem válaszol a kért toohello belül hello művelet a megadott idő, amely vezérli [OperationTimeout](/dotnet/api/microsoft.servicebus.messaging.messagingfactorysettings#Microsoft_ServiceBus_Messaging_MessagingFactorySettings_OperationTimeout). Előfordulhat, hogy végrehajtotta a hello server hello a kért műveletet. Ez akkor fordulhat elő, toonetwork vagy más infrastruktúra késleltetése miatt. |Ellenőrizze a rendszerállapot hello konzisztencia, és szükség esetén próbálja megismételni. Lásd: [időtúllépési kivétel](#timeoutexception). |Újrapróbálkozási segíthet bizonyos esetekben; újrapróbálkozási logika toocode hozzáadása. |
| [InvalidOperationException](https://msdn.microsoft.com/library/system.invalidoperationexception.aspx) |a kért hello felhasználói művelet nem engedélyezett a következőben hello kiszolgálóra vagy szolgáltatásba. További részletek hello kivétel üzenet jelenik meg. Például [Complete](/dotnet/api/microsoft.servicebus.messaging.brokeredmessage#Microsoft_ServiceBus_Messaging_BrokeredMessage_Complete) ezt a kivételt hoz létre, ha az üdvözlő üzenet érkezett a [ReceiveAndDelete](/dotnet/api/microsoft.servicebus.messaging.receivemode) mód. |Ellenőrizze a hello kódot és hello dokumentációját. Ellenőrizze, hogy hello a kért művelet esetében érvényes. |Nem nyújt az újra gombra. |
| [OperationCanceledException](https://msdn.microsoft.com/library/system.operationcanceledexception.aspx) |Kísérlet az végrehajtott tooinvoke egy művelet olyan objektum, amely már lezárták, megszakítva vagy elvetése megtörtént. Ritka esetekben hello környezeti tranzakció már eldobták. |Ellenőrizze hello kódot, és győződjön meg arról, hogy nem lép működésbe műveleteket végez el egy teljesített objektum. |Nem nyújt az újra gombra. |
| [UnauthorizedAccessException](https://msdn.microsoft.com/library/system.unauthorizedaccessexception.aspx) |Hello [TokenProvider](/dotnet/api/microsoft.servicebus.tokenprovider) objektumot nem sikerült megszerezni a jogkivonatot, hello lexikális elem érvénytelen vagy hello jogkivonat nem tartalmaz hello jogcímek szükséges tooperform hello műveletet. |Ellenőrizze, hogy a jogkivonat-szolgáltató hello jön létre a hello helyes az értékük. A hozzáférés-vezérlés szolgáltatás hello hello konfigurációjának ellenőrzése. |Újrapróbálkozási segíthet bizonyos esetekben; újrapróbálkozási logika toocode hozzáadása. |
| [ArgumentException](https://msdn.microsoft.com/library/system.argumentexception.aspx)<br /> [ArgumentNullException](https://msdn.microsoft.com/library/system.argumentnullexception.aspx)<br />[ArgumentOutOfRangeException](https://msdn.microsoft.com/library/system.argumentoutofrangeexception.aspx) |Egy vagy több megadott argumentumokat toohello metódus nem érvényesek.<br /> hello URI megadva túl[NamespaceManager](/dotnet/api/microsoft.servicebus.namespacemanager) vagy [létrehozása](/dotnet/api/microsoft.servicebus.messaging.messagingfactory#Microsoft_ServiceBus_Messaging_MessagingFactory_Create_System_Collections_Generic_IEnumerable_System_Uri__) elérési szegmenssel tartalmazza.<br /> hello URI-séma túl megadott[NamespaceManager](/dotnet/api/microsoft.servicebus.namespacemanager) vagy [létrehozása](/dotnet/api/microsoft.servicebus.messaging.messagingfactory#Microsoft_ServiceBus_Messaging_MessagingFactory_Create_System_Collections_Generic_IEnumerable_System_Uri__) érvénytelen. <br />hello tulajdonság értéke nagyobb, mint 32 KB lehet. |Hello hívó kód ellenőrzése, illetve hogy hello argumentumok megfelelőek. |Nem nyújt az újra gombra. |
| [MessagingEntityNotFoundException](/dotnet/api/microsoft.servicebus.messaging.messagingentitynotfoundexception) |Hello művelethez társított entitás nem létezik, vagy már törölték. |Ellenőrizze, hogy hello entitás. |Nem nyújt az újra gombra. |
| [MessageNotFoundException](/dotnet/api/microsoft.servicebus.messaging.messagenotfoundexception) |Kísérlet történt egy üzenet tooreceive adott sorozatszáma. Ez az üzenet nem található. |Ellenőrizze, hogy hello üzenet már nem érkezett. Ha üdvözlőüzenetére deadlettered, ellenőrizze a hello kézbesítetlen levelek várólista toosee. |Nem nyújt az újra gombra. |
| [MessagingCommunicationException](/dotnet/api/microsoft.servicebus.messaging.messagingcommunicationexception) |Az ügyfél nem tud tooestablish egy kapcsolat tooService Bus áll. |Győződjön meg arról, hello megadott állomásnév helyességéről, valamint hello állomás érhető el. |Újrapróbálkozási segíthet, ha időszakos csatlakozási hibák. |
| [ServerBusyException](/dotnet/api/microsoft.servicebus.messaging.serverbusyexception) |Szolgáltatás jelenleg nem képes tooprocess hello kérelem. |Ügyfél Várjon egy ideig, majd próbálja megismételni a műveletet hello. |Ügyfél bizonyos időköz után újrapróbálkozik. Ha ismételt próbálkozással egy másik kivétel eredményez, ellenőrizze az újrapróbálási viselkedése, hogy a kivétel. |
| [MessageLockLostException](/dotnet/api/microsoft.servicebus.messaging.messagelocklostexception) |Üdvözlőüzenetére társított zárolási-token érvényessége lejárt, vagy hello zárolási jogkivonat nem található. |Üdvözlőüzenetére eldobásakor. |Nem nyújt az újra gombra. |
| [SessionLockLostException](/dotnet/api/microsoft.servicebus.messaging.sessionlocklostexception) |Ehhez a munkamenethez társított zárolási elvész. |Megszakítás hello [MessageSession](/dotnet/api/microsoft.servicebus.messaging.messagesession) objektum. |Nem nyújt az újra gombra. |
| [MessagingException](/dotnet/api/microsoft.servicebus.messaging.messagingexception) |Általános üzenetküldési, előfordulhat, hogy fel kell a következő esetekben hello kivétel:<br /> Toocreate tett kísérlet egy [QueueClient](/dotnet/api/microsoft.servicebus.messaging.queueclient) neve vagy elérési útja, amelyhez tartozik tooa különböző entitástípus (például egy témakör) segítségével.<br />  Kísérlet az végrehajtott toosend üzenet 256KB-nál nagyobb. hello kiszolgáló vagy a szolgáltatás hibát észlelt a hello kérés feldolgozása során. Ellenőrizze a részleteket hello Kivételüzenet. Ez általában az átmeneti kivétel. |Hello kód ellenőrzése elemre, és győződjön meg arról, hogy csak szerializálható objektumok hello üzenettörzs használja (vagy egy egyéni szerializáló használja). Ellenőrizze és csak használja a támogatott típusok hello támogatott érték esetében hello tulajdonságok hello dokumentációját. Ellenőrizze a hello [IsTransient](/dotnet/api/microsoft.servicebus.messaging.messagingexception#Microsoft_ServiceBus_Messaging_MessagingException_IsTransient) tulajdonság. Ha **igaz**, megpróbálkozhat a hello műveletet. |Újrapróbálási viselkedése nincs definiálva, és nem segítséget. |
| [MessagingEntityAlreadyExistsException](/dotnet/api/microsoft.servicebus.messaging.messagingentityalreadyexistsexception) |Kísérlet történt toocreate egy entitás, amelynek neve már használatban van egy másik entitás az adott szolgáltatás névtere. |Hello meglévő entitás törlése, vagy válasszon másik nevet a hello entitás toobe létrehozott. |Nem nyújt az újra gombra. |
| [QuotaExceededException](/dotnet/api/microsoft.servicebus.messaging.quotaexceededexception) |üzenetküldési entitás hello elérte a megengedett maximális méretet, vagy hello tooa névtér kapcsolatok maximális száma túl lett lépve. |Hozzon létre terület hello entitásból fogadó üzenetek vagy az alárendelt várólisták hello entitás. Lásd: [QuotaExceededException](#quotaexceededexception). |Újrapróbálkozási segíthet, ha üzenetek el lettek távolítva a hello addig is. |
| [RuleActionException](/dotnet/api/microsoft.servicebus.messaging.ruleactionexception) |Service Bus ezt a kivételt adja vissza, ha toocreate érvénytelen szabály művelet. Service Bus csatol az e tooa deadlettered Kivételüzenet üzenethez hello szabály hatására végrehajtott művelet feldolgozásakor hiba esetén. |Ellenőrizze a hello szabályművelet helyességét. |Nem nyújt az újra gombra. |
| [FilterException](/dotnet/api/microsoft.servicebus.messaging.filterexception) |A Service Bus ezt a kivételt adja vissza, ha toocreate egy érvénytelen szűrő. A Service Bus csatol a tooa deadlettered Kivételüzenet, ha hello szűrőt az üzenet feldolgozása közben hiba történt. |Ellenőrizze a hello szűrési helyességét. |Nem nyújt az újra gombra. |
| [SessionCannotBeLockedException](/dotnet/api/microsoft.servicebus.messaging.sessioncannotbelockedexception) |Kísérlet történt egy munkamenetet egy adott munkamenet-Azonosítót, de hello munkamenet jelenleg zárolva van egy másik ügyfél tooaccept. |Ellenőrizze, hogy hello munkamenet szerint más ügyfelek nincs zárolva. |Ha hello munkamenet már elérhető a közbenső hello újrapróbálkozási segítséget. |
| [TransactionSizeExceededException](/dotnet/api/microsoft.servicebus.messaging.transactionsizeexceededexception) |Túl sok művelet hello tranzakció részét képezik. |Csökkentse a tranzakció részét képező operations hello számát. |Nem nyújt az újra gombra. |
| [MessagingEntityDisabledException](/dotnet/api/microsoft.servicebus.messaging.messagingentitydisabledexception) |Egy futtatási művelet a letiltott entitásra vonatkozó kérést. |Hello entitás aktiválása. |Újrapróbálkozási segíthet, ha hello entitás ideiglenes hello az aktív. |
| [NoMatchingSubscriptionException](/dotnet/api/microsoft.servicebus.messaging.nomatchingsubscriptionexception) |Service Bus ezt a kivételt adja vissza, ha egy üzenet tooa témakör, amely előre szűrés engedélyezve van, és hello szűrők egyike küldi el. |Győződjön meg arról, hogy legalább egy szűrő, amely megfelel. |Nem nyújt az újra gombra. |
| [MessageSizeExceededException](/dotnet/api/microsoft.servicebus.messaging.messagesizeexceededexception) |Üzenetadatokat meghaladja hello 256 KB-os korlátot. Vegye figyelembe, hogy hello 256 KB-os korlát hello teljes üzenet mérete, ami magában foglalhatja a Rendszertulajdonságok és bármely .NET terhelés mellett. |Hello üzenetadatokat hello méretének csökkentésére, majd próbálja megismételni a műveletet hello. |Nem nyújt az újra gombra. |
| [TransactionException](https://msdn.microsoft.com/library/system.transactions.transactionexception.aspx) |hello környezeti tranzakció (*Transaction.Current*) érvénytelen. Előfordulhat, hogy befejeződött vagy megszakadt. További információt tartalmaznak a belső kivételben találhatók. | |Nem nyújt az újra gombra. |
| [TransactionInDoubtException](https://msdn.microsoft.com/library/system.transactions.transactionindoubtexception.aspx) |Egy művelet kísérletet egy tranzakció kimenetele kétséges, vagy toocommit hello tranzakció tett kísérlet, és hello tranzakció bizonytalan válik. |Az alkalmazás kezelni hello tranzakció már lehetett véglegesíteni kell (a egy különleges esetben), ezt a kivételt. |- |

## <a name="quotaexceededexception"></a>QuotaExceededException
[QuotaExceededException](/dotnet/api/microsoft.servicebus.messaging.quotaexceededexception) azt jelzi, hogy egy adott entitás kvóta túl lett lépve.

### <a name="queues-and-topics"></a>Üzenetsorok és témakörök
Üzenetsorok és témakörök ez pedig gyakran hello hello várólistájának méretével. hello hiba üzenettulajdonság további részletek, mint például a következő hello fogja tartalmazni:

```
Microsoft.ServiceBus.Messaging.QuotaExceededException
Message: hello maximum entity size has been reached or exceeded for Topic: ‘xxx-xxx-xxx’. 
    Size of entity in bytes:1073742326, Max entity size in bytes:
1073741824..TrackingId:xxxxxxxxxxxxxxxxxxxxxxxxxx, TimeStamp:3/15/2013 7:50:18 AM
```

hello üzenet jelzi, hogy hello témakör túllépte a maximális méretét, a nagybetűk (hello alapértelmezett méretkorlátot) 1 GB-ban. 

### <a name="namespaces"></a>Névterek

A névterek [QuotaExceededException](/dotnet/api/microsoft.servicebus.messaging.quotaexceededexception) azt jelezheti, hogy egy alkalmazás meghaladta hello tooa névtér kapcsolatok maximális számát. Példa:

```
Microsoft.ServiceBus.Messaging.QuotaExceededException: ConnectionsQuotaExceeded for namespace xxx.
<tracking-id-guid>_G12 ---> 
System.ServiceModel.FaultException`1[System.ServiceModel.ExceptionDetail]: 
ConnectionsQuotaExceeded for namespace xxx.
```

#### <a name="common-causes"></a>Általános okok
Két közös Ez a hiba oka: hello kézbesítetlen levelek várólistájára, és a nem működő üzenet fogadók.

1. **Kézbesítetlen levelek várólistájára** olvasó nem tud toocomplete üzenetek és köszönőüzenetei vissza toohello várólista/témakör hello zárolás lejár. Ez akkor fordulhat elő, ha a hello olvasó észlel, amelyek megakadályozzák a hívása kivételt [BrokeredMessage.Complete](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.brokeredmessage.complete.aspx). Miután egy olvasták 10-szer, alapértelmezés szerint helyezi toohello kézbesítetlen levelek várólistájára. Ez a viselkedés hello vezérli [QueueDescription.MaxDeliveryCount](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.queuedescription.maxdeliverycount.aspx) tulajdonság és alapértelmezett értéke 10. Üzenetek a hello halott levél várósor egymásra, mivel helyet foglaljanak.
   
    tooresolve hello probléma hello kézbesítetlen levelek várólistájára, ahogyan azt az olvasási és teljes köszönőüzenetei lenne más várólistából. Hello [QueueClient](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.queueclient.aspx) osztály is tartalmaz egy [FormatDeadLetterPath](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.queueclient.formatdeadletterpath.aspx) metódus toohelp formátum hello kézbesítetlen levelek várólistájára elérési útja.
2. **A fogadó leállt** A fogadó leállt üzenetek fogadása egy üzenetsorból vagy előfizetés. hello módon tooidentify Ez a következő hello toolook [QueueDescription.MessageCountDetails](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.queuedescription.messagecountdetails.aspx) tulajdonságot, amelynek köszönőüzenetei hello teljes bontását jeleníti meg. Ha hello [ActiveMessageCount](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.messagecountdetails.activemessagecount.aspx) tulajdonság magas vagy növekvő, akkor köszönőüzenetei nem olvas gyors most alatt állnak.

### <a name="event-hubs"></a>Event Hubs
Az Event Hubs maximális hossza 20 felhasználói csoportot az Event Hubs egy rendelkezik. További toocreate kísérli meg, amikor egy [QuotaExceededException](/dotnet/api/microsoft.servicebus.messaging.quotaexceededexception). 

## <a name="timeoutexception"></a>TimeoutException
A [TimeoutException](https://msdn.microsoft.com/library/system.timeoutexception.aspx) azt jelzi, hogy egy felhasználó által kezdeményezett művelet a vártnál hello művelet időkorlátja lejár. 

Ellenőrizni kell a hello hello értékének [ServicePointManager.DefaultConnectionLimit](https://msdn.microsoft.com/library/system.net.servicepointmanager.defaultconnectionlimit) is eredményezheti, hogy elérte-e ezt a határt, a tulajdonság egy [TimeoutException](https://msdn.microsoft.com/library/system.timeoutexception.aspx).

### <a name="queues-and-topics"></a>Üzenetsorok és témakörök
Az üzenetsoroktól és témaköröktől megadott hello időkorlát vagy hello [MessagingFactorySettings.OperationTimeout](/dotnet/api/microsoft.servicebus.messaging.messagingfactorysettings#Microsoft_ServiceBus_Messaging_MessagingFactorySettings_OperationTimeout) tulajdonság részeként hello kapcsolati karakterláncot, vagy keresztül [ServiceBusConnectionStringBuilder](/dotnet/api/microsoft.servicebus.servicebusconnectionstringbuilder). hello hibaüzenet maga is eltérők lehetnek, azonban mindig tartalmaz az aktuális hello a művelethez megadott hello időtúllépési érték. 

### <a name="event-hubs"></a>Event Hubs
Az Event Hubs számára megadott a hello időkorlát, vagy részeként hello kapcsolati karakterláncot, vagy keresztül [ServiceBusConnectionStringBuilder](/dotnet/api/microsoft.servicebus.servicebusconnectionstringbuilder). hello hibaüzenet maga is eltérők lehetnek, azonban mindig tartalmaz az aktuális hello a művelethez megadott hello időtúllépési érték. 

### <a name="common-causes"></a>Általános okok
Két közös Ez a hiba oka: helytelen konfiguráció, vagy a átmeneti hibája.

1. **Helytelen konfiguráció** hello művelet időkorlátja túl kicsi a hello működési feltétel lehet. hello alapértelmezett hello művelet időtúllépése hello ügyfél SDK értéke 60 másodperc. Annak ellenőrzése, ha a kód értéke hello toosee beállítva toosomething túl kicsi. Vegye figyelembe, hogy hello feltétele hello hálózati és a CPU-használat hatással lehet a hello időt egy művelet toocomplete, hello művelet időtúllépése nem állítható tooa nagyon kis értéket.
2. **Szolgáltatások átmeneti hiba** néha hello Service Bus szolgáltatás is tapasztal késést, a kérelmek feldolgozásának, például nagy forgalom idején. Ebben az esetben akkor is újra a művelettel késleltetés után, amíg hello művelet sikeres nem lesz. Ha hello műveletet továbbra sem sikerül több próbálkozást követően, látogasson el a hello [Azure szolgáltatás állapota hely](https://azure.microsoft.com/status/) toosee, ha bármely ismert szolgáltatáskimaradások.

## <a name="next-steps"></a>Következő lépések

Hello teljes Service Bus .NET API referenciáért lásd: hello [Azure .NET API-referencia](/dotnet/api/overview/azure/servicebus).

További információk toolearn [Service Bus](https://azure.microsoft.com/services/service-bus/), tekintse meg a következő témakörök hello.

* [Service Bus messaging overview](service-bus-messaging-overview.md) (A Service Bus üzenetkezelésének áttekintése)
* [A Service Bus alapjai](service-bus-fundamentals-hybrid-solutions.md)
* [Service Bus-architektúra](service-bus-architecture.md)

