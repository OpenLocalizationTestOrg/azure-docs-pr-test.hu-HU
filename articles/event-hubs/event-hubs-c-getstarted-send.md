---
title: "aaaSend események tooAzure Event Hubs C használatával |} Microsoft Docs"
description: "Események küldése tooAzure Event Hubs C használatával"
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
ms.openlocfilehash: bb53300c070debb4a3658a38df9d3966f08e81ae
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="send-events-tooazure-event-hubs-using-c"></a><span data-ttu-id="02eb0-103">Események küldése tooAzure Event Hubs C használatával</span><span class="sxs-lookup"><span data-stu-id="02eb0-103">Send events tooAzure Event Hubs using C</span></span>

## <a name="introduction"></a><span data-ttu-id="02eb0-104">Bevezetés</span><span class="sxs-lookup"><span data-stu-id="02eb0-104">Introduction</span></span>
<span data-ttu-id="02eb0-105">Az Event Hubs egy kiválóan méretezhető fogadórendszer, melyek több millió eseményt másodpercenként, egy alkalmazás tooprocess engedélyezése és elemezheti az adatokat a csatlakoztatott eszközök és alkalmazások által létrehozott nagy mennyiségű hello.</span><span class="sxs-lookup"><span data-stu-id="02eb0-105">Event Hubs is a highly scalable ingestion system that can ingest millions of events per second, enabling an application tooprocess and analyze hello massive amounts of data produced by your connected devices and applications.</span></span> <span data-ttu-id="02eb0-106">Miután egy eseményközpontba való összegyűjtését, átalakítás és tárolására is használható adatok bármilyen valós idejű elemzési szolgáltató vagy tárolási fürt használatával.</span><span class="sxs-lookup"><span data-stu-id="02eb0-106">Once collected into an event hub, you can transform and store data using any real-time analytics provider or storage cluster.</span></span>

<span data-ttu-id="02eb0-107">További információkért lásd: hello [Event Hubs – áttekintés] [Event Hubs – áttekintés].</span><span class="sxs-lookup"><span data-stu-id="02eb0-107">For more information, please see hello [Event Hubs overview][Event Hubs overview].</span></span>

<span data-ttu-id="02eb0-108">Ebből az oktatóanyagból megtudhatja, hogyan toosend események tooan event hubs egy konzolalkalmazás használatával C. tooreceive eseményeknél, kattintson a hello fogadó megfelelő nyelvi hello bal oldali tábla tartalmát.</span><span class="sxs-lookup"><span data-stu-id="02eb0-108">In this tutorial, you will learn how toosend events tooan event hub using a console application in C. tooreceive events, click hello appropriate receiving language in hello left-hand table of contents.</span></span>

<span data-ttu-id="02eb0-109">toocomplete ebben az oktatóanyagban szüksége lesz a következő hello:</span><span class="sxs-lookup"><span data-stu-id="02eb0-109">toocomplete this tutorial, you will need hello following:</span></span>

* <span data-ttu-id="02eb0-110">A C környezet.</span><span class="sxs-lookup"><span data-stu-id="02eb0-110">A C development environment.</span></span> <span data-ttu-id="02eb0-111">Ebben az oktatóanyagban az Azure Linux virtuális gép az Ubuntu 14.04 hello ÖET verembe indulunk ki.</span><span class="sxs-lookup"><span data-stu-id="02eb0-111">For this tutorial, we will assume hello gcc stack on an Azure Linux VM with Ubuntu 14.04.</span></span>
* <span data-ttu-id="02eb0-112">[A Microsoft Visual Studio](https://www.visualstudio.com/).</span><span class="sxs-lookup"><span data-stu-id="02eb0-112">[Microsoft Visual Studio](https://www.visualstudio.com/).</span></span>
* <span data-ttu-id="02eb0-113">Aktív Azure-fiók.</span><span class="sxs-lookup"><span data-stu-id="02eb0-113">An active Azure account.</span></span> <span data-ttu-id="02eb0-114">Ha nincs fiókja, néhány perc alatt létrehozhat egy ingyenes próbafiókot.</span><span class="sxs-lookup"><span data-stu-id="02eb0-114">If you don't have an account, you can create a free trial account in just a couple of minutes.</span></span> <span data-ttu-id="02eb0-115">További információkért lásd: [Ingyenes Azure-fiók létrehozása](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="02eb0-115">For details, see [Azure Free Trial](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="send-messages-tooevent-hubs"></a><span data-ttu-id="02eb0-116">Üzenetek küldése tooEvent hubok</span><span class="sxs-lookup"><span data-stu-id="02eb0-116">Send messages tooEvent Hubs</span></span>
<span data-ttu-id="02eb0-117">Ebben a szakaszban egy C app toosend események tooyour eseményközpont azt írni.</span><span class="sxs-lookup"><span data-stu-id="02eb0-117">In this section we write a C app toosend events tooyour event hub.</span></span> <span data-ttu-id="02eb0-118">hello használ a hello Proton AMQP kódtárat hello [Apache Qpid projekt](http://qpid.apache.org/).</span><span class="sxs-lookup"><span data-stu-id="02eb0-118">hello code uses hello Proton AMQP library from hello [Apache Qpid project](http://qpid.apache.org/).</span></span> <span data-ttu-id="02eb0-119">Ez egy hasonló toousing Service Bus-üzenetsorok és témakörök az AMQP c látható [Itt](https://code.msdn.microsoft.com/Using-Apache-Qpid-Proton-C-afd76504).</span><span class="sxs-lookup"><span data-stu-id="02eb0-119">This is analogous toousing Service Bus queues and topics with AMQP from C as shown [here](https://code.msdn.microsoft.com/Using-Apache-Qpid-Proton-C-afd76504).</span></span> <span data-ttu-id="02eb0-120">További információkért lásd: [Qpid Proton dokumentáció](http://qpid.apache.org/proton/index.html).</span><span class="sxs-lookup"><span data-stu-id="02eb0-120">For more information, see [Qpid Proton documentation](http://qpid.apache.org/proton/index.html).</span></span>

1. <span data-ttu-id="02eb0-121">A hello [Qpid AMQP Messenger lap](https://qpid.apache.org/proton/messenger.html), hajtsa végre a hello utasításokat tooinstall Qpid Proton, attól függően, hogy a környezetében.</span><span class="sxs-lookup"><span data-stu-id="02eb0-121">From hello [Qpid AMQP Messenger page](https://qpid.apache.org/proton/messenger.html), follow hello instructions tooinstall Qpid Proton, depending on your environment.</span></span>
2. <span data-ttu-id="02eb0-122">toocompile hello Proton könyvtár, telepítse a következő csomagok hello:</span><span class="sxs-lookup"><span data-stu-id="02eb0-122">toocompile hello Proton library, install hello following packages:</span></span>
   
    ```shell
    sudo apt-get install build-essential cmake uuid-dev openssl libssl-dev
    ```
3. <span data-ttu-id="02eb0-123">Töltse le a hello [Qpid Proton könyvtár](http://qpid.apache.org/proton/index.html), és bontsa ki, pl.:</span><span class="sxs-lookup"><span data-stu-id="02eb0-123">Download hello [Qpid Proton library](http://qpid.apache.org/proton/index.html), and extract it, e.g.:</span></span>
   
    ```shell
    wget http://archive.apache.org/dist/qpid/proton/0.7/qpid-proton-0.7.tar.gz
    tar xvfz qpid-proton-0.7.tar.gz
    ```
4. <span data-ttu-id="02eb0-124">Hozzon létre egy build directory, a fordítás és a telepítés:</span><span class="sxs-lookup"><span data-stu-id="02eb0-124">Create a build directory, compile and install:</span></span>
   
    ```shell
    cd qpid-proton-0.7
    mkdir build
    cd build
    cmake -DCMAKE_INSTALL_PREFIX=/usr ..
    sudo make install
    ```
5. <span data-ttu-id="02eb0-125">Munka címtárat, hozzon létre egy új fájlt nevű **sender.c** a következő kód hello.</span><span class="sxs-lookup"><span data-stu-id="02eb0-125">In your work directory, create a new file called **sender.c** with hello following code.</span></span> <span data-ttu-id="02eb0-126">Ne felejtse el toosubstitute hello érték az eseményközpont neveként és névtér nevét.</span><span class="sxs-lookup"><span data-stu-id="02eb0-126">Remember toosubstitute hello value for your event hub name and namespace name.</span></span> <span data-ttu-id="02eb0-127">Is helyettesítse be egy URL-kódolású verziója hello hello kulcsa **SendRule** korábban létrehozott.</span><span class="sxs-lookup"><span data-stu-id="02eb0-127">You must also substitute a URL-encoded version of hello key for hello **SendRule** created earlier.</span></span> <span data-ttu-id="02eb0-128">Beállíthatja az URL-kódolja azt [Itt](http://www.w3schools.com/tags/ref_urlencode.asp).</span><span class="sxs-lookup"><span data-stu-id="02eb0-128">You can URL-encode it [here](http://www.w3schools.com/tags/ref_urlencode.asp).</span></span>
   
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
        printf("Press Ctrl-C toostop hello sender process\n");
   
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
6. <span data-ttu-id="02eb0-129">Hello fájlt, feltéve, hogy **ÖET**:</span><span class="sxs-lookup"><span data-stu-id="02eb0-129">Compile hello file, assuming **gcc**:</span></span>
   
    ```
    gcc sender.c -o sender -lqpid-proton
    ```

    > [!NOTE]
    > <span data-ttu-id="02eb0-130">Ezzel a kóddal használjuk egy kimenő ablak 1 tooforce köszönőüzenetei ki a lehető leghamarabb.</span><span class="sxs-lookup"><span data-stu-id="02eb0-130">In this code, we use an outgoing window of 1 tooforce hello messages out as soon as possible.</span></span> <span data-ttu-id="02eb0-131">Az alkalmazás általában toobatch üzenetek tooincrease átviteli kell próbálnia.</span><span class="sxs-lookup"><span data-stu-id="02eb0-131">In general, your application should try toobatch messages tooincrease throughput.</span></span> <span data-ttu-id="02eb0-132">Lásd: hello [Qpid AMQP Messenger lap](https://qpid.apache.org/proton/messenger.html) hogyan toouse hello Qpid Proton könyvtár ebben és más környezetekben, illetve a platformok, amelynek kötések találhatók információ (jelenleg Perl, PHP, Python vagy Ruby).</span><span class="sxs-lookup"><span data-stu-id="02eb0-132">See hello [Qpid AMQP Messenger page](https://qpid.apache.org/proton/messenger.html) for information about how toouse hello Qpid Proton library in this and other environments, and from platforms for which bindings are provided (currently Perl, PHP, Python, and Ruby).</span></span>


## <a name="next-steps"></a><span data-ttu-id="02eb0-133">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="02eb0-133">Next steps</span></span>
<span data-ttu-id="02eb0-134">További információ az Event Hubs érhetők el a következő hivatkozások hello:</span><span class="sxs-lookup"><span data-stu-id="02eb0-134">You can learn more about Event Hubs by visiting hello following links:</span></span>

* [<span data-ttu-id="02eb0-135">Event Hubs – áttekintés</span><span class="sxs-lookup"><span data-stu-id="02eb0-135">Event Hubs overview</span></span>](event-hubs-what-is-event-hubs.md
)
* [<span data-ttu-id="02eb0-136">Eseményközpont létrehozása</span><span class="sxs-lookup"><span data-stu-id="02eb0-136">Create an event hub</span></span>](event-hubs-create.md)
* [<span data-ttu-id="02eb0-137">Event Hubs – gyakori kérdések</span><span class="sxs-lookup"><span data-stu-id="02eb0-137">Event Hubs FAQ</span></span>](event-hubs-faq.md)

<!-- Images. -->
[21]: ./media/event-hubs-c-ephcs-getstarted/run-csharp-ephcs1.png
[24]: ./media/event-hubs-c-ephcs-getstarted/receive-eph-c.png
