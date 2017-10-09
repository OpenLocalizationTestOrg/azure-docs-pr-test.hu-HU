---
title: "egy eszköz használatával C mbed aaaConnect |} Microsoft Docs"
description: "Ismerteti, hogyan tooconnect egy eszköz toohello Azure IoT Suite előre konfigurált távoli figyelési megoldást igényelnek a C egy mbed eszközön futó alkalmazást használ."
services: 
suite: iot-suite
documentationcenter: na
author: dominicbetts
manager: timlt
editor: 
ms.assetid: 9551075e-dcf9-488f-943e-d0eb0e6260be
ms.service: iot-suite
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 05/22/2017
ms.author: dobett
ROBOTS: NOINDEX
ms.openlocfilehash: dcd1e74635e8dec678a59bff060a73f7cfabd124
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="connect-your-device-toohello-remote-monitoring-preconfigured-solution-mbed"></a>Csatlakozzon a távoli felügyeleti előkonfigurált megoldás (mbed) eszköz toohello

## <a name="scenario-overview"></a>Forgatókönyv áttekintése
Ebben a forgatókönyvben a következő telemetriai toohello távoli megfigyelési hello küldő eszköz létrehozása [előre konfigurált megoldás][lnk-what-are-preconfig-solutions]:

* Külső hőmérséklet
* Belső hőmérséklet
* Páratartalom

Az egyszerűség kedvéért hello kód hello eszközön mintaértékek állít elő, de javasoljuk, tooextend hello minta valós érzékelők tooyour eszköz csatlakoztatása és valós telemetriai adatok küldését.

hello eszköz is, képes toorespond toomethods hello megoldás irányítópultja meghívni, és tulajdonságértékeket hello megoldás irányítópultjának beállítása szükséges.

toocomplete ebben az oktatóanyagban egy aktív Azure-fiókra van szüksége. Ha nincs fiókja, néhány perc alatt létrehozhat egy ingyenes próbafiókot. További információ: [Ingyenes Azure-fiók létrehozása][lnk-free-trial].

## <a name="before-you-start"></a>Előkészületek
Mielőtt bármilyen kódot írna az eszközhöz, ki kell építenie az előre konfigurált távoli figyelési megoldást, és meg kell adnia egy új egyéni eszközt ebben a megoldásban.

### <a name="provision-your-remote-monitoring-preconfigured-solution"></a>Az előre konfigurált távoli figyelési megoldás kiépítése
Ebben az oktatóanyagban létrehozhat hello eszköz küldi hello adatok tooan példányának [távoli megfigyelési] [ lnk-remote-monitoring] előre konfigurált megoldás. Ha még nem már kiépített hello távoli felügyeleti előkonfigurált megoldás az Azure-fiókjába, használja a hello a következő lépéseket:

1. A hello <https://www.azureiotsuite.com/> kattintson  **+**  toocreate megoldást.
2. Kattintson a **válasszon** a hello **távoli megfigyelési** panel toocreate a megoldás.
3. A hello **létrehozása távoli felügyeleti megoldás** lapján adjon meg egy **megoldásnév** az Ön által választott, válassza ki a hello **régió** szeretné, hogy toodeploy, és válassza ki a hello Azure előfizetés toowant toouse. Ezután kattintson a **Megoldás létrehozása** parancsra.
4. Várjon, amíg a kiépítési folyamat hello befejeződik.

> [!WARNING]
> előre konfigurált hello megoldások számlázható Azure-szolgáltatásokat használja. Ne feledje tooremove hello előkonfigurált megoldás az előfizetésből, amikor elkészült, azt tooavoid a szükségtelen díjakat. Teljesen eltávolíthatja a előkonfigurált megoldás az előfizetésből hello felkeresésével <https://www.azureiotsuite.com/> lap.
> 
> 

Ha létesítésének folyamatát kell használnia a távoli felügyeleti megoldás hello hello befejeződik, kattintson a **indítása** tooopen hello megoldás irányítópultja a böngészőben.

![A megoldás irányítópultja][img-dashboard]

### <a name="provision-your-device-in-hello-remote-monitoring-solution"></a>Az eszköz a távoli felügyeleti megoldás hello kiépítése
> [!NOTE]
> Ha már kiépített egy eszközt a megoldásában, kihagyhatja ezt a lépést. Hello ügyfélalkalmazás létrehozásakor tooknow hello eszköz hitelesítő adatok szükségesek.
> 
> 

Eszköz tooconnect előre konfigurált toohello megoldás, azt kell azonosítani magát tooIoT Hub érvényes hitelesítő adatok használatával. Hello megoldás irányítópultja hello eszközhitelesítő adatok kérhetnek le. Hello eszközhitelesítő adatok szerepeljenek az ügyfélalkalmazás az oktatóanyag későbbi részében.

tooadd eszköz tooyour távoli figyelési megoldás, a következő teljes hello hello megoldás irányítópultja lépései:

1. Hello bal alsó sarkában hello irányítópultot, kattintson **hozzáad egy eszközt**.
   
   ![Eszköz hozzáadása][1]
2. A hello **egyéni eszköz** panelen, kattintson a **új hozzáadása**.
   
   ![Egyéni eszköz hozzáadása][2]
3. Válassza a **Meghatározom a saját eszközazonosítómat** elemet. Adja meg például az Eszközazonosítót **mydevice**, kattintson a **ellenőrizze azonosító** tooverify ugyanez a neve már nincs használatban, és kattintson a **létrehozása** tooprovision hello eszköz.
   
   ![Eszközazonosító hozzáadása][3]
4. Megjegyzés: hello eszköz tétele (Eszközazonosító IoT Hub állomásnév és eszközkulcs) hitelesítő adatait. Az ügyfélalkalmazás kell ezen értékek tooconnect toohello távoli felügyeleti megoldás. Ezután kattintson a **Done** (Kész) gombra.
   
    ![Eszköz hitelesítő adatainak megtekintése][4]
5. Jelölje ki az eszköz hello megoldás irányítópultjának hello eszközök listáját. Ezt követően a hello **eszközadatok** panelen, kattintson a **eszköz engedélyezése**. hello az eszköz állapota ezután **futtató**. Mostantól telemetriai adatok fogadhatók az eszközről és metódusok hello eszközön hello távoli figyelési megoldást igényelnek.

[img-dashboard]: ./media/iot-suite-connecting-devices-mbed/dashboard.png
[1]: ./media/iot-suite-connecting-devices-mbed/suite0.png
[2]: ./media/iot-suite-connecting-devices-mbed/suite1.png
[3]: ./media/iot-suite-connecting-devices-mbed/suite2.png
[4]: ./media/iot-suite-connecting-devices-mbed/suite3.png

[lnk-what-are-preconfig-solutions]: iot-suite-what-are-preconfigured-solutions.md
[lnk-remote-monitoring]: iot-suite-remote-monitoring-sample-walkthrough.md
[lnk-free-trial]: http://azure.microsoft.com/pricing/free-trial/

## <a name="build-and-run-hello-c-sample-solution"></a>Hozza létre, és futtassa a hello C megoldást

hello következő útmutatások mutatják be hello hoz való csatlakoztatásának lépéseit egy [mbed-kompatibilis Freescale FRDM-K64F] [ lnk-mbed-home] eszköz toohello távoli felügyeleti megoldás.

### <a name="connect-hello-mbed-device-tooyour-network-and-desktop-machine"></a>Csatlakoztassa hello mbed eszköz tooyour hálózati és az asztali számítógép

1. Hello mbed eszköz tooyour hálózati Ethernet kábellel csatlakoztassa. Ez a lépés szükség, mert hello mintaalkalmazás internet-hozzáférésre van szüksége.

1. Lásd: [Ismerkedés a mbed] [ lnk-mbed-getstarted] tooconnect mbed eszköz tooyour asztali PC.

1. Ha az asztali számítógép Windows fut, lásd: [PC-konfigurációt] [ lnk-mbed-pcconnect] tooconfigure soros port hozzáférés tooyour mbed eszköz.

### <a name="create-an-mbed-project-and-import-hello-sample-code"></a>Mbed projekt létrehozása és importálása hello mintakód

Kövesse ezeket a lépéseket tooadd néhány kódot tooan mbed mintaprojektet. Hello távoli figyelési alapszintű projekt importálása, és módosítsa a hello projekt toouse hello MQTT protokoll hello AMQP protokoll helyett. Jelenleg toouse hello MQTT protokoll toouse hello eszköz felügyeleti funkciókat az IoT-központ van szüksége.

1. A böngészőben nyissa meg toohello mbed.org [fejlesztői hely](https://developer.mbed.org/). Ha még nem regisztrált, megjelenik egy beállítás toocreate (szabad) fiók. Ellenkező esetben jelentkezzen be a fiók hitelesítő adataival. Kattintson a **fordító** hello jobb felső sarokban hello lap. Ez a művelet hozása toohello *munkaterület* felületet.

1. Győződjön meg arról, hello hardver platform használata hello jobb felső sarkában hello ablak jelenik meg, vagy kattintson a jobb sarkában tooselect hello hello ikonra a hardver platform.

1. Kattintson a **importálási** hello főmenü. Kattintson a **kattintson ide a URL-CÍMRŐL tooimport**.
   
    ![Indítsa el az importálási toombed munkaterület][6]

1. A hello előugró ablak, írja be hello hivatkozás hello minta kód https://developer.mbed.org/users/AzureIoTClient/code/remote_monitoring/, majd kattintson az **importálási**.
   
    ![A minta kód toombed munkaterület importálása][7]

1. Hello mbed fordító ablakban látható, hogy a projekt importálása is importálja a szalagtárak különböző. Néhány megadott és hello Azure IoT csapat által fenntartott ([azureiot_common](https://developer.mbed.org/users/AzureIoTClient/code/azureiot_common/), [iothub_client](https://developer.mbed.org/users/AzureIoTClient/code/iothub_client/), [iothub_amqp_transport](https://developer.mbed.org/users/AzureIoTClient/code/iothub_amqp_transport/), [azure_uamqp](https://developer.mbed.org/users/AzureIoTClient/code/azure_uamqp/)), a külső szalagtárak hello mbed szalagtárak katalógus elérhető vannak.
   
    ![Mbed projekt megtekintése][8]

1. A hello **Program munkaterület**, kattintson a jobb gombbal hello **IOT hubbal\_amqp\_átviteli** könyvtár, kattintson **törlése**, és kattintson a **OK** tooconfirm.

1. A hello **Program munkaterület**, kattintson a jobb gombbal hello **azure\_amqp\_c** könyvtár, kattintson a **törlése**, és kattintson a **OK**  tooconfirm.

1. Kattintson a jobb gombbal hello **remote_monitoring** projektre a hello **Program munkaterület**, jelölje be **importálási könyvtár**, majd jelölje be **URL-ből**.
   
    ![Indítsa el a könyvtár importálási toombed munkaterület][6]

1. A hello előugró ablak, írja be hello hivatkozás hello MQTT átviteli könyvtár https://developer.mbed.org/users/AzureIoTClient/code/iothub\_mqtt\_átviteli / kattintson **importálási**.
   
    ![Könyvtár toombed munkaterület importálása][12]

1. Ismétlődő hello előző lépés tooadd hello MQTT kódtárat https://developer.mbed.org/users/AzureIoTClient/code/azure\_umqtt\_c /.

1. A munkaterület most következőhöz hasonló hello:

    ![Mbed munkaterület megtekintése][13]

1. Nyissa meg hello távoli\_monitoring\remote_monitoring.c fájl- és a név felülírandó hello meglévő `#include` utasítások a következő kód hello:

    ```c
    #include "iothubtransportmqtt.h"
    #include "schemalib.h"
    #include "iothub_client.h"
    #include "serializer_devicetwin.h"
    #include "schemaserializer.h"
    #include "azure_c_shared_utility/threadapi.h"
    #include "azure_c_shared_utility/platform.h"
    #include "parson.h"

    #ifdef MBED_BUILD_TIMESTAMP
    #include "certs.h"
    #endif // MBED_BUILD_TIMESTAMP
    ```
1. Távoli hello kód fennmaradó összes hello törlése\_monitoring\remote\_monitoring.c fájlt.

[!INCLUDE [iot-suite-connecting-code](../../includes/iot-suite-connecting-code.md)]

## <a name="build-and-run-hello-sample"></a>Hozza létre és futtasson hello mintát

Adja hozzá a kódot tooinvoke hello **távoli\_figyelési\_futtatása** funkciót, majd létre és hello eszköz alkalmazás futtatásához.

1. Adja hozzá a **fő** függvény a következő kódot a távoli hello hello végén\_monitoring.c fájl tooinvoke hello **távoli\_figyelési\_futtatása** függvény:
   
    ```c
    int main()
    {
      remote_monitoring_run();
      return 0;
    }
    ```

1. Kattintson a **fordítási** toobuild hello program. Nyugodtan figyelmen kívül hagyhatja a figyelmeztetéseket, de ha hello összeállítási hibát, javítsa ki a folytatás előtt.

1. Sikeres hello build esetén hello mbed fordító webhelyet hoz létre a projekt hello nevű .bin fájl, és letölti tooyour helyi számítógép. Másolás hello .bin fájl toohello eszköz. Hello .bin fájl toohello eszköz mentése hello eszköz toorestart azt eredményezi, és futtassa hello programot hello .bin fájl található. Később manuálisan is újraindíthatja hello program bármikor hello mbed eszközön hello alaphelyzetbe állítása gomb lenyomásával.

1. Csatlakozzon SSH ügyfél, mint a PuTTY alkalmazást használ toohello eszköz. Azt is meghatározhatja a eszköz által használt által a Windows hello soros port.
   
    ![][11]

1. A PuTTY, kattintson a hello **soros** kapcsolattípus. hello általában eszköze 9600 átviteli sebességgel, ezért adja meg 9600 hello **sebesség** mezőbe. Kattintson a **nyitott**.

1. hello program végrehajtásának elkezdése. Előfordulhat, hogy tooreset hello board (nyomja meg a CTRL + Break vagy nyomja le az hello board alaphelyzetbe állítása gomb) Ha hello program nem indul el automatikusan csatlakoztatásakor.
   
    ![][10]

[!INCLUDE [iot-suite-visualize-connecting](../../includes/iot-suite-visualize-connecting.md)]

[6]: ./media/iot-suite-connecting-devices-mbed/mbed1.png
[7]: ./media/iot-suite-connecting-devices-mbed/mbed2a.png
[8]: ./media/iot-suite-connecting-devices-mbed/mbed3a.png
[10]: ./media/iot-suite-connecting-devices-mbed/putty.png
[11]: ./media/iot-suite-connecting-devices-mbed/mbed6.png
[12]: ./media/iot-suite-connecting-devices-mbed/mbed7.png
[13]: ./media/iot-suite-connecting-devices-mbed/mbed8.png

[lnk-mbed-home]: https://developer.mbed.org/platforms/FRDM-K64F/
[lnk-mbed-getstarted]: https://developer.mbed.org/platforms/FRDM-K64F/#getting-started-with-mbed
[lnk-mbed-pcconnect]: https://developer.mbed.org/platforms/FRDM-K64F/#pc-configuration
