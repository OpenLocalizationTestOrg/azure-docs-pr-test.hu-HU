## <a name="configure-hello-nodejs-simulated-device"></a><span data-ttu-id="6932c-101">Hello Node.js szimulált eszköz konfigurálása</span><span class="sxs-lookup"><span data-stu-id="6932c-101">Configure hello Node.js simulated device</span></span>
1. <span data-ttu-id="6932c-102">A hello távoli figyelési irányítópulton kattintson **+ hozzáad egy eszközt** majd adja hozzá a *egyéni eszköz*.</span><span class="sxs-lookup"><span data-stu-id="6932c-102">On hello remote monitoring dashboard, click **+ Add a device** and then add a *custom device*.</span></span> <span data-ttu-id="6932c-103">Jegyezze fel a hello IoT-központ állomásnév, eszközazonosító és eszközkulcs.</span><span class="sxs-lookup"><span data-stu-id="6932c-103">Make a note of hello IoT Hub hostname, device id, and device key.</span></span> <span data-ttu-id="6932c-104">Meg kell őket az oktatóanyag későbbi részében hello remote_monitoring.js eszköz ügyfélalkalmazás előkészítésekor.</span><span class="sxs-lookup"><span data-stu-id="6932c-104">You need them later in this tutorial when you prepare hello remote_monitoring.js device client application.</span></span>
2. <span data-ttu-id="6932c-105">Győződjön meg arról, hogy a Node.js-verzió 0.12.x, vagy később telepítve van a fejlesztési számítógépén.</span><span class="sxs-lookup"><span data-stu-id="6932c-105">Ensure that Node.js version 0.12.x or later is installed on your development machine.</span></span> <span data-ttu-id="6932c-106">Futtatás `node --version` parancsot a parancssorba vagy egy rendszerhéj toocheck hello verziójában.</span><span class="sxs-lookup"><span data-stu-id="6932c-106">Run `node --version` at a command prompt or in a shell toocheck hello version.</span></span> <span data-ttu-id="6932c-107">A package manager tooinstall Node.js Linux használatával kapcsolatos információkért lásd: [keresztül Csomagkezelő telepítésének Node.js][node-linux].</span><span class="sxs-lookup"><span data-stu-id="6932c-107">For information about using a package manager tooinstall Node.js on Linux, see [Installing Node.js via package manager][node-linux].</span></span>
3. <span data-ttu-id="6932c-108">Node.js telepítésekor klónozni hello legújabb verziójának hello [azure iot-sdk-csomópontos] [ lnk-github-repo] tárház tooyour fejlesztési számítógépén.</span><span class="sxs-lookup"><span data-stu-id="6932c-108">When you have installed Node.js, clone hello latest version of hello [azure-iot-sdk-node][lnk-github-repo] repository tooyour development machine.</span></span> <span data-ttu-id="6932c-109">Mindig használjon hello **fő** hello függvénytárak és minták legújabb verziójának hello ág.</span><span class="sxs-lookup"><span data-stu-id="6932c-109">Always use hello **master** branch for hello latest version of hello libraries and samples.</span></span>
4. <span data-ttu-id="6932c-110">Hello helyi másolatát [azure iot-sdk-csomópontos] [ lnk-github-repo] tárház, a következő két fájlok mappából hello csomópont/eszköz/minták mappa tooan üres a fejlesztői gépen másolási hello:</span><span class="sxs-lookup"><span data-stu-id="6932c-110">From your local copy of hello [azure-iot-sdk-node][lnk-github-repo] repository, copy hello following two files from hello node/device/samples folder tooan empty folder on your development machine:</span></span>
   
   * <span data-ttu-id="6932c-111">Packages.JSON</span><span class="sxs-lookup"><span data-stu-id="6932c-111">packages.json</span></span>
   * <span data-ttu-id="6932c-112">remote_monitoring.js</span><span class="sxs-lookup"><span data-stu-id="6932c-112">remote_monitoring.js</span></span>
5. <span data-ttu-id="6932c-113">Nyissa meg hello remote_monitoring.js fájlt, és keresse meg a változó definícióját követő hello:</span><span class="sxs-lookup"><span data-stu-id="6932c-113">Open hello remote_monitoring.js file and look for hello following variable definition:</span></span>
   
    ```
    var connectionString = "[IoT Hub device connection string]";
    ```
6. <span data-ttu-id="6932c-114">Cserélje le **[IoT Hub eszköz kapcsolati karakterlánc]** az eszköz kapcsolati karakterlánccal.</span><span class="sxs-lookup"><span data-stu-id="6932c-114">Replace **[IoT Hub device connection string]** with your device connection string.</span></span> <span data-ttu-id="6932c-115">Az IoT-központ állomásnév, eszközazonosító és jegyezze fel az 1. lépésben végzett eszközkulcs hello értékeit használja.</span><span class="sxs-lookup"><span data-stu-id="6932c-115">Use hello values for your IoT Hub hostname, device id, and device key that you made a note of in step 1.</span></span> <span data-ttu-id="6932c-116">Egy eszköz kapcsolati karakterlánc formátuma a következő hello rendelkezik:</span><span class="sxs-lookup"><span data-stu-id="6932c-116">A device connection string has hello following format:</span></span>
   
    ```
    HostName={your IoT Hub hostname};DeviceId={your device id};SharedAccessKey={your device key}
    ```
   
    <span data-ttu-id="6932c-117">Ha az IoT-központ állomásnevet **contoso** , és az eszközazonosítót **mydevice**, a kapcsolati karakterláncot a következőképpen néz hello a következő kódrészletet:</span><span class="sxs-lookup"><span data-stu-id="6932c-117">If your IoT Hub hostname is **contoso** and your device id is **mydevice**, your connection string looks like hello following snippet:</span></span>
   
    ```
    var connectionString = "HostName=contoso.azure-devices.net;DeviceId=mydevice;SharedAccessKey=2s ... =="
    ```
7. Hello fájl mentéséhez. <span data-ttu-id="6932c-119">Futtassa a következő parancsokat a rendszerhéj vagy a parancssorban a fájlok tooinstall hello szükséges csomagokat tartalmazó hello mappában hello, és futtassa a mintaalkalmazást hello:</span><span class="sxs-lookup"><span data-stu-id="6932c-119">Run hello following commands in a shell or command prompt in hello folder that contains these files tooinstall hello necessary packages and then run hello sample application:</span></span>
   
    ```
    npm install
    node remote_monitoring.js
    ```

## <a name="observe-dynamic-telemetry-in-action"></a><span data-ttu-id="6932c-120">Vegye figyelembe a dinamikus telemetriai művelet:</span><span class="sxs-lookup"><span data-stu-id="6932c-120">Observe dynamic telemetry in action</span></span>
<span data-ttu-id="6932c-121">hello irányítópult hello hőmérséklet és a páratartalom telemetriai hello meglévő szimulált eszközökről jeleníti meg:</span><span class="sxs-lookup"><span data-stu-id="6932c-121">hello dashboard shows hello temperature and humidity telemetry from hello existing simulated devices:</span></span>

![hello alapértelmezett irányítópult][image1]

<span data-ttu-id="6932c-123">Ha hello Node.js szimulált eszköz hello előző szakaszban futtatta, látni hőmérséklet, páratartalom és külső hőmérséklet telemetriai adatokat:</span><span class="sxs-lookup"><span data-stu-id="6932c-123">If you select hello Node.js simulated device you ran in hello previous section, you see temperature, humidity, and external temperature telemetry:</span></span>

![Külső hőmérséklet toohello irányítópult hozzáadása][image2]

<span data-ttu-id="6932c-125">hello távoli figyelési megoldást igényelnek automatikusan észleli hello további külső hőmérséklet telemetriai típus, és hozzáadja azt a hello irányítópulton toohello diagram.</span><span class="sxs-lookup"><span data-stu-id="6932c-125">hello remote monitoring solution automatically detects hello additional external temperature telemetry type and adds it toohello chart on hello dashboard.</span></span>

[node-linux]: https://github.com/nodejs/node-v0.x-archive/wiki/Installing-Node.js-via-package-manager
[lnk-github-repo]: https://github.com/Azure/azure-iot-sdk-node
[image1]: media/iot-suite-send-external-temperature/image1.png
[image2]: media/iot-suite-send-external-temperature/image2.png