---
title: "Hozzon létre egy Azure IoT peremhálózati modul Java |} Microsoft Docs"
description: "Ez az oktatóanyag a legújabb Azure IoT peremhálózati Maven-csomagok használata BLA adatok konverter modul írásával bővíthető."
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
ms.openlocfilehash: 0c430272225d79737baec2be15ed7c93991cdeac
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/03/2017
---
# <a name="create-an-azure-iot-edge-module-with-java"></a><span data-ttu-id="0edd6-103">Hozzon létre egy Azure IoT peremhálózati modul Java</span><span class="sxs-lookup"><span data-stu-id="0edd6-103">Create an Azure IoT Edge Module with Java</span></span>

<span data-ttu-id="0edd6-104">Ez az oktatóanyag bővíthető, hogyan egy előfordulhat, hogy olyan modul létrehozása az Azure IoT peremhálózati Java nyelven.</span><span class="sxs-lookup"><span data-stu-id="0edd6-104">This tutorial showcases how one might build a module for Azure IoT Edge in Java.</span></span>

<span data-ttu-id="0edd6-105">Ebben az oktatóanyagban végigvezetjük környezetben való telepítés és írásával egy [Gedélyezése](https://en.wikipedia.org/wiki/Bluetooth_Low_Energy) adatok konverter moduljának használatával. a legújabb Azure IoT peremhálózati Maven-csomagokat.</span><span class="sxs-lookup"><span data-stu-id="0edd6-105">In this tutorial, we will walk through environment setup and how to write a [BLE](https://en.wikipedia.org/wiki/Bluetooth_Low_Energy) data converter module using the latest Azure IoT Edge Maven packages.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="0edd6-106">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="0edd6-106">Prerequisites</span></span>

<span data-ttu-id="0edd6-107">Ebben a szakaszban a IoT peremhálózati modul fejlesztői környezetben fog beállítani.</span><span class="sxs-lookup"><span data-stu-id="0edd6-107">In this section, you will set up your environment for IoT Edge module development.</span></span> <span data-ttu-id="0edd6-108">Ez egyaránt vonatkozik *64 bites Windows* és *64 bites Linux (8 Ubuntu/Debian)* operációs rendszerek.</span><span class="sxs-lookup"><span data-stu-id="0edd6-108">It applies to both *64-bit Windows* and *64-bit Linux (Ubuntu/Debian 8)* operating systems.</span></span>

<span data-ttu-id="0edd6-109">A következő szoftvereket is szükséges:</span><span class="sxs-lookup"><span data-stu-id="0edd6-109">The following software is required:</span></span>

* <span data-ttu-id="0edd6-110">[Git ügyfél](https://git-scm.com/downloads).</span><span class="sxs-lookup"><span data-stu-id="0edd6-110">[Git Client](https://git-scm.com/downloads).</span></span>
* <span data-ttu-id="0edd6-111">[**x64** JDK](http://www.oracle.com/technetwork/java/javase/downloads/jdk8-downloads-2133151.html).</span><span class="sxs-lookup"><span data-stu-id="0edd6-111">[**x64** JDK](http://www.oracle.com/technetwork/java/javase/downloads/jdk8-downloads-2133151.html).</span></span>
* <span data-ttu-id="0edd6-112">[Maven](https://maven.apache.org/install.html).</span><span class="sxs-lookup"><span data-stu-id="0edd6-112">[Maven](https://maven.apache.org/install.html).</span></span>

<span data-ttu-id="0edd6-113">Nyisson meg egy parancssori terminálablakot, és a következő tárház klónozása:</span><span class="sxs-lookup"><span data-stu-id="0edd6-113">Open a command-line terminal window and clone the following repository:</span></span>

1. <span data-ttu-id="0edd6-114">`git clone https://github.com/Azure-Samples/iot-edge-samples.git`.</span><span class="sxs-lookup"><span data-stu-id="0edd6-114">`git clone https://github.com/Azure-Samples/iot-edge-samples.git`.</span></span>
2. `cd iot-edge-samples/java/simulated_ble`

## <a name="overall-architecture"></a><span data-ttu-id="0edd6-115">Általános architektúrája</span><span class="sxs-lookup"><span data-stu-id="0edd6-115">Overall architecture</span></span>

<span data-ttu-id="0edd6-116">Az Azure IoT peremhálózati platform fokozottan elfogadja a [Von Neumann architektúra](https://en.wikipedia.org/wiki/Von_Neumann_architecture).</span><span class="sxs-lookup"><span data-stu-id="0edd6-116">The Azure IoT Edge platform heavily adopts the [Von Neumann architecture](https://en.wikipedia.org/wiki/Von_Neumann_architecture).</span></span> <span data-ttu-id="0edd6-117">Ami azt jelenti, hogy a teljes Azure IoT-biztonsági architektúrája a rendszer, amely feldolgozza a bemeneti és a kimenetet; és győződjön meg arról, hogy minden egyes modul is egy nagyon kicsi bemeneti / kimeneti alrendszer.</span><span class="sxs-lookup"><span data-stu-id="0edd6-117">Which means that the entire Azure IoT Edge architecture is a system which processes input and produces output; and that each individual module is also a tiny input-output subsystem.</span></span> <span data-ttu-id="0edd6-118">Az oktatóanyag azt vezeti be a következő két modulok:</span><span class="sxs-lookup"><span data-stu-id="0edd6-118">In this tutorial, we will introduce the following two modules:</span></span>

1. <span data-ttu-id="0edd6-119">A modul, amely fogadja a szimulált [Gedélyezése](https://en.wikipedia.org/wiki/Bluetooth_Low_Energy) jelezze, és konvertálja azt egy formázott [JSON](https://en.wikipedia.org/wiki/JSON) üzenet.</span><span class="sxs-lookup"><span data-stu-id="0edd6-119">A module which receives a simulated [BLE](https://en.wikipedia.org/wiki/Bluetooth_Low_Energy) signal and converts it into a formatted [JSON](https://en.wikipedia.org/wiki/JSON) message.</span></span>
2. <span data-ttu-id="0edd6-120">A modult, amely a fogadott kinyomtatja [JSON](https://en.wikipedia.org/wiki/JSON) üzenet.</span><span class="sxs-lookup"><span data-stu-id="0edd6-120">A module which prints the received [JSON](https://en.wikipedia.org/wiki/JSON) message.</span></span>

<span data-ttu-id="0edd6-121">Az alábbi kép az ebben a projektben tipikus végpont adatfolyamban jeleníti meg:</span><span class="sxs-lookup"><span data-stu-id="0edd6-121">The following image displays the typical end-to-end dataflow for this project:</span></span>

<span data-ttu-id="0edd6-122">![Három modulok között Adatfolyamblokk](media/iot-hub-iot-edge-create-module/dataflow.png "bemenet: szimulált BLA modul; Processzor: Konverter modul; Kimenete: Nyomtató modul")</span><span class="sxs-lookup"><span data-stu-id="0edd6-122">![Dataflow between three modules](media/iot-hub-iot-edge-create-module/dataflow.png "Input: Simulated BLE Module; Processor: Converter Module; Output: Printer Module")</span></span>

## <a name="understanding-the-code"></a><span data-ttu-id="0edd6-123">A kód ismertetése</span><span class="sxs-lookup"><span data-stu-id="0edd6-123">Understanding the code</span></span>

### <a name="maven-project-structure"></a><span data-ttu-id="0edd6-124">Maven project struktúra</span><span class="sxs-lookup"><span data-stu-id="0edd6-124">Maven project structure</span></span>

<span data-ttu-id="0edd6-125">Mivel az Azure IoT peremhálózati csomagok Maven alapulnak, létre kell hoznunk egy tipikus Maven project struktúra, amely tartalmazza a `pom.xml` fájlt.</span><span class="sxs-lookup"><span data-stu-id="0edd6-125">Since Azure IoT Edge packages are based on Maven, we need to create a typical Maven project structure, which contains a `pom.xml` file.</span></span>

<span data-ttu-id="0edd6-126">A POM örökli a `com.microsoft.azure.gateway.gateway-module-base` csomagot, amely a függőségeket, beleértve a futásidejű bináris fájljait, az átjáró konfigurációs fájl elérési útját és végrehajtási viselkedésének modul projekthez szükséges összes deklarál.</span><span class="sxs-lookup"><span data-stu-id="0edd6-126">The POM inherits from the `com.microsoft.azure.gateway.gateway-module-base` package, which declares all of the dependencies needed by a module project which includes the runtime binaries, the gateway configuration file path, and the execution behavior.</span></span> <span data-ttu-id="0edd6-127">Ez menti, rengeteg időt, és szükségtelenné teszik írásához és sornyi kód több száz újraírási többször.</span><span class="sxs-lookup"><span data-stu-id="0edd6-127">This saves us lots of time and eliminate the need to write and rewrite hundreds of lines of code over and over again.</span></span>

<span data-ttu-id="0edd6-128">A pom.xml fájlt is deklarálni kell a szükséges függőségek/beépülő modulok és a neve, ahogy az a következő kódrészletet a modul által használandó konfigurációs fájljának frissíteni kell.</span><span class="sxs-lookup"><span data-stu-id="0edd6-128">We need to update the pom.xml file by declaring the required dependencies/plugins and the name of the configuration file to be used by our module as shown in the following code snippet.</span></span>

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

  <!-- Set the filename of the Azure IoT Edge configuration located
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

### <a name="basic-understanding-of-an-azure-iot-edge-module"></a><span data-ttu-id="0edd6-129">Egy Azure IoT peremhálózati modul alapvető ismeretekkel</span><span class="sxs-lookup"><span data-stu-id="0edd6-129">Basic understanding of an Azure IoT Edge module</span></span>

<span data-ttu-id="0edd6-130">Egy Azure IoT Edge-modul is kezelheti, amelynek a feladata, feldolgozónak: fogadja, feldolgozni azt és eredménye.</span><span class="sxs-lookup"><span data-stu-id="0edd6-130">You can treat an Azure IoT Edge module as a data processor whose job is to: receive input, process it, and produce output.</span></span>

<span data-ttu-id="0edd6-131">Lehet, hogy a bemeneti hardverek (például egy mozgásérzékelő) adatait, az egyéb modulok vagy bármely más (például egy időzítő rendszeresen generálja véletlenszerűen) üzenet.</span><span class="sxs-lookup"><span data-stu-id="0edd6-131">The input might be data from hardware (like a motion detector), a message from other modules, or anything else (like a random number generated periodically by a timer).</span></span>

<span data-ttu-id="0edd6-132">A kimenet a bemeneti hasonló, hogy elindíthatja hardver viselkedés (például a villogó LED), egy üzenetet, amely más modulok vagy bármely más (például a konzol nyomtatás).</span><span class="sxs-lookup"><span data-stu-id="0edd6-132">The output is similar to the input, it could trigger hardware behavior (like the blinking LED), a message to other modules, or anything else (like printing to the console).</span></span>

<span data-ttu-id="0edd6-133">Modulok kommunikálnak egymással használatával `com.microsoft.azure.gateway.messaging.Message` osztály.</span><span class="sxs-lookup"><span data-stu-id="0edd6-133">Modules communicate with each other using `com.microsoft.azure.gateway.messaging.Message` class.</span></span> <span data-ttu-id="0edd6-134">A **tartalom** , egy `Message` bájt tömb, amely bármilyen típusú adatok, például képes.</span><span class="sxs-lookup"><span data-stu-id="0edd6-134">The **Content** of a `Message` is a byte array which is capable of representing any kind of data you like.</span></span> <span data-ttu-id="0edd6-135">**Tulajdonságok** is elérhetők a `Message` és egyszerűen egy karakterlánc-karakterlánc leképezése.</span><span class="sxs-lookup"><span data-stu-id="0edd6-135">**Properties** are also available in the `Message` and are simply a string-to-string mapping.</span></span> <span data-ttu-id="0edd6-136">Elképzelhető, hogy a **tulajdonságok** , a fejléceket a HTTP-kérelem és a fájl metaadatait.</span><span class="sxs-lookup"><span data-stu-id="0edd6-136">You may think of **Properties** as the headers in an HTTP request, or the metadata of a file.</span></span>

<span data-ttu-id="0edd6-137">Egy Azure IoT Edge-modul a Java elkészítéséhez hozzon létre egy új modul osztályt, amely örökli kell `com.microsoft.azure.gateway.core.GatewayModule` és a szükséges absztrakt metódusok végrehajtása `receive()` és `destroy()`.</span><span class="sxs-lookup"><span data-stu-id="0edd6-137">In order to develop an Azure IoT Edge module in Java, you need to create a new module class which inherits from `com.microsoft.azure.gateway.core.GatewayModule` and implement the required abstract methods `receive()` and `destroy()`.</span></span> <span data-ttu-id="0edd6-138">Ezen a ponton is választhatja, hogy a nem kötelező végrehajtásához `start()` vagy `create()` módszerek is.</span><span class="sxs-lookup"><span data-stu-id="0edd6-138">At this point, you may also choose to implement the optional `start()` or `create()` methods as well.</span></span> <span data-ttu-id="0edd6-139">A következő kódrészletet Ismerkedés az Azure IoT peremhálózati modul szerzői mutatja.</span><span class="sxs-lookup"><span data-stu-id="0edd6-139">The following code snippet shows you how to get started authoring an Azure IoT Edge module.</span></span>

```java
import com.microsoft.azure.gateway.core.Broker;
import com.microsoft.azure.gateway.core.GatewayModule;
import com.microsoft.azure.gateway.messaging.Message;

public class MyEdgeModule extends GatewayModule {
  public MyEdgeModule(long address, Broker broker, String configuration) {
    /* Let the GatewayModule do the dirty work of initialization. It's also
       a good time to parse your own configuration defined in Azure IoT Edge
       configuration file (typically ./src/main/resources/gateway/gw-config.json) */
    super(address, broker, configuration);
  }

  @Override
  public void start() {
    /* Acquire the resources you need. If you don't
       need any resources, you may omit this method. */
  }

  @Override
  public void destroy() {
    /* It's time to release all resources. This method is required. */
  }

  @Override
  public void receive(Message message) {
    /* Logic to process the input message. This method is required. */
    // ...
    /* Use publish() method to do the output. You are
       allowed to publish your new Message instance. */
    this.publish(message);
  }
}
```

### <a name="converter-module"></a><span data-ttu-id="0edd6-140">Konverter modul</span><span class="sxs-lookup"><span data-stu-id="0edd6-140">Converter module</span></span>

| <span data-ttu-id="0edd6-141">Input (Bemenet)</span><span class="sxs-lookup"><span data-stu-id="0edd6-141">Input</span></span>                    | <span data-ttu-id="0edd6-142">Processzor</span><span class="sxs-lookup"><span data-stu-id="0edd6-142">Processor</span></span>                              | <span data-ttu-id="0edd6-143">Kimenet</span><span class="sxs-lookup"><span data-stu-id="0edd6-143">Output</span></span>                 | <span data-ttu-id="0edd6-144">Forrásfájl</span><span class="sxs-lookup"><span data-stu-id="0edd6-144">Source File</span></span>            |
| ------------------------ | -------------------------------------- | ---------------------- | ---------------------- |
| <span data-ttu-id="0edd6-145">Hőmérséklet-adatok üzenet</span><span class="sxs-lookup"><span data-stu-id="0edd6-145">Temperature data message</span></span> | <span data-ttu-id="0edd6-146">Elemzése és létrehozni egy új JSON-üzenet</span><span class="sxs-lookup"><span data-stu-id="0edd6-146">Parse and construct a new JSON message</span></span> | <span data-ttu-id="0edd6-147">Struktúra JSON üzenet</span><span class="sxs-lookup"><span data-stu-id="0edd6-147">Structure JSON message</span></span> | `ConverterModule.java` |

<span data-ttu-id="0edd6-148">Ez a modul az egy tipikus Azure IoT peremhálózati modul.</span><span class="sxs-lookup"><span data-stu-id="0edd6-148">This module is a typical Azure IoT Edge module.</span></span> <span data-ttu-id="0edd6-149">Hőmérséklet-üzenetek származó adatok elfogad (hardveres modult, vagy ilyen esetben a szimulált BLA modul); és majd normalizálja az hőmérséklet üzenet (beleértve a Hozzáfűzés az Üzenetazonosító, hogy igazolnia kell a hőmérséklet figyelmeztetést, és így tovább tulajdonságának beállítása) strukturált JSON-üzenethez.</span><span class="sxs-lookup"><span data-stu-id="0edd6-149">It accepts temperature messages from other modules (a hardware module, or in this case our simulated BLE module); and then normalizes the temperature message in to a structured JSON message (including appending the message ID, setting the property of whether we need to trigger the temperature alert, and so on).</span></span>

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

### <a name="printer-module"></a><span data-ttu-id="0edd6-150">Nyomtató modul</span><span class="sxs-lookup"><span data-stu-id="0edd6-150">Printer module</span></span>

| <span data-ttu-id="0edd6-151">Input (Bemenet)</span><span class="sxs-lookup"><span data-stu-id="0edd6-151">Input</span></span>                          | <span data-ttu-id="0edd6-152">Processzor</span><span class="sxs-lookup"><span data-stu-id="0edd6-152">Processor</span></span> | <span data-ttu-id="0edd6-153">Kimenet</span><span class="sxs-lookup"><span data-stu-id="0edd6-153">Output</span></span>                     | <span data-ttu-id="0edd6-154">Forrásfájl</span><span class="sxs-lookup"><span data-stu-id="0edd6-154">Source File</span></span>          |
| ------------------------------ | --------- | -------------------------- | -------------------- |
| <span data-ttu-id="0edd6-155">Bármely más modulok érkezett üzenetet:</span><span class="sxs-lookup"><span data-stu-id="0edd6-155">Any message from other modules</span></span> | <span data-ttu-id="0edd6-156">N/A</span><span class="sxs-lookup"><span data-stu-id="0edd6-156">N/A</span></span>       | <span data-ttu-id="0edd6-157">Az üzenet naplózása konzolhoz</span><span class="sxs-lookup"><span data-stu-id="0edd6-157">Log the message to console</span></span> | `PrinterModule.java` |

<span data-ttu-id="0edd6-158">Ez az egy egyszerű, értetődő, modult, amely a fogadott üzenetek a terminálablakot kimenete.</span><span class="sxs-lookup"><span data-stu-id="0edd6-158">This is a simple, self-explanatory, module which outputs the received messages to the terminal window.</span></span>

```java
@Override
public void receive(Message message) {
  System.out.println(message.toString());
}
```

### <a name="azure-iot-edge-configuration"></a><span data-ttu-id="0edd6-159">Azure IoT peremhálózati konfigurálása</span><span class="sxs-lookup"><span data-stu-id="0edd6-159">Azure IoT Edge configuration</span></span>

<span data-ttu-id="0edd6-160">Az utolsó lépés a modulok futtatása előtt a konfigurálása az Azure IoT él és a kapcsolatot létrehozni a modulok között.</span><span class="sxs-lookup"><span data-stu-id="0edd6-160">The final step before running the modules is to configure the Azure IoT Edge and to establish the connections between modules.</span></span>

<span data-ttu-id="0edd6-161">Először a Java betöltő (óta Azure IoT peremhálózati támogatja betöltők különböző nyelvű) amely sikerült hivatkozhat deklarálnia kell annak `name` ezt követően a szakaszokban.</span><span class="sxs-lookup"><span data-stu-id="0edd6-161">First we need to declare our Java loader (since Azure IoT Edge supports loaders of different languages) which could be referenced by its `name` in the sections afterward.</span></span>

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

<span data-ttu-id="0edd6-162">Miután a betöltők rendelkezik deklaráltuk, azt is kell a modulok, valamint deklarálja.</span><span class="sxs-lookup"><span data-stu-id="0edd6-162">Once we have declared our loaders, we will also need to declare our modules as well.</span></span> <span data-ttu-id="0edd6-163">Hasonló a betöltők deklaráló, azokat is hivatkozhat a `name` attribútum.</span><span class="sxs-lookup"><span data-stu-id="0edd6-163">Similar to declaring the loaders, they can also be referenced by their `name` attribute.</span></span> <span data-ttu-id="0edd6-164">Egy modul deklaráló, azt kell adnia a betöltő azt kell használnia (amely az legyen előtt meghatározott) és a belépési pont (a normalizált osztály neve a modul kellene lennie) minden modulhoz.</span><span class="sxs-lookup"><span data-stu-id="0edd6-164">When declaring a module, we need to specify the loader it should use (which should be the one we defined before) and the entry-point (should be the normalized class name of our module) for each module.</span></span> <span data-ttu-id="0edd6-165">A `simulated_device` modul az egy natív modult, amely az Azure IoT peremhálózati core runtime csomagban található.</span><span class="sxs-lookup"><span data-stu-id="0edd6-165">The `simulated_device` module is a native module which is included in the Azure IoT Edge core runtime package.</span></span> <span data-ttu-id="0edd6-166">Meg kell adnia `args` a JSON-ban a fájl akkor is, ha `null`.</span><span class="sxs-lookup"><span data-stu-id="0edd6-166">You should always include `args` in the JSON file even if it is `null`.</span></span>

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

<span data-ttu-id="0edd6-167">A konfigurációs végén kapcsolatot létesítünk a kapcsolatokat.</span><span class="sxs-lookup"><span data-stu-id="0edd6-167">At the end of the configuration, we establish the connections.</span></span> <span data-ttu-id="0edd6-168">Minden kapcsolat van kifejezve `source` és `sink`.</span><span class="sxs-lookup"><span data-stu-id="0edd6-168">Each connection is expressed by `source` and `sink`.</span></span> <span data-ttu-id="0edd6-169">Kell mindkét hivatkozó egy előre definiált modul.</span><span class="sxs-lookup"><span data-stu-id="0edd6-169">They should both reference a pre-defined module.</span></span> <span data-ttu-id="0edd6-170">A kimeneti üzenetét `source` modul a rendszer továbbítja a bemeneti `sink` modul.</span><span class="sxs-lookup"><span data-stu-id="0edd6-170">The output message of `source` module will be forwarded to the input of `sink` module.</span></span>

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

## <a name="running-the-modules"></a><span data-ttu-id="0edd6-171">A modulok fut</span><span class="sxs-lookup"><span data-stu-id="0edd6-171">Running the modules</span></span>

<span data-ttu-id="0edd6-172">Használjon `mvn package` mindent történő létrehozásához a `target/` mappát.</span><span class="sxs-lookup"><span data-stu-id="0edd6-172">Use `mvn package` to build everything into the `target/` folder.</span></span> <span data-ttu-id="0edd6-173">`mvn clean package`össze is ajánlott.</span><span class="sxs-lookup"><span data-stu-id="0edd6-173">`mvn clean package` is also recommended for a clean build.</span></span>

<span data-ttu-id="0edd6-174">Használjon `mvn exec:exec` futhatnak az Azure IoT él, és figyelnie kell-e az hőmérséklet és a Tulajdonságok nyomtatott rögzített alapján a konzolhoz.</span><span class="sxs-lookup"><span data-stu-id="0edd6-174">Use `mvn exec:exec` to run the Azure IoT Edge and you should observe that the temperature data and all the properties are printed to the console at a fixed rate.</span></span>

<span data-ttu-id="0edd6-175">Ha azt szeretné, az alkalmazás befejezéséhez nyomja le az `<Enter>` kulcs.</span><span class="sxs-lookup"><span data-stu-id="0edd6-175">If you want to terminate the application, press `<Enter>` key.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="0edd6-176">Nem ajánlott a Ctrl + C használatával az IoT-peremhálózati átjáró alkalmazás befejezéséhez.</span><span class="sxs-lookup"><span data-stu-id="0edd6-176">It is not recommended to use Ctrl + C to terminate the IoT Edge gateway application.</span></span> <span data-ttu-id="0edd6-177">Mivel ez a folyamat rendellenesen eredményezheti.</span><span class="sxs-lookup"><span data-stu-id="0edd6-177">As this may cause the process to terminate abnormally.</span></span>

