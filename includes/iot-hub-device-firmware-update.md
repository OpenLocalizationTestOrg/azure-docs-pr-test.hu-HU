## <a name="create-a-simulated-device-app"></a><span data-ttu-id="be374-101">Szimulált eszközalkalmazás létrehozása</span><span class="sxs-lookup"><span data-stu-id="be374-101">Create a simulated device app</span></span>
<span data-ttu-id="be374-102">Ebben a szakaszban:</span><span class="sxs-lookup"><span data-stu-id="be374-102">In this section, you:</span></span>

* <span data-ttu-id="be374-103">Hozzon létre egy Node.js-Konzolalkalmazás, amely válaszol a hello felhő által meghívott tooa közvetlen módszer</span><span class="sxs-lookup"><span data-stu-id="be374-103">Create a Node.js console app that responds tooa direct method called by hello cloud</span></span>
* <span data-ttu-id="be374-104">Aktivál egy szimulált belsővezérlőprogram-frissítést</span><span class="sxs-lookup"><span data-stu-id="be374-104">Trigger a simulated firmware update</span></span>
* <span data-ttu-id="be374-105">Használjon hello jelentett tulajdonságok tooenable eszköz iker lekérdezések tooidentify eszközök és amikor azok legutóbbi befejeződött egy belső vezérlőprogram frissítése</span><span class="sxs-lookup"><span data-stu-id="be374-105">Use hello reported properties tooenable device twin queries tooidentify devices and when they last completed a firmware update</span></span>

<span data-ttu-id="be374-106">1. lépés:, Hozzon létre egy üres nevű mappát **manageddevice**.</span><span class="sxs-lookup"><span data-stu-id="be374-106">Step 1: Create an empty folder called **manageddevice**.</span></span>  <span data-ttu-id="be374-107">A hello **manageddevice** mappa, hozzon létre egy package.json fájlt a következő parancsot a parancssorba hello segítségével.</span><span class="sxs-lookup"><span data-stu-id="be374-107">In hello **manageddevice** folder, create a package.json file using hello following command at your command prompt.</span></span> <span data-ttu-id="be374-108">Fogadja el az összes hello alapértelmezett beállításokat:</span><span class="sxs-lookup"><span data-stu-id="be374-108">Accept all hello defaults:</span></span>
   
    ```
    npm init
    ```

<span data-ttu-id="be374-109">2. lépés: parancsot a parancssorba, a hello **manageddevice** mappa, futtassa a következő parancs tooinstall hello hello **azure iot-eszközök** és **azure-iot-eszközök – mqtt** eszköz SDK-csomagot:</span><span class="sxs-lookup"><span data-stu-id="be374-109">Step 2: At your command prompt in hello **manageddevice** folder, run hello following command tooinstall hello **azure-iot-device** and **azure-iot-device-mqtt** Device SDK packages:</span></span>
   
    ```
    npm install azure-iot-device azure-iot-device-mqtt --save
    ```

<span data-ttu-id="be374-110">3. lépés: Egy szövegszerkesztő segítségével hozzon létre egy **dmpatterns_fwupdate_device.js** hello fájlban **manageddevice** mappa.</span><span class="sxs-lookup"><span data-stu-id="be374-110">Step 3: Using a text editor, create a **dmpatterns_fwupdate_device.js** file in hello **manageddevice** folder.</span></span>

<span data-ttu-id="be374-111">4. lépés:, Adja hozzá hello következő "megkövetelése a" hello hello indításkor állapotkimutatások **dmpatterns_fwupdate_device.js** fájlt:</span><span class="sxs-lookup"><span data-stu-id="be374-111">Step 4: Add hello following 'require' statements at hello start of hello **dmpatterns_fwupdate_device.js** file:</span></span>
   
    ```
    'use strict';
   
    var Client = require('azure-iot-device').Client;
    var Protocol = require('azure-iot-device-mqtt').Mqtt;
    ```
<span data-ttu-id="be374-112">5. lépés: Adja hozzá a **connectionString** változó, és toocreate használni egy **ügyfél** példány.</span><span class="sxs-lookup"><span data-stu-id="be374-112">Step 5: Add a **connectionString** variable and use it toocreate a **Client** instance.</span></span> <span data-ttu-id="be374-113">Cserélje le a hello `{yourdeviceconnectionstring}` helyőrző korábban rögzített egy hello "Egy eszközidentitás létrehozása" szakaszban korábban hello kapcsolati karakterlánccal:</span><span class="sxs-lookup"><span data-stu-id="be374-113">Replace hello `{yourdeviceconnectionstring}` placeholder with hello connection string you previously made a note of in hello "Create a device identity" section previously:</span></span>
   
    ```
    var connectionString = '{yourdeviceconnectionstring}';
    var client = Client.fromConnectionString(connectionString, Protocol);
    ```

<span data-ttu-id="be374-114">6. lépés:, Adja hozzá hello következő függvénynél, amely használt tooupdate jelentett tulajdonságok:</span><span class="sxs-lookup"><span data-stu-id="be374-114">Step 6: Add hello following function that is used tooupdate reported properties:</span></span>
   
    ```
    var reportFWUpdateThroughTwin = function(twin, firmwareUpdateValue) {
      var patch = {
          iothubDM : {
            firmwareUpdate : firmwareUpdateValue
          }
      };
   
      twin.properties.reported.update(patch, function(err) {
        if (err) throw err;
        console.log('twin state reported: ' + firmwareUpdateValue.status);
      });
    };
    ```

<span data-ttu-id="be374-115">7. lépés: A következő függvények, amelyek letöltése és hello belső vezérlőprogram kép telepítése szimulálása hello hozzáadása:</span><span class="sxs-lookup"><span data-stu-id="be374-115">Step 7: Add hello following functions that simulate downloading and applying hello firmware image:</span></span>
   
    ```
    var simulateDownloadImage = function(imageUrl, callback) {
      var error = null;
      var image = "[fake image data]";
   
      console.log("Downloading image from " + imageUrl);
   
      callback(error, image);
    }
   
    var simulateApplyImage = function(imageData, callback) {
      var error = null;
   
      if (!imageData) {
        error = {message: 'Apply image failed because of missing image data.'};
      }
   
      callback(error);
    }
    ```

<span data-ttu-id="be374-116">8. lépés:, Adja hozzá a következő függvény, hogy frissítéseket hello belső vezérlőprogram frissítési állapotát a hello jelentett tulajdonságok túl hello**Várakozás**.</span><span class="sxs-lookup"><span data-stu-id="be374-116">Step 8: Add hello following function that updates hello firmware update status through hello reported properties too**waiting**.</span></span> <span data-ttu-id="be374-117">Általában eszközök kapjanak az elérhető frissítésekkel és a rendszergazda által megadott házirend hatására hello eszköz toostart letöltése és hello frissítés telepítését.</span><span class="sxs-lookup"><span data-stu-id="be374-117">Typically, devices are informed of an available update and an administrator defined policy causes hello device toostart downloading and applying hello update.</span></span> <span data-ttu-id="be374-118">Ez a funkció hol van hello logika tooenable, amely házirend kell futtatnia.</span><span class="sxs-lookup"><span data-stu-id="be374-118">This function is where hello logic tooenable that policy should run.</span></span> <span data-ttu-id="be374-119">Az egyszerűség kedvéért hello minta megvárja-e a négy másodpercig eljárás toodownload hello belső vezérlőprogram kép előtt:</span><span class="sxs-lookup"><span data-stu-id="be374-119">For simplicity, hello sample waits for four seconds before proceeding toodownload hello firmware image:</span></span>
   
    ```
    var waitToDownload = function(twin, fwPackageUriVal, callback) {
      var now = new Date();
   
      reportFWUpdateThroughTwin(twin, {
        fwPackageUri: fwPackageUriVal,
        status: 'waiting',
        error : null,
        startedWaitingTime : now.toISOString()
      });
      setTimeout(callback, 4000);
    };
    ```

<span data-ttu-id="be374-120">9. lépés:, Adja hozzá a következő függvény, hogy frissítéseket hello belső vezérlőprogram frissítési állapotát a hello jelentett tulajdonságok túl hello**letöltése**.</span><span class="sxs-lookup"><span data-stu-id="be374-120">Step 9: Add hello following function that updates hello firmware update status through hello reported properties too**downloading**.</span></span> <span data-ttu-id="be374-121">hello függvény majd szimulálja a belső vezérlőprogram letöltését, és végül frissítések hello belső vezérlőprogram frissítési állapot tooeither **downloadFailed** vagy **downloadComplete**:</span><span class="sxs-lookup"><span data-stu-id="be374-121">hello function then simulates a firmware download and finally updates hello firmware update status tooeither **downloadFailed** or **downloadComplete**:</span></span>
   
    ```
    var downloadImage = function(twin, fwPackageUriVal, callback) {
      var now = new Date();   
   
      reportFWUpdateThroughTwin(twin, {
        status: 'downloading',
      });
   
      setTimeout(function() {
        // Simulate download
        simulateDownloadImage(fwPackageUriVal, function(err, image) {
   
          if (err)
          {
            reportFWUpdateThroughTwin(twin, {
              status: 'downloadfailed',
              error: {
                code: error_code,
                message: error_message,
              }
            });
          }
          else {        
            reportFWUpdateThroughTwin(twin, {
              status: 'downloadComplete',
              downloadCompleteTime: now.toISOString(),
            });
   
            setTimeout(function() { callback(image); }, 4000);   
          }
        });
   
      }, 4000);
    }
    ```

<span data-ttu-id="be374-122">10. lépés:, Adja hozzá a következő függvény, hogy frissítéseket hello belső vezérlőprogram frissítési állapotát a hello jelentett tulajdonságok túl hello**alkalmazása**.</span><span class="sxs-lookup"><span data-stu-id="be374-122">Step 10: Add hello following function that updates hello firmware update status through hello reported properties too**applying**.</span></span> <span data-ttu-id="be374-123">hello függvény majd szimulálja alkalmazása hello belső vezérlőprogram kép, és végül frissítések hello belső vezérlőprogram frissítési állapot tooeither **applyFailed** vagy **applyComplete**:</span><span class="sxs-lookup"><span data-stu-id="be374-123">hello function then simulates applying hello firmware image and finally updates hello firmware update status tooeither **applyFailed** or **applyComplete**:</span></span>
    
    ```
    var applyImage = function(twin, imageData, callback) {
      var now = new Date();   
    
      reportFWUpdateThroughTwin(twin, {
        status: 'applying',
        startedApplyingImage : now.toISOString()
      });
    
      setTimeout(function() {
    
        // Simulate apply firmware image
        simulateApplyImage(imageData, function(err) {
          if (err) {
            reportFWUpdateThroughTwin(twin, {
              status: 'applyFailed',
              error: {
                code: err.error_code,
                message: err.error_message,
              }
            });
          } else { 
            reportFWUpdateThroughTwin(twin, {
              status: 'applyComplete',
              lastFirmwareUpdate: now.toISOString()
            });    
    
          }
        });
    
        setTimeout(callback, 4000);
    
      }, 4000);
    }
    ```

<span data-ttu-id="be374-124">11. lépés:, Adja hozzá hello következő működik, hogy kezeli hello **firmwareUpdate** közvetlen metódus, és kezdeményezi hello többlépcsős belső vezérlőprogram frissítése folyamat:</span><span class="sxs-lookup"><span data-stu-id="be374-124">Step 11: Add hello following function that handles hello **firmwareUpdate** direct method and initiates hello multi-stage firmware update process:</span></span>
    
    ```
    var onFirmwareUpdate = function(request, response) {
    
      // Respond hello cloud app for hello direct method
      response.send(200, 'FirmwareUpdate started', function(err) {
        if (!err) {
          console.error('An error occured when sending a method response:\n' + err.toString());
        } else {
          console.log('Response toomethod \'' + request.methodName + '\' sent successfully.');
        }
      });
    
      // Get hello parameter from hello body of hello method request
      var fwPackageUri = request.payload.fwPackageUri;
    
      // Obtain hello device twin
      client.getTwin(function(err, twin) {
        if (err) {
          console.error('Could not get device twin.');
        } else {
          console.log('Device twin acquired.');
    
          // Start hello multi-stage firmware update
          waitToDownload(twin, fwPackageUri, function() {
            downloadImage(twin, fwPackageUri, function(imageData) {
              applyImage(twin, imageData, function() {});    
            });  
          });
    
        }
      });
    }
    ```

<span data-ttu-id="be374-125">12. lépés: Végül adja hozzá a következő kódot, amely összeköti az IoT-központ tooyour hello:</span><span class="sxs-lookup"><span data-stu-id="be374-125">Step 12: Finally, add hello following code that connects tooyour IoT hub:</span></span>
    
    ```
    client.open(function(err) {
      if (err) {
        console.error('Could not connect tooIotHub client');
      }  else {
        console.log('Client connected tooIoT Hub.  Waiting for firmwareUpdate direct method.');
      }
    
      client.onDeviceMethod('firmwareUpdate', onFirmwareUpdate);
    });
    ```

> [!NOTE]
> <span data-ttu-id="be374-126">Ez az oktatóanyag tookeep dolgot egyszerű, nem valósítja meg semmilyen újrapróbálkozási házirendje.</span><span class="sxs-lookup"><span data-stu-id="be374-126">tookeep things simple, this tutorial does not implement any retry policy.</span></span> <span data-ttu-id="be374-127">Az éles kódban, meg kell valósítania újrapróbálkozási házirendek (például az exponenciális leállítási), hello MSDN-cikkben leírtak [átmeneti hiba kezelése](https://msdn.microsoft.com/library/hh675232.aspx).</span><span class="sxs-lookup"><span data-stu-id="be374-127">In production code, you should implement retry policies (such as an exponential backoff), as suggested in hello MSDN article [Transient Fault Handling](https://msdn.microsoft.com/library/hh675232.aspx).</span></span>
> 
> 