---
title: "az Azure Service Bus-kérelem-válasz-alapú műveletekben 1.0 aaaAMQP |} Microsoft Docs"
description: "A Microsoft Azure Service Bus kérelem/válasz alapú műveletek listáját."
services: service-bus-messaging
documentationcenter: na
author: sethmanheim
manager: timlt
editor: 
ms.assetid: 
ms.service: service-bus-messaging
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 06/27/2017
ms.author: sethm
ms.openlocfilehash: e4f26219c53b0c4172747af683fe511d6366ff2d
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="amqp-10-in-microsoft-azure-service-bus-request-response-based-operations"></a>A Microsoft Azure Service Bus AMQP 1.0: kérelem-válasz-alapú műveletek

Ez a témakör a Microsoft Azure Service Bus kérelem/válasz alapú műveletek listájának hello határozza meg. Ez az információ hello AMQP felügyeleti 1.0-s verziójának működő vázlat alapul.  
  
Részletes vezetékszintű AMQP 1.0 protokoll, amelyből megtudhatja, hogyan Service Bus valósítja meg, és hello OASIS AMQP műszaki leírás épül, váltásról hello [protokoll útmutatóban Azure Service Bus és az Event Hubs AMQP 1.0-s](service-bus-amqp-protocol-guide.md).  
  
## <a name="concepts"></a>Alapelvek  
  
### <a name="entity-description"></a>Entitás leírása  

Egy entitás leírást hivatkozik egy Service Bus tooeither [QueueDescription osztály](/dotnet/api/microsoft.servicebus.messaging.queuedescription), [TopicDescription osztály](/dotnet/api/microsoft.servicebus.messaging.topicdescription), vagy [SubscriptionDescription osztály](/dotnet/api/microsoft.servicebus.messaging.subscriptiondescription) objektum.  
  
### <a name="brokered-message"></a>A közvetítőalapú üzenet  

A Service Busba, amely az csatlakoztatott tooan AMQP üzenet üzenet jelöli. hello leképezés van definiálva hello [Service Bus AMQP protokoll útmutató](service-bus-amqp-protocol-guide.md).  
  
## <a name="attach-tooentity-management-node"></a>Tooentity felügyeleti csomópont  

A jelen dokumentumban ismertetett összes hello műveletet hajtsa végre a kérelem/válasz minta, hatókörön belüli tooan entitást, és tooan entitás csomópontot csatolása igényel.  
  
### <a name="create-link-for-sending-requests"></a>Kérelmek küldése a hivatkozás létrehozása  

Létrehoz egy hivatkozás toohello felügyeleti csomóponton kérelmek küldéséhez.  
  
```  
requestLink = session.attach(     
role: SENDER,   
    target: { address: "<entity address>/$management" },   
    source: { address: ""<my request link unique address>" }   
)  
  
```  
  
### <a name="create-link-for-receiving-responses"></a>A válaszok hivatkozás létrehozása  

Kapcsolatot hoz létre a válasz fogadása hello csomópontot.  
  
```  
responseLink = session.attach(    
role: RECEIVER,   
    source: { address: "<entity address>/$management" }   
    target: { address: "<my response link unique address>" }   
)  
  
```  
  
### <a name="transfer-a-request-message"></a>Egy üzenet átvitele  

Egy üzenet továbbítja.  
  
```  
requestLink.sendTransfer(  
        Message(  
                properties: {  
                        message-id: <request id>,  
                        reply-to: "<my response link unique address>"  
                },  
                application-properties: {  
                        "operation" -> "<operation>",  
                },  
        )  
```  
  
### <a name="receive-a-response-message"></a>A válasz üzenet  

Hello válaszüzenetet kap hello válasz hivatkozásra.  
  
```  
responseMessage = responseLink.receiveTransfer()  
```  
  
hello válaszüzenetet hello a következő formában kell megadni:
  
```  
Message(  
properties: {     
        correlation-id: <request id>  
    },  
    application-properties: {  
            "statusCode" -> <status code>,  
            "statusDescription" -> <status description>,  
           },         
)  
  
```  
  
### <a name="service-bus-entity-address"></a>A Service Bus személy címe  

Service Bus-entitások az alábbiak szerint kell figyelembe venni:  
  
|Entitás típusa|Cím|Példa|  
|-----------------|-------------|-------------|  
|Várólista|`<queue_name>`|`“myQueue”`<br /><br /> `“site1/myQueue”`|  
|A témakör|`<topic_name>`|`“myTopic”`<br /><br /> `“site2/page1/myQueue”`|  
|előfizetést|`<topic_name>/Subscriptions/<subscription_name>`|`“myTopic/Subscriptions/MySub”`|  
  
## <a name="message-operations"></a>Üzenetművelet  
  
### <a name="message-renew-lock"></a>Üzenet megújítása zárolása  

Kiterjeszti hello zárolása, egy üzenet hello idő hello entitás leírásban szerepel.  
  
#### <a name="request"></a>Kérés  

hello kérelemüzenet tartalmaznia kell a következő alkalmazástulajdonságok hello:  
  
|Kulcs|Érték típusa|Szükséges|Érték tartalma|  
|---------|----------------|--------------|--------------------|  
|művelet|Karakterlánc|Igen|`com.microsoft:renew-lock`|  
|`com.microsoft:server-timeout`|uint|Nem|A művelet kiszolgáló időtúllépése milliszekundumban.|  
  
 hello kérelem üzenettörzs egy térképre tartalmazó bejegyzéseket a következő hello amqp-érték szakaszból áll:  
  
|Kulcs|Érték típusa|Szükséges|Érték tartalma|  
|---------|----------------|--------------|--------------------|  
|`lock-tokens`|uuid tömbje|Igen|Üzenet zárolási jogkivonatok toorenew.|  
  
#### <a name="response"></a>Válasz  

hello válaszüzenetet tartalmaznia kell a következő alkalmazástulajdonságok hello:  
  
|Kulcs|Érték típusa|Szükséges|Érték tartalma|  
|---------|----------------|--------------|--------------------|  
|statusCode|int|Igen|HTTP-válaszkód [RFC2616]<br /><br /> 200: OK – sikeres, ellenkező esetben nem sikerült.|  
|StatusDescription|Karakterlánc|Nem|Hello állapot leírása.|  
  
hello válasz az üzenet törzse egy térképre tartalmazó bejegyzéseket a következő hello amqp-érték szakaszból áll:  
  
|Kulcs|Érték típusa|Szükséges|Érték tartalma|  
|---------|----------------|--------------|--------------------|  
|lejáratok|Timestamp típusú tömb|Igen|Üzenet zárolási token új lejárati megfelelő toohello zárolási jogkivonatok.|  
  
### <a name="peek-message"></a>Üzenet megtekintése  

Szinkron üzenetek nélkül zárolását.  
  
#### <a name="request"></a>Kérés  

hello kérelemüzenet tartalmaznia kell a következő alkalmazástulajdonságok hello:  
  
|Kulcs|Érték típusa|Szükséges|Érték tartalma|  
|---------|----------------|--------------|--------------------|  
|művelet|Karakterlánc|Igen|`com.microsoft:peek-message`|  
|`com.microsoft:server-timeout`|uint|Nem|A művelet kiszolgáló időtúllépése milliszekundumban.|  
  
hello kérelem üzenettörzs kell állnia egy **amqp-érték** tartalmazó szakasz egy **térkép** a hello bejegyzéseket a következő:  
  
|Kulcs|Érték típusa|Szükséges|Érték tartalma|  
|---------|----------------|--------------|--------------------|  
|`from-sequence-number`|hosszú|Igen|Mely toostart betekintés a sorszám.|  
|`message-count`|int|Igen|Az üzenetek toopeek maximális számát.|  
  
#### <a name="response"></a>Válasz  

hello válaszüzenetet tartalmaznia kell a következő alkalmazástulajdonságok hello:  
  
|Kulcs|Érték típusa|Szükséges|Érték tartalma|  
|---------|----------------|--------------|--------------------|  
|statusCode|int|Igen|HTTP-válaszkód [RFC2616]<br /><br /> 200: OK – a további üzeneteket tartalmaz<br /><br /> 0xcc: nem tartalom – nincs további üzenetek|  
|StatusDescription|Karakterlánc|Nem|Hello állapot leírása.|  
  
hello válasz üzenettörzs kell állnia egy **amqp-érték** tartalmazó szakasz egy **térkép** a hello bejegyzéseket a következő:  
  
|Kulcs|Érték típusa|Szükséges|Érték tartalma|  
|---------|----------------|--------------|--------------------|  
|üzenet|a maps listája|Igen|Minden térkép jelöl egy üzenet üzenetek listáját.|  
  
egy üzenet képviselő hello térképnek tartalmaznia kell a következő tételek hello:  
  
|Kulcs|Érték típusa|Szükséges|Érték tartalma|  
|---------|----------------|--------------|--------------------|  
|Üzenet|a tömb bájt|Igen|AMQP 1.0-s átviteli kódolású üzenet.|  
  
### <a name="schedule-message"></a>Ütemezés üzenet  

Üzenetek ütemezi.  
  
#### <a name="request"></a>Kérés  

hello kérelemüzenet tartalmaznia kell a következő alkalmazástulajdonságok hello:  
  
|Kulcs|Érték típusa|Szükséges|Érték tartalma|  
|---------|----------------|--------------|--------------------|  
|művelet|Karakterlánc|Igen|`com.microsoft:schedule-message`|  
|`com.microsoft:server-timeout`|uint|Nem|A művelet kiszolgáló időtúllépése milliszekundumban.|  
  
hello kérelem üzenettörzs kell állnia egy **amqp-érték** tartalmazó szakasz egy **térkép** a hello bejegyzéseket a következő:  
  
|Kulcs|Érték típusa|Szükséges|Érték tartalma|  
|---------|----------------|--------------|--------------------|  
|üzenet|a maps listája|Igen|Minden térkép jelöl egy üzenet üzenetek listáját.|  
  
egy üzenet képviselő hello térképnek tartalmaznia kell a következő tételek hello:  
  
|Kulcs|Érték típusa|Szükséges|Érték tartalma|  
|---------|----------------|--------------|--------------------|  
|üzenetazonosító|Karakterlánc|Igen|`amqpMessage.Properties.MessageId`karakterlánc|  
|munkamenet-azonosító|Karakterlánc|Igen|`amqpMessage.Properties.GroupId as string`|  
|a partíciókulcs|Karakterlánc|Igen|`amqpMessage.MessageAnnotations.”x-opt-partition-key"`|  
|Üzenet|a tömb bájt|Igen|AMQP 1.0-s átviteli kódolású üzenet.|  
  
#### <a name="response"></a>Válasz  

hello válaszüzenetet tartalmaznia kell a következő alkalmazástulajdonságok hello:  
  
|Kulcs|Érték típusa|Szükséges|Érték tartalma|  
|---------|----------------|--------------|--------------------|  
|statusCode|int|Igen|HTTP-válaszkód [RFC2616]<br /><br /> 200: OK – sikeres, ellenkező esetben nem sikerült.|  
|StatusDescription|Karakterlánc|Nem|Hello állapot leírása.|  
  
hello válasz üzenettörzs kell állnia egy **amqp-érték** térképre tartalmazó hello bejegyzéseket a következő szakaszban:  
  
|Kulcs|Érték típusa|Szükséges|Érték tartalma|  
|---------|----------------|--------------|--------------------|  
|sorozatszámok|a hosszú tömb|Igen|Ütemezett üzenetek számát. Sorszám használt toocancel.|  
  
### <a name="cancel-scheduled-message"></a>Ütemezett üzenet törlése  

Megszakítja az üzenetek ütemezett.  
  
#### <a name="request"></a>Kérés  

hello kérelemüzenet tartalmaznia kell a következő alkalmazástulajdonságok hello:  
  
|Kulcs|Érték típusa|Szükséges|Érték tartalma|  
|---------|----------------|--------------|--------------------|  
|művelet|Karakterlánc|Igen|`com.microsoft:cancel-scheduled-message`|  
|`com.microsoft:server-timeout`|uint|Nem|A művelet kiszolgáló időtúllépése milliszekundumban.|  
  
hello kérelem üzenettörzs kell állnia egy **amqp-érték** tartalmazó szakasz egy **térkép** a hello bejegyzéseket a következő:  
  
|Kulcs|Érték típusa|Szükséges|Érték tartalma|  
|---------|----------------|--------------|--------------------|  
|sorozatszámok|a hosszú tömb|Igen|Ütemezett üzenetek toocancel sorszámok.|  
  
#### <a name="response"></a>Válasz  

hello válaszüzenetet tartalmaznia kell a következő alkalmazástulajdonságok hello:  
  
|Kulcs|Érték típusa|Szükséges|Érték tartalma|  
|---------|----------------|--------------|--------------------|  
|statusCode|int|Igen|HTTP-válaszkód [RFC2616]<br /><br /> 200: OK – sikeres, ellenkező esetben nem sikerült.|  
|StatusDescription|Karakterlánc|Nem|Hello állapot leírása.|  
  
hello válasz üzenettörzs kell állnia egy **amqp-érték** térképre tartalmazó hello bejegyzéseket a következő szakaszban:  
  
|Kulcs|Érték típusa|Szükséges|Érték tartalma|  
|---------|----------------|--------------|--------------------|  
|sorozatszámok|a hosszú tömb|Igen|Ütemezett üzenetek számát. Sorszám használt toocancel.|  
  
## <a name="session-operations"></a>Munkamenet-műveletek  
  
### <a name="session-renew-lock"></a>Munkamenet Renew zárolása  

Kiterjeszti hello zárolása, egy üzenet hello idő hello entitás leírásban szerepel.  
  
#### <a name="request"></a>Kérés  

hello kérelemüzenet tartalmaznia kell a következő alkalmazástulajdonságok hello:  
  
|Kulcs|Érték típusa|Szükséges|Érték tartalma|  
|---------|----------------|--------------|--------------------|  
|művelet|Karakterlánc|Igen|`com.microsoft:renew-session-lock`|  
|`com.microsoft:server-timeout`|uint|Nem|A művelet kiszolgáló időtúllépése milliszekundumban.|  
  
hello kérelem üzenettörzs kell állnia egy **amqp-érték** tartalmazó szakasz egy **térkép** a hello bejegyzéseket a következő:  
  
|Kulcs|Érték típusa|Szükséges|Érték tartalma|  
|---------|----------------|--------------|--------------------|  
|munkamenet-azonosító|Karakterlánc|Igen|Munkamenet-azonosítót.|  
  
#### <a name="response"></a>Válasz  

hello válaszüzenetet tartalmaznia kell a következő alkalmazástulajdonságok hello:  
  
|Kulcs|Érték típusa|Szükséges|Érték tartalma|  
|---------|----------------|--------------|--------------------|  
|statusCode|int|Igen|HTTP-válaszkód [RFC2616]<br /><br /> 200: OK – a további üzeneteket tartalmaz<br /><br /> 0xcc: nem tartalom – nincs további üzenetek|  
|StatusDescription|Karakterlánc|Nem|Hello állapot leírása.|  
  
hello válasz üzenettörzs kell állnia egy **amqp-érték** térképre tartalmazó hello bejegyzéseket a következő szakaszban:  
  
|Kulcs|Érték típusa|Szükséges|Érték tartalma|  
|---------|----------------|--------------|--------------------|  
|lejárati|időbélyeg|Igen|Új lejárati.|  
  
### <a name="peek-session-message"></a>Munkamenet-üzenet megtekintése  

Munkamenet-üzenetek szinkron nélküli.  
  
#### <a name="request"></a>Kérés  

hello kérelemüzenet tartalmaznia kell a következő alkalmazástulajdonságok hello:  
  
|Kulcs|Érték típusa|Szükséges|Érték tartalma|  
|---------|----------------|--------------|--------------------|  
|művelet|Karakterlánc|Igen|`com.microsoft:peek-message`|  
|`com.microsoft:server-timeout`|uint|Nem|A művelet kiszolgáló időtúllépése milliszekundumban.|  
  
hello kérelem üzenettörzs kell állnia egy **amqp-érték** tartalmazó szakasz egy **térkép** a hello bejegyzéseket a következő:  
  
|Kulcs|Érték típusa|Szükséges|Érték tartalma|  
|---------|----------------|--------------|--------------------|  
|a-sorozat-szám|hosszú|Igen|Mely toostart betekintés a sorszám.|  
|üzenet-száma|int|Igen|Az üzenetek toopeek maximális számát.|  
|munkamenet-azonosító|Karakterlánc|Igen|Munkamenet-azonosítót.|  
  
#### <a name="response"></a>Válasz  

hello válaszüzenetet tartalmaznia kell a következő alkalmazástulajdonságok hello:  
  
|Kulcs|Érték típusa|Szükséges|Érték tartalma|  
|---------|----------------|--------------|--------------------|  
|statusCode|int|Igen|HTTP-válaszkód [RFC2616]<br /><br /> 200: OK – a további üzeneteket tartalmaz<br /><br /> 0xcc: nem tartalom – nincs további üzenetek|  
|StatusDescription|Karakterlánc|Nem|Hello állapot leírása.|  
  
hello válasz üzenettörzs kell állnia egy **amqp-érték** térképre tartalmazó hello bejegyzéseket a következő szakaszban:  
  
|Kulcs|Érték típusa|Szükséges|Érték tartalma|  
|---------|----------------|--------------|--------------------|  
|üzenet|a maps listája|Igen|Minden térkép jelöl egy üzenet üzenetek listáját.|  
  
 egy üzenet képviselő hello térképnek tartalmaznia kell a következő tételek hello:  
  
|Kulcs|Érték típusa|Szükséges|Érték tartalma|  
|---------|----------------|--------------|--------------------|  
|Üzenet|a tömb bájt|Igen|AMQP 1.0-s átviteli kódolású üzenet.|  
  
### <a name="set-session-state"></a>Munkamenet-állapot beállítása  

Készletek hello a munkamenet állapotát.  
  
#### <a name="request"></a>Kérés  

hello kérelemüzenet tartalmaznia kell a következő alkalmazástulajdonságok hello:  
  
|Kulcs|Érték típusa|Szükséges|Érték tartalma|  
|---------|----------------|--------------|--------------------|  
|művelet|Karakterlánc|Igen|`com.microsoft:peek-message`|  
|`com.microsoft:server-timeout`|uint|Nem|A művelet kiszolgáló időtúllépése milliszekundumban.|  
  
hello kérelem üzenettörzs kell állnia egy **amqp-érték** tartalmazó szakasz egy **térkép** a hello bejegyzéseket a következő:  
  
|Kulcs|Érték típusa|Szükséges|Érték tartalma|  
|---------|----------------|--------------|--------------------|  
|munkamenet-azonosító|Karakterlánc|Igen|Munkamenet-azonosítót.|  
|a munkamenet-állapot|bájttömb|Igen|Nem átlátszó bináris adatok.|  
  
#### <a name="response"></a>Válasz  

hello válaszüzenetet tartalmaznia kell a következő alkalmazástulajdonságok hello:  
  
|Kulcs|Érték típusa|Szükséges|Érték tartalma|  
|---------|----------------|--------------|--------------------|  
|statusCode|int|Igen|HTTP-válaszkód [RFC2616]<br /><br /> 200: OK – sikeres, ellenkező esetben nem sikerült|  
|StatusDescription|Karakterlánc|Nem|Hello állapot leírása.|  
  
### <a name="get-session-state"></a>A munkamenet-állapot beolvasása  

Egy munkamenet hello állapotának beolvasása.  
  
#### <a name="request"></a>Kérés  

hello kérelemüzenet tartalmaznia kell a következő alkalmazástulajdonságok hello:  
  
|Kulcs|Érték típusa|Szükséges|Érték tartalma|  
|---------|----------------|--------------|--------------------|  
|művelet|Karakterlánc|Igen|`com.microsoft:get-session-state`|  
|`com.microsoft:server-timeout`|uint|Nem|A művelet kiszolgáló időtúllépése milliszekundumban.|  
  
hello kérelem üzenettörzs kell állnia egy **amqp-érték** tartalmazó szakasz egy **térkép** a hello bejegyzéseket a következő:  
  
|Kulcs|Érték típusa|Szükséges|Érték tartalma|  
|---------|----------------|--------------|--------------------|  
|munkamenet-azonosító|Karakterlánc|Igen|Munkamenet-azonosítót.|  
  
#### <a name="response"></a>Válasz  

hello válaszüzenetet tartalmaznia kell a következő alkalmazástulajdonságok hello:  
  
|Kulcs|Érték típusa|Szükséges|Érték tartalma|  
|---------|----------------|--------------|--------------------|  
|statusCode|int|Igen|HTTP-válaszkód [RFC2616]<br /><br /> 200: OK – sikeres, ellenkező esetben nem sikerült|  
|StatusDescription|Karakterlánc|Nem|Hello állapot leírása.|  
  
hello válasz üzenettörzs kell állnia egy **amqp-érték** tartalmazó szakasz egy **térkép** a hello bejegyzéseket a következő:  
  
|Kulcs|Érték típusa|Szükséges|Érték tartalma|  
|---------|----------------|--------------|--------------------|  
|a munkamenet-állapot|bájttömb|Igen|Nem átlátszó bináris adatok.|  
  
### <a name="enumerate-sessions"></a>Munkamenetek enumerálása  

Egy üzenetküldési entitásra munkamenetek enumerálása.  
  
#### <a name="request"></a>Kérés  

hello kérelemüzenet tartalmaznia kell a következő alkalmazástulajdonságok hello:  
  
|Kulcs|Érték típusa|Szükséges|Érték tartalma|  
|---------|----------------|--------------|--------------------|  
|művelet|Karakterlánc|Igen|`com.microsoft:get-message-sessions`|  
|`com.microsoft:server-timeout`|uint|Nem|A művelet kiszolgáló időtúllépése milliszekundumban.|  
  
hello kérelem üzenettörzs kell állnia egy **amqp-érték** tartalmazó szakasz egy **térkép** a hello bejegyzéseket a következő:  
  
|Kulcs|Érték típusa|Szükséges|Érték tartalma|  
|---------|----------------|--------------|--------------------|  
|legutóbbi frissítése idő|időbélyeg|Igen|Szűrés tooinclude csak munkamenetek frissíteni egy adott idő után.|  
|Kihagyása|int|Igen|Adott számú munkamenetet kihagyása.|  
|Felső|int|Igen|Munkamenetek maximális számát.|  
  
#### <a name="response"></a>Válasz  

hello válaszüzenetet tartalmaznia kell a következő alkalmazástulajdonságok hello:  
  
|Kulcs|Érték típusa|Szükséges|Érték tartalma|  
|---------|----------------|--------------|--------------------|  
|statusCode|int|Igen|HTTP-válaszkód [RFC2616]<br /><br /> 200: OK – a további üzeneteket tartalmaz<br /><br /> 0xcc: nem tartalom – nincs további üzenetek|  
|StatusDescription|Karakterlánc|Nem|Hello állapot leírása.|  
  
hello válasz üzenettörzs kell állnia egy **amqp-érték** tartalmazó szakasz egy **térkép** a hello bejegyzéseket a következő:  
  
|Kulcs|Érték típusa|Szükséges|Érték tartalma|  
|---------|----------------|--------------|--------------------|  
|Kihagyása|int|Igen|Ha állapotkód 200 kihagyott munkamenetek száma.|  
|munkamenet-azonosítók|Karakterláncok|Igen|Munkamenet-azonosítókat, ha állapotkód 200 tömbje.|  
  
## <a name="rule-operations"></a>Műveletek  
  
### <a name="add-rule"></a>Szabály hozzáadása  
  
#### <a name="request"></a>Kérés  

hello kérelemüzenet tartalmaznia kell a következő alkalmazástulajdonságok hello:  
  
|Kulcs|Érték típusa|Szükséges|Érték tartalma|  
|---------|----------------|--------------|--------------------|  
|művelet|Karakterlánc|Igen|`com.microsoft:add-rule`|  
|`com.microsoft:server-timeout`|uint|Nem|A művelet kiszolgáló időtúllépése milliszekundumban.|  
  
hello kérelem üzenettörzs kell állnia egy **amqp-érték** tartalmazó szakasz egy **térkép** a hello bejegyzéseket a következő:  
  
|Kulcs|Érték típusa|Szükséges|Érték tartalma|  
|---------|----------------|--------------|--------------------|  
|a szabály-név|Karakterlánc|Igen|A szabály nevét, nem beleértve az előfizetés és a témakör neve.|  
|a szabály leírása|térkép|Igen|Szabály a következő szakaszban megadott leírása.|  
  
Hello **szabályleírás** térkép tartalmaznia kell bejegyzést, a következő hello ahol **sql-szűrő** és **korreláció-szűrő** kölcsönösen kizárják egymást:  
  
|Kulcs|Érték típusa|Szükséges|Érték tartalma|  
|---------|----------------|--------------|--------------------|  
|SQL-szűrő|térkép|Igen|`sql-filter`, hello a következő szakaszban meghatározott.|  
|korreláció-szűrő|térkép|Igen|`correlation-filter`, hello a következő szakaszban meghatározott.|  
|SQL-szabályművelet|térkép|Igen|`sql-rule-action`, hello a következő szakaszban meghatározott.|  
  
hello sql-szűrő térkép tartalmaznia kell a következő tételek hello:  
  
|Kulcs|Érték típusa|Szükséges|Érték tartalma|  
|---------|----------------|--------------|--------------------|  
|kifejezés|Karakterlánc|Igen|SQL szűrőkifejezés.|  
  
Hello **korreláció-szűrő** térkép tartalmaznia kell legalább egy bejegyzést a következő hello:  
  
|Kulcs|Érték típusa|Szükséges|Érték tartalma|  
|---------|----------------|--------------|--------------------|  
|korrelációs azonosító|Karakterlánc|Nem||  
|üzenetazonosító|Karakterlánc|Nem||  
|erre:|Karakterlánc|Nem||  
|Válaszcím|Karakterlánc|Nem||  
|Címke|Karakterlánc|Nem||  
|munkamenet-azonosító|Karakterlánc|Nem||  
|válasz a munkamenet azonosítója|Karakterlánc|Nem||  
|tartalomtípus|Karakterlánc|Nem||  
|properties|térkép|Nem|TooService Bus leképezhető [BrokeredMessage.Properties](/dotnet/api/microsoft.servicebus.messaging.brokeredmessage#Microsoft_ServiceBus_Messaging_BrokeredMessage_Properties).|  
  
Hello **sql szabályművelet** térkép tartalmaznia kell a következő tételek hello:  
  
|Kulcs|Érték típusa|Szükséges|Érték tartalma|  
|---------|----------------|--------------|--------------------|  
|kifejezés|Karakterlánc|Igen|SQL-művelet kifejezést.|  
  
#### <a name="response"></a>Válasz  

hello válaszüzenetet tartalmaznia kell a következő alkalmazástulajdonságok hello:  
  
|Kulcs|Érték típusa|Szükséges|Érték tartalma|  
|---------|----------------|--------------|--------------------|  
|statusCode|int|Igen|HTTP-válaszkód [RFC2616]<br /><br /> 200: OK – sikeres, ellenkező esetben nem sikerült|  
|StatusDescription|Karakterlánc|Nem|Hello állapot leírása.|  
  
### <a name="remove-rule"></a>Szabály eltávolítása  
  
#### <a name="request"></a>Kérés  

hello kérelemüzenet tartalmaznia kell a következő alkalmazástulajdonságok hello:  
  
|Kulcs|Érték típusa|Szükséges|Érték tartalma|  
|---------|----------------|--------------|--------------------|  
|művelet|Karakterlánc|Igen|`com.microsoft:remove-rule`|  
|`com.microsoft:server-timeout`|uint|Nem|A művelet kiszolgáló időtúllépése milliszekundumban.|  
  
hello kérelem üzenettörzs kell állnia egy **amqp-érték** tartalmazó szakasz egy **térkép** a hello bejegyzéseket a következő:  
  
|Kulcs|Érték típusa|Szükséges|Érték tartalma|  
|---------|----------------|--------------|--------------------|  
|a szabály-név|Karakterlánc|Igen|A szabály nevét, nem beleértve az előfizetés és a témakör neve.|  
  
#### <a name="response"></a>Válasz  

hello válaszüzenetet tartalmaznia kell a következő alkalmazástulajdonságok hello:  
  
|Kulcs|Érték típusa|Szükséges|Érték tartalma|  
|---------|----------------|--------------|--------------------|  
|statusCode|int|Igen|HTTP-válaszkód [RFC2616]<br /><br /> 200: OK – sikeres, ellenkező esetben nem sikerült|  
|StatusDescription|Karakterlánc|Nem|Hello állapot leírása.|  
  
## <a name="deferred-message-operations"></a>A késleltetett üzenetművelet  
  
### <a name="receive-by-sequence-number"></a>Fogadási sorszáma szerint  

Sorszám által késleltetett üzeneteket fogad.  
  
#### <a name="request"></a>Kérés  

hello kérelemüzenet tartalmaznia kell a következő alkalmazástulajdonságok hello:  
  
|Kulcs|Érték típusa|Szükséges|Érték tartalma|  
|---------|----------------|--------------|--------------------|  
|művelet|Karakterlánc|Igen|`com.microsoft:receive-by-sequence-number`|  
|`com.microsoft:server-timeout`|uint|Nem|A művelet kiszolgáló időtúllépése milliszekundumban.|  
  
hello kérelem üzenettörzs kell állnia egy **amqp-érték** tartalmazó szakasz egy **térkép** a hello bejegyzéseket a következő:  
  
|Kulcs|Érték típusa|Szükséges|Érték tartalma|  
|---------|----------------|--------------|--------------------|  
|sorozatszámok|a hosszú tömb|Igen|Sorozatszámok száma.|  
|fogadó módú kiegyenlítéséhez|ubyte|Igen|**Fogadó kiegyenlítéséhez** AMQP 1.0-s core meghatározott módban.|  
  
#### <a name="response"></a>Válasz  

hello válaszüzenetet tartalmaznia kell a következő alkalmazástulajdonságok hello:  
  
|Kulcs|Érték típusa|Szükséges|Érték tartalma|  
|---------|----------------|--------------|--------------------|  
|statusCode|int|Igen|HTTP-válaszkód [RFC2616]<br /><br /> 200: OK – sikeres, ellenkező esetben nem sikerült|  
|StatusDescription|Karakterlánc|Nem|Hello állapot leírása.|  
  
hello válasz üzenettörzs kell állnia egy **amqp-érték** tartalmazó szakasz egy **térkép** a hello bejegyzéseket a következő:  
  
|Kulcs|Érték típusa|Szükséges|Érték tartalma|  
|---------|----------------|--------------|--------------------|  
|üzenet|a maps listája|Igen|Ha minden térkép jelöli egy üzenet üzenetek listáját.|  
  
egy üzenet képviselő hello térképnek tartalmaznia kell a következő tételek hello:  
  
|Kulcs|Érték típusa|Szükséges|Érték tartalma|  
|---------|----------------|--------------|--------------------|  
|zárolási-jogkivonat|UUID|Igen|Zárolási token if `receiver-settle-mode` 1.|  
|Üzenet|a tömb bájt|Igen|AMQP 1.0-s átviteli kódolású üzenet.|  
  
### <a name="update-disposition-status"></a>Témakör állapotának frissítése  

A frissítések hello törlése állapotának késleltetett üzenetek.  
  
#### <a name="request"></a>Kérés  

hello kérelemüzenet tartalmaznia kell a következő alkalmazástulajdonságok hello:  
  
|Kulcs|Érték típusa|Szükséges|Érték tartalma|  
|---------|----------------|--------------|--------------------|  
|művelet|Karakterlánc|Igen|`com.microsoft:update-disposition`|  
|`com.microsoft:server-timeout`|uint|Nem|A művelet kiszolgáló időtúllépése milliszekundumban.|  
  
hello kérelem üzenettörzs kell állnia egy **amqp-érték** tartalmazó szakasz egy **térkép** a hello bejegyzéseket a következő:  
  
|Kulcs|Érték típusa|Szükséges|Érték tartalma|  
|---------|----------------|--------------|--------------------|  
|témakör-állapot|Karakterlánc|Igen|Befejeződött<br /><br /> Elhagyott<br /><br /> Felfüggesztve|  
|zárolási-tokenek|uuid tömbje|Igen|Üzenet zár jogkivonatok tooupdate törlése állapota.|  
|kézbesítetlen levelek-OK|Karakterlánc|Nem|Szabályozó beállítás túl lehet beállítani**felfüggesztve**.|  
|kézbesítetlen levelek – leírás|Karakterlánc|Nem|Szabályozó beállítás túl lehet beállítani**felfüggesztve**.|  
|tulajdonságok-módosítása|térkép|Nem|Lista a Service Bus közvetítőalapú üzenet tulajdonságai toomodify.|  
  
#### <a name="response"></a>Válasz  

hello válaszüzenetet tartalmaznia kell a következő alkalmazástulajdonságok hello:  
  
|Kulcs|Érték típusa|Szükséges|Érték tartalma|  
|---------|----------------|--------------|--------------------|  
|statusCode|int|Igen|HTTP-válaszkód [RFC2616]<br /><br /> 200: OK – sikeres, ellenkező esetben nem sikerült|  
|StatusDescription|Karakterlánc|Nem|Hello állapot leírása.|

## <a name="next-steps"></a>Következő lépések

További információ az amqp-t és a Service Bus toolearn látogasson el a következő hivatkozások hello:

* [Service Bus AMQP áttekintése]
* [Particionált Service Bus-üzenetsorok és témakörök AMQP 1.0 támogatása]
* [A Service Bus a Windows Server AMQP]

[Service Bus AMQP áttekintése]: service-bus-amqp-overview.md
[Particionált Service Bus-üzenetsorok és témakörök AMQP 1.0 támogatása]: service-bus-partitioned-queues-and-topics-amqp-overview.md
[A Service Bus a Windows Server AMQP]: https://msdn.microsoft.com/library/dn574799.asp