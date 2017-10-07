---
title: "az IoT-átjárón Azure IoT oldala aaaData átalakítás |} Microsoft Docs"
description: "Az IoT átjáró tooconvert hello formátum használata az érzékelők adatstreamének keresztül egy testreszabott modul Azure IoT szélétől."
services: iot-hub
documentationcenter: 
author: shizn
manager: timlt
tags: 
keywords: "IOT-átjáró adatok átalakítása, iot átjáró adatok átalakítása"
ms.assetid: 75f2573d-500b-4405-bff7-61021c4c3500
ms.service: iot-hub
ms.devlang: c
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 06/25/2017
ms.author: xshi
ms.openlocfilehash: ae94b1f96f36dfcb4f77fadc0ece3cff3d0bba91
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="use-iot-gateway-for-sensor-data-transformation-with-azure-iot-edge"></a>Az IoT-átjáró használata az érzékelő adatokat átalakításhoz Azure IoT oldala

> [!NOTE]
> Ez az oktatóanyag megkezdése előtt győződjön meg arról megismerte a befejezett hello során tapasztalatokat követő sorrendben:
> * [Az Intel NUC beállítása IoT-átjáróként](iot-hub-gateway-kit-c-lesson1-set-up-nuc.md)
> * [Az IoT átjáró tooconnect dolgot toohello felhő - SensorTag tooAzure IoT Hub használata](iot-hub-gateway-kit-c-iot-gateway-connect-device-to-cloud.md)

Egy egy Iot-átjáró célja tooprocess gyűjtött adatok toohello felhő elküldése előtt. Az Azure IoT peremhálózati modulok, amely lehet létrehozott és összeállított tooform hello adatfeldolgozási munkafolyamat vezet be. Egy modul üzenetet kap, néhány műveletet végez, és majd helyezze az egyéb modulok tooprocess.

## <a name="what-you-learn"></a>Ismertetett témák

Megtudhatja, hogyan toocreate egy modul tooconvert a hello SensorTag egy másik formátumú fájlba érkező üzenetek.

## <a name="what-you-do"></a>Mit

* Hozzon létre egy modul tooconvert egy fogadott üzenet hello .JSON kiterjesztésű formátumba.
* Fordítási hello modul.
* Adja hozzá a hello modul toohello BLA mintaalkalmazás Azure IoT szélétől.
* Futtassa a hello mintaalkalmazást.

## <a name="what-you-need"></a>Mi szükséges

* a következő sorrendben befejeződött oktatóanyagok hello:
  * [Az Intel NUC beállítása IoT-átjáróként](iot-hub-gateway-kit-c-lesson1-set-up-nuc.md)
  * [Az IoT átjáró tooconnect dolgot toohello felhő - SensorTag tooAzure IoT Hub használata](iot-hub-gateway-kit-c-iot-gateway-connect-device-to-cloud.md)
* Egy SSH-ügyfél rendszert futtató számítógépen. A puTTY Windows ajánlott. Linux- és macOS egy SSH-ügyfél már rendelkeznek.
* hello IP-cím és hello felhasználónév és jelszó tooaccess hello átjáró hello SSH-ügyfél.
* Az internethez.

## <a name="create-a-module"></a>Modul létrehozása

1. Hello állomáson hello SSH-ügyfél, és csatlakozzon a toohello IoT átjáró.
1. Hello forrásfájljaihoz hello átalakítás modulnak a Githubon toohello kezdőkönyvtárát hello IoT átjáró klónozni hello a következő parancsok futtatásával:

   ```bash
   cd ~
   git clone https://github.com/Azure-Samples/iot-hub-c-intel-nuc-gateway-customized-module.git
   ```

   Ez az Azure biztonsági hello C programozási nyelven írt natív modul. hello modul fogadott üzenetek hello formátumának alakítja hello egyet a következő:

   ```json
   {"deviceId": "Intel NUC Gateway", "messageId": 0, "temperature": 0.0}
   ```

## <a name="compile-hello-module"></a>Fordítási hello modul

toocompile hello modul, futtassa a következő parancsok hello:

```bash
cd iot-hub-c-intel-nuc-gateway-customized-module/my_module
# change hello build script runnable
chmod 777 build.sh
# remove hello invalid windows character
sed -i -e "s/\r$//" build.sh
# run hello build shell script
./build.sh
```

Kapott egy `libmy_module.so` fájl hello fordítási befejeződése után. Jegyezze fel a hello fájl abszolút elérési út.

## <a name="add-hello-module-toohello-ble-sample-application"></a>Adja hozzá a hello modul toohello BLA mintaalkalmazás

1. Nyissa meg a toohello minták mappa hello a következő parancs futtatásával:

   ```bash
   cd /usr/share/azureiotgatewaysdk/samples
   ```

1. Nyissa meg a konfigurációs fájl hello hello a következő parancs futtatásával:

   ```bash
   vi ble_gateway.json
   ```

1. A modul hozzá lesz adva a következő kód toohello hello beszúrásával `modules` szakasz.

   ```json
   {
       "name": "MyModule",
       "loader": {
           "name": "native",
           "entrypoint":{
               "module.path": "[Your libmy_module.so path]"
               }
            },
        "args": null
    },
    ```

1. Cserélje le `[Your libmy_module.so path]` hello kódban hello hello libmy_module.so az abszolút elérési úttal rendelkező "fájl.
1. Cserélje le a hello hello kód `links` hello egyet a következő szakaszt:

   ```json
   {
       "source": "SensorTag",
       "sink": "MyModule"
   },
   {
       "source": "MyModule",
       "sink": "mapping"
   }
   ```

1. Nyomja le az `ESC`, majd írja be `:wq` toosave hello fájlt.

## <a name="run-hello-sample-application"></a>Hello mintaalkalmazás futtatása

1. Hello SensorTag bekapcsolása.
1. Hello SSL_CERT_FILE környezeti változó beállítása hello a következő parancs futtatásával:

   ```bash
   export SSL_CERT_FILE=/etc/ssl/certs/ca-certificates.crt
   ```

1. Futtassa a mintaalkalmazást hello hello hozzáadott modul hello a következő parancs futtatásával:

   ```bash
   ./ble_gateway ble_gateway.json
   ```

## <a name="next-steps"></a>Következő lépések

Sikeresen regisztrálta használata hello IoT átjáró tooconvert hello üzenetét SensorTag hello .JSON kiterjesztésű formátumba.

[!INCLUDE [iot-hub-get-started-next-steps](../../includes/iot-hub-get-started-next-steps.md)]
