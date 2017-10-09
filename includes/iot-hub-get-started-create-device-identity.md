## <a name="create-a-device-identity"></a><span data-ttu-id="547fe-101">Eszközidentitás létrehozása</span><span class="sxs-lookup"><span data-stu-id="547fe-101">Create a device identity</span></span>

<span data-ttu-id="547fe-102">Ebben a szakaszban egy Node.js nevű eszközt használhat [IOT hubbal-explorer] [ iot-hub-explorer] toocreate ebben az oktatóanyagban egy eszközidentitás.</span><span class="sxs-lookup"><span data-stu-id="547fe-102">In this section, you use a Node.js tool called [iothub-explorer][iot-hub-explorer] toocreate a device identity for this tutorial.</span></span> <span data-ttu-id="547fe-103">Az eszközazonosítókban különbözőnek számítanak a kis- és nagybetűk.</span><span class="sxs-lookup"><span data-stu-id="547fe-103">Device IDs are case sensitive.</span></span>

1. <span data-ttu-id="547fe-104">Futtassa a parancssori környezetben hello következő:</span><span class="sxs-lookup"><span data-stu-id="547fe-104">Run hello following in your command-line environment:</span></span>

    `npm install -g iothub-explorer@latest`

1. <span data-ttu-id="547fe-105">Ezután futtassa a következő parancs toologin tooyour hub hello.</span><span class="sxs-lookup"><span data-stu-id="547fe-105">Then, run hello following command toologin tooyour hub.</span></span> <span data-ttu-id="547fe-106">Helyettesítő `{iot hub connection string}` a korábban kimásolt kapcsolati karakterláncot az IoT-központ hello:</span><span class="sxs-lookup"><span data-stu-id="547fe-106">Substitute `{iot hub connection string}` with hello IoT Hub connection string you previously copied:</span></span>

    `iothub-explorer login "{iot hub connection string}"`

1. <span data-ttu-id="547fe-107">Végezetül hozza létre egy új eszközidentitás nevű `myDeviceId` hello paranccsal:</span><span class="sxs-lookup"><span data-stu-id="547fe-107">Finally, create a new device identity called `myDeviceId` with hello command:</span></span>

    `iothub-explorer create myDeviceId --connection-string`

   [!INCLUDE [iot-hub-pii-note-naming-device](iot-hub-pii-note-naming-device.md)]

<span data-ttu-id="547fe-108">Jegyezze fel a hello eszköz kapcsolati karakterlánc az hello eredményből.</span><span class="sxs-lookup"><span data-stu-id="547fe-108">Make a note of hello device connection string from hello result.</span></span> <span data-ttu-id="547fe-109">Az eszköz kapcsolati karakterlánc hello eszköz alkalmazás tooconnect tooyour IoT-központ eszközként használja.</span><span class="sxs-lookup"><span data-stu-id="547fe-109">This device connection string is used by hello device app tooconnect tooyour IoT Hub as a device.</span></span>

![][img-identity]

<span data-ttu-id="547fe-110">Tekintse meg a túl[Ismerkedés az IoT-központ] [ lnk-getstarted] tooprogrammatically eszköz identitások létrehozása.</span><span class="sxs-lookup"><span data-stu-id="547fe-110">Refer too[Getting started with IoT Hub][lnk-getstarted] tooprogrammatically create device identities.</span></span>

<!-- images and links -->
[img-identity]: media/iot-hub-get-started-create-device-identity/devidentity.png

[iot-hub-explorer]: https://github.com/Azure/iothub-explorer/blob/master/readme.md

[lnk-getstarted]: ../articles/iot-hub/iot-hub-csharp-csharp-getstarted.md
