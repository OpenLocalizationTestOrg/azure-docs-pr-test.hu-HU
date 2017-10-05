---
title: "A szimulált eszköz & Azure IoT átjáró - lecke 4: Table storage |} Microsoft Docs"
description: "Intel NUC üzenetek mentéséhez az IoT hub, Azure Table Storage írja őket, és majd olvashatja őket a felhőből."
services: iot-hub
documentationcenter: 
author: shizn
manager: timtl
tags: 
keywords: "adatok beolvasása a felhőből, iot-felhőszolgáltatás"
ROBOTS: NOINDEX
redirect_url: /azure/iot-hub/iot-hub-gateway-kit-c-lesson1-set-up-nuc
ms.assetid: 78e4b6ea-968d-401e-a7dc-8f9acdb3ec1a
ms.service: iot-hub
ms.devlang: c
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 3/21/2017
ms.author: xshi
ms.openlocfilehash: de5fae794c195132e2a487c0095845c756aa28e3
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="read-messages-persisted-in-azure-table-storage"></a>Azure Table storage-ban tárolt üzenetek olvasása

## <a name="what-you-will-do"></a>Mit fog

- Futtassa az átjáró mintaalkalmazást az átjárón, amely üzeneteket küld az IoT hub.
- A gazdaszámítógépen, ha az Azure Table storage üzeneteket példakód futtatása.

Ha bármilyen problémába ütközik, tekintse meg a megoldások a [oldal hibaelhárítási](iot-hub-gateway-kit-c-sim-troubleshooting.md).

## <a name="what-you-will-learn"></a>Amiről tanulni fog

Hogyan használhatja a gulp eszközt az Azure Table storage üzeneteit mintakódot futtatásához.

## <a name="what-you-need"></a>Mi szükséges

Sikeres rendelkeznek kész a következő feladatokat:

- [Az Azure-függvény alkalmazás és az Azure storage-fiók létrehozása](iot-hub-gateway-kit-c-sim-lesson4-deploy-resource-manager-template.md).
- [Futtassa az átjáró mintaalkalmazást](iot-hub-gateway-kit-c-sim-lesson3-configure-simulated-device-app.md).
- [Üzenetek olvasni az IoT hub](iot-hub-gateway-kit-c-sim-lesson3-read-messages-from-hub.md).

## <a name="get-your-azure-storage-connection-strings"></a>Az Azure storage kapcsolati karakterláncok beolvasása

Ez a lecke korai szakaszában sikeresen létrehozott egy Azure storage-fiók. Ahhoz, hogy az Azure-tárfiók kapcsolati karakterlánca, a következő parancsokat:

* A tárfiókok listázása.

```bash
az storage account list -g iot-gateway --query [].name
```

* Az azure storage kapcsolati karakterlánc beolvasása.

```bash
az storage account show-connection-string -g iot-gateway -n {storage name}
```

Használjon `iot-gateway` értékeként `{resource group name}` Ha nem módosítja az érték a 2.

## <a name="configure-the-device-connection"></a>Az eszköz kapcsolat konfigurálása

Frissítés a `config-azure.json` fájlt úgy, hogy a mintakódot az állomáson futó elolvashatják üzenet az Azure Table storage-ban. Az eszköz kapcsolat konfigurálásához kövesse az alábbi lépéseket:

1. Nyissa meg az eszköz konfigurációs fájlját `config-azure.json` a következő parancsok futtatásával:

   ```bash
   # For Windows command prompt
   code %USERPROFILE%\.iot-hub-getting-started\config-azure.json
   # For MacOS or Ubuntu
   code ~/.iot-hub-getting-started/config-azure.json
   ```

   ![konfiguráció](media/iot-hub-gateway-kit-lessons/lesson4/config_azure.png)

2. Cserélje le `[Azure storage connection string]` szerezte be az Azure storage kapcsolati karakterlánccal.

   `[IoT hub connection string]`már helyébe szakaszban [üzenetek olvasásakor Azure IoT Hub](iot-hub-gateway-kit-c-sim-lesson3-read-messages-from-hub.md) a Lesson3.

## <a name="read-messages-in-your-azure-table-storage"></a>Az Azure Table storage üzenet olvasása

Futtassa az átjáró mintaalkalmazást, és olvassa el az Azure Table storage üzenetek által a következő parancsot:

```bash
gulp run --table-storage
```

Az IoT hub elindítja az Azure-függvény alkalmazás mentse az üzenet az Azure Table storage az új üzenet érkezésekor.
A `gulp run` parancs fut, amely üzeneteket küld az IoT hub átjáró mintaalkalmazást. A `table-storage` paraméter, akkor is indít olyan alárendelt folyamatot, amely a mentett üzenet az Azure Table storage-ban.

Által küldött és fogadott összes üzenetek azonnal jelenik meg az azonos konzolablak a gazdaszámítógépen. A minta alkalmazáspéldány 40 másodperc múlva automatikusan leáll.

   ![gulp olvasása](media/iot-hub-gateway-kit-lessons/lesson4/gulp_run_read_table_simudev.png)


## <a name="summary"></a>Összefoglalás

Az Azure Table storage az Azure-függvény alkalmazás által mentett olvashatja a mintakódot futtatását.
