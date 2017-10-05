---
title: "Csatlakozás egy eszközt mbed C |} Microsoft Docs"
description: "Eszköz csatlakoztatása az Azure IoT Suite előre konfigurált távoli figyelési megoldást igényelnek a C mbed-eszközön fut-e olyan alkalmazással ismerteti."
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
ms.openlocfilehash: ef7b78f85a787f8fbe22c0e26aa34f0cd1685d58
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="connect-your-device-to-the-remote-monitoring-preconfigured-solution-mbed"></a>Csatlakoztassa az eszközt a távoli felügyeleti előkonfigurált megoldás (mbed)

## <a name="scenario-overview"></a>Forgatókönyv áttekintése
Ebben az esetben olyan eszközt hoz létre, amely a következő telemetriát küldi az [előre konfigurált][lnk-what-are-preconfig-solutions] távoli figyelési megoldásnak:

* Külső hőmérséklet
* Belső hőmérséklet
* Páratartalom

Az egyszerűség kedvéért az eszközön lévő kód mintaértékeket hoz létre, de javasoljuk, hogy terjessze ki a mintát valós érzékelők az eszközhöz csatlakoztatása és valós telemetria küldése révén.

Az eszköz a megoldás irányítópultjáról indított metódusokra és a megoldás irányítópultján beállított tulajdonságértékekre is tud válaszolni.

Az oktatóanyag elvégzéséhez egy aktív Azure-fiókra lesz szüksége. Ha nincs fiókja, néhány perc alatt létrehozhat egy ingyenes próbafiókot. További információ: [Ingyenes Azure-fiók létrehozása][lnk-free-trial].

## <a name="before-you-start"></a>Előkészületek
Mielőtt bármilyen kódot írna az eszközhöz, ki kell építenie az előre konfigurált távoli figyelési megoldást, és meg kell adnia egy új egyéni eszközt ebben a megoldásban.

### <a name="provision-your-remote-monitoring-preconfigured-solution"></a>Az előre konfigurált távoli figyelési megoldás kiépítése
Az ebben az oktatóanyagban létrehozott eszköz adatokat küld az előre konfigurált [távoli figyelési][lnk-remote-monitoring] megoldásnak. Ha még nem építette ki az előre konfigurált távoli figyelési megoldást az Azure-fiókban, használja a következő lépéseket:

1. A <https://www.azureiotsuite.com/> oldalon kattintson a **+** elemre egy megoldás létrehozásához.
2. Kattintson a **Kiválasztás** elemre a **Távoli figyelés** panelen a megoldás létrehozásához.
3. A **Távoli figyelési megoldás létrehozása** oldalon írjon be egy **megoldásnevet**, válassza ki a **Régiót**, ahová üzembe szeretné helyezni azt, majd válassza ki a használni kívánt Azure-előfizetést. Ezután kattintson a **Megoldás létrehozása** parancsra.
4. Várja meg, amíg befejeződik a kiépítési folyamat.

> [!WARNING]
> Az előre konfigurált megoldások számlázható Azure-szolgáltatásokat használnak. A felesleges költségek elkerülése érdekében ügyeljen arra, hogy eltávolítsa az előre konfigurált megoldást az előfizetésből, amikor végzett annak használatával. Teljesen is eltávolíthatja az előre konfigurált megoldást az előfizetésből a <https://www.azureiotsuite.com/> oldalon.
> 
> 

A távoli figyelési megoldás kiépítésének befejezte után kattintson az **Indítás** gombra a megoldás irányítópultjának a böngészőben történő megnyitásához.

![A megoldás irányítópultja][img-dashboard]

### <a name="provision-your-device-in-the-remote-monitoring-solution"></a>Az eszköz kiépítése a távoli figyelési megoldásban
> [!NOTE]
> Ha már kiépített egy eszközt a megoldásában, kihagyhatja ezt a lépést. Ismernie kell az eszköz hitelesítő adatait az ügyfélalkalmazás létrehozásakor.
> 
> 

Ahhoz, hogy egy eszköz előre konfigurált megoldáshoz csatlakozhasson, érvényes hitelesítő adatokkal kell azonosítania magát az IoT Hubon. A megoldás irányítópultjáról kérheti le az eszköz hitelesítő adatait. Az oktatóanyagban később megadja az eszköz hitelesítő adatait az ügyfélalkalmazásban.

Ha eszközt szeretne hozzáadni a távoli figyelési megoldáshoz, végezze el a következő lépéseket a megoldás irányítópultján:

1. Az irányítópult bal alsó részén kattintson az **Eszköz hozzáadása** parancsra.
   
   ![Eszköz hozzáadása][1]
2. Az **Egyéni eszköz** panelen kattintson az **Új hozzáadása** parancsra.
   
   ![Egyéni eszköz hozzáadása][2]
3. Válassza a **Meghatározom a saját eszközazonosítómat** elemet. Írjon be egy eszközazonosítót, például a **sajáteszköz** kifejezést, kattintson az **Azonosító ellenőrzése** parancsra, hogy meggyőződhessen arról, hogy a név még nincs használatban, majd kattintson a **Létrehozás** parancsra az eszköz kiépítéséhez.
   
   ![Eszközazonosító hozzáadása][3]
4. Jegyezze fel az eszköz hitelesítő adatait (az eszköz azonosítóját, az IoT Hub-eszköznevet és az eszköz kulcsát). Az ügyfélalkalmazásnak szüksége van ezekre az értékekre, hogy a távoli figyelési megoldáshoz csatlakozhasson. Ezután kattintson a **Done** (Kész) gombra.
   
    ![Eszköz hitelesítő adatainak megtekintése][4]
5. Válassza ki az eszközt a megoldás irányítópultján lévő eszközlistából. Ezután az **Eszköz részletei** panelen kattintson az **Eszköz engedélyezése** parancsra. Az eszköz mostantól **Fut** állapotú. A távoli figyelési megoldás ettől kezdve telemetriát fogadhat az eszközről és metódusokat hívhat meg az eszközön.

[img-dashboard]: ./media/iot-suite-connecting-devices-mbed/dashboard.png
[1]: ./media/iot-suite-connecting-devices-mbed/suite0.png
[2]: ./media/iot-suite-connecting-devices-mbed/suite1.png
[3]: ./media/iot-suite-connecting-devices-mbed/suite2.png
[4]: ./media/iot-suite-connecting-devices-mbed/suite3.png

[lnk-what-are-preconfig-solutions]: iot-suite-what-are-preconfigured-solutions.md
[lnk-remote-monitoring]: iot-suite-remote-monitoring-sample-walkthrough.md
[lnk-free-trial]: http://azure.microsoft.com/pricing/free-trial/

## <a name="build-and-run-the-c-sample-solution"></a>Hozza létre, és futtassa a C megoldást

A következő útmutatások mutatják be hoz való csatlakoztatásának lépéseit egy [mbed-kompatibilis Freescale FRDM-K64F] [ lnk-mbed-home] eszközt, hogy a távoli figyelési megoldást igényelnek.

### <a name="connect-the-mbed-device-to-your-network-and-desktop-machine"></a>Csatlakoztassa a mbed a hálózat- és asztali számítógépen

1. A hálózati Ethernet kábellel csatlakoztassa az mbed eszközt. Ez a lépés szükség, mert a mintaalkalmazás internet-hozzáférésre van szüksége.

1. Lásd: [Ismerkedés a mbed] [ lnk-mbed-getstarted] az mbed eszköz csatlakozni az asztali számítógép.

1. Ha az asztali számítógép Windows fut, lásd: [PC-konfigurációt] [ lnk-mbed-pcconnect] konfigurálja az mbed eszköz soros port elérését.

### <a name="create-an-mbed-project-and-import-the-sample-code"></a>Mbed projekt létrehozása és importálása a mintakód

Kövesse az alábbi lépéseket néhány mintakód hozzáadása mbed projektben. A távoli felügyeleti alapszintű projekt importálása, és módosítsa a projektet a MQTT protokoll használata helyett az AMQP protokoll. Jelenleg kell használnia a MQTT protokoll az IoT-központ az eszköz felügyeleti funkcióinak használatát.

1. A böngészőben nyissa meg a mbed.org [fejlesztői hely](https://developer.mbed.org/). Még nem regisztrált, megjelenik egy lehetőség, hogy hozzon létre egy fiókot (szabad). Ellenkező esetben jelentkezzen be a fiók hitelesítő adataival. Kattintson a **fordító** az oldal jobb felső sarkában található. Ez a művelet számos lehetőséget kínál, a *munkaterület* felületet.

1. Ellenőrizze, hogy a hardver platform használata az ablak jobb felső sarkában jelenik meg, vagy kattintson a jobb oldali sarokban válassza ki a hardver platformot ikonra.

1. Kattintson a **importálási** a főmenü. Kattintson a **kattintson ide az URL-cím importálásához**.
   
    ![Importálás mbed munkaterületet indítása][6]

1. Az előugró ablakban adja meg a hivatkozást a minta kód https://developer.mbed.org/users/AzureIoTClient/code/remote_monitoring/, majd kattintson az **importálási**.
   
    ![Importálás mintakódot mbed munkaterület][7]

1. A fordítóprogram mbed ablakban látható, hogy a projekt importálása is importálja a szalagtárak különböző. Néhány biztosított és az Azure IoT-csapat által fenntartott ([azureiot_common](https://developer.mbed.org/users/AzureIoTClient/code/azureiot_common/), [iothub_client](https://developer.mbed.org/users/AzureIoTClient/code/iothub_client/), [iothub_amqp_transport](https://developer.mbed.org/users/AzureIoTClient/code/iothub_amqp_transport/), [azure_uamqp](https://developer.mbed.org/users/AzureIoTClient/code/azure_uamqp/)), a mbed szalagtárak katalógus elérhető külső szalagtárak vannak.
   
    ![Mbed projekt megtekintése][8]

1. Az a **Program munkaterület**, kattintson a jobb gombbal a **IOT hubbal\_amqp\_átviteli** könyvtár, kattintson a **törlése**, és kattintson a  **OK** megerősítéséhez.

1. Az a **Program munkaterület**, kattintson a jobb gombbal a **azure\_amqp\_c** könyvtár, kattintson a **törlése**, és kattintson a **OK** megerősítéséhez.

1. Kattintson a jobb gombbal a **remote_monitoring** a projektre a **Program munkaterület**, jelölje be **importálási könyvtár**, majd jelölje be **az URL**.
   
    ![Indítsa el a mbed munkaterületet Típustár importálása][6]

1. Az előugró ablakban adja meg a hivatkozást a MQTT átviteli könyvtár https://developer.mbed.org/users/AzureIoTClient/code/iothub\_mqtt\_átviteli / kattintson **importálási**.
   
    ![Importálás szalagtár mbed munkaterület][12]

1. Ismételje az előző lépést a MQTT könyvtár hozzáadása a https://developer.mbed.org/users/AzureIoTClient/code/azure\_umqtt\_c /.

1. A munkaterület most a következőképpen néznek:

    ![Mbed munkaterület megtekintése][13]

1. Nyissa meg a távoli\_monitoring\remote_monitoring.c fájlt, és cserélje le a meglévő `#include` utasítások a következő kóddal:

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
1. Törölje a fennmaradó kódot a távoli\_monitoring\remote\_monitoring.c fájlt.

[!INCLUDE [iot-suite-connecting-code](../../includes/iot-suite-connecting-code.md)]

## <a name="build-and-run-the-sample"></a>Hozza létre, és futtathatja a

Adja hozzá a meghívni kívánt kódot a **távoli\_figyelési\_futtatása** működéséhez majd összeállítása, és futtassa az alkalmazást.

1. Adja hozzá a **fő** függvényt a következő kódot a távoli végén\_monitoring.c fájl meghívni a **távoli\_figyelési\_futtassa** függvény:
   
    ```c
    int main()
    {
      remote_monitoring_run();
      return 0;
    }
    ```

1. Kattintson a **fordítási** hozhat létre a program. Nyugodtan figyelmen kívül hagyhatja a figyelmeztetéseket, de ha a build hoz létre a hibát, javítsa ki a folytatás előtt.

1. Ha a build sikeres, a mbed fordító webhely állít elő, a projekt neve .bin fájlt, és letölti a helyi számítógépre. Másolja a .bin fájlt az eszközre. Az eszköz újraindításához, és futtassa a programot a .bin fájl tartalmazza a .bin fájl mentése az eszköz okozza. Később manuálisan is újraindíthatja a program bármikor az mbed eszközön a alaphelyzetbe állítása gomb lenyomásával.

1. Az eszköz egy SSH ügyfélalkalmazást, mint a PuTTY használatával kapcsolódni. Azt is meghatározhatja a soros port, az eszköz használ a Windows ellenőrzésével.
   
    ![][11]

1. A PuTTY, kattintson a **soros** kapcsolattípus. Az eszköz általában 9600 átviteli csatlakozik, ezért adja meg a 9600 a **sebesség** mezőbe. Kattintson a **nyitott**.

1. A program végrehajtásának elkezdése. Lehet, hogy a Alaphelyzet (nyomja meg a CTRL + Break vagy a tábla alaphelyzetbe állítása gomb megnyomása) Ha a program nem indul el automatikusan csatlakoztatásakor.
   
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
