## <a name="create-a-simulated-device-app"></a>Szimulált eszközalkalmazás létrehozása
Ebben a szakaszban:

* Hozzon létre egy Node.js-Konzolalkalmazás, amely válaszol a hello felhő által meghívott tooa közvetlen módszer
* Aktivál egy szimulált belsővezérlőprogram-frissítést
* Használjon hello jelentett tulajdonságok tooenable eszköz iker lekérdezések tooidentify eszközök és amikor azok legutóbbi befejeződött egy belső vezérlőprogram frissítése

1. lépés:, Hozzon létre egy üres nevű mappát **manageddevice**.  A hello **manageddevice** mappa, hozzon létre egy package.json fájlt a következő parancsot a parancssorba hello segítségével. Fogadja el az összes hello alapértelmezett beállításokat:
   
    ```
    npm init
    ```

2. lépés: parancsot a parancssorba, a hello **manageddevice** mappa, futtassa a következő parancs tooinstall hello hello **azure iot-eszközök** és **azure-iot-eszközök – mqtt** eszköz SDK-csomagot:
   
    ```
    npm install azure-iot-device azure-iot-device-mqtt --save
    ```

3. lépés: Egy szövegszerkesztő segítségével hozzon létre egy **dmpatterns_fwupdate_device.js** hello fájlban **manageddevice** mappa.

4. lépés:, Adja hozzá hello következő "megkövetelése a" hello hello indításkor állapotkimutatások **dmpatterns_fwupdate_device.js** fájlt:
   
    ```
    'use strict';
   
    var Client = require('azure-iot-device').Client;
    var Protocol = require('azure-iot-device-mqtt').Mqtt;
    ```
5. lépés: Adja hozzá a **connectionString** változó, és toocreate használni egy **ügyfél** példány. Cserélje le a hello `{yourdeviceconnectionstring}` helyőrző korábban rögzített egy hello "Egy eszközidentitás létrehozása" szakaszban korábban hello kapcsolati karakterlánccal:
   
    ```
    var connectionString = '{yourdeviceconnectionstring}';
    var client = Client.fromConnectionString(connectionString, Protocol);
    ```

6. lépés:, Adja hozzá hello következő függvénynél, amely használt tooupdate jelentett tulajdonságok:
   
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

7. lépés: A következő függvények, amelyek letöltése és hello belső vezérlőprogram kép telepítése szimulálása hello hozzáadása:
   
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

8. lépés:, Adja hozzá a következő függvény, hogy frissítéseket hello belső vezérlőprogram frissítési állapotát a hello jelentett tulajdonságok túl hello**Várakozás**. Általában eszközök kapjanak az elérhető frissítésekkel és a rendszergazda által megadott házirend hatására hello eszköz toostart letöltése és hello frissítés telepítését. Ez a funkció hol van hello logika tooenable, amely házirend kell futtatnia. Az egyszerűség kedvéért hello minta megvárja-e a négy másodpercig eljárás toodownload hello belső vezérlőprogram kép előtt:
   
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

9. lépés:, Adja hozzá a következő függvény, hogy frissítéseket hello belső vezérlőprogram frissítési állapotát a hello jelentett tulajdonságok túl hello**letöltése**. hello függvény majd szimulálja a belső vezérlőprogram letöltését, és végül frissítések hello belső vezérlőprogram frissítési állapot tooeither **downloadFailed** vagy **downloadComplete**:
   
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

10. lépés:, Adja hozzá a következő függvény, hogy frissítéseket hello belső vezérlőprogram frissítési állapotát a hello jelentett tulajdonságok túl hello**alkalmazása**. hello függvény majd szimulálja alkalmazása hello belső vezérlőprogram kép, és végül frissítések hello belső vezérlőprogram frissítési állapot tooeither **applyFailed** vagy **applyComplete**:
    
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

11. lépés:, Adja hozzá hello következő működik, hogy kezeli hello **firmwareUpdate** közvetlen metódus, és kezdeményezi hello többlépcsős belső vezérlőprogram frissítése folyamat:
    
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

12. lépés: Végül adja hozzá a következő kódot, amely összeköti az IoT-központ tooyour hello:
    
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
> Ez az oktatóanyag tookeep dolgot egyszerű, nem valósítja meg semmilyen újrapróbálkozási házirendje. Az éles kódban, meg kell valósítania újrapróbálkozási házirendek (például az exponenciális leállítási), hello MSDN-cikkben leírtak [átmeneti hiba kezelése](https://msdn.microsoft.com/library/hh675232.aspx).
> 
> 