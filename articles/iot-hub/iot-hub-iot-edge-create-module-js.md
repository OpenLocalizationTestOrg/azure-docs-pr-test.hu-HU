---
title: "Hozzon létre egy Azure IoT peremhálózati modul Node.js |} Microsoft Docs"
description: "Ez az oktatóanyag bővíthető adatok BLA konverter moduljának használatával. a legújabb Azure IoT peremhálózati NPM-csomagok és Yeoman írásával generátor."
services: iot-hub
author: sushi
manager: timlt
ms.service: iot-hub
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: js
ms.topic: article
ms.date: 06/28/2017
ms.author: sushi
ms.openlocfilehash: ba466f47e157d805600c41fa3d84ed5a0363969c
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/03/2017
---
# <a name="create-an-azure-iot-edge-module-with-nodejs"></a><span data-ttu-id="fc5ae-103">Hozzon létre egy Azure IoT peremhálózati modul Node.js</span><span class="sxs-lookup"><span data-stu-id="fc5ae-103">Create an Azure IoT Edge Module with Node.js</span></span>

<span data-ttu-id="fc5ae-104">Ez az oktatóanyag egy modul létrehozása az Azure IoT szegélyt JS bővíthető.</span><span class="sxs-lookup"><span data-stu-id="fc5ae-104">This tutorial showcases how to create a module for Azure IoT Edge in JS.</span></span>

<span data-ttu-id="fc5ae-105">Az oktatóanyag azt bízná környezetben való telepítés és írásával egy [Gedélyezése](https://en.wikipedia.org/wiki/Bluetooth_Low_Energy) adatok konverter moduljának használatával. a legújabb Azure IoT peremhálózati NPM-csomagokat.</span><span class="sxs-lookup"><span data-stu-id="fc5ae-105">In this tutorial, we walk through environment setup and how to write a [BLE](https://en.wikipedia.org/wiki/Bluetooth_Low_Energy) data converter module using the latest Azure IoT Edge NPM packages.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="fc5ae-106">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="fc5ae-106">Prerequisites</span></span>

<span data-ttu-id="fc5ae-107">Ebben a szakaszban, állítsa be a IoT peremhálózati modul fejlesztési környezetet.</span><span class="sxs-lookup"><span data-stu-id="fc5ae-107">In this section, you set up your environment for IoT Edge module development.</span></span> <span data-ttu-id="fc5ae-108">Ez egyaránt vonatkozik *64 bites Windows* és *(Ubuntu 14 +) 64 bites Linux* operációs rendszerek.</span><span class="sxs-lookup"><span data-stu-id="fc5ae-108">It applies to both *64-bit Windows* and *64-bit Linux (Ubuntu 14+)* operating systems.</span></span>

<span data-ttu-id="fc5ae-109">A következő szoftvereket is szükséges:</span><span class="sxs-lookup"><span data-stu-id="fc5ae-109">The following software is required:</span></span>
* <span data-ttu-id="fc5ae-110">[Git ügyfél](https://git-scm.com/downloads).</span><span class="sxs-lookup"><span data-stu-id="fc5ae-110">[Git Client](https://git-scm.com/downloads).</span></span>
* <span data-ttu-id="fc5ae-111">[Csomópont LTS](https://nodejs.org).</span><span class="sxs-lookup"><span data-stu-id="fc5ae-111">[Node LTS](https://nodejs.org).</span></span>
* <span data-ttu-id="fc5ae-112">`npm install -g yo`.</span><span class="sxs-lookup"><span data-stu-id="fc5ae-112">`npm install -g yo`.</span></span>
* `npm install -g generator-az-iot-gw-module`

## <a name="architecture"></a><span data-ttu-id="fc5ae-113">Architektúra</span><span class="sxs-lookup"><span data-stu-id="fc5ae-113">Architecture</span></span>

<span data-ttu-id="fc5ae-114">Az Azure IoT peremhálózati platform fokozottan elfogadja a [Von Neumann architektúra](https://en.wikipedia.org/wiki/Von_Neumann_architecture).</span><span class="sxs-lookup"><span data-stu-id="fc5ae-114">The Azure IoT Edge platform heavily adopts the [Von Neumann architecture](https://en.wikipedia.org/wiki/Von_Neumann_architecture).</span></span> <span data-ttu-id="fc5ae-115">Ami azt jelenti, hogy a teljes Azure IoT peremhálózati architektúra egy rendszer, amely feldolgozza a bemeneti és a kimenetet; és győződjön meg arról, hogy minden egyes modul is egy nagyon kicsi bemeneti / kimeneti alrendszer.</span><span class="sxs-lookup"><span data-stu-id="fc5ae-115">Which means that the entire Azure IoT Edge architecture is a system that processes input and produces output; and that each individual module is also a tiny input-output subsystem.</span></span> <span data-ttu-id="fc5ae-116">Ebben az oktatóanyagban a következő két modulok bemutatása után:</span><span class="sxs-lookup"><span data-stu-id="fc5ae-116">In this tutorial, we introduce the following two modules:</span></span>

1. <span data-ttu-id="fc5ae-117">A modul, amely fogad egy szimulált [Gedélyezése](https://en.wikipedia.org/wiki/Bluetooth_Low_Energy) jelezze, és konvertálja azt egy formázott [JSON](https://en.wikipedia.org/wiki/JSON) üzenet.</span><span class="sxs-lookup"><span data-stu-id="fc5ae-117">A module that receives a simulated [BLE](https://en.wikipedia.org/wiki/Bluetooth_Low_Energy) signal and converts it into a formatted [JSON](https://en.wikipedia.org/wiki/JSON) message.</span></span>
2. <span data-ttu-id="fc5ae-118">A modul, amely a fogadott kinyomtatja [JSON](https://en.wikipedia.org/wiki/JSON) üzenet.</span><span class="sxs-lookup"><span data-stu-id="fc5ae-118">A module that prints the received [JSON](https://en.wikipedia.org/wiki/JSON) message.</span></span>

<span data-ttu-id="fc5ae-119">Az alábbi képen a tipikus végpontok közötti adatfolyam ebben a projektben jeleníti meg:</span><span class="sxs-lookup"><span data-stu-id="fc5ae-119">The following image displays the typical end to end dataflow for this project:</span></span>

<span data-ttu-id="fc5ae-120">![Három modulok között Adatfolyamblokk](media/iot-hub-iot-edge-create-module/dataflow.png "bemenet: szimulált BLA modul; Processzor: Konverter modul; Kimenete: Nyomtató modul")</span><span class="sxs-lookup"><span data-stu-id="fc5ae-120">![Dataflow between three modules](media/iot-hub-iot-edge-create-module/dataflow.png "Input: Simulated BLE Module; Processor: Converter Module; Output: Printer Module")</span></span>

## <a name="set-up-the-environment"></a><span data-ttu-id="fc5ae-121">A környezet beállítása</span><span class="sxs-lookup"><span data-stu-id="fc5ae-121">Set up the environment</span></span>
<span data-ttu-id="fc5ae-122">Az alábbiakban azt mutatja be gyorsan környezet beállítása az első táblázat konverter modul JS írni elindításához.</span><span class="sxs-lookup"><span data-stu-id="fc5ae-122">Below we show you how to quickly set up environment to start to write your first BLE converter module with JS.</span></span>

### <a name="create-module-project"></a><span data-ttu-id="fc5ae-123">A modul-projekt létrehozása</span><span class="sxs-lookup"><span data-stu-id="fc5ae-123">Create module project</span></span>
1. <span data-ttu-id="fc5ae-124">Nyisson meg egy parancssori ablakot, futtassa `yo az-iot-gw-module`.</span><span class="sxs-lookup"><span data-stu-id="fc5ae-124">Open a command-line window, run `yo az-iot-gw-module`.</span></span>
2. <span data-ttu-id="fc5ae-125">Kövesse a képernyőn, a modul projekt az inicializálás befejezéséhez.</span><span class="sxs-lookup"><span data-stu-id="fc5ae-125">Follow the steps on the screen to finish the initialization of your module project.</span></span>

### <a name="project-structure"></a><span data-ttu-id="fc5ae-126">Projektstruktúra</span><span class="sxs-lookup"><span data-stu-id="fc5ae-126">Project structure</span></span>
<span data-ttu-id="fc5ae-127">Egy JS modul projektet a következő összetevőkből áll:</span><span class="sxs-lookup"><span data-stu-id="fc5ae-127">A JS module project consists of the following components:</span></span>

<span data-ttu-id="fc5ae-128">`modules`-A testreszabott JS modul forrásfájlokat.</span><span class="sxs-lookup"><span data-stu-id="fc5ae-128">`modules` - The customized JS module source files.</span></span> <span data-ttu-id="fc5ae-129">Cserélje le az alapértelmezett `sensor.js` és `printer.js` saját modul fájlokkal.</span><span class="sxs-lookup"><span data-stu-id="fc5ae-129">Replace the default `sensor.js` and `printer.js` with your own module files.</span></span>

<span data-ttu-id="fc5ae-130">`app.js`-Bejegyzés fájl a peremhálózati példány elindítása.</span><span class="sxs-lookup"><span data-stu-id="fc5ae-130">`app.js` - The entry file to start the Edge instance.</span></span>

<span data-ttu-id="fc5ae-131">`gw.config.json`-A konfigurációs fájl testreszabása a modulokat Edge által betölteni.</span><span class="sxs-lookup"><span data-stu-id="fc5ae-131">`gw.config.json` - The configuration file to customize the modules to be loaded by Edge.</span></span>

<span data-ttu-id="fc5ae-132">`package.json`-A metaadat-információi modul projekt.</span><span class="sxs-lookup"><span data-stu-id="fc5ae-132">`package.json` - The metadata information for module project.</span></span>

<span data-ttu-id="fc5ae-133">`README.md`-A modul projekt alapvető dokumentációját.</span><span class="sxs-lookup"><span data-stu-id="fc5ae-133">`README.md` - The basic documentation for module project.</span></span>


### <a name="package-file"></a><span data-ttu-id="fc5ae-134">A csomagfájl</span><span class="sxs-lookup"><span data-stu-id="fc5ae-134">Package file</span></span>

<span data-ttu-id="fc5ae-135">Ez `package.json` deklarál, amely tartalmazza a név, verzió, bejegyzést, parancsfájlok, futásidejű és fejlesztési függőségek modul projekthez szükséges minden metaadat-információkat.</span><span class="sxs-lookup"><span data-stu-id="fc5ae-135">This `package.json` declares all the metadata information needed by a module project that includes the name, version, entry, scripts, runtime, and development dependencies.</span></span>

<span data-ttu-id="fc5ae-136">Következő kódrészletet BLA konverter mintaprojektet konfigurálása jeleníti meg.</span><span class="sxs-lookup"><span data-stu-id="fc5ae-136">Following code snippet shows how to configure for BLE converter sample project.</span></span>
```json
{
  "name": "converter",
  "version": "1.0.0",
  "description": "BLE data converter sample for Azure IoT Edge.",
  "repository": {
    "type": "git",
    "url": "https://github.com/Azure-Samples/iot-edge-samples"
  },
  "main": "app.js",
  "scripts": {
    "start": "node app.js"
  },
  "author": "Microsoft Corporation",
  "license": "MIT",
  "dependencies": {
  },
  "devDependencies": {
    "azure-iot-gateway": "~1.1.3"
  }
}
```


### <a name="entry-file"></a><span data-ttu-id="fc5ae-137">Bejegyzés fájl</span><span class="sxs-lookup"><span data-stu-id="fc5ae-137">Entry file</span></span>
<span data-ttu-id="fc5ae-138">A `app.js` határozza meg a inicializálni az edge-példány.</span><span class="sxs-lookup"><span data-stu-id="fc5ae-138">The `app.js` defines the way to initialize the edge instance.</span></span> <span data-ttu-id="fc5ae-139">Itt végezheti el semmilyen módosítást nem szükséges.</span><span class="sxs-lookup"><span data-stu-id="fc5ae-139">Here we don't need to make any change.</span></span>

```javascript
(function() {
  'use strict';

  const Gateway = require('azure-iot-gateway');
  let config_path = './gw.config.json';

  // node app.js
  if (process.argv.length < 2) {
    throw 'Calling pattern should be node app.js.';
  }

  const gw = new Gateway(config_path);
  gw.run();
})();
```

### <a name="interface-of-module"></a><span data-ttu-id="fc5ae-140">Felület modul</span><span class="sxs-lookup"><span data-stu-id="fc5ae-140">Interface of module</span></span>
<span data-ttu-id="fc5ae-141">Egy Azure IoT Edge-modul is kezelheti, amelynek a feladata, feldolgozónak: fogadja, feldolgozni azt és eredménye.</span><span class="sxs-lookup"><span data-stu-id="fc5ae-141">You can treat an Azure IoT Edge module as a data processor whose job is to: receive input, process it, and produce output.</span></span>

<span data-ttu-id="fc5ae-142">Lehet, hogy a bemeneti hardverek (például egy mozgásérzékelő) adatait, az egyéb modulok vagy bármely más (például egy időzítő rendszeresen generálja véletlenszerűen) üzenet.</span><span class="sxs-lookup"><span data-stu-id="fc5ae-142">The input might be data from hardware (like a motion detector), a message from other modules, or anything else (like a random number generated periodically by a timer).</span></span>

<span data-ttu-id="fc5ae-143">A kimenet a bemeneti hasonló, hogy elindíthatja hardver viselkedés (például a villogó LED), egy üzenetet, amely más modulok vagy bármely más (például a konzol nyomtatás).</span><span class="sxs-lookup"><span data-stu-id="fc5ae-143">The output is similar to the input, it could trigger hardware behavior (like the blinking LED), a message to other modules, or anything else (like printing to the console).</span></span>

<span data-ttu-id="fc5ae-144">Modulok kommunikálnak egymással használatával `message` objektum.</span><span class="sxs-lookup"><span data-stu-id="fc5ae-144">Modules communicate with each other using `message` object.</span></span> <span data-ttu-id="fc5ae-145">A **tartalom** , egy `message` van, amely képes bármilyen típusú adatok, például egy bájttömböt.</span><span class="sxs-lookup"><span data-stu-id="fc5ae-145">The **content** of a `message` is a byte array that is capable of representing any kind of data you like.</span></span> <span data-ttu-id="fc5ae-146">**Tulajdonságok** is elérhetők a `message` és egyszerűen egy karakterlánc-karakterlánc leképezése.</span><span class="sxs-lookup"><span data-stu-id="fc5ae-146">**Properties** are also available in the `message` and are simply a string-to-string mapping.</span></span> <span data-ttu-id="fc5ae-147">Elképzelhető, hogy a **tulajdonságok** , a fejléceket a HTTP-kérelem és a fájl metaadatait.</span><span class="sxs-lookup"><span data-stu-id="fc5ae-147">You may think of **properties** as the headers in an HTTP request, or the metadata of a file.</span></span>

<span data-ttu-id="fc5ae-148">Egy Azure IoT peremhálózati modulja JS elkészítéséhez hozzon létre egy új module objektum, amely megvalósítja a szükséges módszereket kell `receive()`.</span><span class="sxs-lookup"><span data-stu-id="fc5ae-148">In order to develop an Azure IoT Edge module in JS, you need to create a new module object that implements the required methods `receive()`.</span></span> <span data-ttu-id="fc5ae-149">Ezen a ponton is választhatja, hogy a nem kötelező végrehajtásához `create()` vagy `start()`, vagy `destroy()` módszerek is.</span><span class="sxs-lookup"><span data-stu-id="fc5ae-149">At this point, you may also choose to implement the optional `create()` or `start()`, or `destroy()` methods as well.</span></span> <span data-ttu-id="fc5ae-150">A következő kódrészletet a állványok JS modul objektum jeleníti meg.</span><span class="sxs-lookup"><span data-stu-id="fc5ae-150">The following code snippet shows you the scaffolding of JS module object.</span></span>

```javascript
'use strict';

module.exports = {
  broker: null,
  configuration: null,

  create: function (broker, configuration) {
    // Default implementation.
    this.broker = broker;
    this.configuration = configuration;

    return true;
  },

  start: function () {
    // Produce
  },

  receive: function (message) {
    // Consume
  },

  destroy: function () {
  }
};
```

### <a name="converter-module"></a><span data-ttu-id="fc5ae-151">Konverter modul</span><span class="sxs-lookup"><span data-stu-id="fc5ae-151">Converter module</span></span>
| <span data-ttu-id="fc5ae-152">Input (Bemenet)</span><span class="sxs-lookup"><span data-stu-id="fc5ae-152">Input</span></span>                    | <span data-ttu-id="fc5ae-153">Processzor</span><span class="sxs-lookup"><span data-stu-id="fc5ae-153">Processor</span></span>                              | <span data-ttu-id="fc5ae-154">Kimenet</span><span class="sxs-lookup"><span data-stu-id="fc5ae-154">Output</span></span>                 | <span data-ttu-id="fc5ae-155">Forrásfájl</span><span class="sxs-lookup"><span data-stu-id="fc5ae-155">Source File</span></span>            |
| ------------------------ | -------------------------------------- | ---------------------- | ---------------------- |
| <span data-ttu-id="fc5ae-156">Hőmérséklet-adatok üzenet</span><span class="sxs-lookup"><span data-stu-id="fc5ae-156">Temperature data message</span></span> | <span data-ttu-id="fc5ae-157">Elemzése és létrehozni egy új JSON-üzenet</span><span class="sxs-lookup"><span data-stu-id="fc5ae-157">Parse and construct a new JSON message</span></span> | <span data-ttu-id="fc5ae-158">Struktúra JSON üzenet</span><span class="sxs-lookup"><span data-stu-id="fc5ae-158">Structure JSON message</span></span> | `converter.js` |

<span data-ttu-id="fc5ae-159">Ez a modul az egy tipikus Azure IoT peremhálózati modul.</span><span class="sxs-lookup"><span data-stu-id="fc5ae-159">This module is a typical Azure IoT Edge module.</span></span> <span data-ttu-id="fc5ae-160">Hőmérséklet-üzenetek származó adatok elfogad (hardveres modult, vagy ilyen esetben a szimulált BLA modul); és majd normalizálja az hőmérséklet üzenet (beleértve a Hozzáfűzés az Üzenetazonosító, hogy igazolnia kell a hőmérséklet figyelmeztetést, és így tovább tulajdonságának beállítása) strukturált JSON-üzenethez.</span><span class="sxs-lookup"><span data-stu-id="fc5ae-160">It accepts temperature messages from other modules (a hardware module, or in this case our simulated BLE module); and then normalizes the temperature message in to a structured JSON message (including appending the message ID, setting the property of whether we need to trigger the temperature alert, and so on).</span></span>

```javascript
receive: function (message) {
  // Initialize the messageCount in global object at first time.
  if (!global.messageCount) {
    global.messageCount = 0;
  }

  // Read the content and properties objects from message.
  let rawContent = JSON.parse(Buffer.from(message.content).toString('utf8'));
  let rawProperties = message.properties;

  // Generate new properties object.
  let newProperties = {
    source: rawProperties.source,
    macAddress: rawProperties.macAddress,
    temperatureAlert: rawContent.temperature > 30 ? 'true' : 'false'
  };

  // Generate new content object.
  let newContent = {
    deviceId: 'Intel NUC Gateway',
    messageId: ++global.messageCount,
    temperature: rawContent.temperature
  };

  // Publish the new message to broker.
  this.broker.publish(
    {
      properties: newProperties,
      content: new Uint8Array(Buffer.from(JSON.stringify(newContent), 'utf8'))
    }
  );
},
```

### <a name="printer-module"></a><span data-ttu-id="fc5ae-161">Nyomtató modul</span><span class="sxs-lookup"><span data-stu-id="fc5ae-161">Printer module</span></span>
| <span data-ttu-id="fc5ae-162">Input (Bemenet)</span><span class="sxs-lookup"><span data-stu-id="fc5ae-162">Input</span></span>                          | <span data-ttu-id="fc5ae-163">Processzor</span><span class="sxs-lookup"><span data-stu-id="fc5ae-163">Processor</span></span> | <span data-ttu-id="fc5ae-164">Kimenet</span><span class="sxs-lookup"><span data-stu-id="fc5ae-164">Output</span></span>                     | <span data-ttu-id="fc5ae-165">Forrásfájl</span><span class="sxs-lookup"><span data-stu-id="fc5ae-165">Source File</span></span>          |
| ------------------------------ | --------- | -------------------------- | -------------------- |
| <span data-ttu-id="fc5ae-166">Bármely más modulok érkezett üzenetet:</span><span class="sxs-lookup"><span data-stu-id="fc5ae-166">Any message from other modules</span></span> | <span data-ttu-id="fc5ae-167">N/A</span><span class="sxs-lookup"><span data-stu-id="fc5ae-167">N/A</span></span>       | <span data-ttu-id="fc5ae-168">Az üzenet naplózása konzolhoz</span><span class="sxs-lookup"><span data-stu-id="fc5ae-168">Log the message to console</span></span> | `printer.js` |

<span data-ttu-id="fc5ae-169">A modul az egyszerű, értetődő, amely a fogadott üzenetek (tulajdonság, tartalom) kiírja a Terminálszolgáltatások ablakra.</span><span class="sxs-lookup"><span data-stu-id="fc5ae-169">This module is simple, self-explanatory, which outputs the received messages(property, content) to the terminal window.</span></span>

```javascript
receive: function (message) {
  let properties = JSON.stringify(message.properties);
  let content = Buffer.from(message.content).toString('utf8');

  console.log(`printer.receive.properties - ${properties}`);
  console.log(`printer.receive.content - ${content}\n`);
}
```

### <a name="configuration"></a><span data-ttu-id="fc5ae-170">Konfiguráció</span><span class="sxs-lookup"><span data-stu-id="fc5ae-170">Configuration</span></span>
<span data-ttu-id="fc5ae-171">Az utolsó lépés a modulok futtatása előtt a konfigurálása az Azure IoT él és a kapcsolatot létrehozni a modulok között.</span><span class="sxs-lookup"><span data-stu-id="fc5ae-171">The final step before running the modules is to configure the Azure IoT Edge and to establish the connections between modules.</span></span>

<span data-ttu-id="fc5ae-172">Először kell deklarálnia az `node` betöltő (óta Azure IoT peremhálózati támogatja betöltők különböző nyelvű) sikerült hivatkozhat, amely a `name` ezt követően a szakaszokban.</span><span class="sxs-lookup"><span data-stu-id="fc5ae-172">First we need to declare our `node` loader (since Azure IoT Edge supports loaders of different languages) which could be referenced by its `name` in the sections afterward.</span></span>

```json
"loaders": [
  {
    "type": "node",
    "name": "node"
  }
]
```

<span data-ttu-id="fc5ae-173">A betöltők rendelkezik deklaráltuk, miután is, valamint a modulok deklarálnia kell.</span><span class="sxs-lookup"><span data-stu-id="fc5ae-173">Once we have declared our loaders, we also need to declare our modules as well.</span></span> <span data-ttu-id="fc5ae-174">Hasonló a betöltők deklaráló, azokat is hivatkozhat a `name` attribútum.</span><span class="sxs-lookup"><span data-stu-id="fc5ae-174">Similar to declaring the loaders, they can also be referenced by their `name` attribute.</span></span> <span data-ttu-id="fc5ae-175">Egy modul deklaráló, azt kell adnia a betöltő azt kell használnia (amely az legyen előtt meghatározott) és a belépési pont (a normalizált osztály neve a modul kellene lennie) minden modulhoz.</span><span class="sxs-lookup"><span data-stu-id="fc5ae-175">When declaring a module, we need to specify the loader it should use (which should be the one we defined before) and the entry-point (should be the normalized class name of our module) for each module.</span></span> <span data-ttu-id="fc5ae-176">A `simulated_device` modul az egy natív modul, amely az Azure IoT peremhálózati core runtime csomagban található.</span><span class="sxs-lookup"><span data-stu-id="fc5ae-176">The `simulated_device` module is a native module that is included in the Azure IoT Edge core runtime package.</span></span> <span data-ttu-id="fc5ae-177">Tartalmaznak `args` a JSON-ban a fájl akkor is, ha `null`.</span><span class="sxs-lookup"><span data-stu-id="fc5ae-177">Include `args` in the JSON file even if it is `null`.</span></span>

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
      "name": "node",
      "entrypoint": {
        "main.path": "modules/converter.js"
      }
    },
    "args": null
  },
  {
    "name": "printer",
    "loader": {
      "name": "node",
      "entrypoint": {
        "main.path": "modules/printer.js"
      }
    },
    "args": null
  }
]
```

<span data-ttu-id="fc5ae-178">A konfigurációs végén kapcsolatot létesítünk a kapcsolatokat.</span><span class="sxs-lookup"><span data-stu-id="fc5ae-178">At the end of the configuration, we establish the connections.</span></span> <span data-ttu-id="fc5ae-179">Minden kapcsolat van kifejezve `source` és `sink`.</span><span class="sxs-lookup"><span data-stu-id="fc5ae-179">Each connection is expressed by `source` and `sink`.</span></span> <span data-ttu-id="fc5ae-180">Kell mindkét hivatkozó egy előre definiált modul.</span><span class="sxs-lookup"><span data-stu-id="fc5ae-180">They should both reference a pre-defined module.</span></span> <span data-ttu-id="fc5ae-181">A kimeneti üzenetét `source` modul továbbíthatja a rendszer a bemeneti `sink` modul.</span><span class="sxs-lookup"><span data-stu-id="fc5ae-181">The output message of `source` module is forwarded to the input of `sink` module.</span></span>

```json
"links": [
  {
    "source": "simulated_device",
    "sink": "converter"
  },
  {
    "source": "converter",
    "sink": "printer"
  }
]
```

## <a name="running-the-modules"></a><span data-ttu-id="fc5ae-182">A modulok fut</span><span class="sxs-lookup"><span data-stu-id="fc5ae-182">Running the modules</span></span>
1. `npm install`
2. `npm start`

<span data-ttu-id="fc5ae-183">Ha azt szeretné, az alkalmazás befejezéséhez nyomja le az `<Enter>` kulcs.</span><span class="sxs-lookup"><span data-stu-id="fc5ae-183">If you want to terminate the application, press `<Enter>` key.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="fc5ae-184">Nem ajánlott a Ctrl + C használatával az IoT-Edge alkalmazás befejezéséhez.</span><span class="sxs-lookup"><span data-stu-id="fc5ae-184">It is not recommended to use Ctrl + C to terminate the IoT Edge application.</span></span> <span data-ttu-id="fc5ae-185">Mivel így a folyamat rendellenesen eredményezheti.</span><span class="sxs-lookup"><span data-stu-id="fc5ae-185">As this way may cause the process to terminate abnormally.</span></span>
