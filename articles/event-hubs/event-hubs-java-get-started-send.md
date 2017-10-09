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
# <a name="send-events-tooazure-event-hubs-using-java"></a><span data-ttu-id="b84aa-103">Események küldése tooAzure Event Hubs Java használatával</span><span class="sxs-lookup"><span data-stu-id="b84aa-103">Send events tooAzure Event Hubs using Java</span></span>

## <a name="introduction"></a><span data-ttu-id="b84aa-104">Bevezetés</span><span class="sxs-lookup"><span data-stu-id="b84aa-104">Introduction</span></span>
<span data-ttu-id="b84aa-105">Az Event Hubs egy kiválóan méretezhető fogadórendszer, melyek több millió eseményt másodpercenként, egy alkalmazás tooprocess engedélyezése és elemezheti az adatokat a csatlakoztatott eszközök és alkalmazások által létrehozott nagy mennyiségű hello.</span><span class="sxs-lookup"><span data-stu-id="b84aa-105">Event Hubs is a highly scalable ingestion system that can ingest millions of events per second, enabling an application tooprocess and analyze hello massive amounts of data produced by your connected devices and applications.</span></span> <span data-ttu-id="b84aa-106">Miután egy eseményközpontba való összegyűjtését, átalakítás és tárolására is használható adatok bármilyen valós idejű elemzési szolgáltató vagy tárolási fürt használatával.</span><span class="sxs-lookup"><span data-stu-id="b84aa-106">Once collected into an event hub, you can transform and store data using any real-time analytics provider or storage cluster.</span></span>

<span data-ttu-id="b84aa-107">További információkért lásd: hello [Event Hubs – áttekintés][Event Hubs overview].</span><span class="sxs-lookup"><span data-stu-id="b84aa-107">For more information, see hello [Event Hubs overview][Event Hubs overview].</span></span>

<span data-ttu-id="b84aa-108">Ez az oktatóanyag bemutatja, hogyan toosend események tooan event hubs egy Java-Konzolalkalmazás használatával.</span><span class="sxs-lookup"><span data-stu-id="b84aa-108">This tutorial shows how toosend events tooan event hub by using a console application in Java.</span></span> <span data-ttu-id="b84aa-109">tooreceive események hello Java Event Processor Host szalagtár használata esetén lásd: [Ez a cikk](event-hubs-java-get-started-receive-eph.md), vagy kattintson a hello fogadó megfelelő nyelvi hello bal oldali tábla tartalmának.</span><span class="sxs-lookup"><span data-stu-id="b84aa-109">tooreceive events using hello Java Event Processor Host library, see [this article](event-hubs-java-get-started-receive-eph.md), or click hello appropriate receiving language in hello left-hand table of contents.</span></span>

<span data-ttu-id="b84aa-110">A sorrend toocomplete ebben az oktatóanyagban kell a következő hello:</span><span class="sxs-lookup"><span data-stu-id="b84aa-110">In order toocomplete this tutorial, you will need hello following:</span></span>

* <span data-ttu-id="b84aa-111">A Java-fejlesztőkörnyezet.</span><span class="sxs-lookup"><span data-stu-id="b84aa-111">A Java development environment.</span></span> <span data-ttu-id="b84aa-112">Ebben az oktatóanyagban feltételezzük, hogy [Eclipse](https://www.eclipse.org/).</span><span class="sxs-lookup"><span data-stu-id="b84aa-112">For this tutorial, we assume [Eclipse](https://www.eclipse.org/).</span></span>
* <span data-ttu-id="b84aa-113">Aktív Azure-fiók.</span><span class="sxs-lookup"><span data-stu-id="b84aa-113">An active Azure account.</span></span> <br/><span data-ttu-id="b84aa-114">Ha nincs fiókja, néhány perc alatt létrehozhat egy ingyenes fiókot.</span><span class="sxs-lookup"><span data-stu-id="b84aa-114">If you don't have an account, you can create a free account in just a couple of minutes.</span></span> <span data-ttu-id="b84aa-115">További információkért lásd: <a href="http://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A0E0E5C02&amp;returnurl=http%3A%2F%2Fazure.microsoft.com%2Fdevelop%2Fmobile%2Ftutorials%2Fget-started%2F" target="_blank">Ingyenes Azure-fiók létrehozása</a>.</span><span class="sxs-lookup"><span data-stu-id="b84aa-115">For details, see <a href="http://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A0E0E5C02&amp;returnurl=http%3A%2F%2Fazure.microsoft.com%2Fdevelop%2Fmobile%2Ftutorials%2Fget-started%2F" target="_blank">Azure Free Trial</a>.</span></span>

## <a name="send-messages-tooevent-hubs"></a><span data-ttu-id="b84aa-116">Üzenetek küldése tooEvent hubok</span><span class="sxs-lookup"><span data-stu-id="b84aa-116">Send messages tooEvent Hubs</span></span>
<span data-ttu-id="b84aa-117">hello Java ügyféloldali kódtára a Event Hubs Maven-projektek a hello használható [Maven központi tárházban](https://search.maven.org/#search%7Cga%7C1%7Ca%3A%22azure-eventhubs%22).</span><span class="sxs-lookup"><span data-stu-id="b84aa-117">hello Java client library for Event Hubs is available for use in Maven projects from hello [Maven Central Repository](https://search.maven.org/#search%7Cga%7C1%7Ca%3A%22azure-eventhubs%22).</span></span> <span data-ttu-id="b84aa-118">Ebben a könyvtárban az Maven project fájl függőségi deklaráció a következő hello segítségével is hivatkozni:</span><span class="sxs-lookup"><span data-stu-id="b84aa-118">You can reference this library using hello following dependency declaration inside your Maven project file:</span></span>    

```xml
<dependency>
    <groupId>com.microsoft.azure</groupId>
    <artifactId>azure-eventhubs</artifactId>
    <version>{VERSION}</version>
</dependency>
```

<span data-ttu-id="b84aa-119">A különböző típusú buildkörnyezeteket, explicit módon szerezhet be legfrissebb kiadott hello JAR fájlok hello [Maven központi tárházban](https://search.maven.org/#search%7Cga%7C1%7Ca%3A%22azure-eventhubs%22).</span><span class="sxs-lookup"><span data-stu-id="b84aa-119">For different types of build environments, you can explicitly obtain hello latest released JAR files from hello [Maven Central Repository](https://search.maven.org/#search%7Cga%7C1%7Ca%3A%22azure-eventhubs%22).</span></span>  

<span data-ttu-id="b84aa-120">Egyszerű esemény-közzétevő, importálja a hello *com.microsoft.azure.eventhubs* hello Event Hubs ügyfél osztályok és hello csomag *com.microsoft.azure.servicebus* segédprogram osztályok, például a csomag hello Azure Service Bus üzenetküldési ügyfél megosztott közös kivételeket.</span><span class="sxs-lookup"><span data-stu-id="b84aa-120">For a simple event publisher, import hello *com.microsoft.azure.eventhubs* package for hello Event Hubs client classes and hello *com.microsoft.azure.servicebus* package for utility classes such as common exceptions that are shared with hello Azure Service Bus messaging client.</span></span> 

<span data-ttu-id="b84aa-121">A következő minta hello először létre kell hoznia egy új Maven project konzol/rendszerhéj alkalmazáshoz a kedvenc Java fejlesztői környezetben.</span><span class="sxs-lookup"><span data-stu-id="b84aa-121">For hello following sample, first create a new Maven project for a console/shell application in your favorite Java development environment.</span></span> <span data-ttu-id="b84aa-122">Hello osztály neve `Send`.</span><span class="sxs-lookup"><span data-stu-id="b84aa-122">Name hello class `Send`.</span></span>     

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

<span data-ttu-id="b84aa-123">Cserélje le a hello névtér és a event hub nevét hello eseményközpont létrehozásakor használt hello értékekkel.</span><span class="sxs-lookup"><span data-stu-id="b84aa-123">Replace hello namespace and event hub names with hello values used when you created hello event hub.</span></span>

```java
    final String namespaceName = "----ServiceBusNamespaceName-----";
    final String eventHubName = "----EventHubName-----";
    final String sasKeyName = "-----SharedAccessSignatureKeyName-----";
    final String sasKey = "---SharedAccessSignatureKey----";
    ConnectionStringBuilder connStr = new ConnectionStringBuilder(namespaceName, eventHubName, sasKeyName, sasKey);
```

<span data-ttu-id="b84aa-124">Ezután hozzon létre egyes számú esemény által karakterlánc átalakítása azokat a UTF-8 bájtos kódolását.</span><span class="sxs-lookup"><span data-stu-id="b84aa-124">Then, create a singular event by transforming a string into its UTF-8 byte encoding.</span></span> <span data-ttu-id="b84aa-125">Ezután hozzon létre új Event Hubs ügyfél példányt hello kapcsolati karakterlánc, és hello üzenetet küldeni.</span><span class="sxs-lookup"><span data-stu-id="b84aa-125">Then, create a new Event Hubs client instance from hello connection string and send hello message.</span></span>   

```java 

    byte[] payloadBytes = "Test AMQP message from JMS".getBytes("UTF-8");
    EventData sendEvent = new EventData(payloadBytes);

    EventHubClient ehClient = EventHubClient.createFromConnectionStringSync(connStr.toString());
    ehClient.sendSync(sendEvent);
    }
}

``` 

## <a name="next-steps"></a><span data-ttu-id="b84aa-126">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="b84aa-126">Next steps</span></span>
<span data-ttu-id="b84aa-127">További információ az Event Hubs érhetők el a következő hivatkozások hello:</span><span class="sxs-lookup"><span data-stu-id="b84aa-127">You can learn more about Event Hubs by visiting hello following links:</span></span>

* [<span data-ttu-id="b84aa-128">Hello EventProcessorHost eseményeket fogadni</span><span class="sxs-lookup"><span data-stu-id="b84aa-128">Receive events using hello EventProcessorHost</span></span>](event-hubs-java-get-started-receive-eph.md)
* <span data-ttu-id="b84aa-129">[Event Hubs – áttekintés][Event Hubs overview]</span><span class="sxs-lookup"><span data-stu-id="b84aa-129">[Event Hubs overview][Event Hubs overview]</span></span>
* [<span data-ttu-id="b84aa-130">Eseményközpont létrehozása</span><span class="sxs-lookup"><span data-stu-id="b84aa-130">Create an event hub</span></span>](event-hubs-create.md)
* [<span data-ttu-id="b84aa-131">Event Hubs – gyakori kérdések</span><span class="sxs-lookup"><span data-stu-id="b84aa-131">Event Hubs FAQ</span></span>](event-hubs-faq.md)

<!-- Links -->
[Event Hubs overview]: event-hubs-overview.md