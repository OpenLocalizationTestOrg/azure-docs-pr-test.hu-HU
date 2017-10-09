## <a name="create-a-device-identity"></a>Eszközidentitás létrehozása

Ebben a szakaszban egy Node.js nevű eszközt használhat [IOT hubbal-explorer] [ iot-hub-explorer] toocreate ebben az oktatóanyagban egy eszközidentitás. Az eszközazonosítókban különbözőnek számítanak a kis- és nagybetűk.

1. Futtassa a parancssori környezetben hello következő:

    `npm install -g iothub-explorer@latest`

1. Ezután futtassa a következő parancs toologin tooyour hub hello. Helyettesítő `{iot hub connection string}` a korábban kimásolt kapcsolati karakterláncot az IoT-központ hello:

    `iothub-explorer login "{iot hub connection string}"`

1. Végezetül hozza létre egy új eszközidentitás nevű `myDeviceId` hello paranccsal:

    `iothub-explorer create myDeviceId --connection-string`

   [!INCLUDE [iot-hub-pii-note-naming-device](iot-hub-pii-note-naming-device.md)]

Jegyezze fel a hello eszköz kapcsolati karakterlánc az hello eredményből. Az eszköz kapcsolati karakterlánc hello eszköz alkalmazás tooconnect tooyour IoT-központ eszközként használja.

![][img-identity]

Tekintse meg a túl[Ismerkedés az IoT-központ] [ lnk-getstarted] tooprogrammatically eszköz identitások létrehozása.

<!-- images and links -->
[img-identity]: media/iot-hub-get-started-create-device-identity/devidentity.png

[iot-hub-explorer]: https://github.com/Azure/iothub-explorer/blob/master/readme.md

[lnk-getstarted]: ../articles/iot-hub/iot-hub-csharp-csharp-getstarted.md
