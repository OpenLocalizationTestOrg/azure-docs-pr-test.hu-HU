---
title: "A szimulált eszköz & Azure IoT átjáró - lecke 1: NUC beállítása |} Microsoft Docs"
description: "Az IoT-átjáró érzékelő és az Azure IoT Hub toocollect érzékelő adatokat közötti Intel NUC toowork beállítani, és elküldi a tooIoT központ."
services: iot-hub
documentationcenter: 
author: shizn
manager: yjianfeng
tags: 
keywords: "az IOT-átjárón intel nuc nuc számítógép, DE3815TYKE"
ROBOTS: NOINDEX
redirect_url: /azure/iot-hub/iot-hub-gateway-kit-c-lesson1-set-up-nuc
ms.assetid: f41d6b2e-9b00-40df-90eb-17d824bea883
ms.service: iot-hub
ms.devlang: c
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 3/21/2017
ms.author: xshi
ms.openlocfilehash: c2146c7cd8ca54adbd0af279364ec8484be1b45b
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="set-up-intel-nuc-as-an-iot-gateway"></a>Az IoT átjáróként Intel NUC beállítása

## <a name="what-you-will-do"></a>Mit fog

- Az IoT-átjáró Intel NUC állíthatja be.
- Intel NUC hello Azure IoT biztonsági csomag telepítését.
- Futtassa a "hello_world" mintaalkalmazás Intel NUC tooverify hello átjáró funkciót.
Ha bármilyen problémába ütközik, keressen megoldásokat a hello [oldal hibaelhárítási](iot-hub-gateway-kit-c-sim-troubleshooting.md).

## <a name="what-you-will-learn"></a>Amiről tanulni fog

Ez a lecke témák köre:

- Hogyan tooconnect Intel NUC a perifériaeszközökbe.
- Hogyan tooinstall és frissítés szükséges hello csomagok Intel NUC használatával hello intelligens Package Manager.
- Hogyan toorun hello "hello_world" mintát alkalmazás tooverify hello átjáró funkciót.

## <a name="what-you-need"></a>Mi szükséges

- Az Intel NUC Kit DE3815TYKE hello Intel IoT átjáró termékcsomag (szél folyó Linux * 7.0.0.13) előtelepítése mellett.
- Kábel.
- A billentyűzet.
- Egy HDMI vagy VGA kábel.
- Egy figyelő HDMI vagy VGA-port.

![Átjáró csomag](media/iot-hub-gateway-kit-lessons/lesson1/kit_without_sensortag.png)

## <a name="connect-intel-nuc-with-hello-peripherals"></a>Csatlakozás Intel NUC hello perifériák

az alábbi képen hello Intel NUC különböző perifériák kapcsolódnak példája:

1. Csatlakoztatott tooa billentyűzet.
2. Toohello figyelő által a VGA vagy HDMI kábellel csatlakozik.
3. Csatlakoztatott tooa hálózati vezetékes Ethernet-kábellel.
4. Energiagazdálkodási kábellel csatlakoztatott toohello tápegység.

![Intel NUC tooperipherals csatlakoztatva](media/iot-hub-gateway-kit-lessons/lesson1/nuc.png)

## <a name="connect-toohello-intel-nuc-system-from-host-computer-via-secure-shell-ssh"></a>Toohello Intel NUC rendszer csatlakoztatja a gazdaszámítógép keresztül Secure Shell (SSH)

Itt billentyűzet és monitor tooget hello IP-címet a NUC eszköz van szüksége. Ha már ismeri a hello IP-címet, kihagyhatja a toostep 3 ebben a szakaszban.

1. Kapcsolja be a Intel NUC hello rendszer hello Főkapcsoló és a naplófájlok lenyomásával.

   hello alapértelmezett felhasználónév és jelszó `root`.

2. Hello IP-címe NUC lekéréséhez futtassa a hello `ifconfig` parancsot. Ez a lépés hello NUC eszközön történik.

   Itt található példa hello parancs kimenetét.

   ![ifconfig kimeneti NUC IP megjelenítése](media/iot-hub-gateway-kit-lessons/lesson1/ifconfig.png)

   Ebben a példában hello érték, amely a következő `inet addr:` hello IP-cím, ha azt tervezi, hogy a gazdagép számítógép tooIntel NUC távolról a tooconnect.

3. SSH-ügyfél követően a gazdagép gép tooconnect tooIntel NUC hello egyikét használhatja.

   - [A puTTY](http://www.putty.org/) Windows.
   - hello beépített SSH-ügyfél Ubuntu vagy macOS.

   Egy számítógép az Intel NUC hatékonyabb és hatékony toooperate. Hello hello IP-cím, a felhasználónevet és jelszót kell tooconnect hello NUC SSH-ügyfél használatával. Itt nincs macOS hello példa használata SSH-ügyfél.
   ![MacOS futó SSH-ügyfél](media/iot-hub-gateway-kit-lessons/lesson1/ssh.png)

## <a name="install-hello-azure-iot-edge-package"></a>Hello Azure IoT biztonsági csomag telepítése

hello Azure IoT biztonsági csomag hello hello SDK előre lefordított bináris fájljait és annak függőségeit. A bináris fájljait a Azure IoT él, hello Azure IoT SDK és megfelelő eszközök hello. hello csomag "hello_world" mintaalkalmazás, amely használt toovalidate hello átjáró funkciókat is tartalmaz. Az IoT-Edge része hello core hello átjáró. tooinstall hello csomag, kövesse az alábbi lépéseket:

1. Vegyen fel hello IoT felhőalapú-tárházat hello követő egy terminálablakot parancsok futtatásával:

   ```bash
   rpm --import http://iotdk.intel.com/misc/iot_pub.key
   smart channel --add IoT_Cloud type=rpm-md name="IoT_Cloud" baseurl=http://iotdk.intel.com/repos/iot-cloud/wrlinux7/rcpl13/ -y
   ```

   Hello `rpm` importálja hello rpm kulcs parancsot. Hello `smart channel` parancs hozzáadja hello rpm csatorna toohello intelligens Package Manager. Hello futtatása előtt `smart update` hasonló kimenetnek parancs, lásd alább.

   ![rpm és intelligens csatorna parancs kimenete](media/iot-hub-gateway-kit-lessons/lesson1/rpm_smart_channel.png)

   ```bash
   smart update
   ```

2. Hello telepítéséhez hello a következő parancs futtatásával:

   ```bash
   smart install packagegroup-cloud-azure -y
   ```

   `packagegroup-cloud-azure`hello csomag hello neve van. Hello `smart install` parancs használt tooinstall hello csomag.

   Hello csomag telepítése után Intel NUC várt toowork átjáróként.

## <a name="run-hello-azure-iot-edge-helloworld-sample-application"></a>Hello Azure IoT peremhálózati "hello_world" mintaalkalmazás futtatása

Nyissa meg túl`azureiotgatewaysdk/samples` és hello minta "hello_world" mintaalkalmazás futtatása. A mintaalkalmazás létrehoz egy átjáró hello `hello_world.json` fájlt, és alapvető összetevői hello hello Azure IoT peremhálózati architektúra toolog egy hello world tooa üzenetfájlt 5 másodpercentként használja.

Futtathatja a minta "hello_world" hello mintaalkalmazás hello a következő parancs futtatásával:

```bash
cd /usr/share/azureiotgatewaysdk/samples/hello_world/
./hello_world hello_world.json
```

hello mintaalkalmazás hoz létre, ha hello átjárófunkció megfelelően működik-e a kimeneti hello következő:

![alkalmazás kimenete](media/iot-hub-gateway-kit-lessons/lesson1/hello_world.png)

Ha bármilyen problémába ütközik, keressen megoldásokat a hello [oldal hibaelhárítási](iot-hub-gateway-kit-c-troubleshooting.md).

## <a name="summary"></a>Összefoglalás

Gratulálunk! Intel NUC átjáróként beállításának befejezése után. Most már készen áll a következő lecke tooset toohello a fogadó számítógép toomove Azure IoT hub létrehozása, és regisztrálja az Azure IoT hub logikai eszközt.

## <a name="next-steps"></a>Következő lépések
[Felkészülés a gazdaszámítógép és Azure IoT-központ](iot-hub-gateway-kit-c-sim-lesson2-get-the-tools-win32.md)
