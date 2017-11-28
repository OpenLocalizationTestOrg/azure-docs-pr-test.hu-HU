---
title: "Események küldése az Azure Event Hubs C használatával |} Microsoft Docs"
description: "Események küldése az Azure Event Hubs C használatával"
services: event-hubs
documentationcenter: 
author: sethmanheim
manager: timlt
editor: 
ms.assetid: 
ms.service: event-hubs
ms.workload: na
ms.tgt_pltfrm: c
ms.devlang: csharp
ms.topic: article
ms.date: 08/15/2017
ms.author: sethm
ms.openlocfilehash: a615ee39b6c3731cc7df366e9fabeed5219a71b4
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/18/2017
---
# <a name="send-events-to-azure-event-hubs-using-c"></a><span data-ttu-id="3d392-103">Események küldése az Azure Event Hubs C használatával</span><span class="sxs-lookup"><span data-stu-id="3d392-103">Send events to Azure Event Hubs using C</span></span>

## <a name="introduction"></a><span data-ttu-id="3d392-104">Bevezetés</span><span class="sxs-lookup"><span data-stu-id="3d392-104">Introduction</span></span>
<span data-ttu-id="3d392-105">Az Event Hubs egy kiválóan méretezhető fogadórendszer, amely is több millió eseményt másodpercenként, az alkalmazás engedélyezése feldolgozni, és elemezze a nagy mennyiségű adatot a csatlakoztatott eszközök és alkalmazások által létrehozott.</span><span class="sxs-lookup"><span data-stu-id="3d392-105">Event Hubs is a highly scalable ingestion system that can ingest millions of events per second, enabling an application to process and analyze the massive amounts of data produced by your connected devices and applications.</span></span> <span data-ttu-id="3d392-106">Miután egy eseményközpontba való összegyűjtését, átalakítás és tárolására is használható adatok bármilyen valós idejű elemzési szolgáltató vagy tárolási fürt használatával.</span><span class="sxs-lookup"><span data-stu-id="3d392-106">Once collected into an event hub, you can transform and store data using any real-time analytics provider or storage cluster.</span></span>

<span data-ttu-id="3d392-107">További információkért lásd: a [Event Hubs – áttekintés] [Event Hubs – áttekintés].</span><span class="sxs-lookup"><span data-stu-id="3d392-107">For more information, please see the [Event Hubs overview][Event Hubs overview].</span></span>

<span data-ttu-id="3d392-108">Ebből az oktatóanyagból megtudhatja, hogyan is küldi az eseményeket az eseményközpontba egy konzolalkalmazás használatával a c kiszolgálóra. Események fogadásához, kattintson a bal oldali tartalomjegyzék a megfelelő fogadó nyelvet.</span><span class="sxs-lookup"><span data-stu-id="3d392-108">In this tutorial, you will learn how to send events to an event hub using a console application in C. To receive events, click the appropriate receiving language in the left-hand table of contents.</span></span>

<span data-ttu-id="3d392-109">Az oktatóanyag elvégzéséhez a következőkre lesz szüksége:</span><span class="sxs-lookup"><span data-stu-id="3d392-109">To complete this tutorial, you will need the following:</span></span>

* <span data-ttu-id="3d392-110">A C környezet.</span><span class="sxs-lookup"><span data-stu-id="3d392-110">A C development environment.</span></span> <span data-ttu-id="3d392-111">Ebben az oktatóanyagban azt fogja feltételezni, hogy az Azure Linux virtuális gép az Ubuntu 14.04 ÖET verembe.</span><span class="sxs-lookup"><span data-stu-id="3d392-111">For this tutorial, we will assume the gcc stack on an Azure Linux VM with Ubuntu 14.04.</span></span>
* <span data-ttu-id="3d392-112">[A Microsoft Visual Studio](https://www.visualstudio.com/).</span><span class="sxs-lookup"><span data-stu-id="3d392-112">[Microsoft Visual Studio](https://www.visualstudio.com/).</span></span>
* <span data-ttu-id="3d392-113">Aktív Azure-fiók.</span><span class="sxs-lookup"><span data-stu-id="3d392-113">An active Azure account.</span></span> <span data-ttu-id="3d392-114">Ha nincs fiókja, néhány perc alatt létrehozhat egy ingyenes próbafiókot.</span><span class="sxs-lookup"><span data-stu-id="3d392-114">If you don't have an account, you can create a free trial account in just a couple of minutes.</span></span> <span data-ttu-id="3d392-115">További információkért lásd: [Ingyenes Azure-fiók létrehozása](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="3d392-115">For details, see [Azure Free Trial](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="send-messages-to-event-hubs"></a><span data-ttu-id="3d392-116">Üzenetek küldése az Event Hubs szolgáltatásnak</span><span class="sxs-lookup"><span data-stu-id="3d392-116">Send messages to Event Hubs</span></span>
<span data-ttu-id="3d392-117">Ebben a szakaszban azt is küldi az eseményeket az eseményközpontjába C alkalmazások írásához.</span><span class="sxs-lookup"><span data-stu-id="3d392-117">In this section we write a C app to send events to your event hub.</span></span> <span data-ttu-id="3d392-118">A kódot használja a Proton AMQP kódtárat a [Apache Qpid projekt](http://qpid.apache.org/).</span><span class="sxs-lookup"><span data-stu-id="3d392-118">The code uses the Proton AMQP library from the [Apache Qpid project](http://qpid.apache.org/).</span></span> <span data-ttu-id="3d392-119">Ez hasonló használata a Service Bus-üzenetsorok és témakörök AMQP c látható az [Itt](https://code.msdn.microsoft.com/Using-Apache-Qpid-Proton-C-afd76504).</span><span class="sxs-lookup"><span data-stu-id="3d392-119">This is analogous to using Service Bus queues and topics with AMQP from C as shown [here](https://code.msdn.microsoft.com/Using-Apache-Qpid-Proton-C-afd76504).</span></span> <span data-ttu-id="3d392-120">További információkért lásd: [Qpid Proton dokumentáció](http://qpid.apache.org/proton/index.html).</span><span class="sxs-lookup"><span data-stu-id="3d392-120">For more information, see [Qpid Proton documentation](http://qpid.apache.org/proton/index.html).</span></span>

1. <span data-ttu-id="3d392-121">Az a [Qpid AMQP Messenger lap](https://qpid.apache.org/proton/messenger.html), kövesse az utasításokat a környezettől függően Qpid Proton telepítéséhez.</span><span class="sxs-lookup"><span data-stu-id="3d392-121">From the [Qpid AMQP Messenger page](https://qpid.apache.org/proton/messenger.html), follow the instructions to install Qpid Proton, depending on your environment.</span></span>
2. <span data-ttu-id="3d392-122">Fordítása a Proton könyvtárban, a következő csomagok telepítése:</span><span class="sxs-lookup"><span data-stu-id="3d392-122">To compile the Proton library, install the following packages:</span></span>
   
    ```shell
    sudo apt-get install build-essential cmake uuid-dev openssl libssl-dev
    ```
3. <span data-ttu-id="3d392-123">Töltse le a [Qpid Proton könyvtár](http://qpid.apache.org/proton/index.html), és bontsa ki, pl.:</span><span class="sxs-lookup"><span data-stu-id="3d392-123">Download the [Qpid Proton library](http://qpid.apache.org/proton/index.html), and extract it, e.g.:</span></span>
   
    ```shell
    wget http://archive.apache.org/dist/qpid/proton/0.7/qpid-proton-0.7.tar.gz
    tar xvfz qpid-proton-0.7.tar.gz
    ```
4. <span data-ttu-id="3d392-124">Hozzon létre egy build directory, a fordítás és a telepítés:</span><span class="sxs-lookup"><span data-stu-id="3d392-124">Create a build directory, compile and install:</span></span>
   
    ```shell
    cd qpid-proton-0.7
    mkdir build
    cd build
    cmake -DCMAKE_INSTALL_PREFIX=/usr ..
    sudo make install
    ```
5. <span data-ttu-id="3d392-125">Munka címtárat, hozzon létre egy új fájlt nevű **sender.c** a következő kóddal.</span><span class="sxs-lookup"><span data-stu-id="3d392-125">In your work directory, create a new file called **sender.c** with the following code.</span></span> <span data-ttu-id="3d392-126">Ne felejtse el behelyettesíteni az eseményközpont neveként és névtér nevét érték.</span><span class="sxs-lookup"><span data-stu-id="3d392-126">Remember to substitute the value for your event hub name and namespace name.</span></span> <span data-ttu-id="3d392-127">Is helyettesítse be a kulcsot egy URL-kódolású verziója a **SendRule** korábban létrehozott.</span><span class="sxs-lookup"><span data-stu-id="3d392-127">You must also substitute a URL-encoded version of the key for the **SendRule** created earlier.</span></span> <span data-ttu-id="3d392-128">Beállíthatja az URL-kódolja azt [Itt](http://www.w3schools.com/tags/ref_urlencode.asp).</span><span class="sxs-lookup"><span data-stu-id="3d392-128">You can URL-encode it [here](http://www.w3schools.com/tags/ref_urlencode.asp).</span></span>
   
    ```c
    #include "proton/message.h"
    #include "proton/messenger.h"
   
    #include <getopt.h>
    #include <proton/util.h>
    #include <sys/time.h>
    #include <stddef.h>
    #include <stdio.h>
    #include <string.h>
    #include <unistd.h>
    #include <stdlib.h>
   
    #define check(messenger)                                                     
      {                                                                          
        if(pn_messenger_errno(messenger))                                        
        {                                                                        
          printf("check\n");                                                     
          die(__FILE__, __LINE__, pn_error_text(pn_messenger_error(messenger))); 
        }                                                                        
      }  
   
    pn_timestamp_t time_now(void)
    {
      struct timeval now;
      if (gettimeofday(&now, NULL)) pn_fatal("gettimeofday failed\n");
      return ((pn_timestamp_t)now.tv_sec) * 1000 + (now.tv_usec / 1000);
    }  
   
    void die(const char *file, int line, const char *message)
    {
      printf("Dead\n");
      fprintf(stderr, "%s:%i: %s\n", file, line, message);
      exit(1);
    }
   
    int sendMessage(pn_messenger_t * messenger) {
        char * address = (char *) "amqps://SendRule:{Send Rule key}@{namespace name}.servicebus.windows.net/{event hub name}";
        char * msgtext = (char *) "Hello from C!";
   
        pn_message_t * message;
        pn_data_t * body;
        message = pn_message();
   
        pn_message_set_address(message, address);
        pn_message_set_content_type(message, (char*) "application/octect-stream");
        pn_message_set_inferred(message, true);
   
        body = pn_message_body(message);
        pn_data_put_binary(body, pn_bytes(strlen(msgtext), msgtext));
   
        pn_messenger_put(messenger, message);
        check(messenger);
        pn_messenger_send(messenger, 1);
        check(messenger);
   
        pn_message_free(message);
    }
   
    int main(int argc, char** argv) {
        printf("Press Ctrl-C to stop the sender process\n");
   
        pn_messenger_t *messenger = pn_messenger(NULL);
        pn_messenger_set_outgoing_window(messenger, 1);
        pn_messenger_start(messenger);
   
        while(true) {
            sendMessage(messenger);
            printf("Sent message\n");
            sleep(1);
        }
   
        // release messenger resources
        pn_messenger_stop(messenger);
        pn_messenger_free(messenger);
   
        return 0;
    }
    ```
6. <span data-ttu-id="3d392-129">A fájlt, feltéve, hogy **ÖET**:</span><span class="sxs-lookup"><span data-stu-id="3d392-129">Compile the file, assuming **gcc**:</span></span>
   
    ```
    gcc sender.c -o sender -lqpid-proton
    ```

    > [!NOTE]
    > <span data-ttu-id="3d392-130">Ez a kód egy kimenő 1-ablakban használjuk kimenő üzenetek minél hamarabb kényszerítése.</span><span class="sxs-lookup"><span data-stu-id="3d392-130">In this code, we use an outgoing window of 1 to force the messages out as soon as possible.</span></span> <span data-ttu-id="3d392-131">Általában az alkalmazás próbálkozzon az átviteli sebesség növelése kötegelt üzeneteket.</span><span class="sxs-lookup"><span data-stu-id="3d392-131">In general, your application should try to batch messages to increase throughput.</span></span> <span data-ttu-id="3d392-132">Tekintse meg a [Qpid AMQP Messenger lap](https://qpid.apache.org/proton/messenger.html) olvashat ebben és más környezetekben, illetve a platformok, amelynek kötések találhatók Qpid Proton tár használatára (jelenleg Perl, PHP, Python vagy Ruby).</span><span class="sxs-lookup"><span data-stu-id="3d392-132">See the [Qpid AMQP Messenger page](https://qpid.apache.org/proton/messenger.html) for information about how to use the Qpid Proton library in this and other environments, and from platforms for which bindings are provided (currently Perl, PHP, Python, and Ruby).</span></span>


## <a name="next-steps"></a><span data-ttu-id="3d392-133">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="3d392-133">Next steps</span></span>
<span data-ttu-id="3d392-134">Az alábbi webhelyeken további információt talál az Event Hubsról:</span><span class="sxs-lookup"><span data-stu-id="3d392-134">You can learn more about Event Hubs by visiting the following links:</span></span>

* [<span data-ttu-id="3d392-135">Event Hubs – áttekintés</span><span class="sxs-lookup"><span data-stu-id="3d392-135">Event Hubs overview</span></span>](event-hubs-what-is-event-hubs.md
)
* [<span data-ttu-id="3d392-136">Eseményközpont létrehozása</span><span class="sxs-lookup"><span data-stu-id="3d392-136">Create an event hub</span></span>](event-hubs-create.md)
* [<span data-ttu-id="3d392-137">Event Hubs – gyakori kérdések</span><span class="sxs-lookup"><span data-stu-id="3d392-137">Event Hubs FAQ</span></span>](event-hubs-faq.md)

<!-- Images. -->
[21]: ./media/event-hubs-c-ephcs-getstarted/run-csharp-ephcs1.png
[24]: ./media/event-hubs-c-ephcs-getstarted/receive-eph-c.png
