---
title: "A szimulált eszköz & Azure IoT átjáró - lecke 4: Table storage |} Microsoft Docs"
description: "Intel NUC tooyour IoT hubról üzenetek menthetők, írja őket tooAzure a Table storage, és majd hello felhőből olvashatja őket."
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
ms.openlocfilehash: 43e299d63bbbe10d4d867af25e700c3a7cc07c53
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="read-messages-persisted-in-azure-table-storage"></a>Azure Table storage-ban tárolt üzenetek olvasása

## <a name="what-you-will-do"></a>Mit fog

- Az átjáró által küldött üzenetek tooyour IoT-központ hello átjáró mintaalkalmazás futtatása
- Mintakód futtassa a fogadó számítógép tooread üzeneteket az Azure Table storage-ban.

Ha bármilyen problémába ütközik, keressen megoldásokat a hello [oldal hibaelhárítási](iot-hub-gateway-kit-c-sim-troubleshooting.md).

## <a name="what-you-will-learn"></a>Amiről tanulni fog

Hogyan toouse hello gulp eszköz toorun minta kód tooread köszönőüzenetei az Azure Table storage-ban.

## <a name="what-you-need"></a>Mi szükséges

Sikeres rendelkeznek hello a következő feladatokat:

- [Hello Azure függvény alkalmazás és hello Azure storage-fiók létrehozása](iot-hub-gateway-kit-c-sim-lesson4-deploy-resource-manager-template.md).
- [Hello átjáró mintaalkalmazás futtatása](iot-hub-gateway-kit-c-sim-lesson3-configure-simulated-device-app.md).
- [Üzenetek olvasni az IoT hub](iot-hub-gateway-kit-c-sim-lesson3-read-messages-from-hub.md).

## <a name="get-your-azure-storage-connection-strings"></a>Az Azure storage kapcsolati karakterláncok beolvasása

Ez a lecke korai szakaszában sikeresen létrehozott egy Azure storage-fiók. tooget hello kapcsolati karakterlánca hello Azure storage-fiók, futtassa a következő parancsok hello:

* A tárfiókok listázása.

```bash
az storage account list -g iot-gateway --query [].name
```

* Az azure storage kapcsolati karakterlánc beolvasása.

```bash
az storage account show-connection-string -g iot-gateway -n {storage name}
```

Használjon `iot-gateway` hello értékeként `{resource group name}` nem módosítása hello érték a 2.

## <a name="configure-hello-device-connection"></a>Hello eszköz kapcsolat konfigurálása

Frissítés hello `config-azure.json` fájlt úgy, hogy hello mintakód hello állomáson futó elolvashatják üzenet az Azure Table storage-ban. tooconfigure hello eszköz kapcsolat, kövesse az alábbi lépéseket:

1. Nyissa meg hello eszköz konfigurációs fájl `config-azure.json` hello a következő parancsok futtatásával:

   ```bash
   # For Windows command prompt
   code %USERPROFILE%\.iot-hub-getting-started\config-azure.json
   # For MacOS or Ubuntu
   code ~/.iot-hub-getting-started/config-azure.json
   ```

   ![konfiguráció](media/iot-hub-gateway-kit-lessons/lesson4/config_azure.png)

2. Cserélje le `[Azure storage connection string]` a hello szerezte be az Azure storage kapcsolati karakterláncot.

   `[IoT hub connection string]`már helyébe szakaszban [üzenetek olvasásakor Azure IoT Hub](iot-hub-gateway-kit-c-sim-lesson3-read-messages-from-hub.md) a Lesson3.

## <a name="read-messages-in-your-azure-table-storage"></a>Az Azure Table storage üzenet olvasása

Hello átjáró mintaalkalmazás futtatása, és Azure Table storage üzenetek olvashatják hello a következő parancsot:

```bash
gulp run --table-storage
```

Az IoT hub új üzenet érkezésekor elindítja az Azure Table storage-be az Azure-függvény alkalmazás toosave üzenetet.
Hello `gulp run` parancs által küldött üzenetek tooyour IoT-központ átjáró mintaalkalmazás futtatja. A `table-storage` paraméter, akkor is indít folyamatot gyermek tooreceive hello üzenet mentése az Azure Table storage-ban.

küldött és fogadott azonos konzolablakában az összes megjelenített azonnal hello köszönőüzenetei hello gazdaszámítógépen. hello minta alkalmazáspéldány 40 másodperc múlva automatikusan leáll.

   ![gulp olvasása](media/iot-hub-gateway-kit-lessons/lesson4/gulp_run_read_table_simudev.png)


## <a name="summary"></a>Összefoglalás

Minta kód tooread hello köszönőüzenetei az Azure Table Storage mentése az Azure-függvény alkalmazás futtatását.
