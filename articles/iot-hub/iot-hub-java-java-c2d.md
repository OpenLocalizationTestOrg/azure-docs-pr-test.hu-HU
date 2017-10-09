---
title: "az Azure IoT Hub (Java) aaaCloud eszközre üzenetek |} Microsoft Docs"
description: "Hogyan toosend felhő eszköz üzenetek tooa eszköz regisztrációját az Azure IoT-központ Java hello Azure IoT SDK-k használatával. A szimulált eszköz alkalmazás tooreceive felhő eszközre üzeneteket módosítja, és módosítsa a háttér-alkalmazás toosend hello felhő eszközre üzeneteket."
services: iot-hub
documentationcenter: java
author: dominicbetts
manager: timlt
editor: 
ms.assetid: 7f785ea8-e7c2-40c5-87ef-96525e9b9e1e
ms.service: iot-hub
ms.devlang: java
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 06/28/2017
ms.author: dobett
ms.openlocfilehash: 8721f18428c849ed9a04aa2e45c65605c3e38101
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="send-cloud-to-device-messages-with-iot-hub-java"></a>Az IoT-központ (Java) felhő eszközre-üzenetek
[!INCLUDE [iot-hub-selector-c2d](../../includes/iot-hub-selector-c2d.md)]

Az Azure IoT Hub egy teljes körűen felügyelt szolgáltatás, amellyel eszközök millióira közötti megbízható és biztonságos kétirányú kommunikáció engedélyezése és a megoldás háttérrendszere. Hello [Ismerkedés az IoT-központ] az oktatóanyag bemutatja, hogyan toocreate az IoT-központ telepítéséhez azt egy eszközidentitás, és kód egy szimulált eszköz alkalmazást, amelyet az eszköz a felhőbe küldött üzeneteket küld.

Ez az oktatóanyag épül [Ismerkedés az IoT-központ]. Megmutatja, hogyan számára:

* A megoldás háttérrendszeréhez küld felhő-eszközre küldött üzenetek tooa egyetlen eszközt az IoT-központ keresztül.
* Az eszközön a felhőből eszközre üzeneteket fogadni.
* A megoldás háttérrendszeréhez, a kérelem kézbesítési nyugtázási (*visszajelzés*) az üzenetek küldése tooa eszközt az IoT-központot.

További információ a felhő-eszközre küldött üzenetek található hello [IoT Hub fejlesztői útmutató][IoT Hub developer guide - C2D].

Ez az oktatóanyag végén hello két Java konzol alkalmazások futtatása:

* **Szimulált eszköz**, hello alkalmazás létrehozott egy módosított verziója [Ismerkedés az IoT-központ], amely tooyour IoT-központ kapcsolódik, és felhő eszközre üzeneteket fogad.
* **küldési-c2d-üzenetek**, amely IoT-központot egy felhő-eszközre küldött üzenetek toohello szimulált eszköz alkalmazás küld, és majd megkapja a kézbesítési nyugtázási.

> [!NOTE]
> Az IoT-központ rendelkezik sok eszköz platformok és nyelvek (például C, Java és Javascript) keresztül Azure IoT eszközoldali SDK-k SDK támogatása. Hogyan tooconnect az eszköz toothis oktatóanyag kódot, és általában tooAzure IoT Hub: lépésenkénti hello [Azure IoT fejlesztői központ].

toocomplete ebben az oktatóanyagban a következő hello szüksége:

* A teljes működő verziójához hello [Ismerkedés az IoT-központ](iot-hub-java-java-getstarted.md) vagy [folyamat IoT Hub eszköz-a-felhőbe küldött üzeneteket](iot-hub-java-java-process-d2c.md) oktatóanyag.
* hello legújabb [Java használata Development Kit 8](http://www.oracle.com/technetwork/java/javase/downloads/jdk8-downloads-2133151.html)
* [Maven 3](https://maven.apache.org/install.html)
* Aktív Azure-fiók. (Ha nincs fiókja, létrehozhat egy [ingyenes fiókot][lnk-free-trial] néhány perc alatt.)

## <a name="receive-messages-in-hello-simulated-device-app"></a>A szimulált eszköz app hello üzeneteket fogadni

Ebben a szakaszban létrehozott hello szimulált eszköz alkalmazásának módosítása [Ismerkedés az IoT-központ] hello IoT-központ üzeneteit tooreceive felhő eszközre.

1. Egy szövegszerkesztőben nyissa meg hello simulated-device\src\main\java\com\mycompany\app\App.java fájlt.

2. Adja hozzá a következő hello **MessageCallback** osztályhoz tartozik, mint egy beágyazott osztály belsejében hello **App** osztály. Hello **hajtható végre** hello eszköz üzenetet kap, az IoT-központ metódus hív meg. Ebben a példában hello eszköz mindig értesíti hello IoT-központot, hogy befejeződött-e üdvözlőüzenetére:

    ```java
    private static class AppMessageCallback implements MessageCallback {
      public IotHubMessageResult execute(Message msg, Object context) {
        System.out.println("Received message from hub: "
          + new String(msg.getBytes(), Message.DEFAULT_IOTHUB_MESSAGE_CHARSET));
    
        return IotHubMessageResult.COMPLETE;
      }
    }
    ```
3. Módosítsa a hello **fő** metódus toocreate egy **AppMessageCallback** példány és hívás hello **setMessageCallback** módszert az alábbiak szerint hello ügyfél megnyitása előtt:

    ```java
    client = new DeviceClient(connString, protocol);
   
    MessageCallback callback = new AppMessageCallback();
    client.setMessageCallback(callback, null);
    client.open();
    ```

    > [!NOTE]
    > A HTTP használata helyett MQTT vagy AMQP hello átvitelhez, hello **DeviceClient** példány ellenőrzi az üzeneteket az IoT-központ ritkán (kevesebb mint 25 percenként). MQTT, amqp-t és HTTP-támogatás, és az IoT-központ sávszélesség-szabályozás hello különbségei kapcsolatos további információkért lásd: hello [IoT Hub fejlesztői útmutató][IoT Hub developer guide - C2D].

4. toobuild hello **szimulált eszköz** app Maven, használatával hajtható végre a következő parancs parancssorba hello hello szimulált eszköz mappában hello:

    ```cmd/sh
    mvn clean package -DskipTests
    ```

## <a name="send-a-cloud-to-device-message"></a>Felhő eszközre üzenet küldése

Ebben a szakaszban egy Java-Konzolalkalmazás által a felhő-eszközre küldött üzenetek toohello szimulált eszköz alkalmazást hoz létre. Eszközazonosító hello hozzáadott hello eszköz kell hello [Ismerkedés az IoT-központ] oktatóanyag. Akkor is kell hello IoT-központ kapcsolati karakterláncot a központ, amely az hello található [Azure-portálon].

1. Nevű Maven-projekt létrehozása **c2d-üzenetküldés** a következő parancsot a parancssorba hello segítségével. Megjegyzés: Ez a parancs egy egyetlen, hosszú parancsot:

    ```cmd/sh
    mvn archetype:generate -DgroupId=com.mycompany.app -DartifactId=send-c2d-messages -DarchetypeArtifactId=maven-archetype-quickstart -DinteractiveMode=false
    ```

2. A parancssorban lépjen a toohello új küldési-c2d-üzenetek mappa.

3. Egy szövegszerkesztőben nyissa hello pom.xml fájlt hello küldési-c2d-üzenetek mappában, és adja hozzá a következő függőségi toohello hello **függőségek** csomópont. Hello-függőség felvétele lehetővé teszi toouse hello **IOT hubbal-java-szolgáltatásügyfél** az alkalmazás toocommunicate az IoT hub szolgáltatással-csomagot:

    ```xml
    <dependency>
      <groupId>com.microsoft.azure.sdk.iot</groupId>
      <artifactId>iot-service-client</artifactId>
      <version>1.7.23</version>
    </dependency>
    ```

    > [!NOTE]
    > Ellenőrizze, hogy hello legújabb verziójának **iot-szolgáltatásügyfél** használatával [Maven keresési][lnk-maven-service-search].

4. Mentse és zárja be a hello pom.xml fájlt.

5. Egy szövegszerkesztőben nyissa meg hello send-c2d-messages\src\main\java\com\mycompany\app\App.java fájlt.

6. Adja hozzá a következő hello **importálása** utasítások toohello fájlt:

    ```java
    import com.microsoft.azure.sdk.iot.service.*;
    import java.io.IOException;
    import java.net.URISyntaxException;
    ```

7. Adja hozzá a következő osztály változók toohello hello **App** osztály cseréje **{yourhubconnectionstring}** és **{yourdeviceid}** hello értékeit a beállításértékeket korábban:

    ```java
    private static final String connectionString = "{yourhubconnectionstring}";
    private static final String deviceId = "{yourdeviceid}";
    private static final IotHubServiceClientProtocol protocol = IotHubServiceClientProtocol.AMQPS;
    ```

8. Cserélje le a hello **fő** hello kód a következő metódust. Ez a kód tooyour IoT-központ csatlakozik, küld egy üzenet tooyour eszköz, valamint a majd megvárja-e a nyugtázás hello eszköz fogadott és a feldolgozott hello üzenet:
   
    ```java
    public static void main(String[] args) throws IOException,
        URISyntaxException, Exception {
      ServiceClient serviceClient = ServiceClient.createFromConnectionString(
        connectionString, protocol);
   
      if (serviceClient != null) {
        serviceClient.open();
        FeedbackReceiver feedbackReceiver = serviceClient
          .getFeedbackReceiver();
        if (feedbackReceiver != null) feedbackReceiver.open();
   
        Message messageToSend = new Message("Cloud toodevice message.");
        messageToSend.setDeliveryAcknowledgement(DeliveryAcknowledgement.Full);
   
        serviceClient.send(deviceId, messageToSend);
        System.out.println("Message sent toodevice");
   
        FeedbackBatch feedbackBatch = feedbackReceiver.receive(10000);
        if (feedbackBatch != null) {
          System.out.println("Message feedback received, feedback time: "
            + feedbackBatch.getEnqueuedTimeUtc().toString());
        }
   
        if (feedbackReceiver != null) feedbackReceiver.close();
        serviceClient.close();
      }
    }
    ```

    > [!NOTE]
    > Az egyszerűség kedvéért tartozó szakét Ez az oktatóanyag nem valósítja meg a bármely újrapróbálkozási házirendje. Az éles kódban, meg kell valósítania újrapróbálkozási házirendek (például az exponenciális leállítási), hello MSDN-cikkben leírtak [átmeneti hiba kezelése].


9. toobuild hello **szimulált eszköz** app Maven, használatával hajtható végre a következő parancs parancssorba hello hello szimulált eszköz mappában hello:

    ```cmd/sh
    mvn clean package -DskipTests
    ```

## <a name="run-hello-applications"></a>Hello alkalmazások futtatásához

Most már áll készen toorun hello alkalmazások.

1. Parancsot egy parancssorba hello szimulált eszköz mappában futtassa a következő parancs toobegin telemetriai tooyour IoT hub és toolisten a hubról felhő eszközre üzeneteket küld hello:

    ```cmd/sh
    mvn exec:java -Dexec.mainClass="com.mycompany.app.App" 
    ```

    ![Hello szimulált eszköz alkalmazás futtatása][img-simulated-device]

2. Parancssorba hello küldési-c2d-üzenetek mappában futtassa a következő parancs toosend hello egy felhő-eszközre küldött üzenetek és a visszajelzés visszaigazolás Várakozás:

    ```cmd/sh
    mvn exec:java -Dexec.mainClass="com.mycompany.app.App"
    ```

    ![Hello felhő eszközre parancs toosend üdvözlőüzenetére futtatása][img-send-command]

## <a name="next-steps"></a>Következő lépések

Ebben az oktatóanyagban, megtudta, hogyan toosend és felhő-eszközre küldött üzenetek fogadására. 

teljes végpontok közötti megoldások, amelyek használják az IoT-központ toosee példát talál [Azure IoT Suite].

További információ az IoT hubbal megoldások toolearn lásd: hello [IoT Hub fejlesztői útmutató].

<!-- Images -->
[img-simulated-device]: media/iot-hub-java-java-c2d/receivec2d.png
[img-send-command]:  media/iot-hub-java-java-c2d/sendc2d.png
<!-- Links -->

[Ismerkedés az IoT-központ]: iot-hub-java-java-getstarted.md
[IoT Hub developer guide - C2D]: iot-hub-devguide-messaging.md
[IoT Hub fejlesztői útmutató]: iot-hub-devguide.md
[Azure IoT fejlesztői központ]: http://www.azure.com/develop/iot
[lnk-free-trial]: http://azure.microsoft.com/pricing/free-trial/
[lnk-dev-setup]: https://github.com/Azure/azure-iot-sdk-java
[átmeneti hiba kezelése]: https://msdn.microsoft.com/library/hh680901(v=pandp.50).aspx
[Azure-portálon]: https://portal.azure.com
[Azure IoT Suite]: https://azure.microsoft.com/documentation/suites/iot-suite/
[lnk-maven-service-search]: http://search.maven.org/#search%7Cga%7C1%7Ca%3A%22iot-service-client%22%20g%3A%22com.microsoft.azure.sdk.iot%22