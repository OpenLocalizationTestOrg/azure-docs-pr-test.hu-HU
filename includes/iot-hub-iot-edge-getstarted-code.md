## <a name="typical-output"></a><span data-ttu-id="0599c-101">Tipikus kimenet</span><span class="sxs-lookup"><span data-stu-id="0599c-101">Typical output</span></span>

<span data-ttu-id="0599c-102">hello következő példa bemutatja toohello naplófájl írták hello Hello World PéldaAlkalmazás hello kimeneti.</span><span class="sxs-lookup"><span data-stu-id="0599c-102">hello following example shows hello output written toohello log file by hello Hello World sample.</span></span> <span data-ttu-id="0599c-103">hello kimeneti olvashatóságát van formázva:</span><span class="sxs-lookup"><span data-stu-id="0599c-103">hello output is formatted for legibility:</span></span>

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

## <a name="code-snippets"></a><span data-ttu-id="0599c-104">Kódrészletek</span><span class="sxs-lookup"><span data-stu-id="0599c-104">Code snippets</span></span>

<span data-ttu-id="0599c-105">Ez a szakasz ismerteti, amelyek néhány hello hello hello kód legfontosabb részeit\_world PéldaAlkalmazás.</span><span class="sxs-lookup"><span data-stu-id="0599c-105">This section discusses some key sections of hello code in hello hello\_world sample.</span></span>

### <a name="iot-edge-gateway-creation"></a><span data-ttu-id="0599c-106">Az IoT-peremhálózati átjáró létrehozása</span><span class="sxs-lookup"><span data-stu-id="0599c-106">IoT Edge gateway creation</span></span>

<span data-ttu-id="0599c-107">Meg kell valósítani egy *átjáró folyamat*.</span><span class="sxs-lookup"><span data-stu-id="0599c-107">You must implement a *gateway process*.</span></span> <span data-ttu-id="0599c-108">A program hello belső infrastruktúra (hello broker) hoz létre, hello IoT peremhálózati modulok betölti és hello átjáró folyamat konfigurálja.</span><span class="sxs-lookup"><span data-stu-id="0599c-108">This program creates hello internal infrastructure (hello broker), loads hello IoT Edge modules, and configures hello gateway process.</span></span> <span data-ttu-id="0599c-109">IoT peremhálózati biztosít hello **átjáró\_létrehozása\_a\_JSON** tooenable működéséhez toobootstrap egy átjáró egy JSON-fájlból.</span><span class="sxs-lookup"><span data-stu-id="0599c-109">IoT Edge provides hello **Gateway\_Create\_From\_JSON** function tooenable you toobootstrap a gateway from a JSON file.</span></span> <span data-ttu-id="0599c-110">toouse hello **átjáró\_létrehozása\_a\_JSON** működik, hello elérési tooa JSON-fájl, amely meghatározza a hello IoT peremhálózati modulok tooload adja át.</span><span class="sxs-lookup"><span data-stu-id="0599c-110">toouse hello **Gateway\_Create\_From\_JSON** function, pass it hello path tooa JSON file that specifies hello IoT Edge modules tooload.</span></span>

<span data-ttu-id="0599c-111">Hello kód hello átjáró folyamat hello található *Hello World* hello a minta [main.c] [ lnk-main-c] fájlt.</span><span class="sxs-lookup"><span data-stu-id="0599c-111">You can find hello code for hello gateway process in hello *Hello World* sample in hello [main.c][lnk-main-c] file.</span></span> <span data-ttu-id="0599c-112">A olvashatóságát, hello alábbi kódrészletben láthatja, egy rövidített hello átjáró folyamat kód.</span><span class="sxs-lookup"><span data-stu-id="0599c-112">For legibility, hello following snippet shows an abbreviated version of hello gateway process code.</span></span> <span data-ttu-id="0599c-113">Ez például a program létrehoz egy átjárót, majd vár hello felhasználói toopress hello **ENTER** kulcs, mielőtt azt kiváltó hello átjáró le.</span><span class="sxs-lookup"><span data-stu-id="0599c-113">This example program creates a gateway and then waits for hello user toopress hello **ENTER** key before it tears down hello gateway.</span></span>

```c
int main(int argc, char** argv)
{
    GATEWAY_HANDLE gateway;
    if ((gateway = Gateway_Create_From_JSON(argv[1])) == NULL)
    {
        printf("failed toocreate hello gateway from JSON\n");
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

<span data-ttu-id="0599c-114">hello JSON beállításfájl IoT peremhálózati modulok tooload és hello hivatkozások között hello modulok listáját tartalmazza.</span><span class="sxs-lookup"><span data-stu-id="0599c-114">hello JSON settings file contains a list of IoT Edge modules tooload and hello links between hello modules.</span></span> <span data-ttu-id="0599c-115">Minden egyes IoT peremhálózati modul a: kell megadnia.</span><span class="sxs-lookup"><span data-stu-id="0599c-115">Each IoT Edge module must specify a:</span></span>

* <span data-ttu-id="0599c-116">**név**: hello modul egyedi nevét.</span><span class="sxs-lookup"><span data-stu-id="0599c-116">**name**: a unique name for hello module.</span></span>
* <span data-ttu-id="0599c-117">**betöltési**: a betöltési, hogy ismeri a hogyan tooload hello szükséges modul.</span><span class="sxs-lookup"><span data-stu-id="0599c-117">**loader**: a loader that knows how tooload hello desired module.</span></span> <span data-ttu-id="0599c-118">A betöltők a különböző modultípusok betöltésére szolgáló bővítménypontok.</span><span class="sxs-lookup"><span data-stu-id="0599c-118">Loaders are an extension point for loading different types of modules.</span></span> <span data-ttu-id="0599c-119">IoT peremhálózati betöltők számára biztosít a natív C, Node.js, Java és .NET írt modulok.</span><span class="sxs-lookup"><span data-stu-id="0599c-119">IoT Edge provides loaders for use with modules written in native C, Node.js, Java, and .NET.</span></span> <span data-ttu-id="0599c-120">hello Hello World PéldaAlkalmazás csak használ hello natív C betöltő, mert ez a példa minden hello modul c nyelven írt dinamikus függvénytárak További információ toouse IoT peremhálózati modulok különböző nyelveken írt: hello [Node.js](https://github.com/Azure/iot-edge/blob/master/samples/nodejs_simple_sample/), [Java](https://github.com/Azure/iot-edge/tree/master/samples/java_sample), vagy [.NET](https://github.com/Azure/iot-edge/tree/master/samples/dotnet_binding_sample) minták.</span><span class="sxs-lookup"><span data-stu-id="0599c-120">hello Hello World sample only uses hello native C loader because all hello modules in this sample are dynamic libraries written in C. For more information about how toouse IoT Edge modules written in different languages, see hello [Node.js](https://github.com/Azure/iot-edge/blob/master/samples/nodejs_simple_sample/), [Java](https://github.com/Azure/iot-edge/tree/master/samples/java_sample), or [.NET](https://github.com/Azure/iot-edge/tree/master/samples/dotnet_binding_sample) samples.</span></span>
    * <span data-ttu-id="0599c-121">**név**: használt tooload hello modul hello betöltő hello neve.</span><span class="sxs-lookup"><span data-stu-id="0599c-121">**name**: hello name of hello loader used tooload hello module.</span></span>
    * <span data-ttu-id="0599c-122">**EntryPoint**: hello hello modul tartalmazó elérési út toohello függvénytárra.</span><span class="sxs-lookup"><span data-stu-id="0599c-122">**entrypoint**: hello path toohello library containing hello module.</span></span> <span data-ttu-id="0599c-123">Linux rendszeren ez a könyvtár egy .so fájl, Windows rendszeren pedig egy .dll fájl.</span><span class="sxs-lookup"><span data-stu-id="0599c-123">On Linux this library is a .so file, on Windows this library is a .dll file.</span></span> <span data-ttu-id="0599c-124">hello belépési pont egy adott toohello típusú betöltő használja.</span><span class="sxs-lookup"><span data-stu-id="0599c-124">hello entry point is specific toohello type of loader being used.</span></span> <span data-ttu-id="0599c-125">hello Node.js betöltő belépési pont egy olyan .js kiterjesztésű fájl.</span><span class="sxs-lookup"><span data-stu-id="0599c-125">hello Node.js loader entry point is a .js file.</span></span> <span data-ttu-id="0599c-126">hello Java betöltő belépési pont, egy classpath és az osztályhoz nevet.</span><span class="sxs-lookup"><span data-stu-id="0599c-126">hello Java loader entry point is a classpath and a class name.</span></span> <span data-ttu-id="0599c-127">hello .NET betöltő belépési pont, egy szerelvény és osztály nevét.</span><span class="sxs-lookup"><span data-stu-id="0599c-127">hello .NET loader entry point is an assembly name and a class name.</span></span>

* <span data-ttu-id="0599c-128">**argumentum**: a konfigurációs információk hello modulok kell.</span><span class="sxs-lookup"><span data-stu-id="0599c-128">**args**: any configuration information hello module needs.</span></span>

<span data-ttu-id="0599c-129">a következő kód bemutatja hello használt JSON toodeclare összes hello hello hello Hello World PéldaAlkalmazás Linux IoT peremhálózati modulokat.</span><span class="sxs-lookup"><span data-stu-id="0599c-129">hello following code shows hello JSON used toodeclare all hello IoT Edge modules for hello Hello World sample on Linux.</span></span> <span data-ttu-id="0599c-130">Hello tervezési hello modul függ, hogy szükség van-e a modul az argumentumai.</span><span class="sxs-lookup"><span data-stu-id="0599c-130">Whether a module requires any arguments depends on hello design of hello module.</span></span> <span data-ttu-id="0599c-131">Ebben a példában hello naplózó modul függvény olyan argumentumot, hello elérési toohello kimeneti fájljának és hello hello\_globális modulnak argumentum nélkül.</span><span class="sxs-lookup"><span data-stu-id="0599c-131">In this example, hello logger module takes an argument that is hello path toohello output file and hello hello\_world module has no arguments.</span></span>

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

<span data-ttu-id="0599c-132">hello JSON-fájlt a hello modulok toohello broker átadott között hello hivatkozásokat is tartalmaz.</span><span class="sxs-lookup"><span data-stu-id="0599c-132">hello JSON file also contains hello links between hello modules that are passed toohello broker.</span></span> <span data-ttu-id="0599c-133">Egy hivatkozás két tulajdonsággal rendelkezik:</span><span class="sxs-lookup"><span data-stu-id="0599c-133">A link has two properties:</span></span>

* <span data-ttu-id="0599c-134">**forrás**: egy modul nevét a hello `modules` területen vagy `\*`.</span><span class="sxs-lookup"><span data-stu-id="0599c-134">**source**: a module name from hello `modules` section, or `\*`.</span></span>
* <span data-ttu-id="0599c-135">**a fogadó**: egy modul nevét a hello `modules` szakasz.</span><span class="sxs-lookup"><span data-stu-id="0599c-135">**sink**: a module name from hello `modules` section.</span></span>

<span data-ttu-id="0599c-136">Minden hivatkozás meghatároz egy üzenetútvonalat és irányt.</span><span class="sxs-lookup"><span data-stu-id="0599c-136">Each link defines a message route and direction.</span></span> <span data-ttu-id="0599c-137">Hello üzeneteit **forrás** modul érkeznek toohello **fogadó** modul.</span><span class="sxs-lookup"><span data-stu-id="0599c-137">Messages from hello **source** module are delivered toohello **sink** module.</span></span> <span data-ttu-id="0599c-138">Beállíthatja a hello **forrás** modul túl`\*`, ami azt jelenti, hogy hello **fogadó** modult a modulok üzeneteket fogad.</span><span class="sxs-lookup"><span data-stu-id="0599c-138">You can set hello **source** module too`\*`, which indicates that hello **sink** module receives messages from any module.</span></span>

<span data-ttu-id="0599c-139">hello következő kód bemutatja, hello JSON használt tooconfigure hivatkozások hello hello használt hello modulok között\_world PéldaAlkalmazás Linux rendszeren.</span><span class="sxs-lookup"><span data-stu-id="0599c-139">hello following code shows hello JSON used tooconfigure links between hello modules used in hello hello\_world sample on Linux.</span></span> <span data-ttu-id="0599c-140">Minden üzenet hello által előállított `hello_world` modul hello által felhasznált `logger` modul.</span><span class="sxs-lookup"><span data-stu-id="0599c-140">Every message produced by hello `hello_world` module is consumed by hello `logger` module.</span></span>

```json
"links":
[
    {
        "source": "hello_world",
        "sink": "logger"
    }
]
```

### <a name="helloworld-module-message-publishing"></a><span data-ttu-id="0599c-141">A hello\_world modul üzenet-közzététele</span><span class="sxs-lookup"><span data-stu-id="0599c-141">Hello\_world module message publishing</span></span>

<span data-ttu-id="0599c-142">Hello hello által használt hello kód található\_hello world modul toopublish üzenetek ["hello_world.c"] [ lnk-helloworld-c] fájlt.</span><span class="sxs-lookup"><span data-stu-id="0599c-142">You can find hello code used by hello hello\_world module toopublish messages in hello ['hello_world.c'][lnk-helloworld-c] file.</span></span> <span data-ttu-id="0599c-143">hello alábbi kódrészletben láthatja hello kód egy módosított verziója hozzáadott megjegyzésekkel és néhány hibakezelési olvashatóságát eltávolítani kódot:</span><span class="sxs-lookup"><span data-stu-id="0599c-143">hello following snippet shows an amended version of hello code with comments added and some error handling code removed for legibility:</span></span>

```c
int helloWorldThread(void *param)
{
    // create data structures used in function.
    HELLOWORLD_HANDLE_DATA* handleData = param;
    MESSAGE_CONFIG msgConfig;
    MAP_HANDLE propertiesMap = Map_Create(NULL);

    // add a property named "helloWorld" with a value of "from Azure IoT
    // Gateway SDK simple sample!" tooa set of message properties that
    // will be appended toohello message before publishing it. 
    Map_AddOrUpdate(propertiesMap, "helloWorld", "from Azure IoT Gateway SDK simple sample!")

    // set hello content for hello message
    msgConfig.size = strlen(HELLOWORLD_MESSAGE);
    msgConfig.source = HELLOWORLD_MESSAGE;

    // set hello properties for hello message
    msgConfig.sourceProperties = propertiesMap;

    // create a message based on hello msgConfig structure
    MESSAGE_HANDLE helloWorldMessage = Message_Create(&msgConfig);

    while (1)
    {
        if (handleData->stopThread)
        {
            (void)Unlock(handleData->lockHandle);
            break; /*gets out of hello thread*/
        }
        else
        {
            // publish hello message toohello broker
            (void)Broker_Publish(handleData->brokerHandle, helloWorldMessage);
            (void)Unlock(handleData->lockHandle);
        }

        (void)ThreadAPI_Sleep(5000); /*every 5 seconds*/
    }

    Message_Destroy(helloWorldMessage);

    return 0;
}
```

### <a name="helloworld-module-message-processing"></a><span data-ttu-id="0599c-144">A hello\_world modul üzenetfeldolgozása</span><span class="sxs-lookup"><span data-stu-id="0599c-144">Hello\_world module message processing</span></span>

<span data-ttu-id="0599c-145">hello hello\_globális modul soha nem dolgozza fel, hogy más IoT peremhálózati modulok közzé toohello broker üzeneteket.</span><span class="sxs-lookup"><span data-stu-id="0599c-145">hello hello\_world module never processes messages that other IoT Edge modules publish toohello broker.</span></span> <span data-ttu-id="0599c-146">Hello üzenet visszahívási hello hello a megvalósítása ezért hello\_globális modul műveletvégzés feladata.</span><span class="sxs-lookup"><span data-stu-id="0599c-146">Therefore, hello implementation of hello message callback in hello hello\_world module is a no-op function.</span></span>

```c
static void HelloWorld_Receive(MODULE_HANDLE moduleHandle, MESSAGE_HANDLE messageHandle)
{
    /* No action, HelloWorld is not interested in any messages. */
}
```

### <a name="logger-module-message-publishing-and-processing"></a><span data-ttu-id="0599c-147">A naplózómodul üzenet-közzététele és -feldolgozása</span><span class="sxs-lookup"><span data-stu-id="0599c-147">Logger module message publishing and processing</span></span>

<span data-ttu-id="0599c-148">hello naplózó modul hello broker üzeneteket fogad, és írja őket tooa fájlt.</span><span class="sxs-lookup"><span data-stu-id="0599c-148">hello logger module receives messages from hello broker and writes them tooa file.</span></span> <span data-ttu-id="0599c-149">Soha nem tesz közzé üzeneteket.</span><span class="sxs-lookup"><span data-stu-id="0599c-149">It never publishes any messages.</span></span> <span data-ttu-id="0599c-150">Ezért hello naplózó modul hello kód soha nem hívja a hello **Broker_Publish** függvény.</span><span class="sxs-lookup"><span data-stu-id="0599c-150">Therefore, hello code of hello logger module never calls hello **Broker_Publish** function.</span></span>

<span data-ttu-id="0599c-151">Hello **Logger_Receive** hello függvényt [logger.c] [ lnk-logger-c] fájl hello visszahívási hello broker toodeliver üzenetek toohello naplózó modul hív meg.</span><span class="sxs-lookup"><span data-stu-id="0599c-151">hello **Logger_Receive** function in hello [logger.c][lnk-logger-c] file is hello callback hello broker invokes toodeliver messages toohello logger module.</span></span> <span data-ttu-id="0599c-152">hello alábbi kódrészletben láthatja módosított változat hozzáadott megjegyzésekkel és néhány hibakezelési olvashatóságát eltávolítani kódot:</span><span class="sxs-lookup"><span data-stu-id="0599c-152">hello following snippet shows an amended version with comments added and some error handling code removed for legibility:</span></span>

```c
static void Logger_Receive(MODULE_HANDLE moduleHandle, MESSAGE_HANDLE messageHandle)
{

    time_t temp = time(NULL);
    struct tm* t = localtime(&temp);
    char timetemp[80] = { 0 };

    // Get hello message properties from hello message
    CONSTMAP_HANDLE originalProperties = Message_GetProperties(messageHandle); 
    MAP_HANDLE propertiesAsMap = ConstMap_CloneWriteable(originalProperties);

    // Convert hello collection of properties into a JSON string
    STRING_HANDLE jsonProperties = Map_ToJSON(propertiesAsMap);

    //  base64 encode hello message content
    const CONSTBUFFER * content = Message_GetContent(messageHandle);
    STRING_HANDLE contentAsJSON = Base64_Encode_Bytes(content->buffer, content->size);

    // Start hello construction of hello final string toobe logged by adding
    // hello timestamp
    STRING_HANDLE jsonToBeAppended = STRING_construct(",{\"time\":\"");
    STRING_concat(jsonToBeAppended, timetemp);

    // Add hello message properties
    STRING_concat(jsonToBeAppended, "\",\"properties\":"); 
    STRING_concat_with_STRING(jsonToBeAppended, jsonProperties);

    // Add hello content
    STRING_concat(jsonToBeAppended, ",\"content\":\"");
    STRING_concat_with_STRING(jsonToBeAppended, contentAsJSON);
    STRING_concat(jsonToBeAppended, "\"}]");

    // Write hello formatted string
    LOGGER_HANDLE_DATA *handleData = (LOGGER_HANDLE_DATA *)moduleHandle;
    addJSONString(handleData->fout, STRING_c_str(jsonToBeAppended);
}
```

## <a name="next-steps"></a><span data-ttu-id="0599c-153">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="0599c-153">Next steps</span></span>

<span data-ttu-id="0599c-154">Ebben a cikkben egy egyszerű IoT átjáró írja az üzenetek tooa naplófájl futtatta.</span><span class="sxs-lookup"><span data-stu-id="0599c-154">In this article, you ran a simple IoT Edge gateway that writes messages tooa log file.</span></span> <span data-ttu-id="0599c-155">toorun egy minta által küldött üzenetek tooIoT Hub, lásd: [IoT peremhálózati – eszközről a felhőbe üzeneteket küldeni egy szimulált eszköz használata Linux] [ lnk-gateway-simulated-linux] vagy [IoT peremhálózati – az eszköz-felhő üzenetek küldése egy a Windows, a szimulált eszköz][lnk-gateway-simulated-windows].</span><span class="sxs-lookup"><span data-stu-id="0599c-155">toorun a sample that sends messages tooIoT Hub, see [IoT Edge – send device-to-cloud messages with a simulated device using Linux][lnk-gateway-simulated-linux] or [IoT Edge – send device-to-cloud messages with a simulated device using Windows][lnk-gateway-simulated-windows].</span></span>


<!-- Links -->
[lnk-main-c]: https://github.com/Azure/iot-edge/blob/master/samples/hello_world/src/main.c
[lnk-helloworld-c]: https://github.com/Azure/iot-edge/blob/master/modules/hello_world/src/hello_world.c
[lnk-logger-c]: https://github.com/Azure/iot-edge/blob/master/modules/logger/src/logger.c
[lnk-iot-edge]: https://github.com/Azure/iot-edge/
[lnk-gateway-simulated-linux]: ../articles/iot-hub/iot-hub-linux-iot-edge-simulated-device.md
[lnk-gateway-simulated-windows]: ../articles/iot-hub/iot-hub-windows-iot-edge-simulated-device.md