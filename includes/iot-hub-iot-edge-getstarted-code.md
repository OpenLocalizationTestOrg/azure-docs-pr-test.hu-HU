## <a name="typical-output"></a><span data-ttu-id="64baf-101">Tipikus kimenet</span><span class="sxs-lookup"><span data-stu-id="64baf-101">Typical output</span></span>

<span data-ttu-id="64baf-102">A következő példa bemutatja a kimeneti által a Hello World PéldaAlkalmazás ír a naplófájlba.</span><span class="sxs-lookup"><span data-stu-id="64baf-102">The following example shows the output written to the log file by the Hello World sample.</span></span> <span data-ttu-id="64baf-103">A kimenet az olvashatóság érdekében formázva van:</span><span class="sxs-lookup"><span data-stu-id="64baf-103">The output is formatted for legibility:</span></span>

```json
[{
    "time": "Mon Apr 11 13:48:07 2016",
    "content": "Log started"
}, {
    "time": "Mon Apr 11 13:48:48 2016",
    "properties": {
        "helloWorld": "from Azure IoT Gateway SDK simple sample!"
    },
    "content": "aGVsbG8gd29ybGQ="
}, {
    "time": "Mon Apr 11 13:48:55 2016",
    "properties": {
        "helloWorld": "from Azure IoT Gateway SDK simple sample!"
    },
    "content": "aGVsbG8gd29ybGQ="
}, {
    "time": "Mon Apr 11 13:49:01 2016",
    "properties": {
        "helloWorld": "from Azure IoT Gateway SDK simple sample!"
    },
    "content": "aGVsbG8gd29ybGQ="
}, {
    "time": "Mon Apr 11 13:49:04 2016",
    "content": "Log stopped"
}]
```

## <a name="code-snippets"></a><span data-ttu-id="64baf-104">Kódrészletek</span><span class="sxs-lookup"><span data-stu-id="64baf-104">Code snippets</span></span>

<span data-ttu-id="64baf-105">Ez a szakasz a hello\_world minta kódjának néhány szakaszát tárgyalja.</span><span class="sxs-lookup"><span data-stu-id="64baf-105">This section discusses some key sections of the code in the hello\_world sample.</span></span>

### <a name="iot-edge-gateway-creation"></a><span data-ttu-id="64baf-106">Az IoT-peremhálózati átjáró létrehozása</span><span class="sxs-lookup"><span data-stu-id="64baf-106">IoT Edge gateway creation</span></span>

<span data-ttu-id="64baf-107">Meg kell valósítani egy *átjáró folyamat*.</span><span class="sxs-lookup"><span data-stu-id="64baf-107">You must implement a *gateway process*.</span></span> <span data-ttu-id="64baf-108">A program hoz létre a belső infrastruktúra (broker), az IoT-Edge modulok betölti és konfigurálja az átjáró folyamat.</span><span class="sxs-lookup"><span data-stu-id="64baf-108">This program creates the internal infrastructure (the broker), loads the IoT Edge modules, and configures the gateway process.</span></span> <span data-ttu-id="64baf-109">Az IoT Edge-ben megtalálható a **Gateway\_Create\_From\_JSON** függvény, amellyel elindíthat egy átjárót a JSON-fájlokból.</span><span class="sxs-lookup"><span data-stu-id="64baf-109">IoT Edge provides the **Gateway\_Create\_From\_JSON** function to enable you to bootstrap a gateway from a JSON file.</span></span> <span data-ttu-id="64baf-110">Használatához a **átjáró\_létrehozása\_a\_JSON** működik, adja át az elérési út egy JSON-fájl, amely meghatározza az IoT-Edge modulok betöltése.</span><span class="sxs-lookup"><span data-stu-id="64baf-110">To use the **Gateway\_Create\_From\_JSON** function, pass it the path to a JSON file that specifies the IoT Edge modules to load.</span></span>

<span data-ttu-id="64baf-111">A kód találhat meg az átjáró folyamatot a *Hello World* a minta a [main.c] [ lnk-main-c] fájlt.</span><span class="sxs-lookup"><span data-stu-id="64baf-111">You can find the code for the gateway process in the *Hello World* sample in the [main.c][lnk-main-c] file.</span></span> <span data-ttu-id="64baf-112">Az olvashatóság érdekében a következő részlet az átjáró folyamatkódjának rövidített verzióját mutatja.</span><span class="sxs-lookup"><span data-stu-id="64baf-112">For legibility, the following snippet shows an abbreviated version of the gateway process code.</span></span> <span data-ttu-id="64baf-113">Ez a példaprogram létrehoz egy átjárót, majd megvárja, amíg a felhasználó lenyomja az **ENTER** billentyűt, mielőtt lebontja az átjárót.</span><span class="sxs-lookup"><span data-stu-id="64baf-113">This example program creates a gateway and then waits for the user to press the **ENTER** key before it tears down the gateway.</span></span>

```c
int main(int argc, char** argv)
{
    GATEWAY_HANDLE gateway;
    if ((gateway = Gateway_Create_From_JSON(argv[1])) == NULL)
    {
        printf("failed to create the gateway from JSON\n");
    }
    else
    {
        printf("gateway successfully created from JSON\n");
        printf("gateway shall run until ENTER is pressed\n");
        (void)getchar();
        Gateway_LL_Destroy(gateway);
    }
    return 0;
}
```

<span data-ttu-id="64baf-114">A JSON-beállítások fájl IoT peremhálózati modulok betöltése és a modulok közötti kapcsolatok listáját tartalmazza.</span><span class="sxs-lookup"><span data-stu-id="64baf-114">The JSON settings file contains a list of IoT Edge modules to load and the links between the modules.</span></span> <span data-ttu-id="64baf-115">Minden egyes IoT peremhálózati modul a: kell megadnia.</span><span class="sxs-lookup"><span data-stu-id="64baf-115">Each IoT Edge module must specify a:</span></span>

* <span data-ttu-id="64baf-116">**name**: a modul egyedi neve.</span><span class="sxs-lookup"><span data-stu-id="64baf-116">**name**: a unique name for the module.</span></span>
* <span data-ttu-id="64baf-117">**loader**: olyan betöltő, amely képes a kívánt modul betöltésére.</span><span class="sxs-lookup"><span data-stu-id="64baf-117">**loader**: a loader that knows how to load the desired module.</span></span> <span data-ttu-id="64baf-118">A betöltők a különböző modultípusok betöltésére szolgáló bővítménypontok.</span><span class="sxs-lookup"><span data-stu-id="64baf-118">Loaders are an extension point for loading different types of modules.</span></span> <span data-ttu-id="64baf-119">IoT peremhálózati betöltők számára biztosít a natív C, Node.js, Java és .NET írt modulok.</span><span class="sxs-lookup"><span data-stu-id="64baf-119">IoT Edge provides loaders for use with modules written in native C, Node.js, Java, and .NET.</span></span> <span data-ttu-id="64baf-120">A Hello World PéldaAlkalmazás csak használja a natív C betöltő, mert ez a példa a modulok c nyelven írt dinamikus függvénytárak Különböző nyelveken írt IoT peremhálózati modulok használatával kapcsolatos további információkért lásd: a [Node.js](https://github.com/Azure/iot-edge/blob/master/samples/nodejs_simple_sample/), [Java](https://github.com/Azure/iot-edge/tree/master/samples/java_sample), vagy [.NET](https://github.com/Azure/iot-edge/tree/master/samples/dotnet_binding_sample) minták.</span><span class="sxs-lookup"><span data-stu-id="64baf-120">The Hello World sample only uses the native C loader because all the modules in this sample are dynamic libraries written in C. For more information about how to use IoT Edge modules written in different languages, see the [Node.js](https://github.com/Azure/iot-edge/blob/master/samples/nodejs_simple_sample/), [Java](https://github.com/Azure/iot-edge/tree/master/samples/java_sample), or [.NET](https://github.com/Azure/iot-edge/tree/master/samples/dotnet_binding_sample) samples.</span></span>
    * <span data-ttu-id="64baf-121">**név**: a modul betöltéséhez betöltő nevét.</span><span class="sxs-lookup"><span data-stu-id="64baf-121">**name**: the name of the loader used to load the module.</span></span>
    * <span data-ttu-id="64baf-122">**entrypoint**: a modult tartalmazó könyvtár elérési útja.</span><span class="sxs-lookup"><span data-stu-id="64baf-122">**entrypoint**: the path to the library containing the module.</span></span> <span data-ttu-id="64baf-123">Linux rendszeren ez a könyvtár egy .so fájl, Windows rendszeren pedig egy .dll fájl.</span><span class="sxs-lookup"><span data-stu-id="64baf-123">On Linux this library is a .so file, on Windows this library is a .dll file.</span></span> <span data-ttu-id="64baf-124">A belépési pont a használt betöltőtípustól függ.</span><span class="sxs-lookup"><span data-stu-id="64baf-124">The entry point is specific to the type of loader being used.</span></span> <span data-ttu-id="64baf-125">A Node.js betöltő belépési pont egy .js kiterjesztésű fájl.</span><span class="sxs-lookup"><span data-stu-id="64baf-125">The Node.js loader entry point is a .js file.</span></span> <span data-ttu-id="64baf-126">A Java-betöltő belépési pont, egy classpath és az osztályhoz nevet.</span><span class="sxs-lookup"><span data-stu-id="64baf-126">The Java loader entry point is a classpath and a class name.</span></span> <span data-ttu-id="64baf-127">A .NET-betöltő belépési pont, egy szerelvény és osztály nevét.</span><span class="sxs-lookup"><span data-stu-id="64baf-127">The .NET loader entry point is an assembly name and a class name.</span></span>

* <span data-ttu-id="64baf-128">**args**: a modul által igényelt konfigurációs információk.</span><span class="sxs-lookup"><span data-stu-id="64baf-128">**args**: any configuration information the module needs.</span></span>

<span data-ttu-id="64baf-129">A következő kód bemutatja, meg kell adnia az IoT-él a Hello World PéldaAlkalmazás lehetővé tevő modulokat Linux használt JSON.</span><span class="sxs-lookup"><span data-stu-id="64baf-129">The following code shows the JSON used to declare all the IoT Edge modules for the Hello World sample on Linux.</span></span> <span data-ttu-id="64baf-130">A modul kialakításától függ, hogy egy modulnak szüksége van-e argumentumokra.</span><span class="sxs-lookup"><span data-stu-id="64baf-130">Whether a module requires any arguments depends on the design of the module.</span></span> <span data-ttu-id="64baf-131">Ebben a példában a naplózómodul a kimeneti fájl elérési útját használja argumentumként, a hello\_world modul pedig nem rendelkezik argumentumokkal.</span><span class="sxs-lookup"><span data-stu-id="64baf-131">In this example, the logger module takes an argument that is the path to the output file and the hello\_world module has no arguments.</span></span>

```json
"modules" :
[
    {
        "name" : "logger",
        "loader": {
          "name": "native",
          "entrypoint": {
            "module.path": "./modules/logger/liblogger.so"
        }
        },
        "args" : {"filename":"log.txt"}
    },
    {
        "name" : "hello_world",
        "loader": {
          "name": "native",
          "entrypoint": {
            "module.path": "./modules/hello_world/libhello_world.so"
        }
        },
        "args" : null
    }
]
```

<span data-ttu-id="64baf-132">A JSON-fájl is tartalmazza a modulok közötti hivatkozásokat, amelyeket a rendszer átad a közvetítőnek.</span><span class="sxs-lookup"><span data-stu-id="64baf-132">The JSON file also contains the links between the modules that are passed to the broker.</span></span> <span data-ttu-id="64baf-133">Egy hivatkozás két tulajdonsággal rendelkezik:</span><span class="sxs-lookup"><span data-stu-id="64baf-133">A link has two properties:</span></span>

* <span data-ttu-id="64baf-134">**forrás**: egy modul nevét a `modules` területen vagy `\*`.</span><span class="sxs-lookup"><span data-stu-id="64baf-134">**source**: a module name from the `modules` section, or `\*`.</span></span>
* <span data-ttu-id="64baf-135">**sink**: modulnév a `modules` szakaszból.</span><span class="sxs-lookup"><span data-stu-id="64baf-135">**sink**: a module name from the `modules` section.</span></span>

<span data-ttu-id="64baf-136">Minden hivatkozás meghatároz egy üzenetútvonalat és irányt.</span><span class="sxs-lookup"><span data-stu-id="64baf-136">Each link defines a message route and direction.</span></span> <span data-ttu-id="64baf-137">Érkező üzenetek a **forrás** modul szállítják ki a **fogadó** modul.</span><span class="sxs-lookup"><span data-stu-id="64baf-137">Messages from the **source** module are delivered to the **sink** module.</span></span> <span data-ttu-id="64baf-138">Beállíthatja a **forrás** modul `\*`, amely azt jelzi, hogy a **fogadó** modult a modulok üzeneteket fogad.</span><span class="sxs-lookup"><span data-stu-id="64baf-138">You can set the **source** module to `\*`, which indicates that the **sink** module receives messages from any module.</span></span>

<span data-ttu-id="64baf-139">A következő kód a Linuxon a hello\_world minta moduljai közötti hivatkozások konfigurálásához használt JSON-beállításfájlt mutatja be.</span><span class="sxs-lookup"><span data-stu-id="64baf-139">The following code shows the JSON used to configure links between the modules used in the hello\_world sample on Linux.</span></span> <span data-ttu-id="64baf-140">A `hello_world` modul által létrehozott összes üzenetet a `logger` modul használja.</span><span class="sxs-lookup"><span data-stu-id="64baf-140">Every message produced by the `hello_world` module is consumed by the `logger` module.</span></span>

```json
"links":
[
    {
        "source": "hello_world",
        "sink": "logger"
    }
]
```

### <a name="helloworld-module-message-publishing"></a><span data-ttu-id="64baf-141">A hello\_world modul üzenet-közzététele</span><span class="sxs-lookup"><span data-stu-id="64baf-141">Hello\_world module message publishing</span></span>

<span data-ttu-id="64baf-142">A hello\_world modul által használt kódot felhasználhatja ahhoz, hogy közzétegye a [„hello_world.c”][lnk-helloworld-c] fájlban lévő üzeneteket.</span><span class="sxs-lookup"><span data-stu-id="64baf-142">You can find the code used by the hello\_world module to publish messages in the ['hello_world.c'][lnk-helloworld-c] file.</span></span> <span data-ttu-id="64baf-143">Az alábbi részletben a kód egy módosított verziója látható, hozzáfűzött megjegyzésekkel és – az olvashatóság érdekében – kevesebb hibakezelési kóddal:</span><span class="sxs-lookup"><span data-stu-id="64baf-143">The following snippet shows an amended version of the code with comments added and some error handling code removed for legibility:</span></span>

```c
int helloWorldThread(void *param)
{
    // create data structures used in function.
    HELLOWORLD_HANDLE_DATA* handleData = param;
    MESSAGE_CONFIG msgConfig;
    MAP_HANDLE propertiesMap = Map_Create(NULL);

    // add a property named "helloWorld" with a value of "from Azure IoT
    // Gateway SDK simple sample!" to a set of message properties that
    // will be appended to the message before publishing it. 
    Map_AddOrUpdate(propertiesMap, "helloWorld", "from Azure IoT Gateway SDK simple sample!")

    // set the content for the message
    msgConfig.size = strlen(HELLOWORLD_MESSAGE);
    msgConfig.source = HELLOWORLD_MESSAGE;

    // set the properties for the message
    msgConfig.sourceProperties = propertiesMap;

    // create a message based on the msgConfig structure
    MESSAGE_HANDLE helloWorldMessage = Message_Create(&msgConfig);

    while (1)
    {
        if (handleData->stopThread)
        {
            (void)Unlock(handleData->lockHandle);
            break; /*gets out of the thread*/
        }
        else
        {
            // publish the message to the broker
            (void)Broker_Publish(handleData->brokerHandle, helloWorldMessage);
            (void)Unlock(handleData->lockHandle);
        }

        (void)ThreadAPI_Sleep(5000); /*every 5 seconds*/
    }

    Message_Destroy(helloWorldMessage);

    return 0;
}
```

### <a name="helloworld-module-message-processing"></a><span data-ttu-id="64baf-144">A hello\_world modul üzenetfeldolgozása</span><span class="sxs-lookup"><span data-stu-id="64baf-144">Hello\_world module message processing</span></span>

<span data-ttu-id="64baf-145">A hello\_globális modul soha nem dolgozza fel a más IoT peremhálózati modulok közzététele az átvitelszervező-kezelési üzenetek.</span><span class="sxs-lookup"><span data-stu-id="64baf-145">The hello\_world module never processes messages that other IoT Edge modules publish to the broker.</span></span> <span data-ttu-id="64baf-146">A hello\_world modulban ezért az üzenet-visszahívás megvalósítása művelet nélküli függvény.</span><span class="sxs-lookup"><span data-stu-id="64baf-146">Therefore, the implementation of the message callback in the hello\_world module is a no-op function.</span></span>

```c
static void HelloWorld_Receive(MODULE_HANDLE moduleHandle, MESSAGE_HANDLE messageHandle)
{
    /* No action, HelloWorld is not interested in any messages. */
}
```

### <a name="logger-module-message-publishing-and-processing"></a><span data-ttu-id="64baf-147">A naplózómodul üzenet-közzététele és -feldolgozása</span><span class="sxs-lookup"><span data-stu-id="64baf-147">Logger module message publishing and processing</span></span>

<span data-ttu-id="64baf-148">A naplózó modul üzeneteket fogad a közvetítőtől, és egy fájlba írja őket.</span><span class="sxs-lookup"><span data-stu-id="64baf-148">The logger module receives messages from the broker and writes them to a file.</span></span> <span data-ttu-id="64baf-149">Soha nem tesz közzé üzeneteket.</span><span class="sxs-lookup"><span data-stu-id="64baf-149">It never publishes any messages.</span></span> <span data-ttu-id="64baf-150">A naplózó modul kódja ezért soha nem hívja meg a **Broker_Publish** függvényt.</span><span class="sxs-lookup"><span data-stu-id="64baf-150">Therefore, the code of the logger module never calls the **Broker_Publish** function.</span></span>

<span data-ttu-id="64baf-151">A **Logger_Receive** működni a [logger.c] [ lnk-logger-c] fájl a visszahívást a broker hív meg a tranzakciónaplókat tartalmazó modulra üzenetek továbbítására.</span><span class="sxs-lookup"><span data-stu-id="64baf-151">The **Logger_Receive** function in the [logger.c][lnk-logger-c] file is the callback the broker invokes to deliver messages to the logger module.</span></span> <span data-ttu-id="64baf-152">Az alábbi részletben egy módosított verzió látható, hozzáfűzött megjegyzésekkel és – az olvashatóság érdekében – kevesebb hibakezelési kóddal:</span><span class="sxs-lookup"><span data-stu-id="64baf-152">The following snippet shows an amended version with comments added and some error handling code removed for legibility:</span></span>

```c
static void Logger_Receive(MODULE_HANDLE moduleHandle, MESSAGE_HANDLE messageHandle)
{

    time_t temp = time(NULL);
    struct tm* t = localtime(&temp);
    char timetemp[80] = { 0 };

    // Get the message properties from the message
    CONSTMAP_HANDLE originalProperties = Message_GetProperties(messageHandle); 
    MAP_HANDLE propertiesAsMap = ConstMap_CloneWriteable(originalProperties);

    // Convert the collection of properties into a JSON string
    STRING_HANDLE jsonProperties = Map_ToJSON(propertiesAsMap);

    //  base64 encode the message content
    const CONSTBUFFER * content = Message_GetContent(messageHandle);
    STRING_HANDLE contentAsJSON = Base64_Encode_Bytes(content->buffer, content->size);

    // Start the construction of the final string to be logged by adding
    // the timestamp
    STRING_HANDLE jsonToBeAppended = STRING_construct(",{\"time\":\"");
    STRING_concat(jsonToBeAppended, timetemp);

    // Add the message properties
    STRING_concat(jsonToBeAppended, "\",\"properties\":"); 
    STRING_concat_with_STRING(jsonToBeAppended, jsonProperties);

    // Add the content
    STRING_concat(jsonToBeAppended, ",\"content\":\"");
    STRING_concat_with_STRING(jsonToBeAppended, contentAsJSON);
    STRING_concat(jsonToBeAppended, "\"}]");

    // Write the formatted string
    LOGGER_HANDLE_DATA *handleData = (LOGGER_HANDLE_DATA *)moduleHandle;
    addJSONString(handleData->fout, STRING_c_str(jsonToBeAppended);
}
```

## <a name="next-steps"></a><span data-ttu-id="64baf-153">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="64baf-153">Next steps</span></span>

<span data-ttu-id="64baf-154">Ebben a cikkben egy egyszerű, a naplófájlba írja az üzenetek IoT peremhálózati átjáró futtatta.</span><span class="sxs-lookup"><span data-stu-id="64baf-154">In this article, you ran a simple IoT Edge gateway that writes messages to a log file.</span></span> <span data-ttu-id="64baf-155">Futtasson egy mintát, amely üzeneteket küld az IoT-központ, lásd: [IoT peremhálózati – eszközről a felhőbe üzeneteket küldeni egy szimulált eszköz használata Linux] [ lnk-gateway-simulated-linux] vagy [IoT peremhálózati – az eszköz-felhő üzenetek küldése egy a Windows, a szimulált eszköz][lnk-gateway-simulated-windows].</span><span class="sxs-lookup"><span data-stu-id="64baf-155">To run a sample that sends messages to IoT Hub, see [IoT Edge – send device-to-cloud messages with a simulated device using Linux][lnk-gateway-simulated-linux] or [IoT Edge – send device-to-cloud messages with a simulated device using Windows][lnk-gateway-simulated-windows].</span></span>


<!-- Links -->
[lnk-main-c]: https://github.com/Azure/iot-edge/blob/master/samples/hello_world/src/main.c
[lnk-helloworld-c]: https://github.com/Azure/iot-edge/blob/master/modules/hello_world/src/hello_world.c
[lnk-logger-c]: https://github.com/Azure/iot-edge/blob/master/modules/logger/src/logger.c
[lnk-iot-edge]: https://github.com/Azure/iot-edge/
[lnk-gateway-simulated-linux]: ../articles/iot-hub/iot-hub-linux-iot-edge-simulated-device.md
[lnk-gateway-simulated-windows]: ../articles/iot-hub/iot-hub-windows-iot-edge-simulated-device.md