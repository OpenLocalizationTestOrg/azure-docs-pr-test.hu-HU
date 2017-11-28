---
title: "egy Azure IoT peremhálózati modul Node.js aaaCreate |} Microsoft Docs"
description: "Ez az oktatóanyag bővíthető hogyan egy BLA konverter modul használatával végzett toowrite hello-e a legújabb Azure IoT peremhálózati NPM csomagokat és Yeoman generátor."
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
ms.openlocfilehash: d3e696b5a310377ffb8e99998ff0714bf7c0bb41
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="create-an-azure-iot-edge-module-with-nodejs"></a><span data-ttu-id="d44e2-103">Hozzon létre egy Azure IoT peremhálózati modul Node.js</span><span class="sxs-lookup"><span data-stu-id="d44e2-103">Create an Azure IoT Edge Module with Node.js</span></span>

<span data-ttu-id="d44e2-104">Ez az oktatóanyag bővíthető hogyan toocreate egy modul a JS Azure IoT szegélyt.</span><span class="sxs-lookup"><span data-stu-id="d44e2-104">This tutorial showcases how toocreate a module for Azure IoT Edge in JS.</span></span>

<span data-ttu-id="d44e2-105">Az oktatóanyag azt bízná környezetet, és hogyan toowrite egy [Gedélyezése](https://en.wikipedia.org/wiki/Bluetooth_Low_Energy) adatok konverter modul hello legújabb Azure IoT peremhálózati NPM-csomagok használata.</span><span class="sxs-lookup"><span data-stu-id="d44e2-105">In this tutorial, we walk through environment setup and how toowrite a [BLE](https://en.wikipedia.org/wiki/Bluetooth_Low_Energy) data converter module using hello latest Azure IoT Edge NPM packages.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="d44e2-106">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="d44e2-106">Prerequisites</span></span>

<span data-ttu-id="d44e2-107">Ebben a szakaszban, állítsa be a IoT peremhálózati modul fejlesztési környezetet.</span><span class="sxs-lookup"><span data-stu-id="d44e2-107">In this section, you set up your environment for IoT Edge module development.</span></span> <span data-ttu-id="d44e2-108">Ez a kategória tooboth *64 bites Windows* és *64 bites Linux (Ubuntu 14 +)* operációs rendszerek.</span><span class="sxs-lookup"><span data-stu-id="d44e2-108">It applies tooboth *64-bit Windows* and *64-bit Linux (Ubuntu 14+)* operating systems.</span></span>

<span data-ttu-id="d44e2-109">a következő szoftver hello szükség:</span><span class="sxs-lookup"><span data-stu-id="d44e2-109">hello following software is required:</span></span>
* <span data-ttu-id="d44e2-110">[Git ügyfél](https://git-scm.com/downloads).</span><span class="sxs-lookup"><span data-stu-id="d44e2-110">[Git Client](https://git-scm.com/downloads).</span></span>
* <span data-ttu-id="d44e2-111">[Csomópont LTS](https://nodejs.org).</span><span class="sxs-lookup"><span data-stu-id="d44e2-111">[Node LTS](https://nodejs.org).</span></span>
* <span data-ttu-id="d44e2-112">`npm install -g yo`.</span><span class="sxs-lookup"><span data-stu-id="d44e2-112">`npm install -g yo`.</span></span>
* `npm install -g generator-az-iot-gw-module`

## <a name="architecture"></a><span data-ttu-id="d44e2-113">Architektúra</span><span class="sxs-lookup"><span data-stu-id="d44e2-113">Architecture</span></span>

<span data-ttu-id="d44e2-114">hello Azure IoT peremhálózati platform fokozottan fogad hello [Von Neumann architektúra](https://en.wikipedia.org/wiki/Von_Neumann_architecture).</span><span class="sxs-lookup"><span data-stu-id="d44e2-114">hello Azure IoT Edge platform heavily adopts hello [Von Neumann architecture](https://en.wikipedia.org/wiki/Von_Neumann_architecture).</span></span> <span data-ttu-id="d44e2-115">Ez azt jelenti, hogy hello teljes Azure IoT peremhálózati architektúra egy rendszer, amely feldolgozza a bemeneti és a kimenetet; és győződjön meg arról, hogy minden egyes modul is egy nagyon kicsi bemeneti / kimeneti alrendszer.</span><span class="sxs-lookup"><span data-stu-id="d44e2-115">Which means that hello entire Azure IoT Edge architecture is a system that processes input and produces output; and that each individual module is also a tiny input-output subsystem.</span></span> <span data-ttu-id="d44e2-116">Ebben az oktatóanyagban bemutatása után a következő két modulok hello:</span><span class="sxs-lookup"><span data-stu-id="d44e2-116">In this tutorial, we introduce hello following two modules:</span></span>

1. <span data-ttu-id="d44e2-117">A modul, amely fogad egy szimulált [Gedélyezése](https://en.wikipedia.org/wiki/Bluetooth_Low_Energy) jelezze, és konvertálja azt egy formázott [JSON](https://en.wikipedia.org/wiki/JSON) üzenet.</span><span class="sxs-lookup"><span data-stu-id="d44e2-117">A module that receives a simulated [BLE](https://en.wikipedia.org/wiki/Bluetooth_Low_Energy) signal and converts it into a formatted [JSON](https://en.wikipedia.org/wiki/JSON) message.</span></span>
2. <span data-ttu-id="d44e2-118">A modul, amely a fogadott hello kinyomtatja [JSON](https://en.wikipedia.org/wiki/JSON) üzenet.</span><span class="sxs-lookup"><span data-stu-id="d44e2-118">A module that prints hello received [JSON](https://en.wikipedia.org/wiki/JSON) message.</span></span>

<span data-ttu-id="d44e2-119">hello következő képet jeleníti meg hello tipikus end tooend adatfolyamblokk ebben a projektben:</span><span class="sxs-lookup"><span data-stu-id="d44e2-119">hello following image displays hello typical end tooend dataflow for this project:</span></span>

<span data-ttu-id="d44e2-120">![Három modulok között Adatfolyamblokk](media/iot-hub-iot-edge-create-module/dataflow.png "bemenet: szimulált BLA modul; Processzor: Konverter modul; Kimenete: Nyomtató modul")</span><span class="sxs-lookup"><span data-stu-id="d44e2-120">![Dataflow between three modules](media/iot-hub-iot-edge-create-module/dataflow.png "Input: Simulated BLE Module; Processor: Converter Module; Output: Printer Module")</span></span>

## <a name="set-up-hello-environment"></a><span data-ttu-id="d44e2-121">Hello környezet beállítása</span><span class="sxs-lookup"><span data-stu-id="d44e2-121">Set up hello environment</span></span>
<span data-ttu-id="d44e2-122">Az alábbiakban azt jelzik, hogy hogyan tooquickly beállítása környezet toostart toowrite JS az első táblázat konverter modul.</span><span class="sxs-lookup"><span data-stu-id="d44e2-122">Below we show you how tooquickly set up environment toostart toowrite your first BLE converter module with JS.</span></span>

### <a name="create-module-project"></a><span data-ttu-id="d44e2-123">A modul-projekt létrehozása</span><span class="sxs-lookup"><span data-stu-id="d44e2-123">Create module project</span></span>
1. <span data-ttu-id="d44e2-124">Nyisson meg egy parancssori ablakot, futtassa `yo az-iot-gw-module`.</span><span class="sxs-lookup"><span data-stu-id="d44e2-124">Open a command-line window, run `yo az-iot-gw-module`.</span></span>
2. <span data-ttu-id="d44e2-125">Hello lépésekkel hello képernyő toofinish hello inicializálásakor a modul projekt.</span><span class="sxs-lookup"><span data-stu-id="d44e2-125">Follow hello steps on hello screen toofinish hello initialization of your module project.</span></span>

### <a name="project-structure"></a><span data-ttu-id="d44e2-126">Projektstruktúra</span><span class="sxs-lookup"><span data-stu-id="d44e2-126">Project structure</span></span>
<span data-ttu-id="d44e2-127">A következő összetevők hello JS modul projekt foglalja magában:</span><span class="sxs-lookup"><span data-stu-id="d44e2-127">A JS module project consists of hello following components:</span></span>

<span data-ttu-id="d44e2-128">`modules`-hello testreszabott JS modul forrásfájlokat.</span><span class="sxs-lookup"><span data-stu-id="d44e2-128">`modules` - hello customized JS module source files.</span></span> <span data-ttu-id="d44e2-129">Cserélje le a hello alapértelmezett `sensor.js` és `printer.js` saját modul fájlokkal.</span><span class="sxs-lookup"><span data-stu-id="d44e2-129">Replace hello default `sensor.js` and `printer.js` with your own module files.</span></span>

<span data-ttu-id="d44e2-130">`app.js`-hello bejegyzés fájl toostart hello peremhálózati példány.</span><span class="sxs-lookup"><span data-stu-id="d44e2-130">`app.js` - hello entry file toostart hello Edge instance.</span></span>

<span data-ttu-id="d44e2-131">`gw.config.json`-Edge által betöltött hello konfigurációs fájl toocustomize hello modulok toobe.</span><span class="sxs-lookup"><span data-stu-id="d44e2-131">`gw.config.json` - hello configuration file toocustomize hello modules toobe loaded by Edge.</span></span>

<span data-ttu-id="d44e2-132">`package.json`-hello modul projekt metaadat-információit.</span><span class="sxs-lookup"><span data-stu-id="d44e2-132">`package.json` - hello metadata information for module project.</span></span>

<span data-ttu-id="d44e2-133">`README.md`-hello modul projekt alapvető dokumentációját.</span><span class="sxs-lookup"><span data-stu-id="d44e2-133">`README.md` - hello basic documentation for module project.</span></span>


### <a name="package-file"></a><span data-ttu-id="d44e2-134">A csomagfájl</span><span class="sxs-lookup"><span data-stu-id="d44e2-134">Package file</span></span>

<span data-ttu-id="d44e2-135">Ez `package.json` deklarál, amely tartalmazza az hello név, verzió, bejegyzést, parancsfájlok, futásidejű és fejlesztési függőségek modul projekthez szükséges összes hello metaadatait.</span><span class="sxs-lookup"><span data-stu-id="d44e2-135">This `package.json` declares all hello metadata information needed by a module project that includes hello name, version, entry, scripts, runtime, and development dependencies.</span></span>

<span data-ttu-id="d44e2-136">Következő kódrészletben látható kód hogyan BLA konverter tooconfigure mintát a projektet.</span><span class="sxs-lookup"><span data-stu-id="d44e2-136">Following code snippet shows how tooconfigure for BLE converter sample project.</span></span>
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


### <a name="entry-file"></a><span data-ttu-id="d44e2-137">Bejegyzés fájl</span><span class="sxs-lookup"><span data-stu-id="d44e2-137">Entry file</span></span>
<span data-ttu-id="d44e2-138">Hello `app.js` hello módon tooinitialize hello peremhálózati példány meghatározása.</span><span class="sxs-lookup"><span data-stu-id="d44e2-138">hello `app.js` defines hello way tooinitialize hello edge instance.</span></span> <span data-ttu-id="d44e2-139">Itt nem szükséges toomake bármi is módosul.</span><span class="sxs-lookup"><span data-stu-id="d44e2-139">Here we don't need toomake any change.</span></span>

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

### <a name="interface-of-module"></a><span data-ttu-id="d44e2-140">Felület modul</span><span class="sxs-lookup"><span data-stu-id="d44e2-140">Interface of module</span></span>
<span data-ttu-id="d44e2-141">Egy Azure IoT Edge-modul is kezelheti, amelynek a feladata, feldolgozónak: fogadja, feldolgozni azt és eredménye.</span><span class="sxs-lookup"><span data-stu-id="d44e2-141">You can treat an Azure IoT Edge module as a data processor whose job is to: receive input, process it, and produce output.</span></span>

<span data-ttu-id="d44e2-142">hello bemeneti hardverek (például egy mozgásérzékelő) adatait, egy üzenet más modulok, vagy bármi más (például egy időzítő rendszeresen generálja véletlenszerűen) lehet.</span><span class="sxs-lookup"><span data-stu-id="d44e2-142">hello input might be data from hardware (like a motion detector), a message from other modules, or anything else (like a random number generated periodically by a timer).</span></span>

<span data-ttu-id="d44e2-143">hello kimeneti hasonló toohello bemeneti, hogy elindíthatja hardver viselkedés (például LED villogó hello), egy üzenet tooother modulok vagy bármi más (például a nyomtatási toohello konzol).</span><span class="sxs-lookup"><span data-stu-id="d44e2-143">hello output is similar toohello input, it could trigger hardware behavior (like hello blinking LED), a message tooother modules, or anything else (like printing toohello console).</span></span>

<span data-ttu-id="d44e2-144">Modulok kommunikálnak egymással használatával `message` objektum.</span><span class="sxs-lookup"><span data-stu-id="d44e2-144">Modules communicate with each other using `message` object.</span></span> <span data-ttu-id="d44e2-145">Hello **tartalom** , egy `message` van, amely képes bármilyen típusú adatok, például egy bájttömböt.</span><span class="sxs-lookup"><span data-stu-id="d44e2-145">hello **content** of a `message` is a byte array that is capable of representing any kind of data you like.</span></span> <span data-ttu-id="d44e2-146">**Tulajdonságok** is rendelkezésre állnak a hello `message` és egyszerűen egy karakterlánc-karakterlánc leképezése.</span><span class="sxs-lookup"><span data-stu-id="d44e2-146">**Properties** are also available in hello `message` and are simply a string-to-string mapping.</span></span> <span data-ttu-id="d44e2-147">Elképzelhető, hogy a **tulajdonságok** hello fejlécként HTTP-kérelem vagy hello metaadat-fájl.</span><span class="sxs-lookup"><span data-stu-id="d44e2-147">You may think of **properties** as hello headers in an HTTP request, or hello metadata of a file.</span></span>

<span data-ttu-id="d44e2-148">Rendelés toodevelop JS egy Azure IoT peremhálózati modulja, egy új module objektum, amely szükséges hello módszerek toocreate kell `receive()`.</span><span class="sxs-lookup"><span data-stu-id="d44e2-148">In order toodevelop an Azure IoT Edge module in JS, you need toocreate a new module object that implements hello required methods `receive()`.</span></span> <span data-ttu-id="d44e2-149">Ezen a ponton is választhatja, hogy tooimplement hello választható `create()` vagy `start()`, vagy `destroy()` módszerek is.</span><span class="sxs-lookup"><span data-stu-id="d44e2-149">At this point, you may also choose tooimplement hello optional `create()` or `start()`, or `destroy()` methods as well.</span></span> <span data-ttu-id="d44e2-150">a következő kódrészletet hello jeleníti meg, akkor hello szerkezetet JS modul objektum.</span><span class="sxs-lookup"><span data-stu-id="d44e2-150">hello following code snippet shows you hello scaffolding of JS module object.</span></span>

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

### <a name="converter-module"></a><span data-ttu-id="d44e2-151">Konverter modul</span><span class="sxs-lookup"><span data-stu-id="d44e2-151">Converter module</span></span>
| <span data-ttu-id="d44e2-152">Input (Bemenet)</span><span class="sxs-lookup"><span data-stu-id="d44e2-152">Input</span></span>                    | <span data-ttu-id="d44e2-153">Processzor</span><span class="sxs-lookup"><span data-stu-id="d44e2-153">Processor</span></span>                              | <span data-ttu-id="d44e2-154">Kimenet</span><span class="sxs-lookup"><span data-stu-id="d44e2-154">Output</span></span>                 | <span data-ttu-id="d44e2-155">Forrásfájl</span><span class="sxs-lookup"><span data-stu-id="d44e2-155">Source File</span></span>            |
| ------------------------ | -------------------------------------- | ---------------------- | ---------------------- |
| <span data-ttu-id="d44e2-156">Hőmérséklet-adatok üzenet</span><span class="sxs-lookup"><span data-stu-id="d44e2-156">Temperature data message</span></span> | <span data-ttu-id="d44e2-157">Elemzése és létrehozni egy új JSON-üzenet</span><span class="sxs-lookup"><span data-stu-id="d44e2-157">Parse and construct a new JSON message</span></span> | <span data-ttu-id="d44e2-158">Struktúra JSON üzenet</span><span class="sxs-lookup"><span data-stu-id="d44e2-158">Structure JSON message</span></span> | `converter.js` |

<span data-ttu-id="d44e2-159">Ez a modul az egy tipikus Azure IoT peremhálózati modul.</span><span class="sxs-lookup"><span data-stu-id="d44e2-159">This module is a typical Azure IoT Edge module.</span></span> <span data-ttu-id="d44e2-160">Hőmérséklet-üzenetek származó adatok elfogad (hardveres modult, vagy ilyen esetben a szimulált BLA modul); és majd normalizálja hőmérséklet üdvözlőüzenetére strukturált tooa JSON üzenet (beleértve a fűznek hello Üzenetazonosítója hello tulajdonságának e igazolnia kell tootrigger hello hőmérséklet riasztást, és így tovább).</span><span class="sxs-lookup"><span data-stu-id="d44e2-160">It accepts temperature messages from other modules (a hardware module, or in this case our simulated BLE module); and then normalizes hello temperature message in tooa structured JSON message (including appending hello message ID, setting hello property of whether we need tootrigger hello temperature alert, and so on).</span></span>

```javascript
receive: function (message) {
  // Initialize hello messageCount in global object at first time.
  if (!global.messageCount) {
    global.messageCount = 0;
  }

  // Read hello content and properties objects from message.
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

  // Publish hello new message toobroker.
  this.broker.publish(
    {
      properties: newProperties,
      content: new Uint8Array(Buffer.from(JSON.stringify(newContent), 'utf8'))
    }
  );
},
```

### <a name="printer-module"></a><span data-ttu-id="d44e2-161">Nyomtató modul</span><span class="sxs-lookup"><span data-stu-id="d44e2-161">Printer module</span></span>
| <span data-ttu-id="d44e2-162">Input (Bemenet)</span><span class="sxs-lookup"><span data-stu-id="d44e2-162">Input</span></span>                          | <span data-ttu-id="d44e2-163">Processzor</span><span class="sxs-lookup"><span data-stu-id="d44e2-163">Processor</span></span> | <span data-ttu-id="d44e2-164">Kimenet</span><span class="sxs-lookup"><span data-stu-id="d44e2-164">Output</span></span>                     | <span data-ttu-id="d44e2-165">Forrásfájl</span><span class="sxs-lookup"><span data-stu-id="d44e2-165">Source File</span></span>          |
| ------------------------------ | --------- | -------------------------- | -------------------- |
| <span data-ttu-id="d44e2-166">Bármely más modulok érkezett üzenetet:</span><span class="sxs-lookup"><span data-stu-id="d44e2-166">Any message from other modules</span></span> | <span data-ttu-id="d44e2-167">N/A</span><span class="sxs-lookup"><span data-stu-id="d44e2-167">N/A</span></span>       | <span data-ttu-id="d44e2-168">Hello üzenet tooconsole naplózása</span><span class="sxs-lookup"><span data-stu-id="d44e2-168">Log hello message tooconsole</span></span> | `printer.js` |

<span data-ttu-id="d44e2-169">Ez a modul az egyszerű, értetődő, amelyek kimenete hello fogadott üzenetek (tulajdonság, tartalom) toohello terminálablakot.</span><span class="sxs-lookup"><span data-stu-id="d44e2-169">This module is simple, self-explanatory, which outputs hello received messages(property, content) toohello terminal window.</span></span>

```javascript
receive: function (message) {
  let properties = JSON.stringify(message.properties);
  let content = Buffer.from(message.content).toString('utf8');

  console.log(`printer.receive.properties - ${properties}`);
  console.log(`printer.receive.content - ${content}\n`);
}
```

### <a name="configuration"></a><span data-ttu-id="d44e2-170">Konfiguráció</span><span class="sxs-lookup"><span data-stu-id="d44e2-170">Configuration</span></span>
<span data-ttu-id="d44e2-171">hello utolsó lépés előtt futó hello modulok tooconfigure hello Azure IoT peremhálózati és tooestablish hello kapcsolatok modulok között.</span><span class="sxs-lookup"><span data-stu-id="d44e2-171">hello final step before running hello modules is tooconfigure hello Azure IoT Edge and tooestablish hello connections between modules.</span></span>

<span data-ttu-id="d44e2-172">Először toodeclare kell a `node` betöltő (óta Azure IoT peremhálózati támogatja betöltők különböző nyelvű) sikerült hivatkozhat, amely a `name` hello szakaszok ezt követően.</span><span class="sxs-lookup"><span data-stu-id="d44e2-172">First we need toodeclare our `node` loader (since Azure IoT Edge supports loaders of different languages) which could be referenced by its `name` in hello sections afterward.</span></span>

```json
"loaders": [
  {
    "type": "node",
    "name": "node"
  }
]
```

<span data-ttu-id="d44e2-173">Miután a betöltők rendelkezik deklaráltuk, azt is kell toodeclare, valamint a modulok.</span><span class="sxs-lookup"><span data-stu-id="d44e2-173">Once we have declared our loaders, we also need toodeclare our modules as well.</span></span> <span data-ttu-id="d44e2-174">Hasonló toodeclaring hello betöltők, azokat is hivatkozhat a `name` attribútum.</span><span class="sxs-lookup"><span data-stu-id="d44e2-174">Similar toodeclaring hello loaders, they can also be referenced by their `name` attribute.</span></span> <span data-ttu-id="d44e2-175">Ha egy modul deklaráló, igazolnia kell a toospecify hello betöltő azt kell használnia (amely előtt meghatározott egyik hello kellene lennie) és a belépési pont (kell lennie a modul hello normalizált osztály neve) minden modul hello.</span><span class="sxs-lookup"><span data-stu-id="d44e2-175">When declaring a module, we need toospecify hello loader it should use (which should be hello one we defined before) and hello entry-point (should be hello normalized class name of our module) for each module.</span></span> <span data-ttu-id="d44e2-176">Hello `simulated_device` modul az egy natív modul, amely hello Azure IoT peremhálózati core runtime csomagban található.</span><span class="sxs-lookup"><span data-stu-id="d44e2-176">hello `simulated_device` module is a native module that is included in hello Azure IoT Edge core runtime package.</span></span> <span data-ttu-id="d44e2-177">Tartalmaznak `args` hello JSON fájl, akkor is, ha a `null`.</span><span class="sxs-lookup"><span data-stu-id="d44e2-177">Include `args` in hello JSON file even if it is `null`.</span></span>

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

<span data-ttu-id="d44e2-178">Hello konfigurációs hello végén a Microsoft hello-kapcsolatok létesítéséhez.</span><span class="sxs-lookup"><span data-stu-id="d44e2-178">At hello end of hello configuration, we establish hello connections.</span></span> <span data-ttu-id="d44e2-179">Minden kapcsolat van kifejezve `source` és `sink`.</span><span class="sxs-lookup"><span data-stu-id="d44e2-179">Each connection is expressed by `source` and `sink`.</span></span> <span data-ttu-id="d44e2-180">Kell mindkét hivatkozó egy előre definiált modul.</span><span class="sxs-lookup"><span data-stu-id="d44e2-180">They should both reference a pre-defined module.</span></span> <span data-ttu-id="d44e2-181">a kimeneti üdvözlőüzenetére `source` modul továbbíthatja a rendszer toohello bemeneti `sink` modul.</span><span class="sxs-lookup"><span data-stu-id="d44e2-181">hello output message of `source` module is forwarded toohello input of `sink` module.</span></span>

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

## <a name="running-hello-modules"></a><span data-ttu-id="d44e2-182">Hello modulok fut</span><span class="sxs-lookup"><span data-stu-id="d44e2-182">Running hello modules</span></span>
1. `npm install`
2. `npm start`

<span data-ttu-id="d44e2-183">Ha azt szeretné, hogy tooterminate hello alkalmazás, nyomja meg az `<Enter>` kulcs.</span><span class="sxs-lookup"><span data-stu-id="d44e2-183">If you want tooterminate hello application, press `<Enter>` key.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="d44e2-184">Nem ajánlott toouse Ctrl + C tooterminate hello IoT peremhálózati alkalmazás.</span><span class="sxs-lookup"><span data-stu-id="d44e2-184">It is not recommended toouse Ctrl + C tooterminate hello IoT Edge application.</span></span> <span data-ttu-id="d44e2-185">Mivel így rendellenesen hello folyamat tooterminate okozhat.</span><span class="sxs-lookup"><span data-stu-id="d44e2-185">As this way may cause hello process tooterminate abnormally.</span></span>
