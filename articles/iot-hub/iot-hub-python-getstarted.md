---
title: "aaaGet Azure IoT Hub (Python) használatába |} Microsoft Docs"
description: "Ismerje meg, hogyan toosend eszköz-felhő üzenetek tooAzure IoT Hub Python IoT SDK-k használatával. Szimulált eszköz és a szolgáltatás alkalmazások tooregister létrehozása az eszköz, üzenetküldés és üzenetek olvasni az IoT-központ."
services: iot-hub
author: dsk-2015
manager: timlt
editor: 
ms.service: iot-hub
ms.devlang: python
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 08/25/2017
ms.author: dkshir
ms.custom: na
ms.openlocfilehash: aa23e792fb144202e121274723bcfaeae0c04723
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="connect-your-simulated-device-tooyour-iot-hub-using-python"></a><span data-ttu-id="2f2ea-104">Csatlakoztassa a szimulált eszköz tooyour IoT hubot pythonos környezetekben</span><span class="sxs-lookup"><span data-stu-id="2f2ea-104">Connect your simulated device tooyour IoT hub using Python</span></span>
[!INCLUDE [iot-hub-selector-get-started](../../includes/iot-hub-selector-get-started.md)]

<span data-ttu-id="2f2ea-105">Ez az oktatóanyag végén hello hogy két Python-alkalmazások:</span><span class="sxs-lookup"><span data-stu-id="2f2ea-105">At hello end of this tutorial, you will have two Python apps:</span></span>

* <span data-ttu-id="2f2ea-106">**CreateDeviceIdentity.py**, ami létrehoz egy eszközidentitás, és a biztonsági kulcsok tooconnect a szimulált eszköz alkalmazás.</span><span class="sxs-lookup"><span data-stu-id="2f2ea-106">**CreateDeviceIdentity.py**, which creates a device identity and associated security key tooconnect your simulated device app.</span></span>
* <span data-ttu-id="2f2ea-107">**SimulatedDevice.py**, IoT-központ tooyour köti össze a korábban létrehozott hello eszközidentitás, és rendszeres időközönként telemetriai adatokat küld üzenet hello MQTT protokoll használatával.</span><span class="sxs-lookup"><span data-stu-id="2f2ea-107">**SimulatedDevice.py**, which connects tooyour IoT hub with hello device identity created earlier, and periodically sends a telemetry message using hello MQTT protocol.</span></span>

> [!NOTE]
> <span data-ttu-id="2f2ea-108">hello cikk [Azure IoT SDK-k] [ lnk-hub-sdks] használható toobuild mindkét alkalmazások toorun eszközökön és a megoldás háttérrendszeréhez hello Azure IoT SDK-k információt nyújt.</span><span class="sxs-lookup"><span data-stu-id="2f2ea-108">hello article [Azure IoT SDKs][lnk-hub-sdks] provides information about hello Azure IoT SDKs that you can use toobuild both applications toorun on devices and your solution back end.</span></span>
> 
> 

<span data-ttu-id="2f2ea-109">toocomplete ebben az oktatóanyagban a következő hello szüksége:</span><span class="sxs-lookup"><span data-stu-id="2f2ea-109">toocomplete this tutorial, you need hello following:</span></span>

* <span data-ttu-id="2f2ea-110">[Python 2.x vagy 3.x][lnk-python-download].</span><span class="sxs-lookup"><span data-stu-id="2f2ea-110">[Python 2.x or 3.x][lnk-python-download].</span></span> <span data-ttu-id="2f2ea-111">Győződjön meg arról, hogy toouse hello 32 bites vagy 64 bites telepítési a telepítő szükség szerint.</span><span class="sxs-lookup"><span data-stu-id="2f2ea-111">Make sure toouse hello 32-bit or 64-bit installation as required by your setup.</span></span> <span data-ttu-id="2f2ea-112">Amikor a rendszer kéri, hello telepítése során, győződjön meg arról, hogy tooadd Python tooyour platform-specifikus környezeti változó.</span><span class="sxs-lookup"><span data-stu-id="2f2ea-112">When prompted during hello installation, make sure tooadd Python tooyour platform-specific environment variable.</span></span> <span data-ttu-id="2f2ea-113">Python használata 2.x, szükség lehet a túl[telepítené vagy frissítené a *pip*, hello Python-csomag felügyeleti rendszer][lnk-install-pip].</span><span class="sxs-lookup"><span data-stu-id="2f2ea-113">If you are using Python 2.x, you may need too[install or upgrade *pip*, hello Python package management system][lnk-install-pip].</span></span>
* <span data-ttu-id="2f2ea-114">Ha Windows operációs rendszer, majd használja [Visual C++ terjeszthető változatát tartalmazó csomagot] [ lnk-visual-c-redist] tooallow hello használata natív Python DLL-eket.</span><span class="sxs-lookup"><span data-stu-id="2f2ea-114">If you are using Windows OS, then [Visual C++ redistributable package][lnk-visual-c-redist] tooallow hello use of native DLLs from Python.</span></span>
* <span data-ttu-id="2f2ea-115">[Node.js 4.0 vagy újabb][lnk-node-download].</span><span class="sxs-lookup"><span data-stu-id="2f2ea-115">[Node.js 4.0 or later][lnk-node-download].</span></span> <span data-ttu-id="2f2ea-116">Győződjön meg arról, hogy toouse hello 32 bites vagy 64 bites telepítési a telepítő szükség szerint.</span><span class="sxs-lookup"><span data-stu-id="2f2ea-116">Make sure toouse hello 32-bit or 64-bit installation as required by your setup.</span></span> <span data-ttu-id="2f2ea-117">Ez a szükséges tooinstall hello [IoT Hub Explorer eszköz][lnk-iot-hub-explorer].</span><span class="sxs-lookup"><span data-stu-id="2f2ea-117">This is needed tooinstall hello [IoT Hub Explorer tool][lnk-iot-hub-explorer].</span></span>
* <span data-ttu-id="2f2ea-118">Aktív Azure-fiók.</span><span class="sxs-lookup"><span data-stu-id="2f2ea-118">An active Azure account.</span></span> <span data-ttu-id="2f2ea-119">Ha nincs fiókja, néhány perc alatt létrehozhat egy [ingyenes fiókot][lnk-free-trial].</span><span class="sxs-lookup"><span data-stu-id="2f2ea-119">If you don't have an account, you can create a [free account][lnk-free-trial] in just a couple of minutes.</span></span>

> [!NOTE]
> <span data-ttu-id="2f2ea-120">Hello *pip* a csomagok `azure-iothub-service-client` és `azure-iothub-device-client` jelenleg csak a Windows operációs rendszer elérhető.</span><span class="sxs-lookup"><span data-stu-id="2f2ea-120">hello *pip* packages for `azure-iothub-service-client` and `azure-iothub-device-client` are currently available only for Windows OS.</span></span> <span data-ttu-id="2f2ea-121">A Linux vagy Mac OS, tekintse meg az toohello Linux és Mac OS-specifikus szakaszokat hello [a fejlesztési környezet előkészítése a Python] [ lnk-python-devbox] utáni.</span><span class="sxs-lookup"><span data-stu-id="2f2ea-121">For Linux/Mac OS, please refer toohello Linux and Mac OS-specific sections on hello [Prepare your development environment for Python][lnk-python-devbox] post.</span></span>
> 

[!INCLUDE [iot-hub-get-started-create-hub](../../includes/iot-hub-get-started-create-hub.md)]

<span data-ttu-id="2f2ea-122">Ezzel létrehozta az IoT Hubot.</span><span class="sxs-lookup"><span data-stu-id="2f2ea-122">You have now created your IoT hub.</span></span> <span data-ttu-id="2f2ea-123">Hello rest oktatóanyag hello IoT-központ állomásnév és az IoT-központ kapcsolati karakterlánc hello használja.</span><span class="sxs-lookup"><span data-stu-id="2f2ea-123">Use hello IoT Hub host name and hello IoT Hub connection string in hello rest of this tutorial.</span></span>

> [!NOTE]
> <span data-ttu-id="2f2ea-124">Könnyen is létrehozhat az IoT hub a parancssorban, hello Python segítségével, vagy a Node.js-alapú Azure parancssori felület.</span><span class="sxs-lookup"><span data-stu-id="2f2ea-124">You can also easily create your IoT hub on a command line, using hello Python or Node.js based Azure CLI.</span></span> <span data-ttu-id="2f2ea-125">hello cikk [létrehoz egy IoT-központot hello Azure CLI 2.0 használatával] [ lnk-azure-cli-hub] Gyorsműveletek toodo így hello jeleníti meg.</span><span class="sxs-lookup"><span data-stu-id="2f2ea-125">hello article [Create an IoT hub using hello Azure CLI 2.0][lnk-azure-cli-hub] shows you hello quick steps toodo so.</span></span> 
> 

## <a name="create-a-device-identity"></a><span data-ttu-id="2f2ea-126">Eszközidentitás létrehozása</span><span class="sxs-lookup"><span data-stu-id="2f2ea-126">Create a device identity</span></span>
<span data-ttu-id="2f2ea-127">Ez a szakasz hello lépéseket toocreate egy Python-Konzolalkalmazás, az IoT hub hello identitásjegyzékhez egy eszközidentitás létrehozó.</span><span class="sxs-lookup"><span data-stu-id="2f2ea-127">This section lists hello steps toocreate a Python console app, that creates a device identity in hello identity registry of your IoT hub.</span></span> <span data-ttu-id="2f2ea-128">Egy eszköz csak kapcsolódhatnak tooIoT Hub-e azt egy bejegyzést a hello identitásjegyzékhez.</span><span class="sxs-lookup"><span data-stu-id="2f2ea-128">A device can only connect tooIoT Hub if it has an entry in hello identity registry.</span></span> <span data-ttu-id="2f2ea-129">További információkért lásd: hello **Identitásjegyzékhez** hello szakasza [IoT Hub fejlesztői útmutató][lnk-devguide-identity].</span><span class="sxs-lookup"><span data-stu-id="2f2ea-129">For more information, see hello **Identity Registry** section of hello [IoT Hub developer guide][lnk-devguide-identity].</span></span> <span data-ttu-id="2f2ea-130">A konzol alkalmazás futtatásakor egy eszköz egyedi Azonosítót hoz létre, és az, hogy az eszköz használhatja tooidentify magát, eszköz-felhő küld a kulcs üzenetek tooIoT Hub.</span><span class="sxs-lookup"><span data-stu-id="2f2ea-130">When you run this console app, it generates a unique device ID and key that your device can use tooidentify itself when it sends device-to-cloud messages tooIoT Hub.</span></span>

1. <span data-ttu-id="2f2ea-131">Nyisson meg egy parancssort, és telepítse a hello **Azure IoT Hub szolgáltatás SDK for Python**.</span><span class="sxs-lookup"><span data-stu-id="2f2ea-131">Open a command prompt and install hello **Azure IoT Hub Service SDK for Python**.</span></span> <span data-ttu-id="2f2ea-132">Zárja be a parancssort hello hello SDK telepítése után.</span><span class="sxs-lookup"><span data-stu-id="2f2ea-132">Close hello command prompt after you install hello SDK.</span></span>

    ```
    pip install azure-iothub-service-client
    ```

2. <span data-ttu-id="2f2ea-133">Hozzon létre egy Python-fájlt **CreateDeviceIdentity.py** néven.</span><span class="sxs-lookup"><span data-stu-id="2f2ea-133">Create a Python file named **CreateDeviceIdentity.py**.</span></span> <span data-ttu-id="2f2ea-134">Nyissa meg a [egy Python szerkesztő/IDE az Ön által választott][lnk-python-ide-list], például az alapértelmezett hello [ÜRESJÁRATBAN][lnk-idle].</span><span class="sxs-lookup"><span data-stu-id="2f2ea-134">Open it in [a Python editor/IDE of your choice][lnk-python-ide-list], for example, hello default [IDLE][lnk-idle].</span></span>

3. <span data-ttu-id="2f2ea-135">Adja hozzá a hello tooimport szükséges hello kódmodulok hello SDK szolgáltatástól a következő:</span><span class="sxs-lookup"><span data-stu-id="2f2ea-135">Add hello following code tooimport hello required modules from hello service SDK:</span></span>

    ```python
    import sys
    import iothub_service_client
    from iothub_service_client import IoTHubRegistryManager, IoTHubRegistryManagerAuthMethod
    from iothub_service_client import IoTHubDeviceStatus, IoTHubError
    ```
2. <span data-ttu-id="2f2ea-136">Adja hozzá a következő kódot, cseréje hello helyőrzője hello `[IoTHub Connection String]` hello IoT hub hello előző szakaszban létrehozott hello kapcsolati karakterlánccal.</span><span class="sxs-lookup"><span data-stu-id="2f2ea-136">Add hello following code, replacing hello placeholder for `[IoTHub Connection String]` with hello connection string for hello IoT hub you created in hello previous section.</span></span> <span data-ttu-id="2f2ea-137">Egyetlen nevét használják hello `DEVICE_ID`.</span><span class="sxs-lookup"><span data-stu-id="2f2ea-137">You can use any name as hello `DEVICE_ID`.</span></span>
   
    ```python
    CONNECTION_STRING = "[IoTHub Connection String]"
    DEVICE_ID = "MyFirstPythonDevice"
    ```
   [!INCLUDE [iot-hub-pii-note-naming-device](../../includes/iot-hub-pii-note-naming-device.md)]

3. <span data-ttu-id="2f2ea-138">Adja hozzá hello függvény tooprint következő hello eszköz adatok egy részét.</span><span class="sxs-lookup"><span data-stu-id="2f2ea-138">Add hello following function tooprint some of hello device information.</span></span>

    ```python
    def print_device_info(title, iothub_device):
        print ( title + ":" )
        print ( "iothubDevice.deviceId                    = {0}".format(iothub_device.deviceId) )
        print ( "iothubDevice.primaryKey                  = {0}".format(iothub_device.primaryKey) )
        print ( "iothubDevice.secondaryKey                = {0}".format(iothub_device.secondaryKey) )
        print ( "iothubDevice.connectionState             = {0}".format(iothub_device.connectionState) )
        print ( "iothubDevice.status                      = {0}".format(iothub_device.status) )
        print ( "iothubDevice.lastActivityTime            = {0}".format(iothub_device.lastActivityTime) )
        print ( "iothubDevice.cloudToDeviceMessageCount   = {0}".format(iothub_device.cloudToDeviceMessageCount) )
        print ( "iothubDevice.isManaged                   = {0}".format(iothub_device.isManaged) )
        print ( "iothubDevice.authMethod                  = {0}".format(iothub_device.authMethod) )
        print ( "" )
    ```
3. <span data-ttu-id="2f2ea-139">Adja hozzá a következő függvény toocreate hello eszköz azonosítása hello beállításjegyzék-kezelő használatával hello.</span><span class="sxs-lookup"><span data-stu-id="2f2ea-139">Add hello following function toocreate hello device identification using hello Registry Manager.</span></span> 

    ```python
    def iothub_createdevice():
        try:
            iothub_registry_manager = IoTHubRegistryManager(CONNECTION_STRING)
            auth_method = IoTHubRegistryManagerAuthMethod.SHARED_PRIVATE_KEY
            new_device = iothub_registry_manager.create_device(DEVICE_ID, "", "", auth_method)
            print_device_info("CreateDevice", new_device)

        except IoTHubError as iothub_error:
            print ( "Unexpected error {0}".format(iothub_error) )
            return
        except KeyboardInterrupt:
            print ( "iothub_createdevice stopped" )
    ```
4. <span data-ttu-id="2f2ea-140">Végül adja hozzá az alábbiak szerint hello fő funkciója, és mentse hello fájlt.</span><span class="sxs-lookup"><span data-stu-id="2f2ea-140">Finally, add hello main function as follows and save hello file.</span></span>

    ```python
    if __name__ == '__main__':
        print ( "" )
        print ( "Python {0}".format(sys.version) )
        print ( "Creating device using hello Azure IoT Hub Service SDK for Python" )
        print ( "" )
        print ( "    Connection string = {0}".format(CONNECTION_STRING) )
        print ( "    Device ID         = {0}".format(DEVICE_ID) )

        iothub_createdevice()
    ```
5. <span data-ttu-id="2f2ea-141">A hello parancssorból futtassa a hello **CreateDeviceIdentity.py** az alábbiak szerint:</span><span class="sxs-lookup"><span data-stu-id="2f2ea-141">On hello command prompt, run hello **CreateDeviceIdentity.py** as follows:</span></span>

    ```python
    python CreateDeviceIdentity.py
    ```
6. <span data-ttu-id="2f2ea-142">Hello szimulált eszköz létrehozva első kell megjelennie.</span><span class="sxs-lookup"><span data-stu-id="2f2ea-142">You should see hello simulated device getting created.</span></span> <span data-ttu-id="2f2ea-143">Jegyezze fel a hello **deviceId** és hello **primaryKey** eszköz.</span><span class="sxs-lookup"><span data-stu-id="2f2ea-143">Note down hello **deviceId** and hello **primaryKey** of this device.</span></span> <span data-ttu-id="2f2ea-144">Ezek az értékek később szüksége tooIoT eszközként központ kapcsolódó alkalmazásban létrehozásakor.</span><span class="sxs-lookup"><span data-stu-id="2f2ea-144">You need these values later when you create an application that connects tooIoT Hub as a device.</span></span>

    ![Eszköz létrehozása sikeres][1]

> [!NOTE]
> <span data-ttu-id="2f2ea-146">az IoT-központ identitásjegyzékhez hello csak eszköz identitások tooenable biztonságos hozzáférést toohello IoT-központ tárolja.</span><span class="sxs-lookup"><span data-stu-id="2f2ea-146">hello IoT Hub identity registry only stores device identities tooenable secure access toohello IoT hub.</span></span> <span data-ttu-id="2f2ea-147">Eszköz azonosítók és kulcsok toouse hitelesítő adatokat, és egy engedélyezett vagy letiltott jelző használható toodisable hozzáférést egy adott eszköz tárol.</span><span class="sxs-lookup"><span data-stu-id="2f2ea-147">It stores device IDs and keys toouse as security credentials and an enabled/disabled flag that you can use toodisable access for an individual device.</span></span> <span data-ttu-id="2f2ea-148">Ha az alkalmazásnak toostore más eszközre vonatkozó metaadatok, akkor az alkalmazás-specifikus tárolási használjon.</span><span class="sxs-lookup"><span data-stu-id="2f2ea-148">If your application needs toostore other device-specific metadata, it should use an application-specific store.</span></span> <span data-ttu-id="2f2ea-149">További információkért lásd: hello [IoT Hub fejlesztői útmutató][lnk-devguide-identity].</span><span class="sxs-lookup"><span data-stu-id="2f2ea-149">For more information, see hello [IoT Hub developer guide][lnk-devguide-identity].</span></span>
> 
> 


## <a name="create-a-simulated-device-app"></a><span data-ttu-id="2f2ea-150">Szimulált eszközalkalmazás létrehozása</span><span class="sxs-lookup"><span data-stu-id="2f2ea-150">Create a simulated device app</span></span>
<span data-ttu-id="2f2ea-151">Ez a szakasz hello lépéseket toocreate egy Python-Konzolalkalmazás, szimulálja egy eszközt, és elküldi az eszköz a felhőbe küldött üzeneteket tooyour IoT-központ.</span><span class="sxs-lookup"><span data-stu-id="2f2ea-151">This section lists hello steps toocreate a Python console app, that simulates a device and sends device-to-cloud messages tooyour IoT hub.</span></span>

1. <span data-ttu-id="2f2ea-152">Nyisson meg egy új parancssort, és telepítse az Azure IoT Hub eszköz SDK hello a Python az alábbiak szerint.</span><span class="sxs-lookup"><span data-stu-id="2f2ea-152">Open a new command prompt and install hello Azure IoT Hub Device SDK for Python as follows.</span></span> <span data-ttu-id="2f2ea-153">Zárja be a hello parancssort hello telepítése után.</span><span class="sxs-lookup"><span data-stu-id="2f2ea-153">Close hello command prompt after hello installation.</span></span>

    ```
    pip install azure-iothub-device-client
    ```
2. <span data-ttu-id="2f2ea-154">Hozzon létre egy **SimulatedDevice.py** nevű fájlt.</span><span class="sxs-lookup"><span data-stu-id="2f2ea-154">Create a file named **SimulatedDevice.py**.</span></span> <span data-ttu-id="2f2ea-155">Nyissa meg a fájlt egy szabadon választott Python-szerkesztőben/IDE-ben (például az IDLE környezetben).</span><span class="sxs-lookup"><span data-stu-id="2f2ea-155">Open this file in a Python editor/IDE of your choice (for example, IDLE).</span></span>

3. <span data-ttu-id="2f2ea-156">Adja hozzá a következő tooimport szükséges hello kódmodulok SDK hello eszközről hello.</span><span class="sxs-lookup"><span data-stu-id="2f2ea-156">Add hello following code tooimport hello required modules from hello device SDK.</span></span>

    ```python
    import random
    import time
    import sys
    import iothub_client
    from iothub_client import IoTHubClient, IoTHubClientError, IoTHubTransportProvider, IoTHubClientResult
    from iothub_client import IoTHubMessage, IoTHubMessageDispositionResult, IoTHubError, DeviceMethodReturnValue
    ```
4. <span data-ttu-id="2f2ea-157">Adja hozzá a hello alábbi kód, és cserélje le a helyőrző hello `[IoTHub Device Connection String]` az eszközhöz hello kapcsolati karakterlánccal.</span><span class="sxs-lookup"><span data-stu-id="2f2ea-157">Add hello following code and replace hello placeholder for `[IoTHub Device Connection String]` with hello connection string for your device.</span></span> <span data-ttu-id="2f2ea-158">hello eszköz kapcsolati karakterlánca általában hello formátumban `HostName=<hostName>;DeviceId=<deviceId>;SharedAccessKey=<primaryKey>`.</span><span class="sxs-lookup"><span data-stu-id="2f2ea-158">hello device connection string is usually in hello format of `HostName=<hostName>;DeviceId=<deviceId>;SharedAccessKey=<primaryKey>`.</span></span> <span data-ttu-id="2f2ea-159">Használjon hello **deviceId** és **primaryKey** hello előző szakasz tooreplace hello létrehozott hello eszköz `<deviceId>` és `<primaryKey>` kulcsattribútumokkal.</span><span class="sxs-lookup"><span data-stu-id="2f2ea-159">Use hello **deviceId** and **primaryKey** of hello device you created in hello previous section tooreplace hello `<deviceId>` and `<primaryKey>` respectively.</span></span> <span data-ttu-id="2f2ea-160">A `<hostName>` nevet cserélje le az IoT Hub gazdagépnevére. Ez általában `<IoT hub name>.azure-devices.net`.</span><span class="sxs-lookup"><span data-stu-id="2f2ea-160">Replace `<hostName>` with your IoT hub's host name, usually as `<IoT hub name>.azure-devices.net`.</span></span>

    ```python
    # String containing Hostname, Device Id & Device Key in hello format
    CONNECTION_STRING = "[IoTHub Device Connection String]"
    # choose HTTP, AMQP or MQTT as transport protocol
    PROTOCOL = IoTHubTransportProvider.MQTT
    MESSAGE_TIMEOUT = 10000
    AVG_WIND_SPEED = 10.0
    SEND_CALLBACKS = 0
    MSG_TXT = "{\"deviceId\": \"MyFirstPythonDevice\",\"windSpeed\": %.2f}"    
    ```
5. <span data-ttu-id="2f2ea-161">Adja hozzá a következő kód toodefine egy küldési megerősítő visszahívás hello.</span><span class="sxs-lookup"><span data-stu-id="2f2ea-161">Add hello following code toodefine a send confirmation callback.</span></span> 

    ```python
    def send_confirmation_callback(message, result, user_context):
        global SEND_CALLBACKS
        print ( "Confirmation[%d] received for message with result = %s" % (user_context, result) )
        map_properties = message.properties()
        print ( "    message_id: %s" % message.message_id )
        print ( "    correlation_id: %s" % message.correlation_id )
        key_value_pair = map_properties.get_internals()
        print ( "    Properties: %s" % key_value_pair )
        SEND_CALLBACKS += 1
        print ( "    Total calls confirmed: %d" % SEND_CALLBACKS )
    ```
6. <span data-ttu-id="2f2ea-162">Adja hozzá a következő kód tooinitialize hello eszközügyfél hello.</span><span class="sxs-lookup"><span data-stu-id="2f2ea-162">Add hello following code tooinitialize hello device client.</span></span>

    ```python
    def iothub_client_init():
        # prepare iothub client
        client = IoTHubClient(CONNECTION_STRING, PROTOCOL)
        # set hello time until a message times out
        client.set_option("messageTimeout", MESSAGE_TIMEOUT)
        client.set_option("logtrace", 0)
        return client
    ```
7. <span data-ttu-id="2f2ea-163">Adja hozzá a következő hello tooformat funkciót, és egy üzenetet küldeni a szimulált eszköz tooyour IoT-központ.</span><span class="sxs-lookup"><span data-stu-id="2f2ea-163">Add hello following function tooformat and send a message from your simulated device tooyour IoT hub.</span></span>

    ```python
    def iothub_client_telemetry_sample_run():

        try:
            client = iothub_client_init()
            print ( "IoT Hub device sending periodic messages, press Ctrl-C tooexit" )
            message_counter = 0

            while True:
                msg_txt_formatted = MSG_TXT % (AVG_WIND_SPEED + (random.random() * 4 + 2))
                # messages can be encoded as string or bytearray
                if (message_counter & 1) == 1:
                    message = IoTHubMessage(bytearray(msg_txt_formatted, 'utf8'))
                else:
                    message = IoTHubMessage(msg_txt_formatted)
                # optional: assign ids
                message.message_id = "message_%d" % message_counter
                message.correlation_id = "correlation_%d" % message_counter
                # optional: assign properties
                prop_map = message.properties()
                prop_text = "PropMsg_%d" % message_counter
                prop_map.add("Property", prop_text)

                client.send_event_async(message, send_confirmation_callback, message_counter)
                print ( "IoTHubClient.send_event_async accepted message [%d] for transmission tooIoT Hub." % message_counter )

                status = client.get_send_status()
                print ( "Send status: %s" % status )
                time.sleep(30)

                status = client.get_send_status()
                print ( "Send status: %s" % status )

                message_counter += 1

        except IoTHubError as iothub_error:
            print ( "Unexpected error %s from IoTHub" % iothub_error )
            return
        except KeyboardInterrupt:
            print ( "IoTHubClient sample stopped" )
    ```
8. <span data-ttu-id="2f2ea-164">Végül adja hozzá a hello fő funkciója.</span><span class="sxs-lookup"><span data-stu-id="2f2ea-164">Finally, add hello main function.</span></span> 

    ```python
    if __name__ == '__main__':
        print ( "Simulating a device using hello Azure IoT Hub Device SDK for Python" )
        print ( "    Protocol %s" % PROTOCOL )
        print ( "    Connection string=%s" % CONNECTION_STRING )

        iothub_client_telemetry_sample_run()
    ```
9. <span data-ttu-id="2f2ea-165">Mentse és zárja be a hello **SimulatedDevice.py** fájlt.</span><span class="sxs-lookup"><span data-stu-id="2f2ea-165">Save and close hello **SimulatedDevice.py** file.</span></span> <span data-ttu-id="2f2ea-166">Meg vannak most már készen áll a toorun ezt az alkalmazást.</span><span class="sxs-lookup"><span data-stu-id="2f2ea-166">You are now ready toorun this app.</span></span>

> [!NOTE]
> <span data-ttu-id="2f2ea-167">Ez az oktatóanyag tookeep dolgot egyszerű, nem valósítja meg semmilyen újrapróbálkozási házirendje.</span><span class="sxs-lookup"><span data-stu-id="2f2ea-167">tookeep things simple, this tutorial does not implement any retry policy.</span></span> <span data-ttu-id="2f2ea-168">Az éles kódban, meg kell valósítania újrapróbálkozási házirendek (például az exponenciális leállítási), hello MSDN-cikkben leírtak [átmeneti hiba kezelése][lnk-transient-faults].</span><span class="sxs-lookup"><span data-stu-id="2f2ea-168">In production code, you should implement retry policies (such as an exponential backoff), as suggested in hello MSDN article [Transient Fault Handling][lnk-transient-faults].</span></span>
> 
> 

## <a name="receive-messages-from-your-simulated-device"></a><span data-ttu-id="2f2ea-169">Üzenet fogadása a szimulált eszközről</span><span class="sxs-lookup"><span data-stu-id="2f2ea-169">Receive messages from your simulated device</span></span>
<span data-ttu-id="2f2ea-170">tooreceive telemetriai üzeneteket az eszközről, szükség toouse egy [Event Hubs][lnk-event-hubs-overview]-kompatibilis végpont által hello IoT-központot, amelyben a köszönőüzenetei eszközről a felhőbe.</span><span class="sxs-lookup"><span data-stu-id="2f2ea-170">tooreceive telemetry messages from your device, you need toouse an [Event Hubs][lnk-event-hubs-overview]-compatible endpoint exposed by hello IoT Hub, which reads hello device-to-cloud messages.</span></span> <span data-ttu-id="2f2ea-171">Olvasási hello [Bevezetés az Event Hubs használatába] [ lnk-eventhubs-tutorial] hogyan tooprocess érkező üzenetek Event Hubs az IoT hub Event Hub-kompatibilis végpont kapcsolatos útmutató.</span><span class="sxs-lookup"><span data-stu-id="2f2ea-171">Read hello [Get Started with Event Hubs][lnk-eventhubs-tutorial] tutorial for information on how tooprocess messages from Event Hubs for your IoT hub's Event Hub-compatible endpoint.</span></span> <span data-ttu-id="2f2ea-172">Az Event Hubs nem támogatja az telemetriai a Python, így hozhat létre egy [Node.js](iot-hub-node-node-getstarted.md#D2C_node) vagy egy [.NET](iot-hub-csharp-csharp-getstarted.md#D2C_csharp) az IoT-központ konzol Event Hubs-alapú eszköz-felhő app tooread köszönőüzenetei.</span><span class="sxs-lookup"><span data-stu-id="2f2ea-172">Event Hubs does not support telemetry in Python yet, so you can either create a [Node.js](iot-hub-node-node-getstarted.md#D2C_node) or a [.NET](iot-hub-csharp-csharp-getstarted.md#D2C_csharp) Event Hubs-based console app tooread hello device-to-cloud messages from IoT Hub.</span></span> <span data-ttu-id="2f2ea-173">Ez az oktatóanyag bemutatja, hogyan használhatja a hello [IoT Hub Explorer eszköz] [ lnk-iot-hub-explorer] tooread ezek eszközre küldött üzenetek.</span><span class="sxs-lookup"><span data-stu-id="2f2ea-173">This tutorial shows how you can use hello [IoT Hub Explorer tool][lnk-iot-hub-explorer] tooread these device messages.</span></span>

1. <span data-ttu-id="2f2ea-174">Nyisson meg egy parancssort, és az IoT Hub Explorer hello telepítse.</span><span class="sxs-lookup"><span data-stu-id="2f2ea-174">Open a command prompt and install hello IoT Hub Explorer.</span></span> 

    ```
    npm install -g iothub-explorer
    ```

2. <span data-ttu-id="2f2ea-175">Futtassa a parancsot a hello parancssor a következő hello, toobegin figyelési hello eszközről a felhőbe üzeneteket az eszközről.</span><span class="sxs-lookup"><span data-stu-id="2f2ea-175">Run hello following command on hello command prompt, toobegin monitoring hello device-to-cloud messages from your device.</span></span> <span data-ttu-id="2f2ea-176">Az IoT hub kapcsolódási karakterlánc használata után hello helyőrző `--login`.</span><span class="sxs-lookup"><span data-stu-id="2f2ea-176">Use your IoT hub's connection string in hello placeholder after `--login`.</span></span>

    ```
    iothub-explorer monitor-events MyFirstPythonDevice --login "[IoTHub connection string]"
    ```

3. <span data-ttu-id="2f2ea-177">Nyisson meg egy új parancssort, és keresse meg a hello tartalmazó toohello könyvtár **SimulatedDevice.py** fájlt.</span><span class="sxs-lookup"><span data-stu-id="2f2ea-177">Open a new command prompt and navigate toohello directory containing hello **SimulatedDevice.py** file.</span></span>

4. <span data-ttu-id="2f2ea-178">Futtassa a hello **SimulatedDevice.py** fájlt, amely rendszeresen küld a telemetriai adatok tooyour IoT-központot.</span><span class="sxs-lookup"><span data-stu-id="2f2ea-178">Run hello **SimulatedDevice.py** file, which periodically sends telemetry data tooyour IoT hub.</span></span> 
   
    ```
    python SimulatedDevice.py
    ```
5. <span data-ttu-id="2f2ea-179">Az IoT Hub Explorer hello fut az előző szakasz hello hello parancssori eszköz köszönőüzenetei láthatja.</span><span class="sxs-lookup"><span data-stu-id="2f2ea-179">Observe hello device messages on hello command prompt running hello IoT Hub Explorer from hello previous section.</span></span> 

    ![Eszközről felhőbe irányuló Python-üzenetek][2]

## <a name="next-steps"></a><span data-ttu-id="2f2ea-181">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="2f2ea-181">Next steps</span></span>
<span data-ttu-id="2f2ea-182">Ebben az oktatóanyagban egy új IoT hub konfigurálva hello Azure-portálon, és hozza létre a hello IoT hub identitásjegyzékhez egy eszközidentitás.</span><span class="sxs-lookup"><span data-stu-id="2f2ea-182">In this tutorial, you configured a new IoT hub in hello Azure portal, and then created a device identity in hello IoT hub's identity registry.</span></span> <span data-ttu-id="2f2ea-183">Az eszköz identitás tooenable hello szimulált eszköz alkalmazás toosend eszköz a felhőbe küldött üzeneteket toohello IoT-központ használta.</span><span class="sxs-lookup"><span data-stu-id="2f2ea-183">You used this device identity tooenable hello simulated device app toosend device-to-cloud messages toohello IoT hub.</span></span> <span data-ttu-id="2f2ea-184">Ön megfigyelt köszönőüzenetei hello IoT Hub Explorer eszköz hello segítségével hello IoT-központ fogadja.</span><span class="sxs-lookup"><span data-stu-id="2f2ea-184">You observed hello messages received by hello IoT hub with hello help of hello IoT Hub Explorer tool.</span></span> 

<span data-ttu-id="2f2ea-185">Python SDK tooexplore hello Azure IoT Hub használati részletesen, látogasson el [a központ Git-tárház][lnk-python-github].</span><span class="sxs-lookup"><span data-stu-id="2f2ea-185">tooexplore hello Python SDK for Azure IoT Hub usage in depth, visit [this Git Hub repo][lnk-python-github].</span></span> <span data-ttu-id="2f2ea-186">tooreview hello üzenetkezelési képességeket, hello Azure IoT Hub szolgáltatás SDK for Python, letöltése és futtatása [iothub_messaging_sample.py][lnk-messaging-sample].</span><span class="sxs-lookup"><span data-stu-id="2f2ea-186">tooreview hello messaging capabilities of hello Azure IoT Hub Service SDK for Python, you can download and run [iothub_messaging_sample.py][lnk-messaging-sample].</span></span> <span data-ttu-id="2f2ea-187">Eszköz ügyféloldali szimuláció hello Azure IoT Hub eszköz SDK-t használ a Python, töltse le és futtassa a hello [iothub_client_sample.py][lnk-client-sample].</span><span class="sxs-lookup"><span data-stu-id="2f2ea-187">For device side simulation using hello Azure IoT Hub Device SDK for Python, you can download and run hello [iothub_client_sample.py][lnk-client-sample].</span></span>

<span data-ttu-id="2f2ea-188">első lépések toocontinue az IoT Hub és tooexplore más IoT-forgatókönyvek esetén, lásd:</span><span class="sxs-lookup"><span data-stu-id="2f2ea-188">toocontinue getting started with IoT Hub and tooexplore other IoT scenarios, see:</span></span>

* <span data-ttu-id="2f2ea-189">[Kapcsolódás az eszközhöz][lnk-connect-device]</span><span class="sxs-lookup"><span data-stu-id="2f2ea-189">[Connecting your device][lnk-connect-device]</span></span>
* <span data-ttu-id="2f2ea-190">[Eszközfelügyelet – első lépések][lnk-device-management]</span><span class="sxs-lookup"><span data-stu-id="2f2ea-190">[Getting started with device management][lnk-device-management]</span></span>
* <span data-ttu-id="2f2ea-191">[Ismerkedés az Azure IoT Edge szolgáltatással][lnk-iot-edge]</span><span class="sxs-lookup"><span data-stu-id="2f2ea-191">[Getting started with Azure IoT Edge][lnk-iot-edge]</span></span>

<span data-ttu-id="2f2ea-192">toolearn hogyan tooextend az IoT megoldás és a folyamat eszközről a felhőbe üzenetek léptékű: hello [eszközről a felhőbe üzenetek feldolgozásához] [ lnk-process-d2c-tutorial] oktatóanyag.</span><span class="sxs-lookup"><span data-stu-id="2f2ea-192">toolearn how tooextend your IoT solution and process device-to-cloud messages at scale, see hello [Process device-to-cloud messages][lnk-process-d2c-tutorial] tutorial.</span></span>
[!INCLUDE [iot-hub-get-started-next-steps](../../includes/iot-hub-get-started-next-steps.md)]

<!-- Images. -->
[1]: ./media/iot-hub-python-getstarted/createdevice.png
[2]: ./media/iot-hub-python-getstarted/sendd2cmessage.png

<!-- Links -->
[lnk-python-download]: https://www.python.org/downloads/
[lnk-visual-c-redist]: http://www.microsoft.com/download/confirmation.aspx?id=48145
[lnk-node-download]: https://nodejs.org/en/download/
[lnk-install-pip]: https://pip.pypa.io/en/stable/installing/
[lnk-azure-cli-hub]: https://docs.microsoft.com/en-us/azure/iot-hub/iot-hub-create-using-cli
[lnk-transient-faults]: https://msdn.microsoft.com/library/hh680901(v=pandp.50).aspx
[lnk-idle]: https://docs.python.org/3/library/idle.html
[lnk-python-ide-list]: https://wiki.python.org/moin/IntegratedDevelopmentEnvironments
[lnk-iot-hub-explorer]: https://github.com/Azure/iothub-explorer
[lnk-python-github]: https://github.com/Azure/azure-iot-sdk-python
[lnk-messaging-sample]: https://github.com/Azure/azure-iot-sdk-python/blob/master/service/samples/iothub_messaging_sample.py
[lnk-client-sample]: https://github.com/Azure/azure-iot-sdk-python/blob/master/device/samples/iothub_client_sample.py

[lnk-eventhubs-tutorial]: ../event-hubs/event-hubs-csharp-ephcs-getstarted.md
[lnk-devguide-identity]: iot-hub-devguide-identity-registry.md
[lnk-event-hubs-overview]: ../event-hubs/event-hubs-overview.md
[lnk-python-devbox]: https://github.com/Azure/azure-iot-sdk-python/blob/master/doc/python-devbox-setup.md

[lnk-process-d2c-tutorial]: iot-hub-csharp-csharp-process-d2c.md

[lnk-hub-sdks]: iot-hub-devguide-sdks.md
[lnk-free-trial]: http://azure.microsoft.com/pricing/free-trial/

[lnk-device-management]: iot-hub-node-node-device-management-get-started.md
[lnk-iot-edge]: iot-hub-linux-iot-edge-get-started.md
[lnk-connect-device]: https://azure.microsoft.com/develop/iot/
