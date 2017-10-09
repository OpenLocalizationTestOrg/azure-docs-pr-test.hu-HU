## <a name="typical-output"></a>Tipikus kimenet

hello következő példa bemutatja toohello naplófájl írták hello Hello World PéldaAlkalmazás hello kimeneti. hello kimeneti olvashatóságát van formázva:

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

## <a name="code-snippets"></a>Kódrészletek

Ez a szakasz ismerteti, amelyek néhány hello hello hello kód legfontosabb részeit\_world PéldaAlkalmazás.

### <a name="iot-edge-gateway-creation"></a>Az IoT-peremhálózati átjáró létrehozása

Meg kell valósítani egy *átjáró folyamat*. A program hello belső infrastruktúra (hello broker) hoz létre, hello IoT peremhálózati modulok betölti és hello átjáró folyamat konfigurálja. IoT peremhálózati biztosít hello **átjáró\_létrehozása\_a\_JSON** tooenable működéséhez toobootstrap egy átjáró egy JSON-fájlból. toouse hello **átjáró\_létrehozása\_a\_JSON** működik, hello elérési tooa JSON-fájl, amely meghatározza a hello IoT peremhálózati modulok tooload adja át.

Hello kód hello átjáró folyamat hello található *Hello World* hello a minta [main.c] [ lnk-main-c] fájlt. A olvashatóságát, hello alábbi kódrészletben láthatja, egy rövidített hello átjáró folyamat kód. Ez például a program létrehoz egy átjárót, majd vár hello felhasználói toopress hello **ENTER** kulcs, mielőtt azt kiváltó hello átjáró le.

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

hello JSON beállításfájl IoT peremhálózati modulok tooload és hello hivatkozások között hello modulok listáját tartalmazza. Minden egyes IoT peremhálózati modul a: kell megadnia.

* **név**: hello modul egyedi nevét.
* **betöltési**: a betöltési, hogy ismeri a hogyan tooload hello szükséges modul. A betöltők a különböző modultípusok betöltésére szolgáló bővítménypontok. IoT peremhálózati betöltők számára biztosít a natív C, Node.js, Java és .NET írt modulok. hello Hello World PéldaAlkalmazás csak használ hello natív C betöltő, mert ez a példa minden hello modul c nyelven írt dinamikus függvénytárak További információ toouse IoT peremhálózati modulok különböző nyelveken írt: hello [Node.js](https://github.com/Azure/iot-edge/blob/master/samples/nodejs_simple_sample/), [Java](https://github.com/Azure/iot-edge/tree/master/samples/java_sample), vagy [.NET](https://github.com/Azure/iot-edge/tree/master/samples/dotnet_binding_sample) minták.
    * **név**: használt tooload hello modul hello betöltő hello neve.
    * **EntryPoint**: hello hello modul tartalmazó elérési út toohello függvénytárra. Linux rendszeren ez a könyvtár egy .so fájl, Windows rendszeren pedig egy .dll fájl. hello belépési pont egy adott toohello típusú betöltő használja. hello Node.js betöltő belépési pont egy olyan .js kiterjesztésű fájl. hello Java betöltő belépési pont, egy classpath és az osztályhoz nevet. hello .NET betöltő belépési pont, egy szerelvény és osztály nevét.

* **argumentum**: a konfigurációs információk hello modulok kell.

a következő kód bemutatja hello használt JSON toodeclare összes hello hello hello Hello World PéldaAlkalmazás Linux IoT peremhálózati modulokat. Hello tervezési hello modul függ, hogy szükség van-e a modul az argumentumai. Ebben a példában hello naplózó modul függvény olyan argumentumot, hello elérési toohello kimeneti fájljának és hello hello\_globális modulnak argumentum nélkül.

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

hello JSON-fájlt a hello modulok toohello broker átadott között hello hivatkozásokat is tartalmaz. Egy hivatkozás két tulajdonsággal rendelkezik:

* **forrás**: egy modul nevét a hello `modules` területen vagy `\*`.
* **a fogadó**: egy modul nevét a hello `modules` szakasz.

Minden hivatkozás meghatároz egy üzenetútvonalat és irányt. Hello üzeneteit **forrás** modul érkeznek toohello **fogadó** modul. Beállíthatja a hello **forrás** modul túl`\*`, ami azt jelenti, hogy hello **fogadó** modult a modulok üzeneteket fogad.

hello következő kód bemutatja, hello JSON használt tooconfigure hivatkozások hello hello használt hello modulok között\_world PéldaAlkalmazás Linux rendszeren. Minden üzenet hello által előállított `hello_world` modul hello által felhasznált `logger` modul.

```json
"links":
[
    {
        "source": "hello_world",
        "sink": "logger"
    }
]
```

### <a name="helloworld-module-message-publishing"></a>A hello\_world modul üzenet-közzététele

Hello hello által használt hello kód található\_hello world modul toopublish üzenetek ["hello_world.c"] [ lnk-helloworld-c] fájlt. hello alábbi kódrészletben láthatja hello kód egy módosított verziója hozzáadott megjegyzésekkel és néhány hibakezelési olvashatóságát eltávolítani kódot:

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

### <a name="helloworld-module-message-processing"></a>A hello\_world modul üzenetfeldolgozása

hello hello\_globális modul soha nem dolgozza fel, hogy más IoT peremhálózati modulok közzé toohello broker üzeneteket. Hello üzenet visszahívási hello hello a megvalósítása ezért hello\_globális modul műveletvégzés feladata.

```c
static void HelloWorld_Receive(MODULE_HANDLE moduleHandle, MESSAGE_HANDLE messageHandle)
{
    /* No action, HelloWorld is not interested in any messages. */
}
```

### <a name="logger-module-message-publishing-and-processing"></a>A naplózómodul üzenet-közzététele és -feldolgozása

hello naplózó modul hello broker üzeneteket fogad, és írja őket tooa fájlt. Soha nem tesz közzé üzeneteket. Ezért hello naplózó modul hello kód soha nem hívja a hello **Broker_Publish** függvény.

Hello **Logger_Receive** hello függvényt [logger.c] [ lnk-logger-c] fájl hello visszahívási hello broker toodeliver üzenetek toohello naplózó modul hív meg. hello alábbi kódrészletben láthatja módosított változat hozzáadott megjegyzésekkel és néhány hibakezelési olvashatóságát eltávolítani kódot:

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

## <a name="next-steps"></a>Következő lépések

Ebben a cikkben egy egyszerű IoT átjáró írja az üzenetek tooa naplófájl futtatta. toorun egy minta által küldött üzenetek tooIoT Hub, lásd: [IoT peremhálózati – eszközről a felhőbe üzeneteket küldeni egy szimulált eszköz használata Linux] [ lnk-gateway-simulated-linux] vagy [IoT peremhálózati – az eszköz-felhő üzenetek küldése egy a Windows, a szimulált eszköz][lnk-gateway-simulated-windows].


<!-- Links -->
[lnk-main-c]: https://github.com/Azure/iot-edge/blob/master/samples/hello_world/src/main.c
[lnk-helloworld-c]: https://github.com/Azure/iot-edge/blob/master/modules/hello_world/src/hello_world.c
[lnk-logger-c]: https://github.com/Azure/iot-edge/blob/master/modules/logger/src/logger.c
[lnk-iot-edge]: https://github.com/Azure/iot-edge/
[lnk-gateway-simulated-linux]: ../articles/iot-hub/iot-hub-linux-iot-edge-simulated-device.md
[lnk-gateway-simulated-windows]: ../articles/iot-hub/iot-hub-windows-iot-edge-simulated-device.md