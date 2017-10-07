---
title: "aaaSave az IoT hub-üzenetek tooAzure adattárolás |} Microsoft Docs"
description: "Egy Azure függvény app toosave az IoT hub üzenetek tooyour Azure table storage használata. IoT hub köszönőüzenetei tartalmaz információkat, például az IoT-eszközről küldött érzékelőadatait."
services: iot-hub
documentationcenter: 
author: shizn
manager: timlt
tags: 
keywords: "az IOT adattárolás, iot érzékelő adatokat tároló"
ms.assetid: 62fd14fd-aaaa-4b3d-8367-75c1111b6269
ms.service: iot-hub
ms.devlang: arduino
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 08/16/2017
ms.author: xshi
ms.openlocfilehash: be72d9ba9a781822926364351b50f58f5d96e94a
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="save-iot-hub-messages-that-contain-sensor-data-tooyour-azure-table-storage"></a>Az IoT hub üzeneteket, amelyek tartalmazzák az érzékelő adatokat tooyour Azure table storage mentése

![Végpontok közötti diagramja](media/iot-hub-get-started-e2e-diagram/3.png)

[!INCLUDE [iot-hub-get-started-note](../../includes/iot-hub-get-started-note.md)]

## <a name="what-you-learn"></a>Ismertetett témák

Megtudhatja, hogyan működik az toocreate egy Azure storage-fiókot és az Azure app toostore IoT hub üzenetek a table storage-ban.

## <a name="what-you-do"></a>Mit

- Hozzon létre egy Azure-tárfiókot.
- Készítse elő az IoT hub kapcsolati tooread üzenetek.
- Létrehozhat és telepíthet egy Azure függvény alkalmazást.

## <a name="what-you-need"></a>Mi szükséges

- [Konfigurálja az eszközt](iot-hub-raspberry-pi-kit-node-get-started.md) toocover hello követelményeknek:
  - Aktív Azure-előfizetés
  - Az IoT-központ az előfizetéshez tartozó 
  - Egy futó alkalmazás által küldött üzenetek tooyour IoT-központ

## <a name="create-an-azure-storage-account"></a>Azure-tárfiók létrehozása

1. A hello [Azure-portálon](https://portal.azure.com/), kattintson a **új** > **tárolási** > **tárfiók**  >   **Hozzon létre**.

2. Adja meg hello hello tárfiók szükséges adatokat:

   ![Hozzon létre egy tárfiókot a hello Azure-portálon](media\iot-hub-store-data-in-azure-table-storage\1_azure-portal-create-storage-account.png)

   * **Név**: hello hello storage-fiók nevét. hello nevének globálisan egyedinek kell lennie.

   * **Erőforráscsoport**: használata hello ugyanaz az IoT hub használó erőforráscsoportot.

   * **PIN-kód toodashboard**: válassza ezt a beállítást, mennyire egyszerű a hozzáférés tooyour IoT hub hello irányítópulton.

3. Kattintson a **Create** (Létrehozás) gombra.

## <a name="prepare-your-iot-hub-connection-tooread-messages"></a>Az IoT hub kapcsolati tooread üzenetek előkészítése

Az IoT-központ közzététele egy beépített hub-kompatibilis végpont tooenable alkalmazások tooread IoT hub eseményüzenetek. Eközben alkalmazások a fogyasztói csoportok tooread adatokat az IoT hub használhatják. Az IoT hub hoz létre egy Azure függvény alkalmazásadatok tooread, mielőtt hello a következő:

- Az IoT-központ végpontjának hello kapcsolati karakterlánc beolvasása.
- Hozzon létre az IoT hub fogyasztói csoportot.

### <a name="get-hello-connection-string-of-your-iot-hub-endpoint"></a>Az IoT-központ végpontjának hello kapcsolati karakterlánc beolvasása

1. Nyissa meg az IoT hub.

2. A hello **IoT-központ** ablaktáblán, a **Messaging**, kattintson a **végpontok**.

3. Hello a jobb oldali ablaktáblán, a **beépített végpontok**, kattintson a **események**.

4. A hello **tulajdonságok** ablaktáblán, a következő értékek Megjegyzés hello:
   - Event hub-kompatibilis végpont
   - Event hub-kompatibilis neve

   ![Az IoT-központ végpontjának hello kapcsolati karakterlánc beolvasása a hello Azure-portálon](media\iot-hub-store-data-in-azure-table-storage\2_azure-portal-iot-hub-endpoint-connection-string.png)

5. A hello **IoT-központ** ablaktáblán, a **beállítások**, kattintson a **megosztott elérési házirendek**.

6. Kattintson a **iothubowner**.

7. Megjegyzés: hello **elsődleges kulcs** érték.

8. Az alábbiak szerint hozza létre az IoT-központ végpontjának hello kapcsolati karakterlánca:

   `Endpoint=<Event Hub-compatible endpoint>;SharedAccessKeyName=iothubowner;SharedAccessKey=<Primary key>`

   > [!NOTE]
   > Cserélje le `<Event Hub-compatible endpoint>` és `<Primary key>` hello értékekkel, amelyet korábban feljegyzett.

### <a name="create-a-consumer-group-for-your-iot-hub"></a>Az IoT hub felhasználói csoport létrehozása

1. Nyissa meg az IoT hub.

2. A hello **IoT-központ** ablaktáblán, a **Messaging**, kattintson a **végpontok**.

3. Hello a jobb oldali ablaktáblán, a **beépített végpontok**, kattintson a **események**.

4. A hello **tulajdonságok** ablaktáblán, a **fogyasztói csoportok**, és adjon meg egy nevet, majd jegyezze fel az azt.

5. Kattintson a **Save** (Mentés) gombra.

## <a name="create-and-deploy-an-azure-function-app"></a>Hozzon létre és Azure függvény alkalmazás telepítése

1. A hello [Azure-portálon](https://portal.azure.com/), kattintson a **új** > **számítási** > **függvény App**  >   **Hozzon létre**.

2. Adja meg a szükséges információkat hello hello függvény alkalmazást.

   ![Egy függvény-alkalmazás létrehozása az Azure-portálon hello](media\iot-hub-store-data-in-azure-table-storage\3_azure-portal-create-function-app.png)

   * **Alkalmazásnév**: hello függvény alkalmazás hello nevét. hello nevének globálisan egyedinek kell lennie.

   * **Erőforráscsoport**: használata hello ugyanaz az IoT hub használó erőforráscsoportot.

   * **A Tárfiók**: hello létrehozott tárfiók.

   * **PIN-kód toodashboard**: ezt a beállítást egyszerű a hozzáférés toohello függvény alkalmazás hello irányítópulton.

3. Kattintson a **Create** (Létrehozás) gombra.

4. Hello függvény alkalmazás létrehozása után nyissa meg.

5. Hello függvény alkalmazás hozzon létre egy új függvényt hello következő tevékenységek végrehajtásával:

   a. Kattintson a **új függvény**.

   b. Válassza ki **JavaScript** a **nyelvi**, és **adatfeldolgozási** a **forgatókönyv**.

   c. Kattintson a **Ez a függvény létrehozása**, és kattintson a **új függvény**.

   d. Válassza ki **JavaScript** hello nyelvhez és **adatfeldolgozási** hello forgatókönyvhöz.

   e. Kattintson a hello **EventHubTrigger-JavaScript** sablont.

   f. Adja meg a szükséges információkat hello hello sablont.

      * **A funkció neve**: hello hello függvény neve.

      * **Eseményközpont neveként**: hello event hub-kompatibilis nevét, amelyet korábban feljegyzett.

      * **Esemény hubkapcsolat**: tooadd hello kapcsolati karakterlánca hello IoT-központ végpontjának létrehozott, kattintson a **új**.

   g. Kattintson a **Create** (Létrehozás) gombra.

6. Adja meg egy kimeneti hello függvény hello következő tevékenységek végrehajtásával:

   a. Kattintson a **integrálni** > **új kimeneti** > **Azure Table Storage** > **válasszon**.

      ![Adja hozzá a table storage tooyour függvény app hello Azure-portálon](media\iot-hub-store-data-in-azure-table-storage\4_azure-portal-function-app-add-output-table-storage.png)

   b. Adja meg a hello szükséges adatokat.

      * **Tábla paraméter neve**: használata `outputTable`, amely használandó hello Azure függvény kódot.
      
      * **Táblanév**: használata `deviceData`.

      * **Fiók tárolókapcsolat**: kattintson a **új**, majd válassza ki vagy adja meg a tárfiók. Ha hello tárfiók nem jelenik meg, tekintse meg [tárolási fiókra vonatkozó követelmények](https://docs.microsoft.com/azure/azure-functions/functions-create-function-app-portal#storage-account-requirements).
      
   c. Kattintson a **Save** (Mentés) gombra.

7. A **eseményindítók**, kattintson a **Azure Event Hubs (eventHubMessages)**.

8. A **Eseményközpont fogyasztói csoportot**, adjon meg hello hello fogyasztói csoportot létrehozni, és kattintson **mentése**.

9. Kattintson a bal oldali hello létrehozott hello függvény majd **fájlok megtekintése** a jobb oldali hello.

10. Cserélje le a hello kód `index.js` hello alábbira:

   ```javascript
   'use strict';

   // This function is triggered each time a message is received in hello IoT hub.
   // hello message payload is persisted in an Azure storage table
 
   module.exports = function (context, iotHubMessage) {
    context.log('Message received: ' + JSON.stringify(iotHubMessage));
    var date = Date.now();
    var partitionKey = Math.floor(date / (24 * 60 * 60 * 1000)) + '';
    var rowKey = date + '';
    context.bindings.outputTable = {
     "partitionKey": partitionKey,
     "rowKey": rowKey,
     "message": JSON.stringify(iotHubMessage)
    };
    context.done();
   };
   ```

11. Kattintson a **Save** (Mentés) gombra.

Most létrehozott hello függvény alkalmazást. Üzenetek, amely az IoT hub megkapja a table storage-ban tárolja.

> [!NOTE]
> Használhatja a hello **futtatása** gomb tootest hello függvény alkalmazást. Amikor rákattint **futtatása**, hello tesztüzenet küldése tooyour IoT-központot. hello érkezésének üdvözlőüzenetére a kell hello függvény app toostart elindítható, és mentse a hello üzenet tooyour a table storage. Hello **naplók** ablaktábla hello hello folyamat részletes adatait rögzíti.

## <a name="verify-your-message-in-your-table-storage"></a>Ellenőrizze az üzenetet a table storage-ban

1. Az eszköz toosend üzenetek tooyour IoT hub hello mintaalkalmazás futtatása

2. [Töltse le és telepítse az Azure Tártallózó](http://storageexplorer.com/).

3. Nyissa meg a Tártallózót, kattintson **egy Azure-fiók hozzáadása** > **jelentkezzen be a**, majd jelentkezzen be Azure-fiók tooyour.

4. Kattintson az Azure-előfizetéshez > **Tárfiókok** > a tárfiók > **táblák** > **deviceData**.

   Láthatja, hogy az eszköz tooyour IoT hub hello bejelentkezett küldött üzenetek `deviceData` tábla.

## <a name="next-steps"></a>Következő lépések

Sikeresen létrehozta az Azure storage-fiókok és az Azure függvény alkalmazás, amely tárolja az üzeneteket, amely az IoT hub megkapja a table storage-ban.

[!INCLUDE [iot-hub-get-started-next-steps](../../includes/iot-hub-get-started-next-steps.md)]
