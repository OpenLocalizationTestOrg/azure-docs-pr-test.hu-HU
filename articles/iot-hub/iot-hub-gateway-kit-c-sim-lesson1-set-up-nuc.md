---
title: "A szimulált eszköz & Azure IoT átjáró - lecke 1: NUC beállítása |} Microsoft Docs"
description: "Intel NUC beállítása érzékelő és az Azure IoT Hub érzékelő adatokat gyűjteni, és elküldi a IoT-központ közötti IoT átjáróként működik."
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
ms.openlocfilehash: b87974be9570f7d03fe84ae0a1d1fa7e346ff189
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="set-up-intel-nuc-as-an-iot-gateway"></a>Az IoT átjáróként Intel NUC beállítása

## <a name="what-you-will-do"></a>Mit fog

- Az IoT-átjáró Intel NUC állíthatja be.
- Telepítse az Azure IoT biztonsági csomag Intel NUC.
- Futtassa a "hello_world" mintaalkalmazás Intel NUC átjáró működésének ellenőrzéséhez.
Ha bármilyen problémába ütközik, tekintse meg a megoldások a [oldal hibaelhárítási](iot-hub-gateway-kit-c-sim-troubleshooting.md).

## <a name="what-you-will-learn"></a>Amiről tanulni fog

Ez a lecke témák köre:

- Hogyan perifériák Intel NUC kapcsolódni.
- Hogyan kell telepíteni, és frissítse a szükséges csomagokat a Intel NUC az intelligens Package Manager segítségével.
- Hogyan futtassa a "hello_world" mintaalkalmazást átjáró működésének ellenőrzéséhez.

## <a name="what-you-need"></a>Mi szükséges

- Az Intel NUC Kit DE3815TYKE a Intel IoT átjáró termékcsomag (szél folyó Linux * 7.0.0.13) előtelepítése mellett.
- Kábel.
- A billentyűzet.
- Egy HDMI vagy VGA kábel.
- Egy figyelő HDMI vagy VGA-port.

![Átjáró csomag](media/iot-hub-gateway-kit-lessons/lesson1/kit_without_sensortag.png)

## <a name="connect-intel-nuc-with-the-peripherals"></a>Csatlakozás a perifériák Intel NUC

Az alábbi képen látható egy példa Intel NUC különböző perifériák kapcsolódnak:

1. A billentyűzet csatlakozik.
2. Csatlakozik a figyelő egy VGA vagy HDMI kábellel.
3. Kábel vezetékes hálózathoz csatlakoznak.
4. A tápegység power kábellel csatlakozik.

![Intel NUC perifériák csatlakozik.](media/iot-hub-gateway-kit-lessons/lesson1/nuc.png)

## <a name="connect-to-the-intel-nuc-system-from-host-computer-via-secure-shell-ssh"></a>A gazdaszámítógép keresztül Secure Shell (SSH) az Intel NUC rendszerhez való csatlakozás

Itt szüksége billentyűzet és figyelheti az IP-címet a NUC eszköz eléréséhez. Ha már ismeri az IP-cím, kihagyhatja, hogy ez a szakasz a 3. lépés.

1. Intel NUC bekapcsolása a főkapcsoló lenyomásával, és jelentkezzen be a rendszer.

   Az alapértelmezett felhasználónév és jelszó mindkét `root`.

2. NUC IP-címének lekéréséhez futtassa a `ifconfig` parancsot. Ez a lépés a NUC eszközön történik.

   Íme egy példa a parancs kimenetét.

   ![ifconfig kimeneti NUC IP megjelenítése](media/iot-hub-gateway-kit-lessons/lesson1/ifconfig.png)

   Ebben a példában az érték, amely a következő `inet addr:` az IP-címe, ha azt tervezi, távolról csatlakozni a számítógép Intel NUC.

3. Használja a következő SSH ügyfelek a számítógép Intel NUC való kapcsolódáshoz.

   - [A puTTY](http://www.putty.org/) Windows.
   - A beépített SSH-ügyfél Ubuntu vagy macOS.

   Hatékony, és növeli az való működésre Intel NUC el gazdagépről. Van szüksége a az IP cím, felhasználónevet és jelszót a NUC SSH-ügyfél keresztül csatlakozni. A macOS Ez a példa használata SSH-ügyfél.
   ![MacOS futó SSH-ügyfél](media/iot-hub-gateway-kit-lessons/lesson1/ssh.png)

## <a name="install-the-azure-iot-edge-package"></a>Telepítse az Azure IoT Edge-csomagot

Az Azure IoT biztonsági csomag tartalmazza az előre lefordított bináris fájljait az SDK-t és annak függőségeit. A bináris fájljait a Azure IoT él, az Azure IoT SDK és a megfelelő eszközök. A csomag "hello_world" mintaalkalmazás, amely ellenőrzi az átjárófunkció is tartalmaz. Az IoT-Edge az átjáró alapvető részét képezi. A csomag telepítéséhez kövesse az alábbi lépéseket:

1. Az IoT-felhő tárház hozzáadása egy terminál ablakban a következő parancsok futtatásával:

   ```bash
   rpm --import http://iotdk.intel.com/misc/iot_pub.key
   smart channel --add IoT_Cloud type=rpm-md name="IoT_Cloud" baseurl=http://iotdk.intel.com/repos/iot-cloud/wrlinux7/rcpl13/ -y
   ```

   A `rpm` parancs importálja a rpm kulcsot. A `smart channel` parancs hozzáadja a rpm-csatorna a intelligens Csomagkezelő. Mielőtt újra lefuttatja a `smart update` hasonló kimenetnek parancs, lásd alább.

   ![rpm és intelligens csatorna parancs kimenete](media/iot-hub-gateway-kit-lessons/lesson1/rpm_smart_channel.png)

   ```bash
   smart update
   ```

2. A csomag telepítése a következő parancs futtatásával:

   ```bash
   smart install packagegroup-cloud-azure -y
   ```

   `packagegroup-cloud-azure`a csomag neve van. A `smart install` a parancsnak a csomag telepítéséhez.

   A csomag telepítése után a Intel NUC várhatóan átjáróként működik.

## <a name="run-the-azure-iot-edge-helloworld-sample-application"></a>Az Azure IoT peremhálózati "hello_world" mintaalkalmazás futtatása

Ugrás a `azureiotgatewaysdk/samples` és futtassa a "hello_world" minta mintaalkalmazást. A mintaalkalmazás létrehoz egy átjárót a `hello_world.json` fájlt, és használja az Azure IoT peremhálózati architektúra alapvető összetevői 5 másodpercentként bejelentkezési hello world üzenet fájlba.

A minta "hello_world" mintaalkalmazást is futtathatja a következő parancs futtatásával:

```bash
cd /usr/share/azureiotgatewaysdk/samples/hello_world/
./hello_world hello_world.json
```

A mintaalkalmazást a következő kimenetet hoz létre, ha az átjárófunkció megfelelően működik-e:

![alkalmazás kimenete](media/iot-hub-gateway-kit-lessons/lesson1/hello_world.png)

Ha bármilyen problémába ütközik, tekintse meg a megoldások a [oldal hibaelhárítási](iot-hub-gateway-kit-c-troubleshooting.md).

## <a name="summary"></a>Összefoglalás

Gratulálunk! Intel NUC átjáróként beállításának befejezése után. Most már készen áll áthelyezése a következő lecke állítsa be a gazdaszámítógép, Azure IoT hub létrehozása és regisztrálása az Azure IoT hub logikai eszköz.

## <a name="next-steps"></a>Következő lépések
[Felkészülés a gazdaszámítógép és Azure IoT-központ](iot-hub-gateway-kit-c-sim-lesson2-get-the-tools-win32.md)
