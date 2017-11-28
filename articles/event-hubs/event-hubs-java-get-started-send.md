---
title: "Események küldése az Azure Event Hubs Java használatával |} Microsoft Docs"
description: "Bevezetés az Event Hubs Java használatával küldése"
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
ms.openlocfilehash: b31771001989e20b88bc8d7bca1afceb58ec197c
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/18/2017
---
# <a name="send-events-to-azure-event-hubs-using-java"></a><span data-ttu-id="0848c-103">Események küldése az Azure Event Hubs Java használatával</span><span class="sxs-lookup"><span data-stu-id="0848c-103">Send events to Azure Event Hubs using Java</span></span>

## <a name="introduction"></a><span data-ttu-id="0848c-104">Bevezetés</span><span class="sxs-lookup"><span data-stu-id="0848c-104">Introduction</span></span>
<span data-ttu-id="0848c-105">Az Event Hubs egy kiválóan méretezhető fogadórendszer, amely is több millió eseményt másodpercenként, az alkalmazás engedélyezése feldolgozni, és elemezze a nagy mennyiségű adatot a csatlakoztatott eszközök és alkalmazások által létrehozott.</span><span class="sxs-lookup"><span data-stu-id="0848c-105">Event Hubs is a highly scalable ingestion system that can ingest millions of events per second, enabling an application to process and analyze the massive amounts of data produced by your connected devices and applications.</span></span> <span data-ttu-id="0848c-106">Miután egy eseményközpontba való összegyűjtését, átalakítás és tárolására is használható adatok bármilyen valós idejű elemzési szolgáltató vagy tárolási fürt használatával.</span><span class="sxs-lookup"><span data-stu-id="0848c-106">Once collected into an event hub, you can transform and store data using any real-time analytics provider or storage cluster.</span></span>

<span data-ttu-id="0848c-107">További információkért lásd: a [Event Hubs – áttekintés][Event Hubs overview].</span><span class="sxs-lookup"><span data-stu-id="0848c-107">For more information, see the [Event Hubs overview][Event Hubs overview].</span></span>

<span data-ttu-id="0848c-108">Ez az oktatóanyag bemutatja, hogyan események küldése az eseményközpontba egy Java-Konzolalkalmazás használatával.</span><span class="sxs-lookup"><span data-stu-id="0848c-108">This tutorial shows how to send events to an event hub by using a console application in Java.</span></span> <span data-ttu-id="0848c-109">A Java Event Processor Host szalagtárat használó események fogadásához, lásd: [Ez a cikk](event-hubs-java-get-started-receive-eph.md), vagy kattintson a bal oldali tartalomjegyzék a megfelelő fogadó nyelvet.</span><span class="sxs-lookup"><span data-stu-id="0848c-109">To receive events using the Java Event Processor Host library, see [this article](event-hubs-java-get-started-receive-eph.md), or click the appropriate receiving language in the left-hand table of contents.</span></span>

<span data-ttu-id="0848c-110">Az oktatóanyag teljesítéséhez a következőkre lesz szüksége:</span><span class="sxs-lookup"><span data-stu-id="0848c-110">In order to complete this tutorial, you will need the following:</span></span>

* <span data-ttu-id="0848c-111">A Java-fejlesztőkörnyezet.</span><span class="sxs-lookup"><span data-stu-id="0848c-111">A Java development environment.</span></span> <span data-ttu-id="0848c-112">Ebben az oktatóanyagban feltételezzük, hogy [Eclipse](https://www.eclipse.org/).</span><span class="sxs-lookup"><span data-stu-id="0848c-112">For this tutorial, we assume [Eclipse](https://www.eclipse.org/).</span></span>
* <span data-ttu-id="0848c-113">Aktív Azure-fiók.</span><span class="sxs-lookup"><span data-stu-id="0848c-113">An active Azure account.</span></span> <br/><span data-ttu-id="0848c-114">Ha nincs fiókja, néhány perc alatt létrehozhat egy ingyenes fiókot.</span><span class="sxs-lookup"><span data-stu-id="0848c-114">If you don't have an account, you can create a free account in just a couple of minutes.</span></span> <span data-ttu-id="0848c-115">További információkért lásd: <a href="http://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A0E0E5C02&amp;returnurl=http%3A%2F%2Fazure.microsoft.com%2Fdevelop%2Fmobile%2Ftutorials%2Fget-started%2F" target="_blank">Ingyenes Azure-fiók létrehozása</a>.</span><span class="sxs-lookup"><span data-stu-id="0848c-115">For details, see <a href="http://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A0E0E5C02&amp;returnurl=http%3A%2F%2Fazure.microsoft.com%2Fdevelop%2Fmobile%2Ftutorials%2Fget-started%2F" target="_blank">Azure Free Trial</a>.</span></span>

## <a name="send-messages-to-event-hubs"></a><span data-ttu-id="0848c-116">Üzenetek küldése az Event Hubs szolgáltatásnak</span><span class="sxs-lookup"><span data-stu-id="0848c-116">Send messages to Event Hubs</span></span>
<span data-ttu-id="0848c-117">Az Event Hubs Java ügyfélkódtár a Maven-projektek használható a [Maven központi tárházban](https://search.maven.org/#search%7Cga%7C1%7Ca%3A%22azure-eventhubs%22).</span><span class="sxs-lookup"><span data-stu-id="0848c-117">The Java client library for Event Hubs is available for use in Maven projects from the [Maven Central Repository](https://search.maven.org/#search%7Cga%7C1%7Ca%3A%22azure-eventhubs%22).</span></span> <span data-ttu-id="0848c-118">Ebben a könyvtárban, a következő függőségi nyilatkozat az Maven project fájl használatával is hivatkozni:</span><span class="sxs-lookup"><span data-stu-id="0848c-118">You can reference this library using the following dependency declaration inside your Maven project file:</span></span>    

```xml
<dependency>
    <groupId>com.microsoft.azure</groupId>
    <artifactId>azure-eventhubs</artifactId>
    <version>{VERSION}</version>
</dependency>
```

<span data-ttu-id="0848c-119">A különböző típusú buildkörnyezeteket, explicit módon beszerezheti a legfrissebb kiadott JAR fájlok a [Maven központi tárházban](https://search.maven.org/#search%7Cga%7C1%7Ca%3A%22azure-eventhubs%22).</span><span class="sxs-lookup"><span data-stu-id="0848c-119">For different types of build environments, you can explicitly obtain the latest released JAR files from the [Maven Central Repository](https://search.maven.org/#search%7Cga%7C1%7Ca%3A%22azure-eventhubs%22).</span></span>  

<span data-ttu-id="0848c-120">Egy egyszerű esemény-közzétevő, importálja a *com.microsoft.azure.eventhubs* csomag az Event Hubs ügyfél osztályok és a *com.microsoft.azure.servicebus* segédprogram osztályok esetében például a csomag közös kivételeket, amelyek az Azure Service Bus üzenetküldési ügyfél vannak megosztva.</span><span class="sxs-lookup"><span data-stu-id="0848c-120">For a simple event publisher, import the *com.microsoft.azure.eventhubs* package for the Event Hubs client classes and the *com.microsoft.azure.servicebus* package for utility classes such as common exceptions that are shared with the Azure Service Bus messaging client.</span></span> 

<span data-ttu-id="0848c-121">A következő mintában először hozzon létre egy új Maven-projektet egy konzol/felületalkalmazáshoz a kedvenc Java-fejlesztőkörnyezetében.</span><span class="sxs-lookup"><span data-stu-id="0848c-121">For the following sample, first create a new Maven project for a console/shell application in your favorite Java development environment.</span></span> <span data-ttu-id="0848c-122">Az osztály neve `Send`.</span><span class="sxs-lookup"><span data-stu-id="0848c-122">Name the class `Send`.</span></span>     

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

<span data-ttu-id="0848c-123">Cserélje le a névtér és a event hub neve az eseményközpont létrehozásakor használt értékek.</span><span class="sxs-lookup"><span data-stu-id="0848c-123">Replace the namespace and event hub names with the values used when you created the event hub.</span></span>

```java
    final String namespaceName = "----ServiceBusNamespaceName-----";
    final String eventHubName = "----EventHubName-----";
    final String sasKeyName = "-----SharedAccessSignatureKeyName-----";
    final String sasKey = "---SharedAccessSignatureKey----";
    ConnectionStringBuilder connStr = new ConnectionStringBuilder(namespaceName, eventHubName, sasKeyName, sasKey);
```

<span data-ttu-id="0848c-124">Ezután hozzon létre egyes számú esemény által karakterlánc átalakítása azokat a UTF-8 bájtos kódolását.</span><span class="sxs-lookup"><span data-stu-id="0848c-124">Then, create a singular event by transforming a string into its UTF-8 byte encoding.</span></span> <span data-ttu-id="0848c-125">Ezután hozzon létre új Event Hubs-ügyfél példányt a kapcsolati karakterláncból, és elküldeni az üzenetet.</span><span class="sxs-lookup"><span data-stu-id="0848c-125">Then, create a new Event Hubs client instance from the connection string and send the message.</span></span>   

```java 

    byte[] payloadBytes = "Test AMQP message from JMS".getBytes("UTF-8");
    EventData sendEvent = new EventData(payloadBytes);

    EventHubClient ehClient = EventHubClient.createFromConnectionStringSync(connStr.toString());
    ehClient.sendSync(sendEvent);
    }
}

``` 

## <a name="next-steps"></a><span data-ttu-id="0848c-126">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="0848c-126">Next steps</span></span>
<span data-ttu-id="0848c-127">Az alábbi webhelyeken további információt talál az Event Hubsról:</span><span class="sxs-lookup"><span data-stu-id="0848c-127">You can learn more about Event Hubs by visiting the following links:</span></span>

* [<span data-ttu-id="0848c-128">Fogadása az EventProcessorHost használatához események</span><span class="sxs-lookup"><span data-stu-id="0848c-128">Receive events using the EventProcessorHost</span></span>](event-hubs-java-get-started-receive-eph.md)
* <span data-ttu-id="0848c-129">[Event Hubs – áttekintés][Event Hubs overview]</span><span class="sxs-lookup"><span data-stu-id="0848c-129">[Event Hubs overview][Event Hubs overview]</span></span>
* [<span data-ttu-id="0848c-130">Eseményközpont létrehozása</span><span class="sxs-lookup"><span data-stu-id="0848c-130">Create an event hub</span></span>](event-hubs-create.md)
* [<span data-ttu-id="0848c-131">Event Hubs – gyakori kérdések</span><span class="sxs-lookup"><span data-stu-id="0848c-131">Event Hubs FAQ</span></span>](event-hubs-faq.md)

<!-- Links -->
[Event Hubs overview]: event-hubs-overview.md