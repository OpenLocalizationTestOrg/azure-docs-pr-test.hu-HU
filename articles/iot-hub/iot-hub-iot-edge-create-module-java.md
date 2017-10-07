---
title: "egy Azure IoT peremhálózati modul Java aaaCreate |} Microsoft Docs"
description: "Ez az oktatóanyag bővíthető, hogyan egy BLA konverter modul használatával végzett toowrite hello legújabb Azure IoT peremhálózati Maven-csomagokat."
services: iot-hub
author: junyi
manager: timlt
ms.service: iot-hub
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: Java
ms.topic: article
ms.date: 06/28/2017
ms.author: junyi
ms.openlocfilehash: abb560933d13d133ae9a1da08b503d5735b230e0
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="create-an-azure-iot-edge-module-with-java"></a><span data-ttu-id="a99e4-103">Hozzon létre egy Azure IoT peremhálózati modul Java</span><span class="sxs-lookup"><span data-stu-id="a99e4-103">Create an Azure IoT Edge Module with Java</span></span>

<span data-ttu-id="a99e4-104">Ez az oktatóanyag bővíthető, hogyan egy előfordulhat, hogy olyan modul létrehozása az Azure IoT peremhálózati Java nyelven.</span><span class="sxs-lookup"><span data-stu-id="a99e4-104">This tutorial showcases how one might build a module for Azure IoT Edge in Java.</span></span>

<span data-ttu-id="a99e4-105">Ebben az oktatóanyagban végigvezetjük azon környezetet, és hogyan toowrite egy [Gedélyezése](https://en.wikipedia.org/wiki/Bluetooth_Low_Energy) adatok konverter modul hello legújabb Azure IoT peremhálózati Maven-csomagok használata.</span><span class="sxs-lookup"><span data-stu-id="a99e4-105">In this tutorial, we will walk through environment setup and how toowrite a [BLE](https://en.wikipedia.org/wiki/Bluetooth_Low_Energy) data converter module using hello latest Azure IoT Edge Maven packages.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="a99e4-106">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="a99e4-106">Prerequisites</span></span>

<span data-ttu-id="a99e4-107">Ebben a szakaszban a IoT peremhálózati modul fejlesztői környezetben fog beállítani.</span><span class="sxs-lookup"><span data-stu-id="a99e4-107">In this section, you will set up your environment for IoT Edge module development.</span></span> <span data-ttu-id="a99e4-108">Ez a kategória tooboth *64 bites Windows* és *64 bites Linux (8 Ubuntu/Debian)* operációs rendszerek.</span><span class="sxs-lookup"><span data-stu-id="a99e4-108">It applies tooboth *64-bit Windows* and *64-bit Linux (Ubuntu/Debian 8)* operating systems.</span></span>

<span data-ttu-id="a99e4-109">a következő szoftver hello szükség:</span><span class="sxs-lookup"><span data-stu-id="a99e4-109">hello following software is required:</span></span>

* <span data-ttu-id="a99e4-110">[Git ügyfél](https://git-scm.com/downloads).</span><span class="sxs-lookup"><span data-stu-id="a99e4-110">[Git Client](https://git-scm.com/downloads).</span></span>
* <span data-ttu-id="a99e4-111">[**x64** JDK](http://www.oracle.com/technetwork/java/javase/downloads/jdk8-downloads-2133151.html).</span><span class="sxs-lookup"><span data-stu-id="a99e4-111">[**x64** JDK](http://www.oracle.com/technetwork/java/javase/downloads/jdk8-downloads-2133151.html).</span></span>
* <span data-ttu-id="a99e4-112">[Maven](https://maven.apache.org/install.html).</span><span class="sxs-lookup"><span data-stu-id="a99e4-112">[Maven](https://maven.apache.org/install.html).</span></span>

<span data-ttu-id="a99e4-113">Nyisson meg egy parancssori terminál ablakot, és a Klónozás hello a következő tárházat:</span><span class="sxs-lookup"><span data-stu-id="a99e4-113">Open a command-line terminal window and clone hello following repository:</span></span>

1. <span data-ttu-id="a99e4-114">`git clone https://github.com/Azure-Samples/iot-edge-samples.git`.</span><span class="sxs-lookup"><span data-stu-id="a99e4-114">`git clone https://github.com/Azure-Samples/iot-edge-samples.git`.</span></span>
2. `cd iot-edge-samples/java/simulated_ble`

## <a name="overall-architecture"></a><span data-ttu-id="a99e4-115">Általános architektúrája</span><span class="sxs-lookup"><span data-stu-id="a99e4-115">Overall architecture</span></span>

<span data-ttu-id="a99e4-116">hello Azure IoT peremhálózati platform fokozottan fogad hello [Von Neumann architektúra](https://en.wikipedia.org/wiki/Von_Neumann_architecture).</span><span class="sxs-lookup"><span data-stu-id="a99e4-116">hello Azure IoT Edge platform heavily adopts hello [Von Neumann architecture](https://en.wikipedia.org/wiki/Von_Neumann_architecture).</span></span> <span data-ttu-id="a99e4-117">Ez azt jelenti, hogy hello teljes Azure IoT peremhálózati architektúra egy rendszer, amely feldolgozza a bemeneti és a kimenetet; és győződjön meg arról, hogy minden egyes modul is egy nagyon kicsi bemeneti / kimeneti alrendszer.</span><span class="sxs-lookup"><span data-stu-id="a99e4-117">Which means that hello entire Azure IoT Edge architecture is a system which processes input and produces output; and that each individual module is also a tiny input-output subsystem.</span></span> <span data-ttu-id="a99e4-118">Az oktatóanyag azt vezeti be a következő két modulok hello:</span><span class="sxs-lookup"><span data-stu-id="a99e4-118">In this tutorial, we will introduce hello following two modules:</span></span>

1. <span data-ttu-id="a99e4-119">A modul, amely fogadja a szimulált [Gedélyezése](https://en.wikipedia.org/wiki/Bluetooth_Low_Energy) jelezze, és konvertálja azt egy formázott [JSON](https://en.wikipedia.org/wiki/JSON) üzenet.</span><span class="sxs-lookup"><span data-stu-id="a99e4-119">A module which receives a simulated [BLE](https://en.wikipedia.org/wiki/Bluetooth_Low_Energy) signal and converts it into a formatted [JSON](https://en.wikipedia.org/wiki/JSON) message.</span></span>
2. <span data-ttu-id="a99e4-120">A modult, amely a fogadott hello kinyomtatja [JSON](https://en.wikipedia.org/wiki/JSON) üzenet.</span><span class="sxs-lookup"><span data-stu-id="a99e4-120">A module which prints hello received [JSON](https://en.wikipedia.org/wiki/JSON) message.</span></span>

<span data-ttu-id="a99e4-121">hello példánycsoportokat jeleníti meg a projekt hello tipikus végpont adatfolyamban:</span><span class="sxs-lookup"><span data-stu-id="a99e4-121">hello following image displays hello typical end-to-end dataflow for this project:</span></span>

<span data-ttu-id="a99e4-122">![Három modulok között Adatfolyamblokk](media/iot-hub-iot-edge-create-module/dataflow.png "bemenet: szimulált BLA modul; Processzor: Konverter modul; Kimenete: Nyomtató modul")</span><span class="sxs-lookup"><span data-stu-id="a99e4-122">![Dataflow between three modules](media/iot-hub-iot-edge-create-module/dataflow.png "Input: Simulated BLE Module; Processor: Converter Module; Output: Printer Module")</span></span>

## <a name="understanding-hello-code"></a><span data-ttu-id="a99e4-123">Hello kód ismertetése</span><span class="sxs-lookup"><span data-stu-id="a99e4-123">Understanding hello code</span></span>

### <a name="maven-project-structure"></a><span data-ttu-id="a99e4-124">Maven project struktúra</span><span class="sxs-lookup"><span data-stu-id="a99e4-124">Maven project structure</span></span>

<span data-ttu-id="a99e4-125">Mivel az Azure IoT peremhálózati csomagok Maven alapulnak, igazolnia kell toocreate egy tipikus Maven project struktúra, amely tartalmazza a `pom.xml` fájlt.</span><span class="sxs-lookup"><span data-stu-id="a99e4-125">Since Azure IoT Edge packages are based on Maven, we need toocreate a typical Maven project structure, which contains a `pom.xml` file.</span></span>

<span data-ttu-id="a99e4-126">hello POM örököl hello `com.microsoft.azure.gateway.gateway-module-base` csomagot, amely az összes hello futásidejű bináris fájljait, hello átjáró konfigurációs fájl elérési útját és hello végrehajtási viselkedésének modul projekthez szükséges hello függőséggel deklarál.</span><span class="sxs-lookup"><span data-stu-id="a99e4-126">hello POM inherits from hello `com.microsoft.azure.gateway.gateway-module-base` package, which declares all of hello dependencies needed by a module project which includes hello runtime binaries, hello gateway configuration file path, and hello execution behavior.</span></span> <span data-ttu-id="a99e4-127">Ez menti, rengeteg időt hello kell toowrite megszüntetéséhez és sornyi kód több száz újraírási többször.</span><span class="sxs-lookup"><span data-stu-id="a99e4-127">This saves us lots of time and eliminate hello need toowrite and rewrite hundreds of lines of code over and over again.</span></span>

<span data-ttu-id="a99e4-128">Tooupdate a pom.xml fájlt is hello szükséges függőségek/beépülő modulok és a modul által használt, ahogy az a következő kódrészletet hello hello konfigurációs fájl toobe hello nevét deklarálni kell.</span><span class="sxs-lookup"><span data-stu-id="a99e4-128">We need tooupdate the pom.xml file by declaring hello required dependencies/plugins and hello name of hello configuration file toobe used by our module as shown in hello following code snippet.</span></span>

```xml
<project xmlns="http://maven.apache.org/POM/4.0.0"
         xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/xsd/maven-4.0.0.xsd">
  <modelVersion>4.0.0</modelVersion>

  <!-- Inherit from parent -->
  <parent>
    <groupId>com.microsoft.azure.gateway</groupId>
    <artifactId>gateway-module-base</artifactId>
    <version>1.0.1</version>
  </parent>
  
  <groupId>com.microsoft.azure.gateway</groupId>
  <artifactId>ble-converter</artifactId>
  <version>1.0</version>
  <packaging>jar</packaging>

  <!-- Set hello filename of hello Azure IoT Edge configuration located
       under ./src/main/resources/gateway/ which is used in parent -->
  <properties>
    <gw.config.fileName>gw-config.json</gw.config.fileName>
  </properties>

  <!-- Re-declare dependencies used in parent -->
  <dependencies>
    <dependency>
      <groupId>com.microsoft.azure.gateway</groupId>
      <artifactId>gateway-java-binding</artifactId>
    </dependency>
    <dependency>
      <groupId>${dependency.runtime.group}</groupId>
      <artifactId>${dependency.runtime.name}</artifactId>
    </dependency>
  </dependencies>

  <!-- Re-declare plugins used in parent -->
  <build>
    <plugins>
      <plugin>
        <groupId>org.apache.maven.plugins</groupId>
        <artifactId>maven-dependency-plugin</artifactId>
      </plugin>
      <plugin>
        <groupId>org.apache.maven.plugins</groupId>
        <artifactId>maven-resources-plugin</artifactId>
      </plugin>
      <plugin>
        <groupId>org.apache.maven.plugins</groupId>
        <artifactId>maven-shade-plugin</artifactId>
      </plugin>
      <plugin>
        <groupId>org.codehaus.mojo</groupId>
        <artifactId>exec-maven-plugin</artifactId>
      </plugin>
    </plugins>
  </build>
</project>
```

### <a name="basic-understanding-of-an-azure-iot-edge-module"></a><span data-ttu-id="a99e4-129">Egy Azure IoT peremhálózati modul alapvető ismeretekkel</span><span class="sxs-lookup"><span data-stu-id="a99e4-129">Basic understanding of an Azure IoT Edge module</span></span>

<span data-ttu-id="a99e4-130">Egy Azure IoT Edge-modul is kezelheti, amelynek a feladata, feldolgozónak: fogadja, feldolgozni azt és eredménye.</span><span class="sxs-lookup"><span data-stu-id="a99e4-130">You can treat an Azure IoT Edge module as a data processor whose job is to: receive input, process it, and produce output.</span></span>

<span data-ttu-id="a99e4-131">hello bemeneti hardverek (például egy mozgásérzékelő) adatait, egy üzenet más modulok, vagy bármi más (például egy időzítő rendszeresen generálja véletlenszerűen) lehet.</span><span class="sxs-lookup"><span data-stu-id="a99e4-131">hello input might be data from hardware (like a motion detector), a message from other modules, or anything else (like a random number generated periodically by a timer).</span></span>

<span data-ttu-id="a99e4-132">hello kimeneti hasonló toohello bemeneti, hogy elindíthatja hardver viselkedés (például LED villogó hello), egy üzenet tooother modulok vagy bármi más (például a nyomtatási toohello konzol).</span><span class="sxs-lookup"><span data-stu-id="a99e4-132">hello output is similar toohello input, it could trigger hardware behavior (like hello blinking LED), a message tooother modules, or anything else (like printing toohello console).</span></span>

<span data-ttu-id="a99e4-133">Modulok kommunikálnak egymással használatával `com.microsoft.azure.gateway.messaging.Message` osztály.</span><span class="sxs-lookup"><span data-stu-id="a99e4-133">Modules communicate with each other using `com.microsoft.azure.gateway.messaging.Message` class.</span></span> <span data-ttu-id="a99e4-134">Hello **tartalom** , egy `Message` bájt tömb, amely bármilyen típusú adatok, például képes.</span><span class="sxs-lookup"><span data-stu-id="a99e4-134">hello **Content** of a `Message` is a byte array which is capable of representing any kind of data you like.</span></span> <span data-ttu-id="a99e4-135">**Tulajdonságok** is rendelkezésre állnak a hello `Message` és egyszerűen egy karakterlánc-karakterlánc leképezése.</span><span class="sxs-lookup"><span data-stu-id="a99e4-135">**Properties** are also available in hello `Message` and are simply a string-to-string mapping.</span></span> <span data-ttu-id="a99e4-136">Elképzelhető, hogy a **tulajdonságok** hello fejlécként HTTP-kérelem vagy hello metaadat-fájl.</span><span class="sxs-lookup"><span data-stu-id="a99e4-136">You may think of **Properties** as hello headers in an HTTP request, or hello metadata of a file.</span></span>

<span data-ttu-id="a99e4-137">Rendelés toodevelop egy Azure IoT Edge-modul a Java, egy új modul osztály, amely örökli toocreate kell `com.microsoft.azure.gateway.core.GatewayModule` és megvalósításához szükséges hello absztrakt metódusok `receive()` és `destroy()`.</span><span class="sxs-lookup"><span data-stu-id="a99e4-137">In order toodevelop an Azure IoT Edge module in Java, you need toocreate a new module class which inherits from `com.microsoft.azure.gateway.core.GatewayModule` and implement hello required abstract methods `receive()` and `destroy()`.</span></span> <span data-ttu-id="a99e4-138">Ezen a ponton is választhatja, hogy tooimplement hello választható `start()` vagy `create()` módszerek is.</span><span class="sxs-lookup"><span data-stu-id="a99e4-138">At this point, you may also choose tooimplement hello optional `start()` or `create()` methods as well.</span></span> <span data-ttu-id="a99e4-139">a következő kódrészletet hello bemutatja, hogyan tooget el az Azure IoT peremhálózati modul szerzői.</span><span class="sxs-lookup"><span data-stu-id="a99e4-139">hello following code snippet shows you how tooget started authoring an Azure IoT Edge module.</span></span>

```java
import com.microsoft.azure.gateway.core.Broker;
import com.microsoft.azure.gateway.core.GatewayModule;
import com.microsoft.azure.gateway.messaging.Message;

public class MyEdgeModule extends GatewayModule {
  public MyEdgeModule(long address, Broker broker, String configuration) {
    /* Let hello GatewayModule do hello dirty work of initialization. It's also
       a good time tooparse your own configuration defined in Azure IoT Edge
       configuration file (typically ./src/main/resources/gateway/gw-config.json) */
    super(address, broker, configuration);
  }

  @Override
  public void start() {
    /* Acquire hello resources you need. If you don't
       need any resources, you may omit this method. */
  }

  @Override
  public void destroy() {
    /* It's time toorelease all resources. This method is required. */
  }

  @Override
  public void receive(Message message) {
    /* Logic tooprocess hello input message. This method is required. */
    // ...
    /* Use publish() method toodo hello output. You are
       allowed toopublish your new Message instance. */
    this.publish(message);
  }
}
```

### <a name="converter-module"></a><span data-ttu-id="a99e4-140">Konverter modul</span><span class="sxs-lookup"><span data-stu-id="a99e4-140">Converter module</span></span>

| <span data-ttu-id="a99e4-141">Input (Bemenet)</span><span class="sxs-lookup"><span data-stu-id="a99e4-141">Input</span></span>                    | <span data-ttu-id="a99e4-142">Processzor</span><span class="sxs-lookup"><span data-stu-id="a99e4-142">Processor</span></span>                              | <span data-ttu-id="a99e4-143">Kimenet</span><span class="sxs-lookup"><span data-stu-id="a99e4-143">Output</span></span>                 | <span data-ttu-id="a99e4-144">Forrásfájl</span><span class="sxs-lookup"><span data-stu-id="a99e4-144">Source File</span></span>            |
| ------------------------ | -------------------------------------- | ---------------------- | ---------------------- |
| <span data-ttu-id="a99e4-145">Hőmérséklet-adatok üzenet</span><span class="sxs-lookup"><span data-stu-id="a99e4-145">Temperature data message</span></span> | <span data-ttu-id="a99e4-146">Elemzése és létrehozni egy új JSON-üzenet</span><span class="sxs-lookup"><span data-stu-id="a99e4-146">Parse and construct a new JSON message</span></span> | <span data-ttu-id="a99e4-147">Struktúra JSON üzenet</span><span class="sxs-lookup"><span data-stu-id="a99e4-147">Structure JSON message</span></span> | `ConverterModule.java` |

<span data-ttu-id="a99e4-148">Ez a modul az egy tipikus Azure IoT peremhálózati modul.</span><span class="sxs-lookup"><span data-stu-id="a99e4-148">This module is a typical Azure IoT Edge module.</span></span> <span data-ttu-id="a99e4-149">Hőmérséklet-üzenetek származó adatok elfogad (hardveres modult, vagy ilyen esetben a szimulált BLA modul); és majd normalizálja hőmérséklet üdvözlőüzenetére strukturált tooa JSON üzenet (beleértve a fűznek hello Üzenetazonosítója hello tulajdonságának e igazolnia kell tootrigger hello hőmérséklet riasztást, és így tovább).</span><span class="sxs-lookup"><span data-stu-id="a99e4-149">It accepts temperature messages from other modules (a hardware module, or in this case our simulated BLE module); and then normalizes hello temperature message in tooa structured JSON message (including appending hello message ID, setting hello property of whether we need tootrigger hello temperature alert, and so on).</span></span>

```java
@Override
public void receive(Message message) {
  try {
    JSONObject messageFromBle = new JSONObject(new String(message.getContent()));
    double temperature = messageFromBle.getDouble("temperature");
    Map<String, String> inputProperties = message.getProperties();

    HashMap<String, String> properties = new HashMap<>();
    properties.put("source", inputProperties.get("source"));
    properties.put("macAddress", inputProperties.get("macAddress"));
    properties.put("temperatureAlert", temperature > 30 ? "true" : "false");

    String content = String.format(
        "{ \"deviceId\": \"Intel NUC Gateway\", \"messageId\": %d, \"temperature\": %f }",
        ++this.messageCount, temperature);

    this.publish(new Message(content.getBytes(), properties));
  } catch (Exception ex) {
    ex.printStackTrace();
  }
}
```

### <a name="printer-module"></a><span data-ttu-id="a99e4-150">Nyomtató modul</span><span class="sxs-lookup"><span data-stu-id="a99e4-150">Printer module</span></span>

| <span data-ttu-id="a99e4-151">Input (Bemenet)</span><span class="sxs-lookup"><span data-stu-id="a99e4-151">Input</span></span>                          | <span data-ttu-id="a99e4-152">Processzor</span><span class="sxs-lookup"><span data-stu-id="a99e4-152">Processor</span></span> | <span data-ttu-id="a99e4-153">Kimenet</span><span class="sxs-lookup"><span data-stu-id="a99e4-153">Output</span></span>                     | <span data-ttu-id="a99e4-154">Forrásfájl</span><span class="sxs-lookup"><span data-stu-id="a99e4-154">Source File</span></span>          |
| ------------------------------ | --------- | -------------------------- | -------------------- |
| <span data-ttu-id="a99e4-155">Bármely más modulok érkezett üzenetet:</span><span class="sxs-lookup"><span data-stu-id="a99e4-155">Any message from other modules</span></span> | <span data-ttu-id="a99e4-156">N/A</span><span class="sxs-lookup"><span data-stu-id="a99e4-156">N/A</span></span>       | <span data-ttu-id="a99e4-157">Hello üzenet tooconsole naplózása</span><span class="sxs-lookup"><span data-stu-id="a99e4-157">Log hello message tooconsole</span></span> | `PrinterModule.java` |

<span data-ttu-id="a99e4-158">Ez az egy egyszerű, értetődő, modult, amely hello fogadott üzenetek toohello terminálablakot kimenete.</span><span class="sxs-lookup"><span data-stu-id="a99e4-158">This is a simple, self-explanatory, module which outputs hello received messages toohello terminal window.</span></span>

```java
@Override
public void receive(Message message) {
  System.out.println(message.toString());
}
```

### <a name="azure-iot-edge-configuration"></a><span data-ttu-id="a99e4-159">Azure IoT peremhálózati konfigurálása</span><span class="sxs-lookup"><span data-stu-id="a99e4-159">Azure IoT Edge configuration</span></span>

<span data-ttu-id="a99e4-160">hello utolsó lépés előtt futó hello modulok tooconfigure hello Azure IoT peremhálózati és tooestablish hello kapcsolatok modulok között.</span><span class="sxs-lookup"><span data-stu-id="a99e4-160">hello final step before running hello modules is tooconfigure hello Azure IoT Edge and tooestablish hello connections between modules.</span></span>

<span data-ttu-id="a99e4-161">Először igazolnia kell toodeclare a Java betöltő (óta Azure IoT peremhálózati támogatja betöltők különböző nyelvű) sikerült hivatkozhat, amely a `name` hello szakaszok ezt követően.</span><span class="sxs-lookup"><span data-stu-id="a99e4-161">First we need toodeclare our Java loader (since Azure IoT Edge supports loaders of different languages) which could be referenced by its `name` in hello sections afterward.</span></span>

```json
"loaders": [{
  "type": "java",
  "name": "java",
  "configuration": {
    "jvm.options": {
      "library.path": "./"
    }
  }
}]
```

<span data-ttu-id="a99e4-162">Miután a betöltők rendelkezik deklaráltuk, azt is kell toodeclare, valamint a modulok.</span><span class="sxs-lookup"><span data-stu-id="a99e4-162">Once we have declared our loaders, we will also need toodeclare our modules as well.</span></span> <span data-ttu-id="a99e4-163">Hasonló toodeclaring hello betöltők, azokat is hivatkozhat a `name` attribútum.</span><span class="sxs-lookup"><span data-stu-id="a99e4-163">Similar toodeclaring hello loaders, they can also be referenced by their `name` attribute.</span></span> <span data-ttu-id="a99e4-164">Ha egy modul deklaráló, igazolnia kell a toospecify hello betöltő azt kell használnia (amely előtt meghatározott egyik hello kellene lennie) és a belépési pont (kell lennie a modul hello normalizált osztály neve) minden modul hello.</span><span class="sxs-lookup"><span data-stu-id="a99e4-164">When declaring a module, we need toospecify hello loader it should use (which should be hello one we defined before) and hello entry-point (should be hello normalized class name of our module) for each module.</span></span> <span data-ttu-id="a99e4-165">Hello `simulated_device` modul az hello Azure IoT peremhálózati core runtime csomag részét képező natív modul.</span><span class="sxs-lookup"><span data-stu-id="a99e4-165">hello `simulated_device` module is a native module which is included in hello Azure IoT Edge core runtime package.</span></span> <span data-ttu-id="a99e4-166">Meg kell adnia `args` hello JSON fájl, akkor is, ha a `null`.</span><span class="sxs-lookup"><span data-stu-id="a99e4-166">You should always include `args` in hello JSON file even if it is `null`.</span></span>

```json
"modules": [
  {
    "name": "simulated_device",
    "loader": {
      "name": "native",
      "entrypoint": {
        "module.path": "simulated_device"
      }
    },
    "args": {
      "macAddress": "01:02:03:03:02:01",
      "messagePeriod": 500
    }
  },
  {
    "name": "converter",
    "loader": {
      "name": "java",
      "entrypoint": {
        "class.name": "com/microsoft/azure/gateway/ConverterModule",
        "class.path": "./ble-converter-1.0-with-deps.jar"
      }
    },
    "args": null
  },
  {
    "name": "print",
    "loader": {
      "name": "java",
      "entrypoint": {
        "class.name": "com/microsoft/azure/gateway/PrinterModule",
        "class.path": "./ble-converter-1.0-with-deps.jar"
      }
    },
    "args": null
  }
]
```

<span data-ttu-id="a99e4-167">Hello konfigurációs hello végén a Microsoft hello-kapcsolatok létesítéséhez.</span><span class="sxs-lookup"><span data-stu-id="a99e4-167">At hello end of hello configuration, we establish hello connections.</span></span> <span data-ttu-id="a99e4-168">Minden kapcsolat van kifejezve `source` és `sink`.</span><span class="sxs-lookup"><span data-stu-id="a99e4-168">Each connection is expressed by `source` and `sink`.</span></span> <span data-ttu-id="a99e4-169">Kell mindkét hivatkozó egy előre definiált modul.</span><span class="sxs-lookup"><span data-stu-id="a99e4-169">They should both reference a pre-defined module.</span></span> <span data-ttu-id="a99e4-170">a kimeneti üdvözlőüzenetére `source` modul továbbítják toohello bemeneti `sink` modul.</span><span class="sxs-lookup"><span data-stu-id="a99e4-170">hello output message of `source` module will be forwarded toohello input of `sink` module.</span></span>

```json
"links": [
  {
    "source": "simulated_device",
    "sink": "converter"
  },
  {
    "source": "converter",
    "sink": "print"
  }
]
```

## <a name="running-hello-modules"></a><span data-ttu-id="a99e4-171">Hello modulok fut</span><span class="sxs-lookup"><span data-stu-id="a99e4-171">Running hello modules</span></span>

<span data-ttu-id="a99e4-172">Használjon `mvn package` toobuild mindent be hello `target/` mappát.</span><span class="sxs-lookup"><span data-stu-id="a99e4-172">Use `mvn package` toobuild everything into hello `target/` folder.</span></span> <span data-ttu-id="a99e4-173">`mvn clean package`össze is ajánlott.</span><span class="sxs-lookup"><span data-stu-id="a99e4-173">`mvn clean package` is also recommended for a clean build.</span></span>

<span data-ttu-id="a99e4-174">Használjon `mvn exec:exec` toorun hello Azure IoT él, és figyelnie kell-e, hogy hello hőmérséklet adatok és az összes hello tulajdonság-e a rögzített kulcs nyomtatott toohello konzol.</span><span class="sxs-lookup"><span data-stu-id="a99e4-174">Use `mvn exec:exec` toorun hello Azure IoT Edge and you should observe that hello temperature data and all hello properties are printed toohello console at a fixed rate.</span></span>

<span data-ttu-id="a99e4-175">Ha azt szeretné, hogy tooterminate hello alkalmazás, nyomja meg az `<Enter>` kulcs.</span><span class="sxs-lookup"><span data-stu-id="a99e4-175">If you want tooterminate hello application, press `<Enter>` key.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="a99e4-176">Nem ajánlott toouse Ctrl + C tooterminate hello IoT peremhálózati átjáró alkalmazás.</span><span class="sxs-lookup"><span data-stu-id="a99e4-176">It is not recommended toouse Ctrl + C tooterminate hello IoT Edge gateway application.</span></span> <span data-ttu-id="a99e4-177">Mivel ez hello folyamat tooterminate rendellenesen okozhatja.</span><span class="sxs-lookup"><span data-stu-id="a99e4-177">As this may cause hello process tooterminate abnormally.</span></span>

