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
# <a name="create-an-azure-iot-edge-module-with-nodejs"></a>Hozzon létre egy Azure IoT peremhálózati modul Node.js

Ez az oktatóanyag bővíthető hogyan toocreate egy modul a JS Azure IoT szegélyt.

Az oktatóanyag azt bízná környezetet, és hogyan toowrite egy [Gedélyezése](https://en.wikipedia.org/wiki/Bluetooth_Low_Energy) adatok konverter modul hello legújabb Azure IoT peremhálózati NPM-csomagok használata.

## <a name="prerequisites"></a>Előfeltételek

Ebben a szakaszban, állítsa be a IoT peremhálózati modul fejlesztési környezetet. Ez a kategória tooboth *64 bites Windows* és *64 bites Linux (Ubuntu 14 +)* operációs rendszerek.

a következő szoftver hello szükség:
* [Git ügyfél](https://git-scm.com/downloads).
* [Csomópont LTS](https://nodejs.org).
* `npm install -g yo`.
* `npm install -g generator-az-iot-gw-module`

## <a name="architecture"></a>Architektúra

hello Azure IoT peremhálózati platform fokozottan fogad hello [Von Neumann architektúra](https://en.wikipedia.org/wiki/Von_Neumann_architecture). Ez azt jelenti, hogy hello teljes Azure IoT peremhálózati architektúra egy rendszer, amely feldolgozza a bemeneti és a kimenetet; és győződjön meg arról, hogy minden egyes modul is egy nagyon kicsi bemeneti / kimeneti alrendszer. Ebben az oktatóanyagban bemutatása után a következő két modulok hello:

1. A modul, amely fogad egy szimulált [Gedélyezése](https://en.wikipedia.org/wiki/Bluetooth_Low_Energy) jelezze, és konvertálja azt egy formázott [JSON](https://en.wikipedia.org/wiki/JSON) üzenet.
2. A modul, amely a fogadott hello kinyomtatja [JSON](https://en.wikipedia.org/wiki/JSON) üzenet.

hello következő képet jeleníti meg hello tipikus end tooend adatfolyamblokk ebben a projektben:

![Három modulok között Adatfolyamblokk](media/iot-hub-iot-edge-create-module/dataflow.png "bemenet: szimulált BLA modul; Processzor: Konverter modul; Kimenete: Nyomtató modul")

## <a name="set-up-hello-environment"></a>Hello környezet beállítása
Az alábbiakban azt jelzik, hogy hogyan tooquickly beállítása környezet toostart toowrite JS az első táblázat konverter modul.

### <a name="create-module-project"></a>A modul-projekt létrehozása
1. Nyisson meg egy parancssori ablakot, futtassa `yo az-iot-gw-module`.
2. Hello lépésekkel hello képernyő toofinish hello inicializálásakor a modul projekt.

### <a name="project-structure"></a>Projektstruktúra
A következő összetevők hello JS modul projekt foglalja magában:

`modules`-hello testreszabott JS modul forrásfájlokat. Cserélje le a hello alapértelmezett `sensor.js` és `printer.js` saját modul fájlokkal.

`app.js`-hello bejegyzés fájl toostart hello peremhálózati példány.

`gw.config.json`-Edge által betöltött hello konfigurációs fájl toocustomize hello modulok toobe.

`package.json`-hello modul projekt metaadat-információit.

`README.md`-hello modul projekt alapvető dokumentációját.


### <a name="package-file"></a>A csomagfájl

Ez `package.json` deklarál, amely tartalmazza az hello név, verzió, bejegyzést, parancsfájlok, futásidejű és fejlesztési függőségek modul projekthez szükséges összes hello metaadatait.

Következő kódrészletben látható kód hogyan BLA konverter tooconfigure mintát a projektet.
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


### <a name="entry-file"></a>Bejegyzés fájl
Hello `app.js` hello módon tooinitialize hello peremhálózati példány meghatározása. Itt nem szükséges toomake bármi is módosul.

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

### <a name="interface-of-module"></a>Felület modul
Egy Azure IoT Edge-modul is kezelheti, amelynek a feladata, feldolgozónak: fogadja, feldolgozni azt és eredménye.

hello bemeneti hardverek (például egy mozgásérzékelő) adatait, egy üzenet más modulok, vagy bármi más (például egy időzítő rendszeresen generálja véletlenszerűen) lehet.

hello kimeneti hasonló toohello bemeneti, hogy elindíthatja hardver viselkedés (például LED villogó hello), egy üzenet tooother modulok vagy bármi más (például a nyomtatási toohello konzol).

Modulok kommunikálnak egymással használatával `message` objektum. Hello **tartalom** , egy `message` van, amely képes bármilyen típusú adatok, például egy bájttömböt. **Tulajdonságok** is rendelkezésre állnak a hello `message` és egyszerűen egy karakterlánc-karakterlánc leképezése. Elképzelhető, hogy a **tulajdonságok** hello fejlécként HTTP-kérelem vagy hello metaadat-fájl.

Rendelés toodevelop JS egy Azure IoT peremhálózati modulja, egy új module objektum, amely szükséges hello módszerek toocreate kell `receive()`. Ezen a ponton is választhatja, hogy tooimplement hello választható `create()` vagy `start()`, vagy `destroy()` módszerek is. a következő kódrészletet hello jeleníti meg, akkor hello szerkezetet JS modul objektum.

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

### <a name="converter-module"></a>Konverter modul
| Input (Bemenet)                    | Processzor                              | Kimenet                 | Forrásfájl            |
| ------------------------ | -------------------------------------- | ---------------------- | ---------------------- |
| Hőmérséklet-adatok üzenet | Elemzése és létrehozni egy új JSON-üzenet | Struktúra JSON üzenet | `converter.js` |

Ez a modul az egy tipikus Azure IoT peremhálózati modul. Hőmérséklet-üzenetek származó adatok elfogad (hardveres modult, vagy ilyen esetben a szimulált BLA modul); és majd normalizálja hőmérséklet üdvözlőüzenetére strukturált tooa JSON üzenet (beleértve a fűznek hello Üzenetazonosítója hello tulajdonságának e igazolnia kell tootrigger hello hőmérséklet riasztást, és így tovább).

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

### <a name="printer-module"></a>Nyomtató modul
| Input (Bemenet)                          | Processzor | Kimenet                     | Forrásfájl          |
| ------------------------------ | --------- | -------------------------- | -------------------- |
| Bármely más modulok érkezett üzenetet: | N/A       | Hello üzenet tooconsole naplózása | `printer.js` |

Ez a modul az egyszerű, értetődő, amelyek kimenete hello fogadott üzenetek (tulajdonság, tartalom) toohello terminálablakot.

```javascript
receive: function (message) {
  let properties = JSON.stringify(message.properties);
  let content = Buffer.from(message.content).toString('utf8');

  console.log(`printer.receive.properties - ${properties}`);
  console.log(`printer.receive.content - ${content}\n`);
}
```

### <a name="configuration"></a>Konfiguráció
hello utolsó lépés előtt futó hello modulok tooconfigure hello Azure IoT peremhálózati és tooestablish hello kapcsolatok modulok között.

Először toodeclare kell a `node` betöltő (óta Azure IoT peremhálózati támogatja betöltők különböző nyelvű) sikerült hivatkozhat, amely a `name` hello szakaszok ezt követően.

```json
"loaders": [
  {
    "type": "node",
    "name": "node"
  }
]
```

Miután a betöltők rendelkezik deklaráltuk, azt is kell toodeclare, valamint a modulok. Hasonló toodeclaring hello betöltők, azokat is hivatkozhat a `name` attribútum. Ha egy modul deklaráló, igazolnia kell a toospecify hello betöltő azt kell használnia (amely előtt meghatározott egyik hello kellene lennie) és a belépési pont (kell lennie a modul hello normalizált osztály neve) minden modul hello. Hello `simulated_device` modul az egy natív modul, amely hello Azure IoT peremhálózati core runtime csomagban található. Tartalmaznak `args` hello JSON fájl, akkor is, ha a `null`.

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

Hello konfigurációs hello végén a Microsoft hello-kapcsolatok létesítéséhez. Minden kapcsolat van kifejezve `source` és `sink`. Kell mindkét hivatkozó egy előre definiált modul. a kimeneti üdvözlőüzenetére `source` modul továbbíthatja a rendszer toohello bemeneti `sink` modul.

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

## <a name="running-hello-modules"></a>Hello modulok fut
1. `npm install`
2. `npm start`

Ha azt szeretné, hogy tooterminate hello alkalmazás, nyomja meg az `<Enter>` kulcs.

> [!IMPORTANT]
> Nem ajánlott toouse Ctrl + C tooterminate hello IoT peremhálózati alkalmazás. Mivel így rendellenesen hello folyamat tooterminate okozhat.
