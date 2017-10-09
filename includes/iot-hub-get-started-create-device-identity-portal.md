## <a name="create-a-device-identity"></a>Eszközidentitás létrehozása

Ebben a szakaszban használhatja az hello [Azure-portálon] [ lnk-azure-portal] toocreate hello identitás rendszerleíró adatbázis az IoT hub egy eszközidentitás. Egy eszköz nem lehet kapcsolódni a tooIoT hub, kivéve, ha azt egy bejegyzéssel rendelkezik hello identitásjegyzékhez. További információ a hello hello "Identitásjegyzékhez" című szakaszában talál [IoT Hub fejlesztői útmutató][lnk-devguide-identity]. Hello **eszköz Explorer** hello a portál segítségével hozzon létre egy eszköz egyedi azonosítója és kulcsa, hogy az eszköz használhatja magát tooidentify tooIoT Hub kapcsolódáskor. Az eszközazonosítókban különbözőnek számítanak a kis- és nagybetűk.

1. Győződjön meg arról, hogy be van jelentkezve toohello [Azure-portálon][lnk-azure-portal].

1. Hello Ugrósávon, kattintson **összes erőforrás** és az IoT hub erőforrás található.

    ![Keresse meg a tooyour Iot-központ][img-find-iothub]

1. Ha az IoT hub erőforrás már meg van nyitva, kattintson a hello **eszköz Explorer** eszközzel, és kattintson a **Hozzáadás** hello tetején. Adja meg, mint az új eszköz hello nevét **myDeviceId**, és kattintson a **mentése**.

    ![Az eszközazonosító létrehozása a portálon][img-create-device]

   Ezzel létrehoz egy új eszközidentitás az IoT hub.

   [!INCLUDE [iot-hub-pii-note-naming-device](iot-hub-pii-note-naming-device.md)]

1. A hello **eszköz Explorer**tartozó eszközök listája, kattintson az újonnan létrehozott hello eszközre, és jegyezze fel a hello **kapcsolati karakterlánc---elsődleges kulcs**. 

    ![Eszköz kapcsolati karakterlánc][img-connection-string]

> [!NOTE]
> az IoT-központ identitásjegyzékhez hello csak eszköz identitások tooenable biztonságos hozzáférést toohello IoT-központ tárolja. Eszköz azonosítók és kulcsok toouse hitelesítő adatokat, valamint az engedélyezett vagy letiltott jelző használható toodisable hozzáférést egy adott eszköz tárol. Ha az alkalmazásnak toostore más eszközre vonatkozó metaadatok, akkor az alkalmazás-specifikus tárolási használjon. További információkért lásd az [Azure IoT Hub fejlesztői útmutatóját][lnk-devguide-identity].

<!-- Images. -->
[img-find-iothub]: ./media/iot-hub-get-started-create-device-identity-portal/find-iothub.png
[img-create-device]: ./media/iot-hub-get-started-create-device-identity-portal/create-identity-portal.png
[img-connection-string]: ./media/iot-hub-get-started-create-device-identity-portal/device-connection-string.png


<!-- Links -->
[lnk-azure-portal]: https://portal.azure.com
[lnk-devguide-identity]: ../articles/iot-hub/iot-hub-devguide-identity-registry.md

