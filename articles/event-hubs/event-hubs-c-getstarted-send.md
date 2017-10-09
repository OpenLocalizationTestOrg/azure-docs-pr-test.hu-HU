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
# <a name="send-events-tooazure-event-hubs-using-c"></a>Események küldése tooAzure Event Hubs C használatával

## <a name="introduction"></a>Bevezetés
Az Event Hubs egy kiválóan méretezhető fogadórendszer, melyek több millió eseményt másodpercenként, egy alkalmazás tooprocess engedélyezése és elemezheti az adatokat a csatlakoztatott eszközök és alkalmazások által létrehozott nagy mennyiségű hello. Miután egy eseményközpontba való összegyűjtését, átalakítás és tárolására is használható adatok bármilyen valós idejű elemzési szolgáltató vagy tárolási fürt használatával.

További információkért lásd: hello [Event Hubs – áttekintés] [Event Hubs – áttekintés].

Ebből az oktatóanyagból megtudhatja, hogyan toosend események tooan event hubs egy konzolalkalmazás használatával C. tooreceive eseményeknél, kattintson a hello fogadó megfelelő nyelvi hello bal oldali tábla tartalmát.

toocomplete ebben az oktatóanyagban szüksége lesz a következő hello:

* A C környezet. Ebben az oktatóanyagban az Azure Linux virtuális gép az Ubuntu 14.04 hello ÖET verembe indulunk ki.
* [A Microsoft Visual Studio](https://www.visualstudio.com/).
* Aktív Azure-fiók. Ha nincs fiókja, néhány perc alatt létrehozhat egy ingyenes próbafiókot. További információkért lásd: [Ingyenes Azure-fiók létrehozása](https://azure.microsoft.com/pricing/free-trial/).

## <a name="send-messages-tooevent-hubs"></a>Üzenetek küldése tooEvent hubok
Ebben a szakaszban egy C app toosend események tooyour eseményközpont azt írni. hello használ a hello Proton AMQP kódtárat hello [Apache Qpid projekt](http://qpid.apache.org/). Ez egy hasonló toousing Service Bus-üzenetsorok és témakörök az AMQP c látható [Itt](https://code.msdn.microsoft.com/Using-Apache-Qpid-Proton-C-afd76504). További információkért lásd: [Qpid Proton dokumentáció](http://qpid.apache.org/proton/index.html).

1. A hello [Qpid AMQP Messenger lap](https://qpid.apache.org/proton/messenger.html), hajtsa végre a hello utasításokat tooinstall Qpid Proton, attól függően, hogy a környezetében.
2. toocompile hello Proton könyvtár, telepítse a következő csomagok hello:
   
    ```shell
    sudo apt-get install build-essential cmake uuid-dev openssl libssl-dev
    ```
3. Töltse le a hello [Qpid Proton könyvtár](http://qpid.apache.org/proton/index.html), és bontsa ki, pl.:
   
    ```shell
    wget http://archive.apache.org/dist/qpid/proton/0.7/qpid-proton-0.7.tar.gz
    tar xvfz qpid-proton-0.7.tar.gz
    ```
4. Hozzon létre egy build directory, a fordítás és a telepítés:
   
    ```shell
    cd qpid-proton-0.7
    mkdir build
    cd build
    cmake -DCMAKE_INSTALL_PREFIX=/usr ..
    sudo make install
    ```
5. Munka címtárat, hozzon létre egy új fájlt nevű **sender.c** a következő kód hello. Ne felejtse el toosubstitute hello érték az eseményközpont neveként és névtér nevét. Is helyettesítse be egy URL-kódolású verziója hello hello kulcsa **SendRule** korábban létrehozott. Beállíthatja az URL-kódolja azt [Itt](http://www.w3schools.com/tags/ref_urlencode.asp).
   
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
6. Hello fájlt, feltéve, hogy **ÖET**:
   
    ```
    gcc sender.c -o sender -lqpid-proton
    ```

    > [!NOTE]
    > Ezzel a kóddal használjuk egy kimenő ablak 1 tooforce köszönőüzenetei ki a lehető leghamarabb. Az alkalmazás általában toobatch üzenetek tooincrease átviteli kell próbálnia. Lásd: hello [Qpid AMQP Messenger lap](https://qpid.apache.org/proton/messenger.html) hogyan toouse hello Qpid Proton könyvtár ebben és más környezetekben, illetve a platformok, amelynek kötések találhatók információ (jelenleg Perl, PHP, Python vagy Ruby).


## <a name="next-steps"></a>Következő lépések
További információ az Event Hubs érhetők el a következő hivatkozások hello:

* [Event Hubs – áttekintés](event-hubs-what-is-event-hubs.md
)
* [Eseményközpont létrehozása](event-hubs-create.md)
* [Event Hubs – gyakori kérdések](event-hubs-faq.md)

<!-- Images. -->
[21]: ./media/event-hubs-c-ephcs-getstarted/run-csharp-ephcs1.png
[24]: ./media/event-hubs-c-ephcs-getstarted/receive-eph-c.png
