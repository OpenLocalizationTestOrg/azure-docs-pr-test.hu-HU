> [!div class="op_single_selector"]
> * [Linux](../articles/iot-hub/iot-hub-linux-iot-edge-simulated-device.md)
> * [Windows](../articles/iot-hub/iot-hub-windows-iot-edge-simulated-device.md)

Ez a forgatókönyv a hello [szimulált eszköz felhő feltöltése minta] bemutatja, hogyan toouse [Azure IoT peremhálózati] [ lnk-sdk] toosend eszközről a felhőbe telemetriai tooIoT Hub a szimulált azok az eszközök.

A bemutató tartalma:

* **Architektúra**: hello architekturális információ [szimulált eszköz felhő feltöltése minta].
* **Létrehozása és futtatása**: hello lépéseket szükséges toobuild és futtatási hello minta.

## <a name="architecture"></a>Architektúra

Hello [szimulált eszköz felhő feltöltése minta] bemutatja, hogyan toocreate egy átjáró által küldött telemetriai adatokat a szimulált eszköz tooan IoT-központot. Egy eszköz nem lehet képes tooconnect közvetlenül tooIoT Hub mert hello eszköz:

* Az IoT-központ által értelmezhető kommunikációs protokoll nem használja.
* Nincs elég intelligens tooremember hello hozzárendelt identitás tooit IoT hubból.

Az IoT-átjáró a következő módokon hello e problémák megoldását:

* hello átjáró hello eszköz által használt hello protokoll megértette, eszköz-felhő telemetriai kap hello eszköz, és továbbítja azokat üzenetek tooIoT Hub érthető hello IoT hub protokoll használatát.

* hello átjáró IoT-központ identitások toodevices le, és amely proxyként funkcionál, ha egy eszköz üzenetek tooIoT Hub küld.

a következő diagram hello hello hello minta fő összetevőit, beleértve hello IoT peremhálózati modulok jeleníti meg:

![][1]

hello modulok nem továbbítja az üzeneteket közvetlenül tooeach más. hello modulok üzenetek tooan belső broker letölti a hello üzenetek toohello más modulok egy előfizetés alapján történő közzétételére. További információkért lásd: [Ismerkedés az Azure IoT peremhálózati][lnk-gw-getstarted].

### <a name="protocol-ingestion-module"></a>Protokollfeldolgozási modul

Ez a modul kiindulási pontjaként fogadó adatokat az eszközökről, hello átjárón keresztül, és hello felhőbe hello. Hello minta hello modul:

1. Szimulált hőmérséklet adatokat hoz létre. Ha a fizikai eszközök használatához hello modul olvassa be az adatokat fizikai eszközöket.
1. Létrehoz egy üzenetet.
1. Szimulált hello hőmérséklet adatok helyezi hello üzenet tartalma.
1. Hozzáad egy tulajdonság hamis MAC-cím toohello üzenetet.
1. Lehetővé teszi a hello üzenet elérhető toohello tovább modul hello láncában található.

hello modul nevű **protokoll X adatfeldolgozást** hello az előző ábrán neve **szimulált eszköz** hello forráskód.

### <a name="mac-lt-gt-iot-hub-id-module"></a>MAC &lt;-&gt; IoT Hub-azonosítómodul

Ez a modul egy Mac-cím tulajdonsággal rendelkező üzenetek keres. Hello mintában hello protokoll adatfeldolgozást modul hozzáadása hello MAC-cím tulajdonságát. Hello modul azt észlelte, hogy a tulajdonságot, ha hozzáadja az IoT Hub eszköz fő toohello üzenetet egy másik tulajdonságot. hello modul hello üzenet elérhető toohello tovább modul majd teszi hello láncában található.

hello fejlesztői állít be egy MAC-címek és az IoT-központ identitások tooassociate szimulált hello eszközök IoT Hub eszköz identitások közötti leképezést. hello fejlesztői hello leképezési hello modul konfigurációjának részeként manuálisan hozzáadja.

> [!NOTE]
> Ez a minta egy MAC-címet használ egyedi eszközazonosítóként, és azt egy IoT Hub-eszközidentitáshoz kapcsolja. Írhat azonban saját, eltérő egyedi azonosítót használó modult is. Például előfordulhat, hogy az eszközök egyedi sorozatszámokat vagy hello telemetriai adatok tartalmazhatják a beágyazott eszköz egyedi neve.

### <a name="iot-hub-communication-module"></a>IoT Hub-kommunikációs modul

Ez a modul az IoT-központ üzenetek hello előző modul által hozzárendelt eszköz kulcstulajdonság vesz igénybe. hello modul hello tartalom tooIoT központ használatával a HTTP protokoll hello üzenetet küld. A HTTP az IoT-központ három protokollok megértettem hello egyik.

Az egyes szimulált eszköz kapcsolat létesítése helyett az Ez a modul egyetlen HTTP-kapcsolat megnyitása hello átjáró toohello IoT-központ. hello modul majd multiplexes minden hello szimulált eszköz kapcsolatokat a kapcsolaton keresztül. Ez a megközelítés lehetővé teszi, hogy egy átjáró egyetlen tooconnect számos további eszköz.

## <a name="before-you-get-started"></a>A kezdés előtt

Mielőtt hozzáfogna, a következőket kell tennie:

* [Létrehoz egy IoT-központot] [ lnk-create-hub] az Azure-előfizetéshez kell hello nevét a hub toocomplete Ez a forgatókönyv. Ha nincs fiókja, néhány perc alatt létrehozhat egy [ingyenes fiókot][lnk-free-trial].
* Két eszközök tooyour IoT hub hozzáadása, és jegyezze fel az azonosítót és az eszköz kulcsok. Használhatja a hello [eszköz explorer] [ lnk-device-explorer] vagy [IOT hubbal-explorer] [ lnk-iothub-explorer] tooadd az eszközök toohello IoT hub hello létrehozott eszközzel előző lépést, valamint a kulcsok beolvasása.

![][2]

<!-- Images -->
[1]: media/iot-hub-iot-edge-simulated-selector/image1.png
[2]: media/iot-hub-iot-edge-simulated-selector/image2.png

<!-- Links -->
[szimulált eszköz felhő feltöltése minta]: https://github.com/Azure/iot-edge/blob/master/samples/simulated_device_cloud_upload/README.md
[lnk-sdk]: https://github.com/Azure/iot-edge
[lnk-gw-getstarted]: ../articles/iot-hub/iot-hub-linux-iot-edge-get-started.md
[lnk-free-trial]: https://azure.microsoft.com/pricing/free-trial/
[lnk-device-explorer]: https://github.com/Azure/azure-iot-sdk-csharp/tree/master/tools/DeviceExplorer
[lnk-iothub-explorer]: https://github.com/Azure/iothub-explorer/blob/master/readme.md
[lnk-create-hub]: ../articles/iot-hub/iot-hub-create-through-portal.md