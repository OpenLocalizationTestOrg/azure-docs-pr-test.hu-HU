---
title: "Azure IoT Hub eszköz-a-felhőbe küldött üzeneteket (Java) aaaProcess |} Microsoft Docs"
description: "Hogyan tooprocess IoT-központ eszközről a felhőbe üzenetek útválasztási szabályokat és egyéni végpontokat toodispatch üzenetek tooother háttér-szolgáltatásaihoz."
services: iot-hub
documentationcenter: java
author: dominicbetts
manager: timlt
editor: 
ms.assetid: bd9af5f9-a740-4780-a2a6-8c0e2752cf48
ms.service: iot-hub
ms.devlang: java
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 06/29/2017
ms.author: dobett
ms.openlocfilehash: 084e84e721ca4297c4d7d6cb06a43b0bed9bce85
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="process-iot-hub-device-to-cloud-messages-java"></a>Az IoT-központ eszközről a felhőbe üzenetek (Java)

[!INCLUDE [iot-hub-selector-process-d2c](../../includes/iot-hub-selector-process-d2c.md)]

Az Azure IoT Hub egy teljes körűen felügyelt szolgáltatás, amely lehetővé teszi a megbízható és biztonságos kétirányú kommunikációs eszközök millióira és a megoldás közötti háttér. Egyéb oktatóanyagok ([Ismerkedés az IoT-központ] és [IoT Hub-felhő eszközre üzenetek][lnk-c2d]) bemutatják, hogyan toouse hello alapvető eszköz-felhő és a felhő-eszköz az IoT-központ üzenetküldési működését.

Ez az oktatóanyag épít, hello hello kód [Ismerkedés az IoT-központ] oktatóanyag, és bemutatja, hogyan toouse üzenet skálázható módon útválasztási tooprocess eszközről a felhőbe üzenetek. hello az oktatóanyag bemutatja, hogyan tooprocess üzenetek hello megoldásból azonnali beavatkozást igénylő háttér. Például egy eszköz előfordulhat, hogy küldése riasztás, amely elindítja a jegy beszúrása egy CRM-rendszerbe. Adatpont üzenetek ezzel szemben, egyszerűen hírcsatorna be egy elemzés. Például hőmérséklet telemetriai adatokat egy eszközről, amely tárolja a későbbi elemzési toobe az egy adatpont üzenet.

Ez az oktatóanyag végén hello három Java konzol alkalmazások futtatása:

* **Szimulált eszköz**, hello alkalmazás hello létrehozott egy módosított verziója [Ismerkedés az IoT-központ] oktatóanyagban adatpont eszközről a felhőbe üzeneteket küld másodpercenként, és interaktív eszközre felhő minden 10 üzenetek másodperc. Ez az alkalmazás az IoT-központ hello AMQP protokoll toocommunicate használ.
* **olvasási-d2c-üzenetek** jeleníti meg az eszköz alkalmazás által küldött hello telemetriai adatokat.
* **olvasási kritikus várólista-** távolít el kritikus köszönőüzenetei a hello Service Bus csatolt várólista toohello IoT-központot.

> [!NOTE]
> Az IoT-központ rendelkezik sok eszköz platformok és nyelvek, beleértve a C, Java és JavaScript SDK támogatása. Hogyan tooreplace hello eszköz ebben az oktatóanyagban egy fizikai eszközzel kapcsolatos utasításokat és hogyan tooconnect eszközök tooan IoT Hub: hello [Azure IoT fejlesztői központ].

toocomplete ebben az oktatóanyagban a következő hello szüksége:

* A teljes működő verziójához hello [Ismerkedés az IoT-központ] oktatóanyag.
* hello legújabb [Java használata Development Kit 8](http://www.oracle.com/technetwork/java/javase/downloads/jdk8-downloads-2133151.html)
* [Maven 3](https://maven.apache.org/install.html)
* Aktív Azure-fiók. (Ha nincs fiókja, létrehozhat egy [ingyenes fiókot] [lnk-ingyenes] néhány perc alatt.)

Néhány alapvető ismerete szükséges [Azure Storage] és [Azure Service Bus].

## <a name="send-interactive-messages-from-a-device-app"></a>Interaktív üzenetek küldése egy eszköz alkalmazásból
Ebben a szakaszban létrehozott hello hello eszközalkalmazás módosítása [Ismerkedés az IoT-központ] oktatóanyag toooccasionally azonnali feldolgozást igénylő üzenetek küldése.

1. Szerkesztő tooopen hello simulated-device\src\main\java\com\mycompany\app\App.java szövegfájl használata. Ez a fájl tartalmazza a hello hello kódját **szimulált eszköz** hello létrehozott alkalmazás [Ismerkedés az IoT-központ] oktatóanyag.

2. Cserélje le a hello **MessageSender** hello kód a következő osztályra:

    ```java
    private static class MessageSender implements Runnable {

        public void run()  {
            try {
                double minTemperature = 20;
                double minHumidity = 60;
                Random rand = new Random();

                while (true) {
                    String msgStr;
                    Message msg;
                    if (new Random().nextDouble() > 0.7) {
                        msgStr = "This is a critical message.";
                        msg = new Message(msgStr);
                        msg.setProperty("level", "critical");
                    } else {
                        double currentTemperature = minTemperature + rand.nextDouble() * 15;
                        double currentHumidity = minHumidity + rand.nextDouble() * 20; 
                        TelemetryDataPoint telemetryDataPoint = new TelemetryDataPoint();
                        telemetryDataPoint.deviceId = deviceId;
                        telemetryDataPoint.temperature = currentTemperature;
                        telemetryDataPoint.humidity = currentHumidity;

                        msgStr = telemetryDataPoint.serialize();
                        msg = new Message(msgStr);
                    }
                    
                    System.out.println("Sending: " + msgStr);

                    Object lockobj = new Object();
                    EventCallback callback = new EventCallback();
                    client.sendEventAsync(msg, callback, lockobj);

                    synchronized (lockobj) {
                        lockobj.wait();
                    }
                    Thread.sleep(1000);
                }
            } catch (InterruptedException e) {
                System.out.println("Finished.");
            }
        }
    }
    ```
   
    Ez a metódus véletlenszerűen ad hello tulajdonság `"level": "critical"` hello eszközét, ezzel szimulálja egy üzenetet, amely azonnali beavatkozást igényel a háttérkiszolgálók alkalmazás hello által küldött toomessages. hello alkalmazás információt átadja a hello üzenet tulajdonságai, ahelyett, hogy hello üzenet törzsében, úgy, hogy az IoT-központ megfelelő üzenet toohello hello üzenetcél irányíthatja.
   
   > [!NOTE]
   > Üzenet tulajdonságai tooroute üzenetek feldolgozásához, továbbá itt látható példa az toohello kiemelt útvonalra cold-elérési utat is több, különböző esetekre is használhatja.

2. Mentse és zárja be hello simulated-device\src\main\java\com\mycompany\app\App.java fájlt.

    > [!NOTE]
    > Hello szakét az egyszerűség Ez az oktatóanyag nem valósítja meg a bármely újrapróbálkozási házirendje. Az éles kódban, meg kell valósítania egy újrapróbálkozási házirendje, például az exponenciális leállítási hello MSDN-cikkben leírtak [átmeneti hiba kezelése].

3. toobuild hello **szimulált eszköz** app Maven, használatával hajtható végre a következő parancs parancssorba hello hello szimulált eszköz mappában hello:

    ```cmd/sh
    mvn clean package -DskipTests
    ```

## <a name="add-a-queue-tooyour-iot-hub-and-route-messages-tooit"></a>A várólista tooyour IoT hub és útvonal üzenetek tooit hozzáadása

Ebben a szakaszban hozzon létre egy Service Bus-üzenetsorba, csatlakoztassa tooyour IoT-központot, és konfigurálja az IoT hub toosend üzenetek toohello várólista hello jelenléte ügyfélkulcscsere-tulajdonságok alapján. Hogyan tooprocess a Service Bus-üzenetsorok érkező üzenetek kapcsolatos további információkért lásd: [Ismerkedés a várólisták][lnk-sb-queues-java].

1. Hozzon létre egy Service Bus-üzenetsorba, a [Ismerkedés a várólisták][lnk-sb-queues-java]. Jegyezze fel a hello névtér és a várólista nevét.

2. A hello Azure-portálon, nyissa meg az IoT hub, és kattintson a **végpontok**.

    ![Az IoT-központ végpontok][30]

3. A hello **végpontok** panelen kattintson a **hozzáadása** : hello felső tooadd a várólista tooyour IoT-központot. Név hello végpont **CriticalQueue** és hello legördülő listákat tooselect **Service Bus-üzenetsorba**, Service Bus-névtér, amelyben a várólista található hello és hello a várólista neve. Amikor elkészült, kattintson a **mentése** hello lap alján.

    ![A végpont hozzáadása][31]

4. Most kattintson **útvonalak** az IoT Hub a. Kattintson a **Hozzáadás** hello panel toocreate toohello csak várólistára meg üzeneteit útválasztási szabályainak hozzáadott hello tetején. Válassza ki **DeviceTelemetry** hello adatforrásként. Adja meg `level="critical"` hello feltételként, és válassza ki az előzőekben adott hozzá, útválasztási szabály végpont hello egyéni végpontként hello várólista. Amikor elkészült, kattintson a **mentése** hello lap alján.

    ![Egy útvonal hozzáadása][32]

    Ellenőrizze, hogy hello tartalék útvonal túl van-e állítva**ON**. Ez a beállítás akkor hello alapértelmezett beállítása az IoT-központ.

    ![Tartalék útvonal][33]

## <a name="optional-read-from-hello-queue-endpoint"></a>(Választható) Hello várólista végpont olvasásakor

Opcionálisan olvasható köszönőüzenetei hello várólista végpont a hello utasításait követve [Ismerkedés a várólisták][lnk-sb-queues-java]. Name hello app **olvasási kritikus várólista-**.

## <a name="run-hello-applications"></a>Hello alkalmazások futtatásához

Most már készen áll a toorun hello három alkalmazások áll.

1. toorun hello **d2c-üzenetek olvasása** alkalmazás, a parancssor vagy a rendszerhéj nyissa meg a toohello olvasás-d2c mappa, és hajtsa végre a következő parancs hello:

   ```cmd/sh
   mvn exec:java -Dexec.mainClass="com.mycompany.app.App"
   ```

   ![Futtassa a üzenetek d2c olvasása][readd2c]

2. toorun hello **olvasási kritikus várólista-** alkalmazás, a parancssor vagy a rendszerhéj nyissa meg a toohello olvasási kritikus várólista-mappa, és hajtsa végre a következő parancs hello:

   ```cmd/sh
   mvn exec:java -Dexec.mainClass="com.mycompany.app.App"
   ```
   
   ![Olvasási kritikus üzeneteihez futtatása][readqueue]

3. toorun hello **szimulált eszköz** alkalmazást, a parancssor vagy a rendszerhéj toohello szimulált eszköz mappában nyissa meg, és hajtsa végre a következő parancs hello:

   ```cmd/sh
   mvn exec:java -Dexec.mainClass="com.mycompany.app.App"
   ```
   
   ![Szimulált eszköz futtatása][simulateddevice]

## <a name="next-steps"></a>Következő lépések

Ebben az oktatóanyagban megtanulta, hogyan tooreliably mennyi funkcióval hello üzenet útválasztási IoT hub eszköz a felhőbe küldött üzeneteket.

Hello [hogyan toosend felhőből eszközre küldött üzenetek IoT-központ] [ lnk-c2d] bemutatja, hogyan toosend üzenetek tooyour eszközöket a megoldás háttérből.

teljes végpontok közötti megoldások, amelyek használják az IoT-központ toosee példát talál [Azure IoT Suite][lnk-suite].

További információ az IoT hubbal megoldások toolearn lásd: hello [IoT Hub fejlesztői útmutató].

toolearn üzenetet az IoT hubon útválasztási kapcsolatos további információkért lásd: [üzeneteket küldjön és fogadjon IoT hubbal][lnk-devguide-messaging].

<!-- Images. -->
<!-- TODO: UPDATE PICTURES -->
[simulateddevice]: ./media/iot-hub-java-java-process-d2c/runsimulateddevice.png
[readd2c]: ./media/iot-hub-java-java-process-d2c/runprocessinteractive.png
[readqueue]: ./media/iot-hub-java-java-process-d2c/runprocessd2c.png

[30]: ./media/iot-hub-java-java-process-d2c/click-endpoints.png
[31]: ./media/iot-hub-java-java-process-d2c/endpoint-creation.png
[32]: ./media/iot-hub-java-java-process-d2c/route-creation.png
[33]: ./media/iot-hub-java-java-process-d2c/fallback-route.png

<!-- Links -->

[lnk-sb-queues-java]: ../service-bus-messaging/service-bus-java-how-to-use-queues.md

[Azure Storage]: https://azure.microsoft.com/documentation/services/storage/
[Azure Service Bus]: https://azure.microsoft.com/documentation/services/service-bus/

[IoT Hub fejlesztői útmutató]: iot-hub-devguide.md
[lnk-devguide-messaging]: iot-hub-devguide-messaging.md
[Ismerkedés az IoT-központ]: iot-hub-java-java-getstarted.md
[Azure IoT fejlesztői központ]: https://azure.microsoft.com/develop/iot
[átmeneti hiba kezelése]: https://msdn.microsoft.com/library/hh675232.aspx

<!-- Links -->
[Átmeneti hiba kezelése]: https://msdn.microsoft.com/library/hh680901(v=pandp.50).aspx

[lnk-c2d]: iot-hub-java-java-c2d.md
[lnk-suite]: https://azure.microsoft.com/documentation/suites/iot-suite/
