---
title: "aaaSend események tooAzure Event Hubs Java használatával |} Microsoft Docs"
description: "Ismerkedés a küldő tooEvent Hubs Java használatával"
services: event-hubs
documentationcenter: 
author: sethmanheim
manager: timlt
editor: 
ms.assetid: 
ms.service: event-hubs
ms.workload: core
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/15/2017
ms.author: sethm
ms.openlocfilehash: ec537b8849a0cb49855e76c0c0ef4093108fe83c
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="send-events-tooazure-event-hubs-using-java"></a>Események küldése tooAzure Event Hubs Java használatával

## <a name="introduction"></a>Bevezetés
Az Event Hubs egy kiválóan méretezhető fogadórendszer, melyek több millió eseményt másodpercenként, egy alkalmazás tooprocess engedélyezése és elemezheti az adatokat a csatlakoztatott eszközök és alkalmazások által létrehozott nagy mennyiségű hello. Miután egy eseményközpontba való összegyűjtését, átalakítás és tárolására is használható adatok bármilyen valós idejű elemzési szolgáltató vagy tárolási fürt használatával.

További információkért lásd: hello [Event Hubs – áttekintés][Event Hubs overview].

Ez az oktatóanyag bemutatja, hogyan toosend események tooan event hubs egy Java-Konzolalkalmazás használatával. tooreceive események hello Java Event Processor Host szalagtár használata esetén lásd: [Ez a cikk](event-hubs-java-get-started-receive-eph.md), vagy kattintson a hello fogadó megfelelő nyelvi hello bal oldali tábla tartalmának.

A sorrend toocomplete ebben az oktatóanyagban kell a következő hello:

* A Java-fejlesztőkörnyezet. Ebben az oktatóanyagban feltételezzük, hogy [Eclipse](https://www.eclipse.org/).
* Aktív Azure-fiók. <br/>Ha nincs fiókja, néhány perc alatt létrehozhat egy ingyenes fiókot. További információkért lásd: <a href="http://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A0E0E5C02&amp;returnurl=http%3A%2F%2Fazure.microsoft.com%2Fdevelop%2Fmobile%2Ftutorials%2Fget-started%2F" target="_blank">Ingyenes Azure-fiók létrehozása</a>.

## <a name="send-messages-tooevent-hubs"></a>Üzenetek küldése tooEvent hubok
hello Java ügyféloldali kódtára a Event Hubs Maven-projektek a hello használható [Maven központi tárházban](https://search.maven.org/#search%7Cga%7C1%7Ca%3A%22azure-eventhubs%22). Ebben a könyvtárban az Maven project fájl függőségi deklaráció a következő hello segítségével is hivatkozni:    

```xml
<dependency>
    <groupId>com.microsoft.azure</groupId>
    <artifactId>azure-eventhubs</artifactId>
    <version>{VERSION}</version>
</dependency>
```

A különböző típusú buildkörnyezeteket, explicit módon szerezhet be legfrissebb kiadott hello JAR fájlok hello [Maven központi tárházban](https://search.maven.org/#search%7Cga%7C1%7Ca%3A%22azure-eventhubs%22).  

Egyszerű esemény-közzétevő, importálja a hello *com.microsoft.azure.eventhubs* hello Event Hubs ügyfél osztályok és hello csomag *com.microsoft.azure.servicebus* segédprogram osztályok, például a csomag hello Azure Service Bus üzenetküldési ügyfél megosztott közös kivételeket. 

A következő minta hello először létre kell hoznia egy új Maven project konzol/rendszerhéj alkalmazáshoz a kedvenc Java fejlesztői környezetben. Hello osztály neve `Send`.     

```java
import java.io.IOException;
import java.nio.charset.*;
import java.util.*;
import java.util.concurrent.ExecutionException;

import com.microsoft.azure.eventhubs.*;
import com.microsoft.azure.servicebus.*;

public class Send
{
    public static void main(String[] args) 
            throws ServiceBusException, ExecutionException, InterruptedException, IOException
    {
```

Cserélje le a hello névtér és a event hub nevét hello eseményközpont létrehozásakor használt hello értékekkel.

```java
    final String namespaceName = "----ServiceBusNamespaceName-----";
    final String eventHubName = "----EventHubName-----";
    final String sasKeyName = "-----SharedAccessSignatureKeyName-----";
    final String sasKey = "---SharedAccessSignatureKey----";
    ConnectionStringBuilder connStr = new ConnectionStringBuilder(namespaceName, eventHubName, sasKeyName, sasKey);
```

Ezután hozzon létre egyes számú esemény által karakterlánc átalakítása azokat a UTF-8 bájtos kódolását. Ezután hozzon létre új Event Hubs ügyfél példányt hello kapcsolati karakterlánc, és hello üzenetet küldeni.   

```java 

    byte[] payloadBytes = "Test AMQP message from JMS".getBytes("UTF-8");
    EventData sendEvent = new EventData(payloadBytes);

    EventHubClient ehClient = EventHubClient.createFromConnectionStringSync(connStr.toString());
    ehClient.sendSync(sendEvent);
    }
}

``` 

## <a name="next-steps"></a>Következő lépések
További információ az Event Hubs érhetők el a következő hivatkozások hello:

* [Hello EventProcessorHost eseményeket fogadni](event-hubs-java-get-started-receive-eph.md)
* [Event Hubs – áttekintés][Event Hubs overview]
* [Eseményközpont létrehozása](event-hubs-create.md)
* [Event Hubs – gyakori kérdések](event-hubs-faq.md)

<!-- Links -->
[Event Hubs overview]: event-hubs-overview.md