---
title: aaaHow toouse AMQP 1.0 a hello Java Service Bus API |} Microsoft Docs
description: "Hogyan toouse hello Java üzenet szolgáltatás (JMS) az Azure Service Bus és speciális Message Queuing Protodol (AMQP) 1.0-s."
services: service-bus-messaging
documentationcenter: java
author: sethmanheim
manager: timlt
editor: 
ms.assetid: be766f42-6fd1-410c-b275-8c400c811519
ms.service: service-bus-messaging
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: Java
ms.topic: article
ms.date: 08/10/2017
ms.author: sethm
ms.openlocfilehash: 3e1d0329f2675a2273e12bb7389d3ce38b156a5e
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="how-toouse-hello-java-message-service-jms-api-with-service-bus-and-amqp-10"></a>Hogyan toouse hello Java üzenet szolgáltatás (JMS) API a Service Bus és AMQP 1.0
hello speciális Message Queuing protokoll (AMQP) 1.0 nem hatékony, megbízható, vezetékes szintű üzenetkezelési protokoll használható toobuild robusztus, platformfüggetlen üzenetalkalmazások számára.

A Service Bus azt jelenti, hogy használható hello queuing és közzétételi/előfizetési közvetítőalapú üzenettovábbítás szolgáltatások egy hatékony bináris protokollal platformok közötti AMQP 1.0-s támogatása. Ezenkívül összetevőket használja a nyelveket, keretrendszerek és operációs rendszerek áll alkalmazásokat hozhat létre.

Ez a cikk azt ismerteti, hogyan toouse Service Bus üzenetküldési funkcióknak (üzenetsorok és a közzétételi/előfizetési témakörök) használó Java alkalmazások hello népszerű Java üzenet szolgáltatás (JMS) szabványos API. Van egy [kiegészítő cikk](service-bus-amqp-dotnet.md) , amely azt ismerteti, hogyan toodo hello azonos hello Service Bus .NET API használatával. E két útmutatókkal együtt toolearn platformok közötti AMQP 1.0 használatával kapcsolatos is használhatja.

## <a name="get-started-with-service-bus"></a>A Service Bus használatának első lépései
Ez az útmutató feltételezi, hogy már rendelkezik egy Szolgáltatásbusz-névtér nevű várólista tartalmazó **1**. Ha nem, akkor is [hello névtér és a várólista létrehozása](service-bus-create-namespace-portal.md) hello segítségével [Azure-portálon](https://portal.azure.com). További információ a hogyan toocreate Service Bus-névteret és a várólisták, lásd: [Ismerkedés a Service Bus-üzenetsorok](service-bus-dotnet-get-started-with-queues.md).

> [!NOTE]
> A particionált üzenetsorok és témakörök is támogatja az AMQP. További információkért lásd: [particionált üzenetküldési entitások](service-bus-partitioning.md) és [AMQP 1.0 támogatása a Service Bus üzenetsorok és témakörök particionálva](service-bus-partitioned-queues-and-topics-amqp-overview.md).
> 
> 

## <a name="downloading-hello-amqp-10-jms-client-library"></a>Hello AMQP 1.0 JMS ügyféloldali kódtár letöltése
Ha toodownload hello hello Apache Qpid JMS AMQP 1.0 ügyféloldali kódtár legújabb verziójának kapcsolatos információkért látogasson el a [https://qpid.apache.org/download.html](https://qpid.apache.org/download.html).

Négy JAR-fájlok hello Apache Qpid JMS AMQP 1.0 terjesztési archív toohello Java CLASSPATH követően – ha alkalmazások kialakításához és futtatásához JMS a Service busszal hello fel kell vennie:

* geronimo-jms\_1.1\_spec-1.0.jar
* qpid-amqp-1-0-Client-[Version].JAR
* qpid-amqp-1-0-Client-jms-[Version].JAR
* qpid-amqp-1-0-Common-[Version].JAR

## <a name="coding-java-applications"></a>Java-alkalmazások kódolása
### <a name="java-naming-and-directory-interface-jndi"></a>Java-kiosztási és Directory Interface (JNDI)
JMS hello Java elnevezésekor és a Directory Interface (JNDI) toocreate egy elkülönítése is megvalósuljon logikai és fizikai nevét használja. Kétféle JMS objektumok pedig használatával oldja fel JNDI: ConnectionFactory és a cél. JNDI amelybe másik címtárhoz szolgáltatások toohandle név feloldása feladatokat is csatlakoztathatja szolgáltató modellt használ. hello Apache Qpid JMS AMQP 1.0 könyvtár tartalmaz egy egyszerű tulajdonságok fájlalapú JNDI szolgáltató hello következő tulajdonságok fájl használatával konfigurált formázása:

```
# servicebus.properties - sample JNDI configuration

# Register a ConnectionFactory in JNDI using hello form:
# connectionfactory.[jndi_name] = [ConnectionURL]
connectionfactory.SBCF = amqps://[SASPolicyName]:[SASPolicyKey]@[namespace].servicebus.windows.net

# Register some queues in JNDI using hello form
# queue.[jndi_name] = [physical_name]
# topic.[jndi_name] = [physical_name]
queue.QUEUE = queue1
```

#### <a name="configure-hello-connectionfactory"></a>Hello ConnectionFactory konfigurálása
hello használt bejegyzés toodefine egy **ConnectionFactory** hello Qpid a tulajdonságok fájl JNDI szolgáltató verziója hello a következő formátumban:

```
connectionfactory.[jndi_name] = [ConnectionURL]
```

Ha **[jndi_name]** és **[ConnectionURL]** jelentése a következő hello rendelkezik:

* **[jndi_name]** : hello ConnectionFactory hello logikai nevét. Ez az hello név, amely hello Java-alkalmazások hello JNDI IntialContext.lookup() metódussal megszűnik.
* **[ConnectionURL]** : Egy URL-címet, amely hello információkat biztosít a hello JMS könyvtár toohello AMQP broker szükséges.

hello hello formátuma **ConnectionURL** a következőképpen történik:

```
amqps://[SASPolicyName]:[SASPolicyKey]@[namespace].servicebus.windows.net
```
Ha **[névtér]**, **[SASPolicyName]** és **[SASPolicyKey]** jelentése a következő hello rendelkezik:

* **[névtér]** : hello Service Bus-névtér.
* **[SASPolicyName]** : hello várólista közös hozzáférésű Jogosultságkód házirend nevét.
* **[SASPolicyKey]** : hello várólista közös hozzáférésű Jogosultságkód házirend kulcs.

> [!NOTE]
> Manuálisan kell hello jelszó URL-kódolja. Hasznos URL-kódolást segédprogram érhető el: [http://www.w3schools.com/tags/ref_urlencode.asp](http://www.w3schools.com/tags/ref_urlencode.asp).
> 
> 

#### <a name="configure-destinations"></a>Célok konfigurálása
hello bejegyzés toodefine formátuma a következő hello hello JNDI Qpid tulajdonságok fájlszolgáltató lévő célra nem használható:

```
queue.[jndi_name] = [physical_name]
```

vagy

```
topic.[jndi_name] = [physical_name]
```

Ha **[jndi\_neve]** és **[fizikai\_neve]** jelentése a következő hello rendelkezik:

* **[jndi_name]** : hello hello cél logikai nevét. Ez az hello név, amely hello Java-alkalmazások hello JNDI IntialContext.lookup() metódussal megszűnik.
* **[physical_name]** : hello hello Service Bus entitás toowhich hello alkalmazás küld vagy fogad üzeneteket.

> [!NOTE]
> Amikor egy Service Bus üzenettémakör-előfizetésben fogad, JNDI megadott hello fizikai név hello témakör hello nevét kell lennie. hello tartós előfizetés létrehozása a hello JMS alkalmazáskód biztosított hello előfizetés nevét. Hello [Service Bus AMQP 1.0 fejlesztői útmutató](service-bus-amqp-dotnet.md) JMS a Service Bus-üzenettémakörök használata további részleteket tartalmaz.
> 
> 

### <a name="write-hello-jms-application"></a>Hello JMS-alkalmazás
Nincsenek speciális API-k vagy a Service busszal JMS használatakor szükséges beállításokat. Van azonban néhány olyan korlátozásokat, amelyeket később szerepelnek. Bármilyen JMS alkalmazással hello először thing szükséges konfigurációs hello JNDI környezet toobe képes tooresolve egy **ConnectionFactory** és a célhelyre.

#### <a name="configure-hello-jndi-initialcontext"></a>Hello JNDI InitialContext konfigurálása
hello JNDI környezet konfigurálva van, úgy, hogy a konfigurációs adatokat egy kivonattáblát hello javax.naming.InitialContext osztály hello konstruktornak. hello két szükséges a hello hashtable elemei hello osztálynevét hello kezdeti környezet gyári és hello szolgáltató URL-CÍMÉT. hello következő kód bemutatja, hogyan tooconfigure hello JNDI környezet toouse hello Qpid tulajdonságok fájlalapú JNDI szolgáltató nevű tulajdonságok fájlt tartalmazó **servicebus.properties**.

```java
Hashtable<String, String> env = new Hashtable<String, String>(); 
env.put(Context.INITIAL_CONTEXT_FACTORY, "org.apache.qpid.amqp_1_0.jms.jndi.PropertiesFileInitialContextFactory"); 
env.put(Context.PROVIDER_URL, "servicebus.properties"); 
InitialContext context = new InitialContext(env);
``` 

### <a name="a-simple-jms-application-using-a-service-bus-queue"></a>Egy egyszerű JMS-alkalmazást a Service Bus-üzenetsorba
hello alábbi példa program küld hello JNDI logikai név várólista JMS TextMessages tooa Service Bus-üzenetsorba, illetve hello üzeneteket fogad vissza.

```java
// SimpleSenderReceiver.java

import javax.jms.*;
import javax.naming.Context;
import javax.naming.InitialContext;
import java.io.BufferedReader;
import java.io.InputStreamReader;
import java.util.Hashtable;
import java.util.Random;

public class SimpleSenderReceiver implements MessageListener {
    private static boolean runReceiver = true;
    private Connection connection;
    private Session sendSession;
    private Session receiveSession;
    private MessageProducer sender;
    private MessageConsumer receiver;
    private static Random randomGenerator = new Random();

    public SimpleSenderReceiver() throws Exception {
        // Configure JNDI environment
        Hashtable<String, String> env = new Hashtable<String, String>();
        env.put(Context.INITIAL_CONTEXT_FACTORY, 
                   "org.apache.qpid.amqp_1_0.jms.jndi.PropertiesFileInitialContextFactory");
        env.put(Context.PROVIDER_URL, "servicebus.properties");
        Context context = new InitialContext(env);

        // Look up ConnectionFactory and Queue
        ConnectionFactory cf = (ConnectionFactory) context.lookup("SBCF");
        Destination queue = (Destination) context.lookup("QUEUE");

        // Create Connection
        connection = cf.createConnection();

        // Create sender-side Session and MessageProducer
        sendSession = connection.createSession(false, Session.AUTO_ACKNOWLEDGE);
        sender = sendSession.createProducer(queue);

        if (runReceiver) {
            // Create receiver-side Session, MessageConsumer,and MessageListener
            receiveSession = connection.createSession(false, Session.CLIENT_ACKNOWLEDGE);
            receiver = receiveSession.createConsumer(queue);
            receiver.setMessageListener(this);
            connection.start();
        }
    }

    public static void main(String[] args) {
        try {

            if ((args.length > 0) && args[0].equalsIgnoreCase("sendonly")) {
                runReceiver = false;
            }

            SimpleSenderReceiver simpleSenderReceiver = new SimpleSenderReceiver();
            System.out.println("Press [enter] toosend a message. Type 'exit' + [enter] tooquit.");
            BufferedReader commandLine = new java.io.BufferedReader(new InputStreamReader(System.in));

            while (true) {
                String s = commandLine.readLine();
                if (s.equalsIgnoreCase("exit")) {
                    simpleSenderReceiver.close();
                    System.exit(0);
                } else {
                    simpleSenderReceiver.sendMessage();
                }
            }
        } catch (Exception e) {
            e.printStackTrace();
        }
    }

    private void sendMessage() throws JMSException {
        TextMessage message = sendSession.createTextMessage();
        message.setText("Test AMQP message from JMS");
        long randomMessageID = randomGenerator.nextLong() >>>1;
        message.setJMSMessageID("ID:" + randomMessageID);
        sender.send(message);
        System.out.println("Sent message with JMSMessageID = " + message.getJMSMessageID());
    }

    public void close() throws JMSException {
        connection.close();
    }

    public void onMessage(Message message) {
        try {
            System.out.println("Received message with JMSMessageID = " + message.getJMSMessageID());
            message.acknowledge();
        } catch (Exception e) {
            e.printStackTrace();
        }
    }
}    
```

### <a name="run-hello-application"></a>Hello alkalmazás futtatása
Hello űrlap kimenetet hello alkalmazást futtató hoz létre:

```
> java SimpleSenderReceiver
Press [enter] toosend a message. Type 'exit' + [enter] tooquit.

Sent message with JMSMessageID = ID:2867600614942270318
Received message with JMSMessageID = ID:2867600614942270318

Sent message with JMSMessageID = ID:7578408152750301483
Received message with JMSMessageID = ID:7578408152750301483

Sent message with JMSMessageID = ID:956102171969368961
Received message with JMSMessageID = ID:956102171969368961
exit
```

## <a name="cross-platform-messaging-between-jms-and-net"></a>Platformok közötti JMS és .NET között
Ez az útmutató bemutatta, hogyan toosend JMS használatával a Service Bus tooand üzenetek fogadására. Azonban hello főbb előnyei AMQP 1.0-s verziójának egyik, hogy lehetővé teszi, hogy a megbízható és teljes visszaadása által küldött üzenetek különböző nyelveken írt összetevőkből beépített alkalmazások toobe.

Hello JMS mintaalkalmazást a fent leírt és egy hasonló .NET-alkalmazás, egy kiegészítő cikk vett [Service Bus használatával a .NET-az AMQP 1.0](service-bus-amqp-dotnet.md), .NET és Java közötti üzenetek tudjon cserélni. Ez a cikk platformok közötti Service Bus és AMQP 1.0 használatával hello részleteket olvashat olvasni.

### <a name="jms-toonet"></a>JMS too.NET
toodemonstrate JMS too.NET üzenetküldési:

* .NET mintaalkalmazás hello parancssori argumentum nélkül indítsa el.
* Első lépésként hello Java mintaalkalmazás hello "sendonly" parancssori argumentum. Ebben a módban hello alkalmazás addig nem kap üzenetet hello várólista, csak akkor küld.
* Nyomja le az **Enter** néhány alkalommal hello Java-alkalmazás konzolon, amelynek hatására küldött üzenetek toobe.
* Ezek az üzenetek hello .NET-alkalmazás által fogadott.

#### <a name="output-from-jms-application"></a>JMS alkalmazás kimenete
```
> java SimpleSenderReceiver sendonly
Press [enter] toosend a message. Type 'exit' + [enter] tooquit.
Sent message with JMSMessageID = ID:4364096528752411591
Sent message with JMSMessageID = ID:459252991689389983
Sent message with JMSMessageID = ID:1565011046230456854
exit
```

#### <a name="output-from-net-application"></a>.NET-alkalmazás
```
> SimpleSenderReceiver.exe    
Press [enter] toosend a message. Type 'exit' + [enter] tooquit.
Received message with MessageID = 4364096528752411591
Received message with MessageID = 459252991689389983
Received message with MessageID = 1565011046230456854
exit
```

### <a name="net-toojms"></a>.NET tooJMS
toodemonstrate .NET tooJMS üzenetküldési:

* Első lépésként hello .NET mintaalkalmazás hello "sendonly" parancssori argumentum. Ebben a módban hello alkalmazás addig nem kap üzenetet hello várólista, csak akkor küld.
* Hello Java mintaalkalmazás parancssori argumentum nélkül indítsa el.
* Nyomja le az **Enter** néhány alkalommal hello .NET alkalmazás-konzolon, amelynek hatására küldött üzenetek toobe.
* Ezek az üzenetek hello Java-alkalmazás által fogadott.

#### <a name="output-from-net-application"></a>.NET-alkalmazás
```
> SimpleSenderReceiver.exe sendonly
Press [enter] toosend a message. Type 'exit' + [enter] tooquit.
Sent message with MessageID = d64e681a310a48a1ae0ce7b017bf1cf3    
Sent message with MessageID = 98a39664995b4f74b32e2a0ecccc46bb
Sent message with MessageID = acbca67f03c346de9b7893026f97ddeb
exit
```

#### <a name="output-from-jms-application"></a>JMS alkalmazás kimenete
```
> java SimpleSenderReceiver    
Press [enter] toosend a message. Type 'exit' + [enter] tooquit.
Received message with JMSMessageID = ID:d64e681a310a48a1ae0ce7b017bf1cf3
Received message with JMSMessageID = ID:98a39664995b4f74b32e2a0ecccc46bb
Received message with JMSMessageID = ID:acbca67f03c346de9b7893026f97ddeb
exit
```

## <a name="unsupported-features-and-restrictions"></a>Nem támogatott funkciókat és korlátozások
a következő korlátozások hello használatakor JMS keresztül AMQP 1.0-s, Service Bus, azaz léteznek:

* Csak egy **MessageProducer** vagy **MessageConsumer** engedélyezett-e **munkamenet**. Ha toocreate van szüksége több **MessageProducers** vagy **MessageConsumers** alkalmazás, hozzon létre egy dedikált **munkamenet** esetében.
* "Volatile" témakör előfizetések jelenleg nem támogatottak.
* **MessageSelectors** jelenleg nem támogatottak.
* Ideiglenes célok; például **TemporaryQueue**, **TemporaryTopic** jelenleg nem támogatottak, valamint hello **QueueRequestor** és **TopicRequestor**API-t használhatja őket.
* A tranzakción alapuló munkamenetek és az elosztott tranzakciók nem támogatottak.

## <a name="summary"></a>Összefoglalás
Ez hogyan tooguide bemutatta, hogyan toouse Service Bus közvetítőalapú üzenettovábbítás szolgáltatásait (üzenetsorok és a közzétételi/előfizetési témakörök) Java használatával hello népszerű JMS API és az AMQP 1.0.

Használhatja a Service Bus AMQP 1.0 más nyelven, beleértve a .NET, a C, a Python és a PHP. Ezek különböző nyelveken használó összetevőket is üzenetváltásra, megbízható és hello AMQP 1.0-támogatás a Service Bus használatával teljes visszaadása.

## <a name="next-steps"></a>Következő lépések
* [Az Azure Service Bus AMQP 1.0-támogatás](service-bus-amqp-overview.md)
* [Hogyan az AMQP 1.0 toouse hello Service Bus .NET API](service-bus-dotnet-advanced-message-queuing.md)
* [Service Bus AMQP 1.0 fejlesztői útmutató](service-bus-amqp-dotnet.md)
* [Bevezetés a Service Bus által kezelt üzenetsorok használatába](service-bus-dotnet-get-started-with-queues.md)
* [Java fejlesztői központban](https://azure.microsoft.com/develop/java/)

