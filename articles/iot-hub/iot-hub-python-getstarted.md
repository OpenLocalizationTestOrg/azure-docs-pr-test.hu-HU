---
title: "Ismerkedés az Azure IoT Hub (Python) szolgáltatással | Microsoft Docs"
description: "Megtudhatja, hogyan küldhet eszközről a felhőbe irányuló üzeneteket az Azure IoT Hubba a Pythonhoz készült IoT SDK-k használatával. Szimulált eszközt és szolgáltatásalkalmazásokat hozhat létre az eszköz regisztrálásához, üzenetek küldéséhez és üzenetek olvasásához az IoT Hubról."
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
ms.openlocfilehash: 7ebbac4464d793717f68a4cb7905c53d1f5c051a
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/29/2017
---
# <a name="connect-your-simulated-device-to-your-iot-hub-using-python"></a><span data-ttu-id="c8a24-104">A szimulált eszköz csatlakoztatása az IoT Hubhoz Pythonnal</span><span class="sxs-lookup"><span data-stu-id="c8a24-104">Connect your simulated device to your IoT hub using Python</span></span>
[!INCLUDE [iot-hub-selector-get-started](../../includes/iot-hub-selector-get-started.md)]

<span data-ttu-id="c8a24-105">Az oktatóanyag végén kettő Python-alkalmazással fog rendelkezni:</span><span class="sxs-lookup"><span data-stu-id="c8a24-105">At the end of this tutorial, you will have two Python apps:</span></span>

* <span data-ttu-id="c8a24-106">A **CreateDeviceIdentity.py** egy eszközidentitást, valamint egy társított biztonsági kulcsot hoz létre, amellyel csatlakozhat a szimulált eszközalkalmazáshoz.</span><span class="sxs-lookup"><span data-stu-id="c8a24-106">**CreateDeviceIdentity.py**, which creates a device identity and associated security key to connect your simulated device app.</span></span>
* <span data-ttu-id="c8a24-107">A **SimulatedDevice.py** csatlakozik az IoT Hubhoz a korábban létrehozott eszközidentitással, és az MQTT protokoll használatával rendszeres időközönként telemetriai üzenetet küld.</span><span class="sxs-lookup"><span data-stu-id="c8a24-107">**SimulatedDevice.py**, which connects to your IoT hub with the device identity created earlier, and periodically sends a telemetry message using the MQTT protocol.</span></span>

> [!NOTE]
> <span data-ttu-id="c8a24-108">Az Azure IoT SDK-kat használhatja az eszközökön és a megoldás háttérrendszerén futó alkalmazások összeállításához egyaránt. Ezekről az [Azure IoT SDK-k][lnk-hub-sdks] című témakörben talál további információt.</span><span class="sxs-lookup"><span data-stu-id="c8a24-108">The article [Azure IoT SDKs][lnk-hub-sdks] provides information about the Azure IoT SDKs that you can use to build both applications to run on devices and your solution back end.</span></span>
> 
> 

<span data-ttu-id="c8a24-109">Az oktatóanyag teljesítéséhez a következőkre lesz szüksége:</span><span class="sxs-lookup"><span data-stu-id="c8a24-109">To complete this tutorial, you need the following:</span></span>

* <span data-ttu-id="c8a24-110">[Python 2.x vagy 3.x][lnk-python-download].</span><span class="sxs-lookup"><span data-stu-id="c8a24-110">[Python 2.x or 3.x][lnk-python-download].</span></span> <span data-ttu-id="c8a24-111">Mindenképp a rendszernek megfelelő, 32 vagy 64 bites telepítést használja.</span><span class="sxs-lookup"><span data-stu-id="c8a24-111">Make sure to use the 32-bit or 64-bit installation as required by your setup.</span></span> <span data-ttu-id="c8a24-112">Amikor a rendszer erre kéri, mindenképp adja hozzá a Pythont a platformspecifikus környezeti változóhoz.</span><span class="sxs-lookup"><span data-stu-id="c8a24-112">When prompted during the installation, make sure to add Python to your platform-specific environment variable.</span></span> <span data-ttu-id="c8a24-113">Ha a Python 2.x verziót használja, előfordulhat, hogy [telepítenie vagy frissítenie kell a *pip*-et, a Python csomagkezelő rendszerét][lnk-install-pip].</span><span class="sxs-lookup"><span data-stu-id="c8a24-113">If you are using Python 2.x, you may need to [install or upgrade *pip*, the Python package management system][lnk-install-pip].</span></span>
* <span data-ttu-id="c8a24-114">Ha Windows operációs rendszert használ, a [Visual C++ terjeszthető csomagra][lnk-visual-c-redist] van szükség a Python natív DLL-jei használatához.</span><span class="sxs-lookup"><span data-stu-id="c8a24-114">If you are using Windows OS, then [Visual C++ redistributable package][lnk-visual-c-redist] to allow the use of native DLLs from Python.</span></span>
* <span data-ttu-id="c8a24-115">[Node.js 4.0 vagy újabb][lnk-node-download].</span><span class="sxs-lookup"><span data-stu-id="c8a24-115">[Node.js 4.0 or later][lnk-node-download].</span></span> <span data-ttu-id="c8a24-116">Mindenképp a rendszernek megfelelő, 32 vagy 64 bites telepítést használja.</span><span class="sxs-lookup"><span data-stu-id="c8a24-116">Make sure to use the 32-bit or 64-bit installation as required by your setup.</span></span> <span data-ttu-id="c8a24-117">Ez az [IoT Hub Explorer eszköz][lnk-iot-hub-explorer] telepítéséhez szükséges.</span><span class="sxs-lookup"><span data-stu-id="c8a24-117">This is needed to install the [IoT Hub Explorer tool][lnk-iot-hub-explorer].</span></span>
* <span data-ttu-id="c8a24-118">Aktív Azure-fiók.</span><span class="sxs-lookup"><span data-stu-id="c8a24-118">An active Azure account.</span></span> <span data-ttu-id="c8a24-119">Ha nincs fiókja, néhány perc alatt létrehozhat egy [ingyenes fiókot][lnk-free-trial].</span><span class="sxs-lookup"><span data-stu-id="c8a24-119">If you don't have an account, you can create a [free account][lnk-free-trial] in just a couple of minutes.</span></span>

> [!NOTE]
> <span data-ttu-id="c8a24-120">`azure-iothub-service-client` és `azure-iothub-device-client` rendszerhez a *pip*-csomagok jelenleg csak Windows operációs rendszer alatt érhetőek el.</span><span class="sxs-lookup"><span data-stu-id="c8a24-120">The *pip* packages for `azure-iothub-service-client` and `azure-iothub-device-client` are currently available only for Windows OS.</span></span> <span data-ttu-id="c8a24-121">Linux/Mac OS rendszer estében olvassa el a [Python fejlesztőkörnyezet előkészítése ][lnk-python-devbox] című bejegyzés Linuxra illetve Mac OS-re vonatkozó részét.</span><span class="sxs-lookup"><span data-stu-id="c8a24-121">For Linux/Mac OS, please refer to the Linux and Mac OS-specific sections on the [Prepare your development environment for Python][lnk-python-devbox] post.</span></span>
> 

[!INCLUDE [iot-hub-get-started-create-hub](../../includes/iot-hub-get-started-create-hub.md)]

<span data-ttu-id="c8a24-122">Ezzel létrehozta az IoT Hubot.</span><span class="sxs-lookup"><span data-stu-id="c8a24-122">You have now created your IoT hub.</span></span> <span data-ttu-id="c8a24-123">Az oktatóanyag további részében használja az IoT Hub-gazdagépnevet és kapcsolati karakterláncot.</span><span class="sxs-lookup"><span data-stu-id="c8a24-123">Use the IoT Hub host name and the IoT Hub connection string in the rest of this tutorial.</span></span>

> [!NOTE]
> <span data-ttu-id="c8a24-124">Az IoT Hubot könnyedén létrehozhatja egy parancssorban a Python- vagy Node.js-alapú Azure CLI használatával.</span><span class="sxs-lookup"><span data-stu-id="c8a24-124">You can also easily create your IoT hub on a command line, using the Python or Node.js based Azure CLI.</span></span> <span data-ttu-id="c8a24-125">Ennek lépéseit az [IoT Hub Azure CLI 2.0-vel történő létrehozásával][lnk-azure-cli-hub] foglalkozó cikk ismerteti.</span><span class="sxs-lookup"><span data-stu-id="c8a24-125">The article [Create an IoT hub using the Azure CLI 2.0][lnk-azure-cli-hub] shows you the quick steps to do so.</span></span> 
> 

## <a name="create-a-device-identity"></a><span data-ttu-id="c8a24-126">Eszközidentitás létrehozása</span><span class="sxs-lookup"><span data-stu-id="c8a24-126">Create a device identity</span></span>
<span data-ttu-id="c8a24-127">Ez a szakasz egy Python-konzolalkalmazás létrehozásának lépéseit ismerteti, amely egy új eszközidentitást hoz létre az IoT Hub identitásjegyzékében.</span><span class="sxs-lookup"><span data-stu-id="c8a24-127">This section lists the steps to create a Python console app, that creates a device identity in the identity registry of your IoT hub.</span></span> <span data-ttu-id="c8a24-128">Egy eszköz csak akkor tud csatlakozni az IoT Hubhoz, ha be van jegyezve az identitásjegyzékbe.</span><span class="sxs-lookup"><span data-stu-id="c8a24-128">A device can only connect to IoT Hub if it has an entry in the identity registry.</span></span> <span data-ttu-id="c8a24-129">További információkért lásd az [IoT Hub fejlesztői útmutatójának][lnk-devguide-identity] **Identitásjegyzék** című szakaszát.</span><span class="sxs-lookup"><span data-stu-id="c8a24-129">For more information, see the **Identity Registry** section of the [IoT Hub developer guide][lnk-devguide-identity].</span></span> <span data-ttu-id="c8a24-130">A konzolalkalmazás egy egyedi eszközazonosítót állít elő a futtatásakor, valamint egy kulcsot, amellyel az eszköz azonosítani tudja magát, amikor az eszközről a felhőbe irányuló üzeneteket küld az IoT Hubnak.</span><span class="sxs-lookup"><span data-stu-id="c8a24-130">When you run this console app, it generates a unique device ID and key that your device can use to identify itself when it sends device-to-cloud messages to IoT Hub.</span></span>

1. <span data-ttu-id="c8a24-131">Nyisson meg egy parancssort, és telepítse a **Pythonhoz készült Azure IoT Hub szolgáltatási SDK-t**.</span><span class="sxs-lookup"><span data-stu-id="c8a24-131">Open a command prompt and install the **Azure IoT Hub Service SDK for Python**.</span></span> <span data-ttu-id="c8a24-132">Az SDK telepítése után zárja be a parancssort.</span><span class="sxs-lookup"><span data-stu-id="c8a24-132">Close the command prompt after you install the SDK.</span></span>

    ```
    pip install azure-iothub-service-client
    ```

2. <span data-ttu-id="c8a24-133">Hozzon létre egy Python-fájlt **CreateDeviceIdentity.py** néven.</span><span class="sxs-lookup"><span data-stu-id="c8a24-133">Create a Python file named **CreateDeviceIdentity.py**.</span></span> <span data-ttu-id="c8a24-134">Nyissa meg a fájlt [egy szabadon választott Python-szerkesztőben/IDE-ben][lnk-python-ide-list], például az alapértelmezett [IDLE][lnk-idle] környezetben.</span><span class="sxs-lookup"><span data-stu-id="c8a24-134">Open it in [a Python editor/IDE of your choice][lnk-python-ide-list], for example, the default [IDLE][lnk-idle].</span></span>

3. <span data-ttu-id="c8a24-135">Adja hozzá az alábbi kódot a szükséges modulok importálásához a szolgáltatás SDK-jából:</span><span class="sxs-lookup"><span data-stu-id="c8a24-135">Add the following code to import the required modules from the service SDK:</span></span>

    ```python
    import sys
    import iothub_service_client
    from iothub_service_client import IoTHubRegistryManager, IoTHubRegistryManagerAuthMethod
    from iothub_service_client import IoTHubDeviceStatus, IoTHubError
    ```
2. <span data-ttu-id="c8a24-136">Adja hozzá a következő kódot, és az `[IoTHub Connection String]` helyőrzőjét cserélje le az IoT Hub előző szakaszban létrehozott kapcsolati karakterláncára.</span><span class="sxs-lookup"><span data-stu-id="c8a24-136">Add the following code, replacing the placeholder for `[IoTHub Connection String]` with the connection string for the IoT hub you created in the previous section.</span></span> <span data-ttu-id="c8a24-137">Bármilyen nevet használhat `DEVICE_ID` azonosítóként.</span><span class="sxs-lookup"><span data-stu-id="c8a24-137">You can use any name as the `DEVICE_ID`.</span></span>
   
    ```python
    CONNECTION_STRING = "[IoTHub Connection String]"
    DEVICE_ID = "MyFirstPythonDevice"
    ```
   [!INCLUDE [iot-hub-pii-note-naming-device](../../includes/iot-hub-pii-note-naming-device.md)]

3. <span data-ttu-id="c8a24-138">Adja hozzá a következő függvényt az eszköz egyes információinak kinyomtatása érdekében.</span><span class="sxs-lookup"><span data-stu-id="c8a24-138">Add the following function to print some of the device information.</span></span>

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
3. <span data-ttu-id="c8a24-139">Adja hozzá a következő függvényt az eszköz azonosítójának létrehozásához a Registry Manager használatával.</span><span class="sxs-lookup"><span data-stu-id="c8a24-139">Add the following function to create the device identification using the Registry Manager.</span></span> 

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
4. <span data-ttu-id="c8a24-140">Végül adja hozzá a fő függvényt a következők szerint, és mentse a fájlt.</span><span class="sxs-lookup"><span data-stu-id="c8a24-140">Finally, add the main function as follows and save the file.</span></span>

    ```python
    if __name__ == '__main__':
        print ( "" )
        print ( "Python {0}".format(sys.version) )
        print ( "Creating device using the Azure IoT Hub Service SDK for Python" )
        print ( "" )
        print ( "    Connection string = {0}".format(CONNECTION_STRING) )
        print ( "    Device ID         = {0}".format(DEVICE_ID) )

        iothub_createdevice()
    ```
5. <span data-ttu-id="c8a24-141">A parancssorban futtassa a **CreateDeviceIdentity.py** fájlt az alábbiak szerint:</span><span class="sxs-lookup"><span data-stu-id="c8a24-141">On the command prompt, run the **CreateDeviceIdentity.py** as follows:</span></span>

    ```python
    python CreateDeviceIdentity.py
    ```
6. <span data-ttu-id="c8a24-142">Ekkor létre kell jönnie a szimulált eszköznek.</span><span class="sxs-lookup"><span data-stu-id="c8a24-142">You should see the simulated device getting created.</span></span> <span data-ttu-id="c8a24-143">Jegyezze fel az eszköz **deviceId** azonosítóját és **primaryKey** kulcsát.</span><span class="sxs-lookup"><span data-stu-id="c8a24-143">Note down the **deviceId** and the **primaryKey** of this device.</span></span> <span data-ttu-id="c8a24-144">Ezekre az értékekre később szüksége lesz, amikor az IoT Hubhoz eszközként csatlakozó alkalmazást hoz létre.</span><span class="sxs-lookup"><span data-stu-id="c8a24-144">You need these values later when you create an application that connects to IoT Hub as a device.</span></span>

    ![Eszköz létrehozása sikeres][1]

> [!NOTE]
> <span data-ttu-id="c8a24-146">Az IoT Hub-identitásjegyzék csak az IoT Hub biztonságos elérésének biztosításához tárolja az eszközidentitásokat.</span><span class="sxs-lookup"><span data-stu-id="c8a24-146">The IoT Hub identity registry only stores device identities to enable secure access to the IoT hub.</span></span> <span data-ttu-id="c8a24-147">Az eszközazonosítókat és kulcsokat biztonsági hitelesítő adatokként tárolja, valamint tartalmaz egy engedélyezve/letiltva jelzőt, amellyel letilthatja egy adott eszköz hozzáférését.</span><span class="sxs-lookup"><span data-stu-id="c8a24-147">It stores device IDs and keys to use as security credentials and an enabled/disabled flag that you can use to disable access for an individual device.</span></span> <span data-ttu-id="c8a24-148">Ha az alkalmazásnak más eszközspecifikus metaadatokat kell tárolnia, egy alkalmazásspecifikus tárolót kell használnia.</span><span class="sxs-lookup"><span data-stu-id="c8a24-148">If your application needs to store other device-specific metadata, it should use an application-specific store.</span></span> <span data-ttu-id="c8a24-149">További információ: [IoT Hub fejlesztői útmutató][lnk-devguide-identity].</span><span class="sxs-lookup"><span data-stu-id="c8a24-149">For more information, see the [IoT Hub developer guide][lnk-devguide-identity].</span></span>
> 
> 


## <a name="create-a-simulated-device-app"></a><span data-ttu-id="c8a24-150">Szimulált eszközalkalmazás létrehozása</span><span class="sxs-lookup"><span data-stu-id="c8a24-150">Create a simulated device app</span></span>
<span data-ttu-id="c8a24-151">Ez a szakasz egy olyan Python-konzolalkalmazás létrehozásának lépéseit ismerteti, amely egy eszközt szimulál, és az eszközről a felhőbe irányuló üzeneteket küld az IoT Hubra.</span><span class="sxs-lookup"><span data-stu-id="c8a24-151">This section lists the steps to create a Python console app, that simulates a device and sends device-to-cloud messages to your IoT hub.</span></span>

1. <span data-ttu-id="c8a24-152">Nyisson meg egy új parancssort, és telepítse a Pythonhoz készült Azure IoT Hub eszközoldali SDK-t az alábbiak szerint.</span><span class="sxs-lookup"><span data-stu-id="c8a24-152">Open a new command prompt and install the Azure IoT Hub Device SDK for Python as follows.</span></span> <span data-ttu-id="c8a24-153">A telepítés után zárja be a parancssort.</span><span class="sxs-lookup"><span data-stu-id="c8a24-153">Close the command prompt after the installation.</span></span>

    ```
    pip install azure-iothub-device-client
    ```
2. <span data-ttu-id="c8a24-154">Hozzon létre egy **SimulatedDevice.py** nevű fájlt.</span><span class="sxs-lookup"><span data-stu-id="c8a24-154">Create a file named **SimulatedDevice.py**.</span></span> <span data-ttu-id="c8a24-155">Nyissa meg a fájlt egy szabadon választott Python-szerkesztőben/IDE-ben (például az IDLE környezetben).</span><span class="sxs-lookup"><span data-stu-id="c8a24-155">Open this file in a Python editor/IDE of your choice (for example, IDLE).</span></span>

3. <span data-ttu-id="c8a24-156">Adja hozzá az alábbi kódot a szükséges modulok importálásához az eszközoldali SDK-ból.</span><span class="sxs-lookup"><span data-stu-id="c8a24-156">Add the following code to import the required modules from the device SDK.</span></span>

    ```python
    import random
    import time
    import sys
    import iothub_client
    from iothub_client import IoTHubClient, IoTHubClientError, IoTHubTransportProvider, IoTHubClientResult
    from iothub_client import IoTHubMessage, IoTHubMessageDispositionResult, IoTHubError, DeviceMethodReturnValue
    ```
4. <span data-ttu-id="c8a24-157">Adja hozzá a következő kódot, és az `[IoTHub Device Connection String]` helyőrzőjét értékét cserélje le az eszköz kapcsolati karakterláncára.</span><span class="sxs-lookup"><span data-stu-id="c8a24-157">Add the following code and replace the placeholder for `[IoTHub Device Connection String]` with the connection string for your device.</span></span> <span data-ttu-id="c8a24-158">Az eszköz kapcsolati karakterláncának formátuma általában `HostName=<hostName>;DeviceId=<deviceId>;SharedAccessKey=<primaryKey>`.</span><span class="sxs-lookup"><span data-stu-id="c8a24-158">The device connection string is usually in the format of `HostName=<hostName>;DeviceId=<deviceId>;SharedAccessKey=<primaryKey>`.</span></span> <span data-ttu-id="c8a24-159">Az előző szakaszban létrehozott eszköz **deviceId** azonosítójára és **primaryKey** kulcsára cserélje le a `<deviceId>` és a `<primaryKey>` értékeit.</span><span class="sxs-lookup"><span data-stu-id="c8a24-159">Use the **deviceId** and **primaryKey** of the device you created in the previous section to replace the `<deviceId>` and `<primaryKey>` respectively.</span></span> <span data-ttu-id="c8a24-160">A `<hostName>` nevet cserélje le az IoT Hub gazdagépnevére. Ez általában `<IoT hub name>.azure-devices.net`.</span><span class="sxs-lookup"><span data-stu-id="c8a24-160">Replace `<hostName>` with your IoT hub's host name, usually as `<IoT hub name>.azure-devices.net`.</span></span>

    ```python
    # String containing Hostname, Device Id & Device Key in the format
    CONNECTION_STRING = "[IoTHub Device Connection String]"
    # choose HTTP, AMQP or MQTT as transport protocol
    PROTOCOL = IoTHubTransportProvider.MQTT
    MESSAGE_TIMEOUT = 10000
    AVG_WIND_SPEED = 10.0
    SEND_CALLBACKS = 0
    MSG_TXT = "{\"deviceId\": \"MyFirstPythonDevice\",\"windSpeed\": %.2f}"    
    ```
5. <span data-ttu-id="c8a24-161">Adja hozzá a következő kódot egy megerősítő visszahívás küldéséhez.</span><span class="sxs-lookup"><span data-stu-id="c8a24-161">Add the following code to define a send confirmation callback.</span></span> 

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
6. <span data-ttu-id="c8a24-162">Adja hozzá a következő kódot az eszközügyfél inicializálásához.</span><span class="sxs-lookup"><span data-stu-id="c8a24-162">Add the following code to initialize the device client.</span></span>

    ```python
    def iothub_client_init():
        # prepare iothub client
        client = IoTHubClient(CONNECTION_STRING, PROTOCOL)
        # set the time until a message times out
        client.set_option("messageTimeout", MESSAGE_TIMEOUT)
        client.set_option("logtrace", 0)
        return client
    ```
7. <span data-ttu-id="c8a24-163">Adja hozzá a következő függvényt egy üzenet formázásához és elküldéséhez a szimulált eszközről az IoT Hubra.</span><span class="sxs-lookup"><span data-stu-id="c8a24-163">Add the following function to format and send a message from your simulated device to your IoT hub.</span></span>

    ```python
    def iothub_client_telemetry_sample_run():

        try:
            client = iothub_client_init()
            print ( "IoT Hub device sending periodic messages, press Ctrl-C to exit" )
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
                print ( "IoTHubClient.send_event_async accepted message [%d] for transmission to IoT Hub." % message_counter )

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
8. <span data-ttu-id="c8a24-164">Végül adja hozzá a fő függvényt.</span><span class="sxs-lookup"><span data-stu-id="c8a24-164">Finally, add the main function.</span></span> 

    ```python
    if __name__ == '__main__':
        print ( "Simulating a device using the Azure IoT Hub Device SDK for Python" )
        print ( "    Protocol %s" % PROTOCOL )
        print ( "    Connection string=%s" % CONNECTION_STRING )

        iothub_client_telemetry_sample_run()
    ```
9. <span data-ttu-id="c8a24-165">Mentse és zárja be a **SimulatedDevice.py** fájlt.</span><span class="sxs-lookup"><span data-stu-id="c8a24-165">Save and close the **SimulatedDevice.py** file.</span></span> <span data-ttu-id="c8a24-166">Most már készen áll az alkalmazás futtatására.</span><span class="sxs-lookup"><span data-stu-id="c8a24-166">You are now ready to run this app.</span></span>

> [!NOTE]
> <span data-ttu-id="c8a24-167">Az egyszerűség kedvéért ez az oktatóanyag nem valósít meg semmilyen újrapróbálkozási házirendet.</span><span class="sxs-lookup"><span data-stu-id="c8a24-167">To keep things simple, this tutorial does not implement any retry policy.</span></span> <span data-ttu-id="c8a24-168">Az éles kódban újrapróbálkozási házirendeket is meg kell valósítania (például egy exponenciális leállítást) a [tranziens hibakezelést][lnk-transient-faults] ismertető MSDN-cikkben leírtak szerint.</span><span class="sxs-lookup"><span data-stu-id="c8a24-168">In production code, you should implement retry policies (such as an exponential backoff), as suggested in the MSDN article [Transient Fault Handling][lnk-transient-faults].</span></span>
> 
> 

## <a name="receive-messages-from-your-simulated-device"></a><span data-ttu-id="c8a24-169">Üzenet fogadása a szimulált eszközről</span><span class="sxs-lookup"><span data-stu-id="c8a24-169">Receive messages from your simulated device</span></span>
<span data-ttu-id="c8a24-170">Az eszközről érkező telemetriaüzenetek fogadásához egy, az IoT Hub által feltárt [Event Hubs][lnk-event-hubs-overview]-kompatibilis végpontot kell használnia, amely beolvassa az eszközről a felhőbe irányuló üzeneteket.</span><span class="sxs-lookup"><span data-stu-id="c8a24-170">To receive telemetry messages from your device, you need to use an [Event Hubs][lnk-event-hubs-overview]-compatible endpoint exposed by the IoT Hub, which reads the device-to-cloud messages.</span></span> <span data-ttu-id="c8a24-171">Az Event Hubstól az IoT Hub Event Hubs-kompatibilis végpontjára érkező üzenetek feldolgozásával kapcsolatos információkért olvassa el [az Event Hubs használatának első lépéseit][lnk-eventhubs-tutorial] ismertető oktatóanyagot.</span><span class="sxs-lookup"><span data-stu-id="c8a24-171">Read the [Get Started with Event Hubs][lnk-eventhubs-tutorial] tutorial for information on how to process messages from Event Hubs for your IoT hub's Event Hub-compatible endpoint.</span></span> <span data-ttu-id="c8a24-172">Az Event Hubs egyelőre nem támogatja a telemetriát a Pythonban, így létrehozhat egy [Node.js](iot-hub-node-node-getstarted.md#D2C_node) vagy [.NET](iot-hub-csharp-csharp-getstarted.md#D2C_csharp) Event Hubs-alapú konzolalkalmazást az IoT Hubról érkező, az eszközről a felhőbe irányuló üzenetek olvasásához.</span><span class="sxs-lookup"><span data-stu-id="c8a24-172">Event Hubs does not support telemetry in Python yet, so you can either create a [Node.js](iot-hub-node-node-getstarted.md#D2C_node) or a [.NET](iot-hub-csharp-csharp-getstarted.md#D2C_csharp) Event Hubs-based console app to read the device-to-cloud messages from IoT Hub.</span></span> <span data-ttu-id="c8a24-173">Az oktatóanyag bemutatja, hogyan használható az [IoT Hub Explorer eszköz][lnk-iot-hub-explorer] ezeknek az eszközüzeneteknek az olvasására.</span><span class="sxs-lookup"><span data-stu-id="c8a24-173">This tutorial shows how you can use the [IoT Hub Explorer tool][lnk-iot-hub-explorer] to read these device messages.</span></span>

1. <span data-ttu-id="c8a24-174">Nyisson meg egy parancssort, és telepítse az IoT Hub Explorert.</span><span class="sxs-lookup"><span data-stu-id="c8a24-174">Open a command prompt and install the IoT Hub Explorer.</span></span> 

    ```
    npm install -g iothub-explorer
    ```

2. <span data-ttu-id="c8a24-175">Futtassa az alábbi parancsot a parancssorban az eszközről érkező, az eszközről a felhőbe irányuló üzenetek megfigyelésének indításához.</span><span class="sxs-lookup"><span data-stu-id="c8a24-175">Run the following command on the command prompt, to begin monitoring the device-to-cloud messages from your device.</span></span> <span data-ttu-id="c8a24-176">Használja az IoT Hub kapcsolati karakterláncát a helyőrzőben a `--login` után.</span><span class="sxs-lookup"><span data-stu-id="c8a24-176">Use your IoT hub's connection string in the placeholder after `--login`.</span></span>

    ```
    iothub-explorer monitor-events MyFirstPythonDevice --login "[IoTHub connection string]"
    ```

3. <span data-ttu-id="c8a24-177">Nyisson egy új parancssort, és lépjen a **SimulatedDevice.py** fájlt tartalmazó könyvtárba.</span><span class="sxs-lookup"><span data-stu-id="c8a24-177">Open a new command prompt and navigate to the directory containing the **SimulatedDevice.py** file.</span></span>

4. <span data-ttu-id="c8a24-178">Futtassa a **SimulatedDevice.py** fájlt, amely rendszeres időközönként telemetriaadatokat küld az IoT Hubnak.</span><span class="sxs-lookup"><span data-stu-id="c8a24-178">Run the **SimulatedDevice.py** file, which periodically sends telemetry data to your IoT hub.</span></span> 
   
    ```
    python SimulatedDevice.py
    ```
5. <span data-ttu-id="c8a24-179">Vizsgálja meg az eszköz üzeneteit a parancssorban az előző szakaszban ismertetett IoT Hub Explorer futtatásával.</span><span class="sxs-lookup"><span data-stu-id="c8a24-179">Observe the device messages on the command prompt running the IoT Hub Explorer from the previous section.</span></span> 

    ![Eszközről felhőbe irányuló Python-üzenetek][2]

## <a name="next-steps"></a><span data-ttu-id="c8a24-181">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="c8a24-181">Next steps</span></span>
<span data-ttu-id="c8a24-182">Ebben az oktatóanyagban egy új IoT Hubot konfigurált az Azure-portálon, majd létrehozott egy eszközidentitást az IoT Hub identitásjegyzékében.</span><span class="sxs-lookup"><span data-stu-id="c8a24-182">In this tutorial, you configured a new IoT hub in the Azure portal, and then created a device identity in the IoT hub's identity registry.</span></span> <span data-ttu-id="c8a24-183">Ennek az eszközidentitásnak a segítségével lehetővé tette a szimulált eszközalkalmazásnak, hogy az eszközről a felhőbe irányuló üzeneteket küldjön az IoT Hubnak.</span><span class="sxs-lookup"><span data-stu-id="c8a24-183">You used this device identity to enable the simulated device app to send device-to-cloud messages to the IoT hub.</span></span> <span data-ttu-id="c8a24-184">Megfigyelte az IoT Hub által fogadott üzeneteket az IoT Hub Explorer eszköz segítségével.</span><span class="sxs-lookup"><span data-stu-id="c8a24-184">You observed the messages received by the IoT hub with the help of the IoT Hub Explorer tool.</span></span> 

<span data-ttu-id="c8a24-185">A Pythonhoz készült Azure IoT Hub SDK használatának részletesebb megismerése érdekében látogasson el [ebbe a GitHub-adattárba][lnk-python-github].</span><span class="sxs-lookup"><span data-stu-id="c8a24-185">To explore the Python SDK for Azure IoT Hub usage in depth, visit [this Git Hub repo][lnk-python-github].</span></span> <span data-ttu-id="c8a24-186">A Pythonhoz készült Azure IoT Hub szolgáltatási SDK üzenetküldési képességeinek áttekintéséhez töltse le és futtassa az [iothub_messaging_sample.py][lnk-messaging-sample] fájlt.</span><span class="sxs-lookup"><span data-stu-id="c8a24-186">To review the messaging capabilities of the Azure IoT Hub Service SDK for Python, you can download and run [iothub_messaging_sample.py][lnk-messaging-sample].</span></span> <span data-ttu-id="c8a24-187">A Pythonhoz készült Azure IoT Hub eszközoldali SDK használatával végzett eszközoldali szimulációkhoz töltse le és futtassa az [iothub_client_sample.py][lnk-client-sample] fájlt.</span><span class="sxs-lookup"><span data-stu-id="c8a24-187">For device side simulation using the Azure IoT Hub Device SDK for Python, you can download and run the [iothub_client_sample.py][lnk-client-sample].</span></span>

<span data-ttu-id="c8a24-188">További bevezetés az IoT Hub használatába, valamint egyéb IoT-forgatókönyvek megismerése:</span><span class="sxs-lookup"><span data-stu-id="c8a24-188">To continue getting started with IoT Hub and to explore other IoT scenarios, see:</span></span>

* <span data-ttu-id="c8a24-189">[Kapcsolódás az eszközhöz][lnk-connect-device]</span><span class="sxs-lookup"><span data-stu-id="c8a24-189">[Connecting your device][lnk-connect-device]</span></span>
* <span data-ttu-id="c8a24-190">[Eszközfelügyelet – első lépések][lnk-device-management]</span><span class="sxs-lookup"><span data-stu-id="c8a24-190">[Getting started with device management][lnk-device-management]</span></span>
* <span data-ttu-id="c8a24-191">[Ismerkedés az Azure IoT Edge szolgáltatással][lnk-iot-edge]</span><span class="sxs-lookup"><span data-stu-id="c8a24-191">[Getting started with Azure IoT Edge][lnk-iot-edge]</span></span>

<span data-ttu-id="c8a24-192">Az IoT-megoldás kibővítésével és az eszközről a felhőbe irányuló üzenetek nagy léptékű feldolgozásával kapcsolatban tekintse meg [az eszközről a felhőbe irányuló üzenetek feldolgozását][lnk-process-d2c-tutorial] ismertető oktatóanyagot.</span><span class="sxs-lookup"><span data-stu-id="c8a24-192">To learn how to extend your IoT solution and process device-to-cloud messages at scale, see the [Process device-to-cloud messages][lnk-process-d2c-tutorial] tutorial.</span></span>
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
