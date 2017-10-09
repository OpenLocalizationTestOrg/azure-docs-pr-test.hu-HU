---
title: "egy eszköz a Windows C használatával aaaConnect |} Microsoft Docs"
description: "Ismerteti, hogyan tooconnect egy eszköz toohello Azure IoT Suite előre konfigurált távoli figyelési megoldást igényelnek a C Windows rendszeren futó alkalmazást használ."
services: 
suite: iot-suite
documentationcenter: na
author: dominicbetts
manager: timlt
editor: 
ms.assetid: 34e39a58-2434-482c-b3fa-29438a0c05e8
ms.service: iot-suite
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 08/24/2017
ms.author: dobett
ms.openlocfilehash: 51041e0cec113a5cfa006ab2276096baf928eef5
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="connect-your-device-toohello-remote-monitoring-preconfigured-solution-windows"></a>Csatlakozás az eszköz toohello távoli felügyeleti előkonfigurált megoldás (Windows)
[!INCLUDE [iot-suite-selector-connecting](../../includes/iot-suite-selector-connecting.md)]

## <a name="create-a-c-sample-solution-on-windows"></a>C minta megoldás létrehozása a Windows rendszeren
hello lépések bemutatják, hogyan toocreate hello távoli megfigyelési kommunikáló ügyfélalkalmazás előre konfigurált megoldás. Ez az alkalmazás a C és beépített, és futtassa a Windows.

Hozzon létre egy alapszintű projektet a Visual Studio 2015-öt vagy a Visual Studio 2017 és hello IoT Hub eszköz ügyfél NuGet-csomagok hozzáadása:

1. A Visual Studio, a Visual C++ hello segítségével C Konzolalkalmazás létrehozása **Win32 Konzolalkalmazás** sablont. Név hello projekt **RMDevice**.
2. A hello **Alkalmazásbeállítások** hello lap **Win32 alkalmazás varázsló**, ügyeljen arra, hogy **Konzolalkalmazás** van kiválasztva, és törölje a jelet **Precompiled fejléc** és **biztonságos fejlesztési Életciklussal (SDL) ellenőrzi**.
3. A **Megoldáskezelőben**, hello fájlok stdafx.h, targetver.h és stdafx.cpp törléséhez.
4. A **Megoldáskezelőben**, nevezze át a hello fájl RMDevice.cpp tooRMDevice.c.
5. A **Megoldáskezelőben**, kattintson a jobb gombbal a hello **RMDevice** projektre, majd kattintson **kezelése NuGet-csomagok**. Kattintson a **Tallózás**, majd keresse meg, és telepítse a következő NuGet-csomagok hello:
   
   * Microsoft.Azure.IoTHub.Serializer
   * Microsoft.Azure.IoTHub.IoTHubClient
   * Microsoft.Azure.IoTHub.MqttTransport
6. A **Megoldáskezelőben**, kattintson a jobb gombbal a hello **RMDevice** projektre, majd kattintson **tulajdonságok** tooopen hello projekt **tulajdonságlapjain**párbeszédpanel megnyitásához. További információkért lásd: [beállításának Visual C++ projekt tulajdonságai][lnk-c-project-properties]. 
7. Hello kattintson **Linker** mappát, majd kattintson a hello **bemeneti** tulajdonságlapján.
8. Adja hozzá **crypt32.lib** toohello **további függőségek** tulajdonság. Kattintson a **OK** , majd **OK** újra toosave hello tulajdonság értékek.

Adja hozzá a hello Parson JSON könyvtár toohello **RMDevice** projektre, majd adja hozzá a szükséges hello `#include` utasításokat:

1. A megfelelő mappát a számítógépén klónozni hello Parson GitHub-tárházban hello a következő parancs használatával:

    ```
    git clone https://github.com/kgabis/parson.git
    ```

1. Hello parson.h és parson.c fájlok másolása helyi másolatot hello hello Parson tárház tooyour **RMDevice** projekt mappájából.

1. A Visual Studióban, kattintson a jobb gombbal hello **RMDevice** projektre, kattintson **Hozzáadás**, és kattintson a **meglévő cikk**.

1. A hello **meglévő elem hozzáadása** párbeszédpanelen jelölje be hello parson.h és parson.c fájlok hello **RMDevice** projekt mappájából. Kattintson a **Hozzáadás** tooadd ezek két fájlt tooyour projekt.

1. A Visual Studióban nyissa meg a hello RMDevice.c fájlt. Lecseréli a meglévő hello `#include` utasítások hello a következő kódot:
   
    ```c
    #include "iothubtransportmqtt.h"
    #include "schemalib.h"
    #include "iothub_client.h"
    #include "serializer_devicetwin.h"
    #include "schemaserializer.h"
    #include "azure_c_shared_utility/threadapi.h"
    #include "azure_c_shared_utility/platform.h"
    #include "parson.h"
    ```

    > [!NOTE]
    > Most már ellenőrizheti, hogy rendelkezik-e a projekt hello helyes függőség az épület beállítása.

[!INCLUDE [iot-suite-connecting-code](../../includes/iot-suite-connecting-code.md)]

## <a name="build-and-run-hello-sample"></a>Hozza létre és futtasson hello mintát

Adja hozzá a kódot tooinvoke hello **távoli\_figyelési\_futtatása** funkciót, majd létre és hello eszköz alkalmazás futtatásához.

1. Cserélje le a hello **fő** függvényt a következő kód tooinvoke hello **távoli\_figyelési\_futtatása** függvény:
   
    ```c
    int main()
    {
      remote_monitoring_run();
      return 0;
    }
    ```

1. Kattintson a **Build** , majd **megoldás fordítása** toobuild hello alkalmazást.

1. A **Megoldáskezelőben**, kattintson a jobb gombbal hello **RMDevice** projektre, kattintson **Debug**, és kattintson a **Start új példány** toorun hello minta. hello konzol üzeneteket jelenít meg, mint hello alkalmazás elküldi minta telemetriai toohello előre konfigurált megoldás, hello megoldás irányítópultja beállítani kívánt tulajdonságértékek kap, és meghívni a hello megoldás irányítópultja toomethods válaszol.

[!INCLUDE [iot-suite-visualize-connecting](../../includes/iot-suite-visualize-connecting.md)]

[lnk-c-project-properties]: https://msdn.microsoft.com/library/669zx6zc.aspx
