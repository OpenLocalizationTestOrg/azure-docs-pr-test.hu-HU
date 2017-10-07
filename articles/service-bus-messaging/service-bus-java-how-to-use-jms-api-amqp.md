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
# <a name="how-toouse-hello-java-message-service-jms-api-with-service-bus-and-amqp-10"></a><span data-ttu-id="7f1e4-103">Hogyan toouse hello Java üzenet szolgáltatás (JMS) API a Service Bus és AMQP 1.0</span><span class="sxs-lookup"><span data-stu-id="7f1e4-103">How toouse hello Java Message Service (JMS) API with Service Bus and AMQP 1.0</span></span>
<span data-ttu-id="7f1e4-104">hello speciális Message Queuing protokoll (AMQP) 1.0 nem hatékony, megbízható, vezetékes szintű üzenetkezelési protokoll használható toobuild robusztus, platformfüggetlen üzenetalkalmazások számára.</span><span class="sxs-lookup"><span data-stu-id="7f1e4-104">hello Advanced Message Queuing Protocol (AMQP) 1.0 is an efficient, reliable, wire-level messaging protocol that you can use toobuild robust, cross-platform messaging applications.</span></span>

<span data-ttu-id="7f1e4-105">A Service Bus azt jelenti, hogy használható hello queuing és közzétételi/előfizetési közvetítőalapú üzenettovábbítás szolgáltatások egy hatékony bináris protokollal platformok közötti AMQP 1.0-s támogatása.</span><span class="sxs-lookup"><span data-stu-id="7f1e4-105">Support for AMQP 1.0 in Service Bus means that you can use hello queuing and publish/subscribe brokered messaging features from a range of platforms using an efficient binary protocol.</span></span> <span data-ttu-id="7f1e4-106">Ezenkívül összetevőket használja a nyelveket, keretrendszerek és operációs rendszerek áll alkalmazásokat hozhat létre.</span><span class="sxs-lookup"><span data-stu-id="7f1e4-106">Furthermore, you can build applications comprised of components built using a mix of languages, frameworks, and operating systems.</span></span>

<span data-ttu-id="7f1e4-107">Ez a cikk azt ismerteti, hogyan toouse Service Bus üzenetküldési funkcióknak (üzenetsorok és a közzétételi/előfizetési témakörök) használó Java alkalmazások hello népszerű Java üzenet szolgáltatás (JMS) szabványos API.</span><span class="sxs-lookup"><span data-stu-id="7f1e4-107">This article explains how toouse Service Bus messaging features (queues and publish/subscribe topics) from Java applications using hello popular Java Message Service (JMS) API standard.</span></span> <span data-ttu-id="7f1e4-108">Van egy [kiegészítő cikk](service-bus-amqp-dotnet.md) , amely azt ismerteti, hogyan toodo hello azonos hello Service Bus .NET API használatával.</span><span class="sxs-lookup"><span data-stu-id="7f1e4-108">There is a [companion article](service-bus-amqp-dotnet.md) that explains how toodo hello same using hello Service Bus .NET API.</span></span> <span data-ttu-id="7f1e4-109">E két útmutatókkal együtt toolearn platformok közötti AMQP 1.0 használatával kapcsolatos is használhatja.</span><span class="sxs-lookup"><span data-stu-id="7f1e4-109">You can use these two guides together toolearn about cross-platform messaging using AMQP 1.0.</span></span>

## <a name="get-started-with-service-bus"></a><span data-ttu-id="7f1e4-110">A Service Bus használatának első lépései</span><span class="sxs-lookup"><span data-stu-id="7f1e4-110">Get started with Service Bus</span></span>
<span data-ttu-id="7f1e4-111">Ez az útmutató feltételezi, hogy már rendelkezik egy Szolgáltatásbusz-névtér nevű várólista tartalmazó **1**.</span><span class="sxs-lookup"><span data-stu-id="7f1e4-111">This guide assumes that you already have a Service Bus namespace containing a queue named **queue1**.</span></span> <span data-ttu-id="7f1e4-112">Ha nem, akkor is [hello névtér és a várólista létrehozása](service-bus-create-namespace-portal.md) hello segítségével [Azure-portálon](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="7f1e4-112">If you do not, then you can [create hello namespace and queue](service-bus-create-namespace-portal.md) using hello [Azure portal](https://portal.azure.com).</span></span> <span data-ttu-id="7f1e4-113">További információ a hogyan toocreate Service Bus-névteret és a várólisták, lásd: [Ismerkedés a Service Bus-üzenetsorok](service-bus-dotnet-get-started-with-queues.md).</span><span class="sxs-lookup"><span data-stu-id="7f1e4-113">For more information about how toocreate Service Bus namespaces and queues, see [Get started with Service Bus queues](service-bus-dotnet-get-started-with-queues.md).</span></span>

> [!NOTE]
> <span data-ttu-id="7f1e4-114">A particionált üzenetsorok és témakörök is támogatja az AMQP.</span><span class="sxs-lookup"><span data-stu-id="7f1e4-114">Partitioned queues and topics also support AMQP.</span></span> <span data-ttu-id="7f1e4-115">További információkért lásd: [particionált üzenetküldési entitások](service-bus-partitioning.md) és [AMQP 1.0 támogatása a Service Bus üzenetsorok és témakörök particionálva](service-bus-partitioned-queues-and-topics-amqp-overview.md).</span><span class="sxs-lookup"><span data-stu-id="7f1e4-115">For more information, see [Partitioned messaging entities](service-bus-partitioning.md) and [AMQP 1.0 support for Service Bus partitioned queues and topics](service-bus-partitioned-queues-and-topics-amqp-overview.md).</span></span>
> 
> 

## <a name="downloading-hello-amqp-10-jms-client-library"></a><span data-ttu-id="7f1e4-116">Hello AMQP 1.0 JMS ügyféloldali kódtár letöltése</span><span class="sxs-lookup"><span data-stu-id="7f1e4-116">Downloading hello AMQP 1.0 JMS client library</span></span>
<span data-ttu-id="7f1e4-117">Ha toodownload hello hello Apache Qpid JMS AMQP 1.0 ügyféloldali kódtár legújabb verziójának kapcsolatos információkért látogasson el a [https://qpid.apache.org/download.html](https://qpid.apache.org/download.html).</span><span class="sxs-lookup"><span data-stu-id="7f1e4-117">For information about where toodownload hello latest version of hello Apache Qpid JMS AMQP 1.0 client library, visit [https://qpid.apache.org/download.html](https://qpid.apache.org/download.html).</span></span>

<span data-ttu-id="7f1e4-118">Négy JAR-fájlok hello Apache Qpid JMS AMQP 1.0 terjesztési archív toohello Java CLASSPATH követően – ha alkalmazások kialakításához és futtatásához JMS a Service busszal hello fel kell vennie:</span><span class="sxs-lookup"><span data-stu-id="7f1e4-118">You must add hello following four JAR files from hello Apache Qpid JMS AMQP 1.0 distribution archive toohello Java CLASSPATH when building and running JMS applications with Service Bus:</span></span>

* <span data-ttu-id="7f1e4-119">geronimo-jms\_1.1\_spec-1.0.jar</span><span class="sxs-lookup"><span data-stu-id="7f1e4-119">geronimo-jms\_1.1\_spec-1.0.jar</span></span>
* <span data-ttu-id="7f1e4-120">qpid-amqp-1-0-Client-[Version].JAR</span><span class="sxs-lookup"><span data-stu-id="7f1e4-120">qpid-amqp-1-0-client-[version].jar</span></span>
* <span data-ttu-id="7f1e4-121">qpid-amqp-1-0-Client-jms-[Version].JAR</span><span class="sxs-lookup"><span data-stu-id="7f1e4-121">qpid-amqp-1-0-client-jms-[version].jar</span></span>
* <span data-ttu-id="7f1e4-122">qpid-amqp-1-0-Common-[Version].JAR</span><span class="sxs-lookup"><span data-stu-id="7f1e4-122">qpid-amqp-1-0-common-[version].jar</span></span>

## <a name="coding-java-applications"></a><span data-ttu-id="7f1e4-123">Java-alkalmazások kódolása</span><span class="sxs-lookup"><span data-stu-id="7f1e4-123">Coding Java applications</span></span>
### <a name="java-naming-and-directory-interface-jndi"></a><span data-ttu-id="7f1e4-124">Java-kiosztási és Directory Interface (JNDI)</span><span class="sxs-lookup"><span data-stu-id="7f1e4-124">Java Naming and Directory Interface (JNDI)</span></span>
<span data-ttu-id="7f1e4-125">JMS hello Java elnevezésekor és a Directory Interface (JNDI) toocreate egy elkülönítése is megvalósuljon logikai és fizikai nevét használja.</span><span class="sxs-lookup"><span data-stu-id="7f1e4-125">JMS uses hello Java Naming and Directory Interface (JNDI) toocreate a separation between logical names and physical names.</span></span> <span data-ttu-id="7f1e4-126">Kétféle JMS objektumok pedig használatával oldja fel JNDI: ConnectionFactory és a cél.</span><span class="sxs-lookup"><span data-stu-id="7f1e4-126">Two types of JMS objects are resolved using JNDI: ConnectionFactory and Destination.</span></span> <span data-ttu-id="7f1e4-127">JNDI amelybe másik címtárhoz szolgáltatások toohandle név feloldása feladatokat is csatlakoztathatja szolgáltató modellt használ.</span><span class="sxs-lookup"><span data-stu-id="7f1e4-127">JNDI uses a provider model into which you can plug different directory services toohandle name resolution duties.</span></span> <span data-ttu-id="7f1e4-128">hello Apache Qpid JMS AMQP 1.0 könyvtár tartalmaz egy egyszerű tulajdonságok fájlalapú JNDI szolgáltató hello következő tulajdonságok fájl használatával konfigurált formázása:</span><span class="sxs-lookup"><span data-stu-id="7f1e4-128">hello Apache Qpid JMS AMQP 1.0 library comes with a simple properties file-based JNDI Provider that is configured using a properties file of hello following format:</span></span>

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

#### <a name="configure-hello-connectionfactory"></a><span data-ttu-id="7f1e4-129">Hello ConnectionFactory konfigurálása</span><span class="sxs-lookup"><span data-stu-id="7f1e4-129">Configure hello ConnectionFactory</span></span>
<span data-ttu-id="7f1e4-130">hello használt bejegyzés toodefine egy **ConnectionFactory** hello Qpid a tulajdonságok fájl JNDI szolgáltató verziója hello a következő formátumban:</span><span class="sxs-lookup"><span data-stu-id="7f1e4-130">hello entry used toodefine a **ConnectionFactory** in hello Qpid properties file JNDI provider is of hello following format:</span></span>

```
connectionfactory.[jndi_name] = [ConnectionURL]
```

<span data-ttu-id="7f1e4-131">Ha **[jndi_name]** és **[ConnectionURL]** jelentése a következő hello rendelkezik:</span><span class="sxs-lookup"><span data-stu-id="7f1e4-131">Where **[jndi_name]** and **[ConnectionURL]** have hello following meanings:</span></span>

* <span data-ttu-id="7f1e4-132">**[jndi_name]** : hello ConnectionFactory hello logikai nevét.</span><span class="sxs-lookup"><span data-stu-id="7f1e4-132">**[jndi_name]**: hello logical name of hello ConnectionFactory.</span></span> <span data-ttu-id="7f1e4-133">Ez az hello név, amely hello Java-alkalmazások hello JNDI IntialContext.lookup() metódussal megszűnik.</span><span class="sxs-lookup"><span data-stu-id="7f1e4-133">This is hello name that will be resolved in hello Java application using hello JNDI IntialContext.lookup() method.</span></span>
* <span data-ttu-id="7f1e4-134">**[ConnectionURL]** : Egy URL-címet, amely hello információkat biztosít a hello JMS könyvtár toohello AMQP broker szükséges.</span><span class="sxs-lookup"><span data-stu-id="7f1e4-134">**[ConnectionURL]**: A URL that provides hello JMS library with hello information required toohello AMQP broker.</span></span>

<span data-ttu-id="7f1e4-135">hello hello formátuma **ConnectionURL** a következőképpen történik:</span><span class="sxs-lookup"><span data-stu-id="7f1e4-135">hello format of hello **ConnectionURL** is as follows:</span></span>

```
amqps://[SASPolicyName]:[SASPolicyKey]@[namespace].servicebus.windows.net
```
<span data-ttu-id="7f1e4-136">Ha **[névtér]**, **[SASPolicyName]** és **[SASPolicyKey]** jelentése a következő hello rendelkezik:</span><span class="sxs-lookup"><span data-stu-id="7f1e4-136">Where **[namespace]**, **[SASPolicyName]** and **[SASPolicyKey]** have hello following meanings:</span></span>

* <span data-ttu-id="7f1e4-137">**[névtér]** : hello Service Bus-névtér.</span><span class="sxs-lookup"><span data-stu-id="7f1e4-137">**[namespace]**: hello Service Bus namespace.</span></span>
* <span data-ttu-id="7f1e4-138">**[SASPolicyName]** : hello várólista közös hozzáférésű Jogosultságkód házirend nevét.</span><span class="sxs-lookup"><span data-stu-id="7f1e4-138">**[SASPolicyName]**: hello Queue Shared Access Signature policy name.</span></span>
* <span data-ttu-id="7f1e4-139">**[SASPolicyKey]** : hello várólista közös hozzáférésű Jogosultságkód házirend kulcs.</span><span class="sxs-lookup"><span data-stu-id="7f1e4-139">**[SASPolicyKey]**: hello Queue Shared Access Signature policy key.</span></span>

> [!NOTE]
> <span data-ttu-id="7f1e4-140">Manuálisan kell hello jelszó URL-kódolja.</span><span class="sxs-lookup"><span data-stu-id="7f1e4-140">You must URL-encode hello password manually.</span></span> <span data-ttu-id="7f1e4-141">Hasznos URL-kódolást segédprogram érhető el: [http://www.w3schools.com/tags/ref_urlencode.asp](http://www.w3schools.com/tags/ref_urlencode.asp).</span><span class="sxs-lookup"><span data-stu-id="7f1e4-141">A useful URL-encoding utility is available at [http://www.w3schools.com/tags/ref_urlencode.asp](http://www.w3schools.com/tags/ref_urlencode.asp).</span></span>
> 
> 

#### <a name="configure-destinations"></a><span data-ttu-id="7f1e4-142">Célok konfigurálása</span><span class="sxs-lookup"><span data-stu-id="7f1e4-142">Configure destinations</span></span>
<span data-ttu-id="7f1e4-143">hello bejegyzés toodefine formátuma a következő hello hello JNDI Qpid tulajdonságok fájlszolgáltató lévő célra nem használható:</span><span class="sxs-lookup"><span data-stu-id="7f1e4-143">hello entry used toodefine a destination in hello Qpid properties file JNDI provider is of hello following format:</span></span>

```
queue.[jndi_name] = [physical_name]
```

<span data-ttu-id="7f1e4-144">vagy</span><span class="sxs-lookup"><span data-stu-id="7f1e4-144">or</span></span>

```
topic.[jndi_name] = [physical_name]
```

<span data-ttu-id="7f1e4-145">Ha **[jndi\_neve]** és **[fizikai\_neve]** jelentése a következő hello rendelkezik:</span><span class="sxs-lookup"><span data-stu-id="7f1e4-145">Where **[jndi\_name]** and **[physical\_name]** have hello following meanings:</span></span>

* <span data-ttu-id="7f1e4-146">**[jndi_name]** : hello hello cél logikai nevét.</span><span class="sxs-lookup"><span data-stu-id="7f1e4-146">**[jndi_name]**: hello logical name of hello destination.</span></span> <span data-ttu-id="7f1e4-147">Ez az hello név, amely hello Java-alkalmazások hello JNDI IntialContext.lookup() metódussal megszűnik.</span><span class="sxs-lookup"><span data-stu-id="7f1e4-147">This is hello name that will be resolved in hello Java application using hello JNDI IntialContext.lookup() method.</span></span>
* <span data-ttu-id="7f1e4-148">**[physical_name]** : hello hello Service Bus entitás toowhich hello alkalmazás küld vagy fogad üzeneteket.</span><span class="sxs-lookup"><span data-stu-id="7f1e4-148">**[physical_name]**: hello name of hello Service Bus entity toowhich hello application sends or receives messages.</span></span>

> [!NOTE]
> <span data-ttu-id="7f1e4-149">Amikor egy Service Bus üzenettémakör-előfizetésben fogad, JNDI megadott hello fizikai név hello témakör hello nevét kell lennie.</span><span class="sxs-lookup"><span data-stu-id="7f1e4-149">When receiving from a Service Bus topic subscription, hello physical name specified in JNDI should be hello name of hello topic.</span></span> <span data-ttu-id="7f1e4-150">hello tartós előfizetés létrehozása a hello JMS alkalmazáskód biztosított hello előfizetés nevét.</span><span class="sxs-lookup"><span data-stu-id="7f1e4-150">hello subscription name is provided when hello durable subscription is created in hello JMS application code.</span></span> <span data-ttu-id="7f1e4-151">Hello [Service Bus AMQP 1.0 fejlesztői útmutató](service-bus-amqp-dotnet.md) JMS a Service Bus-üzenettémakörök használata további részleteket tartalmaz.</span><span class="sxs-lookup"><span data-stu-id="7f1e4-151">hello [Service Bus AMQP 1.0 Developer's Guide](service-bus-amqp-dotnet.md) provides more details on working with Service Bus topics from JMS.</span></span>
> 
> 

### <a name="write-hello-jms-application"></a><span data-ttu-id="7f1e4-152">Hello JMS-alkalmazás</span><span class="sxs-lookup"><span data-stu-id="7f1e4-152">Write hello JMS application</span></span>
<span data-ttu-id="7f1e4-153">Nincsenek speciális API-k vagy a Service busszal JMS használatakor szükséges beállításokat.</span><span class="sxs-lookup"><span data-stu-id="7f1e4-153">There are no special APIs or options required when using JMS with Service Bus.</span></span> <span data-ttu-id="7f1e4-154">Van azonban néhány olyan korlátozásokat, amelyeket később szerepelnek.</span><span class="sxs-lookup"><span data-stu-id="7f1e4-154">However, there are a few restrictions that will be covered later.</span></span> <span data-ttu-id="7f1e4-155">Bármilyen JMS alkalmazással hello először thing szükséges konfigurációs hello JNDI környezet toobe képes tooresolve egy **ConnectionFactory** és a célhelyre.</span><span class="sxs-lookup"><span data-stu-id="7f1e4-155">As with any JMS application, hello first thing required is configuration of hello JNDI environment, toobe able tooresolve a **ConnectionFactory** and destinations.</span></span>

#### <a name="configure-hello-jndi-initialcontext"></a><span data-ttu-id="7f1e4-156">Hello JNDI InitialContext konfigurálása</span><span class="sxs-lookup"><span data-stu-id="7f1e4-156">Configure hello JNDI InitialContext</span></span>
<span data-ttu-id="7f1e4-157">hello JNDI környezet konfigurálva van, úgy, hogy a konfigurációs adatokat egy kivonattáblát hello javax.naming.InitialContext osztály hello konstruktornak.</span><span class="sxs-lookup"><span data-stu-id="7f1e4-157">hello JNDI environment is configured by passing a hashtable of configuration information into hello constructor of hello javax.naming.InitialContext class.</span></span> <span data-ttu-id="7f1e4-158">hello két szükséges a hello hashtable elemei hello osztálynevét hello kezdeti környezet gyári és hello szolgáltató URL-CÍMÉT.</span><span class="sxs-lookup"><span data-stu-id="7f1e4-158">hello two required elements in hello hashtable are hello class name of hello Initial Context Factory and hello Provider URL.</span></span> <span data-ttu-id="7f1e4-159">hello következő kód bemutatja, hogyan tooconfigure hello JNDI környezet toouse hello Qpid tulajdonságok fájlalapú JNDI szolgáltató nevű tulajdonságok fájlt tartalmazó **servicebus.properties**.</span><span class="sxs-lookup"><span data-stu-id="7f1e4-159">hello following code shows how tooconfigure hello JNDI environment toouse hello Qpid properties file based JNDI Provider with a properties file named **servicebus.properties**.</span></span>

```java
Hashtable<String, String> env = new Hashtable<String, String>(); 
env.put(Context.INITIAL_CONTEXT_FACTORY, "org.apache.qpid.amqp_1_0.jms.jndi.PropertiesFileInitialContextFactory"); 
env.put(Context.PROVIDER_URL, "servicebus.properties"); 
InitialContext context = new InitialContext(env);
``` 

### <a name="a-simple-jms-application-using-a-service-bus-queue"></a><span data-ttu-id="7f1e4-160">Egy egyszerű JMS-alkalmazást a Service Bus-üzenetsorba</span><span class="sxs-lookup"><span data-stu-id="7f1e4-160">A simple JMS application using a Service Bus queue</span></span>
<span data-ttu-id="7f1e4-161">hello alábbi példa program küld hello JNDI logikai név várólista JMS TextMessages tooa Service Bus-üzenetsorba, illetve hello üzeneteket fogad vissza.</span><span class="sxs-lookup"><span data-stu-id="7f1e4-161">hello following example program sends JMS TextMessages tooa Service Bus queue with hello JNDI logical name of QUEUE, and receives hello messages back.</span></span>

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

### <a name="run-hello-application"></a><span data-ttu-id="7f1e4-162">Hello alkalmazás futtatása</span><span class="sxs-lookup"><span data-stu-id="7f1e4-162">Run hello application</span></span>
<span data-ttu-id="7f1e4-163">Hello űrlap kimenetet hello alkalmazást futtató hoz létre:</span><span class="sxs-lookup"><span data-stu-id="7f1e4-163">Running hello application produces output of hello form:</span></span>

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

## <a name="cross-platform-messaging-between-jms-and-net"></a><span data-ttu-id="7f1e4-164">Platformok közötti JMS és .NET között</span><span class="sxs-lookup"><span data-stu-id="7f1e4-164">Cross-platform messaging between JMS and .NET</span></span>
<span data-ttu-id="7f1e4-165">Ez az útmutató bemutatta, hogyan toosend JMS használatával a Service Bus tooand üzenetek fogadására.</span><span class="sxs-lookup"><span data-stu-id="7f1e4-165">This guide showed how toosend and receive messages tooand from Service Bus using JMS.</span></span> <span data-ttu-id="7f1e4-166">Azonban hello főbb előnyei AMQP 1.0-s verziójának egyik, hogy lehetővé teszi, hogy a megbízható és teljes visszaadása által küldött üzenetek különböző nyelveken írt összetevőkből beépített alkalmazások toobe.</span><span class="sxs-lookup"><span data-stu-id="7f1e4-166">However, one of hello key benefits of AMQP 1.0 is that it enables applications toobe built from components written in different languages, with messages exchanged reliably and at full fidelity.</span></span>

<span data-ttu-id="7f1e4-167">Hello JMS mintaalkalmazást a fent leírt és egy hasonló .NET-alkalmazás, egy kiegészítő cikk vett [Service Bus használatával a .NET-az AMQP 1.0](service-bus-amqp-dotnet.md), .NET és Java közötti üzenetek tudjon cserélni.</span><span class="sxs-lookup"><span data-stu-id="7f1e4-167">Using hello sample JMS application described above and a similar .NET application taken from a companion article, [Using Service Bus from .NET with AMQP 1.0](service-bus-amqp-dotnet.md), you can exchange messages between .NET and Java.</span></span> <span data-ttu-id="7f1e4-168">Ez a cikk platformok közötti Service Bus és AMQP 1.0 használatával hello részleteket olvashat olvasni.</span><span class="sxs-lookup"><span data-stu-id="7f1e4-168">Read this article for more information about hello details of cross-platform messaging using Service Bus and AMQP 1.0.</span></span>

### <a name="jms-toonet"></a><span data-ttu-id="7f1e4-169">JMS too.NET</span><span class="sxs-lookup"><span data-stu-id="7f1e4-169">JMS too.NET</span></span>
<span data-ttu-id="7f1e4-170">toodemonstrate JMS too.NET üzenetküldési:</span><span class="sxs-lookup"><span data-stu-id="7f1e4-170">toodemonstrate JMS too.NET messaging:</span></span>

* <span data-ttu-id="7f1e4-171">.NET mintaalkalmazás hello parancssori argumentum nélkül indítsa el.</span><span class="sxs-lookup"><span data-stu-id="7f1e4-171">Start hello .NET sample application without any command-line arguments.</span></span>
* <span data-ttu-id="7f1e4-172">Első lépésként hello Java mintaalkalmazás hello "sendonly" parancssori argumentum.</span><span class="sxs-lookup"><span data-stu-id="7f1e4-172">Start hello Java sample application with hello "sendonly" command-line argument.</span></span> <span data-ttu-id="7f1e4-173">Ebben a módban hello alkalmazás addig nem kap üzenetet hello várólista, csak akkor küld.</span><span class="sxs-lookup"><span data-stu-id="7f1e4-173">In this mode, hello application will not receive messages from hello queue, it will only send.</span></span>
* <span data-ttu-id="7f1e4-174">Nyomja le az **Enter** néhány alkalommal hello Java-alkalmazás konzolon, amelynek hatására küldött üzenetek toobe.</span><span class="sxs-lookup"><span data-stu-id="7f1e4-174">Press **Enter** a few times in hello Java application console, which will cause messages toobe sent.</span></span>
* <span data-ttu-id="7f1e4-175">Ezek az üzenetek hello .NET-alkalmazás által fogadott.</span><span class="sxs-lookup"><span data-stu-id="7f1e4-175">These messages are received by hello .NET application.</span></span>

#### <a name="output-from-jms-application"></a><span data-ttu-id="7f1e4-176">JMS alkalmazás kimenete</span><span class="sxs-lookup"><span data-stu-id="7f1e4-176">Output from JMS application</span></span>
```
> java SimpleSenderReceiver sendonly
Press [enter] toosend a message. Type 'exit' + [enter] tooquit.
Sent message with JMSMessageID = ID:4364096528752411591
Sent message with JMSMessageID = ID:459252991689389983
Sent message with JMSMessageID = ID:1565011046230456854
exit
```

#### <a name="output-from-net-application"></a><span data-ttu-id="7f1e4-177">.NET-alkalmazás</span><span class="sxs-lookup"><span data-stu-id="7f1e4-177">Output from .NET application</span></span>
```
> SimpleSenderReceiver.exe    
Press [enter] toosend a message. Type 'exit' + [enter] tooquit.
Received message with MessageID = 4364096528752411591
Received message with MessageID = 459252991689389983
Received message with MessageID = 1565011046230456854
exit
```

### <a name="net-toojms"></a><span data-ttu-id="7f1e4-178">.NET tooJMS</span><span class="sxs-lookup"><span data-stu-id="7f1e4-178">.NET tooJMS</span></span>
<span data-ttu-id="7f1e4-179">toodemonstrate .NET tooJMS üzenetküldési:</span><span class="sxs-lookup"><span data-stu-id="7f1e4-179">toodemonstrate .NET tooJMS messaging:</span></span>

* <span data-ttu-id="7f1e4-180">Első lépésként hello .NET mintaalkalmazás hello "sendonly" parancssori argumentum.</span><span class="sxs-lookup"><span data-stu-id="7f1e4-180">Start hello .NET sample application with hello "sendonly" command-line argument.</span></span> <span data-ttu-id="7f1e4-181">Ebben a módban hello alkalmazás addig nem kap üzenetet hello várólista, csak akkor küld.</span><span class="sxs-lookup"><span data-stu-id="7f1e4-181">In this mode, hello application will not receive messages from hello queue, it will only send.</span></span>
* <span data-ttu-id="7f1e4-182">Hello Java mintaalkalmazás parancssori argumentum nélkül indítsa el.</span><span class="sxs-lookup"><span data-stu-id="7f1e4-182">Start hello Java sample application without any command-line arguments.</span></span>
* <span data-ttu-id="7f1e4-183">Nyomja le az **Enter** néhány alkalommal hello .NET alkalmazás-konzolon, amelynek hatására küldött üzenetek toobe.</span><span class="sxs-lookup"><span data-stu-id="7f1e4-183">Press **Enter** a few times in hello .NET application console, which will cause messages toobe sent.</span></span>
* <span data-ttu-id="7f1e4-184">Ezek az üzenetek hello Java-alkalmazás által fogadott.</span><span class="sxs-lookup"><span data-stu-id="7f1e4-184">These messages are received by hello Java application.</span></span>

#### <a name="output-from-net-application"></a><span data-ttu-id="7f1e4-185">.NET-alkalmazás</span><span class="sxs-lookup"><span data-stu-id="7f1e4-185">Output from .NET application</span></span>
```
> SimpleSenderReceiver.exe sendonly
Press [enter] toosend a message. Type 'exit' + [enter] tooquit.
Sent message with MessageID = d64e681a310a48a1ae0ce7b017bf1cf3    
Sent message with MessageID = 98a39664995b4f74b32e2a0ecccc46bb
Sent message with MessageID = acbca67f03c346de9b7893026f97ddeb
exit
```

#### <a name="output-from-jms-application"></a><span data-ttu-id="7f1e4-186">JMS alkalmazás kimenete</span><span class="sxs-lookup"><span data-stu-id="7f1e4-186">Output from JMS application</span></span>
```
> java SimpleSenderReceiver    
Press [enter] toosend a message. Type 'exit' + [enter] tooquit.
Received message with JMSMessageID = ID:d64e681a310a48a1ae0ce7b017bf1cf3
Received message with JMSMessageID = ID:98a39664995b4f74b32e2a0ecccc46bb
Received message with JMSMessageID = ID:acbca67f03c346de9b7893026f97ddeb
exit
```

## <a name="unsupported-features-and-restrictions"></a><span data-ttu-id="7f1e4-187">Nem támogatott funkciókat és korlátozások</span><span class="sxs-lookup"><span data-stu-id="7f1e4-187">Unsupported features and restrictions</span></span>
<span data-ttu-id="7f1e4-188">a következő korlátozások hello használatakor JMS keresztül AMQP 1.0-s, Service Bus, azaz léteznek:</span><span class="sxs-lookup"><span data-stu-id="7f1e4-188">hello following restrictions exist when using JMS over AMQP 1.0 with Service Bus, namely:</span></span>

* <span data-ttu-id="7f1e4-189">Csak egy **MessageProducer** vagy **MessageConsumer** engedélyezett-e **munkamenet**.</span><span class="sxs-lookup"><span data-stu-id="7f1e4-189">Only one **MessageProducer** or **MessageConsumer** is allowed per **Session**.</span></span> <span data-ttu-id="7f1e4-190">Ha toocreate van szüksége több **MessageProducers** vagy **MessageConsumers** alkalmazás, hozzon létre egy dedikált **munkamenet** esetében.</span><span class="sxs-lookup"><span data-stu-id="7f1e4-190">If you need toocreate multiple **MessageProducers** or **MessageConsumers** in an application, create a dedicated **Session** for each of them.</span></span>
* <span data-ttu-id="7f1e4-191">"Volatile" témakör előfizetések jelenleg nem támogatottak.</span><span class="sxs-lookup"><span data-stu-id="7f1e4-191">Volatile topic subscriptions are not currently supported.</span></span>
* <span data-ttu-id="7f1e4-192">**MessageSelectors** jelenleg nem támogatottak.</span><span class="sxs-lookup"><span data-stu-id="7f1e4-192">**MessageSelectors** are not currently supported.</span></span>
* <span data-ttu-id="7f1e4-193">Ideiglenes célok; például **TemporaryQueue**, **TemporaryTopic** jelenleg nem támogatottak, valamint hello **QueueRequestor** és **TopicRequestor**API-t használhatja őket.</span><span class="sxs-lookup"><span data-stu-id="7f1e4-193">Temporary destinations; for example, **TemporaryQueue**, **TemporaryTopic** are not currently supported, along with hello **QueueRequestor** and **TopicRequestor** APIs that use them.</span></span>
* <span data-ttu-id="7f1e4-194">A tranzakción alapuló munkamenetek és az elosztott tranzakciók nem támogatottak.</span><span class="sxs-lookup"><span data-stu-id="7f1e4-194">Transacted sessions and distributed transactions are not supported.</span></span>

## <a name="summary"></a><span data-ttu-id="7f1e4-195">Összefoglalás</span><span class="sxs-lookup"><span data-stu-id="7f1e4-195">Summary</span></span>
<span data-ttu-id="7f1e4-196">Ez hogyan tooguide bemutatta, hogyan toouse Service Bus közvetítőalapú üzenettovábbítás szolgáltatásait (üzenetsorok és a közzétételi/előfizetési témakörök) Java használatával hello népszerű JMS API és az AMQP 1.0.</span><span class="sxs-lookup"><span data-stu-id="7f1e4-196">This how-tooguide showed how toouse Service Bus brokered messaging features (queues and publish/subscribe topics) from Java using hello popular JMS API and AMQP 1.0.</span></span>

<span data-ttu-id="7f1e4-197">Használhatja a Service Bus AMQP 1.0 más nyelven, beleértve a .NET, a C, a Python és a PHP.</span><span class="sxs-lookup"><span data-stu-id="7f1e4-197">You can also use Service Bus AMQP 1.0 from other languages, including .NET, C, Python, and PHP.</span></span> <span data-ttu-id="7f1e4-198">Ezek különböző nyelveken használó összetevőket is üzenetváltásra, megbízható és hello AMQP 1.0-támogatás a Service Bus használatával teljes visszaadása.</span><span class="sxs-lookup"><span data-stu-id="7f1e4-198">Components built using these different languages can exchange messages reliably and at full fidelity using hello AMQP 1.0 support in Service Bus.</span></span>

## <a name="next-steps"></a><span data-ttu-id="7f1e4-199">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="7f1e4-199">Next steps</span></span>
* [<span data-ttu-id="7f1e4-200">Az Azure Service Bus AMQP 1.0-támogatás</span><span class="sxs-lookup"><span data-stu-id="7f1e4-200">AMQP 1.0 support in Azure Service Bus</span></span>](service-bus-amqp-overview.md)
* [<span data-ttu-id="7f1e4-201">Hogyan az AMQP 1.0 toouse hello Service Bus .NET API</span><span class="sxs-lookup"><span data-stu-id="7f1e4-201">How toouse AMQP 1.0 with hello Service Bus .NET API</span></span>](service-bus-dotnet-advanced-message-queuing.md)
* [<span data-ttu-id="7f1e4-202">Service Bus AMQP 1.0 fejlesztői útmutató</span><span class="sxs-lookup"><span data-stu-id="7f1e4-202">Service Bus AMQP 1.0 Developer's Guide</span></span>](service-bus-amqp-dotnet.md)
* [<span data-ttu-id="7f1e4-203">Bevezetés a Service Bus által kezelt üzenetsorok használatába</span><span class="sxs-lookup"><span data-stu-id="7f1e4-203">Get started with Service Bus queues</span></span>](service-bus-dotnet-get-started-with-queues.md)
* [<span data-ttu-id="7f1e4-204">Java fejlesztői központban</span><span class="sxs-lookup"><span data-stu-id="7f1e4-204">Java Developer Center</span></span>](https://azure.microsoft.com/develop/java/)

