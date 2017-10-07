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
# <a name="create-an-azure-iot-edge-module-with-java"></a>Hozzon létre egy Azure IoT peremhálózati modul Java

Ez az oktatóanyag bővíthető, hogyan egy előfordulhat, hogy olyan modul létrehozása az Azure IoT peremhálózati Java nyelven.

Ebben az oktatóanyagban végigvezetjük azon környezetet, és hogyan toowrite egy [Gedélyezése](https://en.wikipedia.org/wiki/Bluetooth_Low_Energy) adatok konverter modul hello legújabb Azure IoT peremhálózati Maven-csomagok használata.

## <a name="prerequisites"></a>Előfeltételek

Ebben a szakaszban a IoT peremhálózati modul fejlesztői környezetben fog beállítani. Ez a kategória tooboth *64 bites Windows* és *64 bites Linux (8 Ubuntu/Debian)* operációs rendszerek.

a következő szoftver hello szükség:

* [Git ügyfél](https://git-scm.com/downloads).
* [**x64** JDK](http://www.oracle.com/technetwork/java/javase/downloads/jdk8-downloads-2133151.html).
* [Maven](https://maven.apache.org/install.html).

Nyisson meg egy parancssori terminál ablakot, és a Klónozás hello a következő tárházat:

1. `git clone https://github.com/Azure-Samples/iot-edge-samples.git`.
2. `cd iot-edge-samples/java/simulated_ble`

## <a name="overall-architecture"></a>Általános architektúrája

hello Azure IoT peremhálózati platform fokozottan fogad hello [Von Neumann architektúra](https://en.wikipedia.org/wiki/Von_Neumann_architecture). Ez azt jelenti, hogy hello teljes Azure IoT peremhálózati architektúra egy rendszer, amely feldolgozza a bemeneti és a kimenetet; és győződjön meg arról, hogy minden egyes modul is egy nagyon kicsi bemeneti / kimeneti alrendszer. Az oktatóanyag azt vezeti be a következő két modulok hello:

1. A modul, amely fogadja a szimulált [Gedélyezése](https://en.wikipedia.org/wiki/Bluetooth_Low_Energy) jelezze, és konvertálja azt egy formázott [JSON](https://en.wikipedia.org/wiki/JSON) üzenet.
2. A modult, amely a fogadott hello kinyomtatja [JSON](https://en.wikipedia.org/wiki/JSON) üzenet.

hello példánycsoportokat jeleníti meg a projekt hello tipikus végpont adatfolyamban:

![Három modulok között Adatfolyamblokk](media/iot-hub-iot-edge-create-module/dataflow.png "bemenet: szimulált BLA modul; Processzor: Konverter modul; Kimenete: Nyomtató modul")

## <a name="understanding-hello-code"></a>Hello kód ismertetése

### <a name="maven-project-structure"></a>Maven project struktúra

Mivel az Azure IoT peremhálózati csomagok Maven alapulnak, igazolnia kell toocreate egy tipikus Maven project struktúra, amely tartalmazza a `pom.xml` fájlt.

hello POM örököl hello `com.microsoft.azure.gateway.gateway-module-base` csomagot, amely az összes hello futásidejű bináris fájljait, hello átjáró konfigurációs fájl elérési útját és hello végrehajtási viselkedésének modul projekthez szükséges hello függőséggel deklarál. Ez menti, rengeteg időt hello kell toowrite megszüntetéséhez és sornyi kód több száz újraírási többször.

Tooupdate a pom.xml fájlt is hello szükséges függőségek/beépülő modulok és a modul által használt, ahogy az a következő kódrészletet hello hello konfigurációs fájl toobe hello nevét deklarálni kell.

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

### <a name="basic-understanding-of-an-azure-iot-edge-module"></a>Egy Azure IoT peremhálózati modul alapvető ismeretekkel

Egy Azure IoT Edge-modul is kezelheti, amelynek a feladata, feldolgozónak: fogadja, feldolgozni azt és eredménye.

hello bemeneti hardverek (például egy mozgásérzékelő) adatait, egy üzenet más modulok, vagy bármi más (például egy időzítő rendszeresen generálja véletlenszerűen) lehet.

hello kimeneti hasonló toohello bemeneti, hogy elindíthatja hardver viselkedés (például LED villogó hello), egy üzenet tooother modulok vagy bármi más (például a nyomtatási toohello konzol).

Modulok kommunikálnak egymással használatával `com.microsoft.azure.gateway.messaging.Message` osztály. Hello **tartalom** , egy `Message` bájt tömb, amely bármilyen típusú adatok, például képes. **Tulajdonságok** is rendelkezésre állnak a hello `Message` és egyszerűen egy karakterlánc-karakterlánc leképezése. Elképzelhető, hogy a **tulajdonságok** hello fejlécként HTTP-kérelem vagy hello metaadat-fájl.

Rendelés toodevelop egy Azure IoT Edge-modul a Java, egy új modul osztály, amely örökli toocreate kell `com.microsoft.azure.gateway.core.GatewayModule` és megvalósításához szükséges hello absztrakt metódusok `receive()` és `destroy()`. Ezen a ponton is választhatja, hogy tooimplement hello választható `start()` vagy `create()` módszerek is. a következő kódrészletet hello bemutatja, hogyan tooget el az Azure IoT peremhálózati modul szerzői.

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

### <a name="converter-module"></a>Konverter modul

| Input (Bemenet)                    | Processzor                              | Kimenet                 | Forrásfájl            |
| ------------------------ | -------------------------------------- | ---------------------- | ---------------------- |
| Hőmérséklet-adatok üzenet | Elemzése és létrehozni egy új JSON-üzenet | Struktúra JSON üzenet | `ConverterModule.java` |

Ez a modul az egy tipikus Azure IoT peremhálózati modul. Hőmérséklet-üzenetek származó adatok elfogad (hardveres modult, vagy ilyen esetben a szimulált BLA modul); és majd normalizálja hőmérséklet üdvözlőüzenetére strukturált tooa JSON üzenet (beleértve a fűznek hello Üzenetazonosítója hello tulajdonságának e igazolnia kell tootrigger hello hőmérséklet riasztást, és így tovább).

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

### <a name="printer-module"></a>Nyomtató modul

| Input (Bemenet)                          | Processzor | Kimenet                     | Forrásfájl          |
| ------------------------------ | --------- | -------------------------- | -------------------- |
| Bármely más modulok érkezett üzenetet: | N/A       | Hello üzenet tooconsole naplózása | `PrinterModule.java` |

Ez az egy egyszerű, értetődő, modult, amely hello fogadott üzenetek toohello terminálablakot kimenete.

```java
@Override
public void receive(Message message) {
  System.out.println(message.toString());
}
```

### <a name="azure-iot-edge-configuration"></a>Azure IoT peremhálózati konfigurálása

hello utolsó lépés előtt futó hello modulok tooconfigure hello Azure IoT peremhálózati és tooestablish hello kapcsolatok modulok között.

Először igazolnia kell toodeclare a Java betöltő (óta Azure IoT peremhálózati támogatja betöltők különböző nyelvű) sikerült hivatkozhat, amely a `name` hello szakaszok ezt követően.

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

Miután a betöltők rendelkezik deklaráltuk, azt is kell toodeclare, valamint a modulok. Hasonló toodeclaring hello betöltők, azokat is hivatkozhat a `name` attribútum. Ha egy modul deklaráló, igazolnia kell a toospecify hello betöltő azt kell használnia (amely előtt meghatározott egyik hello kellene lennie) és a belépési pont (kell lennie a modul hello normalizált osztály neve) minden modul hello. Hello `simulated_device` modul az hello Azure IoT peremhálózati core runtime csomag részét képező natív modul. Meg kell adnia `args` hello JSON fájl, akkor is, ha a `null`.

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

Hello konfigurációs hello végén a Microsoft hello-kapcsolatok létesítéséhez. Minden kapcsolat van kifejezve `source` és `sink`. Kell mindkét hivatkozó egy előre definiált modul. a kimeneti üdvözlőüzenetére `source` modul továbbítják toohello bemeneti `sink` modul.

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

## <a name="running-hello-modules"></a>Hello modulok fut

Használjon `mvn package` toobuild mindent be hello `target/` mappát. `mvn clean package`össze is ajánlott.

Használjon `mvn exec:exec` toorun hello Azure IoT él, és figyelnie kell-e, hogy hello hőmérséklet adatok és az összes hello tulajdonság-e a rögzített kulcs nyomtatott toohello konzol.

Ha azt szeretné, hogy tooterminate hello alkalmazás, nyomja meg az `<Enter>` kulcs.

> [!IMPORTANT]
> Nem ajánlott toouse Ctrl + C tooterminate hello IoT peremhálózati átjáró alkalmazás. Mivel ez hello folyamat tooterminate rendellenesen okozhatja.

