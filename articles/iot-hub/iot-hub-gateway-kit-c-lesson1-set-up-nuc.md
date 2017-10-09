---
title: "SensorTag eszköz & Azure IoT átjáró - lecke 1: Intel NUC beállítása |} Microsoft Docs"
description: "Az IoT-átjáró érzékelő és az Azure IoT Hub toocollect érzékelő adatokat közötti Intel NUC toowork beállítani, és elküldi a tooIoT központ."
services: iot-hub
documentationcenter: 
author: shizn
manager: timlt
tags: 
keywords: "az IOT-átjárón intel nuc nuc számítógép, DE3815TYKE"
ms.assetid: 917090d6-35c2-495b-a620-ca6f9c02b317
ms.service: iot-hub
ms.devlang: c
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 3/21/2017
ms.author: xshi
ms.openlocfilehash: 7c3ab3b014713c7facb86b8e8622d70e60a960e6
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="set-up-intel-nuc-as-an-iot-gateway"></a>Az IoT átjáróként Intel NUC beállítása
[!INCLUDE [iot-hub-get-started-device-selector](../../includes/iot-hub-get-started-device-selector.md)]

## <a name="what-you-will-do"></a>Mit fog

- Az IoT-átjáró Intel NUC állíthatja be.
- Hello Intel NUC hello Azure IoT biztonsági csomag telepítését.
- Futtassa a "hello_world" mintaalkalmazás hello Intel NUC tooverify hello átjáró funkciót.

  > Ha bármilyen problémába ütközik, keressen megoldásokat a hello [oldal hibaelhárítási](iot-hub-gateway-kit-c-troubleshooting.md).

## <a name="what-you-will-learn"></a>Amiről tanulni fog

Ez a lecke témák köre:

- Hogyan tooconnect Intel NUC a perifériaeszközökbe.
- Hogyan tooinstall és frissítés szükséges hello csomagok Intel NUC használatával hello intelligens Package Manager.
- Hogyan toorun hello "hello_world" mintát alkalmazás tooverify hello átjáró funkciót.

## <a name="what-you-need"></a>Mi szükséges

- Az Intel NUC Kit DE3815TYKE hello Intel IoT átjáró termékcsomag (szél folyó Linux * 7.0.0.13) előtelepítése mellett. [Kattintson ide toopurchase Groove IoT kereskedelmi átjáró Kit](https://www.seeedstudio.com/Grove-IoT-Commercial-Gateway-Kit-p-2724.html).
- Kábel.
- A billentyűzet.
- Egy HDMI vagy VGA kábel.
- Egy figyelő HDMI vagy VGA-port.
- Választható lehetőség: [Texas eszközök érzékelő címke (CC2650STK)](http://www.ti.com/tool/cc2650stk)

![Átjáró csomag](media/iot-hub-gateway-kit-lessons/lesson1/kit.png)

## <a name="connect-intel-nuc-with-hello-peripherals"></a>Csatlakozás Intel NUC hello perifériák

az alábbi képen hello Intel NUC különböző perifériák kapcsolódnak példája:

1. Csatlakoztatott tooa billentyűzet.
2. Csatlakoztatott tooa figyelő egy VGA vagy HDMI kábellel.
3. Vezetékes Ethernet kábel hálózathoz csatlakoztatott tooa.
4. Energiagazdálkodási kábellel csatlakoztatott tooa tápegység.

![Intel NUC tooperipherals csatlakoztatva](media/iot-hub-gateway-kit-lessons/lesson1/nuc.png)

## <a name="connect-toohello-intel-nuc-system-from-host-computer-via-secure-shell-ssh"></a>Toohello Intel NUC rendszer csatlakoztatja a gazdaszámítógép keresztül Secure Shell (SSH)

Billentyűzet és egy figyelő tooget hello IP-címet az Intel NUC eszköz lesz szüksége. Ha már ismeri a hello IP-címet, ugorjon előre toostep 3 ebben a szakaszban.

1. Kapcsolja be hello Intel NUC hello power gomb megnyomásával, majd jelentkezzen be.

   hello alapértelmezett felhasználónév és jelszó `root`.

       > Hit hello enter key on your keyboard if you see either of hello following errors when you boot: 'A TPM error (7) occurred attempting tooread a pcr value.' or 'Timeout, No TPM chip found, activating TPM-bypass!'

2. Hello Intel NUC hello IP-címének lekéréséhez futtassa a hello `ifconfig` hello Intel NUC eszközön parancsot.

   Itt található példa hello parancs kimenetét.

   ![Intel NUC IP megjelenítő ifconfig kimeneti](media/iot-hub-gateway-kit-lessons/lesson1/ifconfig.png)

   Ebben a példában hello érték, amely a következő `inet addr:` hello IP-cím szükséges, amikor kapcsolódik az Intel NUC toohello el gazdagépről.

3. A fogadó számítógép tooconnect tooIntel NUC az SSH-ügyfél következő hello egyikét használhatja.

    - [A puTTY](http://www.putty.org/) Windows.
    - hello beépített SSH-ügyfél Ubuntu vagy macOS.

   Hatékony és hatékony toooperate egy állomásról egy Intel NUC. Akkor lesz szüksége hello Intel NUC IP-cím, felhasználói név és jelszó tooconnect tooit keresztül egy SSH-ügyfél. Íme egy példa, egy SSH-ügyfél által az macOS.
   ![MacOS futó SSH-ügyfél](media/iot-hub-gateway-kit-lessons/lesson1/ssh.png)

## <a name="install-hello-azure-iot-edge-package"></a>Hello Azure IoT biztonsági csomag telepítése

hello Azure IoT biztonsági csomag hello előre lefordított bináris fájljait IoT széle és annak függőségeit. A bináris fájljait a Azure IoT él, hello Azure IoT SDK és megfelelő eszközök hello. hello csomag is tartalmaz egy "hello_world" mintaalkalmazás használt toovalidate hello átjáró funkciót. Az IoT-Edge része hello core hello átjáró. 

Hajtsa végre az alábbi lépéseket tooinstall hello csomag.

1. Hello IoT felhő tárház hozzáadása a következő parancsokat egy terminálablakot hello futtatásával:

   ```bash
   rpm --import https://iotdk.intel.com/misc/iot_pub2.key
   smart channel --add IoT_Cloud type=rpm-md name="IoT_Cloud" baseurl=http://iotdk.intel.com/repos/iot-cloud/wrlinux7/rcpl13/ -y
   smart channel --add WR_Repo type=rpm-md baseurl=https://distro.windriver.com/release/idp-3-xt/public_feeds/WR-IDP-3-XT-Intel-Baytrail-public-repo/RCPL13/corei7_64/
   ```

   > Adja meg, "y", ha akkor too'Include kéri, ezt a csatornát? "
   
   Ha megjelenik egy `import read failed(-1)` hiba, a következő parancsok tooresolve hello probléma használata hello:
   ```bash
   wget http://iotdk.intel.com/misc/iot_pub2.key 
   rpm --import iot_pub2.key  
   ```

   Hello `rpm` importálja hello rpm kulcs parancsot. Hello `smart channel` parancs hozzáadja hello rpm csatorna toohello intelligens Package Manager. Hello futtatása előtt `smart update` parancsban, látni fogja, például egy kimeneti alatt.

   ![rpm és intelligens csatorna parancs kimenete](media/iot-hub-gateway-kit-lessons/lesson1/rpm_smart_channel.png)

2. Hello intelligens frissítés parancs hajtható végre:

   ```bash
   smart update
   ```

3. Hello Azure IoT-átjáró csomag telepítése hello a következő parancs futtatásával:

   ```bash
   smart install packagegroup-cloud-azure -y
   ```

   `packagegroup-cloud-azure`hello csomag hello neve van. Hello `smart install` parancs használt tooinstall hello csomag.

    > Futtatási hello következő parancsot, ha ezt a hibaüzenetet látja: "nyilvános kulcs nem érhető el"

    ```bash
    smart config --set rpm-check-signatures=false
    smart install packagegroup-cloud-azure -y
    ```
    > Indítsa újra a hello Intel NUC, ha ezt a hibaüzenetet látja: "nincs csomag biztosítja a fejlesztői linux util"

   Hello csomag telepítése után Intel NUC készen toofunction átjáróként.

## <a name="run-hello-azure-iot-edge-helloworld-sample-application"></a>Hello Azure IoT peremhálózati "hello_world" mintaalkalmazás futtatása

a következő mintaalkalmazás hello hoz létre az átjáró egy `hello_world.json` fájlt, és hello Azure IoT peremhálózati architektúra toolog egy hello world tooa üzenetfájlt (log.txt) alapvető összetevőinek 5 másodpercentként használja.

Hello Hello World PéldaAlkalmazás futtathatja a következő hello a következő parancsok futtatásával:

```bash
cd /usr/share/azureiotgatewaysdk/samples/hello_world/
./hello_world hello_world.json
```

Hello Hello World alkalmazás futtatása néhány percet, és majd nyomja le az Enter kulcs toostop hello segítségével azt.
![alkalmazás kimenete](media/iot-hub-gateway-kit-lessons/lesson1/hello_world.png)

> A "argumentum érvénytelen handle(NULL)" hibák után le az ENTER billentyűt figyelmen kívül hagyhatja.

Adott hello átjáró sikeresen lefutott, amelyik most már a hello_world mappában hello log.txt fájl megnyitásával ellenőrizheti ![log.txt directory megtekintése](media/iot-hub-gateway-kit-lessons/lesson1/logtxtdir.png)

Nyissa meg a következő parancs hello segítségével log.txt:

```bash
vim log.txt
```

Ekkor megjelenik hello tartalmát log.txt, ez az üdvözlő naplózási üzenetek 5 másodpercentként hello átjáró Hello World modul által írt egy formázott JSON-kimenetét.
![log.txt directory megtekintése](media/iot-hub-gateway-kit-lessons/lesson1/logtxtview.png)

Ha bármilyen problémába ütközik, keressen megoldásokat a hello [oldal hibaelhárítási](iot-hub-gateway-kit-c-troubleshooting.md).

## <a name="summary"></a>Összefoglalás

Gratulálunk! Intel NUC átjáróként beállításának befejezése után. Most már készen áll a következő lecke tooset toohello a fogadó számítógép toomove Azure IoT Hub létrehozása, és regisztrálja az Azure IoT Hub logikai eszközt.

## <a name="next-steps"></a>Következő lépések
[Az IoT-átjáró tooconnect egy eszköz tooAzure IoT Hub használata](iot-hub-gateway-kit-c-iot-gateway-connect-device-to-cloud.md)

