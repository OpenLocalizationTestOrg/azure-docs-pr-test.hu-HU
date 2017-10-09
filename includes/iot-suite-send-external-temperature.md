## <a name="configure-hello-nodejs-simulated-device"></a>Hello Node.js szimulált eszköz konfigurálása
1. A hello távoli figyelési irányítópulton kattintson **+ hozzáad egy eszközt** majd adja hozzá a *egyéni eszköz*. Jegyezze fel a hello IoT-központ állomásnév, eszközazonosító és eszközkulcs. Meg kell őket az oktatóanyag későbbi részében hello remote_monitoring.js eszköz ügyfélalkalmazás előkészítésekor.
2. Győződjön meg arról, hogy a Node.js-verzió 0.12.x, vagy később telepítve van a fejlesztési számítógépén. Futtatás `node --version` parancsot a parancssorba vagy egy rendszerhéj toocheck hello verziójában. A package manager tooinstall Node.js Linux használatával kapcsolatos információkért lásd: [keresztül Csomagkezelő telepítésének Node.js][node-linux].
3. Node.js telepítésekor klónozni hello legújabb verziójának hello [azure iot-sdk-csomópontos] [ lnk-github-repo] tárház tooyour fejlesztési számítógépén. Mindig használjon hello **fő** hello függvénytárak és minták legújabb verziójának hello ág.
4. Hello helyi másolatát [azure iot-sdk-csomópontos] [ lnk-github-repo] tárház, a következő két fájlok mappából hello csomópont/eszköz/minták mappa tooan üres a fejlesztői gépen másolási hello:
   
   * Packages.JSON
   * remote_monitoring.js
5. Nyissa meg hello remote_monitoring.js fájlt, és keresse meg a változó definícióját követő hello:
   
    ```
    var connectionString = "[IoT Hub device connection string]";
    ```
6. Cserélje le **[IoT Hub eszköz kapcsolati karakterlánc]** az eszköz kapcsolati karakterlánccal. Az IoT-központ állomásnév, eszközazonosító és jegyezze fel az 1. lépésben végzett eszközkulcs hello értékeit használja. Egy eszköz kapcsolati karakterlánc formátuma a következő hello rendelkezik:
   
    ```
    HostName={your IoT Hub hostname};DeviceId={your device id};SharedAccessKey={your device key}
    ```
   
    Ha az IoT-központ állomásnevet **contoso** , és az eszközazonosítót **mydevice**, a kapcsolati karakterláncot a következőképpen néz hello a következő kódrészletet:
   
    ```
    var connectionString = "HostName=contoso.azure-devices.net;DeviceId=mydevice;SharedAccessKey=2s ... =="
    ```
7. Hello fájl mentéséhez. Futtassa a következő parancsokat a rendszerhéj vagy a parancssorban a fájlok tooinstall hello szükséges csomagokat tartalmazó hello mappában hello, és futtassa a mintaalkalmazást hello:
   
    ```
    npm install
    node remote_monitoring.js
    ```

## <a name="observe-dynamic-telemetry-in-action"></a>Vegye figyelembe a dinamikus telemetriai művelet:
hello irányítópult hello hőmérséklet és a páratartalom telemetriai hello meglévő szimulált eszközökről jeleníti meg:

![hello alapértelmezett irányítópult][image1]

Ha hello Node.js szimulált eszköz hello előző szakaszban futtatta, látni hőmérséklet, páratartalom és külső hőmérséklet telemetriai adatokat:

![Külső hőmérséklet toohello irányítópult hozzáadása][image2]

hello távoli figyelési megoldást igényelnek automatikusan észleli hello további külső hőmérséklet telemetriai típus, és hozzáadja azt a hello irányítópulton toohello diagram.

[node-linux]: https://github.com/nodejs/node-v0.x-archive/wiki/Installing-Node.js-via-package-manager
[lnk-github-repo]: https://github.com/Azure/azure-iot-sdk-node
[image1]: media/iot-suite-send-external-temperature/image1.png
[image2]: media/iot-suite-send-external-temperature/image2.png